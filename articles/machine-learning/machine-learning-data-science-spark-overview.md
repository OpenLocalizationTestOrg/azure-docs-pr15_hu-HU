<properties
    pageTitle="Áttekintése használatával Azure hdinsight szolgáltatáshoz a külső adatok tudományos |} Microsoft Azure"
    description="A külső MLlib eszközkészlet életre jelentős gépi tanulási modellezési funkciók a elosztott HDInsight-környezetet."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Adatok tudományos külső használata Azure hdinsight szolgáltatáshoz – áttekintés

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ez az témakörök csomagja megtudhatja, hogy hogyan HDInsight a külső adatok tudományos gyakori műveletek, például adatok bevitel, szolgáltatás műszaki, modellezési és a modell kiértékelés befejezéséhez. A használt adata egy mintája szerepel az 2013 következőt: taxi utazás és jegy ára adatkészlet. A beépített modellek logisztikus és lineáris regressziós, véletlen erdők és átmenetes csillapítja fák tartalmazzák. A témakörök a e modellek tárolása a Azure blob-tárolóhoz (WASB) módját és a pontszám és a prediktív teljesítmény felmérése is megjelennek. Speciális témakörök terjed ki, hogyan lehet a modellek képzett abszolút határokon-ellenőrzési és hyper paraméter használatával. Áttekintés témakör is ismerteti a külső fürt a biztosított három forgatókönyvek a lépések elvégzéséhez szükséges beállítása. 

