<properties
    pageTitle="Bizonyos VMware virtuális gépeken futó és fizikai kiszolgálók az Azure az Azure-webhely helyreállításhoz |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan Azure webhely helyreállítási való replikáció, feladatátvétel és helyreállítási helyszíni VMware virtuális gépeken futó és Azure-kiszolgálók fizikai Windows vagy Linux téve telepítéséhez."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>Párhuzamos VMware virtuális gépeken futó és Azure az Azure-webhely helyreállításhoz fizikai kiszolgálók

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmware-to-azure.md)
- [Klasszikus portál](site-recovery-vmware-to-azure-classic.md)
- [Klasszikus Portal (régi)](site-recovery-vmware-to-azure-classic-legacy.md)


A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md).

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogy miként:

- **Azure való replikáció VMware virtuális gépeken futó**– webhely helyreállítási üzembe való replikáció, feladatátvevő és Azure tárolóhoz a helyszíni VMware virtuális gépeken futó helyreállítás koordinálása.
- **Fizikai Azure-kiszolgálók bizonyos**– Azure webhely helyreállítási üzembe való replikáció, feladatátadási és helyreállítási helyszíni fizikai Windows és Linux kiszolgálók Azure koordinálása.

>[AZURE.NOTE] Ez a cikk ismerteti, hogyan Azure való replikáció. Ha azt szeretné, való replikáció VMware VMs vagy a Windows vagy Linux fizikai kiszolgálók egy másodlagos adatközponthoz, kövesse a [Ez a cikk](site-recovery-vmware-to-vmware.md).

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.

## <a name="enhanced-deployment"></a>Továbbfejlesztett telepítési

Ez a cikk tartalmazza a klasszikus Azure portálon egy továbbfejlesztett telepítési útmutatást tartalmaz. Azt javasoljuk, hogy minden új telepítési verziójú böngészőt használ. Ha korábban már telepítette a korábbi régebbi verzióját használja azt javasoljuk, hogy az új verzió való áttelepítése. Olvassa el a [További](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) információ a névjegyek áttelepítése.

A továbbfejlesztett példányban a fő frissítés. Az alábbiakban összefoglaljuk az végeztünk fejlesztéseket:

- **Nincs infrastruktúra VMs Azure-ban**: adatok a replikáció közvetlenül átmásolja Azure tárterület-fiókkal. Ezeken a replikáció és feladatátvevő nincs szükség állíthat be minden olyan infrastruktúra VMs (konfigurációs kiszolgálója, fő tároló kiszolgáló), azt a régi példányban szükség szerint.  
- **Egységesített telepítési**: egyetlen telepítés egyszerű beállítása és a helyszíni összetevő méretezhetősége biztosít.
- **Telepítési biztonságos**: minden forgalom titkosítva van, és a replikáció management kommunikáció küldjük HTTPS 443-as fölé.
- **Helyreállítási pontok**: összeomlik és az alkalmazás egységes helyreállítási támogatása a Windows és Linux környezetekhez mutat, és mindkét támogatja egy virtuális és a többszörös-virtuális egységes beállításokat.
- **Tesztelni**: Azure gyártási érintő vagy felfüggesztése replikációs nélkül nem zavaró próba áttérni támogatása.
- **Tervezett feladatátvevő**: Azure tervezett áttérni lehetőségével továbbfejlesztett leállítása VMs automatikus átadása előtt támogatása.
- **Visszaállás**: csak a delta módosításokat másolásához a helyszíni webhelyek integrált visszaállás.
- **vSphere 6.0**: korlátozott támogatás VMware Vsphere 6.0 telepítésekhez.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Hogyan segíti a webhely helyreállítási virtuális gépeken futó és fizikai kiszolgálók védelme?


- VMware rendszergazdák beállíthatják üzleti munkaterhelésekből és VMware virtuális gépeken futó alkalmazások Azure helyszínen védelmet. Kiszolgáló menedzserek hogy bizonyos helyszíni fizikai Windows és Linux kiszolgálók az Azure.
- Az Azure webhely helyreállítási konzolja egyetlen helyet biztosít az egyszerű beállítása és kezelése a replikáció, feladatátadási és helyreállítási folyamat.
- Bizonyos VMware virtuális gépeken futó vCenter kiszolgáló által kezelt, ha a webhely helyreállítási e VMs automatikusan, észleljék. Ha gépek ESXi állomáson webhely helyreállítási VMs az állomáson találja.
- Egyszerű feladatátadás futtassa a helyszíni infrastruktúra Azure és csomópontoktól (visszaállítás) az Azure virtuális VMware kiszolgálók a helyszíni webhely.
- Állítsa be a helyreállítási-csomagok szintén alkalmazás munkaterhelésekből, amely több számítógépen is többszintű egy csoportba. Sikertelen lehet ezeket a tervek fölé, és a webhely helyreállítási biztosít többszörös-virtuális összhangot, hogy az azonos munkaterhelésekből futtató számítógépeken állíthatók helyre közös egységes adatok pontra.


## <a name="supported-operating-systems"></a>Támogatott operációs rendszerek

### <a name="windows64-bit-only"></a>Windows (csak a 64 bites)
- A Windows Server 2008 R2 SP1 +
- A Windows Server 2012
- A Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (csak a 64 bites)
- Piros kalap vállalati Linux 6,7, 7.1, 7.2.
- CentOS 6.5 6.6 6,7, 7.0-s, 7.1, 7.2.
- Az Oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3)
- SUSE Linux vállalati kiszolgáló 11 SP3

## <a name="scenario-architecture"></a>Forgatókönyv architektúra

Forgatókönyv összetevők:

- **Egy helyszíni management server**: A management server webhely helyreállítási összetevők futtatása:
    - **Konfigurációs kiszolgálója**: koordinálja a kommunikációt és az adatok replikációs és helyreállítási folyamat kezeli.
    - **Folyamatábra-kiszolgáló**: replikációs átjáró működik-e. Azt adatok fogad védett forrás gépek, optimalizálja a gyorsítótár, a tömörítési és titkosítás és replikációs adatokat küld a Azure tároló. A védett gépek mobilitás szolgáltatás leküldéses telepítését kezeli és a VMware VMs automatikus felderítését hajt végre.
    - **A mesterlapok tároló kiszolgáló**: replikációs adatkezelési az Azure visszaállás során.
    Egy, amely végpontként egy folyamat csak kiszolgáló, annak érdekében, hogy a üzembe méretezni kiszolgálóra is telepítheti.
- **A mobilitás szolgáltatás**: Ez az összetevő telepítve van egyes gépi (VMware virtuális vagy fizikai kiszolgáló) Azure való replikáció kívánt. Azt rögzíti a adatot ír a számítógépen, és a folyamat kiszolgáló úgy továbbítja.
- **Azure**: nem kell minden Azure VMs replikációs és feladatátvételi kezelése létrehozása. A webhely helyreállítási szolgáltatás kezelése az adatkezelési, és az adatok a replikáció közvetlenül átmásolja Azure tároló. Replikált Azure VMs vannak is fel automatikusan csak akkor, amikor Azure áttérni fordul elő. Jó helyen jár Ha azt szeretné, hogy a helyszíni webhely vissza az Azure meghiúsító szüksége lesz állíthat be egy Azure virtuális folyamat kiszolgálója.


Az ábra bemutatja, hogy ezek az összetevők együttműködését.

![architektúra](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Ábra 1: VMware/fizikai az Azure** (Henry Robalino által létrehozott)


## <a name="capacity-planning"></a>Kapacitás tervezése

Amikor tervez beosztását, itt meg kell figyelni:

- **A forrás-környezet**– kapacitás tervezés és a VMware infrastruktúra- és a forrás gépi követelmények teljesítése érdekében.
- **A management server**– a helyszíni kezelési kiszolgálók, amelyek a webhely helyreállítási összetevők futtatása megtervezése.
- **Cél forrástól hálózati sávszélesség**-replikációs között a forrás- és Azure szükséges sávszélességet megtervezése

### <a name="source-environment-considerations"></a>Adatforrás-környezet előtt megfontolandó kérdések

- **Napi maximális sebességének módosítása**– egy védett számítógépre csak egy folyamatábra-kiszolgáló használható, és egyetlen folyamat kiszolgáló legfeljebb 2 TB adatmódosítás napi képes kezelni. 2 TB így, a maximális napi adatok módosítása ráta, amely egy védett számítógépre támogatott.
- **Maximális átviteli**– egy replikált számítógépre tartozhat Azure-ban egy tárterület-fiókjába. Egy szabványos tároló fiók legfeljebb 20 000 kérelmek másodpercenként képes kezelni, és azt javasoljuk, hogy őrizze meg a forrás gépi IOPS keresztben 20 000. A példa esetén készülék 5 lemez és az egyes merevlemez forrás hoz létre 120 IOPS (méret 8K) a forrás lesz az Azure lemez IOPS legfeljebb 500 / belül. A szám kötelező tároló fiókok = a teljes source IOPs/20000.


### <a name="management-server-considerations"></a>Management server kapcsolatos szempontok

A management server webhely helyreállítási összetevők, amelyet kezelni az adatok optimalizálás, a replikáció és a kezelés fut. Látnia kell kezelni a napi módosítása ráta kapacitás védett gépeken futó összes feladatok keresztül, és folyamatosan bizonyos adatok Azure tárolóhoz elég sávszélességgel rendelkezik. Kifejezetten:

- A folyamat kiszolgáló védett gépek replikációs adatok fogad, és optimalizálja a gyorsítótár, a tömörítési és titkosítás Azure küldése előtt. A kulcskezelő kiszolgálótól kell tartalmaznia az alábbi feladatok kellő erőforrások.
- A folyamat kiszolgáló használja a lemezgyorsítótár alapján. Azt javasoljuk, hogy külön gyorsítótár lemezen 600 GB vagy több kezelje a hálózati szűk vagy üzemszünetek tárolt adatok módosítását. A telepítés során beállíthatja a gyorsítótár minden olyan meghajtón, amelynek legalább 5 GB rendelkezésre álló tárhely, de 600 GB, a minimális ajánlási.
- A legjobb azt javasoljuk, hogy a management server helyezkednek el a ugyanabba a hálózatba, és a szegmenst a védelemmel ellátni kívánt gép szerint. Egy másik hálózaton található, de a védelemmel ellátni kívánt gépek 3. a hálózati láthatóság rá kell rendelkeznie.

A management server méret javaslatok az alábbi táblázatban soroljuk fel.

**Management server Processzor** | **A memória** | **Lemezen gyorsítótárméret** | **Adatok sebességének módosítása** | **Védett gépek**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es) | A 16 GB | 300 GB | 500 GB vagy annál kisebb | Telepítse egy kiszolgálóra való replikáció 100-nál kevesebb gépek ezekkel a beállításokkal.
12 vCPUs (2 sockets * 6 magmintákat @ 2,5 GHz-es) | 18 GB | 600 GB | 1 TB 500 GB | Ezekkel a beállításokkal való replikáció 100-150 gépek között egy management server telepítése.
a 16 vCPUs (2 sockets * 8 magmintákat @ 2,5 GHz-es) | 32 GB | 1 TB | 1 TB és 2 TB | Ezekkel a beállításokkal való replikáció 150-200 gépek között egy management server telepítése.
Egy másik folyamat server telepítése | | | > 2 TB | További folyamat kiszolgálók üzembe, ha meg van legfeljebb 200 gépek, végre vagy ha módosítja a napi adatai ráta meghaladja a 2 TB-ot.

