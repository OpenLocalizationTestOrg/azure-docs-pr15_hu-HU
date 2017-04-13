<properties
    pageTitle="Az SQL Server virtuális gép létrehozása az Azure PowerShell (erőforrás-kezelő) |} Microsoft Azure"
    description="Az Azure virtuális létrehozásához az SQL Server virtuális gépek galéria képekkel biztosít a lépéseket, és a PowerShell-parancsfájlokat."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Az SQL Server Azure PowerShell (erőforrás-kezelő) használatával virtuális gép kiépítése

> [AZURE.SELECTOR]
- [Portál](virtual-machines-windows-portal-sql-server-provision.md)
- [A PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre az **Erőforrás-kezelő Azure** telepítési modell Azure PowerShell-parancsmagok használata egyetlen Azure virtuális gép. Ebben az oktatóanyagban azt a képről egyetlen lemezmeghajtó használata SQL-gyűjteményben lévő egyetlen virtuális számítógépen hoz létre. A tárhely, a hálózati és a számítási erőforrások, a virtuális gép által használt új szolgáltatók hozunk létre. Ha az alábbi források közül egy meglévő szolgáltatók, ezek a szolgáltatók inkább is használhatja.

Ha ez a témakör klasszikus verziója van szüksége, olvassa el a [SQL Server Azure PowerShell klasszikus segítségével virtuális gép rendelkezni](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Előfeltételek

Az ebben az oktatóanyagban lesz szüksége:

- Az Azure-fiók és az előfizetés megkezdése előtt. Ha nincs telepítve egyik regisztráljon az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- [Azure PowerShell)](../powershell-install-configure.md), legkisebb verziószáma 1.4.0 vagy újabb verziójának (ebben az oktatóanyagban írt verziójával 1.5.0).
    - Azon verziójával, írja be a következőt **Get-modul Azure-ListAvailable**.

## <a name="configure-your-subscription"></a>Az előfizetés beállítása

Nyissa meg a Windows Powershellt, és létrehozása az Azure-fiók elérésének az alábbi parancsmag futtatásával. Választhat a a bejelentkezési képernyőn írja be hitelesítő adatait. Használja az azonos e-mail címével és jelszavával jelentkezzen be az Azure portal segítségével.

    Add-AzureRmAccount

Sikeresen bejelentkezve, amely tartalmazza a SubscriptionId, amellyel a bejelentkezés képernyőn jelenik meg bizonyos információkat. Az előfizetés, amelyben a program nem módosítja a különböző előfizetéséhez ebben az oktatóprogramban az erőforrások létrejön az. Ha több SubscriptionIds, futtassa az összes a SubscriptionIds térni az alábbi parancsmagot:

    Get-AzureRmSubscription

Egy másik SubscriptionID módosításához futtassa a következő parancsmagot a kívánt SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Kép változók meghatározása

Használhatósági és a kész parancsfájl ebben az oktatóanyagban megértése leegyszerűsítése azt indítása változót szám megadásával. A paraméterértékeket módosítsa igényei, de ügyeljen arra, a megadott értékek módosításakor nevének hossza és speciális karakterek kapcsolatos korlátozások követését.

### <a name="location-and-resource-group"></a>Hely- és erőforráscsoport
Két változó segítségével adja meg az adatterület és az erőforráscsoport, amelybe a más forrásokat nyújt a virtuális gép hoz létre.

Tetszés szerint módosíthatja, és ezután hajtsa végre a következő parancsmagok futtatásával e változók inicializálni.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Tárolási tulajdonságai

A következő változók segítségével adja meg a tárterület-fiók és a virtuális gép által használt tárterület típusát.

Tetszés szerint módosíthatja, és hajthat végre a következő parancsmagot a változók inicializálni. Figyelje meg, hogy ebben a példában azt használja a [Prémium tárhely](../storage/storage-premium-storage.md), amely gyártási munkaterhelésekből ajánlott. További információt az útmutató és más javaslatok olvassa el a [Teljesítmény gyakorlati tanácsok az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-performance.md)című témakört.

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Hálózati tulajdonságai

A következő változók segítségével adja meg a hálózati kapcsolaton, a TCP/IP terhelés módszer, a virtuális hálózat nevét, a virtuális alhálózat nevét a virtuális hálózatok az IP-címek, a tartomány IP-címek a alhálózat és a nyilvános tartomány neve címke a hálózat a virtuális gépen való használatra.

Tetszés szerint módosíthatja, és hajthat végre a következő parancsmagot a változók inicializálni.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Virtuális gép tulajdonságai

A következő változók segítségével a virtuális számítógépnek a neve, a számítógépnek a neve, a virtuális gép méretét és az operációs rendszer lemezre nevét a virtuális gép határozza meg.

Tetszés szerint módosíthatja, és hajthat végre a következő parancsmagot a változók inicializálni.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Kép tulajdonságai

A következő változók segítségével határozza meg a képet a virtuális gép használható. Ebben a példában az SQL Server 2016 Enterprise képet használnak.

Tetszés szerint módosíthatja, és hajthat végre a következő parancsmagot a változók inicializálni.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Tartsa szem előtt, hogy egy teljes listát az SQL Server képet szeretne rendelni, a Get-AzureRmVMImageOffer paranccsal kattint:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

És megjelenik az elérhető egy ajánlja fel a Get-AzureRmVMImageSku paranccsal termékváltozatok. A következő parancs látható, a **SQL2014SP1-WS2012R2** érhető el minden termékváltozatban kínálnak.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása

Az első objektumra, amely hoz létre az erőforrás-kezelő telepítési modell erőforráscsoport. A [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) parancsmag egy Azure erőforráscsoport és az erőforrások létrehozását erőforrás csoport nevét és helyét, a korábban inicializált változók által meghatározott fogjuk használni.

Hajtsa végre a következő parancsmagot a új erőforrás csoportot szeretne létrehozni.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

A virtuális gép szükséges tárolási erőforrásokat, az operációs rendszer lemezen és az SQL Server-adatok, és jelentkezzen be fájlok. Az egyszerűség kedvéért azt hoz létre egy egyetlen lemez egyaránt. További lemezt később a parancsmaggal [Hozzáadása-Azure lemez](https://msdn.microsoft.com/library/azure/dn495252.aspx) helyezze az SQL Server-adatok és a naplófájlok dedikált lemezen csatolhat. A [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) parancsmag szabványos tároló fiók létrehozása az új erőforráscsoport és tároló fióknév, tároló termékváltozat nevét és helyét a korábban inicializált változók használata definiált fogjuk használni.

Hajtsa végre a következő parancsmagot a tárhely új fiók létrehozása.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Hálózati erőforrások létrehozása

A virtuális gép hálózati erőforrások számos megköveteli a hálózati kapcsolat.

- Virtuális gépeken virtuális hálózat szükséges.
- Virtuális hálózat legalább egy definiált alhálózat kell rendelkeznie.
- A hálózati kapcsolat kell adni egy nyilvános vagy egy privát IP-címet.

### <a name="create-a-virtual-network-subnet-configuration"></a>Hozzon létre egy virtuális hálózat alhálózat konfigurálása

A virtuális hálózatba alhálózat konfigurációjának létrehozásával elkezdi azt. Az oktatóprogram, az azt a [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) parancsmaggal alapértelmezett alhálózat hoz létre. A korábban inicializált változók használata definiált előtagot neve és címe a virtuális hálózat alhálózat konfigurálása azt hoz létre.

>[AZURE.NOTE] A virtuális hálózati alhálózat konfiguráció parancsmaggal a további tulajdonságokat határozhatja meg, de ez ebben az oktatóanyagban túlra.

Hajtsa végre a következő parancsmagot a virtuális alhálózat konfiguráció létrehozásához.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Hozzon létre egy virtuális hálózaton

Ezután azt a [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) parancsmaggal virtuális hálózatba hoz létre. Az új erőforráscsoport hoz létre azt a virtuális hálózatba, a neve, hely és cím előtag definiálva, amely a korábban inicializált változók használ, és az adott az előző lépésben megadott alhálózat konfiguráció használja.

Hajtsa végre a következő parancsmagot a virtuális hálózata létrehozásához.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>A nyilvános IP-cím létrehozása

Most, hogy elkészült a definiált virtuális hálózatba, azt konfigurálnia kell a kapcsolatot a virtuális géphez IP-címet. Az ebben az oktatóanyagban azt egy nyilvános IP-cím használatával megcímezheti internetkapcsolat támogatása dinamikus IP hoz létre. Az erőforráscsoport prevously létrehozott, és a neve, hely, elosztási módszert és DNS-tartomány neve címke definiált a korábban inicializált változók használata a nyilvános IP-cím létrehozásához a [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) parancsmag fogjuk használni.

>[AZURE.NOTE] További tulajdonságokat a nyilvános IP-címének a parancsmaggal megadhatja, de ebben az oktatóanyagban kezdeti hatókörének túl. Is létrehozhat egy saját címet vagy a címet egy statikus címmel, de ebben az oktatóanyagban túlra ez is.

Hajtsa végre a következő parancsmagot a nyilvános IP-cím létrehozásához.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>A hálózati kapcsolat létrehozása

Azt most már készen áll a hálózati kapcsolat által használt a virtuális gép létrehozása. A [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) parancsmagot a hálózati kapcsolat létrehozása az erőforráscsoport létrehozott korábbi és a neve, hely, alhálózat és nyilvános IP-cím előzőleg definiált fogjuk használni.

Hajtsa végre a következő parancsmagot a hálózati kapcsolat létrehozásához.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>A virtuális objektum beállítása

Most, hogy elkészült a tárhely és a hálózati erőforrások meghatározás, azt határozza meg a virtuális gép számítási anyagok készen áll. Az oktatóprogram azt fogja adja meg a virtuális gép méretét és a különféle operációs rendszer tulajdonságai, adja meg a hálózati kapcsolat, amely a korábban létrehozott, blob-tárolóhoz határozza meg, és adja meg az operációs rendszer lemezen.

### <a name="create-the-vm-object"></a>A virtuális-objektum létrehozása

Akkor indul el a virtuális gép méretének megadásával. Az ebben az oktatóanyagban azt megadása esetén egy DS13. A [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) parancsmag konfigurálható virtuális gép objektum neve és a korábban inicializált változók használata megadott méret létrehozásához fogjuk használni.

Hajtsa végre a következő parancsmagot a virtuális gép-objektum létrehozása.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Tartsa lenyomva az ujját a nevét és jelszavát, a helyi rendszergazdai hitelesítő adatok hitelesítőadat-objektum létrehozása

Mielőtt azt beállíthatja a virtuális gép az operációs rendszer tulajdonságait, azt kell adnia a hitelesítő adatait a helyi rendszergazdafiók biztonságos karakterláncként. Ehhez fogjuk használni a [Get-hitelesítőadat](https://technet.microsoft.com/library/hh849815.aspx) -parancsmag.

Hajtsa végre a következő parancsmagot, és a Windows PowerShell hitelesítő adatok indítási ablakban írja be a név és a Windows virtuális gépen a helyi rendszergazdafiók jelszavát.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>A virtuális gép operációs rendszer tulajdonságainak beállítása

Most már készen áll a virtuális gép operációs rendszer tulajdonságainak beállítása azt. Windows operációs rendszer típusának beállítása, a [virtuális gép ügynök](virtual-machines-windows-classic-agents-and-extensions.md) telepítve van, megadhatja, hogy a parancsmag lehetővé teszi, hogy az automatikus frissítés és a virtuális számítógépnek a neve, a számítógép neve és a hitelesítő adatok a korábban inicializált változók használata csak a [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) parancsmag fogjuk használni.

Hajtsa végre a következő parancsmagot a virtuális gép az operációs rendszer tulajdonságainak beállításához.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>A hálózati kapcsolat hozzáadása a virtuális gépen

Ezután azt hozzáadja a hálózati kapcsolat létrehozott korábban a virtuális gépen. A [Hozzáadás-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) parancsmag hozzáadása a hálózati kapcsolaton, használja a hálózati kapcsolat változó, amely a korábban megadott fogjuk használni.

Hajtsa végre a következő parancsmagot a virtuális gép a hálózati kapcsolat beállítása.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Állítsa a blob tárolási helye a lemezt a virtuális gépen való használatra

Ezután azt állítja a blob tárolási helye a lemezt a korábban megadott változók segítségével virtuális gépen való használatra.

Hajtsa végre a következő parancsmagot a blob-tároló hely beállítása.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Az operációs rendszer a virtuális gép lemez tulajdonságainak beállítása

Ezután azt fogja az operációs rendszer lemez tulajdonságainak beállítása a virtuális gépen. A [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) parancsmaggal megadhatja, hogy a az operációs rendszer a virtuális gép származó képet, gyorsítótár-olvasható, csak (mert ugyanazon a lemezen telepíti az SQL Server), és adja meg a virtuális számítógépnek a neve és a korábban definiált változók használata definiált operációs rendszer lemez fogjuk használni.

Hajtsa végre a következő parancsmagot a virtuális gép lemez tulajdonságainak beállítása a az operációs rendszer.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Adja meg a virtuális gép platform képével

Az utolsó konfigurációs lépésként adja meg a virtuális gép platform képével. Az oktatóprogram, az azt használják a legújabb SQL Server 2016 CTP képet. A [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) parancsmag használata az ezen a képen a korábban megadott változók által meghatározott fogjuk használni.

Hajtsa végre a következő parancsmagot a virtuális gép platform képével megadásához.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Az SQL-virtuális létrehozása

Most, hogy befejezte a konfigurálási lépéseket, készen áll a virtuális gép létrehozásához. A [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) parancsmag hozhat létre a virtuális gépre van definiált változók használata fogjuk használni.

Hajtsa végre a következő parancsmagot a virtuális gép létrehozása.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

A virtuális gép jön létre. Figyelje meg, hogy egy szabványos tároló fiók létrejön indítási diagnosztika mivel a megadott tárterület-fiók a virtuális gép lemez egy prémium tárterület-fiókkal.

Ez a gép [nyilvános IP-címének](virtual-machines-windows-portal-sql-server-provision.md#Connect)és a saját tartománynevét az Azure-portálon megtekintheti.  

## <a name="example-script"></a>Példa parancsfájl

A következő parancsfájl tartalmazza a teljes PowerShell-parancsprogramot, ebben az oktatóanyagban. Azt feltételezi, hogy már beállítása a **Hozzáadás-AzureRmAccount** és a **Select-AzureRmSubscription** parancsok használata az Azure előfizetést.


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Következő lépések
Miután létrehozta a virtuális gép, készen áll a virtuális gép RDP és beállítása kapcsolat használatával csatlakozhat. További tudnivalókért olvassa el a [Csatlakozás az SQL Server virtuális gép a Azure (erőforrás-kezelő)](virtual-machines-windows-sql-connect.md).
