<properties
    pageTitle="Virtuális gép skála készletek PowerShell-parancsmagok használata létrehozása |} Microsoft Azure"
    description="Első lépések hozhat létre és kezelhet az első Azure virtuális gép skála készletek Azure PowerShell-parancsmagok használata"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="danielsollondon"/>

# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Virtuális gép skála készletek PowerShell-parancsmagok használata létrehozása

Íme egy példa bemutatja, hogyan hozhat létre egy virtuális gép skála Set(VMSS), létrehoz egy VMSS 3 csomópontok, az összes kapcsolódó hálózatok és tároló.

## <a name="first-steps"></a>Első lépések
Győződjön meg róla, a legújabb Azure PowerShell-modult telepítve van, ez fogja tartalmazni a szükséges VMSS létrehozása és kezelése a PowerShell parancsmaggal.
Nyissa meg a parancssor eszközök [Itt](http://aka.ms/webpi-azps) a legújabb Azure modulok érhető el.

Kapcsolódó VMSS keresése parancsmaggal, használja a keresőkifejezést \*VMSS\*.

## <a name="creating-a-vmss"></a>Egy VMSS létrehozása

##### <a name="create-resource-group"></a>Erőforrás-csoport létrehozása

```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

##### <a name="create-storage-account"></a>Tárterület-fiók létrehozása

Tárterület-fiók típusának beállítása / nevet.

```
$stoname = 'sto' + $rgname;
$stotype = 'Standard_LRS';
 New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname -Location $loc -SkuName $stotype -Kind "Storage";

$stoaccount = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname;
```

#### <a name="create-networking-vnet--subnet"></a>Hálózati létrehozása (VNET / alhálózat)

##### <a name="subnet-specification"></a>Alhálózat meghatározása

```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

##### <a name="vnet-specification"></a>VNET meghatározása

```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

#In this case below we assume the new subnet is the only one, note difference if you have one already or have adjusted this code to more than one subnet.
$subnetId = $vnet.Subnets[0].Id;
```

##### <a name="create-public-ip-resource-to-allow-external-access"></a>Hozzon létre nyilvános IP-erőforrás külső hozzáférés engedélyezése

Ez lesz kötni a a terheléselosztó szeretne.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

##### <a name="create-and-configure-load-balancer"></a>Létrehozása és konfigurálása a terheléselosztó

```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

#Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

##### <a name="configure-load-balancer"></a>Terheléselosztó konfigurálása
Kódmentes cím készlet Config létrehozása, ez az NIC, a VMs VMSS a osztja.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Betöltési kiegyensúlyozott vizsgálati Port beállítása, módosítsa a beállításokat tetszés szerint az alkalmazás.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Hálózati Címfordítást szabályok létrehozása a közvetlen útválasztásos kapcsolat (nem terheléselosztása) a VMs a via betöltése terheléselosztó, Megjegyzés: Ez mutatja be, RDP segítségével a VMSS való, ez csak a bemutató és belső VNET módszerek RDP'ing az alábbi kiszolgálókon való használandó.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Hozza létre a betöltése kiegyensúlyozott szabályt, ebben a példában látható betöltése terheléselosztó port 80 kérelmeket, az előző lépéseket beállításainak használatával.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Konfigurációs terheléselosztó hozhat létre.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Ellenőrizze LB beállításait, jelölje be a port konfigurációk terheléselosztása, megjegyzést, akkor jelennek meg hálózati címfordító Eszközig bejövő szabályok mindaddig, amíg a virtuális a VMSS a jönnek létre.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-vmss"></a>Állítsa be és VMSS létrehozása

A feljegyzésben e infrastruktúra példa bemutatja, hogyan beállítás ki, és az internetes forgalmat méretezze át a VMSS, de az itt megadott VMs képek nincs telepítve a bármely web services.

```
#specify VMSS Name
$vmssName = 'vmss' + $rgname;

##specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

$vhdContainer = "https://" + $stoname + ".blob.core.windows.net/" + $vmssName;

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

Hálózati kártya kötni terheléselosztó és alhálózat

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

VMSS Config létrehozása

```
#Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
  	| Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
  	| Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
  	| Set-AzureRmVmssStorageProfile -Name 'test' -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName -VhdContainer $vhdContainer `
  	| Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

VMSS Config összeállítása

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Miután létrehozta a VMSS. Kapcsolódás RDP segítségével ebben a példában a egyes virtuális tesztelheti:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
