<properties
   pageTitle="A felhő üzemkészségének - adatbázis-helyreállítás - SQL-adatbázis |} Microsoft Azure"
   description="Megtudhatja, hogyan támogatja az Azure SQL-adatbázis a felhő üzemkészségének és az adatbázis-helyreállítás és segíti futó felhőalapú kulcsfontosságú alkalmazásokat."
   keywords="üzleti folytonosságot, felhőalapú üzemkészségének, adatbázis vészhelyreállítás, adatbázis helyreállítás"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Az SQL Azure-adatbázis üzemkészségének áttekintése

Az Áttekintés az Azure SQL-adatbázis nyújt a üzemkészségének és a helyreállítás funkcióját ismerteti. Beállítások, javaslatok és oktatóanyagok a zavaró eseményeket, amelyek adatvesztést vagy jelentkezését, hogy nem érhető el, és az adatbázis visszaállítása az biztosít. A vitafórum is tartalmaz, mi a teendő, ha egy felhasználó vagy alkalmazáshiba hatással van az adatok integritását, egy Azure terület tartalmaz egy üzemszünetek vagy az alkalmazás karbantartást igényel. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Üzleti folytonosságot megadására használható funkciók SQL-adatbázis

SQL-adatbázis több üzleti folytonosságot funkciói, köztük az automatikus biztonsági mentés és a választható adatbázis-replikáció biztosít. A becsült helyreállítási idő (Beszúrása) és a legutóbbi tranzakciók adatvesztést különböző tulajdonságokkal minden legyen. Ha megértette ezeket a beállításokat, is – ezek közül válasszon és, a legtöbb esetben őket együttes használata különböző felhasználási területei. Csomagja üzleti folytonosságot fejleszt, mint a maximális elfogadható időt, mielőtt az alkalmazás teljesen helyreállítása után a zavaró esemény – Ez a helyreállítási idő cél (RTO) megértéséhez szükséges. A legutóbbi adatok frissítések (időközönként) az alkalmazás elviseli legnagyobb mennyiségű megértéséhez szükséges elvesztése esetén helyreállítása után a zavaró esemény – a helyreállítási pont cél (Készletben). 

Az alábbi táblázat a három a leggyakoribb forgatókönyvet az Beszúrása és Készletben hasonlítja össze.

| Lehetőség |  Egyszerű szint | Szabványos szint  | Réteg prémium verzió |
|---|---|---|---|
| Az idő visszaállítása biztonsági másolatból pont | Bármelyik visszaállítási pont belül napján   | Bármelyik visszaállítási pont 35 napon belül  | Bármelyik visszaállítási pont 35 napon belül |
GEO-geo replikált biztonsági mentés visszaállítása | < 12h, Készletben < 1ó Beszúrása   | < 12h, Készletben < 1ó Beszúrása   | < 12h, Készletben < 1ó Beszúrása |
|Aktív Geo replikációs | < 30s, Készletben < 5s Beszúrása   | < 30s, Készletben < 5s Beszúrása | < 30s, Készletben < 5s Beszúrása |


### <a name="use-database-backups-to-recover-a-database"></a>Adatbázis visszaállítása az adatbázis biztonsági mentése használatával

SQL-adatbázis automatikusan végrehajtja a teljes adatbázis biztonsági mentése heti, megkülönböztető kombinációi óránként, és tranzakció napló biztonsági mentés öt percenként adatbázis védelmét a vállalkozás az adatok elvesztését a. A biztonsági mentése helyileg felesleges tároló 35 napig-adatbázisok a szokásos és prémium szolgáltatási rétegek, és az egyszerű szolgáltatási réteg-adatbázisok hét napig tárolja,- [szolgáltatási rétegek](sql-database-service-tiers.md) további részleteket lásd a szolgáltatási rétegek. Az adatmegőrzési időszak a szolgáltatási réteg számára nem felel meg a vállalati, ha az adatmegőrzési időszak átméretezheti [a szolgáltatási réteg](sql-database-scale-up.md)módosításával. A teljes és eltérő adatbázis biztonsági másolatok is replikált [párosított adatközpont](../best-practices-availability-paired-regions.md) egy adatok központ üzemszünetek - elleni védelem az [adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md) további részleteket lásd:.

