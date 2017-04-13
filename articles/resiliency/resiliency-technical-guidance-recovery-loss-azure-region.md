<properties
   pageTitle="Az Azure régió technikai útmutató elvesztését a helyreállítás tűrőképessége |} Microsoft Azure"
   description="Szóló ismertetése és rugalmas, könnyen hozzáférhető, hibafa alternatív alkalmazások tervezése, valamint vészhelyreállítás megtervezése"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Azure tűrőképessége technikai útmutató: olyan terület szintű a szolgáltatási zavarok helyreállítása

Azure oszlik fizikailag és logikus régiók néven erőforrás-mennyiség. Egy területet, ahol a egy vagy több adatközpontokkal közelében. Az írás időben Azure 24 régiók tartalmaz a világon.

Ritka körülmények között nem akkor lehet, hogy teljes területen létesítményekhez előfordulhat, hogy nem érhető el, ha például a hálózati hibák miatt. Vagy létesítményekhez elveszhetnek teljesen, például a természeti katasztrófa miatt. Ebből a szakaszból a Azure funkcióinak területek között elosztott alkalmazások készítéséhez. Az ilyen eloszlás segít csökkentése érdekében, hogy egy régióban hiba befolyásolhatja a többi régió lehetőséget.

##<a name="cloud-services"></a>Felhőszolgáltatások

###<a name="resource-management"></a>Erőforrás-kezelés

Különböző régiók számítási példányokat egy külön felhőszolgáltatásba létrehozása a célhely területenként, és ezután közzététele a telepítőcsomag minden felhőszolgáltatásba oszthatja meg. Megjegyzendő, hogy forgalom terjesztése felhőszolgáltatások a különböző területei között végre kell hajtani az alkalmazás fejlesztője által vagy a forgalom alkalmazáskezelési szolgáltatás.

Tartalék szerepkör-példányok vészhelyreállítás előre üzembe számának meghatározása kapacitás tervezés fontos eleme. Egy teljes körű másodlagos telepítési problémát biztosítja, hogy a kapacitás már érhető el, ha szükséges; azonban ez hatékony megduplázódik a költség. Közös mintát, hogy egy kis, másodlagos telepítés, elég nagy kritikus szolgáltatások futtatására. A kis másodlagos telepítési jó ötlet, mind a teljesítmény és a másodlagos környezet konfigurációja teszteléshez foglalni.

>[AZURE.NOTE]Az előfizetés kvóta nem kapacitás garancia. Kvóta egyszerűen hitelképesség korlátozás. Zökkenőmentes beosztását, a szükséges számú szerepkörök meg kell adni a modell, és telepíteni kell a szerepköröket.

###<a name="load-balancing"></a>Terheléselosztás

