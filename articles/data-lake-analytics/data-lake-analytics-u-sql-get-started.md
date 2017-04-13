<properties
   pageTitle="Adatok tó eszközeivel for Visual Studio U – SQL-parancsfájlok kidolgozása |} Azure"
   description="Megtudhatja, hogy miként telepítheti az tó Adateszközök Visual Studio, fejlesztésével és vizsgálat U-SQL nyelvben parancsfájlok. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Oktatóprogram: Azure adatok tó Analytics U – SQL nyelv használatába

U-SQL nyelvben egy nyelvet az összes adatot a bármely skála feldolgozása, a saját kód kifejező power SQL előnyeiről egyesíti. U – SQL méretezhető elosztott lekérdezés képesség lehetővé teszi, hogy hatékony adatelemzés az áruházban és relációs tárolja, például az Azure SQL-adatbázis keresztül.  Lehetővé teszi a folyamat strukturálatlan adatokat a séma alkalmazásával olvasási, egyéni logika és UDF's beszúrása és ahhoz, hogy miként nyomtatott szabályozni kell végrehajtani a méretezés hogyan bővítési tartalmazza. A tervezés filozófiai mögött U-SQL nyelvben kapcsolatos további tudnivalókért olvassa el a [Visual Studio blog közzé](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Bizonyos különbségek vannak az ANSI SQL- vagy az SQL-T. VÁLASSZA például a kulcsszavak például kell lenniük, nagybetűs.

Adatait a típus rendszer és kifejezés nyelvi select záradék hol találhatók a predikátumok stb C# belül.
Ez azt jelenti, hogy az adattípusok C# típusok és az adattípusok C# NULL szemantikáját használja, és belül egy predikátumok összehasonlító műveleteket hajtsa végre a C# szintaxis (például egy == "élőlá").  Ez azt is jelenti, hogy értékei teljes .NET-objektumok, így egyszerűen bármely módszert vonatkozni fognak az objektum (például "f o o o". Split(' ')).