Adatbázis helyreállítása különböző zavaró eseményeket, mind az Adatközpont belül, és egy másik adatközpontja ezeket adatbázis automatikus biztonsági mentés is használhatja. Használja az adatbázis automatikus biztonsági mentést, helyreállítási becsült időpontját attól függ, számos tényező, többek között a egyszerre, az adatbázis mérete, a tranzakció napló méretének és a hálózat sávszélessége ugyanabban a régióban helyreállítása adatbázisok száma. A legtöbb esetben a helyreállítási ideje kisebb, mint 12 óra. Egy másik adatterület helyreállítás, ha az adatvesztést korlátozódik 1 óra óránkénti megkülönböztető adatbázisról geo felesleges tárolására szerint. 

> [AZURE.IMPORTANT] Szeretné helyreállítani az automatikus biztonsági mentés használatával, tagjának kell lennie az SQL Server munkatársi szerepkörök és az előfizetés tulajdonosa – lásd: [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md). Visszaállíthatja az Azure-portálra, PowerShell vagy a REST API-t. Nem használhatja a Transact-SQL nyelvben.

Az üzleti folytonosságot és helyreállítási mechanizmusként használja az automatikus biztonsági mentést, ha az alkalmazás:

- Nem számít misszió kritikus.
- Nem található meg egy kötelező SLA, ezért a 24 óra vagy annál hosszabb legrövidebb leállás nem eredményezi pénzügyi felelősség.
- Alacsony aránya adatainak módosítása (alacsony tranzakciók / h), és egy óra módosítás legfeljebb elvesztése egy elfogadható az adatok elvesztését. 
- A bizalmas költség. 

Ha gyorsabban helyreállítási, használja az [Active Geo-replikáció](sql-database-geo-replication-overview.md) (következő tárgyalt). Ha engedélyezni szeretné a 35 napnál régebbi ponttal adatainak helyreállítása, fontolja meg, az adatbázis rendszeres BACPAC fájlra mutató archiválás van szüksége a (tömörített tartalmazó fájl az adatbázis sémája és a kapcsolódó adatok) tárolt Azure blob-tárolóhoz vagy a másik helyre. Egy tranzakción keresztül egységes adatbázis archívum létrehozásával kapcsolatos további tudnivalókért olvassa el az [adatbázis biztonsági másolatának létrehozása](sql-database-copy.md) és [az adatbázis-példány exportálása](sql-database-export.md)című témakört. 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Aktív Geo-replikáció használata helyreállítási idejének csökkentése érdekében, és az adatok elvesztését a helyreállítás társított korlátozása

A adatbázis biztonsági mentése az adatbázis helyreállításához megelőzve a vállalati zavarok, mellett [Aktív Geo replikációs](sql-database-geo-replication-overview.md) azon részeire lépkedhet, lehetőség van-e négy olvasható másodlagos adatbázisok adatbázis konfigurálása is használhatja. Másodlagos adatbázisok szinkronizált megmaradnak az elsődleges adatbázis-aszinkron replikációs eljárás használatával. Ez a funkció egy adatok központ üzemszünetek vagy -alkalmazások frissítés során üzleti zavarok vírusvédelmet szolgál. Aktív Geo replikációs a ahhoz, hogy jobban a lekérdezési teljesítmény írásvédett lekérdezések földrajzilag szétszórt felhasználók is használható.

