<properties
    pageTitle="VMware virtuális gépeken futó és fizikai kiszolgálók az Azure-webhely helyreállításhoz Azure való replikáció az Azure-portálon |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure webhely helyreállítási való replikáció, feladatátvétel és helyreállítási helyszíni VMware virtuális gépeken futó és a Windows vagy Linux fizikai az Azure portálon Azure-kiszolgálók téve terjesztése"
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
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>Párhuzamos VMware virtuális gépeken futó és fizikai gépek Azure az Azure-webhely helyreállításhoz az Azure portál használatával

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmware-to-azure.md)
- [Azure klasszikus](site-recovery-vmware-to-azure-classic.md)
- [Azure Classic (régi)](site-recovery-vmware-to-azure-classic-legacy.md)

Üdvözli a Azure webhely helyreállítási! Ez a cikk használja, ha azt szeretné, hogy a helyszíni VMware virtuális gépeken futó vagy a Windows vagy Linux fizikai kiszolgálók replikáció az Azure-webhely helyreállítás segítségével az Azure-portálon Azure.

> [AZURE.NOTE] Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) az erőforrások létrehozásáról és használatáról: Azure erőforrás Manager (ARM) és a klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása.

Az Azure-portálon webhely helyreállítási számos új szolgáltatást tartalmaz:

- Az Azure biztonsági mentési és Azure webhely helyreállítási szolgáltatások, hogy állíthat be és kezelheti a üzemkészségének és a egyetlen helyről (BCDR) helyreállítás soroljuk egy egyetlen helyreállítási szolgáltatások tárolóból elemre. Az egyesített irányítópult figyelheti és tevékenységek kezelése a helyszíni webhelyek és a Azure nyilvános felhőben.
- A felhasználók a felhőben megoldás szolgáltató (CSP) program kiépítve Azure előfizetések webhely helyreállítási műveletek az Azure-portálon kezelheti.
- Az Azure-portálon webhely helyreállítási hogy bizonyos gépek ARM tároló fiókokhoz. Feladatátvételkor webhely helyreállítási ARM-alapú VMs Azure hoz létre.
- Webhely helyreállítási klasszikus tároló fiókok való replikáció támogatása továbbra is. Feladatátvételkor a webhely helyreállítási klasszikus modellt használja VMs hoz létre.

Miután ez a cikk-bejegyzést olvas esetleges megjegyzéseket, a képernyő alján a Disqus megjegyzések. Kérje meg a technikai kérdések a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>– Áttekintés

Szervezetek van szüksége a BCDR stratégia, amely azt határozza meg, hogyan alkalmazások, feladatok és adatok fut, és a rendelkezésre álló kapcsolatának során tervezett és a tervezett legrövidebb leállás és helyreállítása a normál munka feltétel minél korábban beállítást. A BCDR vonatkozó stratégia kell az üzleti adatok hagyja, biztonságos és helyreállítható, és győződjön meg róla, munkaterhelésekből által folyamatosan elérhető katasztrófa bekövetkezésekor.

Webhely-helyreállítás egy Azure szolgáltatás, amely helyszíni fizikai kiszolgálók és virtuális gépeken futó a felhőbe (Azure) vagy egy másodlagos adatközponthoz való replikáció orchestrating által a BCDR vonatkozó stratégia beleszámít. Kimaradások a fő tartózkodási helyén lévő előfordulásakor, átadni az alkalmazások és a rendelkezésre álló munkaterhelésekből másodlagos helyre. Nem sikerül visszatérhet a fő tartózkodási helyén amikor visszatér a szokásos műveletek. További információ a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

Ebben a cikkben a replikáció kell minden információt a helyszíni VMware VMs és Azure-kiszolgálók fizikai Windows vagy Linux rendszerhez. Építészeti áttekintést tervezési információk és Azure, a helyszíni kiszolgálók, replikációs beállításai és kapacitás tervezés konfigurációs telepítési lépéseket tartalmazza. Miután beállította a infrastruktúra engedélyezheti replikációs gépeken védelme, és ellenőrizze, hogy feladatátvevő működik.

## <a name="business-advantages"></a>Üzleti előnyei

- Webhely helyreállítási üzleti munkaterhelésének és a VMware VMs és fizikai kiszolgálón futó alkalmazások helyszínen védelmet biztosít.
- A helyreállítási Services portál beállítása, kezelése és figyelje a replikáció, feladatátvevő és helyreállítási egyetlen helyről.
- Webhely-helyreállítás, automatikusan észleljék VMware VMs vSphere hosts adott hozzá.
- Egyszerűen futtathatja feladatátadás a helyszíni infrastruktúra Azure és csomópontoktól (visszaállítás) az Azure virtuális VMware kiszolgálók a helyszíni webhely.
- Több-virtuális engedélyezheti és replikációs csoportok létrehozása az, hogy az alkalmazás munkaterhelésekből többszintű egyszerre több gépek replikáció keresztül. Replikációs csoport gépeken futó összeomlik egységes és alkalmazás egységes helyreállítási pontok van, ha elmulasztják fölé. Feladatátvételi gyűjthető több gépek helyreállítási tervek, hogy a közös átveszi többszintű alkalmazásokat.

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

Ezek a forgatókönyv összetevőket:

- **Konfigurációs kiszolgálója**: koordinálja a kommunikációt és az adatok replikációs és helyreállítási folyamat kezeli a helyszíni gép. Ezen a számítógépen telepíteni a fiókkonfigurációs kiszolgáló és a következő további összetevőit egyetlen telepítőfájlra fogja futtatja:
    - **Folyamatábra-kiszolgáló**: replikációs átjáró működik-e. Azt replikációs adatok fogad védett forrás gépek, optimalizálja a gyorsítótár, a tömörítési és titkosítás és küld az Azure tároló. A védett gépek mobilitás szolgáltatás leküldéses telepítését kezeli, és is VMware VMs automatikus felderítését hajt végre. Az alapértelmezett folyamat kiszolgáló telepítve van a konfigurációs kiszolgálón. További önálló folyamat kiszolgálók, ha át kívánja méretezni a üzembe telepítheti.
    - **A mesterlapok tároló kiszolgáló**: replikációs adatkezelési az Azure visszaállás során.

- **Mobilitás szolgáltatás**: az összetevő telepítve van a számítógépen minden (VMware virtuális vagy fizikai kiszolgáló), amelyet szeretne Azure való replikáció. Azt rögzíti a adatot ír a számítógépen, és a folyamat kiszolgáló úgy továbbítja.
- **Azure**: nem kell minden Azure VMs kezelje a replikáció és Azure való áttérés létrehozása.  Azure előfizetését, replikált adatok és az Azure virtuális hálózati tárolására, hogy az Azure VMs csatlakoztatott hálózati feladatátvétel után Azure tároló fiók szükséges. A tárhely és a hálózat kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre.
- **Visszaállás**: Ha készen áll a helyszíni webhely feladatátvevő után vissza az Azure meghiúsító, kell létrehozása az Azure virtuális ideiglenes folyamat kiszolgáló. Visszaállás befejeződése után törölheti. Történő visszaállás is szüksége VPN (vagy a készült Azure ExpressRoute-) kapcsolat a helyszíni webhely és a Azure hálózat, amelyben az Azure VMs találhatók között. Nehéz visszaállás forgalom esetén, előfordulhat, hogy is be kell állítania egy dedikált fő tároló kiszolgáló helyszíni gépi. Világosabb forgalom az alapértelmezett fő tároló kiszolgáló a konfigurációs kiszolgálón futó is használható.


Az ábra bemutatja, hogy ezek az összetevők együttműködését.

