<properties
    pageTitle="Magas rendelkezésre állásának és a helyreállítás az SQL Server |} Microsoft Azure"
    description="A különböző típusú HADR stratégiák fut az Azure virtuális gépeken futó SQL Server vitafórum."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Az Azure virtuális gépeken futó SQL Server magas rendelkezésre állásának és katasztrófa helyreállítás

## <a name="overview"></a>– Áttekintés

Microsoft Azure virtuális gépeken futó (VMs) SQL Server-is csökkentse a magas rendelkezésre állásának és katasztrófa helyreállítási (HADR) adatbázis-megoldás költségét. A legtöbb SQL Server HADR megoldások Azure virtuális géphez, csak Azure- és hibrid megoldásként támogatottak. Egy csak Azure megoldást a teljes HADR rendszer fut. Azure-ban. A hibrid konfigurációban a megoldást részét fut Azure és a többi részét futtatja a helyszíni a szervezet. Az Azure környezetben rugalmasan lehetővé teszi, hogy lépjen a teljesen vagy részben Azure felel meg a a költségvetési és az SQL Server-adatbázis rendszerek HADR követelményeinek.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Egy HADR megoldás kell ismertetése

Célszerű annak érdekében, hogy az adatbázis-rendszer rendelkezik-e a szolgáltatói szerződést (SLA) igényel HADR funkcióját. Arra, hogy az Azure biztosít magas elérhetősége mechanizmusok, például a felhőszolgáltatások és a hiba helyreállítási észlelése a virtuális gépeken futó javító szolgáltatás nem önmagában garantálja is teljesíti a kívánt SLA. Ezek a mechanizmusok védelme a VMs a magas elérhetőségét, de nem fut a VMs belül SQL Server a magas elérhetőségét. Az SQL Server-példány sikertelen lesz, amíg a virtuális online és a megfelelő lehetőség. Ezenkívül még Azure által biztosított mechanizmusok lehetővé teszi a VMs miatt eseményeket, például a szoftver vagy a hardver hibák és az operációs rendszer frissítése helyreállítása az állásidőt sürgős elérhetőségét.