Ha:

- Minden egyes adatforrás gépen be van állítva 3 lemezt 100 GB.
- 8 Társítások meghajtók 10 k RPM összehasonlítási tárolására azt használt RAID 10 gyorsítótár lemez mértékekhez.

### <a name="network-bandwidth-from-source-to-target"></a>Hálózati sávszélesség forrásból származó célba
Győződjön meg arról, amely a kezdeti replikációs és delta replikációs a [kapacitás Csapattervező eszköz](site-recovery-capacity-planner.md) szükséges sávszélességet számítása

#### <a name="throttling-bandwidth-used-for-replication"></a>A replikáció használt sávszélesség szabályozása

Azure replikált VMware forgalom Ugrás az adott folyamat-kiszolgálón keresztül. Rendelkezésre álló hely helyreállítási replikációs kiszolgálón tárolt következőképpen sávszélesség is szabályozása:

1. Nyissa meg a Microsoft Azure biztonsági másolat beépülő modult a fő management server vagy egy további futtató kiszolgálóra kiépítve folyamat kiszolgálókhoz. Alapértelmezés szerint a helyi a Microsoft Azure biztonsági másolat készül az asztali vagy megtalálhatja a: C:\Program Files\Microsoft Azure helyreállítási szolgáltatások Agent\bin\wabadmin.
2. A beépülő modult a **Tulajdonságainak módosítása**gombra.

    ![A sávszélesség szabályozása](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. A **Throttling** lapon adja meg a sávszélesség a webhely helyreállítási replikációs és az alkalmazandó ütemezés használható.

    ![A sávszélesség szabályozása](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Másik lehetőségként is beállíthatja, hogy szabályozásának a PowerShell használatával. Lássunk egy példát:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Teljes méretre állítása sávszélesség-használat
A replikáció Azure webhely helyreállítási által használt sávszélesség növelése kellene egy beállításkulcs módosítása.

A következő kulcsot szabályozza a lemez replikálása per verzióazonosítójának használt szálak számát

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 Egy "overprovisioned" hálózat az alábbi beállításkulcs van szüksége az alapértelmezett értékek megváltozzanak. 32 legfeljebb támogatjuk.  


[Tudjon meg többet](site-recovery-capacity-planner.md) is megtudhat a részletes kapacitás tervezés.

### <a name="additional-process-servers"></a>További folyamat kiszolgálók

Ha több mint 200 gépek védelme kell vagy napi módosítása ráta értéke nagyobb, 2 TB is hozzáadhat a betöltés kezelése további kiszolgálók. Ha át kívánja méretezni, hogy végezheti el:

- Adatkezelési kiszolgálók számának növelése Példa megvédheti legfeljebb 400 gépek két management kiszolgálóval.
- További folyamat kiszolgálók hozzáadása és forgalom (vagy helyett kívül) kezelésére használható a management server.

Az alábbi táblázat ismerteti, amelyben példa:

- Az eredeti management server beállítása csak konfigurációs kiszolgálójával használatához.
- További folyamat kiszolgáló beállítása.
- Védett virtuális gépeken futó további folyamat a kiszolgáló konfigurálása
- Minden védett forrás gépen be van állítva három lemez 100 GB.

**Eredeti management server**<br/><br/>(konfigurációs kiszolgálója) | **További folyamat kiszolgáló**| **Lemezen gyorsítótárméret** | **Adatok sebességének módosítása** | **Védett gépek**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es), a 16 GB memóriát | 4 vCPUs (2 sockets * 2 magmintákat @ 2,5 GHz-es), 8 GB memóriát | 300 GB | 250 GB vagy annál kisebb | Kisebb, vagy 85 gépek, hogy bizonyos.
8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es), a 16 GB memóriát | 8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es), 12 GB memóriát | 600 GB | 1 TB 250 GB | Hogy bizonyos a 85-150 gépek között.
12 vCPUs (2 sockets * 6 magmintákat @ 2,5 GHz-es), 18 GB memóriát | 12 vCPUs (2 sockets * 6 magmintákat @ 2,5 GHz-es) 24 GB memóriát | 1 TB | 1 TB és 2 TB | Hogy bizonyos, 150-225 gépek között.


Amelyben a kiszolgálók méretezése módja attól függenek, a beállításait a skálával felfelé, vagy a modell méretezése.  Néhány csúcsteljesítményű kezelési és folyamat kiszolgálók üzembe helyezésével méretezni, vagy kevesebb erőforrásokkal további kiszolgálók üzembe helyezésével méretezése. Példa: Ha módosítani szeretné a 220 gépek védelme, sikerült végezze el az alábbiak egyikét:

- Állítsa be az eredeti management server 12vCPU, a 18 GB memóriát, egy további folyamat server 12vCPU, 24 GB memóriát, és a védett gépek használata csak a további folyamat kiszolgáló konfigurálása.
- Vagy azt is megteheti, sikerült két management (2 x 8vCPU, 16 GB RAM) és két további folyamat kiszolgálók (x 8vCPU 1) és 4vCPU x 1 135 + 85 (220) gépek kezelésére, valamint konfigurálni védett gépek használata csak a további folyamat kiszolgálók.