![architektúra](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Ábra 1: VMware fizikai az Azure**

## <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

Az alábbiakban amire szüksége lesz az Azure ebben az esetben telepítéséhez.

**Előfeltételek** | **Részletek**
--- | ---
**Azure-fiók**| Akkor, ha a [Microsoft Azure](http://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**Azure tárhely** | Azure tároló replikált adatok vannak tárolva, és Azure VMs létrehozásakor feladatátadás. <br/><br/>Adatok tárolására szüksége lesz egy szabványos vagy magasabb szintű tárterület-fiókkal, ugyanabban a régióban, mint a helyreállítási szolgáltatások tárolóból elemre.<br/><br/>LRS vagy GRS tároló fiók is használhatja. GRS javasoljuk, hogy a adatokat rugalmas egy területi üzemszünetek esetén, vagy ha az elsődleges régió sem állíthatók. [Tudjon meg többet](../storage/storage-redundancy.md).<br/><br/> [Prémium tároló](../storage/storage-premium-storage.md) általában használatos virtuális gépeken futó, amely folyamatosan magas IO teljesítmény és host IO intenzív munkaterhelésekből alacsony késése van szükség.<br/><br/> Ha szeretne replikált adattárolásra prémium fiókot használ, egy szabványos tároló fiókot, amely rögzítheti helyszíni adatok folyamatban lévő módosítása replikációs naplók tárolására is szüksége lesz.<br/><br/> Figyelje meg, hogy a tárterület-fiókok létrehozása az Azure-portálon keresztül erőforrás csoportok nem mozgathatók. Is prémium tároló fiókok lehetőséget a központi India- és Dél-India védelmet jelenleg nem támogatott.<br/><br/> [Információ](../storage/storage-introduction.md) Azure tárhely.
**Azure hálózati** | Szüksége van egy Azure virtuális hálózat, amely az Azure VMs fog csatlakozni feladatátadás. A helyreállítási szolgáltatások tárolóra azonos régió az Azure virtuális hálózati kell lennie.
**Az Azure visszaállás** | Szüksége van egy ideiglenes folyamat server-Azure virtuális állíthatja be. Ez hozhat létre, ha készen áll visszavétele és fail vissza befejezése után törölheti azt.<br/><br/> Meghiúsító vissza kell egy virtuális Magánhálózati kapcsolat (vagy a készült Azure ExpressRoute) az Azure hálózatról a helyszíni webhelyet.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Konfigurációs kiszolgálója / méretezése folyamat vonatkozó követelmények

Meg kell egy helyszíni gépen van, mint a fiókkonfigurációs kiszolgáló beállítása / méretezési folyamat kiszolgáló.

**Előfeltételek** | **Részletek**
--- | ---
**Konfigurációs kiszolgálója**| Egy helyszíni fizikai vagy virtuális gépen futó Windows Server 2012 R2 van szükség. Ezen a számítógépen telepített összes helyszíni webhely helyreállítási összetevőt.<br/><br/>VMware virtuális replikáció javasoljuk, a kiszolgáló, mint egy könnyen hozzáférhető VMware virtuális rendszerbe. Ha éppen replikálása a fizikai gépek a gép fizikai kiszolgáló lehet.<br/><br/> A helyszíni webhely csomópontoktól az Azure is mindig VMware VMs függetlenül attól, hogy nem sikerült VMs vagy a fizikai kiszolgálón keresztül. Ha nem telepíti a fiókkonfigurációs kiszolgáló, mint egy VMware virtuális kell beállítani egy külön fő tároló kiszolgáló egy VMware virtuális visszaállás forgalmat fogadni.<br/><br/>Ha a kiszolgáló egy VMware virtuális, akkor a hálózati adapteren típusa VMXNET3 kell lennie. Ha egy másik hálózati adapteren típusú kell [VMware frissítés](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) telepítése a vSphere 5.5 kiszolgálón.<br/><br/>A kiszolgálón van, hogy a statikus IP-címet.<br/><br/>A kiszolgáló nem tartományvezérlőnek kell lennie.<br/><br/>A kiszolgáló állomásneve kell 15 karakter vagy annál kisebb.<br/><br/>Az operációs rendszer csak angol kell.<br/><br/> VMware vSphere PowerCLI 6.0 telepíteni kell. a konfiguráció kiszolgálón.<br/><br/>A kiszolgáló internetes hozzáférésre van szükség. Kimenő hozzáférés szükség az alábbi képlettel történik:<br/><br/>HTTP 80 ideiglenes elérésének webhely helyreállítási összetevőt (letöltési MySQL) a telepítés során<br/><br/>Folyamatban lévő kimenő hozzáférést HTTPS 443-as replikációk kezelése<br/><br/>Folyamatban lévő kimenő elérésének HTTPS 9443 replikációs forgalmához (Ez a port módosítható)<br/><br/>A kiszolgálón is szükséges az access az alábbi URL-címek, hogy az Azure csatlakozhat: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net<br/><br/>A kiszolgálón Ha IP-cím alapú tűzfal szabályokat, ellenőrizze, hogy a szabályok kommunikációt Azure. [Azure adatközpont IP-tartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS (443-as) protokoll engedélyezése kell.<br/><br/>Engedélyezi az IP-címtartományok az Azure a régió, az előfizetése, és a nyugati US.<br/><br/>Ez az URL a MySQL-letöltés engedélyezése:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>VMware vCenter/vSphere host vonatkozó követelmények

**Előfeltételek** | **Részletek**
--- | ---
**vSphere**| Egy vagy több VMware vSphere hypervisors van szüksége.<br/><br/>Hypervisors futnia kell vSphere verzió 6.0, 5.5 vagy 5.1 a legújabb frissítésekkel együtt.<br/><br/>Azt javasoljuk, hogy a vSphere hosts és vCenter kiszolgálók ugyanabba a hálózatba, és a folyamat kiszolgálónak (Ez lesz a hálózaton, amelyben a fiókkonfigurációs kiszolgáló található kivéve, ha be van állítva egy külön folyamatot kiszolgáló) találhatók.
**vCenter** | Azt javasoljuk, hogy a vSphere hosts kezelése VMware vCenter kiszolgáló rendszerbe. Meg kell verzióját kell futtatnia vCenter 6.0-s vagy 5.5 a legújabb frissítésekkel együtt.<br/><br/>Ne feledje, hogy webhely helyreállítás nem támogatja az új vCenter vSphere 6.0 szolgáltatások például cross vCenter vMotion, virtuális kötet és tárolását DRS. Webhely helyreállítási terméktámogatási szolgáltatásai, amelyekre is 5.5 verziójában megtalálhatók korlátozódik.


## <a name="protected-machine-prerequisites"></a>Védett számítógép vonatkozó követelmények

**Előfeltételek** | **Részletek**
--- | ---
**Helyszíni (VMware VMs)** | Védelemmel ellátni kívánt VMware VMs VMware eszközök telepítése és futtatása kell rendelkeznie.<br/><br/> Védelemmel ellátni kívánt gépek meg kell felelnie a [Azure Előfeltételek](site-recovery-best-practices.md#azure-virtual-machine-requirements) Azure VMs hozhat létre.<br/><br/>Egyes kapacitásával védett gépeken 1023 GB-nál több kerülni a Webhelyfiókok lehet. A virtuális beállíthatja, hogy legfeljebb 64 lemez (tehát legfeljebb 64 TB). <br/><br/>Minimális 2 GB szabad a telepítési meghajtó összetevő telepítéshez.<br/><br/>A titkosított lemez VMs védelme nem támogatott.<br/><br/>A megosztott lemez Vendég fürt nem támogatott.<br/><br/>Ha az **alkalmazás konzisztencia**engedélyezni szeretné, akkor a **port 20004** kattintson a védett virtuális gép helyi tűzfal, meg kell nyitni.<br/><br/>Az egyesített bővíthető belső vezérlőprogram Interface (UEFI) rendelkező / bővíthető belső vezérlőprogram Interface(EFI) indítási nem támogatott.<br/><br/>Gépi nevek 1 és 63 karakter (betűket, számokat és kötőjelet) közé kell tartalmaznia. A név kell egy betűt vagy számot a végén pedig használja egy betűt vagy számot. Miután engedélyezte a gép replikációs módosíthatja az Azure nevét.<br/><br/>Ha a forrás virtuális hálózati kártya csoportos együttműködési azt alakul át egy egyetlen hálózati Azure való áttérés után<br/><br/>Ha védett virtuális gépeken futó iSCSI-lemez majd webhely helyreállítási alakítja a védett virtuális iSCSI lemez virtuális fájl esetén a virtuális átvétele Azure. Ha a iSCSI cél érhető el, a Azure virtuális azt fog csatlakozni, majd lényegében lásd: a két lemez – a virtuális a Azure virtuális meg, és a forrás iSCSI lemezzel. Ebben az esetben kell választani a iSCSI cél, amely akkor jelenik meg a Azure virtuális.
**A Windows gépek (fizikai vagy VMware)** | A gép a támogatott 64 bites operációs rendszert futtató kell: a Windows Server 2012 R2, Windows Server 2012-ben vagy Windows Server 2008 R2 a legalacsonyabb SP1.<br/><br/> Az operációs rendszer kell telepítve van a C:\ meghajtón. Az operációs rendszer lemez Windows egyszerű lemezen kell lennie, és nem dinamikus. Lehet, hogy az adatok lemez dinamikus.<br/><br/>Webhely helyreállítási VMs egy RDM lemez támogatja. Során visszaálláshoz webhely helyreállítási felhasználja a RDM lemez Ha az eredeti forrás virtuális és RDM lemezt érhető el. Ezek nem érhetők el, ha során visszaállás webhely helyreállítási hoz létre egy új fájlt VMDK minden lemezre.
**Linux gépek** (phyical vagy VMware)|  Szüksége van egy 64 bites támogatott operációs rendszer: Red Hat Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6,6.7,7.0,7.1,7.2; Az Oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3), SUSE Linux vállalati kiszolgáló 11 SP3.<br/><br/>védett gépeken/etc/hosts-fájlok tartalmaznia kell a helyi állomásnév megfeleltetése összes hálózati adapteren társított IP-címek bejegyzéseket.<br/><br/>Ha szeretne csatlakozni egy Azure virtuális gépen futó Linux feladatátvétel biztonságos rendszerhéj ügyfélprogram (ssh) után, győződjön meg arról, hogy a biztonságos rendszerhéj szolgáltatás, kattintson a védett számítógép indítási automatikusan a rendszer indítási van beállítva, és tűzfalszabályokat lehetővé teszi, hogy egy ssh kapcsolatot vele.<br/><br/>A host Name mezőbe, csatlakozási pontok, nevek, az eszköz és Linux rendszerben elérési utak és fájlnevek (pl./stb /; / usr) kell angolul csak.<br/><br/>Védelem csak a következő adathordozós Linux gépekhez engedélyezhető: File system (EXT3, ETX4, ReiserFS, XFS); Többutas szoftver-eszköz hozzárendelést (többutas)); Mennyiségi manager: (LVM2). Fizikai kiszolgálók HP CCISS vezérlő adathordozós nem támogatottak. A ReiserFS formázáshoz csak a SUSE Linux vállalati kiszolgáló 11 SP3 rendszeren támogatott.<br/><br/>Webhely helyreállítási VMs egy RDM lemez támogatja.  A Linux visszaálláshoz során webhely helyreállítási nem fel újra a RDM lemez. Helyette létrehoz egy új fájlt VMDK minden megfelelő RDM lemezre.<br/><br/>Győződjön meg arról, hogy a virtuális VMware a konfigurációs paramétereinek beállítja a disk.enableUUID=true beállítást. Ha még nem létezik, hozza létre a bejegyzést. Szükség szerint ahhoz, hogy egy következetes UUID a VMDK, hogy helyesen csatlakoztatja azt. Ez a beállítás hozzáadása is biztosítható, hogy a változtatások csak delta átkerülnek vissza a helyszíni visszaállás, és nem a teljes replikáció során.
**Mobilitás szolgáltatás** |  **A Windows**: megjelenik az automatikusan leküldéses a mobilitás szolgáltatás szeretné, hogy a folyamat kiszolgáló végezhető műveletek a leküldéses példányát, adja meg egy rendszergazdai fiókkal (helyi rendszergazdák a Windows-gépen) kell Windows operációs rendszert futtató VMs.<br/><br/>**Linux**: automatikusan nyomni az mobilitás szolgáltatás hozzon létre egy fiókot, a folyamat kiszolgáló által végezze el a leküldéses példányát használható kell Linux operációs rendszert futtató VMs való.<br/><br/> Alapértelmezés szerint a számítógép összes lemez replikált. [Kizárja való replikáció lemezen](#exclude-disks-from-replication)a mobilitás szolgáltatás telepítenie kell manuálisan a gépen replikációs engedélyezése előtt.<br/>

## <a name="prepare-for-deployment"></a>Felkészülés a telepítéshez

Felkészülés a telepítési kell:

1. [Az Azure-hálózat beállítása](#set-up-an-azure-network) , amelyben Azure VMs kerül, ha azokat is is be feladatátvétel után. Ezeken kívül történő visszaállás kell a virtuális Magánhálózati kapcsolat (vagy a készült Azure ExpressRoute) beállítása a Azure hálózatról a helyszíni webhely.
2. [Azure tároló fiók beállítása](#set-up-an-azure-storage-account) replikált adatokat.
3. [Fiók előkészítése](#prepare-an-account-for-automatic-discovery) az vCenter kiszolgálón vagy vSphere tárolja, hogy a webhely helyreállítási automatikusan felismeri VMware VMs hozzáadott.
4. [Felkészülés a fiókkonfigurációs kiszolgáló](#prepare-the-configuration-server) annak biztosítására, hogy szükséges URL-címek érhetik el és telepítse vSphere PowerCLI 6.0.


### <a name="set-up-an-azure-network"></a>Az Azure-hálózat beállítása

- A hálózat, amelyben a helyreállítási szolgáltatások tárolóra fogja telepítése, mint az azonos Azure területen kell lennie.
- Attól függően, hogy az erőforrás modell használni kívánt az Azure VMs keresztül nem sikerült ugyanúgy tudja beállítani az Azure hálózat [ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) vagy [Klasszikus módjában](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Annak érdekében, hogy a helyszíni VMware webhely vissza az Azure nem szükséges a virtuális Magánhálózati kapcsolat (vagy a készült Azure ExpressRoute kapcsolata) Azure a hálózatról, amelyben a replikált Azure VMs is található, a helyszíni hálózaton, ahol a fiókkonfigurációs kiszolgáló is található.
- [Megtudhatja, hogy](../vpn-gateway/vpn-gateway-site-to-site-create.md) a támogatott telepítésben a VPN-hely közötti kapcsolatok, modellek, és hogy miként [állíthat be egy kapcsolatot](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- Beállíthatja azt is megteheti [a készült Azure ExpressRoute](../expressroute/expressroute-introduction.md). [Tudjon meg többet](../expressroute/expressroute-howto-vnet-portal-classic.md) is megtudhat a egy készült ExpressRoute az Azure-hálózat beállítása.

> [AZURE.NOTE] Webhely helyreállítási pontról hálózatok [hálózatok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott.

### <a name="set-up-an-azure-storage-account"></a>Azure tároló fiók beállítása

- Akkor, ha a normál vagy Azure replikált adatok tárolására prémium Azure tároló fiókkal. A fiók kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre. Attól függően, hogy az erőforrás modell használni kívánt az Azure VMs keresztül nem sikerült a [ARM](../storage/storage-create-storage-account.md) vagy [Klasszikus üzemmódban](../storage/storage-create-storage-account-classic-portal.md)fiók fogja beállítása.
- Ha a prémium fiók van használatban, létre kell hoznia egy újabb szabványos fiókot beállítani, hogy a helyszíni adatok módosítása folyamatban lévő rögzítés replikációs naplók tárolására replikált adatokhoz.  

> [AZURE.NOTE] [Tárterület-fiókok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott üzembe helyezése a webhely helyreállítási használt tárterület-fiókokat.

### <a name="prepare-an-account-for-automatic-discovery"></a>Az automatikus felderítése fiók előkészítése

A webhely helyreállítási folyamat kiszolgálón, automatikusan észleljék VMware VMs vSphere állomáson, vagy a hosts kezelő vCenter kiszolgálón. Automatikus elvégzéséhez a webhely helyreállítási hitelesítő adatokat, amelyek feltárás hozzáférhet az VMware kiszolgálóhoz. Ez a nem megfelelő, ha éppen replikálása a csak a fizikai gépek.

1. Használata egy kijelölt fiókot a automatikus felderítése hozzon létre egy szerepkört (például Azure_Site_Recovery) a [szükséges engedélyekkel](#vmware-account-permissions)rendelkező vCenter szintjén.
2. Hozzon létre egy új felhasználót vSphere host vagy vCenter kiszolgálói, és a szerepkör hozzárendelése a felhasználóhoz. Később fogja lehetővé teszik webhely helyreállítási, hogy ezeket a hitelesítő adatokat, hogy az automatikus felderítése műveleteket hajthat végre.

    >[AZURE.NOTE] Az írásvédett jogosultság vCenter felhasználói fiók futtatását is lehetővé teszi, hogy feladatátvevő, de nem állítható le védett forrás gépek. Ha szeretne e gépek leállítása a [Azure_Site_Recovery](#vmware-account-permissions) szerepkör lesz szüksége. Ha éppen csak áttérés VMs VMware az Azure és visszaállás nem kell a csak olvasható szerepkört is elegendő.

### <a name="prepare-the-configuration-server"></a>A kiszolgáló előkészítése

1.  Győződjön meg arról, hogy a fiókkonfigurációs kiszolgáló használata esetén a számítógép megfelel-e a [Előfeltételek](#configuration-server-prerequisites). Különösen győződjön meg arról, hogy a számítógép csatlakoztatva van-e az internethez, az alábbi beállításokat:

    - Az alábbi URL-való hozzáférés engedélyezése: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net
    - [Http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) MySQL letöltéséhez való hozzáférés engedélyezése.
    - Engedélyezze az [Azure adatközpont IP-tartományokat](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a (443-as) HTTPS protokollt Azure tűzfalon kapcsolatba lépni.

2.  Letöltheti és telepítheti [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) a konfigurációs kiszolgálón. (Jelenleg PowerCLI más verzióiban nem támogatott, többek között az operációs rendszere 6.0-s verziója, R.)


## <a name="create-a-recovery-services-vault"></a>Hozzon létre egy helyreállítási szolgáltatások tárolóból elemre.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Management** > **biztonsági mentése és a webhely helyreállítási (MOBILE)**. Másik lehetőségként kattinthat **Tallózás** > **Helyreállítási szolgáltatások tárolóból elemre** > **Hozzáadás gombra**.

    ![Új tárolóból elemre](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. **Neve** mezőjében adja meg egy rövid nevet, amely azonosítja a tárolóból elemre. Ha egynél több előfizetése van, válasszon közülük.
4. [Új erőforrás csoport létrehozása](../resource-group-template-deploy-portal.md) vagy jelöljön ki egy meglévőt. Adjon meg egy Azure régiót. Gépek fog kell replikált a területre. Látható, hogy az Azure tárterület és a webhely helyreállítási hálózatokon kell ugyanabban a régióban. Jelölje be a támogatott régiók lásd: [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/) adatokat földrajzi elérhetősége
4. Ha azt szeretné, hogy gyorsan elérhesse a tárolóból elemre az irányítópult kattintson a **Rögzítés a irányítópult** , és kattintson a **Létrehozás**gombra.

    ![Új tárolóból elemre](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

Az új tárolóra jelenik meg az **Irányítópult** > **összes erőforrás**, és a fő **helyreállítási szolgáltatások tárolókban** lap.

## <a name="getting-started"></a>Első lépések

Webhely-helyreállítás az első és első lépések felület és a minél gyorsabban fut biztosít. Azt Előfeltételek ellenőrzi, és végigvezeti a előkövetelményei webhely helyreállítási rendszerbe.

Meg kell válassza ki a gépek való replikáció kívánt, és a kívánt helyre való replikáció. A helyszíni kiszolgálók, Azure beállítások, a replikáció házirendek és kapacitás tervezés, infrastruktúra beállítása. Után a infrastruktúra engedélyeznie replikációs VMs és fizikai kiszolgálókhoz. Ezután adott gépekhez feladatátadás futtatása, és több gépek átadni a helyreállítási terv létrehozása.

Válassza a webhely helyreállítási üzembe módját kezdje el az első lépések. Az első lépések áramlás kissé attól függően, hogy a replikáció követelmények változik.


## <a name="step-1-choose-your-protection-goals"></a>Lépés: 1: Válassza ki a védelem célok

Jelölje be a Mit szeretne való replikáció és a kívánt helyre való replikáció.

1. A **tárolókban helyreállítási szolgáltatások** lap jelölje ki a tárolóból elemre, és kattintson a **Beállítások**gombra.
2. A **Beállítások** > **Első lépések** kattintson a **Webhely helyreállítási** > **lépés 1: Felkészülés infrastruktúra** > **védelem cél**.

    ![Válassza a kitűzött célok](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. **Védelem cél** jelölje ki **Az Azure**, és válassza ki a **Igen, a VMware vSphere hipervizor**. Kattintson **az OK**gombra.

    ![Válassza a kitűzött célok](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Lépés: 2: A forrás-környezet beállítása

Állítsa be a kiszolgáló, és regisztrálni a helyreállítási szolgáltatások tárolóból elemre. Ha éppen replikálása a VMware VMs adjon meg automatikus feltárásához felhasználóit VMware-fiókot.

1. Kattintson a **lépés: 1: Felkészülés infrastruktúra** > **forrás**. Az **Előkészítés forrás** Ha nincs telepítve a fiókkonfigurációs kiszolgáló válassza a **+ konfigurációs kiszolgálója** lehetőséget.

    ![Adatforrás beállítása](./media/site-recovery-vmware-to-azure/set-source1.png)

2. A **Kiszolgáló hozzáadása** lap jelölőnégyzet a **Fiókkonfigurációs kiszolgáló** **Írja be a kiszolgáló**jelenik meg.
3. A kiszolgáló beállítása előtt ellenőrizze a [Előfeltételek](#configuration-server-prerequisites). Az adott ellenőrzése, hogy a gép férhet hozzá a szükséges URL-címek.
4.  Töltse le a helyreállítási egyesített webhelybeállítások telepítőfájlt.
5.  Töltse le a tárolóból elemre regisztrációs billentyűt. Ez az egyesített telepítőjének futtatásakor mobiltelefon szükséges. 5 nap után hoz létre, akkor a kulcs nem érvényes.

    ![Adatforrás beállítása](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Azon a számítógépen, mint a fiókkonfigurációs kiszolgáló használ egyesített telepítendő a fiókkonfigurációs kiszolgáló, a folyamat server és a fő tároló kiszolgáló fut.


### <a name="run-site-recovery-unified-setup"></a>Az egyesített helyreállítás futtatása webhely beállítása

1.  Az egyesített telepítési telepítőfájl futtatása.
2.  **Első lépések** válassza **a fiókkonfigurációs kiszolgáló és a folyamat server telepítése**.

    ![Előzetes teendők](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. **Harmadik fél szoftverlicenc** kattintson **elfogadom** töltse le és telepítse a MySQL.

    ![Harmadik = fél szoftver](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. **Regisztráció** tallózással keresse meg és jelölje ki a regisztrációs kulcsot töltötte le a tárolóból elemre.

    ![Regisztráció](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. **Internetbeállítások** adja meg, hogyan a szolgáltató a konfigurációs kiszolgálón futó Azure webhely helyreállítási az interneten keresztül csatlakozás lesz.

    - Ha azt szeretné, szeretne kapcsolatba lépni a proxy, amely jelenleg beállítása a gépen kattintson a **Csatlakozás a meglévő proxybeállításokhoz**.
    - Ha azt szeretné, hogy a szolgáltató közvetlenül csatlakozni kattintson a **Csatlakozás közvetlenül a proxy nélkül**.
    - Ha a meglévő proxy-hitelesítést igényel, vagy egy egyéni proxykiszolgáló használata a szolgáltató kapcsolatot szeretne, kattintson a **Csatlakozás a egyéni proxybeállításokhoz**.
        - Ha egy egyéni proxy kell adnia a címet, port és hitelesítő adatok
        - Ha használ proxykiszolgálót kell már engedélyezte a [Előfeltételek](#configuration-server-prerequisites)ismertetett URL-címét.

    ![Tűzfal](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. **Előfeltételek, jelölje be** a beállítás hatására a program győződjön meg arról, hogy a telepítés futtatását is lehetővé teszi. Ha egy figyelmeztető üzenet jelenik meg, a **globális idő szinkronizálási jelölje be** a bizonyosodjon meg arról, hogy az idő a rendszeróra**(dátum és** idő beállítása) ugyanaz, mint az időzónát.

    ![Előfeltételek](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. **MySQL** konfigurációban létrehozása a MySQL-kiszolgálói példány telepíthető bejelentkezés hitelesítő adatait.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. **Környezet részletek** kiválasztása e fogjuk való replikáció VMware VMs. Ha, a telepítő ellenőrzi, hogy telepítve van-e a PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. Jelöljön ki **Telepítése hely** , amelyre a bináris telepítése, és tárolhatja a gyorsítótár. Kijelölhet egy meghajtót, amelynek legalább 5 GB rendelkezésre álló tárhely, de azt ajánljuk, hogy a gyorsítótár meghajtót 600 GB szabad lemezterület.

    ![Telepítési hely](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. **Hálózat kiválasztása** adja meg a figyelő (hálózati kártya és SSL-port) amelyen a fiókkonfigurációs kiszolgáló küldhet és fogadhat replikációs adatokat fog. Módosíthatja az alapértelmezett port (9443). Ez a port kívül 443-as port felhasználja replikációs műveletek orchestrates webkiszolgálóra. 443-as replikációs forgalom fogadására kerülni a Webhelyfiókok használható.


    ![Hálózat kiválasztása](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  Az **összefoglaló** tekintse át az információkat, és kattintson a **telepítés**gombra. Telepítés befejezésekor a jelszó jön létre. Ha engedélyezi a replikáció szüksége így másolja a vágólapra, és biztonságos helyen tartani.

    ![Összefoglalás](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Regisztráció a kiszolgáló befejezése után jelenik meg a **Beállítások** > **kiszolgálók** fel a tárolóból elemre.



#### <a name="run-setup-from-the-command-line"></a>A telepítő futtatása a parancssorból

Beállíthatja a parancssorból a konfigurációs kiszolgálója:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Paraméterek:

- / ServerMode: kötelező. Itt adhatja meg, hogy a konfigurációs és a folyamat kiszolgálók telepítése után, vagy a folyamat-kiszolgáló. A bemeneti értékek: CS, PS
- InstallLocation: kötelező. A mappát, amelyben az összetevői telepítve vannak.
- / MySQLCredsFilePath. Kötelező. A fájl elérési útja, a MySQL-kiszolgáló adatokat vannak tárolva. A fájl a következő formátumban kell:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Kötelező. A tárolóból elemre hitelesítő adatok fájl helye
- / EnvType. Kötelező. A telepítési típusát. Értékek: VMware, NonVMware
- / PSIP és /CSIP. Kötelező. A folyamat kiszolgáló és a fiókkonfigurációs kiszolgáló IP-címét.
- / PassphraseFilePath. Kötelező. A jelszó-fájl helyét.
- / BypassProxy. Nem kötelező. Itt adhatja meg, hogy a fiókkonfigurációs kiszolgáló csatlakozik Azure proxy nélkül.
- / ProxySettingsFilePath. Nem kötelező. A proxybeállítások (az alapértelmezett proxy hitelesítési vagy igényel egy egyéni proxy). A fájl a következő formátumban kell:
    - [ProxySettings]
    - ProxyAuthentication = "Igen/nem"
    - A proxykiszolgáló IP = "IP-cím >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Nem kötelező. Port száma replikációs adatok használható.
- SkipSpaceCheck. Nem kötelező. Ugorja át a gyorsítótár terület jelölőnégyzetet.
- AcceptThirdpartyEULA. Kötelező. Jelölő harmadik fél EULA elfogadását jelenti.
- ShowThirdpartyEULA. Kötelező. Megjeleníti a külső EULA. Bemenetként kapott összes paramétert a program figyelmen kívül hagyja.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Az automatikus feltáráshoz használt VMware fiók felvétele

 Amikor a telepítő előkészített rendelkeznie kell [VMware fiók létrehozása](#prepare-an-account-for-automatic-discovery) , amelyek a automatikus felderítése webhely helyreállítási is. Ehhez a fiókhoz adja hozzá a következőképpen:

1. Nyissa meg a **CSPSConfigtool.exe**. Az elérhető, mint az asztali parancsikon és található a [telepítése hely] \home\svsystems\bin mappában.
2. Kattintson a **fiókok kezelése** > **fiókot**.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure/credentials1.png)

3. **Számla részletei** az adja meg a fiók automatikus feltáráshoz használt. Figyelje meg, hogy eltarthat 15 perccel a fiók nevét a portálon jelenjen meg a további információ. Azonnal frissítéséhez kattintson a **Konfigurációs kiszolgálóihoz** > kiszolgálónév > **Kiszolgáló frissítése**.

    ![Részletek](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Csatlakozás vSphere hosts és a vCenter kiszolgálók

Ha éppen replikálása a VMware VMs vSphere állomások és vCenter kiszolgálók csatlakozni.

1. Ellenőrizze, hogy a fiókkonfigurációs kiszolgáló hálózati és fér hozzá a vSphere hosts vCenter kiszolgálók.
2. Kattintson az **Előkészítés infrastruktúra** > **forrás**. Az **Előkészítés forrás** jelölje be a kiszolgáló, és válassza a **+ vCenter** vSphere host vagy vCenter kiszolgáló hozzáadása.
3. **Hozzáadás vCenter** adja meg a vSphere host vCenter kiszolgáló vagy rövid nevét, és adja meg a IP-cím vagy a kiszolgáló teljes Tartománynevét. A port hagyja 443-as kivéve, ha a VMware kiszolgálóját úgy vannak beállítva, hogy egy másik port kérések figyelése. Ezután jelölje ki a fiókot, a VMware kiszolgálóhoz való csatlakozáshoz használt. Kattintson az **OK gombra**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Ha a vCenter kiszolgáló vagy vSphere host-fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal a vCenter vagy host kiszolgálón ad, végezze el arról, hogy a fiók ezek engedélyezve van a jogosultságok: adatközponthoz adattárhoz, mappa, Host, hálózati, erőforrás virtuális gép, vSphere elosztott kapcsolót. Ezeken a vCenter kiszolgáló van szüksége a tárhely nézetek jogosultságot.

Webhely helyreállítási és VMs találja a megadott beállításokkal VMware kiszolgálókhoz csatlakozik.

## <a name="step-3-set-up-the-target-environment"></a>3 lépés: A Céllista környezet beállítása

Ellenőrizze, hogy a replikáció és Azure VMs fog csatlakozni feladatátvétel után egy Azure hálózat tároló fiókkal rendelkezik.

1.  Kattintson az **Előkészítés infrastruktúra** > **cél** , és válassza ki a használni kívánt Azure előfizetést.
2.  Adja meg a telepítési modellt feladatátvétel után VMs használni szeretne.
3.  Webhely helyreállítási ellenőrzi, hogy van-e egy vagy több kompatibilis Azure tároló fiókok és a hálózatok.

    ![Cél](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Ha eddig nem hozott létre a egy tárterület-fiókot, és hozzon létre egy újat szeretne ARM használatával válassza a **+ tárterület-fiókot** , hogy szövegközi ehhez.  Egy fióknevet, típusa, előfizetés és helyét adja meg a **tárterület-fiók létrehozása** lap. A fiók kell ugyanabban a régióban, mint a helyreállítási szolgáltatások tárolóból elemre.

    ![Tárhely](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Megjegyzés:

    - Ha a Klasszikus modell használata tárterület-fiók létrehozása az Azure-portálon kell megtennie. [tudj meg többet](../storage/storage-create-storage-account-classic-portal.md)
    - Ha prémium tároló fiók van használatban replikált adatokat kell, hogy szabványos tárhely fiók beállítása az áruház replikációs naplózza, hogy rögzítési folyamatban lévő módosításokat, a helyszíni adatok.

    > [AZURE.NOTE] Védelem prémium tároló fiókokhoz központi indiai és Dél-indiai jelenleg nem támogatott.

4.  Jelölje ki az Azure hálózaton. Ha eddig nem hozott létre a hálózaton, és azt szeretné, hogy ARM használ, kattintson a **+ hálózat** , beágyazott ehhez. Adja meg a **virtuális hálózat létrehozása** lap nevet a hálózatnak, címtartományokat, alhálózat adatait, előfizetést, és helyét. A hálózat a helyreállítási szolgáltatások tárolóra ugyanazon a helyen kell lennie.

    ![Hálózati](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Ha létre szeretne hozni egy hálózati klasszikus modellt használja az Azure-portálon kell megtennie. [Tudjon meg többet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Lépés: 4: Beállítása replikációs beállításai

1. Hozzon létre egy új replikációs házirend kattintva **Előkészítés infrastruktúra** > **Replikációs beállításai** > **+ létrehozása és hozzárendelése**.
2. **Létrehozás és társítása házirend** adja meg a házirend nevére.
3. A **Készletben küszöb**: Adja meg a Készletben korlátot. Ha folytonos replikációs ennél riasztások jön létre.
5. A **helyreállítási pont adatmegőrzési**adja meg, órában mennyi ideig az adatmegőrzési ablak az egyes helyreállítási pont. Védett gépek ablak bármely pontjára állíthatók helyre. Legfeljebb 24 óra adatmegőrzési prémium tárolóhoz replikált gépekhez támogatott.
6. **Alkalmazás egységes pillanatkép gyakoriság**, adja meg, milyen gyakran (percben) tartalmazó alkalmazás egységes pillanatképek helyreállítási pontok létrejön.
7. A replikáció házirend létrehozásakor alapértelmezés szerint a megfelelő házirend automatikusan létrejön visszaállás. A példában ha a replikáció házirend **munkatárs-házirendet** , majd a visszaállási házirend **munkatárs-házirend-visszaállás**lesz. A házirend mindaddig, amíg egy visszaállás kezdeményez nem használható.  
8. Kattintson az **OK gombra** a házirend létrehozása.

    ![Replikációs házirend](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Amikor létrehoz egy új házirendet meg a fiókkonfigurációs kiszolgáló automatikusan ki van társítva. Kattintson az **OK gombra**.

    ![Replikációs házirend](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Lépés az 5: Kapacitás tervezése

Most, hogy van-e a basic infrastruktúra állíthat be, akkor is kapacitás tervezés és azt tudni, hogy szüksége a további erőforrások.

Webhely helyreállítási biztosít a kapacitás Csapattervező segít a forrás környezetben, webhely helyreállítási összetevői, hálózati és tárolására szolgáló jobb oldali erőforrásokat. A Csapattervező módszerekkel becsült VMs, a lemez és a tárhely átlagos száma alapján a gyors módban, vagy részletes módban, amelyben a terhelést szintjén ábrák fogja bemeneti futtathatja. Mielőtt elkezdené kell:

- A replikáció környezetben, többek között a VMs, per VMs lemezt, és egy lemezen tárolási információkat gyűjthet.
- Becsüljük napi módosítása (tejeskanna) a replikált adatokat is. A [tervezési készülék vSphere kapacitás](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) segítségével segítség.

1.  Kattintson a **Letöltés** töltse le az eszközt, majd futtatásával gombra. [A cikkben olvasható](site-recovery-capacity-planner.md) az eszköz olvashatja el.
2.  Ha végzett választó **Igen** **végzett kapacitásának tervezési?**

    ![Kapacitás tervezése](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

Az alábbi táblázat segítséget nyújt a beosztását, ebben az esetben megtervezése pontok számos rögzíti.


**Összetevő** | **Részletek**
--- | --- | ---
**A replikáció** | **Napi maximális sebességének módosítása**– egy védett számítógépre csak egy folyamatábra-kiszolgáló használható, és egyetlen folyamat kiszolgáló képes kezelni a napi módosítása ráta legfeljebb 2 TB-ot. 2 TB így, a maximális napi adatok módosítása ráta, amely egy védett számítógépre támogatott.<br/><br/> **Maximális átviteli**– egy replikált számítógépre tartozhat Azure-ban egy tárterület-fiókjába. Egy szabványos tároló fiók legfeljebb 20 000 kérelmek másodpercenként képes kezelni, és azt javasoljuk, hogy őrizze meg a forrás gépi IOPS keresztben 20 000. A példa esetén készülék 5 lemez és az egyes merevlemez forrás hoz létre 120 IOPS (méret 8K) a forrás lesz az Azure lemez IOPS legfeljebb 500 / belül. A szám kötelező tároló fiókok = a teljes source IOPs/20000.
**Konfigurációs kiszolgálója** | A kiszolgáló látnia kell kezelni a napi módosítása ráta kapacitás védett gépeken futó összes feladatok keresztül, és szüksége van elég sávszélességgel folyamatosan bizonyos adatok Azure tárolóhoz.<br/><br/> A legjobb azt javasoljuk, hogy a fiókkonfigurációs kiszolgáló helyezkednek el a ugyanabba a hálózatba, és a szegmenst a védelemmel ellátni kívánt gép szerint. Egy másik hálózaton található, de a védelemmel ellátni kívánt gépek 3. a hálózati láthatóság rá kell rendelkeznie.<br/><br/> A konfigurációs kiszolgálója méret javaslatok az alábbi táblázatban soroljuk fel.
**Folyamatábra-kiszolgáló** | Az első folyamat kiszolgáló telepítve van a konfigurációs kiszolgálón alapértelmezés szerint. Ha át kívánja méretezni a környezet további folyamat kiszolgálók telepítheti. Megjegyzés:<br/><br/> A folyamat kiszolgáló védett gépek replikációs adatok fogad, és optimalizálja a gyorsítótár, a tömörítési és titkosítás Azure küldése előtt. A folyamat server gép kell tartalmaznia az alábbi feladatok kellő erőforrások.<br/><br/> A folyamat kiszolgáló használja a lemezgyorsítótár alapján. Azt javasoljuk, hogy külön gyorsítótár lemezen 600 GB vagy több kezelje a hálózati szűk vagy üzemszünetek tárolt adatok módosítását.

### <a name="size-recommendations-for-the-configuration-server"></a>A kiszolgáló méret javaslatok

**PROCESSZOR** | **A memória** | **Lemezen gyorsítótárméret** | **Adatok sebességének módosítása** | **Védett gépek**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es) | A 16 GB | 300 GB | 500 GB vagy annál kisebb | 100-nál kevesebb gépek replikáció.
12 vCPUs (2 sockets * 6 magmintákat @ 2,5 GHz-es) | 18 GB | 600 GB | 1 TB 500 GB | Bizonyos 100-150 gépek között.
a 16 vCPUs (2 sockets * 8 magmintákat @ 2,5 GHz-es) | 32 GB | 1 TB | 1 TB és 2 TB | Bizonyos 150-200 gépek között.
Egy másik folyamat server telepítése | | | > 2 TB | További folyamat kiszolgálók üzembe, ha meg van legfeljebb 200 gépek, végre vagy ha módosítja a napi adatai ráta meghaladja a 2 TB-ot.

Ha:

- Minden egyes adatforrás gépen be van állítva 3 lemezt 100 GB.
- 8 Társítások meghajtók 10 k RPM összehasonlítási tárolására azt használt RAID 10 gyorsítótár lemez mértékekhez.

### <a name="size-recommendations-for-the-process-server"></a>A folyamat kiszolgáló méret javaslatok

Ha több mint 200 gépek védelme kell vagy napi módosítása ráta értéke nagyobb, mint 2 TB további folyamat kiszolgálót, a replikáció terhelés kezelésére is hozzáadhat. Ha át kívánja méretezni, hogy végezheti el:

- Konfigurációs kiszolgálók számának növelése Ha például megvédheti felfelé két konfigurációs kiszolgálóval 400 gépek.
- További folyamat kiszolgálók hozzáadása és forgalom (vagy helyett kívül) kezelésére használható a fiókkonfigurációs kiszolgáló.

Az alábbi táblázat ismerteti, amelyben példa:

- A kiszolgáló tartományként folyamat kiszolgáló nem tervez.
- Beállította az további folyamat kiszolgáló.
- Védett virtuális gépeken futó további folyamat a kiszolgáló által konfigurálása.
- Minden védett forrás gépen be van állítva három lemez 100 GB.

**Konfigurációs kiszolgálója** | **További folyamat kiszolgáló**| **Lemezen gyorsítótárméret** | **Adatok sebességének módosítása** | **Védett gépek**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es), a 16 GB memóriát | 4 vCPUs (2 sockets * 2 magmintákat @ 2,5 GHz-es), 8 GB memóriát | 300 GB | 250 GB vagy annál kisebb | Kisebb, vagy 85 gépek replikáció.
8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es), a 16 GB memóriát | 8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es), 12 GB memóriát | 600 GB | 1 TB 250 GB | Bizonyos 85-150 gépek között.
12 vCPUs (2 sockets * 6 magmintákat @ 2,5 GHz-es), 18 GB memóriát | 12 vCPUs (2 sockets * 6 magmintákat @ 2,5 GHz-es) 24 GB memóriát | 1 TB | 1 TB és 2 TB | Bizonyos 150-225 gépek között.


Amelyben a kiszolgálók méretezése módja attól függenek, a beállításait a skálával felfelé, vagy a modell méretezése.  Néhány csúcsteljesítményű konfigurálása és a folyamat kiszolgálók üzembe helyezésével méretezni, vagy kevesebb erőforrásokkal további kiszolgálók üzembe helyezésével méretezése. Példa: Ha módosítani szeretné a 220 gépek védelme, sikerült végezze el az alábbiak egyikét:

- A kiszolgáló 12vCPU, a 18 GB memóriát, a 12vCPU, 24 GB memóriát, egy további folyamat server telepítése és konfigurálása a védett gépek a további folyamat-kiszolgáló használata.
- Azt is megteheti, sikerült két konfigurációs (2 x 8vCPU, 16 GB RAM) és két további folyamat kiszolgálók (x 8vCPU 1) és 4vCPU x 1 135 + 85 (220) gépek kezelése, valamint konfigurálni védett gépek használata csak a további folyamat kiszolgálókhoz.

[Az alábbi útmutatás](#deploy-additional-process-servers) állíthat be egy további folyamat kiszolgáló.

### <a name="network-bandwidth-considerations"></a>Hálózati sávszélesség kapcsolatos szempontok

A kapacitás Csapattervező eszközzel kiszámítása a replikáció (kezdeti replikációs, majd delta) szükséges sávszélességet. A replikáció sávszélesség-használat szabályozhatják néhány lehetőség áll rendelkezésére:

- **Sávszélességet**: Azure történő másolásához VMware forgalom Ugrás az adott folyamat-kiszolgálón keresztül. A folyamat kiszolgálók futtatott gépeken sávszélesség is szabályozása.
- **Sávszélesség befolyásolják**:, befolyásolhatja a sávszélesség-replikáció beállításkulcsainak néhány használatával használja:
    - A **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** beállításkulcsnak a lemezen (kezdeti vagy delta replikáció) adatátvitel használt szálak számát adja meg. Nagyobb érték esetén a replikáció használható hálózati sávszélesség nő.
    - A **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** adatátvitel visszaállás során használt szálak számát adja meg.

#### <a name="throttle-bandwidth"></a>A sávszélesség szabályozása

1. Nyissa meg a Microsoft Azure biztonsági másolat MMC beépülő modul a számítógépen, mint a folyamat kiszolgáló. Alapértelmezés szerint a Microsoft Azure biztonsági másolat használható érhető el az asztal vagy a C:\Program Files\Microsoft Azure helyreállítási szolgáltatások Agent\bin\wabadmin.
2. A beépülő modult a **Tulajdonságainak módosítása**gombra.

    ![A sávszélesség szabályozása](./media/site-recovery-vmware-to-azure/throttle1.png)

3. A **Throttling** lapon jelölje be az **internetes sávszélesség-használat biztonsági műveletekhez szabályozásának engedélyezése**, és a munka korlátozások, és nem munka óra. 512 KB és másodpercenként 102 MB van érvényes tartományok.

    ![A sávszélesség szabályozása](./media/site-recovery-vmware-to-azure/throttle2.png)

A [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag használatával szabályozásának beállítása. Íme egy példa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** azt jelzi, hogy nincs szabályozásának szükséges.


#### <a name="influence-network-bandwidth"></a>Befolyással sávszélességének

1. A beállításjegyzékben nyissa meg azt a **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Befolyásolhatja a sávszélesség-forgalmat replikáló lemezen, módosítsa a értékét a **UploadThreadsPerVM**, vagy ha még nem létezik, hozza létre a kulcsot.
    - Befolyásolhatja a sávszélesség visszaállás forgalom az Azure, módosítania kell a **DownloadThreadsPerVM**értéket.
2. Az alapértelmezett érték a 4. Egy "overprovisioned" hálózat a beállításkulcsok meg kell változtatni, az alapértelmezett értékeket. A maximális érték 32. Lync-forgalmat optimalizálhatja az értéket.

## <a name="step-6-replicate-applications"></a>Lépés a 6: Párhuzamos alkalmazások

Ellenőrizze, hogy a kívánt való replikáció gépek jeleníthetők meg mobilitás szolgáltatás telepítése, és engedélyezheti a replikáció.

### <a name="install-the-mobility-service"></a>A mobilitás szolgáltatás telepítése

Az első lépés a virtuális gépeken futó és fizikai kiszolgálók védelem bekapcsolása mobilitás szolgáltatás telepítése. Ez a probléma több módon teheti:

- **Folyamat server leküldéses**: gépre replikációs engedélyezésekor leküldéses, és a telepítésekor mobilitás szolgáltatás a folyamat kiszolgálóról. Figyelje meg, hogy leküldéses telepítés nem fordulhat elő, ha gépek már verzióját használja-felfelé-todate az összetevőt.
- **Vállalati leküldéses**: automatikusan telepíti a módszerrel a vállalati leküldéses például WSUS vagy System Center Configuration Manager vagy [Azure automatizálási és a kívánt állapot beállítása](./site-recovery-automate-mobility-service-install.md)összetevőt. Állítsa be a kiszolgáló, mielőtt ezt megteheti.
- **Manuális telepítés**: telepítse az összetevőt manuálisan minden gépen való replikáció kívánt. Állítsa be a kiszolgáló, mielőtt ezt megteheti.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>A Windows gépeken automatikus leküldéses előkészítése

Az alábbi módszerrel, hogy a mobilitás szolgáltatás automatikusan települ a folyamat kiszolgáló előkészítése a Windows gépek.

1.  Hozzon létre egy fiókot, a folyamat kiszolgáló által a számítógép eléréséhez használható. A fiók kell rendelkezik rendszergazdai jogosultságokkal (helyi vagy tartomány), és csak a leküldéses telepítés használatos.

    >[AZURE.NOTE] Ha nem használ a tartományi fiók kell a helyi számítógépen távelérési felhasználói vezérlő letiltása. Ehhez a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System nyilvántartásban hozzáadáshoz Duplaszó (DWORD) LocalAccountTokenFilterPolicy érték 1. A bejegyzés hozzáadása egy CLI típusból **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  A Windows tűzfal a gép szeretné megvédeni, jelölje be **az alkalmazás vagy szolgáltatás átengedése a tűzfal**. Engedélyezze a **fájl- és nyomtatómegosztás** és a **Windows Management műszerezettségi**. A tartományhoz tartozó gépekhez beállíthatja a tűzfal beállításai a csoportházirend-Objektumot.

    ![Tűzfal beállításai](./media/site-recovery-vmware-to-azure/mobility1.png)

2. A létrehozott fiók hozzáadásához:

    - Nyissa meg a **cspsconfigtool**. Az elérhető, mint az asztali parancsikon és található a [telepítése hely] \home\svsystems\bin mappában.
    - A **Fiókok kezelése** lapon kattintson a **Fiók hozzáadása**gombra.
    - A létrehozott fiók hozzáadásához. A fiók felvétele után kell adnia a hitelesítő adatait, ha engedélyezi a replikáció géphez.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Az automatikus leküldéses Linux kiszolgálókon előkészítése

1.  Győződjön meg arról, hogy a védelemmel ellátni kívánt Linux gép támogatott [védett számítógép Előfeltételek](#protected-machine-prerequisites)leírt módon. Győződjön meg róla, nincs hálózati a Linux gép és a folyamat kiszolgáló közötti kapcsolatot.

2.  Hozzon létre egy fiókot, a folyamat kiszolgáló által a számítógép eléréséhez használható. A fiók kell lennie a forrás Linux kiszolgálón legfelső szintű felhasználó, és csak a leküldéses telepítés használatos.

    - Nyissa meg a **cspsconfigtool**. Az elérhető, mint az asztali parancsikon és található a [telepítése hely] \home\svsystems\bin mappában.
    - A **Fiókok kezelése** lapon kattintson a **Fiók hozzáadása**gombra.
    - A létrehozott fiók hozzáadásához. A fiók felvétele után kell adnia a hitelesítő adatait, ha engedélyezi a replikáció géphez.

3.  Ellenőrizze, hogy a forrás Linux kiszolgálón/etc/hosts fájl tartalmaz-e a bejegyzéseket, amely az összes hálózati adapteren társított IP-címek feleltesse meg a helyi hostname (állomásnév).
4.  Telepítse a legújabb openssh, openssh-kiszolgáló, openssl csomagok a gépen való replikáció kívánt.
5.  Győződjön meg róla, SSH engedélyezve és futó port 22.
6.  SFTP alrendszerek és a jelszó-hitelesítés engedélyezése az sshd_config fájl az alábbi képlettel történik:

    - Jelentkezzen be a legfelső szintű.
    - Keresse meg a fájl /etc/ssh/sshd_config a **PasswordAuthentication**kezdődő sort.
    - Vegye ki a megjegyzésjeleket a sor, és módosítsa az értékét **nem** **Igen**értékre.
    - Keresse meg a vonal, amely **alrendszer** kezdődik, és vegye ki a megjegyzésjeleket a sor.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>A mobilitás szolgáltatás manuális telepítése

A telepítőfájlokat **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**konfigurációs kiszolgálóján érhetők el.

Forrás operációs rendszer | Mobilitás szolgáltatás telepítőfájl
--- | ---
A Windows Server (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4 6.5 6.6 (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux vállalati kiszolgáló 11 SP3 (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Az Oracle vállalati Linux 6.4 6.5 (csak a 64 bites) | A Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Mobilitás szolgáltatás telepítése Windows Serveren


1. Töltse le, és futtassa a megfelelő telepítőjét.
2. **Első lépések** a jelölje ki a **mobilitás szolgáltatást**.

    ![Mobilitás szolgáltatás](./media/site-recovery-vmware-to-azure/mobility3.png)

3. **Konfigurációs kiszolgáló részletek** adja meg a fiókkonfigurációs kiszolgáló és a jelszó egyesített telepítő futtatásakor létrehozott IP-címét. A jelszó meghallgathatja futtatásával: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – v** a konfigurációs kiszolgálón.

    ![Mobilitás szolgáltatás](./media/site-recovery-vmware-to-azure/mobility6.png)

4. **Telepítése** helyen hagyja az alapértelmezett, és kattintson a **Tovább gombra** a telepítés megkezdéséhez.
5. **Telepítési folyamatban** lévő Lync-telepítés, és indítsa újra a számítógépet, ha a rendszer kéri. A szolgáltatás telepítése után is tovább tarthat körülbelül 15 percet, hogy állapotát a portálon frissítéséhez.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>A Windows server használatával a parancssorban mobilitás szolgáltatás telepítése

1. A telepítő másolja a kiszolgálón, amelyet védeni kíván helyi mappájában (mondjuk C:\Temp). A telepítő a konfigurációs kiszolgálón a **[telepítése hely] \home\svsystems\pushinstallsvc\repository**csoportban találhatók. A Windows operációs rendszerek csomag lesz egy Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe hasonló neve
2. **Nevezze át** a fájlt MobilitySvcInstaller.exe
3. Futtassa a következő parancsot a ki kell olvasni az MSI-telepítőt. </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>Teljes parancssori szintaxisa

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Paraméterek**

- **/Role:** Kötelező. Itt adhatja meg, hogy a mobilitás szolgáltatás telepítése után. A bemeneti értékek ügynök |} MasterTarget
- **/InstallLocation:** Kötelező. Itt adhatja meg a szolgáltatás telepítési helye.
- **/PassphraseFilePath:** Kötelező. A konfiguráció kiszolgáló jelszava.
- **/LogFilePath:** Kötelező. Ha hozható létre a telepítési naplófájlok helye.



#### <a name="uninstall-mobility-service-manually"></a>Mobilitás szolgáltatás eltávolítása manuálisan

Mobilitás szolgáltatás a programmal hozzáadása eltávolítása a Vezérlőpulton vagy a parancssorból el kell távolítani.

A parancssor mobilitás szolgáltatás eltávolítása parancs

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Mobilitás szolgáltatás telepítése használatával parancssori Linux kiszolgálón

1. Másolja a vágólapra a megfelelő tar archívumban, a fenti táblázatban a kívánt való replikáció Linux géphez alapján.
2. Nyissa meg a rendszerhéj programot, és bontsa ki a tömörített tar archívumba helyi elérési út futtatásával:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Passphrase.txt fájl létrehozása a helyi könyvtár, amelybe kicsomagolta az tar archívum tartalmát. A teendő ez másolja a jelszó C:\ProgramData\Microsoft Azure webhely Recovery\private\connection.passphrase a konfigurációs kiszolgálón, és mentse a passphrase.txt futtatásával *`echo <passphrase> >passphrase.txt`* rendszerhéj.
4. Telepítse a mobilitás szolgáltatás futtatásával *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Adja meg a fiókkonfigurációs kiszolgáló belső IP-címét, és a 443-as port nincs bejelölve. A szolgáltatás telepítése után is tovább tarthat körülbelül 15 percet, hogy állapotát a portálon frissítéséhez.

**A parancssorból is telepítheti**:

1. Másolja a jelszó C:\Program Files (x86) \InMage Systems\private\connection a konfigurációs kiszolgálón, és a Mentés másként "passphrase.txt" a konfigurációs kiszolgálón. Futtassa a következő parancsokat. Ebben a példában a konfigurációs kiszolgáló IP-címe 104.40.75.37 és HTTPS-port kell lennie a 443-as:

A gyártási kiszolgálón telepítése:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

A fő tároló kiszolgáló telepítése:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>A replikáció engedélyezése

#### <a name="before-you-start"></a>Előzetes teendők

Ha éppen replikálása a VMware virtuális gépeken futó tartsa szem előtt a következőket:

- 15 percenként megjelenése VMware VMs és 15 percet is igénybe vagy hosszú, hogy a portálon feltárás után jelennek meg. Hasonlóképpen feltárás készíthet legalább 15 percet vesz igénybe Amikor felvesz egy új vCenter kiszolgáló vagy vSphere host.
- Környezet módosításokat a virtuális gépen (például VMware eszközök telepítése) is igénybe vehet akár 15 percnél frissíteni kell a portálon.
- Az észlelt utoljára az **Utolsó a kapcsolattartó** mező VMware VMs ellenőrizni a **Konfigurációs kiszolgálóihoz** lap vCenter server/vSphere állomáson.
- Anélkül, hogy várnia az ütemezett felfedezése replikációs gépekhez hozzáadásához jelölje ki a kiszolgáló (ne kattintson rá), és kattintson a **frissítés** gombra.
- Ha engedélyezi az replikációs, ha a számítógép készen áll a folyamat kiszolgáló automatikusan a telepítést a mobilitás szolgáltatás rajta.

#### <a name="exclude-disks-from-replication"></a>A replikáció lemez kizárása

Ha engedélyezi a replikáció, alapértelmezés szerint az összes lemez gépre replikált. Lemezen kizárhat való replikáció. Példa nem célszerű lemezt ideiglenes vagy adatok rendelkezik adatforrásokat minden alkalommal, amikor egy számítógépre való replikáció vagy alkalmazás újraindítása (például pagefile.sys vagy SQL Server tempdb). Ha el szeretne rejteni lemezt vegye figyelembe, hogy:

- Csak kizárhatja lemezt, már a mobilitás szolgáltatás telepítve van. Kell [manuálisan telepíteni a mobilitás szolgáltatás](#install-the-mobility-service-manually) , mert a mobilitás szolgáltatás csak telepítve van, a leküldéses mechanizmusa használatával, miután replikációs engedélyezve van.
- Csak egyszerű lemez ki kell zárni való replikáció. Operációs rendszer vagy dinamikus lemezen nem zárja ki.
- Replikációs engedélyezése után nem lehet hozzáadni, vagy távolítsa el a lemezt a replikáció. Ha szeretne hozzáadni, vagy zárja ki a lemezen kell tiltsa le a számítógép védelme és majd engedélyezheti azt.
- Ha működtetéséhez az alkalmazáshoz szükséges lemezen kihagyásához Azure való áttérés után kell létrehozni a dokumentumot saját kezűleg Azure-ban, hogy a replikált alkalmazás futtatását is lehetővé teszi. Másik lehetőségként Azure automatizálási sikerült integrálása a helyreállítási terv létrehozása a lemezt a gép során.
- Manuális Azure-ban létrehozott vissza sikertelen lesz. Ha például ha több mint három lemez sikertelen, és hozzon létre két közvetlenül az Azure, mind az öt sikertelen lesz vissza. Lemezen visszaállás manuálisan létrehozott nem zárja ki.

**Most lehetővé teszi a replikáció az alábbi képlettel történik**:

1. Kattintson a **lépés 2: alkalmazás bizonyos** > **forrás**. Miután engedélyezte a replikáció az első alkalommal fogja a **+ bizonyos** a replikáció további gépekhez engedélyezéséhez a tárolóból elemre kattintson.
2. Kattintson a **forrás** lap > **forrás** , jelölje be a kiszolgáló.
3. A **gép típusa** jelölje ki a **virtuális gépeken futó** vagy a **Fizikai gépek**.
4. Jelölje ki a vCenter kiszolgálót, a vSphere host kezelő **vCenter/vSphere hipervizor** , vagy jelölje be az állomásnév oszlopban. Ez a beállítás nem megfelelő, ha éppen replikálása a fizikai gépek.
5. Jelölje ki a folyamat kiszolgálót. Ha még nem hozott létre bármely további folyamat kiszolgálók ez lesz a kiszolgáló nevét. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. A **cél** jelölje ki a tárolóból elemre előfizetést, és jelölje ki a modell (klasszikus vagy az erőforrás-kezelés) feladatátvétel után Azure-ban használni kívánt **bejegyzés átváltó telepítési modell** .
7. Jelölje ki az Azure tároló fiókot kiegészítését adatokat kell megadnia. Megjegyzés:

    - Egy premium vagy szabványos tároló fiók kiválasztása Ha egy adjon meg egy folyamatban lévő replikációs naplók szabványos további tárterület-fiókot kell prémium fiókot választja. Fiókok a helyreállítási szolgáltatások tárolóra azonos régió kell lennie.
    - Ha azt szeretné, mint a másik tárterület-fiókkal rendelkezik, [Hozzon létre egy újat](#set-up-an-azure-storage-account)is. **Új létrehozása**kattintva hozhat létre egy tároló fiók ARM modellt használja. Ha létre szeretne hozni egy tárterület-fiókját a Klasszikus modell el fogja, hogy [az Azure-portálon](../storage/storage-create-storage-account-classic-portal.md).

8. Jelölje be az Azure hálózat, amelyhez Azure VMs fog csatlakozni, ha az azok esetén is be feladatátvétel után alhálózat. A hálózat kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre. Válassza a **Konfigurálás most kijelölt gépekhez** hálózati beállítás alkalmazása az összes gépekhez ki védelmét. Válassza a **Beállítás később** jelölje ki az Azure hálózati per gépi. Ha nincs telepítve a hálózaton kell [Hozzon létre egy újat](#set-up-an-azure-network). **Új létrehozása**kattintva hozhat létre egy hálózati ARM modellt használja. Ha létre szeretne hozni egy hálózati klasszikus modellt használja a fogja, hogy [az Azure-portálon](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)el. Jelölje ki a alhálózat, ha van ilyen. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. **Virtuális**gépeken futó > **Jelölje ki a virtuális gépeken futó** kattintson, és válassza a minden kívánt való replikáció gépet. Jelölje ki a replikáció engedélyezhető gépek csak. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. A **Tulajdonságok** > **tulajdonságainak beállítása**, jelölje ki a fiókot, amely automatikusan a folyamat kiszolgáló által használandó a mobilitás szolgáltatás telepítése a számítógépen. Alapértelmezés szerint az összes lemez replikált. Kattintson az **Összes lemez** , és törölje a bármely lemezt nem kívánt replikáció. Kattintson **az OK**gombra. Beállíthatja, hogy további tulajdonságot később.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. **Replikációs**beállításai > **replikációs beállítások** ellenőrizze, hogy a megfelelő replikációs házirend van-e jelölve. Házirend-beállítások replikációs **beállításai**módosíthatók > **replikációs házirendek** > házirend neve > **Beállítások szerkesztése elemre**. A házirend végzett módosítások fog vonatkozni replikáló és új gépek.

12. **Több-virtuális konzisztencia** engedélyezése, ha gépek gyűjtése a replikáció csoportba, és adjon meg egy nevet a csoportnak szeretné. Kattintson **az OK**gombra. Megjegyzés:

    - A replikáció gépek replikáció egy csoportba, és az Önnel megosztott összeomlik egységes és alkalmazás egységes helyreállítási pontok, ha elmulasztják fölé.
    - Azt javasoljuk, hogy Ön összegyűjti VMs és fizikai kiszolgálók, hogy azok tükrözni a munkaterhelésekből. Engedélyezése többszörös-virtuális konzisztencia befolyásolhatja a terhelést a teljesítményt, és csak akkor használható, ha gépek azonos terhelését fut, és egységesebb van szüksége.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Kattintson a **Replikáció engedélyezése**gombra. A **Védelem bekapcsolása** feladat **beállításai**végrehajtásának nyomon követhető > **feladatok** > **Webhely helyreállítási feladatokat**. A **Védelem véglegesítése** feladat futtatása a gép után van készen áll a feladatátvevő.

> [AZURE.NOTE] Ha a számítógép kész leküldéses telepített példányának a mobilitás összetevő települ, ha engedélyezve van a védelmet. A összetevő után telepítve van a számítógépen, a védelem feladat elindul, és nem. A hiba után kell manuálisan indítsa újra az egyes gépi. A újraindítása után a védelem feladat elölről kezdődik, és a kezdeti replikáció.

### <a name="view-and-manage-vm-properties"></a>Megtekintheti és kezelheti a virtuális tulajdonságai

Azt javasoljuk, hogy ellenőriznie kell a forrás gép tulajdonságait. Ne feledje, hogy az Azure virtuális neve meg kell felelnie, [Azure virtuális gép](site-recovery-best-practices.md#azure-virtual-machine-requirements)követelményeknek.

1. Kattintson a **Beállítások** > **replikált elemek** >, és válassza ki a gépet. Az **alapvető tudnivalók** lap a gép beállításai és az állapot vonatkozó információkat jelenít meg.

2. **Tulajdonságok** megtekintheti a virtuális replikációs és feladatátvevő adatait.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. A **számítási és a hálózati** > ,**Tulajdonságok kiszámítania** megadhatja, hogy az Azure virtuális nevét és a cél méretét. Módosítsa a név Azure követelmények betartása, ha módosítani szeretné.
Is megtekintheti, és megadhatja a célalkalmazás hálózati alhálózat és jogokat kap az Azure virtuális IP-cím adatait. Vegye figyelembe az alábbiakat:

    - Beállíthatja, hogy a cél IP-címet. Ha Ön nem adja meg egy címet, a sikertelen fölé gépi DHCP fogja használni. Ha, amely nem érhető el, feladatátvételkor, a feladatátvételi nem fognak működni. Az azonos target IP-cím próba feladatátvételi használható, ha a cím érhető el a próba feladatátvevő hálózaton.
    - A hálózati adaptereken számát határozza meg a méretét, adja meg a cél virtuális a számítógépen, az alábbi képlettel történik:

        - A forrás számítógép hálózati adaptereken értéke kisebb vagy egyenlő adaptereken engedélyezett a cél gépi méretét a száma, majd a cél van adaptereken ugyanannyi forrásaként.
        - Ha a forrás virtuális gép kártya száma meghaladja a megengedett a tároló méretét, majd a cél maximális használt.
        - Ha például egy adatforrás számítógépre van két hálózati adaptereken és a cél gépi mérete négy támogatja, a cél gép két adaptereken lesz. Ha a forrás számítógépben két adaptert, de a támogatott cél mérete csak akkor támogatja a egy a cél gép csak egy kártya lesz.     
    - Ha a virtuális fogja az összes csatlakoznak ugyanabba a hálózatba több hálózati adaptereken.

    ![A replikáció engedélyezése](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. A **lemez** megtekintheti a virtuális fog kell replikált az operációs rendszer és az adatok lemezt.


## <a name="step-7-test-the-deployment"></a>7 lépés: A telepítés tesztelése

Annak érdekében, hogy a telepítés tesztelése egy virtuális egyetlen számítógépre vagy a helyreállítási terv, amely tartalmazza az egy vagy több virtuális gépeken futó próba feladatátvevő futtathatja.


### <a name="prepare-for-failover"></a>Feladatátvevő előkészítése

- Javasoljuk, hogy a Azure gyártási hálózatról (Ez az alapértelmezett működés Azure-ban egy új hálózati létrehozásakor) hoz létre, amely magában elszigetelt új Azure hálózat tesztelése feladatátvevő futtatásához. [Tudjon meg többet](site-recovery-failover.md#run-a-test-failover) : a próba feladatátadás futtatása.
- A legjobb teljesítmény elérése érdekében jelenik meg átadni Azure, telepítse az Azure ügynök a védett gépen. Gyorsabb betöltése teszi azt, és a hibaelhárítási segítséget nyújt. Telepítse a [Linux](https://github.com/Azure/WALinuxAgent) vagy [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) -ügynök.
- Teljesen tesztelje a üzembe szüksége lesz a replikált gép a várt módon működnek-infrastruktúra alakítható ki. Ha meg szeretné vizsgálni, az Active Directory és a DNS-virtuális gép létrehozása a tartományvezérlőnek a DNS-ben, és az Azure-webhely helyreállítás Azure való replikáció ez. Olvassa el a további a [próba feladatátvevő szempontjait az Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Győződjön meg arról, hogy fut-e a kiszolgáló. Egyéb esetben feladatátvevő sikertelen lesz.
- Ha már lemezt zárni replikációs szükség lehet ezeket a lemezt manuális létrehozása az Azure feladatátvétel után, hogy az alkalmazás a vártnak fog futni.
- Ha a használni kívánt feladatátvételhez próba feladatátvevő helyett vegye figyelembe az alábbiakat:

    - Ha lehetséges állítsa le a elsődleges gépek feladatátvételhez futtatása előtt. Ez biztosítja, hogy nincs-e a forrás- és a replika gépeken futó operációs rendszert futtató, egy időben. Ha éppen replikálása a VMware VMs majd megadhatja, hogy a webhely helyreállítási kell tennie a forrás gépek leállítása a legjobb munkamennyiség. Attól függően, hogy az elsődleges webhely állapotát Ez lehet vagy nem működik. Ha a webhely helyreállítási beállítás nem kínál fizikai kiszolgálók esetén replikálása.
    - Feladatátvételhez futtatásakor az megáll a adatok replikációs az elsődleges gépek, bármilyen adat delta nem vihetők, után feladatátvételhez kezdődik. Ezenkívül ha feladatátvételhez helyreállítási csomag fog futni mindaddig, amíg befejeződik, még akkor is, ha hiba történik.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Felkészülés a csatlakozás Azure VMs feladatátvétel után

Ha szeretne csatlakozni az Azure VMs RDP segítségével feladatátvétel után, győződjön meg arról, hogy tegye a következőket:

**A helyszíni gépen átadása előtt**:

- Az access az interneten keresztül RDP engedélyezése, győződjön meg arról, hogy TCP- és UDP szabályok kerülnek, az a **nyilvános**, és győződjön meg arról, hogy RDP engedélyezett-e a **Windows tűzfal** -> **engedélyezett alkalmazások és szolgáltatások** minden profilhoz.
- A webhely kapcsolaton keresztül hozzáférés engedélyezése a RDP a számítógépen, és győződjön meg arról, hogy RDP engedélyezett-e a **Windows tűzfal** -> **engedélyezett alkalmazások és szolgáltatások** **tartomány** és a **saját** hálózatokhoz.
- Telepítse az [Azure virtuális ügynök](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) a helyszíni gépen.
- [Manuális telepítés a mobilitás szolgáltatás](#install-the-mobility-service-manually) gépeken helyett a szolgáltatás automatikusan leküldéses a folyamat kiszolgálóval. Ennek oka az, a leküldéses telepítés csak akkor történik, után replikációs engedélyezve van a számítógépen.
- Győződjön meg arról, hogy az operációs rendszer SAN házirend értéke OnlineAll. [tudj meg többet]( https://support.microsoft.com/kb/3031135)
- Kapcsolja ki az IPSec szolgáltatás, a feladatátvételi futtatása előtt.

**Kattintson az Azure virtuális feladatátvétel után**:

- Adja hozzá a nyilvános végpont RDP-protokoll (port 3389), és adja meg a bejelentkezési hitelesítő adatait.
- Gondoskodjon arról, hogy nincs talált tartomány házirendeket, hogy megakadályozni a csatlakozást nyilvános címmel virtuális géphez.
- Próbáljon meg csatlakozni. Ha nem tud kapcsolatba ellenőrizze, hogy fut-e a virtuális. További hibaelhárítási tippeket szól ez a [cikk](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


Ha szeretne hozzáférni az Azure virtuális Linux futtatása után feladatátvevő biztonságos rendszerhéj ügyfélprogram (ssh), tegye a következőket:

**A helyszíni gépen átadása előtt**:

- Győződjön meg arról, hogy a biztonságos rendszerhéj szolgáltatást a Azure virtuális rendszer indításakor automatikus indításúként van beállítva.
- Ellenőrizze, hogy tűzfalszabályokat engedélyezése egy SSH kapcsolatot vele.

**Kattintson az Azure virtuális feladatátvétel után**:

- A hálózati biztonsági csoport szabályokat a virtuális fölé, amelyhez csatlakozik az Azure alhálózat sikertelen engedélyeznie kell a SSH porthoz bejövő kapcsolatokkal.
- Egy nyilvános végpontot kell létrehozni, hogy engedélyezze a bejövő kapcsolatokat az SSH port (TCP-port 22 alapértelmezés szerint).
- Ha a virtuális a virtuális Magánhálózati kapcsolat (Express útvonal vagy webhelyre történő VPN) keresztül érhető el, majd az ügyfél használható segítségével közvetlenül csatlakozik a virtuális SSH keresztül.

**Kattintson az Azure Windows vagy Linux virtuális feladatátvétel után**:

Ha a virtuális gép vagy az alhálózathoz, amelyhez a számítógép tartozik társított hálózati biztonsági csoport, győződjön meg arról, hogy a hálózati biztonsági csoport HTTP-/ HTTPS ahhoz kimenő szabály. Győződjön meg arról is, hogy DNS-Rekordokat, hogy melyik virtuális gépet a hálózat az első nem sikerült feletti helyesen van beállítva. Még a feladatátvételi sikerült időtúllépés hibával-"PreFailoverWorkflow tevékenység WaitForScriptExecutionTask időtúllépési". Megérthető, a részletek, a [Figyelés és hibaelhárítási útmutatójának](site-recovery-monitoring-and-troubleshooting.md#recovery)helyreállítási szakasza vonatkoznak.

## <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

1. Egyetlen számítógépen, a **Beállítások**átadni > **Replikált elemeket**, kattintson a virtuális > **+ próba feladatátvevő** ikonra.

    ![Teszt feladatátvevő](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Helyreállítási csomagra, **Beállítások**átadni > **Helyreállítási tervek**, kattintson a jobb gombbal a terv > **Próba feladatátvevő**. Hozhat létre a helyreállítás tervet, [kövesse az alábbi lépéseket](site-recovery-create-recovery-plans.md).

3. **Teszt című** témakörében jelölje ki a Azure hálózat, amely Azure VMs is csatlakoznak feladatátadás után.
4. Kattintson **az OK** gombra a feladatátvételi megkezdéséhez. A virtuális tulajdonságainak megnyitása vagy a **Próba feladatátvevő** feladat nevében tárolóból elemre kattintva haladásának nyomon követhető > **Beállítások** > **feladatok** > **webhely helyreállítási feladatokat**.
5. **Tesztelés készültségi** állapotát a feladatátvételi elérésekor tegye a következőket:

    1. A replika virtuális gép megtekintése az Azure-portálon. Győződjön meg arról, hogy a virtuális gép sikeresen elindul.
    2. Ha access virtuális gépeken futó felfelé beállítása a helyszíni hálózatról a virtuális géphez távoli asztali kapcsolaton kezdeményezhet.
    3. Kattintson a **kész tesztelje** azt befejezéséhez.

        ![Teszt feladatátvevő](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Kattintson a **jegyzetek** rögzítheti és mentheti a próba feladatátvételi társított észrevételeit.
    5. Kattintson **a vizsgálat feladatátvételi befejeződött** automatikusan jelenjenek meg a tesztkörnyezetben. Miután végzett ezzel a vizsgálati feladatátvételi **készültségi** állapotát jelennek meg.
    6.  Ebben a szakaszban azok az elemek és automatikusan létrehozott webhely helyreállítási a próba feladatátvételkor VMs törlődnek. További elemek próba feladatátvételi létrehozott nem törlődnek.

    > [AZURE.NOTE] Ha a teszt feladatátvétel két héttel már továbbra is által hatályba befejeződik.


6. A feladatátvételi befejeződése után is kell láthatja az Azure replika gépi jelennek meg az Azure-portálon > **virtuális gépeken futó**. Győződjön meg róla, hogy a virtuális megfelelő méretű, a megfelelő hálózathoz csatlakozik, és futó.
7. Ha meg [készített kapcsolatok feladatátvétel után](#prepare-to-connect-to-azure-vms-after-failover) látnia kell a Azure virtuális csatlakozni.

## <a name="monitor-your-deployment"></a>A telepítési figyelése

Itt látható, miként figyelheti a beállításokat, az állapot és az állapot a webhely helyreállítási telepítéshez:

1. Kattintson a **Essentials** irányítópult eléréséhez tárolóra nevére. Az irányítópult azt is megteheti webhely helyreállítási feladatok, replikációs állapotát, a helyreállítási terv, kiszolgáló állapotának és eseményeket.  A csempék és az elrendezésekre vonatkozó lehetőségek, amelyek a leghasznosabb, beleértve a más webhely helyreállítási és biztonsági másolat tárolókban állapotának megjelenítése Essentials is testre szabhatja.<br>
![Alapjai](./media/site-recovery-vmware-to-azure/essentials.png)

2. Az **állapot** csempére figyelheti a webhely kiszolgálók (VMM vagy konfigurációja), amely tapasztalt probléma megoldásához, és a webhely helyreállítási keletkezett az elmúlt 24 óra az eseményeket.
3. Kezelése és a Lync-replikáció **Replikált elemek**, **Helyreállítási tervek**, és a **Webhely helyreállítási feladatok** csempék. Feladatok **beállításai**be tud alsóbb -> **feladatok** -> **Webhely helyreállítási feladatokat**.


## <a name="deploy-additional-process-servers"></a>További folyamat kiszolgálók terjesztése

Ha a túl 200 forrás gépek vagy egy teljes napi tejeskanna megtérülési 2 TB-nél több példányban méretezése, meg kell további folyamat kiszolgálók kezelheti a forgalma.

Jelölje be a [folyamat kiszolgálók javaslatok méretét](#size-recommendations-for-the-process-server) , és kövesse ezeket az utasításokat követve állítsa be a folyamat kiszolgáló. A kiszolgáló beállítása után fogja áttelepítés forrás gépek használatához.

### <a name="install-an-additional-process-server"></a>Egy további folyamat server telepítése

1. A **Beállítások** > **webhely helyreállítási kiszolgálók** kattintson a fiókkonfigurációs kiszolgáló > **folyamat kiszolgáló**.

    ![Folyamatábra-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. **Kiszolgálótípus** kattintson a **folyamat server (helyszíni)**elemre.

    ![Folyamatábra-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Töltse le a helyreállítási egyesített webhelybeállítások fájlt, és indítsa el a folyamat server telepítése, és regisztrálni a tárolóból elemre.
4. **Első lépések** válassza a **Hozzáadás további folyamat kiszolgálók telepítési méretezése**.
5. A varázsló a megszokott módon mikor [állítsa be](#step-2-set-up-the-source-environment) a kiszolgáló.

    ![Folyamatábra-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. **Konfigurációs kiszolgáló részletek** adja meg a fiókkonfigurációs kiszolgáló és a jelszó IP-címét. A jelszó futtatása beszerzése ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** a konfigurációs kiszolgálón.

    ![Folyamatábra-kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Az új folyamat kiszolgáló használatára áttelepítése

1. A **Beállítások** > **webhely helyreállítási kiszolgálók** kattintson a fiókkonfigurációs kiszolgáló, és bontsa ki a **folyamat kiszolgálók**.

    ![Folyamatábra-kiszolgáló frissítése](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Kattintson a jobb gombbal a folyamat kiszolgáló jelenleg használatban van, és kattintson a **Váltás**.

    ![Folyamatábra-kiszolgáló frissítése](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. **Jelölje be a cél folyamat kiszolgálóhoz**válassza az új folyamat kiszolgáló szeretne használni, és válassza az új folyamat kiszolgáló kezelő virtuális gépeken futó. Kattintson a kiszolgálót kapcsolatos tájékozódáshoz információk ikonra. Szeretné, hogy a döntéseket betöltése, az átlag terület való replikáció kijelölt virtuális gépeken az új folyamat kiszolgálóra szükséges jelenik meg. Kattintson a esetében, amelyek az új kiszolgálóra folyamat indításához.

## <a name="vmware-account-permissions"></a>VMware fiókjához tartozó engedélyek

A folyamat kiszolgáló, automatikusan észleljék VMs vCenter kiszolgálón. Automatikus felderítése végre kell meghatározása [(Azure_Site_Recovery) szerepkörbe](#prepare-an-account-for-automatic-discovery) lehetővé teszik webhely helyreállítási a VMware kiszolgáló elérésére. Megjegyzendő, hogy ha csak VMware gépek áttelepítése az Azure, és nem kell az Azure visszaállás kell, meghatározhatja, hogy egy írásvédett szerepkört, amely elegendő. A szükséges szerepkör engedélyei az alábbi táblázatban soroljuk fel.

**Szerepkör** | **Részletek** | **Engedélyek**
--- | --- | ---
Azure_Site_Recovery szerepkör | VMware virtuális feltárás |Ezek a v középre kiszolgáló jogosultságok hozzárendelése:<br/><br/>Adattárhoz -> hozzárendelendő terület, Tallózás adattárhoz, alacsony szinten fájl műveletek., eltávolítás fájl, frissítés virtuális gép fájlok<br/><br/>Hálózati -> hálózati hozzárendelése<br/><br/>Erőforrás -> erőforráskészlet ki van kapcsolva a virtuális gép áttelepítése, virtuális gép bekapcsolva áttelepítése hozzárendelése virtuális gépen<br/><br/>Feladatok létrehozása feladatban frissítés tevékenység -><br/><br/>Virtuális gép -> konfigurálása<br/><br/>Virtuális gép -> beavatkozás a-válasz kérdés, eszköz kapcsolat., állítsa be a CD-re adathordozón, konfigurálása hajlékonylemezen adathordozóra, kikapcsolás Power >, VMware eszközök telepítése<br/><br/>Virtuális gép -> készlet létrehozása, külső.FÜGGV, Unregister -><br/><br/>Virtuális gép -> létesítése -> Engedélyezés virtuális gép letöltési, engedélyezése virtuális gép fájlok feltöltése<br/><br/>Virtuális gép -> pillanatképek -> Eltávolítás pillanatképek
vCenter felhasználói szerepkör | Feltárás VMware virtuális feladatátvevő forrás virtuális leállítás nélkül | Ezek a v középre kiszolgáló jogosultságok hozzárendelése:<br/><br/>Adatközpont objektum –> propagálása gyermek objektumra, szerepkör = csak olvasható <br/><br/>A felhasználó adatközponthoz szinten van-e hozzárendelve, és így összes objektumát hozzáféréssel rendelkezik a adatközponthoz.  Ha azt szeretné, a hozzáférés korlátozására, szerepkör hozzárendelése a az **nem lehet hozzáférni** a **gyermek számára propagálása** objektummal a gyermek objektumok (vSphere hosts, datastores, VMs és hálózatok).
vCenter felhasználói szerepkör | Feladatátvétel és visszaállás | Ezek a v középre kiszolgáló jogosultságok hozzárendelése:<br/><br/>Adatközponthoz objektum – gyermek objektumra, propagálása szerepkör = Azure_Site_Recovery<br/><br/>A felhasználó adatközponthoz szinten van-e hozzárendelve, és így összes objektumát hozzáféréssel rendelkezik a adatközponthoz.  Ha azt szeretné, a hozzáférés korlátozására, szerepkör hozzárendelése a a **nincs hozzáférése** a **gyermek objektumhoz propagálása** a a gyermek objektum (vSphere hosts, datastores, VMs és hálózatok).  
## <a name="next-steps"></a>Következő lépések

- [Tudjon meg többet](site-recovery-failover.md) a különböző típusú feladatátvevő.
- [További tudnivalók a visszaállás](site-recovery-failback-azure-to-vmware.md) ahhoz, hogy a sikertelen fölé Azure-ban operációs rendszert futtató számítógépeken a helyszíni környezetben való.

## <a name="third-party-software-notices-and-information"></a>Harmadik féltől származó szoftver hirdetményeket és információk

Do Not Localize vagy lefordítása

A szoftver- és a Microsoft-termék vagy szolgáltatás futtató belső vezérlőprogram alapul, vagy ezzel a paranccsal beépíti a projektek, az alább felsorolt anyag (együttesen: "külső kód").  A Microsoft a harmadik féltől származó kód nem szerzőnek.  Az eredeti szerzői jogi közleményt és licenc, a melyik Microsoft fogadott e harmadik fél kód oda az alábbi vannak beállítva.

Harmadik fél kód összetevőinek a projektek, az alább felsorolt kapcsolatos szakasz található információkat. Ezek a licencek és információk találhatók csak tájékoztatási célra.  Harmadik fél kód van folyamatban relicensed, a Microsoft által a Microsoft-termék vagy szolgáltatás a Microsoft software licencfeltételeket.  

Szakasz az adatokat, amely alatt áll rendelkezésre, Microsoft az eredeti licencelési feltételek harmadik fél kód összetevők kapcsolódik.

A teljes fájlelérési előfordulhat, hogy megtalálható a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft fenntartja összes nem kifejezett jogot benne, hogy felismerhetővé, estoppel vagy más módon.