[A külső](http://spark.apache.org/) egy megnyitott-forrás párhuzamos, amely támogatja a memóriában feldolgozás a teljesítmény nagy adatok analitikus alkalmazások keretrendszer feldolgozása. A külső feldolgozás motor sebesség, használni, és összetett analytics Kezeléstechnikai épül. Külső meg a memóriában elosztott számítási funkciók tehető közelítéses algoritmusok kinek ajánljuk gépi tanulási és graph számítások. [MLlib](http://spark.apache.org/mllib/) külső 's méretezhető gépi tanulási egy dokumentumtárba, amelynek életre adatmodellezési funkciókat kínál a elosztott környezetben. 

[HDInsight külső](../hdinsight/hdinsight-apache-spark-overview.md) az Azure tárolt ajánlja fel a külső forrásból Megnyitás. A külső fürt, amely futtatását is lehetővé teszi az átalakítás, szűrés és megjelenítésére, az Azure BLOB (WASB) tárolt adatok interaktív külső SQL-lekérdezések **Jupyter PySpark jegyzetfüzetek** támogatása is tartalmaz. PySpark külső a Python API-t. A kódtöredék, amely a megoldásokat, és telepítve van a külső fürt a Jupyter jegyzetfüzetekben itt futtatása adatok ábrázolásához vonatkozó felvétel megjelenítése. Az alábbi témakörök modellezési lépéseit, amely bemutatja, hogyan képzése, felmérése, mentse, és felhasználása modell hibatípusonként-kódot tartalmaz. 

A beállítási lépéseket és az útmutató megadott kódot HDInsight 3.4 külső 1,6 szolgál. Jó helyen jár, a kód itt és a jegyzetfüzeteket az általános és bármely külső fürt kell dolgozni. Alkalmazás használatakor nem HDInsight külső, lehet, hogy a fürt beállítása és kezelése a lépések némileg eltér az itt látható.

## <a name="prerequisites"></a>Előfeltételek

1. kell rendelkeznie egy Azure-előfizetést. Ha a nem már rendelkezik egy, olvassa el az [első Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. szüksége van egy HDInsight 3.4 külső 1,6 fürthöz az útmutató elvégzéséhez. Hozzon létre egyet, lásd: a utasításokat [első lépések: Apache külső létrehozása az Azure hdinsight szolgáltatáshoz a](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). A **Fürt típus válassza a** menüből fürt típusa és verziója van megadva. 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Ez a témakör bemutatja, hogyan Python, hanem Scala használata egy végpont adatok tudományos folyamat feladatok elvégzésére olvassa el az [adatok tudományos az Azure a külső Scala használatával](machine-learning-data-science-process-scala-walkthrough.md)című témakört.

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>A következőt: 2013 Taxi adatok

A következőt: Taxi utazás adatok körülbelül 20 GB-nyi tömörített vesszővel elválasztott értékek (CSV) fájlok (nem tömörített ~ 48 GB), minden út kifizetett 173 milliónál egyes utakat és a díjakat. Utazás rekordokat felfelé kiválasztása és Gyűjtőtár helyét és idő, anonimizált támadó szoftver (illesztőprogram) licencszámmal, és medallion (taxi tartozó egyedi azonosító) számot tartalmazza. Az adatok minden utakat bemutatja a 2013-ben és az egyes az a következő két adatkészleteket megadva:

1. A "trip_data" CSV-fájlok utazás részleteket, például számától tartalmaz, emelje fel és dropoff mutat, út időtartama és utazás hossza. Íme néhány példa rekordot:

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


Hogy ezeket a fájlokat a 0,1 % minta venni, és az utazás csatlakozott\_adatokat, és utazás\_fizetési CVS-fájl átalakítása egyetlen adatkészlet, mint a bemeneti adatkészlet használható ez a forgatókönyv. Bekapcsolódás utazás egyedi billentyűvel\_adatokat, és utazás\_jegy ára tevődik össze a mezők: medallion, támadó szoftver\_engedély és a felvétel\_datetime. Az adathalmaz minden egyes rekord az alábbi attribútumok Taxi a következőt: utazás, amely tartalmazza:


|A mező| Rövid ismertetése
|------|---------------------------------
| medallion |Anonimizált taxi medallion (taxi egyedi azonosító)
| hack_license |    Anonimizált Hackney kocsi licencszámmal
| vendor_id |   Taxi szállító azonosítója
| rate_code | Jegy ára mértéke a következőt: taxi
| store_and_fwd_flag | Előre jelző és tárolása
| pickup_datetime | Dátum és idő elhozatala
| dropoff_datetime | Dropoff dátum és idő
| pickup_hour | Válassza az óra
| pickup_week | Az év hetének elhozatala
| Weekday | Weekday (tartomány 1 – 7)
| passenger_count | A taxi utazás számától
| trip_time_in_secs | A másodpercben megadott üzenetváltási idővel
| trip_distance | Mérföld utazás távolság
| pickup_longitude | Válassza a hosszúság
| pickup_latitude | Szélesség elhozatala
| dropoff_longitude | Dropoff hosszúság
| dropoff_latitude | Dropoff szélesség
| direct_distance | Közvetlen választása közötti távolság be és dropoff helyek
| payment_type | Fizetés típusát (hitelesítésszolgáltatók, hitelkártya stb.)
| fare_amount | Jegy ára összege
| emelt díjas | Emelt díjas
| mta_tax | MTA adó
| tip_amount | Tipp: az összeg
| tolls_amount | Autópályadíjak összege
| total_amount | Végösszeg
| Formabontó | Formabontó (0 és 1 nem vagy Igen)
| tip_class | Tipp: a class (0: 0 $ 1: $0-5, 2: 6-10 USD, 3: $11 – 20, 4: > $20)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>A külső fürt Jupyter jegyzetfüzet hajt végre kódot 

Az Azure portálról Jupyter jegyzetfüzet indíthatja el. Keresse meg a külső fürt az irányítópulton, és kattintson rá a fürt kezelése lapon adja meg. Nyissa meg a jegyzetfüzetet, a külső fürt társított, kattintson a **Fürt irányítópultok** -> **Jupyter jegyzetfüzetet** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

***Https://CLUSTERNAME.azurehdinsight.net/jupyter*** a Jupyter jegyzetfüzetek eléréséhez is tallózással. Az URL-címe CLUSTERNAME részének helyettesítése a saját fürt neve. Szüksége van a jegyzetfüzetek eléréséhez a rendszergazdai fiók jelszava.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Jelölje ki a PySpark néhány példa a PySpark API-t használó előre csomagolt jegyzetfüzetek tartalmazó könyvtár megjelenítéséhez. A jegyzetfüzetek, amelyeknél a mintakódok, ez a témakör a külső csomagja érhetők el a [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


A külső fürt feltöltheti a jegyzetfüzeteket közvetlenül az Github Jupyter jegyzetfüzet kiszolgálóra. A Jupyter a Kezdőlap lapon kattintson a képernyő jobb oldali részén **feltöltése** gombjára. A Fájlkezelőben nyílik meg. Itt illesztheti a jegyzetfüzet Github (nyers tartalom) URL-CÍMÉT, és kattintson a **Megnyitás**gombra. Az alábbi URL-címeit, a PySpark jegyzetfüzetek érhetők el:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Látható a fájl nevét a Jupyter fájllista egy **feltöltése** gombra kattintva újra. Kattintson a **Feltöltés** gombra. Most már importált a jegyzetfüzetet. Ismételje meg ezeket a lépéseket követve töltse fel a jelen útmutató a jegyzetfüzeteket.

> [AZURE.TIP] Kattintson a jobb gombbal a hivatkozások a böngészőben, és válassza a **Hivatkozás másolása** : get github nyers tartalom URL-CÍMÉT. Ez az URL-CÍMÉT beillesztheti a Jupyter feltöltése explorer párbeszédpanel jelenik meg.

Most már van lehetősége:

- Kattintson a jegyzetfüzet olvassa el a kódot.
- Hajtsa végre a minden cella **A SHIFT + ENTER**billentyűkombinációval.
- A teljes jegyzetfüzetet indítása **cellától** -> **futtatni**.
- A lekérdezések automatikus megjelenítés használja.

> [AZURE.TIP] A PySpark kernel automatikusan megjeleníti a kimenet (HiveQL) SQL-lekérdezések. Lehetősége van beállítani található gombokkal **típus** menü a jegyzetfüzet között a képi megjelenítések (táblázatot, kör, sor, terület vagy sáv) számos különböző típusú kijelölése:

![Az általános megközelítés logisztikus regresszió ROC görbe](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Mi az következő?

Most, hogy egy HDInsight külső fürthöz beállítása és a Jupyter jegyzetfüzetek feltöltött, készen áll a témakörök, amelyek megfelelnek a három PySpark jegyzetfüzetek szeretné. Adatai feltárása hogyan, és ezután hogyan hozhat létre, és az adatmodellek felhasználása mutatnak be. A speciális adatok feltárása és modellezése a jegyzetfüzet határokon érvényesítés, abszolút, a hyper-paraméter tartalmazza, és modellezése a kiértékelés mutatja. 

**Adatok feltárása és modellezése a külső:** Ismerje meg az adatkészlet és létrehozásához mutatószám és felmérése a gép modellek tanulási által végezhető műveletek a [bináris osztályozás és a regressziós adatok a külső MLlib eszközkészlet modellekkel létrehozása](machine-learning-data-science-spark-data-exploration-modeling.md) című témakört.

**Modell felhasználás:** Megtudhatja, hogy miként pontszám az osztályozás és a regressziós létrehozott modellek ebben a témakörben, lásd: [pontszámhoz és a külső beépített gépi tanulási modellek kiértékelése](machine-learning-data-science-spark-model-consumption.md).

**Idegen-ellenőrzési és hyperparameter abszolút**: lásd: az [adatok feltárása és modellezése a külső speciális](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) meg, hogyan lehet a modellek képzett abszolút határokon-ellenőrzési és hyper paraméter használatával

