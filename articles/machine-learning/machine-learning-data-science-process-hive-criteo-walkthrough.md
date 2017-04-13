<properties
    pageTitle="A csoportwebhely adatok tudományos folyamat működését: használja az 1 TB Criteo adatkészlet fürtök HDInsight Hadoop |} Microsoft Azure"
    description="Csoportwebhely adatok tudományos eljárással-végpontok közötti forgatókönyv alkalmazó létre és helyezhetnek üzembe a modellben egy nagy (1 TB) nyilvánosan elérhető adatkészlet egy HDInsight Hadoop fürthöz"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>A csoportwebhely adatok tudományos folyamat működését – az Azure hdinsight szolgáltatáshoz Hadoop fürt egy 1 TB adatkészlet

Az útmutató azt mutatja be a eljárással csapat adatok tudományos-végpont – esetben az [Azure hdinsight szolgáltatáshoz Hadoop fürt](https://azure.microsoft.com/services/hdinsight/) tárolni, szolgáltatás adatbázismodellbe, és a nyilvánosan elérhető [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) adatkészleteket a mintaadatok lefelé. Azure gépi tanulási segítségével bináris besorolás modell épülnek ezeket az adatokat. Azt is bemutatják, hogyan teheti közzé a modell egyik egy webszolgáltatásból.

Akkor is elvégezheti a műveleteket, a jelen útmutató található egy IPython-Jegyzetfüzet használata. A felhasználók, akik próbálja meg ezt a megközelítést kívánja nézze át a [struktúra ODBC-kapcsolaton keresztül Criteo forgatókönyv](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) témakört.


## <a name="dataset"></a>Criteo adatkészlet leírása

Az adatok, amely körülbelül 370GB-nyi tömörített gzip TSV-fájlok (nem tömörített ~1.3TB), kattintson a szövegbevitel adatkészletet Criteo 4.3 milliárd-nél több rekordot tartalmazó. A 24 nap származik kattintson az adatok [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/)elérhetővé tett. Adatok tudósok kényelmesebbé hogy van kibontott nekünk kísérletezés a rendelkezésre álló adatokat.

Ez az adatkészlet az egyes rekordok 40 oszlopot tartalmaz:

- az első oszlopban található címke oszlop, amely azt jelzi, hogy a felhasználó kattint egy **hozzáadása** (érték-1) vagy nem kattintson az egyik (értéke 0)
- Ezután 13 oszlopok numerikus, és
- utolsó 26 olyan kockák oszlopok

Az oszlopok anonimizált vannak, és a felsorolásos nevek adatsor használata: szeretne (az a címke oszlop) "Oszlop1" "Col40" (az utolsó kockák oszlopa).            

Íme egy szövegrészt a adatkészlet a két halmazára (sorokat) az első 20 oszlopok:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Hiányzó értékek is szerepelnek a numerikus és kockák oszlopok e adathalmazban. Azt ismertetik, hogy egy egyszerű módszer a hiányzó értékek kezelése. További információt az adatok írja amikor struktúra táblákba tároljuk őket.

**Meghatározása:** *Átkattintás ráta (Parancsra):* Ez a százalékos gombra kattint, az adatok. A Parancsra a Criteo adatkészlet 3,3 %-át vagy 0.033.

## <a name="mltasks"></a>Példák a szövegbevitel feladatok
Két példa előrejelzési problémák foglalkozik az útmutató:

1. **Bináris besorolás**: = esetben előrejelzése, hogy a felhasználó rákattint egy hozzáadása:
    - Osztály 0: Nincs kattintásra
    - Class 1: kattintson a

2. **Regressziós**: = esetben előrejelzése a annak valószínűsége, az Active Directory kattintson a felhasználói funkciók.


## <a name="setup"></a>Adatok tudományos fürthöz felfelé beállítása egy HDInsight szabható Hadoop bemutatása

**Megjegyzés:** Ez általában a egy **felügyeleti** feladatot.

A HDInsight fürt három lépésben a cserélendő analytics-megoldások fejlesztésére Azure adatok tudományos környezet beállítása:

1. [Tárterület-fiók létrehozása](../storage/storage-create-storage-account.md): ehhez a fiókhoz tároló Azure Blob-tárolóban lévő adatok tárolására szolgál. Az adatok HDInsight fürt használt tárolási helye.

2. [Testreszabása Azure hdinsight szolgáltatáshoz Hadoop fürt az adatok tudományos](machine-learning-data-science-customize-hadoop-cluster.md): Ebben a lépésben létrehoz egy Azure hdinsight szolgáltatáshoz Hadoop fürthöz 64 bites Anaconda Python 2.7 csomópontjait telepítve. A HDInsight fürt testreszabásakor befejezéséhez (ebben a témakörben ismertetett), két fontos lépésből áll.

    * A tároló fiók lépésben létrehozott 1 a HDInsight fürthöz létrehozáshoz hozzá kell rendelnie. Ez a tárterület-fiók eléréséhez, amely a fürt belül feldolgozhatók használják.

    * A központi csomópontot a fürt létrehozása után engedélyezze távelérési. Ne feledje, hogy a távoli hozzáférés hitelesítő adatokat, az itt megadott (eltérnek a létrehozása a fürthöz megadott): szükség szerint csoportokat, az alábbi eljárás elvégzéséhez.

3. [Az Azure Machine Learning munkaterület létrehozása](machine-learning-create-workspace.md): Ez Azure gépi tanulási munkaterület gépi tanulási modellek készítéséhez után az eredeti adatok feltárása és lefelé a HDInsight fürt mintavételnél használják.

## <a name="getdata"></a>Első, és a nyilvános forrásból származó adatok felhasználása

A hivatkozásra kattint, a használati feltételek elfogadása és megadása az [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) adatkészlet is elérhető. Itt látható pillanatképét néz ki:

![Criteo feltételek elfogadása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Kattintson a **Folytatás gombra a letöltés** további tudnivalók az adatkészlet és elérhetőségét.

Az adatok egy nyilvános [Azure blob-tároló](../storage/storage-dotnet-how-to-use-blobs.md) helyen található: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. A "wasb" Azure Blob-tárolóhoz hely hivatkozik. 

1. Az adatok a nyilvános blob-tárolóban lévő három almappák kicsomagolt adatok áll.

    1. Az almappa *nyers darab / /* tartalmazza az első 21 napon nap - adatainak\_napra 00\_20
    2. Az almappa *nyers vonaton / /* áll, az adatok egyetlen nap nap\_21
    3. Az almappa *nyers próba / /* áll, az adatokat, két nap nap\_22-es és a nap\_23

2. Akik szeretné kezdeni a nyers gzip adatokat tartalmazó, ezek is rendelkezésre állnak a fő mappában *nyers /* day_NN.gz, ahol NN Ugrás a 00 és 23 szerint.

Alternatív megközelítés férhet hozzá, ismerje meg, és ezeket az adatokat tetszőleges helyi letöltések nem igénylő magyarázza az útmutató a struktúra táblák létrehozásakor modell.

## <a name="login"></a>Jelentkezzen be a fürt headnode

Jelentkezzen be a headnode a fürt, keresse meg a fürt az [Azure portal](https://ms.portal.azure.com) segítségével. Kattintson a bal oldali HDInsight elefánt ikonra, és kattintson duplán a csoport nevére. Nyissa meg azt a **beállítás** lapon, kattintson duplán a csatlakozás ikonra a lap aljára, és adja meg a távoli hozzáférés hitelesítő adatait, amikor a rendszer kéri. Ekkor megjelenik a headnode a fürt.

Íme egy tipikus első jelentkezzen be a fürt headnode néz ki:

![Jelentkezzen be a fürt](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


A bal oldali a "Hadoop parancssori", amely az adatok feltárása a workhorse láthatja. Két hasznos URL - "Hadoop fonal állapot" és "Hadoop neve csomópont" is láthatja. Fonal állapot URL-CÍMÉT jeleníti meg a projekt előrehaladását, és név csomópont URL-címe részletezi fürt konfigurációja miatt.

Most már azt be van állítva, és készen áll kezdje el az első része a Útmutató: adatok feltárása struktúra használ, és felkészülés a adatok Azure gépi tanulási.

## <a name="hive-db-tables"></a>Struktúra adatbázis és tábla létrehozása

A Criteo adatkészlet-struktúra táblázatok létrehozása, nyissa meg a ***Hadoop parancssori*** az asztalon, a központi csomópontot, és adja meg a struktúra könyvtár a parancs megadásával

    cd %hive_home%\bin

>[AZURE.NOTE] Minden struktúra parancs futtatása az útmutató a struktúra lévő / könyvtár ablakot. Ezzel megnyitja elérési út problémák megoldásához automatikusan kezeli. Használjuk adatokra "Struktúra címtár kérdés", "struktúra bin / címtár kérdés", és a "Hadoop parancssori" szót azonos értelemben.

>[AZURE.NOTE]  Bármilyen struktúra lekérdezés végrehajtani, egy mindig az alábbi parancsokat használhatja:

        cd %hive_home%\bin
        hive

Után megjelenik a struktúra replikációs egy "struktúra >"jelentkezzen be, egyszerűen Kivágás és beillesztés a lekérdezés végre.

A következő kódot létrehoz egy adatbázist "criteo", és ezután generál 4 táblák:


* napok napon létrehozott *táblázat létrehozásához megszámolja* \_napra 00\_20,
* a nap létrehozott *tábla, a vonaton adatkészlet* \_21, és
* két *táblában használja a próba adatkészleteket* nap épül\_22-es és a nap\_23 rendre.

Azt felosztása a próba adatkészlet két különböző táblákból, mert a nap egyik egy ünnep, és szeretnénk, határozza meg, ha a modell észleli és közötti különbségek ünnepnap nem ünnepi Átkattintás árfolyam.

A parancsprogram [minta #95; struktúra & #95; létrehozása & #95; criteo & #95; adatbázis & #95; és & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) kényelmesebbé itt látható:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Azt ne feledje, hogy ezek a táblák külső, akkor egyszerűen Azure Blob-tárolóhoz (wasb) helyekre mutatnak.

**Két módon bármely struktúra lekérdezés most megemlítő.**

1. **A struktúra replikációs parancssori használatával**: az első hiba "struktúra" parancs és a másolatot, és illessze be a lekérdezés, a struktúra replikációs parancssori. Ehhez hajtsa végre:

        cd %hive_home%\bin
        hive

    Most már parancssori replikációs, a Kivágás és beillesztés a lekérdezés végrehajtása azt.

2. **Lekérdezések fájlba menti, majd a parancs végrehajtása**: A második pedig a lekérdezések .hql fájl mentése ([minta #95; struktúra & #95; létrehozása & #95; criteo & #95; adatbázis & #95; és & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)), és kattintson a lekérdezés hajtsa végre a következő parancsot:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Erősítse meg az adatbázis és tábla létrehozása

Ezután ellenőrizzük a struktúra lévő az alábbi paranccsal az adatbázis létrehozása vagy címtár kérdés:

        hive -e "show databases;"

Ezzel kap:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Ez megerősíti, hogy az új adatbázis "criteo" létrehozását.

Milyen létrehozott táblák megtekintéséhez egyszerűen ad a struktúra lévő alábbi parancs / címtár kérdés:

        hive -e "show tables in criteo;"

Ezután a következő kimenet láthatja:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Adatok feltárása a struktúra

Most tegye néhány alapvető adatok feltárása a struktúra készen áll azt. Kezdje azzal, hogy a vonaton példák megszámlálása azt, és tesztelje az adattáblák.

### <a name="number-of-train-examples"></a>Példák vonaton száma

A [minta & #95; struktúra & #95; a darab és a #95; vonaton & #95; táblázat & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) tartalmát itt jelennek meg:

        SELECT COUNT(*) FROM criteo.criteo_train;

Együttesen:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Másik lehetőségként egyik is adhatnak a következő parancsot a struktúra lévő / címtár kérdés:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>A két teszt adatkészleteket próba példák száma

A Microsoft most számának példái a két teszt adatkészleteket. Tartalmának [minta & #95; struktúra & #95; darab és a #95; criteo & #95; próba & #95; nap és #95; 22 & #95; táblázat & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) itt áll:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Együttesen:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

A szokásos módon, azt is hívhat a parancsfájl a struktúra lévő / könyvtár üzenet szerint kiállító a parancsot:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Végezetül azt vizsgálja meg az próba példák az nap alapján próba adathalmazban\_23.

A parancs ehhez hasonlít a csak a látható (lásd a [minta & #95; struktúra & #95; a darab és a #95; criteo & #95; próba & #95; nap és #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Ezzel kap:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Címke terjesztési az vonaton adathalmazban

Címke az vonaton adathalmazban eloszlása érdeklődésre számot tartó. A jelenik meg, akkor jelenik meg a [minta és a #95; struktúra & #95; criteo & #95; címke & #95; terjesztési & #95; vonaton & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql)tartalmát:

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

A címke eloszlás együttesen meg:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Ne feledje, hogy a pozitív címkék százalékát körülbelül 3.3 % (megfelel az eredeti adatkészlet).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Hisztogram terjesztését az vonaton adathalmazban néhány numerikus változót

Ábrázolásakor struktúrájának natív "hisztogram\_numerikus" függvény megtudhatja, hogy a numerikus változók eloszlását néz ki. Az alábbiakban a [minta és a #95; struktúra & #95; criteo #95; hisztogram & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql)tartalma:

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Ez a együttesen a következőket:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

A NÉZET – oldalsó kihúzása kombinációja a szokásos felsorolás helyett egy SQL-szerű kimenet létrehozására struktúra szolgál. Ne feledje, hogy a következő táblázat az első oszlop felel meg a bin center, és a második, a bin gyakoriságát.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Néhány numerikus változót az vonaton adathalmazban közelítő percentilisek

A fontos numerikus változóval is a közelítő percentilisek számítása. Natív adatait a struktúra "PERCENTILIS\_hozzávetőlegesen" nekünk végzi. Tartalmának [minta & #95; struktúra #95; criteo & #95; közelítő & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) vannak:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Együttesen:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Azt remark, hogy a százalékos érték elosztása szorosan kapcsolódik a hisztogram eloszlás bármely numerikus változó általában.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Szám egyedi értékek megkeresése az vonaton adathalmazban kockák oszloppal

Az adatok feltárása a Folytatás azt most megkereséséhez néhány kockák oszlop veszik egyedi értékek számát. Ehhez a tartalmát bemutatjuk [minta & #95; struktúra #95; criteo & #95; egyedi & #95, értékek és #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Együttesen:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Azt láthatja, hogy Col15 van-e egyedi értékek 19M! Naïve technikák, például "egy gyorsbillentyűk használatát kódolás" kódolni ilyen magas többdimenziósak kockák változók használata lehessen hozni. Különösen azt magyarázza el, és bemutatják egy hatékony, robusztus technika [Tanulási a megszámolja](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) meghívott elleni hatékony a probléma megoldásához.

Ez a szakasz azt befejezése: Megjeleníti a kockák oszlopokat, valamint egyedi értékek számát. Tartalmának [minta & #95; struktúra #95; criteo & #95; egyedi & #95 értékek és #95; több & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) vannak:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Együttesen:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Ismét láthatja, hogy Col20, kivéve a többi oszlop sok egyedi értékeket tartalmaznak.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Közös előfordulás megszámolja az vonaton adathalmazban kockák változót pár

A kockák változót párban közös előfordulás számát is érdeklődésre számot tartó van. Ez lehet meghatározni a kód használatával [minta & #95; struktúra #95; criteo & #95; párosított & #95; kockák & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Fordított azt a számlálás rendezési szempont az előfordulás, és ebben az esetben keresse meg a képernyő tetején 15. Ezzel kap us:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Példa a adatkészleteket az Azure gépi tanulási lefelé

Gondjai megismerkedett a adatkészleteket, és igazolni, akkor előfordulhat, hogy hogyan változói (beleértve kombinációk), akkor most minta lefelé feltárása a következő típusú adatkészletek, hogy az Azure gépi tanulási levő nyújthat. Visszahívási, amely a probléma, hogy koncentráljon: megadott példa attribútumok (Col2 - Col40 szolgáltatás értékeivel), hogy előre, Oszlop1 esetén pedig 0 (nem kattintson) vagy egy 1 (kattintson).

Lefelé a vonaton mintát, és tesztelje az eredeti méret 1 %-os adatkészleteket, hogy struktúrájának natív RAND() függvényt használni. A következő parancsfájl [minta & #95; struktúra & #95; criteo & #95; felbontásának csökkentése és a #95; vonaton & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) végzi a vonaton adatkészlet számára:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Együttesen:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

A parancsprogram- [minta & #95; struktúra & #95; criteo & #95; felbontásának csökkentése és a #95; próba & #95; nap és #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) jelent az, a vizsgált adatokat, nap\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Együttesen:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Végül a parancsprogram- [minta & #95; struktúra & #95; criteo & #95; felbontásának csökkentése és a #95; próba & #95; nap és #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) jelent az, a vizsgált adatokat, nap\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Együttesen:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Az adatokkal azt saját lefelé mintában szereplő vonaton és tesztelje az Azure gépi tanulási levő készítéséhez adatkészleteket készen áll.

Összetevő végleges fontos előtt azt lépjen tovább Azure gépi tanulási kérdésére választ adhatnak a darab táblázatot. A következő alszint szakaszban ölelik ez egyes részletesen.

##<a name="count"></a>A count táblázatban rövid vitafórumban

Ahogyan azt bemutattuk, a kockák többváltozós van egy nagyon magas dimensionality. Az útmutató – [Oktatás és megszámolja,](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) ezek a változók kódolni hatékony és hatékony módon nevű hatékony módszer azt bemutatása. Ez a módszer további információt tartalmaz a hivatkozást követve.

**Megjegyzés:** Az útmutató azt a feladatokra fókuszálnak kapcsol magas többdimenziósak kockák szolgáltatások kompakt megadott száma a táblák használata Ez nem csak úgy lehet kódolását kockák szolgáltatások; érdekli felhasználók további tájékoztatást a más technikákkal kivehet [egy – melegvíz-kódolásának](http://en.wikipedia.org/wiki/One-hot) és [szolgáltatás kivonatolás](http://en.wikipedia.org/wiki/Feature_hashing).

Darab táblázatok készítésére a számláló adatokra, használjuk az adatok az a mappa nyers/száma. Modellezési szakaszban bemutatjuk felhasználók kockák lehetőségei az elejétől, az alábbi darab táblázatok létrehozásának vagy másik lehetőségként a explorations beépített száma táblázattá használni. A következőkben amikor hivatkozunk "beépített darab táblázatok", "azt jelenti, amely elvégezheti a szükséges száma a táblák használata. Hogyan érhető el az alábbi táblázat nyomtatásáról részletes útmutatást a következő szakaszban találhatók.

## <a name="aml"></a>Azure gépi tanulási modell összeállítása

A modell létrehozása az Azure gépi tanulási folyamat lépései a következők:

1. [Az adatok beolvasása a struktúra táblázatok az Azure gépi tanulási](#step1)
2. [Hozzon létre a kísérlet: az adatok letisztázásának, válasszon egy tanuló és a featurize darab-táblázatokban](#step2)
3. [A modell képzése](#step3)
4. [A modell pontszám a próba-adatok](#step4)
5. [A modell felmérése](#step5)
6. [Közzététel a modell felhasználandó webszolgáltatás](#step6)

Most már készen áll az Azure gépi tanulási studio levő létrehozására azt. A lefelé mintában szereplő adatok a fürt struktúra táblaként menthetők. Az **Adatok importálása** Azure gépi tanulási modul segítségével olvassa el az adatok. A hitelesítő adatokat a fürt tároló fiókjának eléréséhez az alábbiakban találhatók.

### <a name="step1"></a>Lépés: 1: Adatok beolvasása a struktúra táblák Azure gépi tanulási az adatok importálása modulról be, és jelölje ki azt a kísérlet tanulási géphez

Kezdje azzal, hogy kijelöli a **+ Új** -> **kísérlet** -> **Üres kísérlet**. A bal felső sarokban kattintson a **Keresés** mezőbe, keresse meg "Az adatok importálása". Húzással megjelenítheti a kísérlet vászon (középső része a képernyő) az **Adatok importálása** modul használja a modul adatokhoz való hozzáférés.

Ez a az **Adatok importálása** néz ki adatok lekérése a struktúratáblával közben:

![Adatok importálása kap adatok](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Az **Adatok importálása** modulhoz az értékeket, amelyeket a ábra paraméterek példák csak az értékek, meg kell adnia a rendezés. Kattintson az **Adatok importálása** modulhoz a paraméter megadása az alábbiakban néhány általános útmutatást.

1. "Lekérdezés struktúra" válassza az **Adatforrás**
2. Az **adatbázis-lekérdezés struktúra** mezőbe egy egyszerű SELECT * FROM < a\_adatbázis\_name.your\_táblázat\_neve > – elég.
3. **Hcatalog kiszolgáló URI**: Ha a fürt "abc", akkor egyszerűen: az: https://abc.azurehdinsight.net
4. **Hadoop felhasználói fiók neve**: A felhasználó nevét a választott üzembe helyezés a fürt idején. (Nem a távelérési felhasználónév!)
5. **Hadoop felhasználói fiók jelszava**: a felhasználó nevét a fürt üzembe helyezés időben választott jelszavát. (Nem a távelérési jelszó!)
6. **Kimeneti adatai helye**: válassza a "Azure"
7. **Azure tároló fióknév**: A tárterület-fiók fürthöz társított
8. **Azure tároló fiókkulcs**: a tárterület-fiók a kulcs fürthöz társított.
9. **Azure tároló neve**: Ha a csoport neve "abc", akkor egyszerűen "abc," Ez általában.


Miután az **Adatok importálása** befejeződött, az első (akkor látható, a zöld osztásjelek modulban) adatok, mentse ezeket az adatokat egy adatkészlet (nevű egy tetszés szerinti). Ez néz ki:

![Adatok importálása az adatok mentése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Kattintson a jobb gombbal a kimeneti port az **Adatok importálása** modul. Ez a tár egy **adatkészlet mentése** lehetőséget, és a **Megjelenítés** beállítást. A **Megjelenítés** lehetőséget, ha kattint, az adatok mellett a jobb oldali ablaktábla, néhány összesítő statisztika esetében hasznos lehet 100 sor jeleníti meg. Mentse az adatokat, válassza a **Mentés másként adatkészlet** , és kövesse az utasításokat.

A mentett adatkészlet használatra kijelölni egy gépi tanulási kísérlet, keresse meg az adatkészleteket, a **Keresés** mezővel, az alábbi ábrán látható. Egyszerűen írja be a nevét, az adatkészlet részben való hozzáférés, és húzza az adatkészlet a fő rendelte. A fő panel alakzatot elengedése kijelöli a gépi tanulási modellezési való használatra.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] Ehhez a vonaton és a vizsgálat adatkészleteket. Is ne feledje, hogy az adatbázis neve és táblanevek okozó erre a célra. Az ábrán alkalmazott értékek kizárólag az ábra purposes.* *

### <a name="step2"></a>Lépés: 2: Hozzon létre egy egyszerű kísérlet az Azure gépi tanulási kattintással előrejelzésére / nincs a kattintással

Az Azure Machine Learning kísérlet néz ki:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Azt vizsgálja meg a kísérlet fő összetevői most. Ne feledje szükség húzza a mentett vonaton, és megjelenítheti a kísérlet vászon adatkészleteket először tesztelje.

#### <a name="clean-missing-data"></a>Hiányzó adatok letisztázásának

A **Hiányzó adatok karbantartása** modul tartalmaz, annak neve javasol: azt törli a hiányzó adatok, amelyek lehetnek a felhasználó által megadott módon. A modulba keresi, akkor jelenik meg:

![Hiányzó adatok karbantartása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Ebben az esetben azt választotta, hogy az összes hiányzó értékek cseréje a 0-val. Vannak, valamint egyéb beállításokat, amelyek a legördülő lista a modul rögzíthetők is láthatja.

#### <a name="feature-engineering-on-the-data"></a>Ez a funkció az adatok műszaki

Egyedi értékek nagyméretű adatkészletek kockák funkciókkal milliónyi lehet. Naïve módszerek, például egy gyorsbillentyűk használatát kódolásának, amely ilyen magas többdimenziósak kockák szolgáltatások használata teljesen lehessen hozni. Az útmutató azt szemléltetik is használhatja a beépített Azure gépi tanulási modulok ezek nagy többdimenziósak kockák változót kompakt megadott létrehozásához használt darab funkcióit. A végeredmény modell kisebb méretű, így gyorsabban oktatás lapok és más technikákkal használatával igazán hasonló teljesítménymutatók.

##### <a name="building-counting-transforms"></a>Az épület átalakítások számolva

Darab szolgáltatások összeállításához azt használja a **Összeállítása számlálás átalakítása** modult, Azure gépi tanulási elérhető. A modul néz ki:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Fontos megjegyzés** : az **oszlopok száma** mezőben azt adja meg azt végrehajtandó megszámolja a oszlopokra. Ezek általában (mint az imént említett) magas többdimenziósak kockák oszlopokat. Indításkor, hogy megemlítették, hogy az Criteo adatkészlet 26 kockák oszlopot tartalmaz-e: a Col15 Col40 szeretne. Ebben az esetben azt tartalmazó cellákat számlálnia összes őket, és nevezze el az indexek (a 15 vesszővel elválasztva, ahogy azt 40).

A modul használata a MapReduce módban (megfelelő nagy adathalmazok), szükségünk van egy HDInsight Hadoop fürthöz access (szolgáltatás feltárási használt felhasználhassa őket, valamint erre a célra) és a hitelesítő adatait. Előző értékeit mutatják be a kitöltött értékekről néz (cserélje ki a azoknak, a saját használatieset-ábra megadva).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

A fenti ábrán bemutatjuk, hogyan írja be a bemeneti blob-helyet. Ezen a helyen van az adatok darab táblák támaszkodva van fenntartva.


Ez a modul után is mentheti a átalakító később a jobb gombbal a modul a **Mentés másként átalakítás** lehetőséget:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

A fent látható kísérlet architektúrában az adatkészlet "ytransform2" felel meg pontosan egy mentett darab átalakítás. A kísérlet maradéka feltételezzük, az olvasót bizonyos adatok **Összeállítása számlálás átalakítása** modul használva megszámolja készítése, illetve majd segítségével e megszámolja a vonaton darab szolgáltatások készítése és tesztelje a adatkészleteket.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Milyen darab szolgáltatásait szeretné szerepeltetni a vonaton és a vizsgálat adatkészletek kiválasztása

Van még egy kész átalakítása száma, amikor a felhasználók választhatnak, mi a vonaton szerepeltetni, és tesztelje a **Darab táblázat paramétereinek módosítása** modulról adatkészleteket szolgáltatásait. Ez a modul csak bemutatjuk itt teljesség, hanem az egyszerűség nem ténylegesen használja a kísérlet az.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

Ebben az esetben is láthatja, ahogy azt választotta, csak a napló-valószínűleg használni, és ki az oszlopot a vissza figyelmen kívül. Azt is beállíthatja, hogy hány alkalmazzák előzetes példák a simítás, és hogy bármilyen Laplacian zajt vagy nem adja hozzá a szemét bin küszöbértékét, például paramétereket. Megjegyzendő, hogy az alapértelmezett értékek-e a felhasználók, akik az új szolgáltatás generációs ilyen típusú hasznos kiindulási pont áll, és mind speciális funkcióit.

##### <a name="data-transformation-before-generating-the-count-features"></a>A darab funkciók létrehozása előtt átalakítási

Most azt koncentráljon kapcsolatban a vonaton átalakítása egy fontos pontra, és darab szolgáltatások ténylegesen létrehozása előtt adatok vizsgálata. Látható, hogy az adatok a darab átalakítás azt alkalmazása előtt két **Végrehajtása R parancsfájl** modulokat.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Az alábbiakban az első R parancsprogramot:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


A R parancsfájl azt nevezze át az oszlopot, "Col40" "Oszlop1" nevét. Ennek oka az, a darabszám átalakítás vár, ez a formátum neve.

A második R parancsfájl, azt a terjesztési között pozitív és negatív osztályok egyenleg (1 és 0 osztályok rendre) szerint felbontás csökkentését a negatív osztály. Az R parancsfájl Itt megtudhatja, hogy miként művelet:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

Az egyszerű R parancsfájl használjuk "pos\_Neg.\_arány" beállíthatja az egyenleg a pozitív és negatív osztályok között. A fontos, hogy végezze el a óta osztály egyensúly általában javítására teljesítmény előnyöket nyújtja, az osztályozási problémák hol osztály eloszlása ferde (a visszahívás, hogy a lehetőséget választja, felkínálunk 3.3 % pozitív és negatív osztály 96.7 %).

##### <a name="applying-the-count-transformation-on-our-data"></a>Az adatok a darab transzformáció alkalmazása

Végezetül azt segítségével az **Átalakítás alkalmazásához** modul a darab átalakítások alkalmazása a vonaton, és tesztelje a adatkészleteket. Ez a modul egy bemeneteként a mentett darab átalakítás és a vonaton vagy próba adatkészleteket, mint a többi bemeneti tart, és darab funkcióival adatokat ad vissza. Itt látható:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>A következőhöz hasonlóan egy szövegrészt, mely a darabszám funkciói

Érdemes megtekinteni a darab funkciók kinézetét abban az esetben a fontos információkat tartalmazó. Itt akkor jelenik meg ez egy szövegrészt:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

Az ebben a kivonat akkor jelenik meg, hogy az oszlopok megszámlált a azt, hogy beszerzése a száma, és valószínűleg kívül minden releváns backoffs jelentkezzen.

Azt most már készen áll-e transzformált adatkészleteket használatával Azure gépi tanulási modell összeállítása. A következő szakaszban bemutatjuk, hogyan ezt megteheti.

#### <a name="azure-machine-learning-model-building"></a>Azure gépi tanulási modell épület

##### <a name="choice-of-learner"></a>Tanuló megválasztása

Először is azt kell választania egy tanuló. Döntési fa csillapítja két osztály használni a tanuló fogjuk. Az alábbiakban a tanuló a alapértelmezett beállításait:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

A kísérlet az fogjuk válassza az alapértelmezett értékeket. Akkor ne feledje, hogy az alapértelmezett általában jól érthető és jó módszer gyors alaptervek első teljesítményét. Paraméterek abszolút, ha úgy dönt, hogy ha befejezte az alapterv javíthatja a teljesítményt.

#### <a name="train-the-model"></a>A modell képzése

– Oktatás, akkor egyszerűen meghívása a **Vonaton modell** modul. A két bemeneti adatok alapján rá, hogy a két szintű csillapítja döntési fa tanuló és a vonaton adatkészlet. Ez az alábbi látható:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>A modell pontszám

Miután felkínálunk egy képzett modellt, azt készen áll a próba adathalmazban pontszám, és a teljesítmény ki szeretné számítani. Hajtanak végre az alábbi ábrán egy **Felmérése modell** modul együtt látható **Pontszámhoz modell** modulról szerint:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>5 lépés: A modell felmérése

Végezetül azt szeretné modell teljesítményelemző. A két class (bináris) besorolás problémákat egy jó mérték rendszerint a AUC. Ez ábrázolásához azt be a csatlakoztatni egy **Felmérése modell** modulba a **Pontszámhoz modell** modul ehhez. **Megjelenítés** parancsra a **Modell felmérése** modul együttesen egy képet, az alábbihoz hasonlóan:

![A modul táblázatos adatbázisba kerülő modell felmérése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

Bináris (vagy két osztály) besorolás problémák, egy jó előrejelzési pontosság mértéke áll a terület alatt görbe (AUC). A következő akkor jelenik meg az eredmények a modell használata a próba adatkészlet. Úgy juthat az Ez, kattintson a jobb gombbal a kimeneti port a **Modell felmérése** modul, majd a **Megjelenítés**.

![Felmérése modell modul ábrázolása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Lépés a 6: A modell közzé egy webszolgáltatásból
Az azt jelenti, hogy az Azure gépi tanulási modell közzé könnyen legalább webhely-szolgáltatások egy körben elérhetővé tétele a értékes szolgáltatást. Miután befejeződött, amely, bárki a webszolgáltatás az, hogy az előrejelzések szükségük van, és a webszolgáltatás használja az a modell ezeket az előrejelzések bemeneti adatok hívásokat is folytathat.

Ehhez, azt először menti a képzett modell képzett modell objektumként. Ez történik, a jobb gombbal a **Vonaton modell** modul használja a **Mentés másként képzett modell** lehetőséget.

Ezután azt kell létrehoznia bemeneti és kimeneti esetében a webszolgáltatás portokat:

* a beviteli port adatok ugyanabban a formában, mint az előrejelzések szükséges adatokat vesz igénybe.
* egy kimenet port a a pontszáma címkéket és a kapcsolódó valószínűség adja eredményül.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Jelölje be a bemeneti port néhány adatsorok

Érdemes a egy **SQL-átalakítás alkalmazásához** modul használatával jelölje ki a bemeneti port adatok szolgáló csak 10 sorokat. Jelölje be a csak ezek a bemeneti használatával az itt ismertetett SQL-lekérdezés a(z) adatsort:

![Beviteli port adatok](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webszolgáltatás
Most, hogy készen áll egy kis kísérlet közzététele a weben szolgáltatás használható futtatásához.

#### <a name="generate-input-data-for-webservice"></a>A webszolgáltatás bemeneti adatok létrehozása

Zeroth lépésként mivel nagy, a darabszám táblázat azt készíthet tesztadatokat néhány sor, és kimeneti adatai készítése belőlük darab funkcióival. Ez a szolgálhat a webszolgáltatás bemeneti adatok formátumát. Ez az alábbi látható:

![Táblázatos adatbázisba kerülő bemeneti adatok létrehozása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] A bemeneti adatok formátumot használja a **Darab Featurizer** modul KIMENETÉT. Miután a kísérlet futásának, mentése a kimenet a **Darab Featurizer** modulból adatkészletet. Ez az adatkészlet a bemeneti adatok az a webszolgáltatás szolgál.

#### <a name="scoring-experiment-for-publishing-webservice"></a>A közzétételi webszolgáltatás a kísérlet pontozás

Első lépésként bemutatjuk ennek néz ki. Alapvető struktúrájára **Pontszám modell** modul, amely a képzett modell objektum és azt használja a **Darab Featurizer** modul az előző lépésekben létrehozott bemeneti adatok néhány sornyi fogadja el. Projekt meg a Scored címkéket és az eredmény valószínűség "Kiválasztása oszlopok az adatkészlet" használjuk.

![Jelölje ki az oszlopot az adatkészlet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Figyelje meg, hogy miként az **Oszlopok kiválasztása az adatkészlet** modulban használható 'szűrésére"adatok egy adatkészletet. Akkor jelenik meg az alábbi tartalma:

![Az oszlopok kiválasztása az adatkészlet modulban a szűrés](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

A kék bemeneti és kimeneti portok megjelenítéséhez, egyszerűen kattintson a jobb alsó **webszolgáltatás előkészítése** . A kísérlet futó is lehetővé teszi, hogy velünk a webszolgáltatás közzététele: kattintson a **Közzététel WEBSZOLGÁLTATÁS** ikonra a jobb alsó sarokban a itt látható:

![Webszolgáltatás közzététele](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Miután a webszolgáltatás közzétette, a első így néz ki egy weblapot átvált:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

A bal oldalon láthatja két hivatkozást problémák megoldásához segítséget:

* A **KÉRÉS/válasz** szolgáltatás (vagy Erőforrásrekordok) készült egyetlen előrejelzések és milyen azt csatlakozást az e workshop.
* A **KÖTEG végrehajtás** szolgáltatás (BES) köteg előrejelzések szolgál, és van szükség az, hogy a bemeneti adatok Azure Blob-tárolóhoz tárolnak előrejelzések létesítéséhez használatos.

A **KÉRÉS/válasz** időt vesz igénybe us létrejön oldalak hivatkozásra kattintva előre konzerv kód, a C#, python és a j Ez a kód esetében a webszolgáltatás hívása kényelmesen használható. Megjegyzés: ezen a lapon az API-ja kulcs elvégzendő hitelesítéshez.

Érdemes a python kód másolása felett új cella IPython a jegyzetfüzetben.

Itt bemutatjuk python kód egy szakaszában a megfelelő API kulccsal.

![Python kódot.](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Figyelje meg, hogy a azt az alapértelmezett API kulcs cseréli a problémák megoldásához segítséget API billentyűt. A következő válasz kattintva **futtassa** a cella egy IPython jegyzetfüzetben együttesen:

![IPython válasz](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Láthatja, hogy a két teszt példák azt az Android outlookró (JSON keretén belül a python parancsfájl), hogy válaszokat vissza formájában "Scored címkét, Scored valószínűség". Figyelje meg, hogy ebben az esetben választottunk az alapértelmezett érték, amely a előre konzerv kódot tartalmaz (0 összes numerikus oszlopokban, mind a karakterlánc az kockák oszlopok "érték").

Ez megállapítja, a végpontok közötti forgatókönyv megjelenítő Azure gépi tanulási használatával nagyméretű adatkészlet kezelése. Hogy egy adjon az adatok lépések, összeállítás előrejelzési modell, és a felhőben webes szolgáltatás üzembe.
