<properties
    pageTitle="Webhely helyreállítási telepítéshez előkészítése |} Microsoft Azure"
    description="Ez a cikk ismerteti a felkészítése az Azure webhely helyreállítási való replikáció telepítése."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Azure webhely helyreállítási telepítéshez előkészítése

Ez a cikk minden az Azure webhely helyreállítási szolgáltatás által támogatott a replikáció forgatókönyv telepítésének követelményei magas szintű áttekintéséhez olvasni. Az általános követelmé minden esetben elolvasott hozzákapcsolhatja speciális telepítési tudnivalókról minden telepítéshez.

Miután ez a cikk-bejegyzést olvas, megjegyzések vagy olvashatók, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)kérdésekkel a képernyő alján.

## <a name="overview"></a>– Áttekintés

Szervezetek van szüksége a BCDR stratégia, amely azt határozza meg, hogyan alkalmazások, feladatok és adatok fut, és a rendelkezésre álló kapcsolatának során tervezett és a tervezett legrövidebb leállás és helyreállítása a normál munka feltétel minél korábban beállítást. A BCDR vonatkozó stratégia kell az üzleti adatok hagyja, biztonságos és helyreállítható, és győződjön meg róla, munkaterhelésekből által folyamatosan elérhető katasztrófa bekövetkezésekor. 

