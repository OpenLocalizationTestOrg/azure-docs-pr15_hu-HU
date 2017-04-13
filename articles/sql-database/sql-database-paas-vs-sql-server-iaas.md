<properties
    pageTitle="SQL (PaaS) adatbázis összehasonlítása SQL Server (IaaS) VMs meg a felhőben |} Microsoft Azure"
    description="Ismerje meg, hogy melyik felhő SQL Server lehetőséget az alkalmazás illeszkedik: (PaaS) Azure SQL-adatbázis vagy a felhőben a Azure virtuális gépeken futó SQL Server."
    services="sql-database, virtual-machines"
    keywords="Az SQL Server felhőben, a felhőben PaaS adatbázis, SQL Server cloud Az SQL Server, DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Válassza az SQL Server lehetőség felhő: (PaaS) Azure SQL-adatbázis vagy SQL Server Azure VMs (IaaS)

Azure van a Microsoft Azure SQL Server-munkaterhelésekből szolgáltatója két lehetőség közül választhat:

* [Azure SQL-adatbázis](https://azure.microsoft.com/services/sql-database/): a felhőben, más néven egy szolgáltatást (PaaS) adatbázis vagy egy adatbázis szolgáltatásként (DBaaS) szoftverek,-a-szolgáltatási (szoftver) alkalmazások fejlesztéséhez optimalizált platform natív egy SQL-adatbázis. A legtöbb SQL Server-funkció kompatibilitásának is kínál. PaaS kapcsolatos további tudnivalókért olvassa el a [Mi az PaaS](https://azure.microsoft.com/overview/what-is-paas/)című témakört.
* [A Azure virtuális gépeken futó SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/): az SQL Server telepítése, és a a Windows Server virtuális gépeken futó (VMs) Azure, más néven szolgáltatásként (IaaS) infrastruktúra futó a felhőben.
Azure virtuális gépeken futó SQL Server áttelepítése meglévő SQL Server-alkalmazások van optimalizálva. A verzió és az SQL Server kiadásainak érhetők el. SQL Server, lehetővé téve a szolgáltató szükséges és végrehajtó határokon adatbázis-tranzakcióként annyi adatbázisok 100 %-os kompatibilitásának is kínál. Az SQL Server és a Windows teljes hozzáférés is kínál.

Ismerje meg, az egyes beállítások a Microsoft adatok platform tágabb, és kérjen segítséget a megfelelő üzleti igényeinek megfelelő lehetőséget. Akkor fontossági költség megtakarítási vagy előtt, hogy milyen egyéb minimális felügyeleti, hogy ez a cikk is segít eldönteni, hogy melyik módszert biztosítja az Önt érdeklő legtöbb üzleti követelmények szemben.


## <a name="microsofts-data-platform"></a>A Microsoft adatok platform

Az első dolgot megérteni a hozzászólásokat a helyszíni SQL Server-adatbázisok és Azure egyik felhasználhatja azt minden. A Microsoft adatok platform SQL Server technológiát használja, és számára elérhetővé teszi azt fizikai helyszíni gépek, személyes felhőalapú környezetben, külső tárolt magánjellegű felhőalapú környezetben és nyilvános felhő keresztül. SQL Server Azure virtuális mchines lehetővé teszi, hogy egyedi és különböző vállalati igényeknek megfelelően keresztül támogatása a következő környezetekben ugyanazok a kiszolgálói termékekkel, fejlesztőeszközök és tapasztalatok megosztására használatakor a helyszíni és felhőbeli által üzemeltetett telepítésekről, kombinációi.

   ![Az SQL Server-beállítások a felhő: SQL server IaaS vagy szoftver SQL-adatbázis a felhőben.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

A diagram naplójában egyes felületek is kell jellemző van (az X tengelyen) a infrastruktúra fölé felügyeleti szintjét, és a költség hatékonyság megvalósítani adatbázis szintű összesítést telepített és automatizálási (a az Y tengely) mértékét.

Az alkalmazások tervezésekor az alkalmazás az SQL Server részének elhelyezésére négy alapvető beállítások érhetők el:

- Nem virtualizálva fizikai gépeken futó SQL Server
- A helyszíni SQL Server virtualizálva gépek (személyes felhő)
- Az SQL Server az Azure virtuális gép (nyilvános Microsoft cloud)
- Azure SQL-adatbázis (Microsoft nyilvános felhő)

Az alábbi szakaszok tudnivalók: az SQL Server a Microsoft nyilvános felhőben: Azure SQL-adatbázissal, és az SQL Server Azure VMs. Ezeken kívül feltárása a gyakori üzleti motivators meghatározásához, hogy melyik lehetőséget az alkalmazás legjobban.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure SQL-adatbázissal, és az SQL Server Azure VMs részletesebben

**Azure SQL-adatbázis** egy relációs adatbázis-mint-a-(DBaaS) tárolt szolgáltatás *Szoftverek,-a-szolgáltatási (szoftver)* és a *Platform-mint-a-szolgáltatás (PaaS)*üzleti kategóriába esik Azure felhőben. [SQL-adatbázis](sql-database-technical-overview.md) szabványos hardver- és a tulajdonosa, is, majd a Microsoft által fenntartott épül. SQL-adatbázissal fejleszthet közvetlenül a beépített és funkciók szolgáltatás. SQL-adatbázishoz, befizetések lehetőségeket, ha át kívánja méretezni be vagy ki a megszakítás nélkül nagyobb Power használatakor.

**Az SQL Server a Azure virtuális gépeken futó (VMs)** *Infrastruktúra-mint-a-szolgáltatás (IaaS)* üzleti kategóriába esik, és lehetővé teszi az SQL Server futtatását a felhőben virtuális gép belül. SQL-adatbázis hasonló épül szabványos hardver tulajdonosa, is, majd a Microsoft által fenntartott. A virtuális SQL Server használatakor vagy fizetési is – akkor útközben-már szerepelnek egy SQL Server képe az SQL Server-licenc, vagy egyszerűen használata egy meglévő licenc. Is egyszerűen skála felfelé/lefelé és szünet/önéletrajz a virtuális szükség szerint.

Az általános két SQL beállításokkal különböző célokra optimalizált:

- **SQL-adatbázis** általános költségek csökkentése a lehető legkisebb kiépítési és kezeléséről a sok adatbázis van optimalizálva. Csökkenti a mindennapi felügyeleti költségek, mert nem kell minden virtuális gépeken futó, az operációs rendszer vagy az adatbázis-szoftver kezelése. Nem rendelkezik frissítése, nagy elérhetősége vagy [biztonsági](sql-database-automated-backups.md)kezeléséhez. Az általános Azure SQL-adatbázis lényegesen növelheti a számot az adatbázisok kezelése egyetlen informatikai vagy fejlesztési erőforrás.
- **Az SQL Server Azure VMs futó** tárolására van optimalizálva áttelepítése Azure meglévő alkalmazást, és a hibrid környezetekben a felhőbe meglévő helyszíni alkalmazások bővítése Ezeken kívül is használhatja az SQL Server egy virtuális gépen fejlesztése, és tesztelje a hagyományos SQL Server-alkalmazásokat. Az SQL Server Azure VMs jogok illetik meg Önt teljes rendszergazdai dedikált SQL Server-példányt, és egy felhőalapú virtuális fölé. Akkor a tökéletes választani, amikor a szervezetnek már van informatikai erőforrások érhető el a virtuális gépeken futó megőrzéséhez. Ezek a funkciók lehetővé teszi az alkalmazás adott teljesítményét és elérhetőség követelmények megoldására erősen testre szabott rendszer generál.

Az alábbi táblázat összefoglalja az SQL-adatbázis és az SQL Server Azure VMs a fő jellemzői:

|       | SQL-adatbázis | Az SQL Server-Azure virtuális gépen|
| -------------- | ------------ | ---------------------- |
| **Legjobb:** | Új felhő készült alkalmazások, amelyek a fejlesztés és marketing az időkorlátok. |A minimális módosításokkal felhőbe gyors áttelepítési igénylő meglévő alkalmazásokat. Gyors fejlesztés és próba forgatókönyvek, ha nem szeretné, hogy vásárlása helyszíni SQL Server-hardver tesztkörnyezetben. |
|| A csoportok, amely beépített magas elérhetőségét, vészhelyreállítás és frissítésére van szüksége az adatbázis. |A csoportok, amelyek képesek beállítása és kezelése a magas elérhetősége vészhelyreállítás és javítása az SQL Server. Egyes közöljük automatikus szolgáltatások jelentősen egyszerűsítik meg. |
||A csoportok, amelyet nem szeretne az alapul szolgáló operációs rendszer és a konfigurációs beállítások kezelése.| Ha a testre szabott környezet teljes rendszergazdai jogosultságokkal kell.|
||Adatbázisok legfeljebb 1 TB, illetve lehet, [vízszintesen vagy függőlegesen particionálva](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) méretezési mintázattal összeállítására.|Az SQL Server példányai legfeljebb 64 TB a tárhely. A példány támogatniuk tetszőleges számú adatbázist. |
||Az [épület szoftver-mint-a-(szoftver) szolgáltatásalkalmazások](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Áttelepítése és a vállalati és hibrid alkalmazások létrehozásába.|
|||||
|**Erőforrások:**|Nem kíván konfigurálása és kezelése az alapul szolgáló infrastruktúra informatikai anyagok alkalmazhat, de kívánja az alkalmazási réteg összpontosíthat.|Ha bizonyos informatikai erőforrások konfigurálása és kezelése. Egyes közöljük automatikus szolgáltatások jelentősen egyszerűsítik meg.|
|**Tulajdonjogot teljes költsége:**|Megszünteti a hardver költségek és a felügyeleti költségeket.|Megszünteti a hardver költségeket.|
|**Üzleti folytonosságot:**|Nemcsak a beépített hibafa hibatűrést infrastruktúra funkciók Azure SQL-adatbázis biztosít a funkciókat, például [az automatikus biztonsági mentést](sql-database-automated-backups.md), [Pont és az idő](sql-database-recovery-using-backups.md#point-in-time-restore), [Geo-visszaállítási](sql-database-recovery-using-backups.md#geo-restore), és [Aktív Geo replikációs](sql-database-geo-replication-overview.md) üzemkészségének növeléséhez. További információ az [SQL-adatbázis üzleti folytonosságot áttekintése](sql-database-business-continuity.md)című témakörben találhat.|SQL Server Azure VMs lehetővé teszi a magas rendelkezésre állásának és katasztrófa helyreállítási megoldást az igényeinek, az adatbázis beállítása. Ezért a rendszer erősen optimalizált is az alkalmazás. Tesztelje, és úgy, hogy saját maga szükség esetén futtassa a feladatátadás. További tudnivalókért olvassa el a [magas rendelkezésre állásának és a helyreállítás a Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)című témakört.|
|**Hibrid felhő:**|A helyszíni alkalmazás Azure SQL-adatbázis adatainak érheti el.|Az SQL Server Azure VMs beállíthatja, hogy futtassa a részben a felhőben, és a részben a helyszíni alkalmazásokat. Ha például bővítheti a helyszíni hálózaton és az Active Directory tartományi a felhőbe [Azure virtuális hálózaton](../virtual-network/virtual-networks-overview.md)keresztül. Ezeken kívül tárolhat adatfájlok helyszíni [SQL Server Azure-ban adatfájlok](http://msdn.microsoft.com/library/dn385720.aspx)használatával Azure-tárolóban lévő. További információ című témakörben [az SQL Server 2014-es hibrid felhő](http://msdn.microsoft.com/library/dn606154.aspx).|
||[Az SQL Server tranzakció alapú replikációs](https://msdn.microsoft.com/library/mt589530.aspx) előfizető való replikáció adatokat támogatja.|Teljesen támogatja [az SQL Server tranzakció alapú replikációs](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn rendelkezésre állási csoportok](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), integrációs szolgáltatások és a napló szállítási való replikáció adatokat. Hagyományos SQL Server biztonsági másolatok teljesen támogatott is|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Üzleti összefüggések az Azure SQL-adatbázis vagy SQL Server Azure VMs választása

### <a name="cost"></a>Költség

Hogy Ön egy indítási, amely akkor strapped pénzbeli vagy a csoport egy elfogadott vállalatánál szoros tervezett kényszerek alapján működik, korlátozott támogatás legtöbbször az elsődleges illesztőprogram az adatbázisok üzemeltetése kiválasztásakor. Ebben a részben tudnivalók: a számlázást és az Azure-ban két relációs adatbázisból a beállításokkal kapcsolatos alapvető műveletei licencelési: SQL-adatbázishoz, és az SQL Server Azure VMs. Akkor is újraszámolása az összes alkalmazás költség.

#### <a name="billing-and-licensing-basics"></a>Számlázás és licencelési alapjai

**SQL-adatbázis** szolgáltatásként, licenccel nem rendelkező felhasználók értékesítik.  [SQL Server Azure VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) tartalmazza, amelyek fizet licenccel rendelkező értékesítik perc. Ha egy meglévő licenc, akkor ezt is használhatja.  

**SQL-adatbázis** jelenleg több szolgáltatási rétegek, ami vannak számlát kapni óránként a szolgáltatási réteg alapján rögzített sebessége és kiválaszthatja, hogy teljesítményszint érhető el. Ezeken kívül meg vannak számlát kapni a kimenő internetes forgalmat a normál [adatátviteli sebességet](https://azure.microsoft.com/pricing/details/data-transfers/). A Basic, normál és Premium szolgáltatási rétegek célja, hogy az alkalmazás csúcs igényeknek megfelelően alakíthatja, többszintű teljesítmény kiszámíthatóan teljesítményt nyújtsa. Szolgáltatási rétegek között teljesítményszint az alkalmazás változatos átviteli igényeinek megfelelően módosíthatja. Ha az adatbázist tranzakció alapú nagy és a támogatási sok egyidejű felhasználó igényeinek, a támogatási szolgáltatás réteg javasoljuk. Az aktuális támogatott szolgáltatási rétegek a legfrissebb információkért lásd: [Azure SQL-adatbázis szolgáltatási rétegek](sql-database-service-tiers.md). Erőforrások megosztása a teljesítmény között adatbázis-példányok [Rugalmas adatbázis készletek](sql-database-elastic-pool.md) is létrehozhat.

**SQL-adatbázissal**az adatbázis-szoftver automatikusan beállítva, javítással, és frissítette a Microsoft, amely csökkenti a felügyeleti költségek. Ezeken kívül [beépített biztonsági](sql-database-automated-backups.md) funkciói segítséget elérése a jelentős költséget takaríthat meg, különösen akkor, ha az adatbázisok sok van.

**SQL Server Azure VMs**végezze el a platform által biztosított SQL Server képek (amely tartalmazza a licencet), vagy jelenítse meg az SQL Server-licenc. Támogatott SQL Server verziók (2008R2, 2012, 2014, 2016) és (fejlesztőeszközök, Express, webes, szabványos, vállalati) kiadásai érhetők el. Ezeken kívül Előrehozás-a-saját-licenc verziói (BYOL) a képek állnak rendelkezésre. Amennyiben az Azure használata képek, a működési költségeket függ, hogy virtuális méretét, és kiválaszthatja, hogy az SQL Server kiadását. Virtuális méretének vagy SQL Server edition, függetlenül fizet perc licencelési költségét SQL Server és a Windows Server együtt a virtuális lemezre Azure tárolási költségét. A perc számlázási beállítás lehetővé teszi az SQL Server használata, feltéve, hogy szüksége van vásárlási hozzáadása az SQL Server nélkül licencek. Ha a saját SQL Server-licenc előtérbe Azure, az előfizetést terhelő Windows Server és a csak a tárolási költségeket. Előbbre-a-saját engedélyezéséről további tudnivalókért lásd: [Licenc mobilitás Azure a frissítési garancia keresztül](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Az alkalmazás teljes költség kiszámítása

Indításakor felhő platformot használja, a alkalmazást futtató költségét tartalmazza a fejlesztés és felügyeleti költségeket, valamint a nyilvános felhő platform szolgáltatás költségeket.

Az alábbiakban az alkalmazást az SQL-adatbázis és az SQL Server Azure VMs részletes költség kiszámítása:

**Azure SQL-adatbázis használata:**

*Teljes költség alkalmazás = nagyon kis méretre állított felügyeleti költségek + szoftver fejlesztési költségek + SQL-adatbázis szolgáltatás költségek*

**Ha az SQL Server Azure VMs:**

*Teljes költség alkalmazás = nagyon kis méretre állított szoftver fejlesztési költség + felügyeleti költségek + SQL Server és a Windows Server licencelési költségek + Azure tárolási költségek*

További információt a árak az alábbi forrásokban talál:

- [SQL-adatbázis árak](https://azure.microsoft.com/pricing/details/sql-database/)
- A [virtuális gép árak](https://azure.microsoft.com/pricing/details/virtual-machines/) , az [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) - és a [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Számológép árak Azure](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Van egy kis részét SQL Server funkciók, amelyek nem alkalmazható vagy SQL-adatbázis számára nem érhető el. További információt talál [az SQL-adatbázis általános korlátai és irányelveket talál](sql-database-general-limitations.md) és [SQL-adatbázis-Transact-SQL információkat](sql-database-transact-sql-information.md) . Ha az SQL Server meglévő megoldás áthelyezni a felhőbe, olvassa el a [Azure SQL-adatbázishoz SQL Server-adatbázis áttelepítése](sql-database-cloud-migrate.md)című témakört. Egy meglévő helyszíni SQL Server alkalmazást SQL-adatbázis áttelepítése, vegye figyelembe a cloud services ajánlat funkcióját kihasználhatja az alkalmazás frissítése. Például akkor fontolja meg [Azure Web App szolgáltatás](https://azure.microsoft.com/services/app-service/web/) vagy a [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) segítségével növelheti a költség előnyökkel jár a alkalmazási réteg tárolni.

### <a name="administration"></a>Felügyelete

Sok verzióhoz egy felhőalapú szolgáltatásba való áttérés döntés megegyezik sokkal kapcsolatban, mert formában felügyeleti összetettsége a költség. **SQL-adatbázissal**a Microsoft felügyeli az alapul szolgáló hardver. Microsoft automatikusan replikálja magas rendelkezésre állás minden adat, állítja be és frissíti az adatbázis-szoftver, kezeli a terheléselosztás és áttetsző feladatátvevő jelent, a kiszolgáló hiba esetén. Továbbra is az adatbázis felügyelete, de már nincs szüksége az adatbázis-vezérlő, a kiszolgálói operációs rendszer vagy a hardver kezelése.  Elemek továbbra is felügyelete többek között az adatbázisok és bejelentkezések, index és a lekérdezés beállítása, és naplózás és biztonsági.

Az **SQL Server Azure VMs**Ha az operációs rendszer és az SQL Server-példányt konfigurációs teljes hozzáféréssel. A virtuális gép van, hogy mikor frissítés/frissítés az operációs rendszer és az adatbázis-szoftver és mikor további rendszerekben, például a víruskereső szoftver telepítése döntse el. Néhány automatikus funkció jelentősen kibővíti a javítási biztonsági és magas elérhetősége egyszerűsítése érdekében találhatók. Ezeken kívül szabályozhatja, hogy a virtuális, a lemezt a szám és a tárhely konfigurációk méretét. Azure lehetővé teszi, hogy szükség szerint módosítsa a virtuális méretét. Információ a [virtuális gép és felhőalapú szolgáltatást méretű az Azure](../virtual-machines/virtual-machines-linux-sizes.md)témakörben talál. 

### <a name="service-level-agreement-sla"></a>Szolgáltatásiszint-szerződés (SLA)

Sok informatikai részlegeknek készült funkcióiról felfelé idejű kötelezettségek, egy szolgáltatási szerződés SZOLGÁLTATÁSISZINT-értekezlet fontosságú felső. Ebben a részben megnézi a mi SLA vonatkozik adatbázisonként szolgáltatója lehetőséget.

A Microsoft **SQL-adatbázis** Basic, normál és Premium szolgáltatási rétegek biztosít az elérhetőség SLA 99.99 %-át. A legfrissebb információkat olvassa el a [Szolgáltatási szint szerződés](https://azure.microsoft.com/support/legal/sla/sql-database/)című témakört. SQL-adatbázis szolgáltatási rétegek és a támogatott üzleti folytonosságot tervek a legfrissebb információkért olvassa el a [Szolgáltatási rétegek](sql-database-service-tiers.md)című témakört.

A Microsoft **SQL Server Azure VMs futó**, biztosít az elérhetőség SLA csak a virtuális gép eltakaró 99.95 %-os. E szolgáltatásiszint-szerződés nem foglalkozik (például SQL Server) a virtuális futó folyamatok és igényel, hogy Ön üzemelteti legalább két virtuális példányokat egy elérhetőségének beállítása. A legfrissebb információkért lásd: a [Virtuális SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Az adatbázis magas elérhetőség (HA) VMs belül be kell állítania a támogatott magas elérhetőségi beállítások közül az SQL Server, például [AlwaysOn rendelkezésre állási csoportok](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Egy támogatott magas elérhetősége beállítást nem nyújt további szolgáltatásiszint-szerződés, de lehetővé teszi, hogy elérése > 99.99 % adatbázis elérhetőségét.

### <a name="market"></a>Piaci idő

**SQL-adatbázis** a felhőben készült alkalmazások jobb megoldás akkor, ha Fejlesztőeszközök hatékonyság és az idő piaci gyors kritikus. Programozott kereskedelmi hasonló funkciókat, a tökéletes felhő fejlesztője és a fejlesztők számára, az alapul szolgáló operációs rendszer és az adatbázis kezeléséhez szükséges a csökkenti a. Ha például használhatja a [REST API -val](http://msdn.microsoft.com/library/azure/dn505719.aspx) és a [PowerShell-parancsmagok](http://msdn.microsoft.com/library/azure/dn546726.aspx) automatizálhatja és felügyeleti műveleteket az ezreselválasztó az adatbázisok kezelése. Szolgáltatások, például [Rugalmas adatbázis-készletek](sql-database-elastic-pool.md) koncentráljon az alkalmazási réteg, és a megoldás kézbesítése piaci gyorsabb teszi lehetővé.

**Az SQL Server Azure VMs futó** tökéletes, ha a meglévő vagy új alkalmazás igényel nagy adatbázisok, egymáshoz adatbázisokhoz vagy SQL Server vagy a Windows valamennyi szolgáltatását. Érdemes emellett közelítését Ha meg szeretné áttelepíteni meglévő helyszíni alkalmazások és Azure, mint az adatbázisok-van. Mivel az nem kell a bemutatót, az alkalmazást és az adatok rétegek módosítása, menthet, időt és a tervezett rearchitecting a meglévő megoldás a. Ehelyett összpontosíthat áttelepítése az összes megoldás Azure és néhány teljesítmény optimalizálásokat, amely a Azure platform szükség lehet elvégezni. További tudnivalókért olvassa el a [Teljesítményét ajánlott eljárások a Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Összefoglalás

Ez a cikk SQL-adatbázis és az SQL Server megismerkedett a Azure virtuális gépeken futó (VMs), és tárgyalt gyakori üzleti motivators, amely az Ön döntése hatással lehet. Az alábbiakban a bemutatókkal javaslatok összefoglalása:

Válassza az **Azure SQL-adatbázis** , ha:

- Új, felhőalapú alkalmazások kihasználhatja a költség megtakarítási készítésekor, és adja meg, hogy a felhőszolgáltatásokba optimalizálása. Ezt a megközelítést egy teljes körű felügyelt felhőalapú szolgáltatás előnyei a segítséget nyújt az alsó első alkalommal piaci és biztosíthat a hosszú távú költség-optimalizálási.

- Az adatbázisok közös adatkezelési műveletek hajthatók végre, és erősebb elérhetősége SLA adatbázisok van szükség Microsoft szeretné használni.



Válassza az **SQL Server Azure VMs** , ha:

- Van meglévő helyszíni alkalmazások szeretné áttelepíteni, vagy kiterjesztése a felhőben, vagy ha nagyobb, mint 1 TB vállalati alkalmazások szeretne. Ezt a módszert nyújt a 100 %-os SQL-kompatibilis, nagy adatbázis beosztását, SQL Server és a Windows teljes hozzáféréssel előnyeit, és a helyszíni tunneling biztonságos. Ezt a megközelítést kis méretűre állítása fejlesztés és módosítása meglévő alkalmazások költségeket.

- Meglévő informatikai erőforrások vannak, és végül saját javítása, biztonsági mentés és adatbázis magas elérhetőségét. Figyelje meg, hogy az automatikus funkciókkal jelentősen egyszerűsítése ezeket a műveleteket. 


## <a name="next-steps"></a>Következő lépések
- Lásd: [SQL-adatbázis oktatóprogram: SQL-adatbázis létrehozása az Azure portálon percben](sql-database-get-started.md) a SQL-adatbázis használata.
- Lásd:, [az SQL-adatbázis ár] (https://azure.microsoft.com/pricing/details/sql-database/).
- Lásd: [egy SQL Server Azure virtuális gép rendelkezni](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) az első lépések az SQL Server Azure VMs.
- Lásd: [SQL Server Azure virtuális-gépen: tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
