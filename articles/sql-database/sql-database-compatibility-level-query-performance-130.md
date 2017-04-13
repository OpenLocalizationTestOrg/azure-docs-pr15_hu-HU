<properties
    pageTitle="Kompatibilitási szintnek mérje fel, hogy miként |} Microsoft Azure"
    description="A lépéseket, illetve annak megállapítása, hogy melyik kompatibilitási szintet a legjobb az adatbázis Azure SQL-adatbázis vagy a Microsoft SQL Server eszközöket"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>A lekérdezési teljesítmény növelése kompatibilitásra szint 130 Azure SQL-adatbázisban


Azure SQL-adatbázis fut átlátszó adatbázisok ezer több száz számos különböző kompatibilitási szinten megőrzése és a Microsoft SQL Server megfelelő verziójára a visszamenőleges kompatibilitásra biztosítása az összes vevő!

Ezért semmi nem fog tudni módosíthatja a létező adatbázisokat a legújabb kompatibilitási szintet az új lekérdezés-optimalizáló és a lekérdezés processzor szolgáltatások előnyt felhasználókat. Az előzmények emlékeztetőt az Igazítás az engedélyszintekről az alapértelmezett kompatibilitási SQL-verzió a következők:

- 100: az SQL Server 2008 és Azure SQL-adatbázis V11.
- 110: az SQL Server 2012 és Azure SQL-adatbázis V11.
- 120-ra: az SQL Server 2014-es és Azure SQL-adatbázis V12.
- 130: az SQL Server 2016-ban és az Azure SQL-adatbázis V12.


> [AZURE.IMPORTANT] **Mid június 2016**Azure SQL-adatbázisban, kezdve az alapértelmezett kompatibilitási szint helyett **az újonnan létrehozott** adatbázisok 120 130 lesz.
> 
> Mid június 2016 előtt létrehozott adatbázisok fog *nem* érinti, és megőrzi a jelenlegi kompatibilitási szintet (100, 110 vagy 120-ra). Adatbázisok Azure SQL-adatbázis verzióról történő V12 V11 nem lesz a kompatibilitási szintjük telepíthetők át vagy módosítani.


Ez a cikk mutatunk kompatibilitási szintet 130, és hogyan használhatja ezeket előnyökkel jár a előnyeit. A lehetséges-hatásai a lekérdezési teljesítményt a meglévő SQL-alkalmazásokat a cím azt.


## <a name="about-compatibility-level-130"></a>Kompatibilitással kapcsolatos 130 szint


Ha szeretné, hogy az adatbázis kompatibilitási bővítené, első lépésként az alábbi Transact-SQL-utasítás végrehajtása.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Mielőtt ezt a módosítást szintre 130 az **újonnan** létrehozott adatbázisok történik, nézzük milyen Ez a változás tekintse át az összes néhány nagyon egyszerű lekérdezés példákból, és ellenőrizze, hogy hogyan bárki előnyeivel azt.

A relációs adatbázisok lekérdezés feldolgozás nagyon bonyolult, és megtudhatja, hogy a belső tervezés választási lehetőségek és viselkedések sok számítógép tudomány és szabványban vezethet. A dokumentumban a tartalom szándékosan egyszerűbb lett meggyőződni arról, hogy a minimális technikai tájékoztatatást rendelkező összes felhasználó megérteni a kompatibilitási szintjének módosítása a hatásait, és határozza meg, hogyan alkalmazások élvezheti.

