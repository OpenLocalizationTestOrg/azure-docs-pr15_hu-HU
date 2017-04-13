<properties
   pageTitle="Az adatmegőrzési szabály időbeli táblák korábbi adatok kezelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként szeretné tartani a felügyelheti korábbi adatokat időbeli adatmegőrzési szabály használatával."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Az adatmegőrzési szabály időbeli táblák korábbi adatok kezelése

Időbeli táblák növelheti az adatbázis mérete több, mint normál táblázatok, különösen akkor, ha egy hosszabb ideig megőrzi az előzmények adatainak. Így a korábbi adatok adatmegőrzési szabály tervezése és kezelése az életciklus minden időbeli táblázat fontos eleme. Időbeli Azure SQL-adatbázisban táblák és könnyen használható adatmegőrzési mechanizmusa, amely segít a feladat elvégzéséhez.

Időbeli előzmények adatmegőrzési lehet konfigurálva a táblázat egyes szintjén, amely lehetővé teszi, hogy a felhasználók rugalmas elévülési létrehozása házirendeket. Egyszerű időbeli adatmegőrzési alkalmazása: táblázat létrehozása vagy séma módosítása közben kell beállítani csak az egyik paraméter igényel.

Adatmegőrzési szabály határozza meg, miután Azure SQL-adatbázis elkezdenek ellenőrzése rendszeresen esetén automatikus adatok karbantartása jogosultak korábbi sorokat. Egyező sorok azonosítása és az előzmények táblázatban azok kiszállítása átlátszó, fordul elő van ütemezve, és a rendszer futtatása a háttérben feladatra. Az előzmények táblázatsorok kor feltétel alapján az oszlop, amely SYSTEM_TIME időszak végén van véve. Az adatmegőrzési időszak, például van beállítva hat hónap, ha a táblázatsorok karbantartása használatára jogosult a következő feltétel felel meg:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

A fenti példában azt feltételezi, hogy **ValidTo** oszlop SYSTEM_TIME időszak vége felel meg.

##<a name="how-to-configure-retention-policy"></a>Hogyan lehet adatmegőrzési szabály beállítása?

Mielőtt beállítaná az adatmegőrzési időbeli tábla, először ellenőrizze hogy időbeli korábbi adatmegőrzési engedélyezve van *az adatbázis szinten*-e.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Adatbázis jelző **is_temporal_history_retention_enabled** alapértelmezés szerint be van állítva, de a felhasználók módosíthatják az adatbázis módosítása utasítással. Is automatikusan beállítás ki művelet [idő visszaállítása a pont](sql-database-point-in-time-restore-portal.md) után. Ahhoz, hogy az adatbázis időbeli előzmények adatmegőrzési karbantartása, hajtsa végre a következő utasítást:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Adatmegőrzési időbeli táblák is beállíthatja, még akkor is, ha **is_temporal_history_retention_enabled** ki KAPCSOLVA, de a lejárt sorok automatikus karbantartása ebben az esetben nem aktiválódik.


Adatmegőrzési szabály állítható be tábla létrehozása során a HISTORY_RETENTION_PERIOD paraméter megadása:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Azure SQL-adatbázis lehetővé teszi, hogy adja meg az adatmegőrzési időszak különböző időegységek használatával: nap, hét, hónap és év. Ha HISTORY_RETENTION_PERIOD nincs megadva, végtelen adatmegőrzési tekinti. VÉGTELEN kulcsszó explicit módon is használhatja.

Bizonyos esetekben célszerű adatmegőrzés konfigurálása táblázat létrehozása után, vagy módosíthatja a korábban beállított értékét. Ebben az esetben ALTER TABLE utasítás használata:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  SYSTEM_VERSIONING az adatmegőrzési időszak értéke *nem őrzi meg a* kikapcsolva beállítást. SYSTEM_VERSIONING beállítást bekapcsolva HISTORY_RETENTION_PERIOD megadott kifejezetten eredményez az végtelen az adatmegőrzési időszak nélkül.

Az adatmegőrzési aktuális állapotának áttekintése, az alábbi lekérdezéssel, hogy illesztő időbeli adatmegőrzési bevezetés jelző az adatmegőrzési időszak az egyes táblák adatbázis szintre:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Hogyan SQL-adatbázis törlése kor sorokat?

