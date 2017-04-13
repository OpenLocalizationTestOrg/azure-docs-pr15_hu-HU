<properties 
    pageTitle="Szimulált hibrid felhő tesztkörnyezetben |} Microsoft Azure" 
    description="Szimulált hibrid felhő környezet kialakítása informatikai pro vagy a tesztelés fejlesztési két Azure virtuális hálózatok és VNet-VNet kapcsolat használatával." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Teszteléshez egy felhőalapú szimulált hibrid környezet beállítása

Ez a cikk végigvezet egy felhőalapú szimulált hibrid környezet létrehozása a Microsoft Azure két Azure virtuális hálózatot használ. Az alábbiakban az eredményül kapott konfigurációt.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Keltő szűrő megőrzi a felhőben hibrid üzemi környezetben, és a áll:

- Az Azure virtuális hálózaton (a TestLab virtuális) a szolgáltatott szimulált és egyszerűsített a helyszíni hálózaton.
- Az Azure (TestVNET) tárolt szimulált határokon helyszíni virtuális hálózat.
- A VNet-VNet kapcsolat, a két virtuális hálózatok között.
- Másodlagos tartományvezérlőnek a TestVNET virtuális hálózaton.

Ez biztosít, amelyben pont alap és közös indítása:

- Kidolgozása, és tesztelje a alkalmazásokat felhő szimulált hibrid környezetben.
- Hozzon létre próba konfigurációk számítógépeket, néhány a TestLab virtuális hálózaton belül, és egy hibrid felhőalapú informatikai munkaterhelésekből hasonlóan a TestVNET virtuális hálózaton belül.

Vannak négy fő fázisait a hibrid felhő tesztkörnyezetben beállítása:

1.  Állítsa be a TestLab virtuális hálózat.
2.  Hozzon létre a határokon helyszíni virtuális hálózat.
3.  A VNet-VNet virtuális Magánhálózati kapcsolat hozható létre.
4.  Állítsa be a DC2. 

Ez a beállítás egy Azure-előfizetést igényel. Ha az MSDN webhelyen vagy a Visual Studio előfizetéssel rendelkezik, olvassa el a [havi Azure hitelképesség Visual Studio előfizetők számára](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)című témakört.

>[AZURE.NOTE] Virtuális gépeken futó és az Azure virtuális hálózati átjárók merülnek fel egy folyamatban lévő pénzbeli költséget, ha futnak. A költség szemben az MSDN számlát kapni, vagy a kifizetett előfizetés. Az Azure virtuális Magánhálózati átjáró két Azure virtuális gépeken futó formájában történik. A költségek minimalizálásához tesztkörnyezetben létrehozása, és hajtsa végre a szükséges tesztelése és a bemutató minél gyorsabban.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>1 fázis: A TestLab virtuális hálózat konfigurálása

