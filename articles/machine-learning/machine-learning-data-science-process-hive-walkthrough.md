<properties
    pageTitle="A csoportwebhely adatok tudományos folyamat működését: használata Hadoop fürtök |} Microsoft Azure"
    description="A csapat adatok tudományos folyamat használatának-végpontok közötti forgatókönyv egy HDInsight Hadoop fürthöz létre és helyezhetnek üzembe a nyilvánosan elérhető adatkészlet használatával modell alkalmazásával."
    services="machine-learning,hdinsight"
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>A csoportwebhely adatok tudományos folyamat működését: HDInsight Hadoop fürt használatával

Az útmutató a [Csapat adatok tudományos folyamat (TDSP)](data-science-process-overview.md) -végpontok közötti helyzetben tárolja, és ez a funkció a [Következőt: Taxi utakat](http://www.andresmh.com/nyctaxitrips/) nyilvánosan elérhető adatkészlet adatbázismodellbe adatainak és lefelé az adatokat minta használatával egy [Azure hdinsight szolgáltatáshoz Hadoop fürt](https://azure.microsoft.com/services/hdinsight/) használjuk. Az adatok modellek Azure gépi tanulási bináris és multiclass osztályozás és a regressziós prediktív tevékenységek kezelése a készültek.

Útmutató mutatja, hogy hogyan kezelje a HDInsight Hadoop fürt használatával adatkezelési hasonló esethez nagyobb (1 adjon) adatkészletet, akkor olvassa el a [Csapat adatok tudományos folyamat – használatával Azure hdinsight szolgáltatáshoz Hadoop fürt 1 TB adatkészletet](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Akkor is elvégezheti a műveleteket a használja az 1 TB adatkészlet forgatókönyv bemutatott egy IPython-Jegyzetfüzet használata. A felhasználók, akik próbálja meg ezt a megközelítést kívánja nézze át a [struktúra ODBC-kapcsolaton keresztül Criteo forgatókönyv](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) témakört.


## <a name="dataset"></a>Leírás a következőt: Taxi utakat adatkészlet

A következőt: Taxi utazás adatok körülbelül 20GB-nyi tömörített vesszővel elválasztott értékek (CSV) fájlok (nem tömörített ~ 48GB), minden út kifizetett 173 milliónál egyes utakat és a díjakat. Utazás rekordokat tartalmaz, a felvétel és Gyűjtőtár helyét, és időt, anonimizált támadó szoftver (illesztőprogram) licenc száma és medallion (taxi tartozó egyedi azonosító). Az adatok minden utakat bemutatja a 2013-ben és az egyes az a következő két adatkészleteket megadva:

1. A "trip_data" CSV-fájlok utazás részleteket, például a utasok, felvétel és dropoff pontok, út időtartama és az utazás hossza tartalmazzák. Íme néhány példa rekordot:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. A "trip_fare" CSV-fájlok a jegy ára kifizetett minden út, például a fizetés típusát, jegy ára összege, emelt díjas és adót, tippek és autópályadíjak, és a végösszeget kifizetett részletek tartalmazzák. Íme néhány példa rekordot:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Bekapcsolódás utazás egyedi billentyűvel\_adatokat, és utazás\_jegy ára tevődik össze a mezők: medallion, támadó szoftver\_engedély és a felvétel\_datetime.

Úgy juthat az összes részletes adott utazás fontos, három billentyűkkel való elegendő: a "medallion", "ellophatja\_licenc" és "felvétel\_datetime".

Néhány további információra kíváncsi, az adatok azt ismertetik, amikor azt tárolja őket struktúra táblákba hamarosan.

## <a name="mltasks"></a>Példák a szövegbevitel feladatok
Amikor a elemzésében adatokat, annak megállapítása, az előrejelzések azt szeretné, hogy milyen közelít alapján segítséget nyújt a jelezhető, hogy szüksége lesz, ha meg szeretné jeleníteni a folyamatban lévő feladatokat.
Íme három példa azt címét, akinek összetétel alapul az útmutató az előrejelzési problémákhoz a *Tipp\_összeg*:

1. **Bináris besorolás**: előrejelzésére e vagy sem ezt a tippet fizettek utazása, azaz egy *Tipp\_összeg* nagyobb, mint 0 Ft értéke egy szám pozitív példa, miközben egy *Tipp\_összeg* 0 Ft van a negatív példa.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiclass besorolás**: előre kifizetett az utazás tipp összegek a tartomány. Azt osztása a *Tipp\_összeg* öt intervallumokat vagy osztályok:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressziós feladatot**: a kifizetett utazása tipp mennyiségét előrejelzésére.  


## <a name="setup"></a>Egy HDInsight Hadoop-fürthöz fejlett analitikai beállítása

>[AZURE.NOTE] Ez általában a egy **felügyeleti** feladatot.

Beállíthatja a fejlett analitikai ellátott egy HDInsight fürthöz három lépésben az Azure környezetben:

1. [Tárterület-fiók létrehozása](../storage/storage-create-storage-account.md): ehhez a fiókhoz tároló Azure Blob-tárolóban lévő adatok tárolására szolgál. Az adatok HDInsight fürt használt is itt találhatók.

2. [A speciális Analytics folyamat és technológia testreszabása Azure hdinsight szolgáltatáshoz Hadoop fürt](machine-learning-data-science-customize-hadoop-cluster.md). Ebben a lépésben létrehoz az Azure hdinsight szolgáltatáshoz Hadoop 64 bites Anaconda Python 2.7 fürtre csomópontjait telepítve. Nincsenek két fontos lépést kell tartania, miközben a HDInsight fürt testreszabása.

    * Ne feledje, hogy a tárhely fiók létrehozásakor a HDInsight fürthöz lépés: 1 létrehozott, amelyre a hivatkozás. Ehhez a fiókhoz tároló belül a fürt feldolgozott adatok eléréséhez használják.

    * A csoport létrehozását követően távoli hozzáférés engedélyezése a fürt központi csomópontot. Nyissa meg a **beállítás** lapon, majd kattintson a **Távoli engedélyezése**. Ebben a lépésben adja meg a távoli bejelentkezéshez használt felhasználói hitelesítő adatokkal.

3. [Az Azure gépi tanulási munkaterület létrehozása](machine-learning-create-workspace.md): Ez Azure gépi tanulási munkaterület gépi tanulási modellek létrehozásához használja. A feladat befejezése az eredeti adatok feltárása után, és használja a HDInsight fürt mintavételnél lefelé címzettjei.

## <a name="getdata"></a>Az adatok beolvasása egy nyilvános adatforrások

>[AZURE.NOTE] Ez általában a egy **felügyeleti** feladatot.

A [Következőt: Taxi utakat](http://www.andresmh.com/nyctaxitrips/) adatkészlet nyilvános helyéről, akkor előfordulhat, hogy használja az [Adatok áthelyezése, és az Azure Blob-tárolóhoz](machine-learning-data-science-move-azure-blob.md) ismertetett eljárások valamelyikét másolhatja az adatokat a számítógépen.

Itt azt ismertetik használatával hogyan AzCopy adatokat tartalmazó fájlok átvitele. Töltse le és telepítse AzCopy kövesse a képernyőn megjelenő utasításokat a [– Első lépések a AzCopy parancssori segédprogram](../storage/storage-use-azcopy.md).

1. A parancssorablakban hiba az alábbi AzCopy parancsokat, amely a kívánt célhelyre *< path_to_data_folder >* lecserélve:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Ha befejezte a másolás, 24 zip-fájlok összesen kiválasztott adatok mappában vannak. Bontsa ki az ugyanazt a címtárhoz letöltött fájlok helyi számítógépre. Jegyezze fel annak a mappának, ahol a nem tömörített fájlok találhatók. Ez a mappa lesz a továbbiakban: a *< elérési út\_való\_unzipped_data\_fájlok\> * következik van.


## <a name="upload"></a>Töltse fel az adatokat a alapértelmezett tároló Azure hdinsight szolgáltatáshoz Hadoop fürt

>[AZURE.NOTE] Ez általában a egy **felügyeleti** feladatot.

Az alábbi parancsok AzCopy cserélje ki a következő paraméterek a tényleges értékek, amelyek a megadott a Hadoop fürt létrehozásakor, és az adatfájlok kicsomagolás.

* ***& #60; path_to_data_folder >*** a címtárban (együtt elérési út), amelyek a kicsomagolt adatfájlok tartalmazzák a számítógépen  
* ***& #60; tároló fióknév Hadoop fürt >*** a HDInsight fürt társított tároló fiók
* ***& #60; az alapértelmezett tároló Hadoop fürt >*** a fürt által használt alapértelmezett tárolóját. Ne feledje, hogy az alapértelmezett tároló neve általában a fürthöz nevét. Ha például a csoport neve "abc123.azurehdinsight.net", ha az alapértelmezett tároló abc123 egy.
* ***& #60; tároló fiókkulcs >*** a fürt által használt tárterület-fióknak a billentyűt

A parancssorba vagy a számítógépen a Windows PowerShell ablak a következő parancsokat két AzCopy.

Ez a parancs az alapértelmezett tárolóban a Hadoop fürt ***nyctaxitripraw*** címtár feltölti az út-adatokat.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Ez a parancs az alapértelmezett tárolóban a Hadoop fürt ***nyctaxifareraw*** címtár feltölti jegy ára adatokat.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Az adatok célszerű most Azure Blob-tárolóhoz, és készen áll a HDInsight fürt belül felhasználandó.

## <a name="#download-hql-files"></a>Jelentkezzen be a fő csomópont Hadoop fürt és és a felkészülés felderítő adatelemzés

>[AZURE.NOTE] Ez általában a egy **felügyeleti** feladatot.

A központi csomópontot a fürt felderítő adatelemzés és a le a példákat talál arra az adatok eléréséhez az [Access a címsor csomópontot, Hadoop fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode)leírt eljárást követve.

Az útmutató azt elsősorban lekérdezésekkel [struktúra](https://hive.apache.org/), egy SQL-szerű lekérdezésnyelv írt előzetes adatok explorations végrehajtásához. A struktúra lekérdezések .hql fájlt tárolja. Azt majd lefelé minta modellek készítéséhez használható belül Azure gépi tanulási ezeket az adatokat.

A fürt Felkészülés felderítő adatelemzés, akkor töltse le a .hql fájlokat a megfelelő struktúra parancsfájlokat [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) helyi könyvtár (C:\temp) a központi csomópontra tartalmazó. Ehhez nyissa meg a **parancssort** az a központi csomópontot a fürt belül, és a következő két parancsok kiadását:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Ezek a parancsok két letölti az útmutató a fő csomópont helyi könyvtár ***C:\temp & #92;*** szükséges .hql fájlokat.

## <a name="#hive-db-tables"></a>Hozzon létre a struktúra adatbázist és táblákat, particionálva havonként

>[AZURE.NOTE] Ez általában a egy **felügyeleti** feladatot.

Azt most már készen áll a következőt: taxi adatkészlet struktúra táblákat létrehozni.
Az a központi csomópontot a Hadoop fürt nyissa meg a ***Hadoop parancssori*** a központi csomópont az asztalon, és adja meg a a struktúra könyvtár a parancs megadásával

    cd %hive_home%\bin

>[AZURE.NOTE] **Minden struktúra parancs futtatása az útmutató a fenti struktúra lévő / könyvtár ablakot. Ez lesz gondoskodik a problémák megoldásához elérési út automatikusan. Használjuk adatokra "Struktúra címtár kérdés", "struktúra bin / címtár kérdés", és a "Hadoop parancssori" szót azonos értelemben a jelen útmutató.**

A struktúra címtár parancssorból a következő parancsot a Hadoop parancs sort szúr be a központi csomópont struktúra adatbázist és táblákat létrehozni a struktúra lekérdezés elküldése:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Az alábbiakban tartalmát a ***C:\temp\sample\_struktúra\_létrehozása\_db\_és\_tables.hql*** fájl, amely struktúra adatbázis ***nyctaxidb*** és táblázatok ***utazás*** és ***jegy ára***hoz létre.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Ez a struktúra parancsfájl hozza létre a két tábla:

* a "út" tábla utazás adatait tartalmazza az egyes menethelyzetben (részletek, felvételi idő, utazást távolság és az idő)
* a "jegy ára" tábla jegy ára adatait tartalmazza (jegy ára összege, tipp: összeg, autópályadíjak és pótdíjak).

Ha ezeket a műveleteket, ha további segítségre van szüksége, vagy helyettesítő melyeket vizsgálja meg, akkor olvassa el a szakasz [elküldése struktúra lekérdezések közvetlenül a Hadoop parancssori ](machine-learning-data-science-move-hive-tables.md#submit)szeretné.

## <a name="#load-data"></a>Adatok betöltése partíciók által struktúra táblázatokhoz

>[AZURE.NOTE] Ez általában a egy **felügyeleti** feladatot.

A következőt: taxi adatkészlet rendelkezik ahhoz, hogy gyorsabban feldolgozás és a lekérdezés időpont engedélyezése havonként természetes felosztásához. A PowerShell-parancsok az alábbi (a struktúra címtárból a **Hadoop parancssori**kiadott) havonként particionálva "út" és "jegy ára" struktúra táblák adat betöltése.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

A *minta\_struktúra\_betöltése\_adatok\_által\_partitions.hql* fájl **betöltése** a következő parancsokat tartalmaz.

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

A fontos tudni, hogy a feltárása folyamat az alábbi használjuk struktúra lekérdezések számos is magában foglalja a csak egyetlen partíciót vagy csak néhány partíciót az. De ezeket a lekérdezéseket futtatható a teljes adatok között.

### <a name="#show-db"></a>A HDInsight Hadoop fürt adatbázisok megjelenítése

Szeretné megjeleníteni a HDInsight Hadoop fürt belül a Hadoop parancssorablakban létrehozott adatbázisok, futtassa a következő parancsot a Hadoop parancssort:

    hive -e "show databases;"

### <a name="#show-tables"></a>A struktúra táblák nyctaxidb adatbázis megjelenítése

Szeretné megjeleníteni a táblákat az nyctaxidb adatbázist, futtassa a következő parancsot a Hadoop parancssort:

    hive -e "show tables in nyctaxidb;"

Ellenőrizzük, hogy a táblák vannak particionálva a az alábbi parancsot:

    hive -e "show partitions nyctaxidb.trip;"

A várt eredményt ad nézhet ki:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Hasonlóképpen azt is győződjön meg arról, hogy a jegy ára táblázat particionálva van-e a az alábbi parancsot:

    hive -e "show partitions nyctaxidb.fare;"

A várt eredményt ad nézhet ki:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Adatok feltárása és szolgáltatás műszaki struktúrában

>[AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

Az adatok feltárása és a tevékenységek, az adatok a struktúra táblákba betöltött mérnöki szolgáltatás struktúra lekérdezésekkel lehet elvégezni. Az alábbi példák olyan műveleteket, hogy azt ismerteti, hogy a szakasz tartalma:

- A 10 legjobb bejegyzések megtekintése mindkét táblázatban.
- Ismerkedjen meg néhány mezők változó idő Windows adatok terjesztését.
- Vizsgálja meg a hosszúság és a szélesség mező adatok minőségének.
- Bináris és multiclass besorolás címkét alapján a **Tipp\_összeg**.
- Szolgáltatások elő a közvetlen utazás távolságokat számítások.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Feltárása: Táblázat út a 10 legjobb rekordjainak megtekintéséhez

>[AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

Lásd: az adatok néz ki, hogy vizsgálja meg 10 rekordot minden táblából. Futtassa a következő két lekérdezések külön-külön a rekordok megkeresése a Hadoop parancssori konzol struktúra címtár parancssorából.

Úgy juthat az a 10 legjobb rekordok "út" táblázatban az első hónapban:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Úgy juthat az "jegy ára" a táblázat felső 10 rekordot az első hónapban:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Érdemes gyakran kényelmes megtekintésre fájl mentése a rekordokat. A fenti lekérdezésnek kis módosításának ez teljesít:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Feltárása: Rekordok számának megtekintése az egyes a 12 partíciók

>[AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

A fontos módszerrel utakat száma a naptári év során változik. Csoportosítás havonként lehetővé teszi, hogy látható a terjesztési utak néz ki, hogy mi.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Ezzel kap velünk a kimenet:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Ebben az esetben az első oszlop a hónap és a második pedig utakat számú, a hónap.

Azt is megszámlálható rekordok száma az út adathalmazon a következő parancsot a struktúra címtár parancssorba.

    hive -e "select count(*) from nyctaxidb.trip;"

Együttesen:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Hasonló az adatkészlet számára az út látható parancsok azt állíthatnak struktúra lekérdezések adatkészlet számára az jegy ára a rekordok számát érvényesítéséhez struktúra címtár parancssorából.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Ezzel kap velünk a kimenet:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Figyelje meg, hogy a pontos ugyanannyi havonta utakat a két adathalmaz ad vissza. Ez lehetővé teszi, hogy az adatok megfelelően betöltése első érvényesítése.

A rekordok jegy ára adatkészlet teljes megszámlálása végezhető struktúra címtár parancssorából alatti paranccsal:

    hive -e "select count(*) from nyctaxidb.fare;"

Együttesen:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

A két tábla rekordjainak száma is ugyanúgy történik. Ezzel a megoldással a második érvényességi, hogy az adatok megfelelően betöltése.

### <a name="exploration-trip-distribution-by-medallion"></a>Feltárása: Utazás terjesztési medallion szerint

>[AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

Ebben a példában a medallion (taxi számok) azonosítja a 100-nál több utakat adott időszakon belül. A lekérdezés előnyeivel particionált táblázat elérése, mivel azt a partíciót változó **hónap**van tárolni. A lekérdezés eredményében a egy helyi fájl queryoutput.tsv kerülnek `C:\temp` a központi csomópontra.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Az alábbiakban tartalmát *minta\_struktúra\_út\_darab\_által\_medallion.hql* ellenőrzést a fájl.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

A medallion a következőt: taxi adathalmazon egyedi fülkét azonosítja. Azonosítjuk mely megfelelőségértékelő "elfoglalt" azzal, hogy melyek készült több, mint utakat időtartamban egy adott időszakban. A következő példa, hogy a több, mint a száz utakat első három hónap, és menti a lekérdezés eredményét egy helyi fájl C:\temp\queryoutput.tsv megfelelőségértékelő azonosítja.

Az alábbiakban tartalmát *minta\_struktúra\_út\_darab\_által\_medallion.hql* ellenőrzést a fájl.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

A struktúra címtár parancssorból hiba az alábbi parancsot:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Feltárása: Utazás terjesztési medallion és hack_license

>[AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

Amikor egy adatkészlet felfedezése, gyakran szeretnénk vizsgálja meg a csoportok, az értékek további előfordulásainak száma. Ebben a szakaszban adja meg a művelet megfelelőségértékelő és illesztőprogramokat példa.

A *minta\_struktúra\_út\_darab\_által\_medallion\_license.hql* fájl "medallion" és "hack_license" jegy ára adatkészlet csoportokat, és az egyes kombinációk számát adja eredményül. Az alábbiakban a tartalmát.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Ezt a lekérdezést a cab és utakat csökkenő száma szerint rendezett adott illesztőprogram kombinációja adja eredményül.

A struktúra címtár-parancssorában futtassa:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

A lekérdezés eredményét egy helyi fájl C:\temp\queryoutput.tsv kerülnek.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Feltárása: Értékelése az adatok minősége, jelölje be a érvénytelen szélesség és hosszúság rekordok

>[AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

A közös felderítő adatelemzés célja gyomláláskor érvénytelen vagy hibás bejegyzések meg. Ebben a szakaszban a példa azt határozza meg, hogy a hosszúság vagy a szélesség mező a következőt: kívül távol egy értéket tartalmaz-e. Mivel ez éppen valószínű, hogy ilyen rekord egy hibás hosszúság – szélesség értéket tartalmaz-e, szeretnénk kiküszöbölheti a található bármely modellezési használandó adatokat.

Az alábbiakban tartalmát *minta\_struktúra\_minőség\_assessment.hql* fájl ellenőrzésre.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


A struktúra címtár-parancssorában futtassa:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

A *-S* argumentumban szereplő Ez a parancs nem jelenik meg az állapot képernyőn nyomat a struktúra térkép/kicsinyítés feladatok. Ez akkor hasznos, mert így különbözik a képernyő nyomtatása a struktúra lekérdezés eredménye a olvashatóbbá.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>És magyarázat: Bináris osztály elosztása utazás tippek

> [AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

A bináris besorolás probléma a [Szövegbevitel feladatok példák](machine-learning-data-science-process-hive-walkthrough.md#mltasks) szakaszban ismertetett érdemes tudnia, hogy ezt a tippet adott-e. Tippek az e eloszlása bináris:

* Tipp: a megadott (osztály 1, a tipp\_összeg > 0 Ft)  
* nincs ötlet (osztály 0, tipp\_összeg = 0 Ft).

A *minta\_struktúra\_Formabontó\_frequencies.hql* fájl alább látható módon végzi.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

A struktúra címtár-parancssorában futtassa:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Feltárása: Osztály terjesztését a multiclass beállítással

> [AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

A multiclass besorolás probléma a [Szövegbevitel feladatok példák](machine-learning-data-science-process-hive-walkthrough.md#mltasks) szakaszban ismertetett az adatsort is házirendkezelő természetes osztályozási, ha azt szeretné előre megadott tanácsok mennyiségét. Intervallumok ábrázolásakor tipp tartományok definiálásáról a lekérdezésben. Az osztály terjesztését számára a különböző tipp: a tartományokat, akkor használja a *minta\_struktúra\_tipp\_tartomány\_frequencies.hql* fájlt. Az alábbiakban a tartalmát.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

A következő parancsot a parancssor Hadoop-konzolon:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Feltárása: Két hosszúság – szélesség helyek közvetlen távolságát számítja ki.

> [AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

A közvetlen távolság történő változásának mértéke lehetővé, hogy us megtudhatja, hogy azt, és a tényleges utazás távolság közötti eltérést. Ez a funkció akkor ösztönözheti a rámutat, hogy egy személy előfordulhat, hogy kevesebb valószínűleg tipp: Ha az azok tudni, hogy a vezető szándékosan tett őket sokkal hosszabb beadási.

Tényleges utazás távolság és két hosszúság – szélesség pontok (a "nagy kör" távolság) közötti [távolság Haversine](http://en.wikipedia.org/wiki/Haversine_formula) összehasonlítása megtekintéséhez használjuk a rendelkezésre álló trigonometrikus függvények belül struktúra, így:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

A fenti lekérdezés R a sugár, a földet a mérföld, és a pi radián alakul. Figyelje meg, hogy a hosszúság – szélesség pontok "szűrés" Ha el szeretné távolítani a következőt: terület távol értékeket.

Ebben az esetben azt írni az eredmény "queryoutputdir" nevű könyvtárában. Az alább látható módon parancssorozat először hoz létre a kimeneti könyvtár, és kattintson a a struktúra parancsot futtatja.

A struktúra címtár-parancssorában futtassa:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


A lekérdezés eredményének kerülnek 9 Azure BLOB ***queryoutputdir/000000\_0*** való ***queryoutputdir/000008\_0*** az alapértelmezett tároló a Hadoop fürt alatt.

Az egyes BLOB méretének megtekintéséhez a struktúra címtár parancssorból azt futtassa a következő parancsot:

    hdfs dfs -ls wasb:///queryoutputdir

Egy adott fájl tartalmának megtekintéséhez mondja ki 000000\_0, Hadoop-féle használjuk `copyToLocal` parancsot, tehát.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`lehet, hogy a nagyméretű fájlok nagyon lassan, és nem ajánlott a velük való használatra.  

A legfontosabb előnye, hogy ezeket az adatokat az Azure blob tárolnak, hogy az adatok használata az [Adatok importálása] Azure gépi tanulási belül is mutatunk[ import-data] modulra.


## <a name="#downsample"></a>Azure gépi tanulási az adatok és a Szerkesztés levő minta lefelé

> [AZURE.NOTE] Ez általában az **Adatok tudósok** tevékenység.

A felderítő adatok elemzése fázis után azt már készen áll az adatokat az Azure gépi tanulási levő készítéséhez minta lefelé. Ez a szakasz bemutatjuk struktúra lekérdezés használata minta lefelé az adatokat, majd érhető el az [Adatok importálása] [ import-data] Azure gépi tanulási moduljának.

### <a name="down-sampling-the-data"></a>Az adatok mintavételi lefelé

Az eljárás két lépésből áll. Először a **nyctaxidb.trip** és **nyctaxidb.fare** tábla azt Bekapcsolódás három kulcsokon található összes rekord: "medallion", "ellophatja\_licenc", és a "felvétel\_datetime". Akkor azt hoz létre, a bináris besorolás címke **Formabontó** és a több elem osztály besorolás címke **Tipp\_osztály**.

Használhatja a le mintát a adatokat közvetlenül az [Adatok importálása] [ import-data] Azure gépi tanulási moduljának szükség egy belső struktúratáblával a fenti lekérdezés eredményeinek tárolása. Következik létrehozunk egy belső struktúratáblával és annak tartalmát az illesztett és lefelé a mintában szereplő adatok kitöltéséhez.

A lekérdezés alkalmazza a szabványos struktúra funkciók közvetlenül az órát nap, hét év weekday (1 jelző hétfő és 7 jelző vasárnap) létrehozásához a a "felvétel\_datetime" mezőbe, és a felvétel és dropoff helyeken közvetlen távolságát. Felhasználók [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) is hivatkozhat ilyen funkciók teljes listáját.

A lekérdezés majd lefelé minták az adatokat, hogy a lekérdezés eredményében a Azure gépi tanulási Studio elfér. Az eredeti adatkészlet mindössze 1 %-os importálja a Studio.

Az alábbiakban tartalmának *minta\_struktúra\_előkészítése\_az\_aml\_full.hql* fájlt, az adatok előkészítése az Azure gépi tanulási épület adatmodell.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Ezt a lekérdezést a struktúra címtár parancssorból futtatása

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Most már van egy belső tábla "nyctaxidb.nyctaxi_downsampled_dataset", amelyek használatával az [Adatok importálása] is elérhető[ import-data] az Azure gépi tanulási modulra. Ezenkívül a adatkészlet előfordulhat, hogy használjuk gépi tanulási modellek készítéséhez.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Azure gépi tanulási lefelé mintában szereplő adatok eléréséhez az adatok importálása modul használata

Struktúra lekérdezések az [Adatok importálása] kiállító előfeltételei szerint[ import-data] Azure gépi tanulási modulja, szükség az Azure gépi tanulási munkaterület való hozzáférés és a hozzáférést a fürt és a kapcsolódó tároló fiók hitelesítő adatait.

Néhány részletes tudnivalókat az [Adatok importálása] [ import-data] modul és a bemeneti paramétereket:

**HCatalog kiszolgáló URI**: Ha a fürt nevének abc123, akkor egyszerűen: az: https://abc123.azurehdinsight.net

**Hadoop felhasználói fiók neve** : A felhasználó nevét a választott a fürt (**nem** a távelérési felhasználónevet)

**Hadoop ser fiók jelszava** : A jelszó (**nem** a távelérési jelszó) a fürt választott

**Kimeneti adatok helyét** : Ez legyen az Azure választja.

**Azure tároló fióknév** : a fürt társított alapértelmezett tárterület-fiók neve.

**Azure tároló neve** : Ez az alapértelmezett tároló nevet a fürt, pedig általában ugyanaz, mint a csoport nevét. "Abc123" nevű fürt Ez az csak abc123.

> [AZURE.IMPORTANT] **Minden olyan tábla azt kívánja lekérdezés használatával az [Adatok importálása] [ import-data] Azure gépi tanulási moduljának egy belső tábla kell lennie.** Tipp Ha D.db adatbázis táblájához T egy belső tábla meghatározásakor a következőképpen történik.

A struktúra címtár parancssorból hiba a parancsot:

    hdfs dfs -ls wasb:///D.db/T

Ha a táblázat egy belső táblázatban, és azt van töltve, tartalmának itt kell mutatnia. Határozza meg, hogy-e a táblázat egy belső tábla egy másik úgy, hogy az Azure tároló Intézővel. Annak segítségével keresse meg a fürt alapértelmezett tároló nevét, és ezután szűrni a táblázat neve. Ha a táblázat és annak tartalmának jelenik meg, ezzel megerősíti, hogy egy belső tábla.

Az alábbiakban a struktúra lekérdezés és az [Adatok importálása] pillanatképét[ import-data] modul:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Jegyzet, hogy a mintában szereplő adatokat tárolja az alapértelmezett tárolóban lefelé, mivel az eredményül kapott struktúra lekérdezés az Azure gépi tanulási rendkívül egyszerű pedig egyszerűen egy "VÁLASSZA a * nyctaxidb.nyctaxi a\_felbontáscsökkentésének\_adatok".

Az adatkészlet használhatók kezdőpontjának gépi tanulási modellek készítéséhez.

### <a name="mlmodel"></a>Az Azure gépi tanulási levő összeállítása

Most már vagyunk épület adatmodell és [Azure gépi tanulási](https://studio.azureml.net)modell bevezetésének a folytatáshoz. Készen áll a kapcsolatfelvételi fent előrejelzési problémáknak megcímezheti használni adatai:

**1. bináris besorolás**: előrejelzésére engedélyezi vagy tiltja a tipp fizettek utazása.

**Használt tanuló:** Második szintű logisztikus regresszió

egy. Ez a probléma a célhely (vagy osztály) felirat van "Formabontó". Az eredeti lefelé mintát adatkészlet rendelkezik-e besorolás kísérletből cél szivárgást néhány oszlopok. Különösen: tipp\_osztály, tipp\_összeg és a teljes\_összege a cél címkét, amely nem érhető el időt kipróbálása azt tárja fel információkat. Oszlopok eltávolítása azt az [Oszlopok kiválasztása az adatkészlet] használatával szempontokat[ select-columns] modulra.

Az alábbi pillanatkép a kísérlet előre-e egy adott út ezt a tippet kifizetett jeleníti meg.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Ez a kísérlet a cél címke terjesztését volt nagyjából 1:1.

Az alábbi pillanatkép tipp eloszlását osztály címkéket a bináris besorolás probléma jeleníti meg.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Emiatt azt szerezze be a 0.987 egy AUC, az alábbi ábrán látható módon.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass besorolás**: előrejelzésére az utazás az előzőleg definiált osztályok használatával kifizetett tipp összegek cellatartományt.

**Használt tanuló:** Multiclass logisztikus regresszió

egy. Ez a probléma a célhely (vagy osztály) címkéje "Tipp\_osztály" tehet öt értékek (0,1,2,3,4). A bináris besorolás lehetőséget választja, ahogy van néhány cél szivárgást a kísérlet az oszlopok. Különösen: Formabontó, tipp\_végösszeg,\_összege a cél címkét, amely nem érhető el időt kipróbálása azt tárja fel információkat. Ezekben az oszlopokban az [Oszlopok kiválasztása az adatkészlet] eltávolítása[ select-columns] modulra.

A lenti pillanatkép jeleníti meg a kísérlet előre, mely mappában lévő ezt a tippet valószínűleg esik (osztály 0: tipp: = $0, 1 osztály: > 0 Ft és tipp tipp < = $5, 2-es osztályú: > $5 és tipp tipp < = $10, a Class 3: > 10 USD és tipp tipp < = $20, osztály 4: tipp > $20)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Most már megjelenítése a tényleges vizsgálati osztály terjesztési néz ki. Láthatja, hogy közben osztály 0 és 1 osztály elterjedt, a többi osztály is ritka.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Ez a kísérlet az használjuk fejetlenséget mátrixok tekintse meg az előrejelzési pontosság. Az alábbiakban látható.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Figyelje meg, hogy az osztály pontosság a elterjedt osztályok ugyan a meglehetősen hatékony, a modell nem a jó feladat az "oktatás" meg az egyes osztályok.


**3. regressziós tevékenység**: előre kifizetett utazása tipp mennyiségét.

**Használt tanuló:** Csillapítja döntési fa

egy. Ez a probléma a célhely (vagy osztály) címkéje "Tipp\_összege". Ebben az esetben is a cél szivárgást: Formabontó, tipp\_osztály összes\_összeg; Ezek a változók beállíthatja, hogy nem érhető el a szokásos idő kipróbálása tipp fokkal információt. Ezekben az oszlopokban az [Oszlopok kiválasztása az adatkészlet] eltávolítása[ select-columns] modulra.

A pillanatkép belows a kísérlet előrejelzésére az adott tipp mennyiségét jeleníti meg.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. A regressziós problémákat azt a négyzetben hiba az előrejelzések, meghatározása együtthatója és hasonló megjeleníti a pontosság az előrejelzési mérje le. Bemutatjuk ezeket az alábbi.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Láthatja, hogy meghatározása együtthatója kapcsolatban van 0.709, varianciáját körülbelül 71 %-át úgy magyarázható a modell együtthatók.

> [AZURE.IMPORTANT] Azure gépi tanulási, és hogyan érhetik el és használhatják ezt kapcsolatos további információért olvassa el [Mi az gépi tanulási?](machine-learning-what-is-machine-learning.md). Játsszon el kitöltenem, az Azure gépi tanulási gépi tanulási kísérletek nagyon hasznos erőforrásokat a [Cortana üzletiintelligencia-gyűjtemény](https://gallery.cortanaintelligence.com/). A gyűjtemény kísérletek színtartomány foglalkozik, és Azure gépi tanulási funkciók cellatartomány egy alapos bevezetés nyújt.

## <a name="license-information"></a>Licenc információ

Ez a példa forgatókönyv és a kapcsolódó parancsfájlok meg vannak osztva Microsoft MIT-licenc csoportban. Ellenőrizze a LICENSE.txt fájl könyvtárában található további részleteket GitHub minta kódot.

## <a name="references"></a>Hivatkozások

• [Andrés Monroy a következőt: Taxi utakat letöltési oldalon](http://www.andresmh.com/nyctaxitrips/)  
• [a következőt: Taxi utazás adatok Chris Whong fOILing](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taxi a következőt: és Limousine jutalék kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
