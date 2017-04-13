<properties
    pageTitle="Azure webhely helyreállítás: Gyakori kérdések |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogy a népszerű kérdések Azure webhely helyreállítása."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure webhely helyreállítás: Gyakori kérdések

Ez a cikk gyakori kérdéseket Azure webhely helyreállítási tartalmazza. Ez a cikk elolvasása után kérdései vannak, ha fel őket a [Azure helyreállítási szolgáltatások fórumán](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Általános

### <a name="what-does-site-recovery-do"></a>Mit tesz a webhely helyreállítási?

Az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, orchestrating és Azure, illetve egy másodlagos adatközponthoz a helyszíni virtuális gépeken futó és fizikai kiszolgálók replikációs automatizálása webhely helyreállítási hozzájárul. [Tudjon meg többet](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Mi a webhely helyreállítási megvédheti?

- **A Hyper-V virtuális gépeken futó**: webhely helyreállítási megvédheti bármely terhelést a Hyper-V virtuális futó.
- **Fizikai kiszolgálók**: webhely helyreállítási megvédheti fizikai kiszolgálón a Windows vagy Linux rendszerhez.
- **VMware virtuális gépeken futó**: webhely helyreállítási megvédheti minden futó egy VMware virtuális terhelést.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Webhely helyreállítási támogatja az erőforrás-kezelő Azure modell?

Nemcsak a webhely-helyreállítás az Azure klasszikus portálon webhely helyreállítási érhető el az erőforrás-kezelő támogatási az Azure portálon. Telepítési találatokkal webhely helyreállítási az Azure-ban a portálon egyesíti telepítési felületet nyújt, és, hogy bizonyos VMs és fizikai kiszolgálók klasszikus tároló vagy az erőforrás-kezelő tároló. Az alábbiakban a támogatott telepítések:

- [VMware VMs vagy a fizikai kiszolgálók Azure való replikáció az Azure-portálon](site-recovery-vmware-to-azure.md)
- [A Hyper-V VMs VMM felhőket a replikáció az Azure az Azure-portálon](site-recovery-vmm-to-azure.md)
- [A Hyper-V VMs (nélkül VMM) bizonyos az Azure az Azure-portálon](site-recovery-hyper-v-site-to-azure.md)
- [A Hyper-V VMs VMM felhőket a replikáció az Azure-portálon másodlagos webhelyre](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Mit kell a Hyper-V webhely helyreállítási való replikáció téve?

A Hyper-V kiszolgálóhoz szükségesek függ, hogy a telepítéshez. Nézze meg a Hyper-V Előfeltételek a:

- [Esetében, amelyek a Hyper-V VMs (nélkül VMM) Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Esetében, amelyek a Hyper-V VMs (a VMM) Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Esetében, amelyek a Hyper-V VMs egy másodlagos adatközponthoz](site-recovery-vmm-to-vmm.md#before-you-start)

- Ha Ön éppen esetében, amelyek egy másodlagos adatközponthoz további információ: [a Hyper-V VMs támogatott vendég operációs rendszeren](https://technet.microsoft.com/library/mt126277.aspx).
- Ha Ön éppen esetében, amelyek Azure, webhely helyreállítási támogatja a összes a vendégként való bekapcsolódáshoz operációs rendszerek, amelyek [támogatják az Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Védhetők VMs, amikor a Hyper-V ügyfél operációs rendszeren működik?

Nem, VMs kiszolgálón a Hyper-V támogatott a Windows server gépen futó kell lennie. Ha módosítani szeretné egy ügyfélszámítógép védelme, mint egy fizikai számítógépre [Azure](site-recovery-vmware-to-azure.md) vagy egy [másodlagos adatközponthoz](site-recovery-vmware-to-vmware.md)sikerült bizonyos.


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Milyen munkaterhelésekből is védelme a webhely helyreállítási?

A támogatott virtuális vagy fizikai kiszolgálón futó legtöbb feladatok védelme webhely helyreállítási is használhatja. Webhely helyreállítási támogatja a replikáció alkalmazás szem előtt, hogy az alkalmazások intelligens állapotra állíthatók helyre. A Microsoft-alkalmazásokhoz, például a SharePoint, az Exchange, Dynamics, SQL Server és az Active Directory integrálódik, és szorosan működik-e a vezető szállítók, beleértve az Oracle, SAP, IBM és piros kalap. [Tudjon meg többet](site-recovery-workload.md) is megtudhat a terhelést védelem.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Végezze el a Hyper-V hosts kell VMM felhő?

Ha szeretne egy másodlagos adatközponthoz való replikáció a Hyper-V VMs kell lennie, majd a Hyper-V egy VMM felhőben kiszolgálók tárolja. Ha szeretné az Azure való replikáció, majd, hogy bizonyos VMs a Hyper-V host kiszolgálókon vagy VMM felhőket nélkül. [További információ:](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Telepíthetem VMM a webhely helyreállítás, ha egy VMM kiszolgáló csak van?

igen. Hogy bizonyos ugyanazon a kiszolgálón VMM felhő között, vagy hogy vagy bizonyos VMs a Hyper-V Servers a VMM felhőben való Azure. Helyszíni telepítésű való replikáció a helyszíni javasoljuk, hogy VMM kiszolgáló az elsődleges és másodlagos webhelyeken.  [További információ](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Milyen fizikai kiszolgálók is védelme?

Fizikai kiszolgálón a Linux és a Windows Azure vagy másodlagos webhelyre, hogy bizonyos. [Megtudhatja, hogy](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) operációs rendszerre vonatkozó követelmények.  A azonos követelmények vonatkoznak, hogy éppen replikálják fizikai kiszolgálók, Azure, illetve egy másodlagos webhely-e.

Figyelje meg, hogy fizikai kiszolgálók fog futni VMs Azure-ban, ha megszakad a helyszíni kiszolgálót. Egy helyszíni fizikai kiszolgálóhoz visszaállás jelenleg nem támogatott, de vissza a Hyper-V vagy VMware rendszeren futó virtuális gép sikertelen lehet.


### <a name="what-vmware-vms-can-i-protect"></a>Milyen VMware VMs is védelme?

VMware VMs védelme szüksége lesz egy vSphere hipervizor és virtuális gépeken futó VMware eszközök futtatása. Azt is javasoljuk, hogy kezelheti a hypervisors VMware vCenter kiszolgáló. [Tudjon meg többet](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) a pontos követelmények kiegészítését VMware kiszolgálók és VMs, Azure, illetve egy másodlagos webhelyet.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Lehet-e kezelni vészhelyreállítás a ág irodák a webhely helyreállítási?

igen. Webhely helyreállítási való replikáció és feladatátvételi téve a ág irodák használatakor vissza egy egyesített üzletifolyamat-tervező és a fiók office feladatok nézetét egy központi helyen. Egyszerűen feladatátadás futtatása és felügyelete a központi irodából az összes ágak vészhelyreállítás anélkül, hogy a fiókok megtalálhatók.

## <a name="security"></a>Biztonsági

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>A webhely helyreállítás szolgáltatással küldött replikációs adatok?

Webhely-helyreállítás nem, nem metsz replikált adatokat, és nincs semmilyen adatot a virtuális gépeken futó vagy a fizikai kiszolgálókon rendszert futtató.
A replikáció adatcsere helyszíni a Hyper-V hosts, VMware hypervisors, vagy a fizikai kiszolgálók és Azure tároló vagy a másodlagos webhely között. Webhely helyreállítási adatokat metsz nincs lehetősége van. A webhely helyreállítási szolgáltatás csak a metaadat-alapú való replikáció és feladatátvételi téve szükséges küldi.

Webhely-helyreállítás az ISO 27001:2013, 27018, HIPAA DPA tanúsítvánnyal és SOC2 és FedRAMP JAB értékelést folyamatban van.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Megfelelőségi okokból még a helyszíni metaadatok ugyanabban a földrajzi régióban belül kell maradnia. Is webhely helyreállítási segítse a szolgáltatás?

igen. Egy webhely helyreállítási tárolóból elemre egy tartomány létrehozásakor azt győződjön meg arról, hogy engedélyezése és a replikáció téve szükség, és adott területen belül maradjon feladatátvevő metaadatokkal adatait a földrajzi határérték.

### <a name="does-site-recovery-encrypt-replication"></a>Nem webhely helyreállítási titkosítása replikációs?

Virtuális gépeken futó és a fizikai kiszolgálók között a helyszíni webhelyek titkosítási-a-hálózaton átvitt replikálja funkció használható. És fizikai kiszolgálók esetében, amelyek Azure virtuális gépeken futó titkosítási-a-hálózaton átvitt és a titkosítási-a-többi (az Azure) támogatott.


## <a name="replication"></a>A replikáció


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Van bármilyen esetében, Azure virtuális gépeken futó amelyek előfeltételei?

Azure való replikáció kívánt virtuális gépeken futó meg kell feleljen az [Azure követelményeknek](site-recovery-best-practices.md#virtual-machines).

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hogy a Hyper-V generációs 2 virtuális gépeken futó is bizonyos az Azure?

igen. Webhely helyreállítási generációs 1 feladatátvételkor a 2 generációs alakítja át. A visszaállás a gép alakul vissza 2 generációs. [További információ:](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Ha e Azure való replikáció hogyan végezze el tudom fizet az Azure VMs?

Normál replikáció során adatok geo felesleges Azure tárolóhoz van replikált, és nem kell minden Azure IaaS virtuális gép díjat fizet kezeléséről a jelentős kihasználásához. Feladatátvevő Azure készítésekor webhely helyreállítási automatikusan létrehozza a Azure IaaS virtuális gépeken futó, és ezt követően meg fognak számlát kapni a számítási erőforrások, amely akkor felhasználása Azure-ban.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Van-e egy SDK csomagjában talál a automatikus rendszer-Helyreállítás munkafolyamat automatizálásához használható?

igen. Automatizálható a Rest API-t, PowerShell vagy az Azure SDK webhely helyreállítási munkafolyamatok. Jelenleg támogatott esetek üzembe helyezése a PowerShell használatá webhely helyreállítás:

- [Bizonyos VMMs felhőket a Hyper-V VMs Azure PowerShell klasszikus](site-recovery-deploy-with-powershell.md)
- [VMMs felhőket a Hyper-V VMs Azure PowerShell-kezelő való replikáció](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Azure PowerShell klasszikus bizonyos a Hyper-V VMs VMM nélkül](site-recovery-hyper-v-site-to-azure-classic.md)
- [A Hyper-V VMs nélkül VMM bizonyos az Azure PowerShell erőforrás-kezelő](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Ha e Azure való replikáció tároló fiók milyen típusú van szükségem?

- **Azure klasszikus portálon**: webhely helyreállítási az Azure klasszikus portálon használja telepít, akkor el kell egy [szabványos geo felesleges tárterület-fiók](../storage/storage-redundancy.md#geo-redundant-storage). Prémium tároló jelenleg nem támogatott. A fiók kell lennie az ugyanazon régió, a webhely helyreállítási tárolóból elemre.

- **Azure portálon**: webhely helyreállítási az Azure-portálon használja telepít, akkor el kell LRS vagy GRS tároló fiók. GRS javasoljuk, hogy a adatokat rugalmas egy területi üzemszünetek esetén, vagy ha az elsődleges régió sem állíthatók. A fiók kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre. Csak akkor, ha éppen replikálása, VMware VMs vagy a fizikai kiszolgálók prémium tároló támogatott.

### <a name="how-often-can-i-replicate-data"></a>Milyen gyakran hogy bizonyos adatok?

- **A Hyper V:** A Hyper-V VMs 30 másodpercenként, 5 perccel vagy 15 perccel replikálhatók. Ha be van állítva a SAN replikációs replikációs szinkron.
- **VMware és fizikai kiszolgálók:** A replikáció gyakoriságának itt nem releváns. A replikáció folyamatos.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Bővíthetik a replikáció meglévő helyreállítási webhelyről a másikra harmadlagos?

Bővített vagy láncolt replikációs használata nem támogatott. Ez a funkció [Visszajelzés](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication)fórumán kérhet.


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Az offline replikációs lehet Azure való replikáció először lehet tenni?

Ez nem támogatott. Ez a funkció [Visszajelzési fórum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)kérhet.


### <a name="can-i-exclude-specific-disks-from-replication"></a>Is adott lemezt is kizárhat való replikáció?

Ez támogatott [VMware VMs és fizikai kiszolgálók replikálása](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) Azure, az Azure portálon közben.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Virtuális gépeken futó dinamikus lemezen, hogy bizonyos?

Dinamikus lemezen a Hyper-V virtuális gépeken futó verzióazonosítójának támogatottak. Szintén támogatottak, amikor esetében, VMware VMs és fizikai gépek amelyek Azure. Az operációs rendszer lemezen egyszerű lemezen kell lennie.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Vehetek fel egy új számítógépre való replikáció meglévő csoport?

Új gépek hozzáadása meglévő replikációs csoportok funkció használható. Ehhez jelölje ki a replikációs csoport (a "Replikált elemek" lap), és kattintson a jobb gombbal/kijelölése helyi menüje a replikáció csoportra, és jelölje ki a megfelelő lehetőséget.

![Replikációs csoport hozzáadása](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>A Hyper-V replikációs forgalmához kiosztott sávszélesség is szabályozása?

igen. Erről további tudnivalók a telepítés parancs a sávszélesség szabályozása:

- [Kiegészítését VMware VMs és fizikai kiszolgálókat tervezési kapacitás](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Kapacitás VMM felhőket a Hyper-V VMs kiegészítését tervezése](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [A Hyper-V VMs nélkül VMM kiegészítését tervezési kapacitás](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Feladatátvevő


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Ha e vagyok sikertelenül fölé Azure, hogyan férhetek hozzá az Azure virtuális gépeken futó feladatátvétel után?

Hozzáférhet az Azure VMs biztonságos Internet-kapcsolaton keresztül, a webhely VPN, vagy a készült Azure ExpressRoute keresztül. Felkészülés a dolgokat, hogy csatlakoztatni számos kell. További információ a:

- [Csatlakozás Azure VMs VMware VMs vagy a fizikai kiszolgálók után](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Csatlakozás Azure VMs VMM felhőket a Hyper-V VMs feladatátvételének után](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Csatlakozás után a Hyper-V VMs nélkül VMM feladatátvételének Azure VMs](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Ha e átadni Azure hogyan Azure győződjön meg arról, hogy az adatok az rugalmassá?

Azure címtárfrissítések lett tervezve. Webhely helyreállítási már visszafejtett a másodlagos Azure adatközponthoz megfelelően az Azure SLA áttérni, szükség esetén. Ez történik, ha azt győződjön meg arról, hogy a metaadat-alapú, és tárolókban az ugyanazon a tárolóból elemre, amelyhez földrajzi régióban belül maradjon.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Ha e vagyok replikálása között két adatközpontokkal mi történik, ha az elsődleges adatközponthoz egy váratlan üzemszünetek?

A másodlagos webhelyről feladatátvételhez válthat. Webhely helyreállítási connectivity az elsődleges webhelyről, ha azt szeretné, hajtsa végre a feladatátvételi nem szükséges.

### <a name="is-failover-automatic"></a>Az automatikus feladatátvevő?

Feladatátvevő nem automatikus. A portálon egyetlen kattintással feladatátadás kezdeményez vagy [Webhely helyreállítási PowerShell](https://msdn.microsoft.com/library/dn850420.aspx) használatával feladatátvevő elindítani. A webhely helyreállítási portálon egy egyszerű műveletet vissza nem működnek.

Automatizálása, sikerült helyszíni Orchestrator vagy Operations Manager virtuális gép hiba feltárása, és kattintson a a feladatátvételt a SDK használatával válthat.

- [További információ:](site-recovery-create-recovery-plans.md) helyreállítási csomagokról.
- [További információ:](site-recovery-failover.md) feladatátvevő kapcsolatban.
- [További](site-recovery-failback-azure-to-vmware.md) tudnivalók a hibás biztonsági VMware VMs és fizikai kiszolgálók


## <a name="service-providers"></a>Szolgáltatók


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>A szolgáltató vagyok. Webhely helyreállítási dedikált és megosztott infrastruktúra modellek működik?

Igen, a webhely helyreállítási mindkét dedikált és megosztott infrastruktúra modellek támogatja.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Egy szolgáltatót az a webhely helyreállítás szolgáltatással megosztott bérlői azonosítója?

nem. Bérlői identitás névtelen marad. A bérlők a webhely helyreállítási portál eléréséhez nem szükséges. A service provider rendszergazda hogyan kommunikáljon a portálon.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Bérlői alkalmazás adatok minden eddiginél ugrik Azure?

Service provider tulajdonú webhelyek között verzióazonosítójának alkalmazás adatokat a Azure soha nem ugrik. Adatok titkosítva van, a hálózaton átvitt és replikált közvetlenül az internetszolgáltató webhelyek között.

Ha Ön éppen esetében, amelyek Azure, alkalmazás adatokat küldi el Azure tárhely, de nem a webhely helyreállítási szolgáltatás. Adatok titkosított az-hálózaton átvitt, és továbbra is a titkosított Azure-ban.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>A bérlők számla, Azure szolgáltatások fog kapni?

nem. Azure a számlázási kapcsolat közvetlenül a szolgáltató lesz. Szolgáltatók egyedi számlák létrehozása a bérlők a felelősek.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Ha e vagyok esetében, amelyek Azure, szükséges Azure virtuális gépeken futó futhat mindig?

Nem, adatok replikált van az előfizetés az Azure tároló fiókba. Teszt feladatátvevő (DR részletező) vagy egy tényleges feladatátvevő végrehajtásakor webhely helyreállítási automatikusan létrehozza virtuális gépeken futó előfizetését.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Hajtsa végre, biztosítják a bérlői szinten elkülönítési lehet Azure való replikáció?

igen.

### <a name="what-platforms-do-you-currently-support"></a>Milyen platformon jelenleg támogatják?

Azure csomag, felhőalapú Platform a rendszer támogatjuk, és a System Center-alapú (2012 vagy újabb) telepítések. [Tudjon meg többet](https://technet.microsoft.com/library/dn850370.aspx) : Azure csomag és a webhely helyreállítási integráció.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Támogatják egyetlen Azure csomag és egyetlen VMM server-telepítések?

Igen, Azure való replikáció a Hyper-V virtuális gépeken futó, vagy bizonyos internetszolgáltató webhelyek között.  Figyelje meg, hogy ha, bizonyos internetszolgáltató webhelyek között, Azure runbook integráció nem használható.


## <a name="next-steps"></a>Következő lépések

- Olvassa el a [webhely helyreállítás – áttekintés](site-recovery-overview.md)
- Tudnivalók a [helyreállítási webhely-architektúra](site-recovery-components.md)  
