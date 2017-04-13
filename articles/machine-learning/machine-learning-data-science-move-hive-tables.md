<properties
    pageTitle="Létrehozása és adatok betöltése Blob-tárolóhoz struktúra tábláinak |} Microsoft Azure"
    description="Struktúra táblázatok létrehozása és blob-táblázatok struktúra, hogy az adat betöltése"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Létrehozása és adatok betöltése struktúra tábláinak Azure blob-tárolóhoz

Ez a témakör bemutatja az általános struktúra táblázatok létrehozása és adatok betöltése Azure blob-tárolóhoz struktúra-lekérdezéseket. Néhány is útmutatást a szétválasztás struktúra táblák és használatáról a optimalizált sor oszlopos (ORC) formázás lekérdezési teljesítmény javítása érdekében.

Ez a **menü** ismertetik, hogyan lehet adatok ingest cél környezetekben, ahol tárolt és feldolgozása közben a csapat adatok tudományos folyamat (TDSP) a az adatokat tartalmazó témakörökre mutató hivatkozásokat tartalmaz.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy:

* Az Azure tároló fiókot hozta létre. Ha utasításokat, [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)témakörben találhatók. 
* A HDInsight szolgáltatáshoz a testre szabott Hadoop fürtre kiépítve.  Ha a utasításokat van szüksége, olvassa el a [fejlett analitikai az Azure hdinsight szolgáltatáshoz Hadoop testreszabása fürt](machine-learning-data-science-customize-hadoop-cluster.md).
* A fürthöz, engedélyezett távelérési bejelentkezve, és a Hadoop parancssori konzol megnyitni. Ha a utasításokat van szüksége, olvassa el a [hozzáférést a címsor csomópontot, Hadoop fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Töltse fel az adatok Azure blob-tárolóhoz
Ha létrehozott egy Azure virtuális gép [az Azure virtuális gép fejlett analitikai beállítása](machine-learning-data-science-setup-virtual-machine.md)a utasításokat követve, a parancsprogram kell letöltötte az *C:\\felhasználók\\\<felhasználónév\>\\dokumentumok\\adatok tudományos parancsfájlok* könyvtár a virtuális gépen. Ezek a struktúra lekérdezések csak szükség csatlakoztatása a saját adatszerkezet és Azure blob tárolási konfigurációt Beküldési készen áll a megfelelő mezőket.

Feltételezzük, hogy struktúra táblázatok adatait **nem tömörített** táblázatos formátumban, és hogy az adatok feltöltötte, az alapértelmezett (vagy egy további) tároló a Hadoop fürt által használt tárterület-fiók.

Ha a **Következőt: Taxi utazás adatok**gyakorlat szeretne kell:

- **Töltse le** a 24 [következőt: Taxi utazás](http://www.andresmh.com/nyctaxitrips) adatfájlokat (12 utazás fájlok és 12 jegy ára fájlokat),
- **bontsa ki az** összes fájl CSV-fájlba, majd
- **Töltse fel** őket a Azure tároló fiókot az eljárás által létrehozott alapértelmezett (vagy megfelelő tárolóban) a [Microsoft Azure hdinsight szolgáltatáshoz Hadoop testreszabása fürt speciális Analytics folyamat és technológia](machine-learning-data-science-customize-hadoop-cluster.md) témakörben tagolt. A folyamat a .csv fájl feltöltése a tárterület-fiók a alapértelmezett tároló ezen a [lapon](machine-learning-data-science-process-hive-walkthrough.md#upload)található.


## <a name="submit"></a>Módját struktúra lekérdezések

Struktúra lekérdezések használatával küldhetők el:

1. [Struktúra lekérdezések Hadoop parancssori keresztül Hadoop fürt headnode elküldése](#headnode)
2. [A struktúra szerkesztő struktúra lekérdezés elküldése](#hive-editor)
3. [Azure PowerShell-parancsait struktúra lekérdezés elküldése](#ps)

Struktúra lekérdezések SQL-szerű. Ha ismeri a SQL, akkor azt tapasztalhatja az [SQL-felhasználók Cheat lap struktúra](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) hasznos.

Struktúra lekérdezés elküldésekor is megadhatja, hogy a cél struktúra lekérdezésből a kimenet képernyőn vagy a központi csomópontra helyi fájl vagy egy Azure blob legyen.


###<a name="headnode"></a>1. a struktúra lekérdezéseket Hadoop parancssori keresztül Hadoop fürt headnode küldhet

Ha a struktúra lekérdezés összetettebb, elküldené közvetlenül az a központi csomópontot, a Hadoop fürt általában vezet mint elküldené a struktúra szerkesztő vagy Azure PowerShell parancsfájlok gyorsabb kapcsolja.

Jelentkezzen be az a központi csomópontot a Hadoop fürt, nyissa meg a Hadoop parancssorban az asztalon, a fej csomópont, és adja meg a parancs `cd %hive_home%\bin`.

Háromféleképpen struktúra lekérdezések a Hadoop parancssorból elküldése:

* közvetlenül
* .hql fájlok használata
* a struktúra parancs konzolban

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Közvetlenül a Hadoop parancssori struktúra lekérdezések nyújt.

Futtatását is lehetővé teszi például parancs `hive -e "<your hive query>;` egyszerű struktúra lekérdezések közvetlenül a Hadoop parancssori küldhetik. Íme egy példa, ahol a piros mezőben ismertet a struktúra elküld parancsot, és a zöld mező ismertet a kimenet a struktúra lekérdezésből.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Lekérdezések struktúra .hql fájlok küldése

Ha a struktúra lekérdezés bonyolultabb, és több sort tartalmaz, Szerkesztés parancsot, vagy a struktúra parancs konzol lekérdezések nem célszerű. A központi csomópontot a Hadoop fürt szövegszerkesztőben segítségével a központi csomópont helyi könyvtárában található .hql fájl mentése a struktúra lekérdezések egy alternatív megoldás. A .hql fájlban a struktúra lekérdezés használatával küldhetők, majd a `-f` argumentum a következők szerint:

    hive -f "<path to the .hql file>"

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Folyamatban állapota képernyőn nyomtatás struktúra lekérdezések mellőzése**

Alapértelmezés szerint a Hadoop parancssori, struktúra lekérdezés elküldése után a térkép/kicsinyítés feladat előrehaladását kinyomtatta a képernyőn. A képernyő nyomtatása a térkép/kicsinyítés projekt előrehaladásának mellőzése, argumentumot is használhatja `-S` ("S" nagybetűs) a parancsot a sor az alábbiak szerint:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Struktúra lekérdezések struktúra parancs konzolban nyújt.

Először is megadhat a struktúra parancs konzol parancs futtatásával `hive` a Hadoop parancssori, és küldje struktúra lekérdezések struktúra parancs konzolban. Lássunk egy példát. Ebben a példában a piros dobozt két oszlopban adja meg a struktúra parancs konzol használt parancsok és a struktúra lekérdezés elküldése struktúra parancs konzolban, a kurzor kiemelése. A zöld mező kiemeli a kimenet a struktúra lekérdezésből.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

Az előző példákban közvetlenül kimeneti a struktúra lekérdezés eredményében a képernyőn. Is írhat a kimeneti fájl helyi a központi csomópontra, illetve az Azure blob. Egyéb eszközök segítségével majd ezek alapján további elemzésnek struktúra lekérdezések kimenetét.

**Kimeneti struktúra lekérdezés eredménye egy helyi fájl.**

Kimeneti struktúra lekérdezés eredménye egy helyi könyvtár a központi csomópontra, be kell elküldés a struktúra a Hadoop parancssorban az alábbi képlettel történik:

    hive -e "<hive query>" > <local path in the head node>

A következő példában a struktúra lekérdezés eredménye egy fájlba írt `hivequeryoutput.txt` címtárban `C:\apps\temp`.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Kimeneti struktúra lekérdezési eredmények egy Azure blob**

A struktúra lekérdezés között az Azure blob, a Hadoop fürt alapértelmezett tárolóban is készíthető. A struktúra lekérdezés a következőképpen történik:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

A következő példában a struktúra lekérdezés eredménye egy blob könyvtárra írt `queryoutputdir` a Hadoop fürt alapértelmezett tárolóban. Ebben az esetben csak szeretne adja meg a könyvtár nevét a blob-név nélkül. Ha könyvtár és a blob nevét, például ad elő hiba `wasb:///queryoutputdir/queryoutput.txt`.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Az alapértelmezett tároló Azure tároló Intézővel Hadoop-fürt nyit meg, ha megjelenik a kimenet a struktúra lekérdezés, az alábbi ábrán látható módon. A szűrő (piros téglalap kiemelt) alkalmazása csak beolvashatja a megadott betűkkel a nevek blob.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. a struktúra szerkesztő struktúra lekérdezés elküldése

A lekérdezés Console (struktúra szerkesztése) is használhatja az űrlap *URL-cím beírásával https://&#60; Hadoop fürt neve >.azurehdinsight.net/Home/HiveEditor* be egy webböngészőben. Kell-e jelentkezve a lásd: a konzol, és így a Hadoop fürt hitelesítő adatok itt kell.

###<a name="ps"></a>3. a struktúra lekérdezések Azure PowerShell-parancsokkal elküldése

A PowerShell használatával struktúra lekérdezéseket küldhet. Című témakör nyújt tájékoztatást [nyújt a struktúra feladatok PowerShell használatával](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Struktúra adatbázis és tábla létrehozása

A struktúra lekérdezések megosztott a [tárházba Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) , és onnan letölthető.

Az alábbiakban a struktúra lekérdezést, amely egy struktúratáblával hoz létre.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Az alábbiakban a ismerteti a mezőket, amelyeket a beépülő modul kell és más beállításokat:

- **& #60; az adatbázis neve >**: az adatbázist szeretne létrehozni, a nevére. Ha csak szeretne használni az alapértelmezett adatbázist, a lekérdezés *... adatbázis létrehozása* hiányzik is.
- **& #60; a táblázat neve >**: a táblázatot, amelyet létre szeretne hozni az adott adatbázis nevét. Az alapértelmezett adatbázis használni kívánt, ha a tábla is közvetlenül elé *& #60; a táblázat neve >* nélkül & #60; az adatbázis neve >.
- **& #60; a mező elválasztó >**: az adatok fájl fel kell tölteni a struktúra táblázat határoló elválasztó karaktert.
- **& #60; elválasztó vonal >**: elválasztó karaktert, amely az adatfájl sorokat.
- **& #60; tárolási helye >**: az Azure tárolási helye a struktúra táblák az adatok mentéséhez. Ha nem ad meg *HELYÉT és #60; tárolási helye >*, az adatbázist és táblákat tárolódnak *struktúra/raktári/* könyvtár az alapértelmezett tárolóban alapértelmezés szerint a struktúra fürt. Ha szeretne megadni a tárolási helye, a tárolóhely azt az adatbázis és tábla alapértelmezett tárolóban kell. Ezen a helyen van a fürt formátuma az alapértelmezett tároló viszonyított helyeként elé *"wasb: / / / és a #60; 1 könyvtár > /"* vagy *"wasb: / / / és a #60; 1 könyvtár > / & #60; 2 könyvtár > /"*stb. A lekérdezés végrehajtása, után a relatív könyvtárak az alapértelmezett tárolóban jönnek létre.
- **TBLPROPERTIES("Skip.Header.line.Count"="1")**: Ha az adatfájl egy élőfej sort tartalmaz, van-e ez a tulajdonság **a végén** a *tábla létrehozása* lekérdezés. Egyéb esetben fejlécsora töltődik be a táblázatba rekordként. Ha az adatfájl nincs élőfej vonal, ebben a konfigurációban nem hagyható a lekérdezésben.

## <a name="load-data"></a>Adatok betöltése struktúra táblázatokhoz
Az alábbiakban a struktúra lekérdezés betöltő adatok struktúra táblába.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; elérési útját blob-adatok >**: fel kell tölteni a struktúratáblával blob fájl, az alapértelmezett tárolóban a HDInsight Hadoop fürt a *& #60; elérési útját blob-adatok >* formátumban kell *"wasb: / / / és a #60; ez tárolóban címtár > / & #60; blob fájlnév >"*. A HDInsight Hadoop fürt egy további tárolóban is lehet a blob-fájlt. Ebben az esetben *& #60; elérési útját blob-adatok >* formátumban kell *"wasb: / / & #60; tároló name>@&#60;storage fióknév >.blob.core.windows.net/ & #60; blob fájlnév >"*.

    >[AZURE.NOTE] A blob adatokat struktúratáblával fel kell tölteni a el szeretné helyezni az alapértelmezett vagy a tárterület-fiók a Hadoop fürt további tároló tartalmaz. Ellenkező esetben a *BETÖLTÉS* lekérdezést meghiúsul panaszos, hogy nem fér hozzá az adatokat.


## <a name="partition-orc"></a>Speciális témakörök: táblázat és a tár struktúra adatokkal ORC particionálva

Az adatok túl nagy, a táblázat szétválasztás esetén csak a táblázat néhány partíciót áttekinthetőbbé kell lekérdezések előnyös. Például érdemes lehetővé teszi a naplóadatokat a webhely partition dátum szerinti.

Nemcsak a szétválasztás struktúra táblázatok, érdemes emellett előnyös struktúra adattárolásra optimalizált sor oszlopos (ORC) formátumban. ORC formázás kapcsolatos további tudnivalókért olvassa el a <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">ORC használata fájlok javítja a teljesítményt, amikor struktúra olvasása, ír, és adatok feldolgozása</a>című témakört.

### <a name="partitioned-table"></a>Particionált táblázat
Az alábbiakban a struktúra lekérdezést, amely egy particionált táblázatot hoz létre, és betölti az adatokat a mikrofonba.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Lekérdezés particionált táblázatok, célszerű annak **kezdete** vehet fel a partíciót feltétel a `where` ehhez záradék javítja jelentősen keresés hatékonyságát.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Struktúra adattárolásra ORC formátumban

Nem közvetlenül be fogja adatok blob-tárolóból ORC formátumban tárolt struktúra táblákba. A lépések, amelyek a végrehajtandó betöltése a következők adatok az Azure BLOB-struktúra táblák ORC formátumban tárolja.

Hozzon létre egy külső táblában **Tárolt AS TEXTFILE** betöltés adatok és a táblázat blob-tárolóhoz.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Hozzon létre egy belső táblázatot ugyanarra a sémára, az 1, az azonos mező elválasztó a külső táblázatként, és a struktúra adattárolásra ORC formátumban.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Jelölje ki az adatokat a külső táblázat lépés: 1, és abba szúrja be a ORC tábla

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Ha a TEXTFILE tábla *& #60; az adatbázis neve >. & #60; a külső textfile táblázat neve >* , a 3 partíciók, amelynek a `SELECT * FROM <database name>.<external textfile table name>` parancs kiválasztása a partíciót változó mezőként egy visszaadott adatok megadása. A Beszúrás a *& #60; az adatbázis neve >. & #60; ORC táblanév >* óta nem sikerül *& #60; az adatbázis neve >. & #60; ORC táblázat neve >* nem rendelkezik a partíciót változó a táblázat séma mezőként egy. Ebben az esetben kell kifejezetten jelölje ki a mezőket szeretné íratni *& #60; az adatbázis neve >. & #60; ORC táblanév >* az alábbi képlettel történik:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Biztonságos húzhatja a *& #60; a külső textfile táblázat neve >* esetén az alábbi lekérdezés használata után az összes adat beszúrta *& #60; az adatbázis neve >. & #60; ORC táblanév >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Leírt lépések végrehajtását követően ezt az eljárást, rendelkeznie kell a használatra kész ORC formátumban adatokat tartalmazó táblázat.  