Webhely-helyreállítás egy Azure szolgáltatás, amely helyszíni fizikai kiszolgálók és virtuális gépeken futó a felhőbe (Azure) vagy egy másodlagos adatközponthoz való replikáció orchestrating által a BCDR vonatkozó stratégia beleszámít. Kimaradások a fő tartózkodási helyén lévő előfordulásakor, átadni az alkalmazások és a rendelkezésre álló munkaterhelésekből másodlagos helyre. Nem sikerül visszatérhet a fő tartózkodási helyén amikor visszatér a szokásos műveletek. További információ a [Mi az webhely helyreállítási?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Válassza a telepítési modellje

Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) létrehozásáról és használatáról az erőforrások: az erőforrás-kezelő Azure modell és a klasszikus szolgáltatások kezelése modell. Azure szintén két portálokról – az [Azure klasszikus portál](https://manage.windowsazure.com/) , amely támogatja a klasszikus telepítési modell, és az [Azure portál](https://ms.portal.azure.com/) mindkét telepítési az adatmodellek támogatása.

Webhely-helyreállítás az a klasszikus portál és az Azure portál is elérhető. Az Azure klasszikus portálon a klasszikus szolgáltatások kezelése modellel támogatja a webhely helyreállítási. Az Azure-portálon támogatja a Klasszikus modell vagy erőforrás-kezelő telepítések. [További információ:](site-recovery-overview.md#site-recovery-in-the-azure-portal) telepítéséről az Azure portálján.

Amikor, esetén válassza a telepítési modell megjegyzés, amely:

- Azt javasoljuk, hogy használja az [Azure-portál](https://portal.azure.com) és az erőforrás-kezelő modell lehetőség szerint.
- Webhely helyreállítási tartalmaz egy egyszerűbb, illetve további intuitív bevezetés élmény az Azure-portálon.
- Az Azure portál használata esetén, hogy bizonyos gépek klasszikus és az erőforrás-kezelő tároló Azure-ban. Ezeken kívül az Azure-portálon is használhatja LRS vagy GRS tároló fiókok.
- Az Azure-portálra, hogy állítsa be, és BCDR szolgáltatások kezelése egyetlen helyről egyesíti a egyetlen helyreállítási szolgáltatások tárolóból elemre a webhely helyreállítási és biztonsági másolat szolgáltatásokat.
- A felhasználók a felhőben megoldás szolgáltató (CSP) program kiépítve Azure előfizetések webhely helyreállítási műveletek az Azure-portálon kezelheti.
- Esetében, amelyek VMware VMs vagy a fizikai gépek Azure az Azure-portálon számos új funkcióiról, például a prémium tároló támogatása, és lehetővé teszi az adott lemez nélkül való replikáció.


## <a name="select-your-deployment-scenario"></a>Jelölje ki a telepítéshez

**Telepítési** | **Részletek** | **Azure portál** | **Klasszikus portál** | **A PowerShell**
---|---|---|---|---
**Azure VMware VMs** | Bizonyos VMware VMs Azure-tárolóhoz | Az Azure-ban VMs portálon is átadni klasszikus vagy az erőforrás-kezelő tárhely<br/><br/> Az [Azure portal](site-recovery-vmware-to-azure.md) telepítési biztosít a egyesíti telepítési környezet és a szolgáltatás előnyeit számos. | Az Azure klasszikus portálon és a [bővített funkciójú modell](site-recovery-vmware-to-azure-classic.md) telepítheti és klasszikus Azure tároló átadni.<br/><br/> Új telepítési kerülni a Webhelyfiókok használt régebbi modell is van. |  A PowerShell használata jelenleg nem támogatott.
**Azure a Hyper-V VMs** | A Hyper-V VMs Azure tárolóhoz. System Center VMM felhőket vagy nélkül VMM felügyelt a Hyper-V hosts VMs lehet. | Az Azure-ban portál VMs klasszikus vagy az erőforrás-kezelő tároló végződhet fölé.<br/><br/> Az Azure portal egyesíti telepítési felületet nyújt. [Tudjon meg többet](site-recovery-vmm-to-azure.md) is megtudhat a VMM felhőket a Hyper-V VMs replikálása. [Tudjon meg többet](site-recovery-hyper-v-site-to-azure.md) is megtudhat a replikálása a Hyper-V VMs (nélkül VMM).| A klasszikus Azure portálon is nem sikerül VMs fölé klasszikus Azure-tárolóhoz | A PowerShell telepítésének támogatott.
**Fizikai Azure-kiszolgálók** | Fizikai Windows vagy Linux kiszolgálók bizonyos Azure-tárolóhoz | Az Azure-ban portál kiszolgálók klasszikus vagy az erőforrás-kezelő tároló végződhet fölé.<br/><br/> Az [Azure portal](site-recovery-vmware-to-azure.md) telepítési eszköz az egyszerűsített telepítési és funkció előnyeinek számú biztosít. | Az Azure klasszikus portálon és a [bővített funkciójú modell](site-recovery-vmware-to-azure-classic.md) telepítheti és klasszikus Azure tároló átadni.<br/><br/> Új telepítési kerülni a Webhelyfiókok használt régebbi modell is van. | A PowerShell telepítésének jelenleg nem támogatott.
**Másodlagos webhely kiszolgálók VMware VMs/tényleges* | Bizonyos VMware VMs vagy a fizikai Windows vagy Linux kiszolgálók másodlagos webhelyre. [Tudjon meg többet és a letöltési](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) InMage Scout felhasználói útmutatóban. | Nem érhető el az Azure-portálon | A klasszikus portálon InMage Scout fogja letöltése a webhely helyreállítási tárolóból elemre. | A PowerShell telepítésének jelenleg nem támogatott
**A Hyper-V VMs másodlagos webhelyre** | A Hyper-V VMs VMM felhőket a replikáció egy másodlagos felhőbe | Az [Azure portál](site-recovery-vmm-to-vmm.md) , hogy bizonyos VMM felhőket a Hyper-V VMs egy másodlagos webhelyre a Hyper-V replika csak. SAN jelenleg nem támogatott. | Az Azure klasszikus portálon, hogy bizonyos VMM felhőket a Hyper-V VMs a [Hyper-V kópia](site-recovery-vmm-to-vmm-classic.md) vagy [SAN replikációs](site-recovery-vmm-san.md) másodlagos webhelyre | A PowerShell telepítésének támogatott



## <a name="check-what-you-need-for-deployment"></a>Jelölje be a telepítéshez szükséges

### <a name="replicate-to-azure"></a>Azure való replikáció

**Követelmények** | **Részletek** 
---|---
**Azure-fiók** | Akkor, ha a [Microsoft Azure](http://azure.microsoft.com/) -fiókjába.<br/><br/> Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**Azure tárhely** | Azure tárolója tárolja replikált adatokat, és Azure VMs létrehozásakor feladatátadás. Azure való replikáció, szüksége lesz az [Azure tárterület-fiók](../storage/storage-introduction.md).<br/><br/>Egy vagy több [szabványos GRS tárterület-fiókok](..storage/storage-redundancy.md#geo-redundant-storage)esetén üzembe helyezése a webhely helyreállítási a klasszikus portálon lesz szüksége.<br/><br/> Ha az Azure-portálon használja telepítését GRS vagy LRS tárterület is használhatja.<br/><br/> Ha replikálása, VMware VMs vagy a fizikai kiszolgálók az Azure portál prémium tárolója esetén támogatott. Figyelje meg, hogy egy prémium tárterület-fiók használata is szüksége lesz egy szabványos tároló fiókot, amely rögzítheti helyszíni adatok folyamatban lévő módosítása replikációs naplók tárolásához. [Prémium tároló](../storage/storage-premium-storage.md) általában használatos virtuális gépeken futó, amely folyamatosan magas IO teljesítmény és host IO intenzív munkaterhelésekből alacsony késése van szükség.<br/><br/> Ha szeretne replikált adattárolásra prémium fiókot használ, egy szabványos tároló fiókot, amely rögzítheti helyszíni adatok folyamatban lévő módosítása replikációs naplók tárolására is szüksége lesz.
**Azure hálózati** | Azure való replikáció, szüksége lesz egy Azure hálózat, amely az Azure VMs fog csatlakozni létrehozásakor azok is feladatátvétel után.<br/><br/> Ha éppen üzembe helyezése a klasszikus portálon klasszikus hálózathoz használni. Ha a telepítését, az Azure-portálon használja, a hagyományos, vagy az erőforrás-kezelő hálózati is használhatja.<br/><br/> A hálózat kell lennie az ugyanazon régió, a tárolóból elemre.
**Hálózati hozzárendelése (az Azure VMM)** | Hogy esetében, amelyek a VMM Azure, ha a [hálózati hozzárendelést](site-recovery-network-mapping.md) biztosítja, hogy Azure VMs megfelelő hálózatokhoz kapcsolódó feladatátvétel után.<br/><br/> Hálózati hozzárendelést beállítása kell virtuális hálózatok konfigurálása a VMM portálon.
**A helyszíni** | **VMware VMs**: szüksége van egy helyi számítógépen futó webhely helyreállítási összetevők, a VMware vSphere hosts/vCenter server és a kívánt való replikáció VMs. [További információ:](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Fizikai kiszolgálók**: Ha éppen replikálása a fizikai kiszolgálók szüksége lesz egy helyszíni gépek webhely helyreállítási összetevők és a kívánt való replikáció fizikai kiszolgálók. [További információ:](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Ha [visszavétele](site-recovery-failback-azure-to-vmware.md) Azure való áttérés után szüksége lesz egy VMware infrastruktúra megtennie.<br/><br/> **A Hyper-V VMs**: Ha azt szeretné, való replikáció VMM felhőket a Hyper-V VMs szüksége van egy VMM kiszolgáló, és mely VMs a védelemmel ellátni kívánt a Hyper-V állomások találhatók. [További információ:](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Ha bizonyos a Hyper-V VMs VMM nélkül szeretne szüksége lesz a Hyper-V hosts, amelyen VMs találhatók. [További információ:](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Védett gépek** | Azure való replikáció fog védett gépek [Azure Előfeltételek](#azure-virtual-machine-requirements) leírása alább feleljen meg.


### <a name="replicate-to-a-secondary-site"></a>Bizonyos másodlagos webhelyre

**Követelmények** | **Részletek** 
---|---
**Azure-fiók** | Akkor, ha a [Microsoft Azure](http://azure.microsoft.com/) -fiókjába.<br/><br/> Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**A helyszíni** | **VMware VMs**: a elsődleges hely szüksége van egy folyamat kiszolgáló gyorsítótárazás, tömörítése és replikációs adatok titkosítása mielőtt elküldené a másodlagos webhelyet. A másodlagos webhelyen kezeléséhez, és figyelje a telepítési és a fő tároló kiszolgáló konfigurációs kiszolgáló fogja telepítése. Azt javasoljuk az egyszerűbb kezelés vContinuum kiszolgáló is. Ezenkívül van szüksége egy vagy több VMware vSphere tárolja, és tetszés szerint egy vCenter kiszolgáló. [További információ](site-recovery-vmware-to-vmware.md)<br/><br/> **Felhőket VMM a Hyper-V VMs**: VMM kiszolgálók beállítása kell, és a Hyper-V hosts VMs tartalmazó szeretné való replikáció. [További információ:](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Hálózati hozzárendelést (VMM való VMM)** | Ha Ön éppen esetében, amelyek a VMM VMM, a [hálózati hozzárendelést](site-recovery-network-mapping.md) biztosítja, hogy a replika VMs megfelelő hálózatokhoz kapcsolódó feladatátvétel után, és optimálisan kerülnek be a Hyper-V hosts. Hálózati hozzárendelést beállítása kell virtuális hálózatok konfigurálása az VMM kiszolgálókon.
**Tárterület-hozzárendelés** | Ha Ön éppen esetében, amelyek a VMM VMM, tetszés szerint beállíthatja [tároló hozzárendelése](site-recovery-storage-mapping.md) a tárhely cél megadása replikált VMs. Tárterület-hozzárendelés a Hyper-V kópia és a SAN replikáció állíthat be.<br/><br/> Tárterület-megfeleltetés jelenleg nem támogatott az Azure-portálon.


## <a name="verify-url-access"></a>Ellenőrizze az URL-cím hozzáférést

Ellenőrizze, hogy az alábbi URL-kiszolgálókról érhetők el.

**URL-CÍME** | **A VMM VMM** | **Az Azure VMM** | **Azure a Hyper-V** | **Az Azure VMware**
---|---|---|---|---
*. accesscontrol.windows.net | Engedélyezése | Engedélyezése | Engedélyezése | Engedélyezése
*. backup.windowsazure.com | Nem kötelező. | Engedélyezése | Engedélyezése | Engedélyezése
*. hypervrecoverymanager.windowsazure.com | Engedélyezése | Engedélyezése | Engedélyezése | Engedélyezése
*. store.core.windows.net | Engedélyezése | Engedélyezése | Engedélyezése | Engedélyezése
*. blob.core.windows.net | Nem kötelező. | Engedélyezése | Engedélyezése | Engedélyezése
https://www.msftncsi.com/ncsi.txt | Engedélyezése | Engedélyezése | Engedélyezése | Engedélyezése
https://dev.MySQL.com/Get/archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Nem kötelező. | Nem kötelező. | Nem kötelező. | Engedélyezése

## <a name="azure-virtual-machine-requirements"></a>Azure virtuális gép vonatkozó követelmények

Webhely helyreállítási való replikáció virtuális gépeken futó és fizikai, bármilyen Azure által támogatott operációs rendszert futtató kiszolgálók telepítheti. Ide tartoznak a legtöbb verziójának a Windows és az Linux rendszerhez. Kell győződjön meg arról, hogy a helyszíni virtuális gépeken futó, amelyet védeni kíván Azure követelményeknek felel meg.


**A szolgáltatás** | **Követelmények** | **Részletek**
---|---|---
A Hyper-V host | Futnia kell a Windows Server 2012 R2 | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha az operációs rendszere nem támogatott
VMware hipervizor  | Támogatott operációs rendszer | [A rendszerkövetelmények ellenőrzése](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
A vendégként való bekapcsolódáshoz operációs rendszer | A Hyper-V való replikáció Azure: webhely helyreállítási támogatja az összes operációs rendszerek, amelyek [támogatják az Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> VMware és fizikai kiszolgáló replikáció: jelölje be a Windows és Linux [vonatkozó követelmények](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Előfeltételek jelölőnégyzet sikertelen lesz, ha nem támogatott.
A vendégként való bekapcsolódáshoz operációs rendszer architektúra | 64 bites | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha nem támogatott
Operációs rendszer lemez mérete |  1023 GB | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha nem támogatott
Operációs rendszer lemez száma | 1 | Előfeltételek jelölőnégyzet sikertelen lesz, ha nem támogatott.
Adatok lemez száma | a 16 vagy annál kisebb (maximális értéke a virtuális gép eredményezne méretének függvényt. A 16 = XL) | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha nem támogatott
Adatok lemez virtuális mérete | Importálhat 1023 GB | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha nem támogatott
Hálózati adaptereken | Több kártya támogatottak. |
Statikus IP-cím | Támogatott | Ha az elsődleges virtuális gépet használ egy statikus IP-címet, megadhatja, hogy a statikus IP-címe a virtuális gép Azure-ban létrehozott. Figyelje meg, hogy nem támogatja a Hyper-v futó linux virtuális gép statikus IP-címet.
iSCSI lemez | Nem támogatott | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha nem támogatott
Megosztott virtuális | Nem támogatott | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha nem támogatott
FC lemez | Nem támogatott | Sikertelen lesz a Előfeltételek jelölőnégyzetet, ha nem támogatott
Merevlemez-formátum| VIRTUÁLIS <br/><br/> VHDX | Bár a VHDX jelenleg nem támogatja az Azure-ban, webhely helyreállítási automatikusan átalakítja VHDX virtuális Ha nem sikerül keresztül az Azure. Ha nem sikerül vissza a helyszíni a virtuális gépeken futó továbbra is használja a VHDX formátumot.
BitLocker | Nem támogatott | A virtuális gép védelme előtt BitLocker kell tiltható le.
Virtuális gép neve| 1 – 63 karakter. Korlátozott betűket, számokat és kötőjelet. Érdemes végén pedig használja egy betűt vagy számot | Az érték a webhely helyreállítási virtuális gép tulajdonságok módosítása
Virtuális gép típusa | <p>1 előállítása</p> <p>2 – Windows előállítása</p> |  2 generációs virtuális készülék egyszerű lemez, amelyek tartalmazzák még a lemez formátummal 1 vagy 2 adaton, amely kisebb, mint 300 GB-os támogatott VHDX OS típusát. Linux generációs 2 virtuális gépeken futó nem támogatottak. [További információ olvasható](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>A telepítési optimalizálása

Az alábbi tippek segítségével optimalizálása és a üzembe méretezni.

- **Operációs rendszer mennyiségi méret**: a virtuális gép, bizonyos az Azure az operációs rendszer mennyiségi kell legfeljebb 1 TB. Ha ennél több kötet manuálisan áthelyezheti őket egy másik lemezre telepítés megkezdése előtt.
- **Adatok lemezre méret**: Ha, hogy esetében, amelyek Azure virtuális gépen, nyolc legfeljebb 1 TB-lehetőség van legfeljebb 32 adatok lemezre. Hatékony bizonyos és ~ 32 TB virtuális gép átveszi.
- **Helyreállítási terv korlátozások**: webhely helyreállítási virtuális gépeken futó ezer is méretezhető. A helyreállítási terv fejeződjön be fölé együtt, azt a korlátozása gépek 50 helyreállítási tervben alkalmazásokhoz modellben készültek.
- **Azure szolgáltatás korlátozások**: minden Azure előfizetés előtelepített egy sor olyan alapértelmezett korlátokat egyes cloud services stb. Azt javasoljuk, hogy egy próba-előfizetése az erőforrások elérhetőségének ellenőrzése áttérni futtatása. Módosíthatja, hogy ezek a korlátok Azure támogatási keresztül.
- **Kapacitás tervezés**: További információ: [kapacitás tervezés](site-recovery-capacity-planner.md) webhely-helyreállítás.
- **A replikáció sávszélesség**: Ha Ön a replikáció sávszélesség rövid vegye figyelembe, hogy:
    - **Készült ExpressRoute**: webhely helyreállítási működik-e a készült Azure ExpressRoute és WAN optimizers például Riverbed. [Szeretné, többet is](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) megtudhat a készült ExpressRoute.
    - **Replikációs forgalom**: webhely helyreállítási használ egy intelligens kezdeti replikációs csak adatblokk, és nem a teljes virtuális hajt végre. Folyamatban lévő replikáció során replikált csak a módosításokat.
    - **Hálózati forgalmának engedélyezésére**: megadhatja, hogy [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) beállítja a cél IP-címe és a port alapján szabálya replikációs használható hálózati forgalmának engedélyezésére.  Ezenkívül ha, hogy esetében, amelyek az Azure biztonságimásolat-ügynök használatával Azure webhely helyreállítási beállíthatja úgy, hogy Agent szabályozásának. [További információ:](https://support.microsoft.com/kb/3056159).
- **RTO**: a helyreállítási idő cél (RTO) webhely helyreállítási számíthat mérésére ajánlott a próba feladatátvevő futtat, és a webhely helyreállítási feladatok elemezheti, hogy mennyi időzítése nézet esetén, a művelet elvégzéséhez. Ha az Azure adatkapcsolat esetén fölé, az a legjobb RTO azt javasoljuk, hogy az összes kézi műveletek integrálása az Azure automatizálási és helyreállítási tervek automatizálhatja.
- **Készletben**: webhely helyreállítási támogatja a közelében szinkron helyreállítási pont célkitűzés (Készletben), ha az, hogy Azure bizonyos. Feltételezi, hogy a megfelelő sávszélesség a adatközponthoz és Azure között.


##<a name="service-urls"></a>Szolgáltatás URL-címei
Ellenőrizze, hogy az URL-címet a kiszolgálóról hozzáférhetők.


**URL-címei** | **A VMM VMM** | **Az Azure VMM** | **Azure-webhelyére a Hyper-V** | **Az Azure VMware**
---|---|---|---|---
 \*. accesscontrol.windows.net | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges
 \*. backup.windowsazure.com |  | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges
 \*. hypervrecoverymanager.windowsazure.com | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges
 \*. store.core.windows.net | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges
 \*. blob.core.windows.net |  | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges
 https://www.msftncsi.com/ncsi.txt | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges  | Hozzáférés szükséges
 https://dev.MySQL.com/Get/archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Hozzáférés szükséges


## <a name="next-steps"></a>Következő lépések

Tanulási és összehasonlítása az általános telepítési követelmények után olvassa el a részletes Előfeltételek, és indítsa el az egyes forgatókönyv üzembe helyezése.

- [Azure való replikáció a VMware virtuális gépeken futó](site-recovery-vmware-to-azure-classic.md)
- [Azure párhuzamos fizikai kiszolgálók](site-recovery-vmware-to-azure-classic.md)
- [Bizonyos VMM felhőket a Hyper-V server Azure való](site-recovery-vmm-to-azure.md)
- [Az Azure bizonyos a Hyper-V virtuális gépeken futó (nélkül VMM)](site-recovery-hyper-v-site-to-azure.md)
- [A Hyper-V VMs bizonyos másodlagos webhelyre](site-recovery-vmm-to-vmm.md)
- [A Hyper-V VMs névkiszolgálóit SAN másodlagos webhelyre](site-recovery-vmm-san.md)
- [A Hyper-V VMs névkiszolgálóit egyetlen VMM kiszolgáló](site-recovery-single-vmm.md)