Ha az elsődleges adatbázis kikapcsol vagy szeretné vinni offline karbantartási tevékenységek, az elsődleges (más néven feladatátvevő), és csatlakozni az újonnan előléptetett elsődleges alkalmazások beállítása egy másodlagos gyorsan előléptetheti. A tervezett feladatátvételi nem az adatok elvesztését nem. A feladatátvételhez néhány nagyon legutóbbi tranzakciók miatt jellegét aszinkron replikációs adatvesztés kis mennyiségű lehetnek. Feladatátvétel után is újabb visszaállás - vagy terv szerint, vagy ha az Adatközpont ismét online állapotba kerül. Minden esetben a felhasználók legrövidebb leállás kis mennyiségű tapasztal, és újra kell. 

> [AZURE.IMPORTANT] Aktív Geo replikációs használatához használata kell lennie az előfizetés tulajdonosa, vagy az SQL Server rendszergazdai jogosultságok. Beállíthatja, és az Azure-portálra, PowerShell vagy az engedélyek az előfizetéshez tartozó vagy Transact-SQL nyelvben használatával REST API feladatátvételi engedélyek belül az SQL Server használatával.

Aktív Geo replikációs használja, ha az alkalmazás megfelel az alábbi feltételek valamelyikének:

- Fontos misszió.
- Egy szolgáltatásiszint-szerződés (SLA), amely nem teszi lehetővé az állásidőt legalább 24 órát tartalmaz.
- Legrövidebb leállás pénzügyi felelősség eredményezi.
- Magas sebessége adatok megváltoztatása magas pedig egy óra az adatok elvesztését nem elfogadható.
- Aktív geo-replikáció további költségét prioritásánál alacsonyabb potenciális pénzügyi felelősség és üzleti társított elvesztése.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Adatbázis visszaállítása egy felhasználó vagy alkalmazás hiba után

* Senki nem tökéletes! A felhasználó előfordulhat, hogy bizonyos adatok véletlenül töröl, véletlenül engedje el egy fontos táblázatban vagy akár a teljes adatbázis törlése. Vagy az alkalmazás előfordulhat, hogy véletlenül jó adatok felülírása hibás-alkalmazás hiba miatt. 

Ebben az esetben, ezek a helyreállítási lehetőségeket.

### <a name="perform-a-point-in-time-restore"></a>Végezze el a pont és az idő visszaállítása

Az automatikus biztonsági mentés segítségével egy másolatot az adatbázisról ismert jó pontjához helyreállítása időben, feltéve, hogy az adatbázis az adatmegőrzési időszak belül ideje. Miután az adatbázis visszaállítása az eredeti adatbázis cserélje ki a visszaállított adatbázist, vagy másolja a szükséges adatokat az adatok az eredeti adatbázis. Ha az adatbázis aktív Geo replikációs használ, azt javasoljuk, a szükséges adatokat másol a visszaállított másolatot az eredeti adatbázisba. Az eredeti adatbázis cseréje a visszaállított adatbázist, ha meg kell újrakonfigurálni és szinkronizálnia aktív Geo replikációs (ami egy nagy adatbázis igazán némi időt vehet igénybe). 