További tudnivalókért olvassa el a [U – SQL referencia](http://go.microsoft.com/fwlink/p/?LinkId=691348)című témakört.

###<a name="prerequisites"></a>Előfeltételek

Meg kell adnia a [oktatóanyag: adatok tó eszközeivel for Visual Studio U – SQL-parancsfájlok kidolgozása](data-lake-analytics-data-lake-tools-get-started.md).

Az oktatóprogram során adatok tó Analytics feladatot a következő U-SQL nyelvben forgatókönyvvel futtatta:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Ez a parancsfájl átalakítási lépések nem tartozik. Azt beolvassa a forrásfájl **SearchLog.tsv**nevű, schematizes, és a sorhalmazon exportálja az **SearchLog-első-u – sql.csv**nevű fájl.

Figyelje meg az időtartam mező adattípusa mellett a kérdőjel ikonra. Ez azt jelenti, lehet, hogy az időtartam mező üres.

Egyes fogalmak és a parancsfájl található kulcsszavak:

- **Sorhalmazon változók**: minden lekérdezési kifejezés, amelyet sorhalmazt változó tevékenységekhez hozzárendelhető. U-SQL nyelvben követi a T-SQL nyelvben változó elnevezési mintát, például **@searchlog** betűs.
    Megjegyzés: a hozzárendelés mellőzéséhez végrehajtása. Csupán a kifejezés nevek és lehetőséget nyújt a felhalmozódásának összetettebb kifejezések.
- **KIBONTÁSA** lehetőséget nyújt a séma definiálása a olvasható. A séma oszlop nevét és C# típus nevének pár oszloponként határozza meg. Akkor használja a egy úgynevezett **készülék**, például **Extractors.Tsv()** tsv fájlok kibontásához. Egyéni vagyis fejleszthet.
- **Kimeneti** sorhalmazt tart, és azt serializes. A Outputters.Csv() kimeneti egy vesszővel tagolt fájlhoz, a megadott helyre. Egyéni Outputters is készíthet.
- Figyelje meg a két elérési utak is relatív elérési utak. Abszolút elérési út is használhatja.  Ha például

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Abszolút elérési utat kell használnia az tároló fiókok csatolt fájlokat szeretné elérni.  A csatolt Azure tárterület-fiókjában tárolt fájlok szintaxisa a következő:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Nyilvános BLOB vagy nyilvános tárolók hozzáférési engedélyek Azure Blob-tárolóban jelenleg nem támogatott.

## <a name="use-scalar-variables"></a>Skaláris változók használata

Használhatja skaláris változók, valamint a parancsprogram-karbantartási könnyebb. Az előző U-SQL nyelvben parancsprogramot is kell írni az alábbiak szerint:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Sorhalmazok átalakítása

Használja a **Kijelölés** átalakítása sorhalmazok:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

A WHERE záradékban használt [C# logikai kifejezés](https://msdn.microsoft.com/library/6a71f45d.aspx). A C# nyelvén saját kifejezések és a függvények is használhatja. Logikai Kötőszóval (jelölőnégyzetből) és (ORs) disjunctions kombinálásával összetettebb szűrés is végezhet.

A következőt használja a DateTime.Parse() módszer, és egy együtt.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Figyelje meg, hogy üzemel-e a második lekérdezés eredménye az első sorhalmaz, és így az eredmény egy kialakítási a két szűrő. Szintén felhasználhatja egy változó nevét, és a neveket a lexically tükrözik.

## <a name="aggregate-rowsets"></a>Összesítő sorhalmazok

Az SQL-U biztosít a már jól ismert **ORDER BY**, **GROUP BY** és összesítéseket.

Az alábbi lekérdezés megtalálja a teljes időtartama / régió, és a felső sorrendben 5 időtartamok majd exportálja.

Az SQL-U sorhalmazok nem őrzi meg a következő lekérdezés a sorrendjüket. Tehát olyan eredménye megrendeléséhez kell ORDER BY hozzáadása a kimenő utasítás alább látható módon:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Az SQL-U ORDER BY záradék rendelkezik lehet kombinálni a FÁJLLEHÍVÁSI záradék, jelölje be a kifejezésben.

Záradék U-SQL nyelvben PROBLÉMÁKAT korlátozhatja a csoportok, amelyek a HAVING feltételnek felelnek meg a kimenet használható:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Csatlakozás adatok

U-SQL nyelvben közös illesztés operátorok, például a belső ILLESZTÉS, balra/jobbra/teljes külső ILLESZTÉS, FÉLIG ILLESZTÉS, ha be szeretne kapcsolódni, nem csak a táblázatok, de a bármely sorhalmazok (azokat is készült fájlok) tartalmaz.

A következő parancsfájl a searchlog egy reklámot benyomást napló a csatlakozik, és ad velünk a hirdetések, a lekérdezési karakterlánc az egy adott dátum.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


Csak akkor támogatja az ANSI-kompatibilis illesztés szintaxisára U-SQL nyelvben: Rowset1 ILLESZTÉS Rowset2 tovább predikátumok. A régi a Rowset1, ahol Rowset2 predikátumok szintaxisa nem támogatott.
Az illesztés predikátumok működnie kell az egyenlő illesztéssé és kifejezés nélkül. Kifejezés használata típust szeretné, ha hozzáadása előző sorhalmazt select záradékban. Ha szeretne tenni más összehasonlítási, áthelyezheti a WHERE záradék be.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Adatbázisok, táblázat értékű függvények, nézetek és táblázatok létrehozása

U-SQL nyelvben lehetővé teszi, hogy adatokat felhasználhatja egy adatbázist és -sémafájlok környezetében. Így nem kell mindig olvashatja őket írni vagy fájlokat.

Minden U-SQL nyelvben parancsfájl alapértelmezett környezete fut egy alapértelmezett adatbázis (mester) és az alapértelmezett séma (DBO). A saját adatbázist és/vagy a séma hozhat létre. A környezet beállításához használja az **használata** utasítás módosítsa a környezetét.


### <a name="create-a-table-valued-function-tvf"></a>Táblázat értékű függvény (TVF) létrehozása

Az előző U-SQL nyelvben parancsfájl, ismételni kivonat ugyanazon forrásfájl olvasásakor. U-SQL nyelvben táblázat értékű függvény lehetővé teszi az adatok későbbi használathoz beágyazására.   

A következő parancsfájl létrehoz egy *Searchlog()* nevű az alapértelmezett adatbázis és -sémafájlok TVF:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

A következő parancsfájl megtudhatja, hogy hogyan a meghatározott az előző parancsfájl TVF használata:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Nézetek létrehozása

Ha csak egy lekérdezés-kifejezés, amely absztrakt szeretne, és nem szeretne paraméterezni azt, létrehozhat egy táblázat értékű függvény helyett egy nézetet.

A következő parancsfájl *SearchlogView* nevű az alapértelmezett adatbázisból és a séma nézet létrehozása:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

A következő parancsfájl bemutatja az definiált nézet:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Táblázatok létrehozása

Hasonlóan relációs adatbázisból táblázat U-SQL nyelvben teszi lehetővé, hogy hozzon létre egy táblázatot egy előre definiált séma, vagy hozzon létre egy táblázatot, és előállítani a lekérdezést, amely a táblázat (más néven hozzon létre táblázat AS kijelölése vagy CTAS) feltölti az sémát alapján.

A következő parancsfájl hozzon létre egy adatbázist, és a két tábla:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Lekérdezési táblázatok

A táblák (hozza létre az előző parancsfájl) ugyanúgy lekérdezésként, az adatfájlok keresztül lekérdezés. Helyett használja a kivonat sorhalmazt, akkor most már csak hivatkozhat a táblázat neve.

A korábban használt átalakítás parancsfájl módosult olvasható a tábla:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Figyelje meg, hogy jelenleg nem futtatható a válassza ki a azonos parancsfájl, mint a parancsfájl táblájához táblázatot kezdeményezik.


##<a name="conclusion"></a>Elfogadásáról

Az oktatóprogram alá tartozó része csak egy kis U-SQL nyelvben. Ebben az oktatóanyagban körét, mert azt nem terjed ki minden, például:

- Használata CROSS kicsomagolni karakterláncok, tömbök és térképek részei sorokká vonatkozik.
- Működniük particionált adathalmazok (fájl beállítása és particionált táblázatok).
- A felhasználó által definiált operátorok, például vagyis, outputters, processzorok, felhasználó által definiált aggregators C# fejlesztése.
- Az SQL-U ablakok elrendezése függvényt használhatja.
- Kezelheti a nézetek, a táblázat értékű függvények és a tárolt eljárásokat U – SQL-kódot.
- Tetszőleges egyéni kód futtatható a feldolgozás csomópontot.
- Csatlakozás SQL Azure-adatbázisok és federate lekérdezések őket, és az adatok U-SQL nyelvben és Azure adatok tó.

## <a name="see-also"></a>Lásd még:

- [Microsoft Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)
- [Az SQL-U parancsfájlok tó Adateszközök for Visual Studio kidolgozása](data-lake-analytics-data-lake-tools-get-started.md)
- [U-SQL nyelvben ablak függvényeivel Azure adatok tó Analytics feladatokhoz](data-lake-analytics-use-window-functions.md)
- [Figyelésére és Azure portálon Azure adatok tó Analytics feladatok hibaelhárítása](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Ossza meg velünk véleményét

- [A szolgáltatás kérelem küldése](http://aka.ms/adlafeedback)
- [Segítség kérése a fórumokon](http://aka.ms/adlaforums)
- [Visszajelzés küldése a U-SQL nyelvben](http://aka.ms/usqldiscuss)