Következzenek a táblázatot a Mi a kompatibilitási szintet 130 elérhetővé teszi rövid figyelmébe.  További részleteket találhat szintjén [ALTER adatbázis kompatibilitási (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/bb510680.aspx), de rövid összefoglalása:

- Utasítás Beszúrás jelölje ki a beillesztési művelet több szálon történő is lehet, de lehetnek a párhuzamos tervet, miközben előtt a művelet volt egy téma szerint rendezett.
- A memória optimalizálása táblázat és a táblázat változók lekérdezések is ettől a párhuzamos tervek, miközben előtt a művelet lett is egyetlen-téma szerint rendezett.
- Táblázat memória optimalizálása statisztikai most is mintát és automatikus frissítése. Lásd: [újdonságai adatbázismotort: A memóriában OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) további információt.
- Köteg mód v/s oszlop áruházból indexek változások sor mód
  - Rendezés egy oszlop áruházból indexű táblázat most köteg módban van.
  - Ablakok elrendezése összegzések köteg módban, például kimutatások TSQL eltérés/ÉRDEKLŐDŐ működni.
  - Több különböző záradékok oszlop tároló tábla lekérdezései köteg üzemmódban működnek.
  - DOP alatt futó lekérdezések = 1 vagy köteg módban is végrehajtása a soros csomaggal.
- Utolsó hivatkozás becslés javítása ténylegesen kompatibilitási szintet 120-ra, és onnan érkező, de azokat meg operációs rendszert futtató alacsonyabb szinten kompatibilitási (azaz 100 vagy 110), az áthelyezés kompatibilitási szint 130 is állapotba kerül e fejlesztések, és ezeket az alkalmazás a lekérdezési teljesítmény is élvezheti.


## <a name="practicing-compatibility-level-130"></a>Kompatibilitási szintet 130 gyakorlás


Első pedig hozzuk működésbe az egyes táblázatokat, indexek és véletlen – a gyakorlás az alábbi új funkciók néhány hoz létre. A TSQL parancsfájl példákat, vagy SQL Server 2016 alapján Azure SQL-adatbázis futtatható. Az Azure SQL-adatbázis létrehozásakor ellenőrizze, hogy úgy dönt, legalább egy P2 adatbázis mert kell legalább egy pár magmintákat engedélyezni szeretné a több szálon futó műveletvégzés, és ezért ezek a funkciók összekapcsolhatók.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Most következzenek a részletek, a lekérdezés feldolgozása funkciók lesz kompatibilis szinthez 130 részét.


## <a name="parallel-insert"></a>A párhuzamos BESZÚRÁSA


A kompatibilitási szintet 120-ra és 130, melyik rendre hajtja végre a BEILLESZTÉSI művelet összefűzött egyetlen adatmodell (120), és a több szálon történő modell (130) a BEILLESZTÉSI művelet végrehajtása a lenti TSQL kimutatások hajt végre.


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Igénylésével tényleges a lekérdezéstervet megjeleníti a grafikus ábrázolása vagy az XML-tartalmat, melyik hivatkozás becslés függvényt jön létre a lejátszás határozza meg. Megjeleníti a tervek egymás mellett a szám 1, hogy jól láthatja, hogy az oszlop áruházból BESZÚRÁSA adatvégrehajtás Ugrás a soros a 120 párhuzamos 130. Is ne feledje, hogy a léptető ikon ábrázoló bemutató arra, hogy most két párhuzamos nyilak léptető végrehajtása 130 tervben módosítása valóban párhuzamos. Ha nagy beszúrási műveletek elvégzéséhez, a párhuzamos végrehajtás csatolva van az adatbázishoz, rendelkezésére core száma lesz jobb teljesítményt mutat; legfeljebb 100 időpontok egy gyorsabb függően az a helyzet!


*Ábra 1: Beszúrás művelet módosítja a soros szélesség kompatibilitásra szint 130.*


![Ábra 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>Egymás utáni köteg mód


Adatsorok feldolgozása közben kompatibilitási szintet 130 áthelyezése hasonlóan lehetővé teszi a parancsfájl módban. Első lépésként köteg módú műveletek érhetők csak már a helyükön vannak áruházból oszlopindex. Egy köteg második, általában jelöl ~ 900 sorokat, és használja a kód logika multicore Processzor, újabb memória átviteli optimalizálva, és közvetlenül használja a tömörített adatok az oszlop tároló, amikor csak lehetséges. Következő körülmények között SQL Server 2016, az ~ 900 sorok dolgozhat helyett 1 sorok időben, és következtében a művelet a teljes általános költség most osztva a teljes köteg, a teljes költségét sor csökkentése. Ez az a oszlop áruházból tömörítés alapvetően kombinálni műveletek megosztott összeg csökkenti a várakozási részt egy VÁLASZTÓ köteg üzemmódot. Az oszlop tároló kapcsolatos részletesebb tájékoztatás található, és a köteg [Columnstore indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx)módtól.


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Alább látható, mint a lekérdezés tervek egymás mellett a szám 2, akkor is megfigyelésével lásd: a feldolgozás mód megváltozott a kompatibilitási szintű, melynek következtében a lekérdezések végrehajtása mindkét kompatibilitási szintbe teljesen, azt láthatja, hogy feldolgozás legtöbbször töltött sor módban (86 %) összehasonlítva a köteg mód (14 %), ahol 2 kötegekben dolgozott. Növelje az adatkészlet, segítsége nő.


*Ábra 2: Művelet módosítások VÁLASSZA a soros kompatibilitási szintet 130 köteg módra.*


![Ábra 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>A rendezés végrehajtás köteg mód


Hasonlóan, mint a fenti, de egy rendezési művelet alkalmazni az áttűnés köteg mód (kompatibilitási szint 130) sor módból (kompatibilitási szint 120-ra) javíthatja a rendezési művelet ugyanezen okokból teljesítményét.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Látható egymás mellett a 3-as szám, akkor láthatja, hogy a rendezési művelet sor módban a költség 81 %-át képviseli, amíg a köteg mód, csak a költség (a kurzor 81-56 %-át a rendezést, magát a) 19 %-át képviseli.


*Ábra 3: Rendezési művelet változik sorában kompatibilitási szintet 130 köteg módra.*


![Ábra 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Természetesen e minták csak tartalmazó sorok, ezer tízesre vagyis semmi, amikor megjeleníti a legtöbb SQL-kiszolgálók a rendelkezésre álló adatokat e napok. Csak a project ezeket ellen sorok millióit helyett, és ez a néhány percig, amíg a terhelést a jellegét függőben mindennap kímélni végrehajtás lefordíthatja.


## <a name="cardinality-estimation-ce-improvements"></a>Hivatkozás becslés (CE) javítása


Jelent meg az SQL Server 2014-es, bármilyen adatbázis kompatibilitási szintjén 120-ra vagy a fent operációs rendszert futtató láthatóvá válik az új hivatkozás becslés felhasználása használható funkciók körét. Lényegében hivatkozás becslés a logika alapján határozza meg, hogyan SQL server a becsült költség alapján lekérdezés végrehajtása. A becslés számítja ki a társított objektumok, amelyek lekérdezik részt statisztika beviteli. Gyakorlatilag, a magas szintű, hivatkozás becslés függvényei sor darab ad becslést együtt az értékek száma egyedi érték, a telepítési információ, valamint ismétlődő megszámolja található a táblákat és objektumokat a lekérdezésre hivatkozik. Első ezek ad becslést történt, vezethet szükségtelen lemeztevékenység miatt nem elegendő memória támogatás (azaz TempDB kijutás), illetve a soros csomag végrehajtása a kijelölt fölé egy párhuzamos csomag végrehajtását néhány nevet. Elfogadásáról, nem a megfelelő becslést ad a lekérdezés-végrehajtási egy általános teljesítményére csökkenés vezethet. A másik oldalon jobb becslést ad a pontosabb ad becslést, jobban lekérdezés végrehajtások vezet!

Említett előtt, lekérdezés optimalizálásokat és ad becslést összetett frissítését, de ha azt szeretné, ha többet szeretne tudni a lekérdezés csomagok és hivatkozás becslés, hivatkozhat egy mélyebb merülési [Optimalizálása a lekérdezés tervek és az SQL Server 2014-es hivatkozás becslés](https://msdn.microsoft.com/library/dn673537.aspx) , a dokumentumot.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Melyik hivatkozás becslés jelenleg használ?


Határozza meg a melyik hivatkozás becslése futtatja a lekérdezést, csak használata az alábbi lekérdezés minták. Megjegyzés: Ez a példa első kompatibilitási szintet 110, a régi hivatkozás becslés függvény használatát fog futni.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Végrehajtási befejezése után kattintson az XML-hivatkozásra, és tekintse meg az első léptető tulajdonságainak alább látható módon. Megjegyzés: a tulajdonság neve nevű CardinalityEstimationModelVersion 70 jelenleg meg. Nem jelenti azt, hogy az adatbázis kompatibilitási szintje pedig az SQL Server 7.0-s verzió (értéke a fenti TSQL kimutatásokban láthatóként 110), de az érték 70 egyszerűen jelöli a régi hivatkozás becslés szolgáltatásait óta az SQL Server 7.0, amelyet nem fő változatok addig SQL Server 2014-es (amely megtalálható a kompatibilitási szintű 120-ra).


*4-es szám: A CardinalityEstimationModelVersion értéke 70 110 vagy alatti kompatibilitási szintet használata esetén.*


![A 4-es szám](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


Azt is megteheti 130 kompatibilitási szintjének módosítása, és az új hivatkozás becslés függvényt letiltása a állítsa bekapcsolva beállításokkal [MEGVÁLTOZTATHATJA adatbázis hatóköre](https://msdn.microsoft.com/library/mt629158.aspx)LEGACY_CARDINALITY_ESTIMATION használatával. Ez lesz pontosan megegyező 110 szempontból egy hivatkozás becslés függvény, a legújabb lekérdezésfeldolgozást kompatibilitási szintet használata közben. Ezzel a módszerrel élvezhetik az új lekérdezésfeldolgozást funkciók a legújabb kompatibilitási szintű (azaz a köteg mód kikapcsolása) lesz, de továbbra is a régi hivatkozás becslés funkciókat támaszkodhat szükség esetén.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


A kompatibilitási szintet 120-ra vagy a 130 egyszerűen áthelyezése lehetővé teszi, hogy az új hivatkozás becslés funkciók. Ebben az esetben az alapértelmezett CardinalityEstimationModelVersion állítja be ennek megfelelően 120-ra vagy a 130 mint alább látható.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Ábra 5: A CardinalityEstimationModelVersion értéke 130 130 kompatibilitási szintet használata esetén.*


![Ábra 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>A hivatkozás becslés különbségek aláírnia


Most vegyük kissé további összetett lekérdezés érintő INNER JOIN az egyes predikátumok WHERE záradékot tartalmazó, és tekintse meg a sorok száma becslés a régi hivatkozás becslés függvény első.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


A sor becslés a régi hivatkozás becslés szolgáltatásával követelések 194,284 sorok, 200,704 sorok, a lekérdezés hatékony végrehajtása adja eredményül. Természetesen, mielőtt said, ezek sorok száma az eredmények is függ, hogy milyen gyakran futtatta az előző minták, amelyeket felhasználhat a minden futtatásakor a minta táblák feltölti. Természetesen a lekérdezés a predikátumok is hatással lesz a tényleges becslése kívül az asztal alakzatot, a tartalom, adatok, és hogyan adat ténylegesen összehangolására egymással.


*Ábra 6: A sorok száma becslés kikapcsolt 194,284 vagy 6000 sorok várható 200,704 sorokból.*


![Ábra 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Ugyanúgy, nézzük most a ugyanazon lekérdezés végrehajtása az új hivatkozás becslés szolgáltatásokkal.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Megjeleníti a az alábbi most már láthatja, hogy a sor becslés-e a 202,877, vagy sokkal közelebb és nagyobb, mint a régi hivatkozás becslés.

*A 7 szám: A sorok száma becslés ettől kezdve 202,877 194,284 helyett.*


![Ábra 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


A valóságban az eredményhalmaz 200,704 sorok (de összes, attól függ, hogy milyen gyakran did az előző minták a lekérdezések futtatása, de fontosabb, a TSQL a RAND() utasítást használja, mivel a tényleges értékek visszaadott eltérhetnek egy Futtatás a következő). Adott ebben a példában, ezért az új hivatkozás becslés hatékonyabban a sorok számát becslése, mert 202,877 sokkal közelebb 200,704, mint 194,284 jelent! Vezetéknév, ha például módosítja a WHERE záradék az egyenlő predikátumokkal (helyett ">" példány), ez teheti a becslést ad a régi között, és a hivatkozás új, még több eltér, attól függően, hogy hány megfelelő függvénnyel is használható lesz.

Természetesen ebben az esetben alatt álló ~ 6000 sorok tényleges száma a ki nem jelenítik meg az egyes esetekben az adatokat. Most már a Transzponálás erre a több tábla és összetettebb lekérdezések és időnként a becsült sorok millióit lehet kikapcsolása sorok millióit, és emiatt a megfelelő végrehajtási terv fel-vagy TempDB kijutás, és hogy több I/O, amelyek nem elegendő memória támogatás kérése a kockázat sokkal nagyobb.

A lehetőség, gyakorlat esetén ezen összehasonlítása a leggyakoribb lekérdezések és adatkészleteket, és saját maga által mekkora részét a régi és új ad becslést érinti, miközben néhány csak válhat további kikapcsolása a valójában a vagy néhány másoknak csak egyszerűen közelebb az aktuális sor megszámolja a eredményhalmazok a ténylegesen visszaadott is találhat. Az alakzat a lekérdezések, az Azure SQL-adatbázis jellemzők, a természeti és az adatkészleteket, és a velük kapcsolatos elérhető statisztika méretét, hogy az összes függ. Ha csak az Azure SQL-adatbázis példány hozta létre, a lekérdezés-optimalizáló kell tudomást szerez, lefut az előző lekérdezés készült statisztika újrafelhasználása, hanem nulláról indulva összeállítása. Igen a becsült az, hogy nagyon környezetfüggő és szinte minden kiszolgáló és az alkalmazás helyzet alkalmazásra. Fontos szem előtt kell tartania eleme!


## <a name="some-considerations-to-take-into-account"></a>Néhány kapcsolatos szempontok figyelembevétele


Bár a legtöbb munkaterhelésekből előnyös elfogadása a kompatibilitási szintet gyártási környezetben, mielőtt a kompatibilitási szintet 130, amelyen alapvetően 3 lehetőség közül választhat:

1. Kompatibilitási szintre 130 helyezi, és látni, hogyan műveletet végre. Abban az esetben azt veszi észre, hogy bizonyos hátrányait, egyszerűen csak a kompatibilitás szintű állítsa vissza az eredeti szintekre, vagy hagyja 130 és csak fordított hivatkozás becslése Visszatérés a régi mód (fent bemutatottak ez egyedül is a probléma megoldása).
2. Hasonló gyártási terhelés alatt a meglévő alkalmazások alapos tesztelése, finomabb és a teljesítmény, mielőtt gyártási ellenőrzése. Problémák esetén ugyanaz, mint a fenti programban bármikor visszatérhet az eredeti kompatibilitási szintet, vagy egyszerűen fordított hivatkozás becslése a régi módba.
3. A végleges lehetőséget, és a legutóbbi módja a kérdésekkel foglalkozik, mint is kihasználhatja a lekérdezés áruházból. Az aktuális ajánlott parancs, amely az! Megkönnyítése a lekérdezések a kompatibilitási elemzésének szint 120-ra, vagy alatti 130, és nem javasoljuk elég lekérdezés tárolót kell használnia. Lekérdezés áruházból érhető el az Azure SQL-adatbázis V12 legújabb verzióját, és azt lehetővé teszi a lekérdezési teljesítmény hibaelhárítási segítséget. Érdemes egy nézetbeli adatok író az adatbázis összegyűjtése és a bemutató összes lekérdezésekkel kapcsolatos részletes előzmények, a lekérdezés áruházból. Ezzel megkönnyíti az teljesítmény forensics diagnosztizálása és a probléma elhárításához idő csökkentésével. A további információt találhat [lekérdezés tár: A nézetbeli adatok író az adatbázis](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


At magas szintű, ha már rendelkezik egy sor olyan adatbázisok kompatibilitási szintjén 120-ra vagy alatt, és áthelyezése egy részüket 130, vagy mert a terhelést automatikusan kiépítése új adatbázisokra, amelyek lesz hamarosan állítható be 130, alapértelmezés szerint szeretné fontolja meg a követő:

- Mielőtt módosítja a termelési új kompatibilitási szintre, engedélyezze a lekérdezés áruházból. További információt a hivatkozott is módosítani szeretné az adatbázis-kompatibilis üzemmód [és a lekérdezés tárolót kell használnia](https://msdn.microsoft.com/library/bb895281.aspx) .
- Ezután tesztelje az összes fontos feladatok jellemző adatokat, és egy gyártási jellegű környezetben, és a teljesítmény tapasztalt összehasonlítása és lekérdezés áruház által jelzett lekérdezések használatával. Néhány hátrányait tapasztal, a lekérdezés áruházzal regressed lekérdezések azonosítása és használható a lekérdezés áruházból beállítás kényszerítése terv (más néven megtervezése rögzítése). Ebben az esetben, véglegesen tartson a kompatibilitási szintet 130, és a lekérdezés áruház által javasolt használni a korábbi lekérdezéstervet.
- Ha szeretné kihasználhatja az új szolgáltatások és funkciók Azure SQL-adatbázis (Ez az SQL Server 2016 fut), de érzékenyek a módosításokat a kompatibilitási szintet 130, utolsó megoldásként által indított érdemes megfontolni sikerült kényszerítése kompatibilitási szintjének biztonsági másolatot az adatbázis módosítása utasítás segítségével a terhelést illő szintre. De először arról, hogy a lekérdezés áruházból terv rögzítése lehetőséget a legjobb megoldást, mert nem használja az 130 alapvetően kapcsolat SQL Server korábbi verziójában funkciók szintjén.
- Ha több adatbázis tartó multitenant alkalmazások, szükségessé válhat az adatbázisok egységes kompatibilitási szintjének biztosítása adatbázisokra; végig a kiépítési logika frissítése régi és kiépített újonnan melyeket. Lehet, hogy az alkalmazás terhelést teljesítményének bizalmas arra, hogy bizonyos adatbázisok futtatja a különböző kompatibilitási szinten, és ezért kompatibilitási szintű konzisztencia bármilyen adatbázis át lehet szükséges, az azonos élmény ügyfeleinek, a minden végig a fal biztosítása érdekében. Figyelje meg, hogy még nem megbízás, akkor igazán attól függ, milyen hatással van az alkalmazás által a kompatibilitási szintet.
- Az utolsó a hivatkozás becslés kapcsolatban, és hasonlóan a kompatibilitási szintet gyártási, a továbblépés előtt módosítása ajánlott tesztelje a termelési terhelést határozza meg, ha az alkalmazás előnyeivel a hivatkozás becslés javítása az új feltételekkel.


## <a name="conclusion"></a>Elfogadásáról


Azure SQL-adatbázis használata minden SQL Server 2016 kiemelés összekapcsolhatók egyértelműen javíthatja a lekérdezés végrehajtások. Ugyanúgy, mint-van! Természetesen minden olyan új funkciót, például a megfelelő végzett kiértékelésre kell elvégezni a pontos feltételeket, amelyek mellett a adatbázis terhelést működik a legjobb határozza meg. Tapasztalat azt mutatja, hogy a legtöbb terhelést várhatóan legalább átlátszó alatt fusson kompatibilitási szintet 130, új lekérdezésfeldolgozást függvények és az új hivatkozás becslés használata közben. Amely said ésszerűen, mindig van néhány kivétel, és hajtsa végre a megfelelő esedékes szorgalom megállapíthatja, hogy mennyi előnnyel jár, hogy a bővítések fontos értékelést. És a lekérdezés áruházból ismét lehet egy nagy segítséget a munkavégzés helye!

Az SQL Azure alakul ki, mint a jövőben számíthat 140 kompatibilitási szintet. Idő szükség, hogy megkezdi hozhat létre a csoportot Mi ez jövőbeli kompatibilitási szintet 140 állapotba kerül, ahogyan azt röviden az itt tárgyalt 130 kompatibilitási mennyire van arra az aktuális.

Most, akkor kezdje nem elfelejt, június 2016 indulva Azure SQL-adatbázis változik az alapértelmezett kompatibilitási szint 120-ra az újonnan létrehozott adatbázisok 130. A felhasználónevek!


## <a name="references"></a>Hivatkozások


- [Adatbázismotort újdonságai](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blog: Lekérdezési áruházból: az adatbázis Borko Novakovic, június 8 2016 által egy nézetbeli adatok hangfelvevővel](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [ALTER adatbázis kompatibilitási szintet (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [ALTER ADATBÁZIS KONFIGURÁCIÓS HATÓKÖRE](https://msdn.microsoft.com/library/mt629158.aspx)

- [Azure SQL-adatbázis V12 130 kompatibilitási szintje](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Az SQL Server-optimalizálás, a lekérdezés tervek 2014-es hivatkozás becslés](https://msdn.microsoft.com/library/dn673537.aspx)

- [Columnstore indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blog: Továbbfejlesztett lekérdezési teljesítmény kompatibilitási szintet 130 Azure SQL-adatbázisban, Alain Lissoir, amelyet május 6 2016-ban](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