[Az alábbi útmutatás](#deploy-additional-process-servers) állíthat be egy további folyamat kiszolgáló.

## <a name="before-you-start-deployment"></a>Telepítés megkezdése előtt

A táblázat összefoglalja a üzembe helyezése ebben az esetben előfeltételei.


### <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

**Előfeltételek** | **Részletek**
--- | ---
**Azure-fiók**| Akkor, ha a [Microsoft Azure](https://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**Azure tárhely** | Azure tároló fiók replikált adatok tárolására lesz szüksége. Azure tároló replikált adatok vannak tárolva, és Azure VMs is be, amikor feladatátadás. <br/><br/>Egy [szabványos geo felesleges tárterület-fiók](../storage/storage-redundancy.md#geo-redundant-storage)szükséges. A fiók kell a webhely helyreállítás szolgáltatással ugyanabban a régióban, és társítjuk az azonos előfizetést. Figyelje meg, hogy a replikáció prémium verzió tároló használata jelenleg nem támogatott, és ne használható.<br/><br/>Az áthelyezés az [Új Azure portál](../storage/storage-create-storage-account.md) használata erőforráscsoport létrehozott tárterület-fiókok nem támogatjuk. [Információ](../storage/storage-introduction.md) Azure tárhely.<br/><br/>
**Azure hálózati** | Szüksége van egy Azure virtuális hálózat, amely az Azure VMs fog csatlakozni feladatátadás. A webhely helyreállítási tárolóra azonos régió az Azure virtuális hálózati kell lennie.<br/><br/>Jegyzet, hogy Azure való áttérés után vissza sikertelen lesz szüksége van egy virtuális Magánhálózati kapcsolat (vagy a készült Azure ExpressRoute) beállítása a Azure hálózatról a helyszíni webhelyet.


### <a name="on-premises-prerequisites"></a>A helyszíni vonatkozó követelmények

**Előfeltételek** | **Részletek**
--- | ---
**Management server** | Szüksége van egy helyszíni Windows 2012 R2 kiszolgálón egy virtuális gép vagy a fizikai kiszolgálón futó. A helyszíni webhely helyreállítási összetevőket a kiszolgálóra telepített<br/><br/> Azt javasoljuk, hogy a kiszolgáló, mint egy könnyen hozzáférhető VMware virtuális rendszerbe. A helyszíni webhely csomópontoktól az Azure kifolyólag lehet VMware VMs függetlenül attól, hogy a meg nem sikerült VMs vagy a fizikai kiszolgálón keresztül. Ha egy VMware virtuális nem állítja a Management server kell beállítani egy külön fő tároló kiszolgáló egy VMware virtuális visszaállás forgalmat fogadni.<br/><br/>A kiszolgáló nem tartományvezérlőnek kell lennie.<br/><br/>A kiszolgálón van, hogy a statikus IP-címet.<br/><br/>A kiszolgáló állomásneve kell 15 karakter vagy annál kisebb.<br/><br/>Az operációs rendszer nyelvét csak angol kell.<br/><br/>A management server internet-hozzáférés szükséges.<br/><br/>Van szüksége a kiszolgálóról kimenő access az alábbi képlettel történik: ideiglenes elérésének HTTP 80 webhely helyreállítási összetevőt (letöltési MySQL); a telepítés során Folyamatban lévő kimenő hozzáférést HTTPS 443-as replikációk kezelése; Folyamatban lévő kimenő elérésének HTTPS 9443 replikációs forgalmához (Ez a port módosítható)<br/><br/> Ügyeljen arra, hogy az URL-címet a management server elérhetők: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>A kiszolgálón Ha IP-cím alapú tűzfal szabályokat, ellenőrizze, hogy a szabályok kommunikációt Azure. [Azure adatközpont IP-tartományok](https://www.microsoft.com/download/details.aspx?id=41653) és a HTTPS (443-as) port lehetővé kell. Fehér lista IP-címtartományok az Azure a régió, az előfizetése, és a nyugati USA-beli is kell. Az URL-cím [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") nem töltődnek le a MySQL.
**VMware vCenter/ESXi host**: | Egy vagy több vMware vSphere ESX/ESXi hypervisors kezelése a VMware virtuális gépeken futó szüksége van ESX/ESXi 6.0, 5.5 vagy 5.1 verziója fut a legújabb frissítésekben.<br/><br/> Azt javasoljuk, hogy a ESXi hosts kezelése VMware vCenter kiszolgáló rendszerbe. Meg kell verzióját kell futtatnia vCenter 6.0-s vagy 5.5 a legújabb frissítésekkel együtt.<br/><br/>Ne feledje, hogy webhely helyreállítás nem támogatja az új vCenter vSphere 6.0 szolgáltatások például cross vCenter vMotion, virtuális kötet és tárolását DRS. Webhely helyreállítási terméktámogatási szolgáltatásai, amelyekre is 5.5 verziójában megtalálhatók korlátozódik.
**Védett gépek**: | **AZURE**<br/><br/>Védelemmel ellátni kívánt gépek meg kell felelnie a [Azure Előfeltételek](site-recovery-best-practices.md#azure-virtual-machine-requirements) Azure VMs hozhat létre.<br><br/>Ha szeretne csatlakozni az Azure VMs feladatátvétel után majd kell engedélyezése a távoli asztali kapcsolat, kattintson a helyi tűzfal.<br/><br/>Egyes kapacitásával védett gépeken 1023 GB-nál több kerülni a Webhelyfiókok lehet. A virtuális beállíthatja, hogy legfeljebb 64 lemez (tehát legfeljebb 64 TB). Ha nagyobb, mint 1 TB lemez fontolja meg adatbázis-replikáció: például SQL Server mindig a vagy Oracle-adatok Guard.<br/><br/>Minimális 2 GB szabad a telepítési meghajtó összetevő telepítéshez.<br/><br/>A megosztott lemez Vendég fürt nem támogatott. Ha egy adatbázis-replikáció: például SQL Server mindig a vagy Oracle-adatok Guard fontolja csoportosított telepítés.<br/><br/>Az egyesített bővíthető belső vezérlőprogram felület UEFI () / bővíthető belső vezérlőprogram Interface(EFI) indítási nem támogatott.<br/><br/>Gépi nevek 1 és 63 karakter (betűket, számokat és kötőjelet) közé kell tartalmaznia. A név kell egy betűt vagy számot a végén pedig használja egy betűt vagy számot. Után egy számítógépre védett módosíthatók az Azure nevét.<br/><br/>**VMware VMs**<br/><br>VMware vSphere PowerCLI 6.0 telepíteni kell. Kattintson a management server (konfigurációs kiszolgálója).<br/><br/>Védelemmel ellátni kívánt VMware VMs VMware eszközök telepítése és futtatása kell rendelkeznie.<br/><br/>Ha a forrás virtuális hálózati kártya csoportos együttműködési azt alakul át egy egyetlen hálózati Azure való áttérés után<br/><br/>Ha védett VMs iSCSI-lemez majd webhely helyreállítási alakítja a védett virtuális iSCSI lemez virtuális fájl esetén a virtuális átvétele Azure. Ha iSCSI-tároló érhető el, a Azure virtuális azt fogja iSCSI-tároló csatlakozni, majd lényegében lásd: a két lemez – a virtuális a Azure virtuális a és a forrás iSCSI lemezzel. Ebben az esetben kell választani a iSCSI cél meg a sikertelen Azure virtuális felett megjelenő.<br/><br/>[Tudjon meg többet](#vmware-permissions-for-vcenter-access) is megtudhat a a VMware webhely helyreállítási szükséges felhasználói engedélyekről.<br/><br/> **A WINDOWS SERVER gépek (a VMware virtuális vagy a fizikai server)**<br/><br/>A kiszolgáló a támogatott 64 bites operációs rendszert futtató kell: a Windows Server 2012 R2, Windows Server 2012-ben vagy Windows Server 2008 R2 a legalacsonyabb SP1.<br/><br/>Az operációs rendszer C:\ meghajtón kell telepíteni és az operációs rendszer lemez Windows egyszerű lemezen (OS kerülni a Webhelyfiókok telepíthető Windows dinamikus lemezen.) kell lennie.<br/><br/>A Windows Server 2008 R2 gépekhez szüksége lesz a .NET-keretrendszer 3.5.1-es telepítve van.<br/><br/>Rendszergazdai jogokkal rendelkező fiók kell (helyi gazdai jogosultsággal kell rendelkeznie a Windows-gépen) a leküldéses telepítéséhez a Windows-kiszolgálók mobilitás szolgáltatást. Ha a megadott egy tartományon kívüli fiókot kell a helyi számítógépen távelérési felhasználói vezérlő letiltása. [Tudjon meg többet](#install-the-mobility-service-with-push-installation).<br/><br/>Webhely helyreállítási VMs RDM lemez támogatja.  Visszaállás során webhely helyreállítási felhasználja a RDM lemez Ha az eredeti forrás virtuális és RDM lemezt érhető el. Ezek nem érhetők el, ha során visszaállás webhely helyreállítási hoz létre egy új fájlt VMDK minden lemezre.<br/><br/>**LINUX GÉPEK**<br/><br/>Szüksége van egy 64 bites támogatott operációs rendszer: piros kalap vállalati Linux 6,7; Centos 6.5, 6.6,6.7; Az Oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3), SUSE Linux vállalati kiszolgáló 11 SP3.<br/><br/>védett gépeken/etc/hosts-fájlok tartalmaznia kell a helyi állomásnév megfeleltetése összes hálózati adapteren társított IP-címek bejegyzéseket. <br/><br/>Ha szeretne csatlakozni egy Azure virtuális gépen futó Linux feladatátvétel biztonságos rendszerhéj ügyfélprogram (ssh) után, győződjön meg arról, hogy a biztonságos rendszerhéj szolgáltatás, kattintson a védett számítógép indítási automatikusan a rendszer indítási van beállítva, és tűzfalszabályokat lehetővé teszi, hogy egy ssh kapcsolatot vele.<br/><br/>Védelem csak a következő adathordozós Linux gépekhez engedélyezhető: File system (EXT3, ETX4, ReiserFS, XFS); Többutas szoftver-eszköz hozzárendelést (többutas)); Mennyiségi manager: (LVM2). Fizikai kiszolgálók HP CCISS vezérlő adathordozós nem támogatottak. A ReiserFS formázáshoz csak a SUSE Linux vállalati kiszolgáló 11 SP3 rendszeren támogatott.<br/><br/>Webhely helyreállítási VMs RDM lemez támogatja.  A Linux visszaálláshoz során webhely helyreállítási nem fel újra a RDM lemez. Helyette létrehoz egy új fájlt VMDK minden megfelelő RDM lemezre.

Csak a Linux virtuális - győződjön meg arról, hogy a disk.enableUUID=true beállítás beállítja a virtuális VMware a konfigurációs paraméterei. Ha a sor nem létezik, vegye fel. Ez szükséges ahhoz, hogy egy következetes UUID a VMDK, hogy helyesen csatlakoztatja azt. Ne feledje, hogy ezt a beállítást, nélkül visszaállás okoz a teljes letöltését akkor is, ha a virtuális elérhető prem. Ez a beállítás hozzáadása biztosítja, hogy csak a delta módosításokat átvitt vissza visszaállás során.

## <a name="step-1-create-a-vault"></a>Lépés: 1: Hozzon létre egy tárolóból elemre

1. Jelentkezzen be az [adatkezelési portál](https://manage.windowsazure.com/).
2. Bontsa ki a **Data Services** > **Helyreállítási szolgáltatások** , és kattintson a **Webhely helyreállítási tárolóból elemre**.
3. Kattintson a **létrehozhat új** > **gyors létrehozásához**.
4. A **név**mezőbe írja be egy rövid nevet, amely azonosítja a tárolóból elemre.
5. **Régió**jelölje ki a földrajzi régióban esetében a tárolóból elemre. Jelölje be a támogatott régiók lásd: [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/) adatokat földrajzi elérhetősége
6. Kattintson a **Létrehozás tárolóból elemre**.
    ![Új tárolóból elemre](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Jelölje be az állapotsoron a tárolóra sikeresen létrehozott megerősítéséhez. A tárolóból elemre a fő **Helyreállítási-szolgáltatások** lapon megjelenik **aktív** .

## <a name="step-2-set-up-an-azure-network"></a>Lépés: 2: Az Azure-hálózat beállítása

Az Azure-hálózat beállítása, hogy az Azure VMs fog hálózathoz csatlakozik feladatátvétel után, és, hogy a helyszíni webhely csomópontoktól is a várt módon működik.

1. Az Azure-portálon > **virtuális hálózat létrehozása** , adja meg a hálózat nevét. IP-címhez tartomány- és alhálózat mezőben.
2. Virtuális Magánhálózati/készült ExpressRoute hozzáadása a hálózathoz, ha kell tennie visszaállás kellene. Virtuális Magánhálózati/készült ExpressRoute feladatátvevő követően felvehetők a hálózathoz.

[Szeretné, többet is](../virtual-network/virtual-networks-overview.md) megtudhat a Azure hálózatok.

> [AZURE.NOTE] Webhely helyreállítási pontról hálózatok [hálózatok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott.

## <a name="step-3-install-the-vmware-components"></a>3 lépés: A VMware-összetevők telepítése

Való replikáció virtuális VMware tetszés gépek a következő VMware-összetevők telepítése a kulcskezelő kiszolgálótól:

1. [Töltse le](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) és telepítse VMware vSphere PowerCLI 6.0.
2. Indítsa újra a kiszolgálót.


## <a name="step-4-download-a-vault-registration-key"></a>Lépés: 4: Töltse le a tárolóból elemre regisztrációs kulcs

1. A management server nyissa meg a webhely konzol Azure-ban. A **Helyreállítási-szolgáltatások** lapon kattintson a tárolóból elemre kattintva nyissa meg az első lépések lap. Rövid útmutató az ikonnal bármikor is megnyitható.

    ![Rövid útmutató az első ikonra](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. **Első lépések** lapján kattintson a **Cél erőforrások előkészítése** > **Töltse le a regisztrációs billentyűt**. A regisztrációs fájl automatikusan jön létre. Érvénytelen, így az 5 nap után jön létre.


## <a name="step-5-install-the-management-server"></a>5 lépés: A management server telepítése
> [AZURE.TIP] Ügyeljen arra, hogy az URL-címet a management server elérhetők:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.MySQL.com/Get/archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. Az **Első lépések** lapon töltse le a egységesített telepítőfájlt a kiszolgálóra.

2. Futtassa a telepítőfájlt telepítés webhely helyreállítási egyesített beállítási varázsló indításához.

3.  **Első lépések** válassza **a fiókkonfigurációs kiszolgáló és a folyamat server telepítése**.

    ![Előzetes teendők](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. **Harmadik fél szoftverlicenc** kattintson **elfogadom** töltse le és telepítse a MySQL. 

    ![Harmadik = fél szoftver](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. **Regisztráció** tallózással keresse meg és jelölje ki a regisztrációs kulcsot töltötte le a tárolóból elemre.

    ![Regisztráció](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. **Internetbeállítások** adja meg, hogyan a szolgáltató a konfigurációs kiszolgálón futó Azure webhely helyreállítási az interneten keresztül csatlakozás lesz.

    - Ha azt szeretné, szeretne kapcsolatba lépni a proxy, amely jelenleg beállítása a gépen kattintson a **Csatlakozás a meglévő proxybeállításokhoz**.
    - Ha azt szeretné, hogy a szolgáltató közvetlenül csatlakozni kattintson a **Csatlakozás közvetlenül a proxy nélkül**.
    - Ha a meglévő proxy-hitelesítést igényel, vagy egy egyéni proxykiszolgáló használata a szolgáltató kapcsolatot szeretne, kattintson a **Csatlakozás a egyéni proxybeállításokhoz**.
        - Ha egy egyéni proxy kell adnia a címet, port és hitelesítő adatok
        - Ha meg van már engedélyezni az alábbi URL-címek proxykiszolgálót használ:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Tűzfal](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. **Előfeltételek, jelölje be** a beállítás hatására a program győződjön meg arról, hogy a telepítés futtatását is lehetővé teszi. 

    
    ![Előfeltételek](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Ha egy figyelmeztető üzenet jelenik meg, a **globális idő szinkronizálási jelölje be** a bizonyosodjon meg arról, hogy az idő a rendszeróra**(dátum és** idő beállítása) ugyanaz, mint az időzónát.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. **MySQL** konfigurációban létrehozása a MySQL-kiszolgálói példány telepíthető bejelentkezés hitelesítő adatait.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. **Környezet részletek** kiválasztása e fogjuk való replikáció VMware VMs. Ha, a telepítő ellenőrzi, hogy telepítve van-e a PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. Jelöljön ki **Telepítése hely** , amelyre a bináris telepítése, és tárolhatja a gyorsítótár. Kijelölhet egy meghajtót, amelynek legalább 5 GB rendelkezésre álló tárhely, de azt ajánljuk, hogy a gyorsítótár meghajtót 600 GB szabad lemezterület.

    ![Telepítési hely](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. **Hálózat kiválasztása** adja meg a figyelő (hálózati kártya és SSL-port) amelyen a fiókkonfigurációs kiszolgáló küldhet és fogadhat replikációs adatokat fog. Módosíthatja az alapértelmezett port (9443). Ez a port kívül 443-as port felhasználja replikációs műveletek orchestrates webkiszolgálóra. 443-as replikációs forgalom fogadására kerülni a Webhelyfiókok használható.


    ![Hálózat kiválasztása](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  Az **összefoglaló** tekintse át az információkat, és kattintson a **telepítés**gombra. Telepítés befejezésekor a jelszó jön létre. Ha engedélyezi a replikáció szüksége így másolja a vágólapra, és biztonságos helyen tartani.

    ![Összefoglalás](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  Az **összefoglaló** tekintse át az információkat.

    ![Összefoglalás](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure helyreállítási szolgáltatási ügynökkel a proxy kell lennie a telepítő.
>A telepítés befejezése után indítsa el a "Microsoft Azure helyreállítási szolgáltatások Shell" nevű alkalmazást a Windows Start menüből. A megnyíló felfelé parancssorablakban futtassa a következő számú parancs a proxykiszolgáló beállításainak beállítása.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>A telepítő futtatása a parancssorból

Is futtathatja az egyesített varázsló a parancssorból, az alábbi képlettel történik:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Ha:

- / ServerMode: kötelező. Itt adhatja meg, hogy a telepített telepítse a konfigurációs és a folyamat kiszolgálók vagy a folyamat-kiszolgáló (további folyamat kiszolgálók telepítéséhez használt). A bemeneti értékek: CS, PS
- InstallDrive: kötelező. Adja meg a mappát, ahová az alkatrészek telepíti.
- / MySQLCredFilePath. Kötelező. Adja meg a MySQL-kiszolgáló hitelesítő adatok esetén a szövegegység egy fájl elérési útját. Ismerkedés a fájl létrehozása a sablon.
- / VaultCredFilePath. Kötelező. A tárolóból elemre hitelesítő adatok fájl helye
- / EnvType. Kötelező. Telepítési típusát. Értékek: VMware, NonVMware
- / PSIP és /CSIP. Kötelező. A folyamat kiszolgáló és a fiókkonfigurációs kiszolgáló IP-címe.
- / PassphraseFilePath. Kötelező. A jelszó-fájl helyét.
- / ByPassProxy. Nem kötelező. Adja meg a management server Azure proxy nélkül csatlakozik.
- / ProxySettingsFilePath. Nem kötelező. Adja meg az egyéni proxy (alapértelmezett proxy-hitelesítést igényel kiszolgálói), vagy egyéni proxykiszolgáló beállításainak




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>6 lépés: A vCenter kiszolgáló hitelesítő adatainak beállítása

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

A folyamat kiszolgáló, automatikusan észleljék VMware VMs vCenter kiszolgáló által kezelt. A automatikus felderítése webhely helyreállítási van szüksége a fiókkal és a hitelesítő adatokat a vCenter kiszolgáló elérésére jogosult. Ez a nem megfelelő, ha éppen replikálása a csak a fizikai kiszolgálók.

Ehhez az alábbi képlettel történik:

1. A vCenter kiszolgáló hozzon létre egy szerepkört (**Azure_Site_Recovery**) a vCenter szintjén a [szükséges engedélyekkel](#vmware-permissions-for-vcenter-access).
2. A **Azure_Site_Recovery** szerepkör hozzárendelése egy felhasználóhoz, a vCenter.

    >[AZURE.NOTE] A csak olvasható szerepkör tartalmazó vCenter felhasználói fiók futtatását is lehetővé teszi a feladatátvevő védett forrás gépek való kilépés nélkül. Ha szeretne e gépek leállítása a Azure_Site_Recovery szerepkör lesz szüksége. Ne feledje, hogy ha éppen csak áttérés VMs VMware az Azure, és nem kell visszaállás kattintson a csak olvasható szerepkör elegendő.

3. A fiók hozzáadása nyissa meg a **cspsconfigtool**. Az elérhető, mint az asztali parancsikon és található a [telepítése hely] \home\svsystems\bin mappában.
2. a **Fiókok kezelése** lapon kattintson a **Fiók hozzáadása**gombra.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. A **Számla részletei** adja hozzá a hitelesítő adatokat a vCenter kiszolgáló elérésére használható. Figyelje meg, hogy a portálon jelenjen meg a fiók nevét a 15 percnél hosszabb is eltelhet. Azonnal frissíteni, kattintson a frissítés gombra a **Konfigurációs kiszolgálók** fülre.

    ![Részletek](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>7 lépés: VCenter kiszolgálók és a ESXi hosts hozzáadása

Ha fel kell vennie egy vCenter server (vagy ESXi állomás) VMware VMs esetén replikálása.

1. **Kiszolgálók** > **Konfigurációs kiszolgálóihoz** fülre, majd válasszon a fiókkonfigurációs kiszolgáló > **Hozzáadás vCenter kiszolgáló**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Adja hozzá a vCenter kiszolgáló vagy ESXi host részletek, a megadott elérheti az előző lépésben a vCenter kiszolgáló és a folyamat kiszolgáló felderítésére VMware VMs, amely kezelhetők a vCenter kiszolgáló által használt fiók nevére. Figyelje meg, hogy a vCenter kiszolgáló vagy ESXi host kell elhelyezni a kiszolgáló, amelyen telepítve van a folyamat kiszolgáló azonos hálózaton.

    >[AZURE.NOTE] Ha ad a vCenter kiszolgáló vagy ESXi host-fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal a vCenter vagy host kiszolgálón, majd győződjön meg arról, hogy a vCenter vagy ESXi fiókok jogosultság szükséges alábbi engedélyezve: adatközponthoz adattárhoz, mappa, Jost, hálózati, erőforrás virtuális gép, vSphere elosztott kapcsolót. Ezeken a vCenter kiszolgáló van szüksége a tárhely nézetek jogosultságot.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Feltárás befejeződése után a vCenter kiszolgáló szerepel a **Konfigurációs Servers** fülre.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>8 lépés: A védelem csoport létrehozása

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Védelem csoportok virtuális gépeken futó vagy használata ugyanabban a Dokumentumvédelmi beállításokat védelemmel ellátni kívánt fizikai kiszolgálók logikai csoportok. Adatvédelmi beállítások alkalmazása védelem csoportot, és vesz fel a csoport összes virtuális gépeken futó/tényleges gépek alkalmazva vannak a ezeket a beállításokat.

1. Nyissa meg a **Védett elemek** > **Védelem csoportot** , és kattintson a védelem csoport hozzáadása.

    ![Védelem csoport létrehozása](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. A **Védelem csoportbeállítások megadása** lapon adja meg a csoport nevét, és **a** jelölje ki a fiókkonfigurációs kiszolgáló, amelyen létre szeretne hozni a csoport. **Cél** Azure.

    ![Védelem csoport beállításai](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. A lapon **Adja meg a replikáció** beállításainak a replikáció a csoport összes gépekhez használt.

    ![Védelem csoport replikációs](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Több virtuális konzisztencia**: Ha bekapcsolja a létrehozott megosztott alkalmazás egységes helyreállítási pontok védelem csoportjában a gépek között. Ez a beállítás az összes védelem csoportjában a gép futtatja a azonos terhelését esetén leginkább megfelelő. Minden számítógép ugyanazt az adatpont visszakerül. Ez a lehetőség e VMware VMs vagy a Windows vagy Linux fizikai kiszolgálók replikálása esetén.
    - **Készletben küszöb**: Beállítja a Készletben. Értesítések jön létre, ha a folytonos adatok védelme replikáció meghaladja a beállított Készletben küszöbértéket.
    - **Helyreállítási pont adatmegőrzési**: Adja meg az adatmegőrzési ablakban. Védett gépek bármely pontjára, ezen ablakon belül állíthatók helyre.
    - **Alkalmazás egységes pillanatkép gyakoriság**: Itt adhatja meg, milyen gyakran helyreállítási pontokat tartalmazó alkalmazás egységes pillanatképek jön létre.

A bejelölést kattintva védelem csoport létrejön a megadott nevű. In Addition létrejön egy második védelem csoport a nevű < védelem-csoport-neve-visszaállás). A védelem csoport használja, ha nem sikerül a helyszíni webhelyek Azure való áttérés után. Azok az **Elemek védett** lapon esetén létrehozott figyelheti a védelem csoportokat.

## <a name="step-9-install-the-mobility-service"></a>Lépés a 9: Mobilitás szolgáltatás telepítése

Az első lépés a virtuális gépeken futó és fizikai kiszolgálók védelem bekapcsolása mobilitás szolgáltatás telepítése. Kétféle módon teheti meg:

- Automatikusan leküldéses, és a szolgáltatás telepítése a folyamat kiszolgálóról minden gépen. Megjegyzés: Ha gépek hozzáadása védelem csoporthoz, és már megfelelő verzióját a mobilitás leküldéses telepítése nem történik.
- A vállalati leküldéses mód, például WSUS vagy System Center Configuration Manager szolgáltatás automatikusan telepíti. Győződjön meg arról, hogy be van állítva a management server mielőtt ezt megteheti.
- Telepítse kézzel védelemmel ellátni kívánt minden gépen. e biztos abban, hogy beállította a management server mielőtt ezt megteheti.


### <a name="install-the-mobility-service-with-push-installation"></a>Leküldéses telepítés mobilitás szolgáltatás telepítése

Védelem a csoportnak gépek hozzáadásakor a mobilitás szolgáltatás automatikusan tolni és a folyamat kiszolgáló által minden számítógépre telepítve.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>A Windows gépeken automatikus leküldéses előkészítése

Az alábbi módszerrel, hogy a mobilitás szolgáltatás automatikusan települ a folyamat kiszolgáló előkészítése a Windows gépek.

1.  Hozzon létre egy fiókot, a folyamat kiszolgáló által a számítógép eléréséhez használható. A fiók (helyi vagy tartomány) rendszergazdai jogosultságokkal kell tartalmaznia. Figyelje meg, hogy a hitelesítő adatokat a mobilitás szolgáltatás telepítése leküldéses csak használják.

    >[AZURE.NOTE] Ha nem használ a tartományi fiók kell a helyi számítógépen távelérési felhasználói vezérlő letiltása. Ehhez a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System nyilvántartásban hozzáadáshoz DUPLASZÓ LocalAccountTokenFilterPolicy az 1 értéket tartalmazó csoportban. A beállításjegyzék hozzáadása egy CLI tétel nyissa meg a parancsot, vagy a PowerShell használatával adja meg **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  A Windows tűzfal a gép szeretné védelme, jelölje be **az alkalmazás vagy szolgáltatás átengedése a tűzfalat** , és engedélyezze a **fájl- és nyomtatómegosztás** és a **Windows Management műszerezettségi**. A tartományhoz tartozó gépekhez beállíthatja a tűzfal házirend a csoportházirend-Objektumot.

    ![Tűzfal beállításai](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. A létrehozott fiók hozzáadásához:

    - Nyissa meg a **cspsconfigtool**. Az elérhető, mint az asztali parancsikon és található a [telepítése hely] \home\svsystems\bin mappában.
    - A **Fiókok kezelése** lapon kattintson a **Fiók hozzáadása**gombra.
    - A létrehozott fiók hozzáadásához. A fiók felvétele után kell adnia a hitelesítő adatait, amikor felvesz egy számítógépre védelem csoportot.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Az automatikus leküldéses Linux kiszolgálókon előkészítése

1.  Győződjön meg arról, hogy [a helyszíni Előfeltételek](#on-premises-prerequisites)ismertetett módon támogatja-e a védelemmel ellátni kívánt Linux gépen. Biztosítják a hálózati kapcsolat áll fenn a védelemmel ellátni kívánt számítógép közötti és a folyamat kiszolgálón futó a management server.

2.  Hozzon létre egy fiókot, a folyamat kiszolgáló által a számítógép eléréséhez használható. A fiók kell lennie a forrás Linux kiszolgálón legfelső szintű felhasználó. Figyelje meg, hogy a hitelesítő adatokat a mobilitás szolgáltatás telepítése leküldéses csak használják.

    - Nyissa meg a **cspsconfigtool**. Az elérhető, mint az asztali parancsikon és található a [telepítése hely] \home\svsystems\bin mappában.
    - A **Fiókok kezelése** lapon kattintson a **Fiók hozzáadása**gombra.
    - A létrehozott fiók hozzáadásához. A fiók felvétele után kell adnia a hitelesítő adatait, amikor felvesz egy számítógépre védelem csoportot.

3.  Ellenőrizze, hogy a forrás Linux kiszolgálón/etc/hosts fájl tartalmaz-e a bejegyzéseket, amely az összes hálózati adapteren társított IP-címek feleltesse meg a helyi hostname (állomásnév).
4.  Telepítse a legújabb openssh, openssh-kiszolgáló, openssl csomagok a védelemmel ellátni kívánt gépen.
5.  Győződjön meg róla, SSH engedélyezve és futó port 22.
6.  SFTP alrendszerek és a jelszó-hitelesítés engedélyezése az sshd_config fájl az alábbi képlettel történik:

    - Jelentkezzen be a legfelső szintű.
    - Keresse meg a fájl /etc/ssh/sshd_config a PasswordAuthentication kezdődő sort.
    - Vegye ki a megjegyzésjeleket a sor, és módosítsa az értékét **nem** **Igen**értékre.
    - Keresse meg a vonal, amely **alrendszer** kezdődik, és vegye ki a megjegyzésjeleket a sor.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>A mobilitás szolgáltatás manuális telepítése

A telepítőfájlokat C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository érhetők el.

Forrás operációs rendszer | Mobilitás szolgáltatás telepítőfájl
--- | ---
A Windows Server (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4 6.5 6.6 (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux vállalati kiszolgáló 11 SP3 (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Az Oracle vállalati Linux 6.4 6.5 (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>A Windows server manuális telepítése


1. Töltse le, és futtassa a megfelelő telepítőjét.
2. **Első lépések **a jelölje ki a **mobilitás szolgáltatást**.

    ![Mobilitás szolgáltatás](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. Adja meg az IP-címe a management server és a jelszó a management server összetevők telepítésekor létrehozott **Konfigurációs kiszolgáló részleteket** . A jelszó meghallgathatja futtatásával: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** a kiszolgálóra.

    ![Mobilitás szolgáltatás](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. **Telepítése** helyen hagyja az alapértelmezett helyet, majd kattintson a **következő** telepítés megkezdéséhez.
5. **Telepítési folyamatban** lévő Lync-telepítés, és indítsa újra a számítógépet, ha a rendszer kéri.

A parancssorból is telepítheti:

UnifiedAgent.exe [/ szerepkör < ügynök/MasterTarget >] [/ InstallLocation <Installation Directory>] [/ CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Ha:

- / Szerepkör: kötelező. Itt adhatja meg, hogy a mobilitás szolgáltatás telepítése után.
- / InstallLocation: kötelező. Itt adhatja meg a szolgáltatás telepítési helye.
- / PassphraseFilePath: kötelező. Adja meg a konfiguráció kiszolgáló jelszava.
- / LogFilePath: kötelező. Log beállítási fájlok helyének megadása

#### <a name="uninstall-mobility-service-manually"></a>Mobilitás szolgáltatás eltávolítása manuálisan

Mobilitás szolgáltatás a programmal hozzáadása eltávolítása a Vezérlőpulton vagy a parancssorból el kell távolítani.

A parancssor mobilitás szolgáltatás eltávolítása parancs

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Az IP-címét a management server módosítása

A varázsló futtatása után módosíthatja a management server IP-címét az alábbi képlettel történik:

1. Nyissa meg a fájl hostconfig.exe (az asztali található).
2. A **globális** lapon módosíthatja a management server IP-címét.

    >[AZURE.NOTE] Csak akkor módosítsa a management server IP-címét. A portszámot management server kommunikáció kell lennie a 443-as, és HTTPS használatát engedélyezni kell a balra. A jelszó ne lehessen módosítani.

    ![Adatkezelési kiszolgáló IP-címe](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Manuálisan telepíteni a kiszolgálón Linux:

1. Másolja a vágólapra a megfelelő tar archívumba, a fenti táblázatban a védelemmel ellátni kívánt Linux géphez alapján.
2. Nyissa meg a rendszerhéj programot, és bontsa ki a tömörített tar archívumban helyi elérési út futtatásával:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Passphrase.txt fájl létrehozása a helyi könyvtár, amelybe kicsomagolta az tar archívum tartalmát. Ehhez a másolja a jelszó C:\ProgramData\Microsoft Azure webhely Recovery\private\connection.passphrase a kiszolgálóra, és mentse azt a passphrase.txt futtatásával *`echo <passphrase> >passphrase.txt`* rendszerhéj.
4. A mobilitás szolgáltatás telepítése beírásával *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Adja meg a management server belső IP-címét, és a 443-as port nincs bejelölve.

**A parancssorból is telepítheti**:

1. Másolja a jelszó C:\Program Files (x86) \InMage Systems\private\connection a kiszolgálóra, és a Mentés másként "passphrase.txt" a kiszolgálóra. Futtassa a következő parancsokat. Ebben a példában a management server IP-cím 104.40.75.37 és HTTPS-port kell lennie a 443-as:

A gyártási kiszolgálón telepítése:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

A fő tároló kiszolgáló telepítése:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>10 lépés: A gép védelem bekapcsolása

Védelem bekapcsolása vegyen fel virtuális gépeken futó és fizikai kiszolgálók védelem a csoportnak. Mielőtt elkezdené, vegye figyelembe az alábbiakat, ha éppen védelme a VMware virtuális gépeken futó:

- 15 percenként megjelenése VMware VMs, és 15 percnél hosszabb, hogy megjelenjen a webhely helyreállítási portálon feltárás után is eltelhet.
- Környezet módosításokat a virtuális gépen (például VMware eszközök telepítése) is igénybe vehet frissíteni kell a webhely helyreállítási 15 percnél hosszabb.
- Ellenőrizni észlelt legutóbb VMware VMs az **Utolsó a kapcsolattartó** mező az vCenter server/ESXi állomáshoz a **Konfigurációs kiszolgálók** lapján.
- Ha már létrehozott védelem csoportot, és ezt követően vCenter kiszolgáló vagy ESXi állomás hozzáadása eltarthat 15 percnél hosszabb frissítheti az Azure webhely helyreállítási portál és a virtuális gépeken futó szeretné, hogy a **védelem csoportba gépek hozzáadása** párbeszédpanelen.
- Ha azonnal folytatásához gépek felvétele védelem csoportba ütemezett felfedezése várakozás nélkül szeretné, jelölje ki a konfigurációs kiszolgálója (ne kattintson rá), és kattintson a **frissítés** gombra.

Ezenkívül vegye figyelembe, hogy:

- Azt javasoljuk, hogy a védelem csoportok tervezővel, hogy azok tükrözni a munkaterhelésekből. Ha például hozzáadása az azonos csoportba adott alkalmazást futtató számítógépeken.
- Gépek védelem csoport beállításakor a folyamat kiszolgáló automatikusan verembe küldi, és telepíti a mobilitás szolgáltatást, ha még nincs telepítve. Figyelje meg, hogy szüksége lesz, hogy a leküldéses szerkezet előkészítése az előző lépésben leírt módon.


Védelem csoport gépek felvétele:

1. Kattintson a **védett elemek** > **Védelem csoport** > **gépek** > gépek hozzáadása. A legjobb \As
2. **Jelölje ki** a virtuális gépeken futó Ha VMware virtuális gépeken futó védelmén jelöljön ki egy vCenter kiszolgálót, amely a virtuális gépeken futó (vagy a EXSi fogadó, amelyen futtatja) kezeli, és válassza a gép.

    ![Védelem bekapcsolása](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  **Jelölje ki** a virtuális gépeken futó Ha fizikai kiszolgálók védelmén a **Fizikai gépek felvétele** varázslóban adja meg az IP-cím és a felhasználóbarát név. Ezután jelölje ki a operációs rendszer termékcsaládon kívüli.

    ![Védelem bekapcsolása](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. **Adja meg a cél erőforrások** jelölje ki a tárhely fiókot a replikáció használ, és adja meg, hogy a beállításokat az összes munkaterhelésekből használandó. Figyelje meg, hogy prémium verzió tároló használata jelenleg nem támogatott.

    >[AZURE.NOTE] 1. nem támogatjuk az áthelyezés tároló fiókok létrehozott erőforráscsoport az [Új Azure portál](../storage/storage-create-storage-account.md) használata.                           2.[tároló fiókok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott üzembe helyezése a webhely helyreállítási használt tárterület-fiókokat.

    ![Védelem bekapcsolása](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. A **Fiókok adja meg,** jelölje ki a fiókot, [konfigurálva](#install-the-mobility-service-with-push-installation) a mobilitás szolgáltatás automatikus telepítéshez használni kívánt.

    ![Védelem bekapcsolása](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Kattintson a Befejezés gépek hozzáadása a védelem csoport, és indítsa el az egyes gépi kezdeti replikációs.

    >[AZURE.NOTE] Ha a szolgáltatás automatikusan települ, amelyeken nincs telepítve, ahogy azok a védelem csoport felvették gépeken mobilitás a leküldéses telepítési készült. A szolgáltatás telepítése után a védelem feladat elindul, ezért nem. A hiba után kell manuálisan indítsa újra az egyes gépi, amely még a mobilitás szolgáltatás telepítve van. A újraindítása után a védelem feladat elölről kezdődik, és a kezdeti replikáció.

A **feladatok** lap állapot figyelheti meg.

![Védelem bekapcsolása](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Emellett a védelem állapot **Védett**az elemek figyelhető > <protection group name> > **virtuális gépeken futó**. Miután a kezdeti replikáció befejeződik, és a rendszer szinkronizálja az adatokat, gépi állapotra változik** védett**.

![Védelem bekapcsolása](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>11 lépés: A védett gépi tulajdonságainak beállítása

1. Miután egy számítógépre **védett** állapota feladatátvevő tulajdonságait is beállíthatja. A védelem csoport adatait jelölje be a számítógépet, és nyissa meg a **beállítás** lapon.
2. Webhely helyreállítási automatikusan tulajdonságok javasol az Azure virtuális és észleli, hogy a helyi hálózati beállítások.

    ![Virtuális gép tulajdonságainak beállítása](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. Ezeket a beállításokat módosíthatja:

    -  **Azure virtuális neve**: Ez az Azure-ban a gépre feladatátvétel után biztosítandó neve. A név kell felelnie Azure.

    -  **Azure virtuális méret**: A hálózati adaptereken számát adja meg, ha a cél virtuális gép méretének határozza meg. [További információ:](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) méretű és kártyák. Megjegyzés:

        - Ha virtuális géphez méretének módosítása, majd mentse a beállításokat, a hálózati kártya számát változik, a **beállítás** lapon legközelebb megnyitásakor. A hálózati adaptereken a cél virtuális gépeken futó egy minimális és maximális száma a választott virtuális gép méretének által támogatott hálózati adaptereken hálózati adaptereken virtuális-gépen száma.
            - A forrás számítógép hálózati adaptereken értéke kisebb vagy egyenlő adaptereken engedélyezett a cél gépi méretét a száma, majd a cél van adaptereken ugyanannyi forrásaként.
            - Ha a forrás virtuális gép kártya száma meghaladja a megengedett célalkalmazás méretét, majd a cél maximális használt.
            - Ha például egy adatforrás számítógépre van két hálózati adaptereken és a cél gépi mérete négy támogatja, a cél gép két adaptereken lesz. Ha a forrás számítógépben két adaptert, de a támogatott cél mérete csak akkor támogatja a egy a cél gép csak egy kártya lesz.
        - Ha a virtuális gép minden kártya kell több hálózati adaptereken az azonos Azure hálózathoz csatlakoztatva.
    - **Azure hálózati**: Adjon meg egy Azure hálózat, amely az Azure VMs feladatátvétel után kapcsolódik. Ha Ön nem adja majd az Azure VMs nem kell minden hálózathoz csatlakoztatva. Ezenkívül kell az Azure-hálózat adja meg, ha szeretné visszaállás az Azure a helyszíni webhely. Visszaállás között az Azure és egy helyszíni hálózat virtuális Magánhálózati kapcsolatot igényel.
    - **Azure IP-cím vagy alhálózat**: az összes hálózati adapteren, jelölje be a alhálózat a Azure virtuális kapcsolódni kell. Megjegyzés:
        - Ha a hálózati kártya a forrás számítógép be van állítva egy statikus IP-címet használja majd megadhatja a statikus IP-címet az Azure virtuális. Ha Ön nem adja meg a statikus IP-cím bármely elérhető IP-cím rendel hozzá. Ha a cél IP-cím szerepel, de már használja-e egy másik virtuális Azure-ban feladatátvevő meghiúsul. Ha a hálózati kártya a forrás gép DHCP használatára van beállítva, lesz, hogy DHCP Azure beállítását.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Lépésben 12: Helyreállítási terv létrehozása és futtatása feladatátvevő

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

Egyetlen géphez feladatátvevő futtatása, vagy több virtuális gépeken futó ugyanaz a feladat végrehajtásához, vagy azonos terhelését futtatott átveszi. Egyszerre több számítógépen átadni, felveheti a helyreállítási terv.

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása

1. A **Helyreállítási tervek** lapon kattintson a **Helyreállítási terv hozzáadása** , majd a helyreállítási terv adja. Adja meg a terv adatait, és válassza ki a **Azure** a célként.

    ![Helyreállítási terv konfigurálása](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. **Jelölje ki** a virtuális gépen jelöljön ki egy védelem csoportot, és válassza a gépek a csoport hozzáadása a helyreállítási csomagot.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Testre szabhatja a csomagot, csoportok és a sorozat létrehozása a sorrendben, amelyben a helyreállítási tervben gépek sikertelen fölé. Parancsfájlok és a képernyőn megjelenő utasításokat a kézi műveletekhez is hozzáadhat. Parancsfájlok manuálisan vagy által [Azure automatizálási Runbooks](site-recovery-runbook-automation.md)használatával hozhat létre. [Tudjon meg többet](site-recovery-create-recovery-plans.md) a helyreállítási terv testreszabásáról.

## <a name="run-a-failover"></a>Feladatátvevő futtatása

Feladatátvevő futtatása előtt vegye figyelembe, hogy:


- Győződjön meg arról, hogy a management server működik, és a rendelkezésre álló - egyébként feladatátvevő sikertelen lesz.
- Ha egy nem tervezett feladatátvételre feljegyzést, amely:

    - Ha lehetséges állítsa le a elsődleges gépek feladatátvételhez futtatása előtt. Ez biztosítja, hogy nincs-e a forrás- és a replika gépeken futó operációs rendszert futtató, egy időben. Ha éppen replikálása a VMware VMs majd feladatátvételhez futtatásakor megadhatja, hogy a webhely helyreállítási kell tennie ajánlott annak érdekében, hogy állítsa le a forrás gépek. Attól függően, hogy az elsődleges webhely állapotát Ez lehet vagy nem működik. Ha a webhely helyreállítási beállítás nem kínál fizikai kiszolgálók esetén replikálása.
    - Feladatátvételhez végrehajtásakor az megáll a adatok replikációs az elsődleges gépek, bármilyen adat delta nem vihetők, után feladatátvételhez kezdődik.

- Ha feladatátvétel után a replika Azure virtuális gép csatlakozni szeretne, távoli asztali kapcsolat a forrás gépen előtt engedélyezni a feladatátvételi futtatja, és engedélyezze RDP kapcsolatot a tűzfalon keresztül. Is kell RDP engedélyezése a nyilvános végpont az Azure virtuális gép feladatátvétel után. Kövesse az alábbi [tanácsokat](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) annak érdekében, hogy működik-e RDP feladatátvevő után.

>[AZURE.NOTE] Azure feladatátvevő tehát tenni a legjobb teljesítmény elérése érdekében beszerzéséhez győződjön meg arról, hogy telepítve van az Azure ügynök a védett gépen. Ez a segíti a gyorsabb betöltése, és lehetővé teszi a diagnosztikai problémák esetén. Lehet, hogy Linux ügynök megtalálható [Itt](https://github.com/Azure/WALinuxAgent) - és Windows ügynök megtalálható [Itt](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

Futtassa a próba áttérni hasonlóan a feladatátvétel és helyreállítási folyamatok elszigetelt hálózat, amely nincs hatással a termelési környezetén, és a normál replikációs továbbra is a szokásos módon. Teszt feladatátvevő elindítja a forrásból, és futtathatja a probléma több módon:

- **Nem adja meg az Azure-hálózaton**: Ha a teszt fog egyszerűen ellenőrizze, hogy virtuális gépeken futó indítása megfelelően jelennek meg az Azure hálózat nélkül próba feladatátvevő futtatása. Virtuális gépeken futó feladatátvétel után nem csatlakozik Azure hálózathoz.
- **Adja meg az Azure-hálózaton**: ilyen típusú feladatátvevő ellenőrzi a, hogy a teljes replikációs környezet akár a várt módon származik, és, hogy az Azure virtuális gépeken futó csatlakozik a megadott hálózati.


1. **Helyreállítási tervek** lapon jelölje ki azt a csomagot, és kattintson a **Vizsgálat feladatátvevő**.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. **Erősítse meg tesztelése című** témakörében válassza a **nincs** megadható, hogy ne az Azure-hálózat a próba feladatátvételi, vagy jelölje ki a hálózat, amely a vizsgálat VMs feladatátvétel után is csatlakoznak. A jelölőnégyzet be van jelölve a feladatátvételi indítása gombra.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. A **feladatok** lap feladatátvevő haladásuk figyelemmel kíséréséhez.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. A feladatátvételi befejeződése után is kell láthatja az Azure replika gépi jelennek meg az Azure-portálon > **virtuális gépeken futó**. Ha azt szeretné, a nyissa meg a virtuális végpont 3389 portjához kell Azure virtuális RDP-alapú elindítására.

5. Miután, ha már végzett, amikor feladatátvevő eléri a teljes tesztelés fázis, kattintson a teljes vizsgálati befejezéséhez. Jegyzetek rögzítheti és mentheti a próba feladatátvételi társított észrevételeit.

6. Kattintson **a vizsgálat feladatátvételi befejeződött** automatikusan jelenjenek meg a tesztkörnyezetben. Miután végzett ezzel a vizsgálati feladatátvételi **készültségi** állapotát jelennek meg. Azok az elemek és a vizsgálat feladatátvételkor automatikusan létrehozott VMs törlődnek. Figyelje meg, hogy ha a teszt feladatátvétel két héttel már továbbra is elkészült által hatályba.



### <a name="run-an-unplanned-failover"></a>Feladatátvételhez futtatása

Tervezett feladatátvevő Azure kezdeményezni és lehet végrehajtani, akkor is, ha az elsődleges webhely nem érhető el.


1. A **Helyreállítási tervek** lapon jelölje ki azt a csomagot, és **feladatátvevő**kattintson > **Nem tervezett feladatátvételre**.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Ha Ön éppen replikálása VMware virtuális gépeken futó választhat, próbálja meg és állítsa le a helyszíni VMs. Ez a lehető legjobb és feladatátvevő továbbra is e munkamennyiség tud-e vagy sem. Ha nem a sikeres részletek jelenik meg a **feladatok **lap > **Tervezett feladatátvevő feladatokat**.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Ez a beállítás nem érhető el, ha éppen replikálása a fizikai kiszolgálók. Próbálja ki és leállíthatja azok manuálisan, ha lehetséges, meg kell.

3. **Erősítse meg című** témakörében ellenőrizze a feladatátvevő irányának (az Azure), és jelölje ki a feladatátvételi használni kívánt helyreállítási pontot. Ha engedélyezte a több-virtuális az replikációs tulajdonságai konfigurálásakor visszaállíthatja a legújabb alkalmazásával vagy összeomlik egységes helyreállítási pontra. **Egyéni helyreállítási pont** időben korábbi pontjához helyreállítása is kijelölhet. A jelölőnégyzet be van jelölve a feladatátvételi indítása gombra.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Várja meg a tervezett feladatátvevő feladat elvégzéséhez. Figyelemmel követheti a feladatátvevő állapotát, a **feladatok** lap. Figyelje meg, hogy akkor is, ha hiba lép fel, nem tervezett feladatátvételkor a helyreállítási terv fut, amíg nem fejeződik be. Kell tenni is láthatja a replika Azure virtuális gépeken futó az Azure-portálon jelennek meg gépi.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Csatlakozás Azure virtuális gépeken futó replikált feladatátvétel után

Hogy csatlakoztatni replikált az Azure virtuális gépeken futó után az alábbi feladatátvevő mire lesz szüksége:

1. Távoli asztali kapcsolaton engedélyezni kell a elsődleges gépen.
2. A Windows tűzfal elsődleges gépre RDP engedélyezése.
3. Feladatátvétel után kell RDP hozzáadása az Azure virtuális gép nyilvános végpontot.

[További](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) információt.


## <a name="deploy-additional-process-servers"></a>További folyamat kiszolgálók terjesztése

Ha át kívánja méretezni a üzembe túl 200 forrás gépek vagy a teljes napi tejeskanna ráta meghaladja a kimenő esetén 2 TB-ot, akkor el kell további folyamat kiszolgálók kezelheti a forgalma. Állíthat be egy további folyamat kiszolgáló [További folyamat kiszolgálók](#additional-process-servers) követelményei ellenőrzése, és kövesse az utasításokat itt állíthatja be a folyamat kiszolgáló. A kiszolgáló beállítása után ahhoz, hogy használhassa a forrás gépek is beállíthatja.

### <a name="set-up-an-additional-process-server"></a>További folyamat kiszolgáló beállítása

Úgy állítja be egy további folyamat kiszolgálót az alábbi képlettel történik:

- Futtassa a folyamat csak kiszolgáló felügyeleti kiszolgáló konfigurálása az egyesített varázslót.
- Ha a kezelt adatok replikációs csak az új folyamat server használatával kell ehhez a védett gépek áttelepítése.

### <a name="install-the-process-server"></a>A folyamat server telepítése

1. Az első lépések lap töltse le a egységesített telepítőfájlt a webhely helyreállítási összetevő telepítéshez. A telepítést.
2. **Első lépések** válassza a **Hozzáadás további folyamat kiszolgálók telepítési méretezése**.

    ![Folyamatábra-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. A varázsló a megszokott módon mikor [beállítása](#step-5-install-the-management-server) a első management server.

4. **Konfigurációs kiszolgáló részletek** adja meg a eredeti management server, amelyen telepítve a fiókkonfigurációs kiszolgáló és a jelszó IP-címét. Az eredeti kiszolgálóra futtatása ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** beszerzése a hozzáférési kódot.

    ![Folyamatábra-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Az új folyamat kiszolgáló használatára áttelepítése

1. Nyissa meg a **Konfigurációs kiszolgálóihoz** > **Server** > nevét, az eredeti management Server > **Kiszolgáló részleteket**.

    ![Folyamatábra-kiszolgáló frissítése](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. **Folyamatábra-kiszolgálók** kattintson a lista **Módosítása folyamat kiszolgáló** mellett a kiszolgáló módosítani szeretné.

    ![Folyamatábra-kiszolgáló frissítése](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. A **Változás folyamat Server** > **Cél folyamat kiszolgáló** jelölje be az új management server, és válassza ki a virtuális gépeken futó, amelyet az új folyamat kiszolgáló fogja kezelni. Kattintson a kiszolgálót kapcsolatos tájékozódáshoz információk ikonra. Az átlag terület való replikáció kijelölt virtuális gépeken az új folyamat kiszolgálóra szükséges annak érdekében, hogy a döntéseket betöltése jelenik meg. Kattintson a esetében, amelyek az új kiszolgálóra folyamat indításához.

    ![Folyamatábra-kiszolgáló frissítése](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>VMware vCenter hozzáférési engedélyek

A folyamat kiszolgáló, automatikusan észleljék VMs vCenter kiszolgálón. Automatikus felderítése végre kell egy szerepkört (Azure_Site_Recovery) megadása a vCenter kiszolgáló elérésére webhely-helyreállítás engedélyezése vCenter szintjén. Megjegyzendő, hogy ha csak VMware gépek áttelepítése az Azure, és nem kell az Azure visszaállás kell, meghatározhatja, hogy egy írásvédett szerepkört, amely elegendő. Beállította az engedélyek ismertetett módon [lépés 6: a vCenter kiszolgáló hitelesítő adatok](#step-6-set-up-credentials-for-the-vcenter-server) a szerepkör engedélyei az alábbi táblázatban soroljuk fel.

**Szerepkör** | **Részletek** | **Engedélyek**
--- | --- | ---
Azure_Site_Recovery szerepkör | VMware virtuális feltárás |Ezek a v középre kiszolgáló jogosultságok hozzárendelése:<br/><br/>Adattárhoz -> hozzárendelendő terület, Tallózás adattárhoz, alacsony szinten fájl műveletek., eltávolítás fájl, frissítés virtuális gép fájlok<br/><br/>Hálózati -> hálózati hozzárendelése<br/><br/>Erőforrás -> erőforráskészlet ki van kapcsolva a virtuális gép áttelepítése, virtuális gép bekapcsolva áttelepítése hozzárendelése virtuális gépen<br/><br/>Feladatok létrehozása feladatban frissítés tevékenység -><br/><br/>Virtuális gép -> konfigurálása<br/><br/>Virtuális gép -> beavatkozás a-válasz kérdés, eszköz kapcsolat., állítsa be a CD-re adathordozón, konfigurálása hajlékonylemezen adathordozóra, kikapcsolás Power >, VMware eszközök telepítése<br/><br/>Virtuális gép -> készlet létrehozása, külső.FÜGGV, Unregister -><br/><br/>Virtuális gép -> létesítése -> Engedélyezés virtuális gép letöltési, engedélyezése virtuális gép fájlok feltöltése<br/><br/>Virtuális gép -> pillanatképek -> Eltávolítás pillanatképek
vCenter felhasználói szerepkör | Feltárás VMware virtuális feladatátvevő forrás virtuális leállítás nélkül | Ezek a v középre kiszolgáló jogosultságok hozzárendelése:<br/><br/>Adatközpont objektum –> propagálása gyermek objektumra, szerepkör = csak olvasható <br/><br/>A felhasználó adatközponthoz szinten van-e hozzárendelve, és így összes objektumát hozzáféréssel rendelkezik a adatközponthoz.  Ha azt szeretné, a hozzáférés korlátozására, szerepkör hozzárendelése a az **nem lehet hozzáférni** a **gyermek számára propagálása** objektummal a gyermek objektumok (ESX hosts, datastores, VMs és hálózatok).
vCenter felhasználói szerepkör | Feladatátvétel és visszaállás | Ezek a v középre kiszolgáló jogosultságok hozzárendelése:<br/><br/>Adatközponthoz objektum – gyermek objektumra, propagálása szerepkör = Azure_Site_Recovery<br/><br/>A felhasználó adatközponthoz szinten van-e hozzárendelve, és így összes objektumát hozzáféréssel rendelkezik a adatközponthoz.  Ha azt szeretné, a hozzáférés korlátozására, szerepkör hozzárendelése a a **nincs hozzáférése **a **gyermek objektumhoz propagálása** a a gyermek objektum (ESX hosts, datastores, VMs és hálózatok).  



## <a name="third-party-software-notices-and-information"></a>Harmadik féltől származó szoftver hirdetményeket és információk

Do Not Localize vagy lefordítása

A szoftver- és a Microsoft-termék vagy szolgáltatás futtató belső vezérlőprogram alapul, vagy ezzel a paranccsal beépíti a projektek, az alább felsorolt anyag (együttesen: "külső kód").  A Microsoft a harmadik féltől származó kód nem szerzőnek.  Az eredeti szerzői jogi közleményt és licenc, a melyik Microsoft fogadott e harmadik fél kód oda az alábbi vannak beállítva.

Harmadik fél kód összetevőinek a projektek, az alább felsorolt kapcsolatos szakasz található információkat. Ezek a licencek és információk találhatók csak tájékoztatási célra.  Harmadik fél kód van folyamatban relicensed, Microsoft a Microsoft licencelési szoftverlicenc a Microsoft-termék vagy szolgáltatás szerint.  

Szakasz az adatokat, amely alatt áll rendelkezésre, Microsoft csoportban az eredeti licencfeltételeket harmadik fél kód összetevők kapcsolódik.

A teljes fájlelérési előfordulhat, hogy megtalálható a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft fenntartja összes nem kifejezett jogot benne, hogy felismerhetővé, estoppel vagy más módon.

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a visszaállás](site-recovery-failback-azure-to-vmware-classic.md) ahhoz, hogy a sikertelen fölé Azure-ban operációs rendszert futtató számítógépeken a helyszíni környezetben való.