A törlési folyamat attól függ, hogy az előzmények táblázat index elrendezését. Fontos figyelje meg, hogy *csak előzmények csoportosított indexű (B fa vagy columnstore) is rendelkeznek függvényt adatmegőrzési házirend konfigurálva*. Háttér tevékenység lejárt adatok karbantartása végrehajtásához függvényt az adatmegőrzési időszak összes időbeli tábla jön létre.
Karbantartási logika a csoportosított rowstore (B-fa) indexhez törlése (legfeljebb 10 KB) kisebb szövegadattömb lejárt sorához nyomása adatbázis naplója és I/O alrendszer a kis méretre. Bár a Lemezkarbantartó logika szükséges B fa index, a régebbi, mint az adatmegőrzési időszak nem erősen garantálni sorok törlése sorrendjének használja. Emiatt *nem feltétlenül a minden alkalmazás karbantartása sorrendben való függőség*.

A csoportosított columnstore a Lemezkarbantartó feladatot egyszerre eltávolítja a teljes [sor csoportok](https://msdn.microsoft.com/library/gg492088.aspx) (általában tartalmaz a sorok minden 1 millió), vagyis nagyon hatékony, különösen akkor, ha a korábbi adatok egy nagy tempójában jön létre.

![Csoportosított columnstore adatmegőrzési](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Kiváló adatok tömörítés és hatékony adatmegőrzési karbantartása tesz csoportosított columnstore index esetek a tökéletes választás esetén a terhelést gyorsan hoz létre a nagy mennyiségű korábbi adat. Minta a változások követésének és naplózás, trend analysis vagy IoT adatok bevitel intenzív [időbeli táblák használó tranzakció alapú feldolgozás munkaterhelésekből](https://msdn.microsoft.com/library/mt631669.aspx) tipikus.

##<a name="index-considerations"></a>Index kapcsolatos szempontok

A Lemezkarbantartó rowstore csoportosított indexű táblák feladathoz index induljon az oszlop megfelelő SYSTEM_TIME időszak végén. Ha ilyen index nem léteznek, nem tudja függvényt az adatmegőrzési időszak beállítása:

*Üzenet 13765, Level 16, State 1 <br> </br> függvényt az adatmegőrzési időszak beállítása nem sikerült, a rendszer verziószámmal időbeli táblázat "temporalstagetestdb.dbo.WebsiteUserInfo" mert az előzmények táblázat "temporalstagetestdb.dbo.WebsiteUserInfoHistory" nem tartalmaz szükséges csoportosított index. A csoportosított columnstore vagy az oszlop, amely megfelel a SYSTEM_TIME végére kezdődő B fa index létrehozása a verziók táblázatban az időszak.*

Fontos figyelje meg, hogy az alapértelmezett előzmények táblázat már létrehozott Azure SQL-adatbázis van csoportosított indexbe, amely megfelel az adatmegőrzési szabály. Ha, hogy az index függvényt az adatmegőrzési időszak táblázat eltávolítása, a művelet a következő hiba miatt sikertelen lesz:

*Üzenet 13766, Level 16, State 1 <br> </br> nem húzhatja a csoportosított index "WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory", mert a lejárt adatok automatikus karbantartási van használatban. Fontolja meg beállítást HISTORY_RETENTION_PERIOD végtelen a megfelelő rendszer verziószámmal időbeli táblázatban Ha módosítani szeretné az index húzhatja.*

Kattintson a csoportosított columnstore index karbantartása akkor működik a optimálisan, ha a korábbi sorok a növekvő sorrendben (az időszak oszlop végére megrendelte), amely esetén mindig az esetben az előzmények táblázat van töltve kizárólag a SYSTEM_VERSIONIOING eljárás szerint vannak beszúrva. Ha az előzmények táblában lévő sorok nem vége (amely lehet a helyzet, ha a meglévő adatokból a korábbi áttelepített) az időszak oszlop szerint vannak rendezve, újra hozzon létre csoportosított columnstore index fölött B fa rowstore index rendelt megfelelően, optimális teljesítmény eléréséhez.

Mivel a rendszer-a verziószámozás működése természetesen bevezetett sor csoportok a következőhöz megváltoztatva módosíthatja azt is, ne a csoportosított columnstore index és a függvényt az adatmegőrzési időszak Előzmények táblán Újraépítés. Ha csoportosított columnstore index az előzmények táblában lévő újraépítéséhez van szüksége, ezt megteheti a újbóli létrehozásával fölött kompatibilis index, B – fa megőrzése mellett, a rendszeres adatok karbantartása szükséges rowgroups a rendezés. Ugyanezt a megközelítést kell tenni, ha a meglévő előzmények táblázatot, amely magában csoportosított oszlop tárgymutató nélkül garantált adatok sorrendben időbeli táblázat létrehozása:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Ha függvényt az adatmegőrzési időszak az előzmények táblázat a csoportosított columnstore indexű van konfigurálva, a táblázatok nem hozható létre a további nem csoportosított B fa indexek:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Fent utasítás végrehajtása kísérlete a következő hibával meghiúsul:

*Üzenet 13772, Level 16, State 1 <br> </br> nem hozható létre nem csoportosított index időbeli táblába "WebsiteUserInfoHistory" óta függvényt az adatmegőrzési időszak és a csoportosított columnstore indexszel rendelkezik.*

##<a name="querying-tables-with-retention-policy"></a>Adatmegőrzési szabály tábla lekérdezése

Időbeli tábla minden lekérdezés automatikusan függvényt adatmegőrzési elkerülése érdekében járhat, és a várttól eltérően működik, mivel lejárt sorok törölheti a Lemezkarbantartó tevékenység *bármely pontján az időt és a tetszőleges sorrendbe*által egyező korábbi sorok kiszűrése.

Az alábbi képen látható az egyszerű lekérdezés lekérdezéstervet:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

A lekérdezéstervet az időszak oszlop (ValidTo) végéhez további szűrt szerepel az "Csoportosított Index beolvasása" operátor (kiemelve) előzmények tábla. Ez a példa feltételezi, hogy 1 hónap adatmegőrzési időszak WebsiteUserInfo táblán lett beállítva.

![Adatmegőrzési lekérdezés szűrő](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Ha közvetlenül lekérdezés táblába, megjelenhet sorokat, amelyek régebbi, mint a megadott adatmegőrzési időszak, de minden garancia nélkül megismételhető lekérdezés eredménye. A következő képen a lekérdezés végrehajtása lekérdezéstervet a előzmények táblázatban további alkalmazott szűrők nélkül:

![Adatmegőrzési szűrő nélkül előzmények lekérdezése](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Nem használják a üzleti logikai funkcióinak túl az adatmegőrzési időszak táblába olvasása, ahogy a várttól eltérően működik, vagy nem várt eredmények jelenhetnek meg. Időbeli lekérdezések használatára vonatkozó SYSTEM_TIME záradék időbeli táblában található adatok elemzése az ajánlott.

##<a name="point-in-time-restore-considerations"></a>Mutasson az idő visszaállítás előtt megfontolandó kérdések

Új adatbázis létrehozásakor [időben konkrét pontjához meglévő adatbázis](sql-database-point-in-time-restore-portal.md)visszaállítása az adatbázis szintjén letiltva időbeli adatmegőrzési rendelkezik. (**is_temporal_history_retention_enabled** jelző beállítása nélkül). Ez a funkció lehetővé teszi, hogy vizsgálja meg, visszaállítása, minden korábbi sorok nélkül tartania, hogy lejárt sorok eltávolítása előtt jelenik meg a lekérdezés azokat. Használhatja azt *vizsgálja meg a korábbi adatok túl konfigurált az adatmegőrzési időszak*.

Tegyük fel, hogy időbeli táblázat van-e egy HÓNAPPAL az adatmegőrzési időszak megadott. Ha az adatbázis támogatási szolgáltatás réteg jött létre, lenne adatbázis állapotát az adatbázis-példány létrehozása felfelé vissza az elmúlt 35 napra. Amely hatékony lehetővé teszi elemzése legfeljebb 65 napnál régebbi közvetlenül az előzmények táblázat lekérdezésével korábbi sorokat.

Időbeli adatmegőrzési karbantartása aktiválni kívánt, futtassa a következő Transact-SQL-utasítás után pont az idő visszaállítása:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Következő lépések

Időbeli táblázat használata az alkalmazások kivétel [Azure SQL-adatbázisban időbeli táblázatok – első lépések](sql-database-temporal-tables.md)című témakörben olvashat.

Látogasson el a csatorna 9 [valós ügyfél időbeli végrehajtása sikeres szövegegység](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) és [live időbeli bemutató](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)megtekintés.

Időbeli táblák részletes információt tekintse át az [MSDN dokumentációt](https://msdn.microsoft.com/library/dn935015.aspx).