A [következő konfigurációs tesztkörnyezetben](https://technet.microsoft.com/library/mt771177.aspx) témakörben a képernyőn megjelenő utasításokat segítségével a DC1 APP1 és ÜGYFÉL1 számítógépek beállítása a TestLab nevű Azure virtuális hálózat. 

Ezután indítsa el az Azure PowerShell-parancssorában.

> [AZURE.NOTE] A következő parancs az Azure PowerShell használata 1,0 és újabb verziók állítja be.

Bejelentkezés a fiókjába.

    Login-AzureRMAccount

Ismerkedés az előfizetése nevére a következő parancsot.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Az Azure előfizetés beállítása. A alapkonfiguráció a fázis 1 létrehozásához használt ugyanabban az előfizetésben használni. Mindent, ami az idézőjelekkel együtt cseréje a < és > karaktert, a helyes nevet.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Ezután adja hozzá egy átjáró alhálózat a alapkonfiguráció, az Azure átjáró tárolására szolgáló TestLab virtuális hálózatához.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Ezután kérése a TestLab virtuális hálózat az átjáró hozzárendelése a nyilvános IP-címet.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Ezután hozzon létre az átjárót.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Ne feledje, hogy új átjárókat is igénybe vehet, 20 perc vagy több hozhat létre.

A helyi számítógépen Azure portálról csatlakozni DC1 CORP\User1 hitelesítő adataival. A CORP tartomány konfigurálásához, hogy a számítógépek és a felhasználók használni a helyi tartományvezérlőnek hitelesítési futtatása DC1-rendszergazdai szintű a Windows PowerShell parancssorból ezek a parancsok.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Ez a beállítás az aktuális.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>2 fázis: A TestVNET virtuális hálózat létrehozása

Először a TestVNET virtuális hálózat létrehozása, és a hálózati biztonsági csoport védelmét.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Ezután kérése egy nyilvános IP-címet az átjáró TestVNET virtuális hálózatok kiosztandó és az átjáró létrehozása.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Ez a beállítás az aktuális.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>3 fázis: A VNet-VNet kapcsolat létrehozása

Először szerezzen be egy véletlen kriptográfiailag erős, 32 karakteres előre megosztott kulcsot a hálózati vagy a biztonsági rendszergazda. Másik megoldás használja az információkat [létrehozása egy olyan IPsec előzetesen megosztott kulcs véletlen karakterláncot](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) a egy előre megosztott kulcsot juthat.

Ezután ezek a parancsok használatával hozza létre a VNet-VNet virtuális Magánhálózati kapcsolatot, ami befejezéséhez egy kis időt vehet igénybe.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Néhány perc múlva kell létre.

Ez a beállítás az aktuális.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>4 fázis: DC2 konfigurálása

Először hozzon létre egy virtuális gép DC2 számára. Ezek a parancsok a Azure PowerShell-parancssorában futtassa a helyi számítógépre.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Ezután csatlakozni az új DC2 virtuális gép az Azure portálról.

Ezután beállítása a Windows tűzfal szabály egyszerű kapcsolat tesztelése forgalmának engedélyezésére. Egy rendszergazdai szintű a Windows PowerShell parancssorból kapott DC2 ezek a parancsok futtathatók.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

A ping parancsot az IP-cím 10.0.0.4 négy sikeres választ kell eredményez. Ez a forgalom vizsgálata a VNet-VNet kapcsolaton keresztül.

Ezután adja hozzá a felesleges adatok lemez a DC2 olyan új kötet, amelynek f betűjele.

1.  A bal oldali ablaktáblában a Kiszolgálókezelő kattintson a **fájl- és tároló szolgáltatást**, és válassza a **lemez**.
2.  A Tartalom ablaktáblán, a **lemezt** csoportban kattintson **lemez 2** (a **partíciót** **Ismeretlen**meg).
3.  Kattintson a **Feladatok nézetre**, és kattintson az **Új mennyiségi**.
4.  Kattintson az előtte kezdje el az új mennyiségi varázsló lapján található, kattintson a **Tovább**gombra.
5.  A válassza ki a kiszolgáló és a lemez lapra kattintson a **lemez 2**elemre, és kattintson a **Tovább gombra**. Amikor a rendszer kéri, kattintson az **OK gombra**.
6.  Adja meg a mennyiségi lap méretét kattintson a **Tovább**gombra.
7.  A kijelölés betűt vagy mappa lapra kattintson a **Tovább**gombra.
8.  A fájl kiválasztása rendszer beállításai lapon kattintson a **Tovább**gombra.
9.  A Confirm beállítások lapon kattintson a **Létrehozás**gombra.
10. Amikor elkészült, kattintson a **Bezárás**gombra.

Ezután konfigurálása DC2 egy replika tartományvezérlőnek a vallalat.kontraktor.hu tartomány. Ezek a parancsok a Windows PowerShell parancssorából futni DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Figyelje meg, hogy adni a CORP\User1 jelszó és a címtár szolgáltatások visszaállítása mód Címtárszolgáltatások jelszót, és indítsa újra a DC2 kéri.

Most, hogy a TestVNET virtuális hálózat saját DNS-kiszolgáló (DC2) van, meg kell adnia a DNS-kiszolgáló használata a TestVNET virtuális hálózat.

1.  Az Azure portál bal oldali ablaktáblában kattintson a virtuális hálózatok ikonra, és válassza a **TestVNET**.
2.  A **Beállítások** lapon kattintson a **DNS-kiszolgálók**.
3.  Az **elsődleges DNS-kiszolgálót**írja be a **192.168.0.4** 10.0.0.4 cseréje.
4.  Kattintson a **Mentés**gombra.

Ez a beállítás az aktuális. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
A szimulált hibrid felhőalapú környezetben most már készen áll a tesztelésre.

## <a name="next-step"></a>Következő lépés

- Állítsa be a környezetet [üzleti alkalmazás webalapú sor](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) .
