<properties
    pageTitle="Hozzon létre egy internetes terheléselosztó és az IPv6 az Azure erőforrás-kezelő használatával az Azure CLI |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó és az IPv6 az Azure erőforrás-kezelő az Azure CLI használatával"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="az IPv6, azure terheléselosztó, kettős Papírhalom, nyilvános ip, natív ipv6, Mobiltelefonról, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Hozzon létre egy internetes terheléselosztó és az IPv6 az Azure erőforrás-kezelő az Azure CLI használatával

> [AZURE.SELECTOR]
- [A PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sablon](./load-balancer-ipv6-internet-template.md)

Az Azure terheléselosztó az egy réteg-4-es (TCP, UDP) terheléselosztó. A terheléselosztó magas elérhetősége biztosítja terjesztése bejövő forgalom megfelelő szolgáltatás példányainak cloud Services vagy a betöltés terheléselosztó meg virtuális gépeken futó között. Azure terheléselosztó is bemutathatja több portok vagy több IP-címek szolgáltatások.

## <a name="example-deployment-scenario"></a>Példa telepítéshez

Az alábbi ábra szemlélteti a terheléselosztási megoldást üzembe helyezéséhez a jelen cikkben ismertetett példa sablon segítségével.

![Betöltési terheléselosztó eset](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Ebben az esetben a következő Azure erőforrások létrehoznia:

- két virtuális gépeken futó (VMs)
- minden egyes virtuális társított IPv4 és az IPv6-címek a virtuális hálózati felhasználói felülete
- az internetes terheléselosztó IPv4- és az IPv6 nyilvános IP-címet a
- a két VMs egy elérhetősége értékre, amely tartalmazza.
- két töltse be a nyilvános VIP hozzárendelése a magánjellegű végpontok terheléselosztó szabályok

## <a name="deploying-the-solution-using-the-azure-cli"></a>A használja az Azure CLI megoldást üzembe helyezné

A következő lépések bemutatják, hogyan hozhat létre egy internetes Azure erőforrás-kezelő használata CLI terheléselosztó. Az Azure erőforrás-kezelő, az egyes erőforrások létrehozott és beállított külön-külön, és, majd írja erőforrás létrehozása.

Egy terheléselosztó üzembe helyezéséhez létrehozása, és adja meg a következő objektumoknak:

- Előtér-IP-konfiguráció – a bejövő hálózati forgalmat a nyilvános IP-címeket tartalmazza.
- Háttéradatbázis cím alkalmazáskészlet - a virtuális gépeken futó hálózati forgalmának engedélyezésére fogadjon a terheléselosztó hálózati kapcsolatok (NIC) tartalmaz.
- Egy nyilvános portjához a terheléselosztó hozzárendelése a háttéradatbázist cím készletben port szabályok terheléselosztás szabályok - tartalmazza.
- Bejövő hálózati Címfordítást szabályok – a terheléselosztó egy nyilvános portjához hozzárendelése egy adott virtuális gép a háttéradatbázist cím készletben port szabályokat tartalmaz.
- Ellenőrzi,-állapot szondákat elérhetőségének megállapítása a háttéradatbázist cím készletben virtuális gépeken futó példányok használt tartalmazza.

További tudnivalókért lásd: [Azure erőforrás-kezelő terheléselosztó támogatása](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Állítsa be a CLI környezet Azure erőforrás-kezelő

Ebben a példában a CLI eszközök futnak azt egy PowerShell-parancsablakban. Azt nem használja az Azure PowerShell-parancsmagok, de PowerShell a parancsfájlok futtatásának engedélyezése az olvashatóságot és újrafelhasználása használjuk.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Erőforrás-kezelő módba vált, az **azure konfiguráció mód** parancs futtatásával.

        azure config mode arm

    Várt kimenet:

        info:    New mode is arm

3. Jelentkezzen be az Azure és előfizetések listáját.

        azure login

    Írja be a Azure hitelesítő adatait, amikor a rendszer kéri.

        azure account list

    Válassza ki a használni kívánt előfizetést. Jegyezze fel a következő lépésben az előfizetés azonosítója.

4. Állítsa be a PowerShell változók CLI parancsok való használatra.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Erőforráscsoport, egy terheléselosztó, egy virtuális hálózati és alhálózat létrehozása

1. Erőforráscsoport létrehozása

        azure group create $rgName $location

2. Hozzon létre egy terheléselosztó

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Hozzon létre egy virtuális hálózati (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Hozzon létre két alhálózat e VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Nyilvános IP-címek az előtér-készlet létrehozása

1. A PowerShell-változó beállítása

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Az előtér-IP-készlet létrehozása a nyilvános IP-címet.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]A terheléselosztó a tartomány címke nyilvános IP-használja, mint a teljesen minősített tartománynév. Ez a változás klasszikus telepítési, amely a felhőalapú szolgáltatást használ nevet, amint a terheléselosztó FQDN.
    >Ebben a példában a teljesen minősített tartománynév *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Előtér- és készletek létrehozása

Ez a példa hoz létre az előtér-IP-készlet kapja a bejövő hálózati forgalmat a terheléselosztó és a háttéradatbázist IP készletre, ahol az előtér-készlet küld-e a terheléselosztása hálózati forgalmának engedélyezésére.

1. A PowerShell-változó beállítása

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. A nyilvános IP-a terheléselosztó és az előző lépésben létrehozott társítása előtér-IP-készlet létrehozása.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>A vizsgálati, hálózati Címfordítást szabályok és LB szabályok létrehozása

Ez a példa hozza létre az alábbiakat:

- TCP 80-as port elérhetőségének ellenőrzése a vizsgálati szabályok
- Ha port 3389 porthoz 3389 RDP<sup>1</sup> minden bejövő forgalmát hálózati Címfordítást szabály
- Ha port 3391 porthoz 3389 RDP<sup>1</sup> minden bejövő forgalmát hálózati Címfordítást szabály
- betöltési terheléselosztó szabály minden bejövő forgalmat a címeket, a háttéradatbázist készletben 80-as portjához 80-as port egyenleg.

<sup>1</sup> a hálózati Címfordítást szabályokat kell egy adott virtuális gép példány a terheléselosztó mögött van társítva. A port 3389 érkező forgalmat a rendszer elküldi az adott virtuális gép és a hálózati Címfordítást szabályhoz tartozó port. Egy hálózati Címfordítást szabályhoz (UDP vagy TCP) protokoll kell megadnia. Nem lehet két protokoll hozzárendelni ugyanazt a portot.

1. A PowerShell-változó beállítása

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. A vizsgálati létrehozása

    Az alábbi példa létrehoz egy TCP vizsgálati, amely ellenőrzi a háttéradatbázist TCP 80 portot kapcsolatot minden 15 másodperc. Ezt megjelöli a háttéradatbázist erőforrás nem érhető el két egymást követő hiba után.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Bejövő hálózati Címfordítást szabályok, amelyek lehetővé teszik a háttéradatbázist erőforrásokhoz RDP-kapcsolatok létrehozása

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Hozzon létre egy terheléselosztó szabályok, amelyek a forgalom küldése másik háttéradatbázis portokat attól függően, mely előtér a kérést kapott

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. A beállítások ellenőrzése

        azure network lb show --resource-group $rgName --name $lbName

    Várt kimenet:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>NIC létrehozása

Hozzon létre NIC, és a társítani a hálózati Címfordítást szabályokat, betöltés terheléselosztó szabályok és szondákat.

1. A PowerShell-változó beállítása

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Hozzon létre egy hálózati az egyes háttéradatbázist, és az IPv6 konfiguráció hozzáadása.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>A háttéradatbázist virtuális erőforrások létrehozása, és csatolja az egyes hálózati kártya

VMs létrehozásához tároló fiókkal kell rendelkeznie. A terheléselosztás, a VMs kell egy elérhetőségének beállítása tagjainak. VMs létrehozásával kapcsolatos további tudnivalókért lásd: [létrehozása az Azure virtuális PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md)

1. A PowerShell-változó beállítása

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] Ebben a példában a VMs egyszerű szöveges használja a felhasználónevét és jelszavát. Megfelelő ügyelni kell Ha használja a hitelesítő adatok törlése a. Biztonságosabb módszer a PowerShellben hitelesítő kezelését a [Get-hitelesítőadat](https://technet.microsoft.com/library/hh849815.aspx) -parancsmag című témakör tartalmaz.

2. A tárterület-fiók és az elérhetőség készlet létrehozása

    Egy meglévő tárterület-fiókot is használja, a VMs létrehozásakor. A következő parancs létrehoz egy új tárterület-fiókot.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Ezután létrehozása az elérhetőségét.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. A virtuális gépeken futó létrehozását a társított NIC

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-cli.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