Ezeken kívül Geo felesleges tároló (GRS) az Azure egy geo replikációs nevű funkció hajtanak végre, amelyeket nem lehet egy megfelelő katasztrófa helyreállítási megoldás az adatbázisok. A replikáció geo aszinkron adatokat küld, mert nemrégiben végzett módosításokat elveszhetnek katasztrófa esetén. Az [adatokat, és jelentkezzen be a fájlok különböző lemezen nem támogatott a replikáció Geo](#geo-replication-support) szakasz geo replikációs korlátozások kapcsolatos további információkért ismerkedhet meg.

## <a name="hadr-deployment-architectures"></a>HADR telepítési architektúrákban

Azure-ban támogatott SQL Server HADR technológiák a következők:

- [Mindig a csoportok elérhetősége](https://technet.microsoft.com/library/hh510230.aspx)
- [Adatbázis tükrözése](https://technet.microsoft.com/library/ms189852.aspx)
- [Log szállítási](https://technet.microsoft.com/library/ms187103.aspx)
- [Biztonsági mentési és visszaállítási Azure Blob-tároló szolgáltatással](https://msdn.microsoft.com/library/jj919148.aspx)
- [Mindig a Feladatátvevőfürt példányok](https://technet.microsoft.com/library/ms189134.aspx)

A közös magas rendelkezésre állásának és katasztrófa helyreállítási lehetőségeket is tartalmazó SQL Server megoldást végrehajtásához technológiák egyesítéséhez lehetőség. Attól függően, hogy a technológiát használ a hibrid telepítés megkövetelheti egy virtuális Magánhálózati alagutas az Azure virtuális hálózathoz. Az alábbi szakaszokban mutatja, hogy a példa telepítési architektúrákban részét.

## <a name="azure-only-high-availability-solutions"></a>Ha csak Azure: magas elérhetősége megoldások

Telepítve van egy nagy elérhetősége megoldás az SQL Server-adatbázisok csoportokkal mindig a elérhetősége Azure-ban, vagy tükrözése az adatbázis.

| Technológia                               | Példa architektúrákban                    |
| ---------------------------------------- | ---------------------------------------- |
| **Mindig a csoportok elérhetősége**        | Az összes elérhetősége kópiák magas elérhetőség az azonos régión belüli Azure VMs futó. Szeretne beállítani egy tartományvezérlőnek virtuális, mert a Windows Server feladatátvevő fürtözés (WSFC) csak az Active Directory tartományi.<br/> ![Mindig a csoportok elérhetősége](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>További tudnivalókért lásd: [Konfigurálása mindig a elérhetősége csoportok Azure (grafikus)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Mindig a Feladatátvevőfürt példányok** | Feladatátvevő fürt példányok (FCI), a megosztott tároló igénylő 2 különböző módokon hozhat létre.<br/><br/>1. a egy két-csomópont WSFC az Azure VMs adathordozós külső fürtképző megoldást által támogatott operációs rendszert futtató FCI. Egy adott példa SIOS DataKeeper használó című témakörben [WSFC és a 3 fél szoftver SIOS Datakeeper fájlmegosztás magas elérhetőséget](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. a egy két-csomópont WSFC rendszerrel távoli iSCSI tároló Azure VMs FCI megosztott blokkokból álló tárhely készült ExpressRoute keresztül. Például a NetApp magánjellegű tároló (házirend) keresztül az Azure VMs való Equinix készült ExpressRoute iSCSI-tároló közzététele.<br/><br/>Harmadik fél megosztott tárolási és adatok replikációs megoldások lépjen kapcsolatba a szállító eléréséhez a feladatátvevő kapcsolatos problémák megoldásához.<br/><br/>Figyelje meg, hogy fölött [Azure fájltároló](https://azure.microsoft.com/services/storage/files/) FCI használata nem támogatott még, mivel a megoldás prémium tároló nem használja. A támogatási ez leggyorsabban dolgozunk. |

## <a name="azure-only-disaster-recovery-solutions"></a>Ha csak Azure: katasztrófa helyreállítási megoldások

Az SQL Server-adatbázisok csoportokkal mindig a elérhetőség Azure-ban, adatbázis tükrözési vagy biztonsági másolatot katasztrófa helyreállítási megoldást van, és tárolási okkal visszaállítása.

| Technológia                               | Példa architektúrákban                    |
| ---------------------------------------- | ---------------------------------------- |
| **Mindig a csoportok elérhetősége**        | Elérhetőség kópiák vészhelyreállítás az Azure VMs több adatközpontokkal keresztül futó. A területi határokon megoldás hely teljes üzemszünetek védelmet. <br/> ![Mindig a csoportok elérhetősége](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Terület belül az összes másolatnál ugyanazt a felhőalapú szolgáltatást, és az azonos VNet belül kell lennie. Egyes régiókra egy külön VNet lesz, mert ezek a megoldások VNet VNet kapcsolat szükséges. További tudnivalókért lásd: a [Configure egy webhely VPN az Azure klasszikus portálon](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Adatbázis tükrözése**                   | Egyszerű és Tükörmargók és más adatközpont esetén vészhelyreállítás futtató kiszolgálók. Telepíteni kell az kiszolgálói tanúsítványok, mert az active directory-tartománya nem időtartomány több adatközpontokkal.<br/>![Adatbázis tükrözése](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Biztonsági mentési és visszaállítási Azure Blob-tároló szolgáltatással** | Végleges adatbázis biztonsági másolat közvetlenül egy másik adatközpontban vészhelyreállítás blob-tárolóhoz.<br/>![Biztonsági mentési és visszaállítási](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>További tudnivalókért olvassa el a [biztonsági mentése és visszaállítása az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-backup-recovery.md)című témakört. |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hibrid informatikai: Katasztrófa helyreállítási megoldások

Telepítve van az SQL Server-adatbázisok hibrid-környezetben mindig a elérhetősége csoportok, az adatbázis-tükrözéshez, a napló szállítási és a biztonsági másolat katasztrófa helyreállítási megoldást, és Azure blog adathordozós visszaállítása.

| Technológia                               | Példa architektúrákban                    |
| ---------------------------------------- | ---------------------------------------- |
| **Mindig a csoportok elérhetősége**        | Néhány elérhetősége kópiát az Azure VMs és más kópiák webhelyközi vészhelyreállítás helyszíni futó operációs rendszert futtató. A gyártási webhely lehet használatát vagy a helyszíni, vagy az Azure adatközpontban.<br/>![Mindig a csoportok elérhetősége](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Mivel minden elérhetősége kópiák a azonos WSFC fürt kell lennie, akkor a WSFC fürt mindkét hálózatban (a több elem alhálózat WSFC fürtre) kell időtartomány. A fenti konfiguráció szükséges a virtuális Magánhálózati kapcsolat közötti Azure és a helyszíni hálózaton.<br/><br/>Az adatbázisok sikeres vészhelyreállítás is telepíteni kell egy replika tartományvezérlőnek katasztrófa helyreállítási webhelyén.<br/><br/>Azure replikájának ad hozzá egy meglévő mindig a elérhetőség csoporthoz SSMS replika hozzáadása varázsló segítségével lehetőség. További tudnivalókért lásd: oktatóprogram: Azure használatának kiterjesztése a mindig a rendelkezésre állási csoport. |
| **Adatbázis tükrözése**                   | Egy partner fut az Azure virtuális és a más futó helyszíni webhelyközi vészhelyreállítás kiszolgálói tanúsítványok használatával. Partnerek nem kell az Active Directory-tartomány, és nincs virtuális Magánhálózati kapcsolat szükség.<br/>![Adatbázis tükrözése](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Egy másik adatbázis forgatókönyv tükrözése az Azure virtuális és a más futó helyszíni Active Directory tartományában webhelyközi vészhelyreállítás futtató egyik partner magába foglalja. A [virtuális Magánhálózati kapcsolatot az Azure virtuális hálózati és a helyszíni hálózaton között](../vpn-gateway/vpn-gateway-site-to-site-create.md) szükség.<br/><br/>Az adatbázisok sikeres vészhelyreállítás is telepíteni kell egy replika tartományvezérlőnek katasztrófa helyreállítási webhelyén. |
| **Log szállítási**                         | Az Azure virtuális és a más futó operációs rendszert futtató egyik kiszolgáló helyszíni webhelyközi vészhelyreállítás. Log szállítási függ Windows fájlmegosztás, ezért a virtuális Magánhálózati kapcsolat között az Azure virtuális hálózati és a helyszíni hálózaton.<br/>![Log szállítási](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Az adatbázisok sikeres vészhelyreállítás is telepíteni kell egy replika tartományvezérlőnek katasztrófa helyreállítási webhelyén. |
| **Biztonsági mentési és visszaállítási Azure Blob-tároló szolgáltatással** | A helyszíni gyártási adatbázisok közvetlenül az Azure blob-tárolóhoz vészhelyreállítás készített biztonsági másolatot.<br/>![Biztonsági mentési és visszaállítási](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>További tudnivalókért olvassa el a [biztonsági mentése és visszaállítása az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-backup-recovery.md)című témakört. |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Fontos dolgot az SQL Server HADR Azure-ban

Azure VMs, a tárhely és a hálózat van egy helyszíni nem virtualizálva informatikai infrastruktúrát eltérő működési jellemzői. A sikeres HADR SQL Server Azure-ban megoldást végrehajtásához megérteni a különbségeket, és a tervezése a megoldás, hogy beleférjenek őket.

### <a name="high-availability-nodes-in-an-availability-set"></a>Az elérhetőség magas elérhetősége csomópontok beállítása

Elérhetőség beállítja az Azure lehetővé teszi a magas elérhetősége csomópontok helyezhet külön hibafa tartományok (FDs), és a frissítés tartományok (UDs). Az Azure VMs elérhetősége ugyanazok a kivágást telepítenie kell őket az ugyanazt a felhőalapú szolgáltatást. Csak az ugyanazt a felhőalapú szolgáltatást található csomópontok elérhetőség mindazon vehet részt. További információ a [virtuális gépeken futó elérhetőségének kezelése](virtual-machines-windows-manage-availability.md)című cikkben talál.

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>Azure hálózati WSFC fürt működése

Az Azure-ban nem-szerinti DHCP szolgáltatás okozhatják létrehozását bizonyos WSFC fürt konfigurációk a fürt hálózati neve alatt ismétlődő IP-címmel, például ugyanazt a címet a fürt csomópontok egyikeként miatt sikertelen lesz. Ez a probléma, amikor mindig a elérhetősége csoportok, amelyek függ, hogy a WSFC funkció alkalmazhat.

Ha két-csomópont fürt létrejön, és online állapotba, vegye figyelembe az alkalmazási példát:

1. A fürt online állapotba kerül, majd NODE1 kéri a fürt hálózat nevét egy dinamikusan hozzárendelt IP-címet.

2. Nincs IP-cím eltérő NODE1 saját IP-cím képlettel a DHCP szolgáltatást, mivel a DHCP szolgáltatás ismer fel, hogy a kérelem származik NODE1 magát.

3. A Windows észleli, hogy egy ismétlődő cím hozzá van rendelve, NODE1 és a fürt hálózat nevét, és az alapértelmezett fürt csoport nem sikerül online állapotba.

4. Az alapértelmezett fürt csoport NODE2, amely NODE1's IP-cím tekinti a fürt IP-címét, és megjeleníti az alapértelmezett fürt csoport online lép.

5. Amikor megkísérel NODE2 NODE1 kapcsolatot létrehozni, a csomagok NODE1 irányuló soha ne hagyja NODE2, mert NODE1's IP-cím feloldja saját magának. NODE2 nem tud NODE1 kapcsolatot létrehozni, majd elveszti a kvórumot, és leállítja a fürt.

6. Időközben NODE1 csomagok NODE2 küldhet, de NODE2 nem lehet válaszolni. NODE1 elveszti a kvórumot, és leállítja a fürt.

Ebben az esetben elkerülhető oszt ki egy még nem használt statikus IP-cím, például egy kapcsolaton belüli IP-cím 169.254.1.1, például a fürt hálózat nevét, annak érdekében, hogy online fürt hálózat nevét. Egyszerűsítése érdekében ezt a folyamatot, olvassa el a [Windows Feladatátvevőfürt konfigurálása mindig a elérhetősége csoportok Azure-ban](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx)című témakört.

További tudnivalókért lásd: [Konfigurálása mindig a elérhetősége csoportok Azure (grafikus)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Elérhetőség csoport figyelő támogatás

Elérhetőség csoport hallgatók fut a Windows Server 2008 R2, a Windows Server 2012-ben, a Windows Server 2012 R2 és a Windows Server 2016 Azure VMs támogatottak. Ez a támogatás engedélyezve van az Azure VMs, amelyek az elérhetőség csoport csomópontok terheléselosztás végpontok használatával lett lehetséges. Hajtsa végre a hallgatók mindkét ügyfélalkalmazásokban Azure, valamint a helyszíni konstrukciókban futó személyre a speciális konfigurációs lépéseket.

Kétféleképpen lehet fő beállítása a figyelő: (nyilvános) külső vagy belső. A külső (nyilvános) figyelő szemben lévő terheléselosztó internetes használ, és a kapcsolódó az egy nyilvános virtuális IP (virtuális), amely az interneten keresztül érhető. Egy belső figyelő egy belső terheléselosztó használ, és csak támogatja az ügyfelek virtuális hálózaton belül. Bármelyik terheléselosztó típus betöltése, engedélyeznie kell a közvetlen vissza. 

Az elérhetőség csoport átnyúlik több Azure alhálózat (például tartalmazó Azure régiók telepítés), ha az ügyfél kapcsolati karakterlánc tartalmazhat "**MultisubnetFailover = True**". Ennek eredményeképpen a párhuzamos kapcsolatfelvételi a különböző alhálózathoz kópiájába. Egy figyelő beállításával kapcsolatos útmutatásért lásd:

- [Mindig a elérhetősége csoportok Azure-ILB figyelő konfigurálása](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Egy külső figyelő mindig a elérhetősége csoportok Azure konfigurálása](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Továbbra is csatlakozhat mindegyik elérhetőség kópiában külön-külön való csatlakozással közvetlenül a szolgáltatás példánya. Is mivel a mindig a elérhetősége csoportok adatbázis-ügyfelek tükrözési kompatibilis, csatlakozhat az elérhetőség kópiák például adatbázis-partnerek tükrözés, mindaddig, amíg a replikák megtörténik az adatbázis-tükrözéshez hasonlít:

- Egy elsődleges kópiát, és egy másodlagos replika

- A másodlagos replika nem olvasható (**Másodlagos olvasható** beállítást **nem**értékre) van beállítva

Egy példa ügyfél kapcsolati karakterlánc, amely megfelel a adatbázis tükrözési hasonló konfigurációs ADO.NET-vagy SQL Server natív ügyfele nem éri el:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Ügyfélcsatlakozás kapcsolatos további tudnivalókért lásd:

- [Kapcsolati karakterlánc kulcsszavak használata SQL Server natív ügyfele](https://msdn.microsoft.com/library/ms130822.aspx)
- [Csatlakozás ügyfelek tükrözési (SQL Server) munkamenet-adatbázishoz](https://technet.microsoft.com/library/ms175484.aspx)
- [A hibrid informatikai rendelkezésre állási csoport figyelő csatlakoztatása](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Rendelkezésre állási csoport hallgatók Ügyfélcsatlakozás és alkalmazás feladatátvételi (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Rendelkezésre állási csoportok kapcsolati karakterláncot az adatbázis-tükrözési használata](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>A hibrid informatikai hálózati késés

Kell telepítenie a HADR megoldás feltételezve, hogy a helyszíni hálózaton és Azure közötti hálózati magas válaszidejű ideig is lehet. Kópiák Azure való telepítésekor kell használni aszinkron jóváhagyás szinkron jóváhagyás helyett a szinkronizálás módot. Ha telepíti az adatbázis-kiszolgálók tükrözési mind a helyszíni és az Azure, a nagy teljesítményű mód használata helyett a magas biztonsági módot.

### <a name="geo-replication-support"></a>GEO-replikáció támogatása

Az Azure lemezt GEO replikációs nem támogatja az adatfájl és naplófájlt ugyanazt az adatbázis különböző lemezen tárolja. GRS replikálja a módosítások minden lemezen, egymástól függetlenül és aszinkron. Ez az eljárás a geo replikált példányát, de több lemezre geo replikált példányainak között nem egyetlen lemez írási sorrendre garantálja. Ha úgy állítja be az adatfájl és a naplófájl külön lemezen tárolására szolgáló adatbázis, katasztrófa után helyreállított lemezt tartalmazhat az adatfájl legújabb példányát, mint az SQL Server és a tranzakciók savas tulajdonságainak írási javaslatokat megjelenítő bejelentkezve töréspontok naplófájl. Ha nem rendelkezik a vezérlőt, amellyel a tárterület-fiók geo replikációs letiltása, adatok, és jelentkezzen be lévő összes fájlt az adott adatbázis ugyanazon a lemezen kell megtartani. Ha egynél több lemez miatt az adatbázis mérete kell használnia, katasztrófa helyreállítási adatok redundancia biztosítása érdekében a fent felsorolt megoldásokkal telepítendő szüksége.

## <a name="next-steps"></a>Következő lépések

Ha kell az SQL Server-Azure virtuális gép létrehozásához, olvassa el a [virtuális gép SQL Server Azure a kiépítési](virtual-machines-windows-portal-sql-server-provision.md)című témakört.

A legjobb teljesítmény elérése érdekében az SQL Server-Azure virtuális futó című témakörben kaphat az útmutató [Teljesítmény gyakorlati tanácsok az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-performance.md).

Futó SQL Server Azure VMs kapcsolódó témakörök [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)talál.

### <a name="other-resources"></a>Egyéb források

- [Egy új Active Directory-erdőt telepítse az Azure-ban](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Rendelkezésre állási csoportok az Azure virtuális mindig WSFC fürthöz létrehozása](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
