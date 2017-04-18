<properties
    pageTitle="Az SQL Server Azure prémium tároló használja |} Microsoft Azure"
    description="Ez a cikk a klasszikus telepítési modell készült erőforrásokat használ, és futtatása a Azure virtuális gépeken futó SQL Server Azure prémium tároló használata útmutatást ad."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>A virtuális gépeken futó SQL Server Azure prémium tároló használata


## <a name="overview"></a>– Áttekintés

[Azure prémium tároló](../storage/storage-premium-storage.md) van a következő generációs, ahol a kis késés és magas átviteli IO-tárterületét. A legjobb kulcs IO intenzív-munkaterhelésekből, például a IaaS [virtuális gépeken futó](https://azure.microsoft.com/services/virtual-machines/)SQL Server működik.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ez a cikk tervezés és való áttelepítés prémium tároló alkalmazás SQL Servert futtató virtuális gép útmutatást tartalmaz. Ide tartoznak a Azure infrastruktúra (hálózati, tároló) és a vendégként való bekapcsolódáshoz Windows virtuális lépéseket. A [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) a példában a teljes átfogó végpont áttelepítésének kihasználhatja a PowerShell továbbfejlesztett helyi SSD tároló nagyobb VMs áthelyezése.

Fontos, hogy a végpontok közötti folyamat életképtelennek Azure prémium tárolási SQL Server-IAAS VMs megértése. Ide tartoznak:

- A Előfeltételek prémium tároló alkalmazás azonosítója.
- Példák a prémium tároló új telepítési SQL Server IaaS telepítése.
- Példák áttelepítése meglévő telepítésekről, önálló kiszolgálók és telepítések csoportokkal SQL mindig a elérhetősége is.
- Lehetséges áttelepítési módszerek.
- Teljes-végpont példa Azure, a Windows és az SQL Server lépéseket az áttelepítés végrehajtása egy meglévő mindig a.

Háttér többet az Azure virtuális gépeken futó SQL Server kiszolgálón olvassa el [Az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).

**Szerző:** Daniel Sol **technikai véleményezők:** Péter Carlos Vargas hering, Sanjay Mishra, Pravin Mital, Juergen Balázs, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Prémium tárolására vonatkozó követelmények

Vannak olyan prémium tároló használatával több előfeltételei.

### <a name="machine-size"></a>Gépi mérete

A prémium tároló szüksége lesz DS adatsor virtuális gépeken futó (virtuális) használata. Ha még nem használta a felhőszolgáltatásában előtt az DS sorozat gépek, törölje a meglévő virtuális, csatolt lemezt hagyja, és majd hozzon létre egy új felhőszolgáltatásba DS * szerepkör méretét, a virtuális újbóli létrehozása előtt. Virtuális gép méretű kapcsolatos további tudnivalókért olvassa el a [virtuális gép és felhőalapú szolgáltatást méretű az Azure](virtual-machines-linux-sizes.md)című témakört.

### <a name="cloud-services"></a>Felhőszolgáltatások

Csak használhatja DS * VMs prémium adathordozós egy új felhőszolgáltatásában létrehozásakor. Ha használ az SQL Server mindig Azure-ban, a mindig a figyelő az Azure belső vagy külső betöltés terheléselosztó IP-cím társított egy felhőalapú szolgáltatásba fog vonatkozni. Ez a cikk ebben az esetben állásának áthelyezé szolgáltatásaival.

> [AZURE.NOTE] Egy Lépésben * sorozat a első virtuális, amely telepíti, az új Felhőszolgáltatásba kell lennie.

### <a name="regional-vnets"></a>Területi VNETS

A Tartományi * VMs meg kell adnia a virtuális hálózati (VNET) a VMs kell területi szolgáltatója. A "kiszélesíti" a VNET lehetővé teszi a nagyobb VMs más fürt kell építenie és engedélyezi a kommunikációt közöttük. Az alábbi képernyőképen a kijelölt hely területi VNETs, látható, mivel az első eredmény egy "keskeny" VNET.

![RegionalVNET][1]

A regionális VNET áttelepítése a Microsoft támogatási jegyek előléptetheti, Microsoft módosítása, majd területi VNETs, hogy az áttelepítés végrehajtásához módosíthatja a tulajdonság AffinityGroup a hálózati konfigurálásról. Először exportálja a hálózat konfigurálása PowerShell, és majd cserélje le a **AffinityGroup** tulajdonság **VirtualNetworkSite** eleme egy **helyen** tulajdonság. Adja meg, `Location = XXXX` hol `XXXX` Azure terület. Kattintson az új konfigurációt importálni.

Ha például figyelembe az alábbi VNET konfiguráció:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Ez egy területi VNET nyugati Európában áthelyezni, módosítsa a konfiguráció a következő:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Tárterület-fiókok

Meg kell, hogy be van állítva az prémium tároló új tárterület-fiók létrehozása. Figyelje meg, hogy prémium tárhely használatának megadása a tárhely fiók nem egyedi VHD, azonban egy Lépésben * sorozat virtuális használata esetén a virtuális csatolhat Premium és a szokásos tárterület-fiókból. Ha nem szeretné helyezni az operációs rendszer virtuális prémium tároló fiókhoz ez foglalkozhat.

A következő **Új-AzureStorageAccountPowerShell** parancs a "Premium_LRS" **típus** hoz létre prémium tárterület-fiókot.

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>VHD gyorsítótár beállításai

Fő különbségét lemezt alkotó prémium tárterület-fiók létrehozása az lemez gyorsítótár beállítása. Az SQL Server Data mennyiségi lemezt ajánlott "**Olvasás gyorsítótárazás**" használható. A napló mennyiségű tranzakció a lemez gyorsítótár beállítást kell a "**nincs**". Ez különbözik a szabványos tároló fiókok javaslatok.

Miután a VHD van csatolva, a gyorsítótár beállítása nem módosítható. Kellene leválasztása, majd a virtuális csatlakoztassa a frissített gyorsítótár-beállításokkal.

### <a name="windows-storage-spaces"></a>A Windows tárhelyek

Használhatja a [Windows tárhelyek](https://technet.microsoft.com/library/hh831739.aspx) előző szabványos adathordozós ahogy, ez lehetővé teszi, hogy egy virtuális, amely már van felhasználásával tárhelyek áttelepítése. [Függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (9-es és előre lépés) a példa bemutatja, hogyan kibontása és a több csatolt VHD egy virtuális importálása a Powershell-kódot.

Tárterület-készletek teljesítmény növelése és csökkentése késés használták szabványos Azure tárterület-fiókkal. Érték található előfordulhat, hogy tárterületet készletek prémium adathordozós vizsgálata új telepítések, de a tárhely beállítással további összetettsége felveszik.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Tárterület-készletek melyik Azure virtuális lemez térkép keresése

Mivel csatolt VHD különböző gyorsítótár beállítása javaslatok, dönthet másolja a vágólapra a VHD prémium tárterület-fiókjába. Jó helyen jár Ha ismét csatlakoztatja a lapelrendezéshez őket az új Tartományi adatsor virtuális, szükség lehet a gyorsítótár beállításainak módosításához. Sokkal egyszerűbb a gyorsítótár beállításainak ajánlja fel, ha rendelkezik az SQL-adatfájlok és naplófájlok (helyett egy egyetlen virtuális is tartalmazó) külön VHD prémium tároló alkalmazása a.

> [AZURE.NOTE] Ha az SQL Server adatokat, és jelentkezzen be fájlok ugyanazon kötet, a gyorsítótár lehetőséget választja a IO access mintázatok az adatbázis-munkaterhelésekből az függ. Csak a tesztelés is bemutatják, mely gyorsítótár-beállítás a legjobb ebben az esetben.

Azonban több VHD tekintse meg kell tevődnek össze, amelyek tárhelyek Windows rendszer használata esetén az eredeti parancsfájlok azonosítása, amelyhez csatolva VHD olyan milyen adott készletben, így Ezután beállíthatja a gyorsítótár beállításainak megfelelően az egyes merevlemez.

Ha eredeti parancsfájl elérhető mutatja, hogy a tárhely kvótáját megfeleltetése VHD, amely nem rendelkezik, a az alábbi lépéseket a lemezterülettel/készlet hozzárendelés meghatározása is használhatja.

Az egyes merevlemez kövesse az alábbi lépéseket:

1. Lista beszerzése az csatolni virtuális, a **Get-AzureVM** paranccsal lemez:

    Get-AzureVM - ServiceName <servicename> -neve <vmname> |} Get-AzureDataDisk

1. Megjegyzés: a Diskname és logikai.

    ![DisknameAndLUN][2]

1. A virtuális be a távoli asztal. Ugrás a **Számítógép-kezelés** | **Eszközkezelő** | **lemezmeghajtók**. A "Microsoft virtuális lemez" tulajdonságainak megtekintése

    ![VirtualDiskProperties][3]

1. A logikai itt értéke a logikai értékre, adja meg, ha a virtuális csatolása a virtuális hivatkozást.
1. A "Microsoft virtuális lemez" lépjen a **Részletek** fülre, majd a **Tulajdonságok** listában keresse meg **Illesztőprogram kulcsot**. Az **érték**vegye figyelembe az **eltolás**, amely 0002, az alábbi képernyőképen. A 0002 azt jelzi, hogy a PhysicalDisk2, amely a tárhely kvótáját hivatkozik.

    ![VirtualDiskPropertyDetails][4]

2. Az egyes tárolókészlethez kiírása ki a társított lemez:

    Get-StoragePool – rövid név AMS1pooldata |} Get-lemezmeghajtó

    ![GetStoragePool][5]