További információt és a lépések részletes leírását az adatbázis visszaállítása pontra az idő az Azure portálon megvásárlásáról és használatáról a PowerShell, [pont-a-time visszaállítása](sql-database-recovery-using-backups.md#point-in-time-restore). Nem tudja visszaállítani, használja a Transact-SQL nyelvben.

### <a name="restore-a-deleted-database"></a>A törölt adatbázis visszaállítása

Ha az adatbázis törlődik, de a logikai kiszolgáló nem törli, visszaállíthatja a törölt adatbázis a pontra, ahol törölték. Törölt adatoszlop azonos logikai SQL server adatbázis biztonsági másolatának visszaállítása Ez. Visszaállíthatja az eredeti néven használatával, vagy adja meg egy új nevet vagy a visszaállított adatbázist.

További információt és a lépések részletes leírását a törölt adatbázis visszaállítása az Azure portálon megvásárlásáról és használatáról a PowerShell, [törölt adatbázis visszaállítása](sql-database-recovery-using-backups.md#deleted-database-restore). Nem tudja visszaállítani, használja a Transact-SQL nyelvben.

> [AZURE.IMPORTANT] Ha a logikai kiszolgáló törölnek, nem tudja visszaállítani a törölt adatbázis. 

### <a name="import-from-a-database-archive"></a>Egy adatbázis archívum importálása

Ha meg van már az archiválás az adatbázis kívül a jelenlegi, automatikus biztonsági másolatok adatmegőrzési időszak történt az adatok elvesztését, akkor [az archivált BACPAC-fájl importálása](sql-database-import.md) , egy új adatbázist. Ezen a ponton az eredeti adatbázis cserélje ki az importált adatbázist, vagy másolja a szükséges adatokat az importált adatok az eredeti adatbázist. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Adatbázis-Azure rendezett regionális adatok központ üzemszünetek egy másik területére helyreállítása

<!-- Explain this scenario -->

Bár a ritka, az Azure adatközpont egy üzemszünetek van. Egy üzemszünetek fordul elő, amikor egy üzleti állásidőt azzal, előfordulhat, hogy csak utolsó néhány percet, vagy előfordulhat, hogy utolsó órát okozza. 

- Egy, hogy megvárja, amíg az újrakapcsolódáskor amikor az adatok központ üzemszünetek véget ért az adatbázist. Ez a módszer is engedheti meg magának kapcsolat nélkül is az adatbázis-alkalmazásokhoz. Ha például fejlesztési projekt vagy ingyenes próbaverziót nem kell folyamatosan használata. Amikor egy adatközpont egy üzemszünetek van, akkor nem fogja tudni a üzemszünetek mennyi ideig fog tartani, így ez a beállítás csak akkor működik, ha már nincs szükség az adatbázis egy ideje.
- Egy másik, hogy bármelyik másik adatterület áttérni alkalmazás használatakor aktív Geo replikációs vagy a helyreállítása geo felesleges adatbázis biztonsági mentése (Geo-visszaállítás) segítségével. Adja át csupán néhány másodpercet, miközben a biztonsági mentés helyreállítási óráig tart.

Amikor a művelet végrehajtása, mennyi ideig tart a visszaállítására, és mennyi az adatok elvesztését, akkor egy adatok központ üzemszünetek megelőzve merülnek fel, attól függ, hogy úgy dönt, hogy az alkalmazásban a fent ismertetett üzleti folytonosságot funkcióját használni. Sor kerülhet előfordulhat, hogy válasszon olyan adatbázis biztonsági mentése és az aktív Geo-replikációs attól függően, hogy az alkalmazás igényeknek megfelelően alakíthatja. Alkalmazás tervezés szempontok önálló adatbázisok és rugalmas készletek ezen üzleti folytonosságot szolgáltatások vitafórum olvassa el a [Tervezés egy alkalmazást a felhőalapú vészhelyreállítás](sql-database-designing-cloud-solutions-for-disaster-recovery.md) és [rugalmas készlet Vészhelyreállítási stratégiák meghatározása](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)című témakört.

Az alábbi szakaszokban lépésekkel állíthatja helyre a adatbázis biztonsági mentése vagy a aktív Geo replikációs áttekintést nyújtanak. Részletes köztük a tervezés követelményeknek, bejegyzés helyreállítási lépéseket és hasonlóan egy üzemszünetek olvashat a katasztrófa helyreállítási részletező végrehajtásához, című témakör tartalmazza [egy SQL-adatbázis-üzemszünetek helyreállítani](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Egy üzemszünetek előkészítése

Az üzleti folytonosságot funkció használata, függetlenül a következőket kell tennie:

- Azonosítása, és készítse elő a cél kiszolgálót, beleértve a kiszolgálói szintű tűzfalszabályokat, a bejelentkezések és a fő adatbázist webhelycsoportszintű engedélyek.
- Ügyfelek és ügyfélalkalmazásokban az új kiszolgálóhoz irányítja módjának meghatározása
- A dokumentum más függőségek, például a naplózási beállítások és értesítések 
 
Ha nem tervezzük és előkészítése megfelelően, az alkalmazások arra az online után feladatátvevő vagy a helyreállítás további ideig tart, és valószínűleg is szükséges a terhelési – hibás együttes egyszerre hibaelhárítás.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>A másodlagos geo replikált adatbázist áttérni 

Ha a helyreállítási mechanizmusként, [egy másodlagos geo replikált áttérni kényszerítése](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database)aktív Geo replikációs használja. Másodpercek a másodlagos válik az új elsődleges előléptetett van, és készen áll új tranzakciók rögzítése és bármely lekérdezés – az adatok volna még nem replikált adatvesztés csupán néhány másodpercet válaszolni. A feladatátvevő automatizálása a további tudnivalókért lásd [Tervezés egy alkalmazást a felhőalapú vészhelyreállítás](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Ha az Adatközpont ismét online állapotba kerül, azt is megteheti az eredeti elsődleges csomópontoktól (vagy ne).

### <a name="perform-a-geo-restore"></a>Végezze el a Geo visszaállítása 

Alkalmazás használatakor automatikus biztonsági mentés geo felesleges tároló ismétlésekkel a helyreállítási mechanizmusként, [kezdeményezése egy adatbázis-helyreállítás Geo-visszaállítási használatával](sql-database-disaster-recovery.md#recover-using-geo-restore). Helyreállítási általában esedékes 12 órán belül – az adatok elvesztését, akár egy óra mikor határozza meg az utolsó óránkénti megkülönböztető biztonsági másolat tett és replikált. Mindaddig, amíg a helyreállítás befejezése után az adatbázis nem rögzítése a tranzakciókat, vagy új lekérdezést. 

> [AZURE.NOTE] Az Adatközpont ismét online állapotba kerül, mielőtt átkapcsolhatná az alkalmazás a visszaállított adatbázist, ha meg szeretné szakítani a helyreállítás egyszerűen.  

### <a name="perform-post-failover--recovery-tasks"></a>Végrehajtása utáni feladatátvevő és helyreállítási feladatok 

Bármelyik helyreállítási mechanizmusa helyreállítása után a felhasználók előtt a következő további feladatokat kell elvégeznie és alkalmazások vissza lépéseket:

- Átirányítás ügyfelek és az új kiszolgálót és a visszaállított adatbázist ügyfélalkalmazásai
- Győződjön meg róla, megfelelő kiszolgálói szintű tűzfalszabályokat vannak még a felhasználók számára a csatlakozás (vagy használja az [adatbázis szintű tűzfalak](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Győződjön meg róla, megfelelő bejelentkezési adatok és a fő adatbázist webhelycsoportszintű engedélyek vannak még (vagy használja a [felhasználók található](https://msdn.microsoft.com/library/ff929188.aspx))
- Állítsa be a naplózás, szükség szerint
- Értesítések, szükség szerint konfigurálása

## <a name="upgrade-an-application-with-minimal-downtime"></a>A minimális legrövidebb leállás alkalmazás frissítése

Előfordul, hogy egy alkalmazást kell miatt tervezett karbantartásokkal kapcsolatos információk, például az alkalmazás frissítésre offline állapotba. [Kezelése alkalmazás frissítése](sql-database-manage-application-rolling-upgrade.md) az Active Geo-replikáció használata ahhoz, hogy az a felhő alkalmazás legrövidebb leállás lekicsinyítheti a frissítés során, és adjon meg egy helyreállítási elérési utat, abban az esetben, ha valami mentésük közbeni ismerteti. Ez a cikk vizsgálja meg a frissítési folyamat orchestrating két különböző módszer, és ismerteti, hogy a előnyét és a kompromisszumok az egyes beállítások.

## <a name="next-steps"></a>Következő lépések

Alkalmazás tervezési szempontjait önálló adatbázisok és rugalmas készletek vitafórum olvassa el a [felhő vészhelyreállítás céljával tervezés](sql-database-designing-cloud-solutions-for-disaster-recovery.md) és [rugalmas készlet Vészhelyreállítási stratégiák meghatározása](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)című témakört.






