<properties
    pageTitle="Áttelepítés az Azure prémium tároló |} Microsoft Azure"
    description="A meglévő virtuális gépeken futó áttelepítése az Azure prémium tárhelyet. Prémium tároló nagy teljesítményű, alacsony-késés lemezt támogatása a Azure virtuális gépeken futó futó lehet/O-igényes munkaterhelésekből kínál."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Azure prémium tárolóhoz áttelepítése

## <a name="overview"></a>– Áttekintés

Azure prémium tároló biztosítja e/O-igényes munkaterhelésekből futó virtuális gépeken futó nagy teljesítményű, alacsony-késés lemezt támogatása. Az alkalmazás virtuális lemezt Azure prémium tárolóhoz áttelepítésével előnyeit, a sebesség és a teljesítmény lemezt a következő készíthet.

Ez az útmutató célja, hogy az aktuális rendszerből prémium tárolóhoz így fokozatosan térhet előkészítése új felhasználók Azure prémium tároló jobb. Az útmutató címek három fő összetevői ezt a folyamatot: 

  - [Az áttelepítés prémium tárolóhoz megtervezése](#plan-the-migration-to-premium-storage)
  - [Előkészítése és prémium tárolási virtuális merevlemez (VHD) másolása](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Azure virtuális gép prémium tárolót használó létrehozása](#create-azure-virtual-machine-using-premium-storage)

VMs áttelepítése a más platformokhoz készült Azure prémium tárolóhoz, vagy meglévő Azure VMs prémium tárolási szabványos tárhelyről áttelepítése. Ez az útmutató magában foglalja mind az alábbi két forgatókönyvet lépései. Kövesse a lépéseket a megfelelő szakaszban az esete megadott.

>[AZURE.NOTE] A szolgáltatás – áttekintés és a prémium tárolási prémium tárolóban lévő árak talál: [Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárhelyet](storage-premium-storage.md). Azt javasoljuk, hogy bármelyik megkövetelése a magas IOPS Azure prémium tárolóhoz a legjobb teljesítmény elérése érdekében az alkalmazás a virtuális gép lemezre áttelepítése. Ha a lemez magas IOPS nem igényel, ez esetben megtartják a virtuális gép lemez adatokat tárolja a merevlemez-meghajtók (HDDs) helyett SSD szabványos tároló korlátozhatja költségeket.

Az áttelepítési folyamatot egészében befejezése szükség lehet további műveletek végrehajtása előtt és után ebből az útmutatóból leírt lépésekkel. Többek között virtuális hálózatok vagy végpontja beállítása vagy az alkalmazáson belül magát az alkalmazás néhány állásidőt igénylő kód változtatásokat. Az alábbi műveletek egyedi, mindegyik alkalmazásra, és őket, hogy a teljes átmenet prémium tárterület is zökkenőmentesen elvégezhető a lehető ebből az útmutatóból leírt lépésekkel együtt kell végrehajtania.


## <a name="plan-the-migration-to-premium-storage"></a>Az áttelepítés prémium tárolóhoz megtervezése

Ez a szakasz, kövesse a jelen cikk áttelepítési készen áll, és segít, hogy a legjobb döntés a virtuális és a lemez biztosítja.

### <a name="prerequisites"></a>Előfeltételek
- Szüksége lesz egy Azure-előfizetést. Ha nincs telepítve egyik, hozzon létre egy hónap [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) előfizetést, vagy keresse fel az [Azure árak](https://azure.microsoft.com/pricing/) a további beállítások.
- Végrehajtása a PowerShell-parancsmagok, szüksége lesz a Microsoft Azure PowerShell-modult. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) a telepítés pont és a telepítési utasításokat.
- Azure VMs prémium tároló futó használni kívánja, amikor akkor használja a prémium tároló alkalmas VMs. Normál és a prémium tárolási prémium tárolási alkalmas VMs használhatja. Prémium tároló lemezt érhetők el további virtuális típusú a jövőben. Elérhető Azure virtuális lemez típusú és méretű a további tudnivalókért lásd: a [virtuális gépeken futó méretét](../virtual-machines/virtual-machines-windows-sizes.md) és a [betűméretet a Cloud Services](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Megfontolandó szempontok

Az Azure virtuális támogatja a csatolása a több prémium tároló lemezt, hogy az alkalmazások beállíthatja, hogy legfeljebb 64 TB egy virtuális-tárterületét. Prémium adathordozós az alkalmazások az olvasási műveletek rendkívül alacsony késések virtuális egy második lemez átviteli per érhet egy virtuális és 2000 MB 80,000 IOPS (bemeneti és kimeneti műveletek / szekundum). A különféle VMs és a lemez lehetőség közül választhat. Ez a szakasz segít-beállítás, amely a leginkább illik a terhelést.

#### <a name="vm-sizes"></a>Virtuális méretek
[Virtuális gépeken futó méretét](../virtual-machines/virtual-machines-windows-sizes.md)az Azure virtuális méret alkalmazásra vonatkozó technikai adatok jelennek meg. Tekintse át a virtuális gépeken futó prémium tároló használata, és válassza a terhelést leginkább illik leginkább megfelelő virtuális méretét teljesítményének jellemzőit. Győződjön meg arról, hogy van elegendő a sávszélesség érhető el a virtuális meghajtóra a merevlemez-forgalmat.


#### <a name="disk-sizes"></a>Lemezen méretek
A virtuális használható lemezt a három típusba sorolhatók és az egyes adott IOPs, átviteli korlátai. Ezek a korlátok során figyelembe az alkalmazás beosztását, teljesítmény, méretezhetőség igényeinek megfelelően a lemez típusának kiválasztása a virtuális, és betölti csúcs.

|Prémium tároló lemez típusa|P10|P20|P30|
|:---:|:---:|:---:|:---:|
|Lemez mérete|128 GB|512 GB|1024 GB (1 TB)|
|Egy lemezen IOPS|500|2300|5000|
|Egy lemezen átviteli|Másodpercenként 100 MB|150 MB / szekundum|200 MB / szekundum|

Attól függően, hogy a terhelést állapítsa meg, ha további adatokat lemez szükséges a virtuális. A virtuális több állandó adatok lemezt csatolhat. Ha szükséges, a is paritásos végig a lemezt a beosztását, és a hangerő teljesítményének növeléséhez. (Ismerje meg az lemezcsíkozás [Itt](storage-premium-storage-performance.md#disk-striping).) Ha a prémium tároló adatok lemezt [Tárhelyek]használatával paritásos[4], amely egyetlen oszlopot az egyes merevlemez használt kell beállítani. Ellenkező esetben a Sávos mennyiségi teljesítménye lehet kisebb, mint a várható forgalmat páratlan terjesztési miatt a lemezen. A Linux VMs a *mdadm* segédprogrammal azonos elérése. Című cikkben [Szoftver RAID konfigurálása Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) további információt.

#### <a name="storage-account-scalability-targets"></a>Tárterület fiók méretezhetőség cél
Prémium tároló fiókok van az [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md)kívül az alábbi méretezhetőség célokat. Ha az alkalmazás igényeknek megfelelően alakíthatja, mint a méretezhetőség célok egyetlen tárolási fiók, több tárterület-fiók használata az alkalmazás összeállítása, és az adatok partition át ezeket a tárterület-fiókokat.

|Teljes fiók kapacitás|Teljes sávszélesség helyileg felesleges tárterület-fiók|
|:--|:---|
|A lemez kapacitása: 35TB<br />Pillanatkép kapacitása: 10 TB|A bejövő + kimenő másodpercenként legfeljebb 50 gigabits|

Prémium tároló jellemzői további tájékoztatást kivétele [méretezhetőség és a teljesítmény célok prémium tároló használata esetén](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Gyorsítótár-házirend lemezre
Alapértelmezés szerint gyorsítótár-házirend lemez *Írásvédett* prémium adatokhoz lemezre, és az *Írási és olvasási* Premium operációs rendszer lemez a virtuális csatolva. Az alkalmazás IOS az optimális teljesítmény eléréséhez ajánlott konfiguráció beállítás. Az adatok írási-nehéz vagy csak olvasásra lemez (például SQL Server naplófájljának) tiltsa le a lemez gyorsítótárazás, hogy jobban alkalmazás teljesítményének érhet el. A meglévő adatok lemezt gyorsítótár beállításainak frissítheti [Azure-portálon](https://portal.azure.com) vagy a *- HostCaching* paramétert a *Set-AzureDataDisk* parancsmag használatával.

#### <a name="location"></a>Hely
Válasszon egy helyet, ahol Azure prémium tárhely áll rendelkezésre. Lásd: [Azure Services régió szerint](https://azure.microsoft.com/regions/#services) elérhető helyek legfrissebb információkat. A tároló fiókként, hogy tárolja a lemezen a virtuális képet ad sok jobb teljesítményt külön régióban vannak azonos régióban található VMs.

#### <a name="other-azure-vm-configuration-settings"></a>Egyéb Azure virtuális beállításait
Az Azure virtuális létrehozása esetén a program megkéri bizonyos virtuális beállításainak konfigurálása. Ne feledje, hogy néhány beállítást rögzített a virtuális, teljes módosításához, vagy később hozzáadhatja mások közben. Tekintse át a Azure virtuális konfigurációs beállítások, és győződjön meg arról, hogy ezek van konfigurálva megfelelően az terhelést igényeknek megfelelően alakíthatja.

### <a name="optimization"></a>Optimalizálás

[Azure prémium tároló: a nagy teljesítményű Tervező](storage-premium-storage-performance.md) Azure prémium tárolót használó nagy teljesítményű alkalmazások létrehozásába nyújt útmutatást. Kövesse az útmutatásokat teljesítmény gyakorlati tanácsok az alkalmazás által használt technológiák alkalmazandó együtt.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Előkészítése és prémium tároló virtuális merevlemez (VHD) másolása

Az alábbi szakasz nyújt útmutatást VHD előkészítése az a virtuális és Azure tároló VHD másolása.

- [1 alkalmazási helyzetek: "e vagyok áttelepítése meglévő Azure VMs Azure prémium tárolóhoz."](#scenario1)
- [2 alkalmazási helyzetek: "e vagyok áttelepítése VMs a más platformokhoz készült Azure prémium tárolóhoz."](#scenario2)

### <a name="prerequisites"></a>Előfeltételek

Felkészülés a VHD az áttelepítésre, akkor van szükség:

- Az Azure előfizetéssel, a tárterület-fiók és az adott tárterület-fiókban, amelyhez a virtuális másolhatja a tároló. Ne feledje, hogy a cél tárterület-fiókot is egy normál vagy prémium tároló fiók attól függően, hogy a követelmény.
- A virtuális generalize, ha azt tervezi, hogy több virtuális példányok készítése eszközt. Ha például a Windows vagy adatb-sysprep az Ubuntu sysprep.
- Töltse fel a virtuális fájlt tároló fiókba eszközt. Lásd: az [adatok, mellettük a AzCopy parancssori segédprogram átadása](storage-use-azcopy.md) , vagy használja az [Azure tároló explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Ez az útmutató ismerteti, hogy a virtuális a AzCopy eszközzel másolása.

> [AZURE.NOTE] Ha úgy dönt, hogy a szinkronizált másolat beállítás AzCopy, az optimális teljesítmény eléréséhez, másolja a vágólapra a virtuális az eszközöket az Azure virtuális, amely a cél tárterület-fiókként ugyanabban a régióban futtatásával. Ha a másolandó virtuális merevlemez-Azure virtuális egy másik régióbeli, lehet, hogy a teljesítmény lassabban.
>
> Nagy mennyiségű adatot felülírását korlátozott sávszélesség, érdemes megfontolni [az Azure importálás/exportálás szolgáltatással adatátviteli Blob-tárolóhoz](storage-import-export-service.md); Ez lehetővé teszi, hogy az adatok átvitele szállítási merevlemez-meghajtók által az Azure adatközponthoz. Az importálás/exportálás Azure szolgáltatás segítségével adatok másolása egy szabványos tárterület-fiókjába. Miután az adatokat a szabványos tárterület-fiókjában, a [Másolás Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) vagy a AzCopy segítségével az adatok átvitele prémium tárterület-fiókját.
>
> Figyelje meg, hogy a Microsoft Azure csak támogatja-e a rögzített méretű virtuális fájlokat. VHDX fájlok vagy dinamikus VHD nem támogatottak. Ha egy dinamikus virtuális, átalakíthatja a [Konvertálás-virtuális](http://technet.microsoft.com/library/hh848454.aspx) parancsmaggal rögzített méretű.

### <a name="scenario1"></a>1 alkalmazási helyzetek: "e vagyok áttelepítése meglévő Azure VMs Azure prémium tárolóhoz."

Meglévő Azure VMs áttelepíteni kívánt, állítsa le a virtuális, a kívánt virtuális típusonként VHD előkészítése és majd másolja ki a virtuális AzCopy vagy PowerShell.

A virtuális kell lennie egy könnyen áttekinthető állapot áttelepítéséhez teljesen lefelé. Lesznek a legrövidebb leállás mindaddig, amíg az áttelepítés befejeződik.

#### <a name="step-1-prepare-vhds-for-migration"></a>Lépés: 1. Áttelepítési VHD előkészítése

Ha meglévő Azure VMs prémium tárolóhoz, a virtuális lehetnek:

- Egy általános operációs rendszer képe
- Egyedi operációs rendszer lemezen
- Adatok lemezen

Az alábbi végigvezetjük a virtuális előkészítési 3 forgatókönyvekben.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Több virtuális példányt hozzon létre egy általános operációs rendszer virtuális használatával

Ha egy virtuális több általános Azure virtuális példányok létrehozására használt program feltöltése, először az egy sysprep segédprogrammal virtuális generalize kell. Ez a cikk a virtuális, amely a helyszíni, vagy a felhőben vonatkozik. Sysprep a virtuális eltávolítja a számítógép-specifikus információkat.

>[AZURE.IMPORTANT] Készítsen egy pillanatképet, vagy biztonsági másolatot készíteni a virtuális előtt generalizing azt. A futó sysprep leállítja és felszabadítása a virtuális példány. Kövesse a Windows operációs rendszer virtuális sysprep végre az alábbi lépéseket. Figyelje meg, hogy a Sysprep parancs futtatása esetén kell, hogy állítsa le a virtuális gépen. Rendszer-előkészítő eszközről további tudnivalókért lásd: [Sysprep áttekintése](http://technet.microsoft.com/library/hh825209.aspx) vagy [Sysprep technikai útmutató](http://technet.microsoft.com/library/cc766049.aspx).

1. Nyisson meg egy parancssorablakot rendszergazdaként.
2. Írja be a Sysprep nyissa meg a következő parancsot:

        %windir%\system32\sysprep\sysprep.exe

3. A rendszer előkészítése eszközben választó írja be a rendszer Out kész felület (OOBE), jelölje be a Generalize jelölőnégyzetet, válassza a **Leállítás**, és kattintson az **OK gombra**, az alábbi képen látható módon. Sysprep az operációs rendszer generalize, és a állítsa le a rendszer.

    ![][1]

Egy Ubuntu virtuális használja a sysprep adatb azonos eléréséhez. Lásd: [a sysprep adatb](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) további információt. Lásd még: néhány egyéb Linux operációs rendszerek Megnyitás [Linux Server kiépítési szoftvert](http://www.cyberciti.biz/tips/server-provisioning-software.html) .

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Hozzon létre egy virtuális példányát egy egyedi operációs rendszer virtuális használatával

Ha egy alkalmazást a virtuális, amely a számítógépen meghatározott adatokat igényel, nem generalize a virtuális. Hozzon létre egy egyedi Azure virtuális példányt egy nem általános virtuális használható. Például ha tartományvezérlőnek a virtuális tartalmaz, sysprep végrehajtása teszi a tartományvezérlőnek hatástalan. Tekintse át az alkalmazások a virtuális és sysprep futtatja rajtuk a virtuális generalizing előtt hatását.

##### <a name="register-data-disk-vhd"></a>Adatok lemez virtuális regisztrálása

Esetén adatok lemezt áttelepítendő Azure-ban győződjön meg arról, hogy az alábbi adatok lemezt használó VMs le.

Kövesse a lépéseket, és így átmásolhatja a virtuális Azure prémium tárolóhoz kiépített adatok lemezen regisztrálja az alábbiakban leírtak.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Lépés: 2. A virtuális céljának létrehozása

A VHD karbantartásának tárterület-fiók létrehozása. A VHD helyének tervezésekor, vegye figyelembe az alábbiakat:

- A target prémium tárterület-fiókot.
- A fiók tárolóhelyen ugyanaz, mint prémium tároló az utolsó szakaszban létrehoz alkalmas Azure VMs kell lennie. Új tárterület-fiók vagy szükségletek azonos tárterület-fiók terv átmásolhatja.
- Másolja a vágólapra, és mentse a a tárhely fiókkulcs a cél tárterület-fiók a következő szakaszra.

Az adatok lemezt lehetősége van néhány adatot lemezt tartani egy szabványos tárterület-fiókkal (például lemez alacsonyabb tároló rendelkező), de nyomatékosan javasoljuk, hogy minden adat gyártási terhelés prémium tároló alkalmazás áthelyezése.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>3 a lépést. Virtuális AzCopy vagy PowerShell másolása

Meg kell keresse meg a tároló elérési utat és a tárhely fiók használatával feldolgozása az alábbi két lehetőség közül választhat. Az **Azure-portálon**található tároló elérési utat és a tárterület a fiókkulcs > **tároló**. Tároló URL-címe lesz, például "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>1 beállítást: Másolása egy virtuális a AzCopy (aszinkron másolása)

AzCopy használ, egyszerűen feltöltheti a virtuális az interneten keresztül. Attól függően, hogy a VHD méretét ezzel időt vehet igénybe. Ha ezzel a beállítással, jelölje be a fiók bejövő adatok/kilépési tárterületkorlátok ne felejtse el. [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md) talál részleteket.

1. Töltse le és telepítse a AzCopy innen: [AzCopy legújabb verziója](http://aka.ms/downloadazcopy)
2. Nyissa meg az Azure Powershellt, és nyissa meg a mappát, amelyen telepítve van a AzCopy.
3. A következő paranccsal másolja a virtuális "Forrásból" "Cél" fájlt.

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Példa:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Az alábbiakban a AzCopy parancs használt paraméterek leírása:

 - * */Forrás: * &lt;forrás&gt;:*** a mappa, illetve a tárhely tároló URL-címet, amely a virtuális tartalmazza.
 - * */SourceKey: * &lt;forrás fiókkulcs&gt;:*** tároló fiókkulcs a forrás tároló fiók.
 - * */Dest: * &lt;cél&gt;:*** másolni szeretné a virtuális tárolási tároló URL-CÍMÉT.
 - * */DestKey: * &lt;cél fiókkulcs&gt;:*** tároló fiókkulcs a cél tároló fiók.
 - * */Mintázat: * &lt;Fájlnév&gt;:*** adja meg a fájlnevet, valamint a virtuális másolni.

AzCopy eszköz használatával kapcsolatos részletekért olvassa el [a AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>2 lehetőség: Másolása egy virtuális PowerShell (Synchronized másolása)

A virtuális fájlt a PowerShell parancsmaggal Start-AzureStorageBlobCopy is másolhatja. A következő paranccsal a Azure PowerShell virtuális másolni. A forrás- és céltáblák tárterület-fiókjából megfelelő értékeket tartalmazó <> szereplő értékek cseréje Ez a parancs használatához VHD nevű a cél tárterület-fiókjában tároló kell rendelkeznie. Ha a tároló nem létezik, hozzon létre egyet az parancs futtatása előtt.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Példa:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>2 alkalmazási helyzetek: "e vagyok áttelepítése VMs a más platformokhoz készült Azure prémium tárolóhoz."

Ha az Azure virtuális a nem - Azure Felhőbeli tárhelyről vannak áttelepítette, először exportálnia kell a virtuális egy helyi mappába. Forrás teljes elérési útját a helyi könyvtár virtuális tároló praktikus rendelkezik, és AzCopy segítségével töltse fel az Azure-tárterületet.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Lépés: 1. A helyi könyvtár virtuális exportálása

##### <a name="copy-a-vhd-from-aws"></a>Másolja a virtuális AWS

1. AWS használatakor az EC2 példány exportálása egy virtuális a egy Amazon S3 időszakot. Amazon EC2 előfordulását az Amazon EC2 parancssori kezelőfelületről eszköz telepítéséhez, és futtassa a létrehozás-példány-export-tevékenység parancsot a EC2 példány exportálása virtuális fájlba exportál Amazon dokumentációjában leírt lépésekkel. Ügyeljen arra, hogy a lemez #95; a kép & #95; **virtuális** használata FORMÁTUM változó fut a **- példány-export-feladat létrehozása** parancsot. Az exportált fájl virtuális program menti az Amazon S3 időszakot jelöl ki, hogy a folyamat során.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. A virtuális fájl letöltése a S3 gyűjtő. Jelölje ki a virtuális fájlt, majd a **Műveletek** > **letöltése**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>A virtuális másolása más nem Azure felhőből

Ha az Azure virtuális a nem - Azure Felhőbeli tárhelyről vannak áttelepítette, először exportálnia kell a virtuális egy helyi mappába. Másolja a teljes forrás elérési útját a virtuális tároló helyi könyvtár.

##### <a name="copy-a-vhd-from-on-premise"></a>Másolja a virtuális helyszíni:

Ha helyszíni környezetben van virtuális áttérés, szüksége lesz a teljes forrás elérési tárolási helyére a virtuális. A forrás elérési útja lehet egy kiszolgáló helyét vagy fájlmegosztáson.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Lépés: 2. A virtuális céljának létrehozása

A VHD karbantartásának tárterület-fiók létrehozása. A VHD helyének tervezésekor, vegye figyelembe az alábbiakat:

- A szóban forgó tároló fiókot lehet attól függően, hogy az alkalmazás követelmény normál vagy magasabb szintű tároló.
- A tároló fiók terület ugyanaz, mint prémium tároló az utolsó szakaszban létrehoz alkalmas Azure VMs kell lennie. Új tárterület-fiók vagy szükségletek azonos tárterület-fiók terv átmásolhatja.
- Másolja a vágólapra, és mentse a a tárhely fiókkulcs a cél tárterület-fiók a következő szakaszra.

Nyomatékosan javasoljuk, hogy áthelyezése minden adat gyártási terhelés prémium tároló használatára.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>3 a lépést. Töltse fel a virtuális Azure-tárolóhoz

Most, hogy a virtuális a helyi címtárban, AzCopy vagy AzurePowerShell segítségével feltöltése a .vhd Azure-tárhelyet. Mindkét kérdésre itt találhatók:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>1 beállítást: Azure PowerShell hozzáadása-AzureVhd használatával a .vhd fájl feltöltése

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Példa <Uri> lehet, hogy **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Példa <FileInfo> lehet, hogy **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>2 beállítás: A .vhd fájl feltöltése AzCopy használatával

AzCopy használ, egyszerűen feltöltheti a virtuális az interneten keresztül. Attól függően, hogy a VHD méretét ezzel időt vehet igénybe. Ha ezzel a beállítással, jelölje be a fiók bejövő adatok/kilépési tárterületkorlátok ne felejtse el. [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md) talál részleteket.

1. Töltse le és telepítse a AzCopy innen: [AzCopy legújabb verziója](http://aka.ms/downloadazcopy)
2. Nyissa meg az Azure Powershellt, és nyissa meg a mappát, amelyen telepítve van a AzCopy.
3. A következő paranccsal másolja a virtuális "Forrásból" "Cél" fájlt.

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Példa:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Az alábbiakban a AzCopy parancs használt paraméterek leírása:

 - * */Forrás: * &lt;forrás&gt;:*** a mappa, illetve a tárhely tároló URL-címet, amely a virtuális tartalmazza.
 - * */SourceKey: * &lt;forrás fiókkulcs&gt;:*** tároló fiókkulcs a forrás tároló fiók.
 - * */Dest: * &lt;cél&gt;:*** másolni szeretné a virtuális tárolási tároló URL-CÍMÉT.
 - * */DestKey: * &lt;cél fiókkulcs&gt;:*** tároló fiókkulcs a cél tároló fiók.
 - **/BlobType: lap:** Megadja, hogy a cél lap blob-e.
 - * */Mintázat: * &lt;Fájlnév&gt;:*** adja meg a fájlnevet, valamint a virtuális másolni.

AzCopy eszköz használatával kapcsolatos részletekért olvassa el [a AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Egyéb lehetőségek a virtuális feltöltése

Egy virtuális is feltöltheti a tárhely fiókja végre a következő módon:

- [Azure tároló másolás Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure tároló Explorer feltöltése BLOB](https://azurestorageexplorer.codeplex.com/)
- [Tárterület importálás/exportálás szolgáltatás REST API-hivatkozás](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Azt javasoljuk, hogy az importálás/exportálás szolgáltatást használja, ha 7 napnál hosszabb időt tölt fel, a becsült. Az adatok méretét és a továbbított egységből idő becsléséhez [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) is használhatja.
>
> Az importálás/exportálás használható másolja egy szabványos tárterület-fiókba. Meg kell másolja szabványos tárhelyről prémium tároló fiókba, például AzCopy eszköz használata.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Azure VMs prémium tárolót használó létrehozása

Után a virtuális feltöltött vagy másolni kívánt tárterület-fiókjába, kövesse az ebben a szakaszban a virtuális regisztrálhat-OS képként, vagy az operációs rendszer lemezre az esete, és egy virtuális példány készítése című témakörben leírt útmutatást. Az adatok lemezre virtuális a virtuális kapcsolódhat, a már létrehozott. Ez a szakasz végén áttelepítési mintaparancsfájl megadva. Egyszerű parancsfájl nem egyezik meg a minden esetben. Előfordulhat, hogy frissítenie kell a parancsfájlt, és az adott forgatókönyv megfelelően. Szeretné látni, ha ezt a parancsfájlt igényektől vonatkoznak, olvassa el [A áttelepítési mintaparancsfájl](#a-sample-migration-script).

### <a name="checklist"></a>Feladatlista

1.  Várja meg, amíg másolása virtuális lemezt befejeződött.
2.  Ellenőrizze, hogy prémium tároló érhető el a szeretné áttelepíteni kívánt tartományban lévő.
3.  Döntse el, hogy az új virtuális adatsor fogja használni. Egy alkalmas prémium tárterületet kell tenni, és a méret célszerű lehet attól függően, a régióban elérhető, és szükségletek.
4.  Adja meg a használni kívánt pontos virtuális méretét. Virtuális méret kell lennie elég nagy támogatja az adatok lemez van számát. Pl. Ha 4 adatok lemezre, a virtuális a 2-es vagy több magmintákat kell rendelkeznie. Célszerű power, memória feldolgozása is, és a hálózati sávszélességre van szüksége.
5.  Prémium tárterület-fiók létrehozása a célhely régióban. Ez az a fiókot, de kell használni az új virtuális.
6.  Részletet az aktuális virtuális hasznos, beleértve a lemez és a megfelelő virtuális BLOB listáját.

Készítse elő az állásidőt alkalmazásból. Végezze el egy könnyen áttekinthető áttelepítést, minden feldolgozás leállítása a jelenlegi rendszerben van. Csak ezután érheti azt az új platformra való áttelepítheti egységes állapotra. Legrövidebb leállás időtartam áttelepítendő lemezt adatok mennyiségét függ.

>[AZURE.NOTE] Ha egy erőforrás-kezelő Azure virtuális speciális virtuális lemezről hoz létre, olvassa el [ezt a sablont](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) üzembe helyezése a meglévő lemezzel erőforrás-kezelő virtuális.

### <a name="register-your-vhd"></a>A virtuális regisztrálása

Hozzon létre egy virtuális az operációs rendszer virtuális vagy lemezen adatok csatolása egy új virtuális regisztrálnia kell őket. Hajtsa végre az alábbi lépéseket a virtuális esete.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Általános operációs rendszer virtuális több Azure virtuális példány létrehozása

Általános OS kép virtuális feltöltése a tárterület-fiókjába, után regisztrálja az **Azure virtuális képe** , hogy egy vagy több virtuális példányát a azt is létrehozhat. Az alábbi PowerShell-parancsmagok segítségével regisztrálhatja a virtuális Azure virtuális OS képként. Adja meg a készültségi tárolóhoz URL-CÍMÉT, ahol virtuális másolta.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Másolja a vágólapra, és mentse a új Azure virtuális kép nevével. A fenti példában *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Egyedi operációs rendszer virtuális egyetlen Azure virtuális példány létrehozása

Az egyedi OS virtuális feltöltése a tárterület-fiókjába, után nyilvántartásba ezt az **Azure-OS lemez** , hogy egy virtuális példányt belőle hozhat létre. A virtuális regisztrálni, az Azure-OS lemez PowerShell-parancsmagok használata Adja meg a készültségi tárolóhoz URL-CÍMÉT, ahol virtuális másolta.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Másolja a vágólapra, és mentse a új Azure-OS lemez nevét. A fenti példában *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Adatok új Azure virtuális példányok csatolni virtuális lemezre jelölőnégyzetből.

Az adatok lemez virtuális feltöltése tárterület-fiókjába, után nyilvántartásba ezt az Azure adatok lemez, hogy az új Tartományi adatsor DSv2 sorozat vagy GS sorozat Azure virtuális példány kapcsolható.

A virtuális regisztrálni, az Azure adatok lemez PowerShell-parancsmagok használata Adja meg a készültségi tárolóhoz URL-CÍMÉT, ahol virtuális másolta.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Másolja a vágólapra, és mentse a új Azure adatok lemez nevét. A fenti példában *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Hozzon létre egy prémium tároló alkalmas virtuális

Az operációs rendszer kép egyszer vagy OS lemez regisztrált, új Tartományi adatsort, DSv2-sorozat vagy GS-sorozat virtuális létrehozása. Fogja használni, akkor az operációs rendszer kép vagy regisztrált operációs rendszer lemez nevét. Válassza ki a virtuális típusát a prémium tároló réteg. Az alábbi példában a *Standard_DS2* virtuális méretének használja azt.

>[AZURE.NOTE] Frissítse a lemez méretét, hogy a kapacitása és teljesítménnyel kapcsolatos követelmények és Azure szabad mérete megegyezik.

Kövesse a lépésenkénti PowerShell-parancsmagok az új virtuális létrehozásához az alábbi. Első lépésként állítsa be az általános paramétereket:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Első lépésként hozzon létre egy felhőalapú szolgáltatásba, amelyben, amelyen az új VMs.

    New-AzureService -ServiceName $serviceName -Location $location

Ezután az esete létrehozása az Azure virtuális példány OS képe vagy OS lemez regisztrált.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Általános operációs rendszer virtuális több Azure virtuális példány létrehozása

Hozzon létre az egy vagy több új Tartományi sorozat Azure virtuális példányokat az **Azure-OS kép** regisztrálta. Adja meg a OS kép neve nevet a virtuális konfiguráció új virtuális létrehozásakor, alább látható módon.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Egyedi operációs rendszer virtuális egyetlen Azure virtuális példány létrehozása

Új Tartományi sorozat Azure virtuális példány létrehozása az **Azure-OS lemez** regisztrálta. Adja meg a OS lemez neve a virtuális konfiguráció a új virtuális létrehozásakor, alább látható módon.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Adja meg, hogy más Azure virtuális információkat, például egy felhőalapú szolgáltatás, a régió, a tárterület-fiók, a elérhetőségének beállítása és a gyorsítótár házirend. Ne feledje, hogy a virtuális példány kell közös található társított operációs rendszer vagy adatok lemezre, így a kijelölt felhőalapú szolgáltatás, a terület és a tárolás fiókot kell lenniük a máshol, mint az alapul szolgáló VHD ezeket a lemezt a.

### <a name="attach-data-disk"></a>Adatok lemez csatolása

Végül ha adatok lemez VHD van regisztrálva, csatolhat őket az új prémium tároló alkalmas Azure virtuális.

Segítségével követően PowerShell-parancsmag adatok lemezre csatolása a új virtuális, és adja meg a gyorsítótár házirend. Az alábbi példában a gyorsítótár házirend értéke *csak olvasható*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] További lépésekre lehet szükség esetén az alkalmazás, amely támogatja az útmutató nem vonatkoznak.

### <a name="checking-and-plan-backup"></a>Ellenőrzés és a biztonsági mentés megtervezése

Miután a új virtuális lépéseket, elérni az ugyanazon felhasználónév használatával, és jelszó megegyezik az eredeti virtuális, és ellenőrizze, hogy minden várt módon működik. Az összes a beállítások, beleértve a sávos kötet lenne a új virtuális szerepel.

Az utolsó lépésként biztonsági mentés megtervezése és az alkalmazás szükséges az új virtuális karbantartási ütemezés szerint.

### <a name="a-sample-migration-script"></a>Áttelepítési mintaparancsfájl

Ha több VMs áttelepítendő, keresztül PowerShell-parancsfájlokat automatizálási hasznos lesz. Az alábbiakban, amely egy virtuális áttelepítése automatizálja mintaparancsfájl. Megjegyzés alatt parancsfájl Ez csak egy példa, illetve hogy az aktuális virtuális lemezre kapcsolatos néhány feltételezések. Előfordulhat, hogy frissítenie kell a parancsfájlt, és az adott forgatókönyv megfelelően.

A feltételezések a következők:

- Klasszikus Azure VMs készít.
- A forrás-OS lemez és a forrás adatainak lemezre tárolási ugyanazzal a fiókkal és ugyanabban a tárolóban szerepelnek. Ha az operációs rendszer lemezen és a adatokat tartalmazó lemezen nem ugyanazon a helyen, AzCopy vagy Azure PowerShell használatával VHD másolása tároló fiókok és tárolók felett. Az előző lépésben vonatkoznak: [Másolás virtuális AzCopy vagy PowerShell](#copy-vhd-with-azcopy-or-powershell). Ezt a parancsfájlt felel meg az igényektől szerkesztése másra, de azt ajánljuk, AzCopy vagy a PowerShell használatával könnyebben és gyorsabban nehézsége.

Az automatizálási parancsfájl alatt áll rendelkezésre. Cserélje ki a szöveget, és frissítse a parancsfájlt, és az adott forgatókönyv megfelelően.

>[AZURE.NOTE] A meglévő parancsfájl használatával nem őrzi meg a forrást virtuális hálózati konfigurációja miatt. Meg kell re-config a hálózati beállítások meg az áttelepített VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimalizálás

A jelenlegi virtuális beállításokat is testre szabható kifejezetten jól használható normál lemezt. Ha például a teljesítmény növelése érdekében a sávos kötet sok lemezt használatával. Például helyett 4 lemezre külön-külön prémium tárolón, akkor a költség optimalizálhatja úgy, hogy egyetlen lemez. Optimalizálásokat, mint ez esetben alapon kezelhető, és csak az egyéni lépéseket az áttelepítés után kell. Emellett látható, hogy ezt a folyamatot jól nem működnek a és az alkalmazásokat, attól függenek, a lemez elrendezése definiált beállítását.

##### <a name="preparation"></a>Előkészítése

1.  Töltse ki az egyszerű áttelepítés, a korábbi szakaszban leírt módon. Optimalizálásokat az áttelepítés után az új virtuális fog történni.
2.  Adja meg az új lemezre méretű optimalizált konfiguráció szükséges.
3.  Határozza meg az új lemez előírásoknak aktuális lemez/kötet megfeleltetésének.

##### <a name="execution-steps"></a>A lépések végrehajtása

1.  Hozzon létre új lemezt a prémium tároló virtuális a megfelelő méretű.
2.  Jelentkezzen be a virtuális és az adatok másolása, az aktuális kötet, hogy a kötet megfelelteti új lemezre. Ehhez a jelenlegi kötet feleltesse meg az új lemez kell.
3.  Ezután váltani szeretne az új lemezre alkalmazás beállításainak módosítása, és a régi kötet leválasztása.

Lemezre teljesítmény növelése érdekében az alkalmazás javítása, kérjük, tekintse át az [Alkalmazás teljesítményének optimalizálása](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Alkalmazás-áttelepítés

Adatbázisok és más összetett alkalmazások szükség lehet speciális lépéseket az áttelepítés az alkalmazás-szolgáltató által meghatározott. Saját alkalmazás dokumentációjában találhat. Pl. a szokásos adatbázisok telepíthető át keresztül biztonsági mentése és visszaállítása.

## <a name="next-steps"></a>Következő lépések

A különböző forgatókönyvekben áttelepítése virtuális gépeken futó az alábbi forrásokban talál:

- [Azure virtuális gépeken futó tároló fiókok közötti áttelepítése](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Hozzon létre, és töltse fel a Windows Server virtuális Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Létrehozásáról és a Linux operációs rendszert tartalmazó virtuális merevlemez feltöltése](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Microsoft Azure az Amazon AWS áttelepítése virtuális gépek](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Az alábbi források, ha többet szeretne tudni az Azure-tárhely és Azure virtuális gépeken futó is látható:

- [Azure tárhely](https://azure.microsoft.com/documentation/services/storage/)
- [Azure virtuális gépeken futó](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Prémium tárterület: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