Különböző régiók forgalom terheléselosztó szükséges a forgalom projektmenedzsment megoldás. Azure [Azure forgalom Manager](https://azure.microsoft.com/services/traffic-manager/)biztosít. Is készíthet hasonló forgalom-kezelési lehetőségek nyújtó külső szolgáltatások előnyeit.

###<a name="strategies"></a>Stratégiák

Sok alternatív stratégiák elosztott számítási terület keresztül végrehajtásához érhetők el. Ezek az adott üzleti követelmények és az alkalmazás körülmények között kell igazítani. Magas szintű az alábbi módszerek az alábbi kategóriákba osztható meg:

  * __Katasztrófa meg újratelepítése__: Ezzel a módszerrel a az alkalmazás van üzlethez nulláról katasztrófa idején. Ez a megfelelő garantált helyreállítási egyszerre nem igénylő nem kritikus alkalmazások.

  * __Tartalék meleg (aktív/passzív)__: egy másodlagos szolgáltatott szolgáltatás alternatív területen jön létre, és szerepkörök telepítik zökkenőmentes minimális kapacitás; a szerepkörök azonban nem kapott gyártási forgalmat. Ez a módszer akkor lehet hasznos, nem készített forgalom elosztása régiók alkalmazásokhoz.

  * __Tartalék meleg (aktív/aktív)__: az alkalmazás fogadásához gyártási betöltése több régióban lett tervezve. Vészhelyreállításra szükséges-nál magasabb kapacitásának a felhőszolgáltatások az egyes régiókra előfordulhat, hogy be kell állítani. Másik lehetőségként a felhőszolgáltatások méretezni előfordulhat, hogy szükség szerint egy katasztrófa és feladatátvételi idején meg. Ez a módszer igényli jelentős befektetés alkalmazás Tervező, de van jelentős előnyökkel jár. Ezek közé tartozik az alsó és garantált helyreállítási idő, folytonos vizsgálata összes helyreállítási helyek és kapacitás hatékony használatát.

Elosztott tervezett teljes vitafórum ehhez a dokumentumhoz hatókörén kívüli van. További tudnivalókért lásd [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetőségét](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Virtuális gépeken futó

Helyreállítási az infrastruktúra szolgáltatást (IaaS) virtuális gépeken futó (VMs) hasonlít a platform, mint (PaaS) szolgáltatás több szempontból helyreállítási számítja ki. Fontos különbségek vannak, akkor jó helyen jár, annak oka, hogy egy IaaS virtuális áll a virtuális, mind a virtuális lemez.

  * __Használata Azure biztonsági másolat létrehozása, amelyek egységes alkalmazás közötti terület biztonsági mentést__.
  [Azure biztonsági másolat](https://azure.microsoft.com/services/backup/) segítségével a felhasználók alkalmazás egységes biztonsági másolatok létrehozása több virtuális lemezre, ez utóbbiak pedig a biztonsági másolatok replikációs területek között. Művelet kiválasztása geo-replikáció a biztonsági másolat tárolóból elemre a létrehozási idején. Figyelje meg, hogy a replikáció a biztonsági másolat kell beállítania a létrehozási idején tárolóból elemre. Azt nem állíthatók be később. Terület elvész, ha a Microsoft fog elérhetővé teheti a biztonsági másolatok ügyfeleknek. Ügyfelek tudják saját konfigurált visszaállítási pontok visszaállításához.

  * __Az adatok lemez az operációs rendszer lemezről külön__. Fontos szempont IaaS VMs, hogy az operációs rendszer lemezen nem módosítható a virtuális újbóli létrehozása nélkül. Ez nem probléma esetén a helyreállítási stratégia katasztrófa követő telepítsen újra. Jó helyen jár akkor lehetséges probléma alkalmazás használatakor a meleg tartalék megközelítés kapacitás foglalni. Ennek megfelelően alkalmazza, a megfelelő operációs rendszer lemez rendszerbe az elsődleges és másodlagos helyekre kell rendelkeznie, és az alkalmazás adatokat egy külön meghajtón kell tárolni. Ha lehetséges használja a szabványos operációs rendszer konfiguráció, amely a két helyen is biztosítható. Után feladatátvevő majd az adatok meghajtó kell kapcsolni a a meglévő IaaS VMs a a másodlagos Adatközpont. Segítségével AzCopy elérhető információk, amelyek az adatok lemez(ek) távoli webhelyre.

  * __Ügyeljen a potenciális konzisztencia problémák több virtuális lemezre geo feladatátvétel után__. Virtuális lemez tároló Azure BLOB megvalósítása, és az ugyanazt a replikáció geo jellemző van. [Azure biztonsági másolat](https://azure.microsoft.com/services/backup/) használja, hacsak vannak konzisztencia semmilyen garanciát lemezre, mert geo replikációs aszinkron, és önállóan replikálja. Egyes virtuális lemezt vannak garantált e az összeomlást egységes az állapot geo átváltó után, de nem következetes lemezre. Ez problémákat okozhat bizonyos esetekben (például az esetében lemezcsíkozás).

##<a name="storage"></a>Tárhely

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Helyreállítási blob, tábla, várólista és virtuális lemezterület Geo felesleges tárolására használatával

Azure, BLOB, táblák, sorok és virtuális lemezt az összes geo replikált alapértelmezés szerint. Ez hivatkozik, amely szerint Geo felesleges tároló (GRS). GRS tároló adatait egy el egy adott földrajzi régióban mérföld párosított adatközponthoz több száz replikálja. Adja meg a további tartóssági abban az esetben, ha nem a fő adatközponthoz katasztrófa GRS szolgál. Amikor feladatátadás Microsoft vezérlők és feladatátvételi korlátozódik, amelyben az eredeti elsődleges helye tekintendő helyrehozhatatlan lehetővé teszi időtartama a fő katasztrófák. Bizonyos esetekben ez lehet akár több napig. Bár a szinkronizálás intervallum nem még hatálya alá egy szolgáltatásiszint-szerződés adatok általában replikált néhány percen belül.

A geo feladatátvétel megelőzve-e ugyanakkora hogyan érhető el a fiók (az URL-cím és a fiók kulcs nem változik). A tárterület-fiókot, azonban lesz egy másik régióbeli feladatátvétel után. Ez hatással lehetnek a tárterület-fiókkal területi affinitás igénylő alkalmazásokat. Még a szolgáltatások és alkalmazások, amelyekhez nincs szükség a tárterület-fiók az azonos adatközponthoz határokon-adatközponthoz időtartama és sávszélesség díj az lehet, hogy kell lenyűgöző forgalom áthelyezése a feladatátvevő régió ideiglenes okok. Ez a sikerült tényező általános katasztrófa helyreállítási stratégia be.

Nemcsak a automatikusan átveszi GRS által biztosított Azure vezetett szolgáltatás, amely olvasási hozzáférést biztosít az adatok, a másodlagos tárolóhelyen példányát. Link-olvasásra Geo felesleges tároló (TS-GRS).

GRS és a TS-GRS tárolása kapcsolatos további tudnivalókért olvassa el a [Azure tároló replikációs](../storage/storage-redundancy.md)című témakört.

###<a name="geo-replication-region-mappings"></a>A replikáció GEO régió hozzárendeléseket:

Fontos tudnivalók a hol található az adatok geo replikált, a érdekében, hogy hol szeretné telepíteni az adatok a adathordozós területi affinitás igénylő másik példányát. A következő táblázat mutatja az elsődleges és másodlagos helye párosítása:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Árak GEO-replikáció:

A replikáció GEO Azure tároló aktuális árak szerepel. Ezt hívják Geo felesleges tároló (GRS). Ha nem szeretné, hogy az adatok geo replikált geo replikációs letilthatja a fiókjához. Azaz helyileg felesleges tárhely, és azt alapdíjával díjazott GRS összehasonlítva kedvezményes ár.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Annak megállapítása, ha egy geo-történt

Geo átváltó fordul elő, ha ez az [Azure állapotjelző irányítópultja](https://azure.microsoft.com/status/)fog közzéteendő. Alkalmazások segítségével miként állíthat észlelésére, azonban a tárterület-fiókjukkal geo régió megfigyelésével egy automatizált eszközöket. Ez a más helyreállítási műveletek, például számítási erőforrások, ahol átkerül a tárhely geo tartományban lévő aktiválás indításához használható. Elvégezhető lekérdezés ahhoz, hogy ez a Szolgáltatáskezelés API, [Tároló fiók tulajdonságok beolvasása](https://msdn.microsoft.com/library/ee460802.aspx)használatával. A megfelelő tulajdonságokat a következők:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>Virtuális lemez és geo átváltó

A szakasz tárgyalt virtuális lemezen, garanciát nem jelentenek adatok konzisztencia virtuális lemezre feladatátvevő után. Ahhoz, hogy a biztonsági másolatok helyességét, például az adatok védelme Manager biztonsági termék használandó kevésbé részletes adatok és visszaállításához alkalmazást.

##<a name="database"></a>Adatbázis

###<a name="sql-database"></a>SQL-adatbázis

Azure SQL-adatbázis kétféle helyreállítási biztosít: Geo-visszaállítás és az aktív Geo-replikáció.

####<a name="geo-restore"></a>GEO-visszaállítása

A Basic, normál és Premium adatbázisok [GEO-visszaállítási](../sql-database/sql-database-recovery-using-backups.md#geo-restore) is érhető el. Ha az adatbázis nem érhető el a hol helyezkedik el az adatbázis régióban esemény miatt biztosít az alapértelmezett helyreállítási beállítást. Pont és az idő visszaállítása hasonlóan a Geo-visszaállítási adatbázis biztonsági másolatok geo felesleges Azure tároló támaszkodik. Visszaállítása biztonsági másolatból a geo replikált, és ezért szeretne a tárhely kimaradások elsődleges régióban rugalmassá. További részletekért olvassa el [az Azure SQL-adatbázis vagy egy másodlagos áttérni visszaállítása](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Aktív Geo replikációs

Az összes adatbázis-meghatározási [Aktív Geo replikációs](../sql-database/sql-database-geo-replication-overview.md) érhető el. Több olyan szigorú helyreállítási követelmények Geo-visszaállítási is kínálhatnak, mint rendelkező alkalmazások készült. Aktív Geo replikációs használ, a különböző régiók kiszolgálókon négy olvasható formátumú másodlagos zónák hozhat létre. A formátumú másodlagos zónák közül való áttérés kezdeményezhet. Aktív Geo replikációs ezenkívül támogatja az alkalmazás frissítése vagy áthelyezés jelenik meg, valamint a terheléselosztás csak olvasható munkaterhelésekből használható. A részletekért lásd: [a replikáció Geo konfigurálása](../sql-database/sql-database-geo-replication-portal.md) és [a másodlagos adatbázishoz](../sql-database/sql-database-geo-replication-failover-portal.md)átadni. Olvassa el [egy alkalmazást a felhőalapú vészhelyreállítás tervezés aktív Geo replikációs SQL-adatbázisban](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) és a [felhő alkalmazások kezelése közbeni használatával SQL adatbázis aktív Geo replikációs](../sql-database/sql-database-manage-application-rolling-upgrade.md) tervezés és a megvalósítása alkalmazások és az alkalmazások frissítés legrövidebb leállás nélkül olvashat.

###<a name="sql-server-on-virtual-machines"></a>A virtuális gépeken futó SQL Server

Több módon is ellenőrizhető helyreállítási és az SQL Server 2012 (vagy újabb verzió) fut az Azure virtuális gépeken futó magas elérhetősége érhetők el. További tudnivalókért olvassa el a [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)című témakört.

##<a name="other-azure-platform-services"></a>Más Azure platform szolgáltatások

Amikor megpróbál a felhőszolgáltatásában futtassa a több Azure régióban, figyelembe kell vennie a következmények az egyes a függőségeket. Az alábbi szakaszok szolgáltatásspecifikus útmutatás feltételezi, hogy kell használnia az azonos Azure szolgáltatás egy alternatív Azure adatközponthoz. Ez magában foglalja a konfigurációs és adatok replikációs feladatok is.

>[AZURE.NOTE]Egyes esetekben ezeket a lépéseket segít teljes adatközponthoz esemény, hanem egy szolgáltatásspecifikus üzemszünetek csökkentésében. Az alkalmazás szempontjából egy szolgáltatásspecifikus üzemszünetek lehet, hogy közvetlenül az korlátozása, és lenne szükség ideiglenes áttelepítése a szolgáltatást egy alternatív Azure területére.

###<a name="service-bus"></a>Szolgáltatás Bus

Azure Service Bus Azure régiók nem átterjedő egyedi névteret használja. Így az első követelmény, hogy a szükséges szolgáltatás bus névtér az alternatív régióban beállítása. Vannak azonban olyan is aszinkron üzenet tartósságára szempontjait. Vannak olyan több stratégiák üzenetek kiegészítését Azure területek között. A következő replikációs stratégiák és más Vészhelyreállítási stratégiák meghatározása a részletekért olvassa [Gyakorlati tanácsok a szolgáltatás Bus kimaradások és katasztrófák elleni szigetelő alkalmazások](../service-bus-messaging/service-bus-outages-disasters.md). Egyéb elérhetősége szempontok [Szolgáltatás Bus (elérhetőség)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services)című témakör tartalmaz.

###<a name="app-service"></a>Alkalmazás szolgáltatás

Az Azure alkalmazás szolgáltatásalkalmazás Web Apps alkalmazások és a Mobile alkalmazások, például egy másodlagos Azure régió áttelepítéséhez biztonsági másolatot a rendelkezésre álló közzétételi webhelyen kell rendelkeznie. Ha a üzemszünetek nem jár a teljes Azure adatközponthoz, esetleg FTP segítségével töltse le a legújabb biztonsági másolatot a webhely tartalmát. Majd új alkalmazás létrehozása a másodlagos régióban, kivéve, ha korábban ezt megtette kapacitás foglalni. Az új régió közzététele a webhelyen, és végezze el a szükséges konfigurációs módosításokat. Ezek a változások lehetnek a csatlakozási_karakterlánc adatbázis vagy az egyéb területhez kötött beállításokat. Ha szükséges, a webhely SSL-tanúsítvány felvétele, és módosítsa a CNAME DNS-rekord úgy, hogy az egyéni tartománynevet a redeployed Azure Web App URL-címe mutat.

###<a name="hdinsight"></a>Hdinsight szolgáltatáshoz

Az adatok HDInsight társított Azure Blob-tárolóban lévő alapértelmezés szerint vannak tárolva. HDInsight igényel, hogy a Hadoop fürtre MapReduce feladatok feldolgozása kell közös elhelyezni elemezni az adatokat tartalmazó tárterület-fiókként ugyanabban a régióban. A rendelkezésre álló Azure tárolóhoz geo replikációs szolgáltatást használja, ha az adatok, ha volt replikált az adatokat, ha valamilyen oknál fogva az elsődleges régió már nem érhető el, a másodlagos régióban érheti el. Az adatainak van már replikált és továbbra is a feldolgozás régióban hozhat létre egy új Hadoop fürt. Egyéb elérhetősége szempontok [HDInsight (elérhetőség)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services)című témakör tartalmaz.

###<a name="sql-reporting"></a>SQL-jelentés

Ekkor a egy Azure terület funkcióvesztést helyreállítása SQL Reporting több példányával, különböző Azure régiókban van szükség. Ezekben az esetekben SQL Reporting ugyanazokat az adatokat kell érhet el és adatokat kell rendelkeznie a saját helyreállítási katasztrófa esetén megtervezése. Minden alkalommal RDL-fájl külső biztonsági másolatait is meg tudja őrizni.

###<a name="media-services"></a>Media Services

Azure Media Services a különböző helyreállítási megközelítés kódolását és a folyamatos átvitelű tartalmaz. A folyamatos átvitelű általában több kritikus egy területi üzemszünetek során. Felkészülés a, Media Services fiókkal kell rendelkeznie két különböző Azure régiókban. A két régió kell elhelyezni a kódolt tartalmat. Során hiba a másik területére irányíthatja át a továbbított forgalmat. Kódolást bármely Azure régióban hajtható végre. Ha kódolást időérzékeny, például az élő esemény feldolgozása közben, kész feladatok egy alternatív adatközponthoz nyújt a hibák során kell lennie.

###<a name="virtual-network"></a>Virtuális hálózati

Konfigurációs fájl területen alternatív Azure virtuális hálózat beállítása a leggyorsabban biztosítanak. Az elsődleges Azure régió, [a virtuális hálózati beállítások exportálása](../virtual-network/virtual-networks-create-vnet-classic-portal.md) a jelenlegi hálózati hálózati konfigurációja fájlba a virtuális hálózat konfigurálása után Az elsődleges régióban üzemszünetek esetében a tárolt konfigurációs fájl [visszaállítása a virtuális hálózat](../virtual-network/virtual-networks-create-vnet-classic-portal.md) . Beállította más felhőszolgáltatások, virtuális gépeken futó vagy határokon helyszíni beállításokat, hogy az új virtuális hálózat használata.

##<a name="checklists-for-disaster-recovery"></a>Ellenőrzőlisták a vészhelyreállítás

##<a name="cloud-services-checklist"></a>Cloud Services ellenőrzőlista

  1. Tekintse át a jelen dokumentum Cloud Services szakaszát.
  2. Hozzon létre egy keresztté-régió katasztrófa helyreállítási stratégia.
  3. Ismerje meg az alternatív régióban kapacitás lefoglalására kompromisszumok.
  4. Használja a forgalom útválasztási eszközök, például az Azure forgalom Manager.

##<a name="virtual-machines-checklist"></a>Virtuális gépeken futó ellenőrzőlista

  1. Tekintse át a jelen dokumentum virtuális gépeken futó szakaszát.
  2. [Azure biztonsági másolat](https://azure.microsoft.com/services/backup/) segítségével alkalmazás egységes biztonsági másolatok létrehozása a területek között.

##<a name="storage-checklist"></a>Tároló feladatlista

  1. Tekintse át a dokumentum tárolási szakaszát.
  2. Ne tiltsa le a tároló-erőforrások geo replikációs.
  3. A replikáció geo alternatív régió megelőzve feladatátvevő megértése
  4. Hozzon létre egyéni biztonsági stratégiák a felhasználó által felügyelt feladatátvevő stratégiák.

##<a name="sql-database-checklist"></a>SQL-adatbázis ellenőrzőlista

  1. Tekintse át a jelen dokumentum SQL-adatbázis szakaszát.
  2. Szükség szerint használható [Geo-visszaállítási](../sql-database/sql-database-recovery-using-backups.md#geo-restore) vagy [Geo-replikáció](../sql-database/sql-database-geo-replication-overview.md) .

##<a name="sql-server-on-virtual-machines-checklist"></a>A feladatlista virtuális gépeken futó SQL Server

  1. Olvassa el a jelen dokumentum szakaszát virtuális gépeken futó SQL Server.
  2. Keresztté-AlwaysOn elérhetősége területcsoportok vagy az adatbázis-tükrözéshez használja.
  3. Váltakozva biztonsági mentése és a blob-tároló visszaállításához.

##<a name="service-bus-checklist"></a>Szolgáltatás Bus ellenőrzőlista

  1. Tekintse át a jelen dokumentum szolgáltatás Bus szakaszát.
  2. Állítsa be a szolgáltatás Bus névteret alternatív területen.
  3. Fontolja meg az egyéni replikációs stratégiák üzenetek területek között.

##<a name="app-service-checklist"></a>Alkalmazás szolgáltatás ellenőrzőlista

  1. Tekintse át a jelen dokumentum alkalmazás szolgáltatás szakaszát.
  2. Naplók karbantartása: az elsődleges régió kívül a webhely biztonsági másolatok.
  3. Ha üzemszünetek részleges, próbálja meg FTP az aktuális webhely beolvasásához.
  4. Tervezze meg a webhely üzembe alternatív területen az új vagy meglévő webhelyre.
  5. Tervezze meg az alkalmazás és a DNS-CNAME rekordokat a módosításokat.

##<a name="hdinsight-checklist"></a>HDInsight ellenőrzőlista

  1. Tekintse át a jelen dokumentum HDInsight szakaszát.
  2. Hozzon létre egy új Hadoop fürt a régióban replikált adatokkal.

##<a name="sql-reporting-checklist"></a>SQL-jelentés feladatlista

  1. Olvassa el a jelen dokumentum SQL Reporting szakaszát.
  2. Alternatív SQL Reporting példány egy másik régióbeli karbantartása.
  3. Naplók karbantartása: egy külön terv való replikáció a cél a régióba.

##<a name="media-services-checklist"></a>Media Services ellenőrzőlista

  1. Olvassa el ezt a dokumentumot a Media Services című szakaszát.
  2. Alternatív területen Media Services-fiók létrehozása.
  3. A két régió, a támogatási adatfolyam feladatátvevő ugyanazt a tartalmat kódolását.
  4. A szolgáltatási zavarok megelőzve egy másik területére kódolási feladatok nyújt.

##<a name="virtual-network-checklist"></a>Virtuális hálózati feladatlista

  1. Tekintse át a jelen dokumentum virtuális hálózati szakaszát.
  2. Használati exportálja az Access hozza létre azt egy másik tartományban lévő virtuális hálózati beállítások.

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a [technikai útmutató Azure tűrőképessége](./resiliency-technical-guidance.md)sorozat része. A sorozat a következő cikk [egy helyszíni adatközponthoz való Azure helyreállítása](./resiliency-technical-guidance-recovery-on-premises-azure.md)koncentrál.