Most használható társíthatja az információk a tárterület-készletek fizikai lemezen VHD csatolt.

Ha VHD van rendelve fizikai lemezen a tárterület-készletek leválasztása, majd másolja a fölé egy prémium tárterület-fiókkal, csatolja őket a megfelelő gyorsítótár-beállításokkal. Kérjük, nézze meg a példában a [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)lépések 8 – 12. Ezek a lépések bemutatják, hogyan bontsa ki a virtuális csatolt virtuális lemez konfiguráció CSV-fájlba, a VHD, megváltoztathatja a lemez gyorsítótár beállításai, és végül telepítsen újra a virtuális adatsorként DS virtuális az összes csatolt lemezt.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Virtuális tároló sávszélessége és a virtuális tároló átviteli

Tárterület teljesítmény mennyiségét a megadott DS * virtuális méret és a virtuális méretű függ. A VMs csatolható VHD számú különböző juttatások és azok (MB/s) támogatja a maximális sávszélesség van. Az adott sávszélesség számára olvassa el a [virtuális gép és felhőalapú szolgáltatást méretű az Azure](virtual-machines-linux-sizes.md)című témakört.

Nagyobb IOPS nagyobb lemez méretek elérését. Amikor az áttelepítési útvonalat figyelni a figyelembe. További információ: [a című táblázatban IOPS és a merevlemez-típusok](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Végül vegye figyelembe, VMs rendelkezik-e másik maximális lemez sávszélessége támogatják az összes lemez csatolt. Nagy terhelés alatt a maximális lemezterület sávszélességgel, hogy virtuális szerepkört méret telítsük van sikerült. Példa egy Standard_DS14 támogatja 512 MB/s; felfelé ezért három P30 lemezre az a virtuális lemezre sávszélességét sikerült telítsük. De ebben a példában az átviteli korlát sikerült kimerítve attól függően, hogy az Olvasás és írás IOs vegyesen.

## <a name="new-deployments"></a>Új telepítések

A következő két szakaszok bemutatják, hogyan telepítheti az SQL Server VMs prémium tárolóhoz. Említett előtt, akkor nem feltétlenül szükséges a OS lemez prémium tároló alakzatot a helyére. Előfordulhat, hogy a teendő, ha vannak kívánó bármely intenzív IO munkaterhelésekből elhelyezése az operációs rendszer virtuális válassza ki.

Az első példa bemutatja a meglévő Azure gyűjtemény képek felhasználásával. A második példában egy meglévő szabványos tárterület-fiókot tartalmazó egyéni virtuális kép használata mutatja.

> [AZURE.NOTE] Ezek a példák feltételezik, hogy már létrehozott egy területi VNET.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Hozzon létre egy új virtuális prémium adathordozós gyűjtemény képpel

Az alábbi példában helyezze az operációs rendszer virtuális prémium tároló alakzatot, és csatolja a prémium tároló VHD mutatja. Azonban az operációs rendszer lemez is elhelyezése egy szabványos tárterület-fiókot, és csatolja a VHD prémium tárterület-fiókjában található. Mindkét esetek igazolni vannak.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Lépés: 1: Prémium tárterület-fiók létrehozása


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Lépés: 2: Hozzon létre egy új, felhőalapú szolgáltatásba

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Lépés 3: (Nem kötelező) egy felhőalapú szolgáltatás virtuális lefoglalása
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Lépés: 4: Hozzon létre egy virtuális tároló
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Lépés 5: Úgy, hogy a OS virtuális a normál vagy prémium tárhely
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Lépés a 6: Hozzon létre virtuális
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Hozzon létre egy új virtuális prémium tároló használatára egy egyéni képe

Ebben az esetben a meglévő a testre szabott képeket, amelyek egy szabványos tároló fiókban találhatók meg azt mutatja be. Említett, hogy szeretné-e az operációs rendszer virtuális elhelyezése prémium tároló meg kell másolja a vágólapra a képet, hogy létezik-e a szabványos tárterület-fiókot, és helyezze át a prémium tároló ahhoz, hogy használni. Kép helyszíni környezetbe, ha ezt a módszert is használhatja másolása, amely közvetlenül a prémium tárterület-fiókot.

#### <a name="step-1-create-storage-account"></a>Lépés: 1: A tárterület-fiók létrehozása
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Lépés: 2 felhőalapú szolgáltatás hozzon létre.
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>3 lépés: A meglévő kép használata:
A meglévő képet is használhatja. Vagy [egy meglévő gépi képe vennie](virtual-machines-windows-classic-capture-image.md)is. Megjegyzés: a kép, nem kell DS* gép gépi. Ha befejezte a képet, az alábbi lépésekkel bevásárlólista másolja a vágólapra a prémium tárterület-fiókjába a * *Start-AzureStorageBlobCopy** PowerShell-parancsmag segítségével.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Lépés: 4: Másolás Blob tároló fiókok között
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Lépés az 5: Rendszeresen a Másolás állapotának ellenőrzése:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Lépés a 6: Adja hozzá a kép lemezt, Azure lemezre előfizetés tárházba
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Előfordulhat, hogy annak ellenére, hogy a állapotjelentések szerint sikeres, sikerült továbbra is hibaüzenet a lemez bérleti. Ebben az esetben várjon körülbelül 10 perc alatt.

#### <a name="step-7--build-the-vm"></a>7 lépés: A virtuális összeállítása
Itt készítésekor a virtuális a képet, és két prémium tároló VHD:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Mindig a elérhetősége csoportok nem használó meglévő telepítések

> [AZURE.NOTE] Meglévő telepítésekhez lásd: Ez a témakör a [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.

Különböző dolgot, nem használja az mindig a elérhetősége csoportok és azok, végezze el az SQL Server telepítési. Ha nem használja az mindig beállítást, és egy meglévő önálló SQL Server van, frissíthet prémium tárolóhoz új felhőalapú szolgáltatás, tárterület fiók használatával. Vegye figyelembe az alábbiakat:

- **Egy új SQL Server virtuális létrehozása**. Új telepítések ismertetett módon hozhat létre egy új SQL Server-virtuális, amely egy prémium tárterület-fiókot használnak. Ezután biztonsági, és az SQL Server konfigurálása és a felhasználó adatbázisok visszaállítása. Az alkalmazás kell frissíteni kell az új SQL Server hivatkozni, ha a belső és külső felekkel van használatban. Másolja a vágólapra az összes "kívül db" objektum, mintha egy egymás mellett (párhuzamos) az SQL Server áttelepítési voltak éppen kimutatásadatokat. Ide tartoznak a objektumok, például bejelentkezések, a tanúsítványok és a csatolt kiszolgáló.
- **Az SQL Server meglévő virtuális áttelepítése**. Ehhez az SQL Server virtuális offline állapotúvá, majd átvitele egy új, felhőalapú szolgáltatásba, amely magában foglalja másolása az összes a csatolt VHD prémium tárterület-fiókjába. A virtuális online állapotba kerül, amikor az alkalmazás a kiszolgáló állomásneve, mielőtt hivatkoznak. Ne feledje, hogy a meglévő lemez méretének hatással van a teljesítmény tulajdonságait. Ha például egy 400 GB szabad kap felfelé kerekít egy egy P20. Ha tudja, hogy nincs szüksége lemez teljesítmény, akkor is hozza létre a virtuális, mint egy Lépésben sorozat virtuális, és csatolja a prémium tároló VHD a mérete és teljesítménye specifikáció van szüksége. Ezután sikerült leválasztása, és csatlakoztassa az SQL-adatbázis fájlokat újra.

> [AZURE.NOTE] Ha a virtuális lemezt, vegye figyelembe a méret méretétől függően másolása azt jelenti, hogy milyen prémium tároló lemez esnek, akkor ez határozza meg, hogy a lemez teljesítmény specifikációja. Azure lesz, a legközelebbi lemez felfelé kerek mérete, ha egy 400 GB szabad, ez felfelé kerekíti a egy P20. Attól függően, hogy a meglévő IO követelményeinek az operációs rendszer virtuális előfordulhat, hogy nem szeretné áttelepíteni a prémium tárterület-fiókjába.

Ha az SQL Server külső felekkel érhető el, a felhőalapú szolgáltatás virtuális változik. Akkor is kell frissítés végpontjait, a hozzáférés-vezérlési listák és a DNS-beállításait.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Mindig a elérhetősége csoportok használó meglévő telepítések

> [AZURE.NOTE] Meglévő telepítésekhez lásd: Ez a témakör a [Előfeltételek](#prerequisites-for-premium-storage) című szakaszát.

Az eredetileg ebben a részben fog megnézi az hogy mindig a hogyan kommunikáljon a hálózat Azure. Azt majd megszakad lefelé az alábbi két forgatókönyvet az áttelepítés: áttelepítéseket is hol elfogadhatók néhány legrövidebb leállás és a hol kell elérése a minimális legrövidebb leállás áttelepítések.

A helyszíni SQL Server mindig a elérhetősége csoportok egy figyelő a helyszíni, amely egy virtuális DNS-név mellett egy vagy több SQL-kiszolgálók közötti megosztott IP-címének regisztrálja használja. Az ügyfélgépek hogyan kapcsolódnak azok vannak áthaladó a figyelő IP az elsődleges SQL Server. Ez az a kiszolgáló, amely a mindig a IP-erőforrás tulajdonosa adott időben.

![A DeploymentsUseAlways][6]

A Microsoft Azure egy hálózati a virtuális a hozzárendelt csak egy IP-címet is így a helyszíni, mint hardverabsztrakciós azonos rétege elérése érdekében Azure használja a belső és külső terheléselosztókkal (ILB/ELB) rendelt IP-címét. Az IP-erőforráshoz, a kiszolgáló közötti megosztott az azonos IP ILB/ELB szerint értékre van állítva. Ez a DNS-ben közzétett, és ügyfél forgalom az elsődleges SQL Server replika átadott ILB/ELB keresztül. A ILB/ELB tudja, melyik SQL Server elsődleges óta szondákat használja a mindig a IP-erőforrás probe. Az előző példában azt ellenőrzi, hogy minden csomópontot, amelynek a ELB/ILB által hivatkozott zárólap, amelyik válaszol az elsődleges SQL Server.

> [AZURE.NOTE] A ILB és ELB mindkét rendelt egy adott Azure felhőalapú szolgáltatásba, ezért Azure-ban bármely felhő áttelepítési fog valószínűleg azt jelenti, hogy módosítja a betöltés terheléselosztó IP.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Áttelepítése mindig a telepítési környezetének egyes legrövidebb leállás is lehetővé tevő

Van két stratégiák mindig a telepítési környezetének egyes legrövidebb leállás engedélyező áttelepítendő:

1. **További másodlagos kópiák hozzáadása meglévő mindig a fürthöz**
1. **Új mindig a fürt áttelepítése**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. további másodlagos kópiák hozzáadása meglévő mindig a fürthöz

Több stratégia is hozzá további formátumú másodlagos zónák a mindig a elérhetőség csoporthoz. Adja hozzá ezeket a felosztott egy új, felhőalapú szolgáltatásba, és a figyelő frissítése az új betöltés terheléselosztó IP-szüksége.

##### <a name="points-of-downtime"></a>Legrövidebb leállás pontjai:

- Adatérvényesítési fürt.
- Mindig a feladatátadás vizsgálata új formátumú másodlagos zónák.

Ha használja Windows tároló készletek belül a virtuális-magasabb IO kapacitása, majd ezek kell tenni kapcsolat nélkül egy teljes fürt ellenőrzés során. Az adatérvényesítési-teszt csomópontok hozzáadása a fürthöz szükség. A próba futtatásához szükséges időt változhat, tesztelje Ez a képviselő tesztkörnyezetben egy közelítő idő a mennyi ez vesz igénybe.

Hol végezhet manuális feladatátvétel és tesztelése chaos az újonnan hozzáadott csomópontok ahhoz, hogy mindig a magas elérhetősége függvények vártnak idő kell rendelkezni.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Le kell állítani a tárterület-készletek helyének SQL Server összes előfordulását az érvényesítési futtatása előtt.
##### <a name="high-level-steps"></a>Magas szintű lépések

1. Hozzon létre két új SQL-kiszolgálók csatolt prémium adathordozós új felhőszolgáltatásában.
1. Másolja a teljes biztonsági másolatok fölé, és állítsa vissza a **NORECOVERY**.
1. Másolja a vágólapra fölé "ki a felhasználó DB" függő objektumok, például a bejelentkezés stb.
1. Létrehozhat új egy új belső betöltés terheléselosztó (ILB) vagy egy külső betöltés terheléselosztó (ELB) használja, és majd állítsa be a betöltés kiegyensúlyozott végpontok mindkét új csomóponton.
> [AZURE.NOTE] Ellenőrizze minden csomópontok nincsenek-e a megfelelő végpont konfigurációját a folytatás előtt

1. Állítsa le a felhasználói/alkalmazás hozzáférést az SQL Server (Ha a tárterület-készletek használata).
1. Állítsa le az SQL Server motor Services csomópontjait (Ha a tárterület-készletek használata).
1. Új csomópontok fürtre, és futtassa a teljes érvényességi hozzáadása.
1. Miután érvényesítése sikeres, indítsa el az SQL Server szolgáltatások.
1. Transaction naplók biztonsági mentése és visszaállítása felhasználói adatbázisok.
1. Új csomópontokat illesztheti be a mindig a elérhetőség csoportban, és helyezze a **szinkron**való replikáció.
1. Adja hozzá az IP-cím erőforrás, az új felhőalapú szolgáltatás ILB/ELB Powershellen keresztül a mindig a több elem webhely példa a [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)alapján. A Windows fürt állítsa be a régi új csomópontok az **IP-cím** erőforrás **lehetséges tulajdonosok** . Olvassa el a [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)"Hozzáadása IP-cím erőforrás azonos alhálózat" szakaszát.
1. Áttérni egy új csomópontok.
1. Győződjön meg az új csomópontok automatikus feladatátvevő partnerek és a vizsgálat feladatátadás.
1. Rendelkezésre állási csoport eredeti csomópontok eltávolítása.

##### <a name="advantages"></a>Előnyei

- Az új SQL-kiszolgálókat lehet előtt mindig a hozzáadásuk tesztelje a (SQL Server és alkalmazás).
- A virtuális méretének módosítása, és testre szabhatja a tárhely a pontos követelményeknek. Azonban hasznos lenne megtartása összes SQL-görbék azonos.
- Megadhatja, hogy az adatbázis biztonsági másolatok átvitele a másodlagos replikák bekapcsolásakor. Ez különbözik használatával Azure **Start-AzureStorageBlobCopy** parancsmag VHD, mert ez egy aszinkron másolatot.

##### <a name="disadvantages"></a>Hátrányai
- Windows tároló készletek használata esetén nincs fürt legrövidebb leállás az új további csomópontok a teljes fürt ellenőrzése során.
- Attól függően, hogy az SQL Server-verzióra, és a másodlagos kópiák meglévő számát akkor előfordulhat, hogy nem tud további másodlagos kópiák hozzáadása meglévő formátumú másodlagos zónák törlése nélkül.
- Hosszú SQL adatok átviteli idő beállítása a formátumú másodlagos zónák közben is okozhatja.
- Nincs további költség áttelepítéskor közben párhuzamosan futó új gépek van.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. a új mindig a fürtre áttelepítése

Egy másik stratégia, hogy sosem csomópontok új felhőszolgáltatásában egy teljesen új mindig a fürt létrehozását, és majd irányítsa át az ügyfelek használatához.

##### <a name="points-of-downtime"></a>Legrövidebb leállás pontjai

Hiányoznak az állásidőt alkalmazások és a felhasználók az új mindig a figyelő át. Az állásidőt függ:

- Végleges tranzakció napló biztonsági mentés visszaállítása új kiszolgálókon adatbázisokhoz szükséges idő.
- Az ennyi idő ügyfélalkalmazásokban új mindig a figyelő használatához szükséges.

##### <a name="advantages"></a>Előnyei

- Ellenőrizheti, hogy a tényleges munkakörnyezetben, SQL Server, és OS hozza létre a módosításokat.
- Lehetősége van a tárhely testreszabása és esetleg a virtuális méretének csökkentése érdekében. Ez a eredményezhet költségek csökkentéséhez.
- Az SQL Server-összeállítás vagy verzió frissítheti a folyamat során. Az operációs rendszer is frissíthet.
- Az előző mindig a fürt egyszínű visszaállítás cél működhet.

##### <a name="disadvantages"></a>Hátrányai

- Módosítsa a figyelő DNS nevét, ha azt szeretné, hogy mindkét párhuzamosan futó mindig a fürt szüksége. Az áttelepítés során felsőbb felügyeleti Ez hozzáadja az az ügyfél alkalmazás karakterláncok kell megfelelően figyelő új nevét.
- A szinkronizálási mechanizmusa között a két környezetben való áttelepítés előtt az utolsó szinkronizálás követelmények lekicsinyítheti a lehető legközelebb be kell állítani.
- Nincs bekerül költség áttelepítés során, amíg az új környezet operációs rendszert futtató van.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Áttelepítése mindig a telepítés minimális legrövidebb leállás

Minimális legrövidebb leállás telepítésekhez áttelepítése mindig a két stratégiák vannak:

1. **Egy meglévő másodlagos csatlakozást: egyetlen helyet**
1. **Meglévő másodlagos Replica(s) csatlakozást: több webhelyen**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. a csatlakozást egy meglévő másodlagos: egyetlen helyet

Minimális legrövidebb leállás több stratégia egy meglévő felhő másodlagos készíthet, és eltávolítása abból az aktuális felhőszolgáltatásba. Másolja a vágólapra a VHD az új prémium tárterület-fiókra, majd az új felhőalapú szolgáltatás hozzon létre a virtuális. Frissítse a figyelő fürtözés és feladatátvevő.

##### <a name="points-of-downtime"></a>Legrövidebb leállás pontjai

- Hiányoznak az állásidőt a terheléselosztás végponttal frissíti az utolsó csomópontot.
- Az ügyfél újracsatlakozás attól függően, hogy az ügyfél és a DNS-konfigurációs tarthat.
- Ha úgy dönt, hogy az mindig fürt csoport offline állapotba cserélheti jobbra az IP-címek, nincs további legrövidebb leállás. Az IP-címe hozzáadott erőforrás-vagy függőség és a lehetséges tulajdonosok segítségével elkerülheti a. Olvassa el a [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)"Hozzáadása IP-cím erőforrás azonos alhálózat" szakaszát.

> [AZURE.NOTE] Ha azt szeretné, hogy a partnerként mindig a feladatátvevő részt vesz a hozzáadott csomópontot, kell adni a betöltés kiegyensúlyozott egy hivatkozást tartalmazó Azure zárólap hozzáadása. A **Hozzáadás-AzureEndpoint** parancs művelet futtatásakor a figyelő megnyitása, de új kapcsolatok maradjon aktuális kapcsolatok nem tudja megállapítani, amíg a terheléselosztó frissült. A tesztelés ez volt úgy, hogy utolsó 90 120seconds meg kell vizsgálni.

##### <a name="advantages"></a>Előnyei

- Nincs extra áttelepítés során felmerülő költség.
- Egy az egyhez áttelepítést.
- Csökkentett összetettsége.
- Nagyobb IOPS a prémium tároló termékváltozatok tesz lehetővé. Ha a lemezt a virtuális a leválasztott és az új felhőszolgáltatásba, másolja a 3rd fél eszköz használható, amely biztosítja magasabb teljesítmények virtuális méretének növeléséhez. Virtuális méretű növekszik, a [fórumot vitafórum](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows)talál.

##### <a name="disadvantages"></a>Hátrányai

- HA és DR ideiglenes mérvű nem áttelepítés során.
- Mivel ez 1:1 áttelepítést, be kell támogató VHD, a száma, akkor előfordulhat, hogy nem tud a VMs downsize minimális virtuális alkalmazásához.
- Ebben az esetben az Azure **Start-AzureStorageBlobCopy** parancsmag használna, amely olyan, aszinkron. Nincs semmilyen SLA példány befejezési. A szükséges idő a példányszám függ, amíg ez attól függ, várakozási sorban is függ továbbítani szeretné az adatok mennyiségét. A Másolás idő nő, ha az áthelyezés lesz egy másik Azure adatközpont támogatja a prémium tárolását egy másik tartományban lévő. Ha csak 2 csomópontok, fontolja meg egy lehetséges kezelési abban az esetben, ha a Másolás tovább tart, mint a tesztelés. Ez a következő ötletek lehetnek.
    - HA az ideiglenes 3 SQL Server csomópont hozzáadása az elfogadott legrövidebb leállás az áttelepítés előtt.
    - Futtassa az áttelepítés Azure ütemezett karbantartás kívül.
    - Gondoskodjon arról, hogy helyesen állította be a fürtkvórum.  

##### <a name="high-level-steps"></a>Magas szintű lépések

A dokumentum nem a teljes végpont példa bemutatják, azonban a [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) részletesen végrehajtani, valószínűleg kihasználják.

![MinimalDowntime][8]

- Gyűjthet a lemez konfigurációjának, és távolítsa el a csomópont (csatolt VHD nem törli).
- Prémium tárterület-fiók létrehozása és VHD másolja a szabványos tárterület-fiók
- Új felhőalapú szolgáltatás hozzon létre és telepítsen újra a SQL2 virtuális adott felhőszolgáltatásában. A virtuális a másolt eredeti OS virtuális használ, és a vágólapra másolt VHD létrehozása.
- Állítsa be a ILB / ELB, és adja hozzá a végpontok.
- Frissítés figyelő egyikével:
    - Kapcsolat nélküli véve a mindig a csoport és a mindig a figyelő frissítésekor új ILB / ELB IP-címet.
    - És IP-cím erőforrás az új felhőalapú szolgáltatás ILB/ELB Powershellen keresztül be Windows fürtszolgáltatása felvétele. Az IP-cím erőforrás lehetséges tulajdonosainak meg az áttelepített csomópontra, SQL2, majd vagy függőség a hálózat nevét az Ez legyen. Olvassa el a [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)"Hozzáadása IP-cím erőforrás azonos alhálózat" szakaszát.
- Jelölje be a DNS-konfigurációs/propagálási az ügyfeleknek.
- SQL1 virtuális áttelepítése, és a 2 – 4 lépéseivel.
- Ha lépéseket 5ii, majd ezt hozzáadhatja SQL1 lehetséges tulajdonosaként az IP-címe hozzáadott erőforráshoz
- Tesztelje a feladatátadás.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. a létező másodlagos replica(s) csatlakozást: több webhelyen

Ha egynél több Azure adatközpontban (Adatközpont) csomópontot, vagy ha egy hibrid környezet, majd használhatja mindig a konfigurációs a környezetet legrövidebb leállás céljából.

A megoldás szinkron a helyszíni vagy másodlagos Azure Adatközpont, majd fölé, hogy az SQL Server áttérni a mindig a szinkronizálás átállítása. Ezután másolja a vágólapra a VHD prémium tárterület-fiókjába, és telepítsen újra a gépet be egy új felhőszolgáltatásba. Frissítse a figyelő, és ezután visszavétele.

##### <a name="points-of-downtime"></a>Legrövidebb leállás pontjai

Az alternatív az Adatközpont és vissza való áttérés ideje az állásidőt áll. Azt is függ, hogy az ügyfél és a DNS-konfigurációs, és az ügyfél újracsatlakozás késleltethető.
Vegye figyelembe az alábbi példa a mindig a konfiguráció hibrid:

![MultiSite1][9]

##### <a name="advantages"></a>Előnyei

- Meglévő infrastruktúra is használhatja.
- Ha a beállítás a frissítés előtti először a DR Azure Adatközpont az Azure formában.
- DR Azure Adatközpont tárolására is kell újrakonfigurálni.
- Áttelepítéskor, kivéve a próba feladatátadás legalább két feladatátadás van.
- Nem kell biztonsági másolatot az SQL Server-adatok áthelyezése és visszaállítása.

##### <a name="disadvantages"></a>Hátrányai

- Attól függően, hogy az SQL Server ügyfélelérési előfordulhat, hogy nagyobb késés az SQL Server egy másik Adatközpont az alkalmazás futása közben.
- Másolás idején VHD prémium tárolóhoz hosszú lehet. Az Ön döntése e tartani a csomópont az elérhetőség csoportban a hatással lehet. Szempont idejére napló dolgozik terhelések futtatja az áttelepítés során szükséges, mivel az elsődleges csomópontok kell tartani a árvává tranzakciókat a naplóban. Ezért a tudta növelni jelentősen.
- Ebben az esetben az Azure **Start-AzureStorageBlobCopy** parancsmag használna, amely olyan, aszinkron. Nincs nincs SLA befejezését. Az idő a példányszám változik, amíg ez attól függ, várakozási sorban, is függ továbbítani szeretné az adatok mennyiségét. Ezért a 2nd adatközpont egy csomópont csak van, abban az esetben, ha a Másolás tovább tart, mint a tesztelés kezelési lépéseket kell tennie. Ez a következő ötletek lehetnek.
    - HA az ideiglenes 2nd SQL csomópont hozzáadása az elfogadott legrövidebb leállás az áttelepítés előtt.
    - Futtassa az áttelepítés Azure ütemezett karbantartás kívül.
    - Gondoskodjon arról, hogy helyesen állította be a fürtkvórum.

Ebben az esetben feltételezi, hogy az van dokumentált a telepítés, és tudja, hogyan tárolására van megfeleltetve, annak érdekében, hogy az optimális lemez gyorsítótár beállításainak végezze el a módosításokat.

##### <a name="high-level-steps"></a>Magas szintű lépések
![Multisite2][10]

- A helyszíni / Azure Adatközpont az SQL Server elsődleges másodlagos, illetve győződjön meg a többi automatikus feladatátvevő Partner (AFP).
- Lemezen konfigurációs információk összegyűjtése az SQL2, és távolítsa el a csomópont (csatolt VHD nem törli).
- Prémium tárterület-fiók létrehozása, és másolja a VHD a szabványos tárterület-fiókból.
- Hozzon létre egy új, felhőalapú szolgáltatásba, és hozzon létre a SQL2 virtuális a kapcsolódó díjak tároló lemezt.
- Állítsa be a ILB / ELB, és adja hozzá a végpontok.
- Frissítse a mindig a figyelő új ILB / ELB IP-cím és a próba feladatátvevő.
- Ellenőrizze a DNS-beállításait.
- Módosítsa a AFP SQL2, és SQL1 áttelepítéséhez, és 2 – 5 lépéseivel.
- Tesztelje a feladatátadás.
- Lépjen a AFP vissza SQL1 és SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>. Melléklet: Egy többhelyes funkció mindig a fürthöz áttelepítése prémium-tárolóhoz

Ez a témakör a többi mindig a fürt több webhelyen konvertálása prémium tároló részletes példája biztosít. Azt is alakítja a figyelő használja a külső terheléselosztó (ELB) belső terheléselosztó (ILB).

### <a name="environment"></a>Környezet

- A Windows 2k 12 / 2k SQL 12
- 1 DB fájlok SP-on
- 2 x egyes csomópontok tárterület-készletek

![Appendix1][11]

### <a name="vm"></a>VIRTUÁLIS:

Ebben a példában fogjuk helyez át a egy ELB ILB bemutatására. ELB volt ILB, mielőtt érhető el, így ezzel jeleníti meg, hogy hogyan válthat Ez az áttelepítés során.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Előtti lépéseket: Előfizetés csatlakozás

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Lépés: 1: Hozzon létre új tárterület-fiókot, és a felhő szolgáltatás
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Lépés: 2: Az erőforrások engedélyezett hibákat növelése<Optional>
Bizonyos a mindig a elérhetősége csoporthoz tartozó erőforrások korlátozás van érvényben a pontra, ahol a fürt szolgáltatás megpróbálja indítsa újra az erőforráscsoport is előfordulhatnak hány hibák. Ajánlott növelésével ezzel miközben meg vannak útmutató alapján ezt az eljárást óta Ha nem saját kezűleg feladatátvétel és a kiváltó ok mező feladatátadás érheti ezt a korlátot közelébe gépek bezárásával.

Lenne körültekintően hiba támogatás, ehhez az Feladatátvevőfürt-kezelő duplán kattintva nyissa meg a mindig a erőforrás-csoport tulajdonságait:

![Appendix3][13]

A maximális hibák 6 módosítása

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Lépés 3: Összeadás IP-cím erőforrás fürt csoport<Optional>

Ha csak egy IP-cím a fürt csoport van, és ez a felhőben alhálózat igazodik, ügyeljen arra, ha véletlenül kapcsolat nélküli csomópontjait fürt a felhőben, majd a fürt IP-erőforrás adott hálózaton és fürt hálózat neve nem tudnak online állapotba. Ezzel megelőzve frissítések megakadályozza más fürt erőforrásokhoz.

#### <a name="step-4-dns-configuration"></a>Lépés: 4: DNS-konfigurációja

Zavartalan végrehajtásához áttűnés attól függ, hogy hogyan DNS folyamatban van a kihasználtság és frissíteni.
Mindig telepítve van, amikor létrehoz egy Windows fürt erőforráscsoport Feladatátvevőfürt-kezelő nyit meg, jelenik meg, hogy legalább három erőforrások lesz, a dokumentum hivatkozó a két dolog:

- Virtuális Network Name (VNN) – Ez a tartománynév, hogy az ügyfél csatlakozni csatlakozhat az SQL Server-kiszolgálók mindig a via meg.
- IP-cím erőforrás – Ez az a VNN társított IP-címét, és egynél több lehet többhelyszínű konfigurációban egy webhely/alhálózat IP-címe lesz.

Csatlakozás SQL Server, az SQL Server ügyfél illesztőprogram fogja a DNS-rekordokat a figyelő társított beolvasásához, és próbáljon meg csatlakozni az egyes mindig a társított IP-címet, amikor alatti témákat ölelik fel néhány tényezők, amelyek befolyásolják a.

Egyidejű DNS-rekordjait a figyelő neve társított száma attól függ, hogy nem csak a megadott tartozó, IP-címek, de a "RegisterAllIpProviders'setting az, hogy az mindig a VNN erőforrás feladatátvételét.

Ha telepíti a mindig a Azure-ban különböző lépéseket is el hozza létre a figyelő és IP-címek, kézzel kell beállítania a "RegisterAllIpProviders' 1, ez különbözik és egy a helyi telepítési mindig a hol már értéke 1.

Ha a "RegisterAllIpProviders" 0, akkor csak akkor láthatja egy DNS-rekordot a DNS-ben a figyelő társított:

![Appendix4][14]

Ha a 'RegisterAllIpProviders' 1:

![Appendix5][15]

A az alábbi fog kiírása a VNN beállításokat, és megadása, felhívjuk a figyelmét arra, a változtatások csak akkor lépnek érvénybe, meg kell a VNN offline állapotba helyezni, és kapcsolja vissza online e véve a figyelő offline okozó ügyfél kapcsolódási zavarok.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

Meg kell frissíteni a mindig a figyelő frissített IP-címet, amely egy terheléselosztó hivatkoznak áttelepítési leendő Ez magában foglalja az IP-cím erőforrás eltávolítása és hozzáadása. Az IP-frissítés után kell az új IP-címet a DNS-zóna változásáról és, hogy az ügyfelek frissíteni a helyi DNS-gyorsítótár biztosítása.

Ha az ügyfelek egy másik hálózati szakaszában találhatók, és egy másik DNS-kiszolgáló hivatkozás, akkor fontolja meg, mi történik a DNS-zóna átadása az áttelepítés során, az alkalmazás újracsatlakozáshoz lesz korlátozza legalább minden új IP-címek zóna átadása idő a értesülnie. Ha az itt az ideje kényszer, érdemes megvitatása, és egy növekményes zóna letöltése a Windows a csoportoknak a kényszer, és szeretne egy alsó élettartam (TTL) is helyezi el a DNS-szolgáltatója rekord, így az ügyfelek frissítése. További tudnivalókért lásd: [Növekményes zóna átvitel](https://technet.microsoft.com/library/cc958973.aspx) és [Kezdő-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

A TTL (élettartam), a DNS-rekordot a mindig a Azure-ban a figyelő társított alapértelmezés szerint 1200 másodperc van. Előfordulhat, hogy kívánt csökkentése ezt, ha kényszer ahhoz, hogy az ügyfélprogramok az áttelepítés során a DNS frissítése a frissített IP-címet a értesülnie idő területen. Megtekintheti és konfigurációjának okai meg a VNN beállításainak módosítása:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Felhívjuk a figyelmét arra, minél kisebb az "HostRecordTTL", akkor fordul elő, a DNS-forgalom nagyobb összegű.

##### <a name="client-application-settings"></a>Ügyfél beállításai

Ha az SQL ügyfélalkalmazás támogatja a .net 4.5 SQLClient, akkor használhatja "MULTISUBNETFAILOVER = TRUE" kulcsszó, ez célszerű lehetővé teszi a gyorsabb kapcsolat SQL mindig a rendelkezésre állási csoport feladatátvételkor kell alkalmazni. Felsorolja a mindig a figyelő párhuzamosan társított összes IP-címek keresztül, és a több olyan szigorú TCP újrapróbálkozási sebességétől átadta.

A fenti beállítások kapcsolatos további tudnivalókért tekintse át a [MultiSubnetFailover kulcsszó és kapcsolódó funkciók](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). [Magas elérhetőségét, vészhelyreállítás SqlClient támogatása](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx)is megjelenik.

#### <a name="step-5-cluster-quorum-settings"></a>Lépés 5: Fürt kvórum beállításai

Egyszerre ki legalább egy SQL Server lefelé kell véve fogja, mint fürt kvórum beállítást, ha a 2 csomópontok használja a fájl megosztása tanú (FSW) módosítsa, beállíthatja a kvórum lehetővé teszi a többsége csomópontot, és dinamikus szavazás csatlakozást és állandó maradjon egy csomópontra működéséhez: az.


    Set-ClusterQuorum -NodeMajority  

Kezelését és konfigurálását fürtkvórum további tudnivalókért tekintse át a [beállítása és kezelése a Windows Server 2012 Feladatátvevőfürt kvórumokról](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Lépés a 6: Meglévő végpontok és a hozzáférés-vezérlési listák kibontása
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Mentse ezeket szövegfájlhoz.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>7 lépés: Feladatátvevő partnerek és a replikáció módok módosítása

Ha több mint 2 SQL-kiszolgálók, kell egy másik másodlagos egy másik Adatközpont, vagy a helyszíni környezetbe feladatátvételének átállítása "Szinkron" és az automatikus feladatátvevő Partner (AFP) lehetőséget, ez az, HA karbantartása, míg a módosításokat. Ehhez a TSQL keresztül bár SSMS módosítása:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>8 lépés: A másodlagos virtuális eltávolítása a felhőbeli szolgáltatástól

Érdemes tervezési egy másodlagos cloud-csomópont először áttelepítendő jelenleg elsődleges esetén kézi feladatátvevő kell kezdeményez.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Lépés a 9: Lemez gyorsítótárazás CSV-fájlban beállításainak módosítása és mentése

Az adaton ezek meg kell csak olvasható.

TLOG kötet ezek meg kell nincs értékre ÁLLÍTÁSÁVAL.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>10 lépés: Másolás VHD
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Ellenőrizheti, hogy a prémium tárterület-fiókjába a VHD másolás állapotát:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Várja meg, amíg a mind, a program sikeres formájában rögzíti.

Az egyes BLOB adatok:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Lépés 11: Külső.FÜGGV OS lemez

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Lépés a 12: Importálása a másodlagos új felhőalapú szolgáltatás

Az alábbi kód is használ a hozzáadott itt importálása a számítógép és a retainable virtuális használható.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Lépés 13: Létrehozása ILB új felhő Svc, a betöltés hozzáadása kiegyensúlyozott végpontok és a hozzáférés-vezérlési listák
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>14 lépés: A mindig frissítse
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Most, távolítsa el a régi felhőalapú szolgáltatás IP-címet.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>15 lépés: DNS-frissítés ellenőrzése

Meg kell most jelölje be a DNS-kiszolgálóiról az SQL Server-ügyfél hálózatokon, és győződjön meg arról, hogy fürtözés van a rekord felvételét elemre további host hozzáadott IP-címét. Ha ezeket a DNS-kiszolgálók frissítése nem rendelkezik, fontolja meg a DNS-zóna átvitel kényszerítése, és győződjön meg arról, hogy az ügyfelek a nincs alhálózat képesek úgy, hogy mindkét mindig a IP-címek, így Önnek nem kell megvárja, amíg az automatikus DNS-replikáció: az.

#### <a name="step-16-reconfigure-always-on"></a>Lépés 16: A mindig átkonfigurálása

Ezen a ponton várja meg a másodlagos, hogy a helyszíni csomópont teljesen szinkronizálnia és való váltáshoz szinkron replikációs csomópont és elérhetővé teszik a AFP áttelepítésének csomópont.  

#### <a name="step-17-migrate-second-node"></a>Lépés 17: A második csomópont áttelepítése
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Lépés a 18: Gyorsítótár beállításai CSV-fájlban lemez és mentése

Az adaton ezek meg kell csak olvasható.

TLOG kötet ezek meg kell nincs értékre ÁLLÍTÁSÁVAL.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Lépés 19: Másodlagos csomópont új független tárterület-fiók létrehozása
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Lépés 20: Másolás VHD
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Az összes VHD ellenőrizni a virtuális állapot másolása: ForEach (a $diskobjects $disk) {$lun = $disk. Logikai $vhdname $disk.vhdname $cacheoption = = $disk. HostCaching $disklabel = $disk. Lemezcímke $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Várja meg, amíg a mind, a program sikeres formájában rögzíti.

Kapcsolatos információk egyes BLOB: #Check induvidual blob állapot Get-AzureStorageBlobCopyState-"danRegSvcAms-dansqlams1-2014-07-03.vhd" Blob-tároló $containerName-környezet $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Lépés 21: Külső.FÜGGV OS lemez
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>22 lépés: Adja hozzá a betöltés kiegyensúlyozott végpontok és a hozzáférés-vezérlési listák
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Lépés 23: Próba feladatátvevő

Most már tájékoztatnia kell szinkronizálni a helyszíni mindig a csomópontot, helyezése a replikáció szinkron módba, és várja, akkor a rendszer szinkronizálja az áttelepített csomópontot. Kattintson az első csomópontot a helyszíni áttérni át, amely olyan, a AFP. Miután, amely foglalkozott, módosítsa az utolsó áttelepített csomópontot a AFP.

Kell tesztelése feladatátadás csomópontjait között, és futtassa azonban chaos tesztek feladatátadás munkához a várt módon, és az egy időben manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>24 lépés: Halasztani fürt kvórum beállítások / DNS TTL (élettartam) / feladatátvevő Pntrs / a szinkronizálási beállítások
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Ugyanahhoz az alhálózathoz IP-cím erőforrás hozzáadása

Ha csak 2 SQL-kiszolgálóval rendelkezik, és őket egy új felhőszolgáltatásba át szeretne térni, de szeretné megtartani az ugyanazon alhálózat, elkerülheti a figyelő offline véve törli az eredeti mindig a IP-cím, és adja hozzá az új IP-címet. Ha a VMs egy másik alhálózathoz nem kell ehhez, mint egy adott alhálózat fog hivatkozó fürt további hálózat lesz.

Miután meg az áttelepített másodlagos vett fel, és megjelenik az új IP-cím erőforrás átadása a meglévő elsődleges előtt új felhőalapú szolgáltatás, ezeket a lépéseket belül a feladatátvevő felülete kell tennie:

Az IP-cím hozzáadásához lásd: [függelék](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)lépés 14.

1. Az aktuális IP-cím erőforrás, az alábbi példában "dansqlams4" a "Meglévő elsődleges SQL Server" lehetséges tulajdonosának módosítása:

    ![Appendix13][23]

1. Az új IP-cím erőforrás "Migrated másodlagos SQL Server", az alábbi példában "dansqlams5" lehetséges tulajdonosának módosítása:

    ![Appendix14][24]

1. Amikor megadja ezt a feladatátvevő is, és amikor az utolsó csomópont áttelepítése a lehetséges tulajdonosok módosítani kell, úgy, hogy csomópont bekerül a lehetséges tulajdonosaként:

    ![Appendix15][25]

## <a name="additional-resources"></a>További források
- [Azure prémium tárhely](../storage/storage-premium-storage.md)
- [Virtuális gépeken futó](https://azure.microsoft.com/services/virtual-machines/)
- [Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
