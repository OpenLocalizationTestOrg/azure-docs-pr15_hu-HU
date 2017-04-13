<properties
    pageTitle="Adatok feltárása és modellezése a külső |} Microsoft Azure"
    description="A külső MLlib eszközkészlet adatok feltárása és modellezési funkciók bővíthető."
    services="machine-learning"
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="data-exploration-and-modeling-with-spark"></a>Adatok feltárása és modellezése a külső

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ez a forgatókönyv használja HDInsight a külső adatok feltárása és bináris besorolás feladatok modellezése egy mintája szerepel a következőt: a regressziós utazás taxiköltség és 2013 adatkészlet fizetési.  Azt végigvezeti a az [Adatok tudományos folyamat](http://aka.ms/datascienceprocess), a végpontok közötti, használja az HDInsight külső fürt feldolgozásra és Azure BLOB-tárolja az adatokat, és az adatmodellek. A folyamat tallózása, és megjeleníti az adatokat egy tároló Azure Blob vett fel, és majd előkészíti a cserélendő modellek össze az adatokat. Ezek a modellek a külső MLlib eszközkészlet használata bináris osztályozás és a regressziós modellezési feladatok elvégzésére build.

- A **bináris besorolás** tevékenység-e a tipp az utazás kifizetett előrejelzésére. 
- Az egyéb tipp jellemzők alapján tipp mennyiségét előrejelzésére, akkor a **regressziós** feladatot. 

A modellek használjuk logisztikus és lineáris regressziós, véletlen erdők és átmenetes csillapítja fák tartalmazza:

- [A SGD lineáris regressziós](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) egy lineáris regressziós modell Stochastic színátmenet süllyedési (SGD) módszer használó és optimalizáló és szolgáltatás méretezés tipp összegek előre kifizetett. 
- [A LBFGS logisztikus regresszió](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) vagy a "logit" regressziós regressziós modell, amely is használható, amikor a függő változó adatok besorolás végezze el a kockák. LBFGS egy kvázi Newton optimalizálási algoritmus, amely megközelíti a Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmus memóriát korlátozott mennyiségű használatával és gépi tanulási körben használható.
- [Véletlen erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes.  Overfitting kockázata csökkentheti hány döntési fák egyesítheti őket. Véletlen erdők segítségével regressziós és besorolását képes kezelni a kockák funkciók és kiterjeszthetők a multiclass besorolás beállítása. Ezeket a szolgáltatás méretezése nincs szükség, és képesek nemlinearitás rögzítése és a kapcsolati funkciót. Véletlen erdők a legtöbb sikeres gépi tanulási a modellek osztályozás és a regressziós közül.
- [Színátmenet csillapítja fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) döntési fák együttes. GBTs láthatja el ismeretekkel a döntési fák ismételt minimalizálásához veszteség függvényt. GBTs segítségével regressziós és besorolását kockák szolgáltatások képes kezelni, szolgáltatás méretezése nincs szükség, és nemlinearitás rögzítése és a kapcsolati funkció. Azok is használható a multiclass-besorolás beállítása.

A modellezési lépéseket is tartalmazhatnak képzése, felmérése és mentése a különböző típusú modell ábrázoló kódot. A megoldás kód és kattintva jelenítse meg a megfelelő felvétel Python használatban van.   


>[AZURE.NOTE] Bár a külső MLlib eszközkészlet tervezve a nagy adathalmazok, viszonylag kis minta (170K sorokat, az eredeti a következőt: adatkészlet körülbelül 0,1 %-át használatával ~ 30 Mb) használatos itt kényelmesebbé. Az itt megadott torna fut hatékony (KB) egy HDInsight fürt 2 dolgozó csomópontokkal. A ugyanazt kódot, kisebb módosításokkal nagyobb-készletek, megfelelő módosításával az adatok a memóriában gyorsítótár és fürt méretének módosítása feldolgozása használható.

## <a name="prerequisites"></a>Előfeltételek

Azure-fiók és egy HDInsight külső meg kell egy HDInsight 3.4 külső 1,6 fürthöz az útmutató elvégzéséhez szükséges. Nézze meg az utasításokat a [Áttekintése adatok tudományos Azure hdinsight szolgáltatáshoz a külső használatával](machine-learning-data-science-spark-overview.md) való ezeknek a követelményeknek. Témakör is itt használt Taxi a következőt: 2013 adatokat és a külső fürt Jupyter jegyzetfüzet hajt végre kódot útmutatást leírását tartalmazza. Az ebben a témakörben mintakódok tartalmazó **pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb** Jegyzetfüzet [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)érhető el. 


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Telepítés: tárolási helye, tárak és az előre beállított külső környezetben

Külső el tudja olvasni és Azure tároló Blob (más néven WASB) szeretne írni. Tárolt valamelyik meglévő adatait, nincs dolgozható külső és az eredmények tárolt újra WASB használatával.

Ahhoz, hogy WASB modellek vagy fájlok mentése, az elérési utat kell megfelelő módon kell megadni. Az alapértelmezett tároló a külső fürthöz csatolt egy elérési utat kezdődő hivatkozhat: "wasb: / / /". Más helyekről alapján hivatkozunk "wasb: / /".


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Tárolási helye a könyvtár elérési WASB beállítása

A következő kódot minta olvasható az adatok és a modell tároló könyvtár, amelyhez a modell kimenet mentésekor elérési helyét adja meg:


    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Tárak importálása

Állítson be is szükség van szükség tárak importálása. A külső környezet beállítása, és importálja a szükséges tárak kódot a következő:


    # IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Előre definiált külső környezet és PySpark magics

A PySpark mag Jupyter jegyzetfüzetek által biztosított van egy előre definiált környezetben. Így nem kell beállítania a külső vagy struktúra környezetek kifejezetten az alkalmazás használata megkezdése előtt fejlesztéséhez. Ezek a környezetek érhetők el, alapértelmezés szerint. Ezek a környezetek a következők:

- sc – a külső 
- sqlContext - struktúra számára

A PySpark kernel tartalmaz néhány előre definiált "magics", melyek, amelyek a felhívhatja speciális parancsok %%. Nincsenek két olyan parancsokat, az alábbi mintakódok használt.

- **%% helyi** Megadja, hogy a kód, a további vonalak helyileg futtatható. Kód érvényes Python kódot kell lennie.
- **%%sql -o <variable name>** A sqlContext ellen struktúra lekérdezés végrehajtása. Ha a -o paramétert a lekérdezés eredményének állandó-e a a %% helyi Python környezetben, mint egy Pandas DataFrame.
 

További információt a mag Jupyter jegyzetfüzetekben és az előre definiált "magics", amely a biztosítanak, lásd: [mag HDInsight HDInsight külső Linux fürt Jupyter jegyzetfüzetek érhető el](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).
 

## <a name="data-ingestion-from-public-blob"></a>A nyilvános blob adatok bevitel

Az adatok tudományos folyamat első lépésként a forrásból származó elemezni kívánt adatokat ingest hol van az adatok feltárása és modellezése környezetbe helyezkedik el. A környezet külső szerepel az útmutató. Ebben a szakaszban a kódot, és fejezze be a tevékenységek sorozatának tartalmazza:

- az adatok minta modellezni ingest
- olvassa el a beviteli adathalmazban (tárolt .tsv-fájlként)
- formázhatja, és az adatok letisztázásának
- Hozzon létre és objektumok (RDDs vagy adatok-keretek) a memóriában gyorsítótár
- Regisztráljon az SQL-környezetben temp-táblázatként azt.

Az alábbiakban adatok szempontjából a kódot.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
    
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**KIMENET:**

Cella fölötti végrehajtani készített idő: 51.72 másodpercig


## <a name="data-exploration--visualization"></a>Adatok feltárása és a képi megjelenítés

A külső adatok üzembe, ha az adatok tudományos folyamat lépés, az adatok feltárása és a Megjelenítés alaposabb ismereteket szerezhet. Ebben a részben azt vizsgálja meg az SQL lekérdezésekkel taxi adatokat, így a cél változók és a leendő szolgáltatások vizuális ellenőrzésre. Kifejezetten azt a taxi utakat, a gyakoriság tipp összegek és hogyan tippek típusától függően változnak kifizetés összege, és írja be a személy megszámolja gyakoriságának ábrázolni.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Rajzolja meg a személy darab gyakoriságok taxi utakat minta hisztogram

A kód és a későbbi kódrészletek a SQL mágikus használatával lekérdezheti a minta és a helyi mágikus az adatok ábrázolásához.

- **SQL mágikus (`%%sql`)** A HDInsight PySpark kernel támogatja a sqlContext egyszerűen beágyazott HiveQL lekérdezéseket. A (-o VARIABLE_NAME) argumentumot is fennáll, az SQL-lekérdezés eredményét egy Pandas DataFrame Jupyter kiszolgálói szerint. Ez azt jelenti, hogy a helyi módban érhető el.
- A ** `%%local` mágikus** futtatásához használt kód helyi meghajtóra, amely a headnode a HDInsight fürt Jupyter kiszolgálón. Általában használni `%%local` színű együtt a `%%sql` színű -o paraméterrel. A -o paraméter szeretné továbbra is fennáll helyileg az SQL-lekérdezés eredményét majd %% helyi mágikus kódtöredék szemben a helyi meghajtóra megőrződnek SQL-lekérdezések kimenetét helyi futtatásához a következő készlete szeretne elindítani.

A kimenet automatikusan formájában jelenik meg a kód futtatása után.

A lekérdezés olvassa be a személy darabszám szerint utakat. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Ez a kód hoz létre egy helyi adatok keret a lekérdezés eredménye, és rajzolja meg az adatokat. A `%%local` mágikus létrehoz egy helyi adatok-keretben, `sqlResults`, a matplotlib megrajzolásához az használható. 

>[AZURE.NOTE] A PySpark mágikus többször szerepel az útmutató. Ha az adatok mennyiségét túl nagy, a kell példa a elférő adatok-keret létrehozása a helyi memóriában.

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Az alábbiakban a kódot, és rajzolja meg a utakat személy száma szerint

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**KIMENET:**

![Utazás gyakoriság személy darabszám szerint](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Választhat számos különböző típusú megjelenítések (táblázatot, kör, sor, terület vagy sáv) között található gombokkal **típus** menü a jegyzetfüzet. A sáv ábra itt jelenik meg.
    
### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Tipp: összegek, és hogyan tipp összeg személy darab és a jegy ára összegek változik hisztogram ábrázolni.

SQL-lekérdezés a mintaadatok használja.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE
    
    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Ez a kód cella az SQL-lekérdezés használatával hozzon létre három felvétel az adatokat.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**KIMENET:** 

![Tipp: az összeg felosztása](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Tipp: az összeg személy darabszám szerint](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tipp: az összeg jegy ára összeg](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Ez a funkció modellezési műszaki, átalakítási és adatok előkészítése
Ez a szakasz ismerteti és a kód menetét előkészíthetik az adatokat a Machine Learning modellezési való használatra. Azt szemlélteti, hogyan lehet hajtsa végre az alábbi műveleteket:

- Hozzon létre egy új szolgáltatást binning órával a forgalom idő időszakok
- Tárgymutató- és kódolását kockák funkciók
- Az Machine Learning függvények bemeneti címkézett pont-objektumok létrehozása
- Véletlen alszint példákat talál arra az adatok létrehozása, és ossza képzés, és a kifejezéskészletek vizsgálata
- Ez a funkció méretezése
- A memóriában gyorsítótár-objektumok


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Hozzon létre egy új szolgáltatást binning órával a forgalom idő időszakok

Ez a kód jeleníti meg, hogyan hozhat létre egy új szolgáltatást binning óra forgalom idő időszakok be és az eredményül kapott adatok keret a memóriában gyorsítótárazásának. Ha többször rugalmassá elosztott adatkészleteket (RDDs) és az adatok-keretek használnak, továbbfejlesztett végrehajtási időket gyorsítótárazás vezet. Ennek megfelelően azt gyorsítótár RDDs és adat-keretek a forgatókönyv több szakaszában. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**KIMENET:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Tárgymutató- és kódolását modellezési funkciók bevitt kockák szolgáltatásai

Ebben a szakaszban a tárgymutató- vagy a modellezési funkciók bevitt kockák funkcióinak kódolását mutatja. Modellezési és MLlib funkcióit igénylő szolgáltatások, amelyeknek az indexelt és kódolt használata előtt kockák bemeneti adatok előrejelzésére. Attól függően, hogy a modellt kell indexelni vagy különböző módokon kódolását őket:  

- **Fastruktúrájú-alapú modellezési** igényel numerikus értékeket vehet fel kell kódolva kategóriákat (például egy szolgáltatást a három kategóriába is lehet kódolt 0; 1; 2). Ez a MLlib [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) függvény által biztosított. Ez a funkció az egyik oszlopra, amely a címke gyakoriságok szerint rendezett címke tárgymutatók használata címkék karakterlánc oszlop kódolja. Bár az indexelt beviteli és adatok kezelése numerikus értékű, a fastruktúrájú-alapú algoritmusok megfelelően kezelné megfelelően kategóriák adható meg. 

- **Logistic és a regressziós modellek** megkövetelése kódolást egy gyorsbillentyűk használatát, ahol, például egy szolgáltatást a három kategóriába kibontható három szolgáltatás oszlopba, az egyes tartalmazó 0 vagy 1, attól függően, hogy egy megfigyelés kategóriájára. MLlib Itt [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) függvényét kódolást egy gyorsbillentyűk használatát. A kódoló címke indexek oszlopa bináris módszereket, értékű legfeljebb egyetlen egy-egy oszlopban rendeli hozzá. A kódolás lehetővé teszi, hogy a várt numerikus értéket tartalmazó funkciókat, például a logisztikus regresszió kockák funkcióinak alkalmazza az algoritmusok.

Az alábbiakban a kódot az index és a kockák szolgáltatások kódolását:


    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

Cella fölötti végrehajtani készített idő: 1.28 másodpercig

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Az Machine Learning függvények bemeneti címkézett pont-objektumok létrehozása

Ez a témakör a kódot, amely bemutatja, hogyan tárgymutató-kockák szöveges adatok címkézett pont adattípusként és kódolását azt, hogy képzése és MLlib logisztikus regresszió és egyéb besorolás modellek tesztelése szolgál. Címkével ellátott pont objektumai rugalmassá elosztott adatkészleteket (RDD) formázott oly módon, mint a bemeneti adatok Machine Learning algoritmusok MLlib a legtöbb van szükség. [Címkével ellátott pont](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektoros, vagy a sűrű, vagy a ritka, társított címke/választ.  

Ez a témakör a kódot, amely bemutatja, hogyan tárgymutató-kockák szöveges adatok [pont címkézett](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) adattípusként és kódolását azt, hogy képzése és MLlib logisztikus regresszió és egyéb besorolás modellek tesztelése szolgál. Címkével ellátott pont objektumai rugalmassá elosztott adatkészleteket (RDD) tartalmazó egy címke (cél/válasz változó), valamint a szolgáltatás vektoros. Ez a formátum számos Machine Learning algoritmusok MLlib a bemeneti adataiként van szüksége.

Az alábbiakban a kódot az index és a bináris osztályozási text funkcióinak kódolását.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Az alábbiakban a tárgymutató-kockák text funkcióinak lineáris regressziós elemzésre és kódolását kódot.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Véletlen alszint példákat talál arra az adatok létrehozása, és ossza képzés, és a kifejezéskészletek vizsgálata

Ez a kód véletlen példákat talál arra az adatok (25 %-át az alábbi használt) hoz létre. Nem szükséges ebben a példában az adatkészlet miatt méretét, de azt mutatja be, hogyan akkor is minta itt így tudja, hogyan kell használni a saját probléma, szükség esetén. Nagy minták esetén ez lehet jelentős oktatás modellek időt takaríthat meg. Ezután azt felosztása a minta egy oktatás (75 %-át az alábbi) és a tesztelés részét (Itt 25 %) osztályozás és a regressziós modellezési használni.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

Cella fölötti végrehajtani készített idő: 0,24 másodpercig


### <a name="feature-scaling"></a>Ez a funkció méretezése

Szolgáltatás beosztását, más néven adatok normalizálás adatblokkok, hogy szolgáltatások, amelyeknek körben folyósított értékek vannak nem adott fölösleges összehasonlítani a objektív függvény. A szolgáltatás méretezés kódját a [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) méretezni a szolgáltatásokat az egység eltérése használja. Azt által biztosított MLlib a lineáris regressziós a Stochastic színátmenetes süllyedési (SGD), a népszerű algoritmus képzési számos más gépi tanulási modellek például rendeződik hátrányait vagy a támogatási vektoros gépek (SVM) való használatra.

>[AZURE.NOTE] A LinearRegressionWithSGD algoritmus bizalmas funkció a méretezés kell rendelkeznie talált.

Az alábbiakban a kód skála változók regularized lineáris SGD algoritmus való használatra.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

Cella fölötti végrehajtani készített idő: 13.17 másodpercig


### <a name="cache-objects-in-memory"></a>A memóriában gyorsítótár-objektumok

Tanfolyamok és algoritmusok Machine Learning kipróbálása idő csökkenthető a bemeneti adatok keret objektumok az osztályozás, regressziós, és szolgáltatások méretezett gyorsítótárazás.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:** 

Cella fölötti végrehajtani készített idő: 0,15 másodpercig


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>E vagy sem ezt a tippet bináris besorolás modellekkel kifizetett előrejelzésére

Ebből a szakaszból megtudhatja, hogyan három használata modellek a bináris besorolás tevékenység előrejelzésére, attól függetlenül, hogy ezt a tippet kifizetett taxi utazása. A modell mutatják be a következő:

- Logisztikus regresszió rendeződik 
- Véletlen erdő modell
- Színátmenet teljesítményfokozó fák

Minden egyes kódot tartalmazó szakaszban épület modell oszlik lépéseket: 

1. **Modell oktatás** adatokat egy paraméter megadása
2. A mértékek próba adatkészlet **modell értékelése**
3. A **modell mentése** a blob-jövőbeli felhasználás

### <a name="classification-using-logistic-regression"></a>Minősítés logisztikus regresszió alapján

Ebben a szakaszban a kód láthatja el ismeretekkel a felmérése és mentse egy logisztikus regresszió modell = esetben előrejelzése, vagy sem a tipp a következőt: taxi utazás és jegy ára adathalmazban utazása kifizetett [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) mutatja.

**A KtgE és hyperparameter abszolút logisztikus regresszió modell képzése**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    
    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET:** 

Együtthatók: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

METSZ:-0.0111216486893

Cella fölötti végrehajtani készített idő: 14.43 másodpercig

**A szokásos mértékek bináris besorolás modell felmérése**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**KIMENET:** 

ÁR területen = 0.985297691373

ROC területen = 0.983714670256

Összefoglaló stat

Pontosság = 0.984304060189

Visszahívása = 0.984304060189

Az F1 Pontszám = 0.984304060189

Cella fölötti végrehajtani készített idő: 57.61 másodpercig

**A ROC görbe ábrázolni.**

A *predictionAndLabelsDF* táblázatként *tmp_results*az előző cellában van regisztrálva. *tmp_results* használható do lekérdezések és a kimeneti eredmények keretbe sqlResults adatok-megrajzolásához. Az alábbiakban a kódot.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Az alábbiakban az előrejelzések indíthat, és rajzolja meg a ROC görbe a kódot.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()
    

**KIMENET:**

![Logisztikus regresszió ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)


### <a name="random-forest-classification"></a>Véletlen erdő besorolás

Ebben a szakaszban a kód képzése, felmérése és mentése a véletlen erdő modell, amely = esetben előrejelzése, vagy sem a tipp a következőt: taxi utazás és jegy ára adathalmazban utazása kifizetett mutatja.
    
    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

ROC területen = 0.985297691373

Cella fölötti végrehajtani készített idő: 31.09 másodpercig


### <a name="gradient-boosting-trees-classification"></a>Színátmenet teljesítményfokozó fák besorolás

Ebben a szakaszban a kód láthatja el ismeretekkel a, értékelni, és mentse a színátmenet teljesítményfokozó fák modell, amely = esetben előrejelzése, vagy sem ezt a tippet be a következőt: taxi utazás utazása kifizetett és adatkészlet fizetési mutatja.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET:**

ROC területen = 0.985297691373

Cella fölötti végrehajtani készített idő: 19.76 másodpercig


## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Tipp összegek regressziós modellek taxi utakat előrejelzésére

Ebből a szakaszból megtudhatja, hogyan használata három modell a regressziós tevékenység előrejelzésére mennyiségét a tipp: a kifizetett egyéb tipp jellemzők alapján taxi utazás. A modell mutatják be a következő:

- Lineáris regressziós rendeződik
- Véletlen erdő
- Színátmenet teljesítményfokozó fák

Ezek a modellek szerepelnek a bevezetőben. Minden egyes kódot tartalmazó szakaszban épület modell oszlik lépéseket: 

1. **Modell oktatás** adatokat egy paraméter megadása
2. A mértékek próba adatkészlet **modell értékelése**
3. A **modell mentése** a blob-jövőbeli felhasználás

### <a name="linear-regression-with-sgd"></a>Lineáris regressziós SGD együtt 

Ebben a szakaszban a kód egy lineáris regressziós optimalizálása stochastic színátmenet süllyedési (SGD) használó betanítása méretezett lehetőségeinek haszná, és hogyan pontszám felmérése és mentse a modellt az Azure Blob-tároló (WASB) látható.

>[AZURE.TIP] A saját környezete szakaszban lehet a LinearRegressionWithSGD modellek közelítését kapcsolatos problémák, és a paraméterek kell megváltozott/optimalizálva gondosan érvényes modell beszerzése. Méretezés változót jelentősen konvergencia megkeresheti. 


    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

Együtthatók: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

METSZ: 0.853872718283

GYÁNE = 1.24190115863

R-sqr = 0.608017146081

Cella fölötti végrehajtani készített idő: 58.42 másodpercig


### <a name="random-forest-regression"></a>Véletlen erdő regressziós

Ebben a szakaszban a kód képzése, felmérése és mentése = esetben a tipp összege a következőt: taxi utazás adatok előrejelzése véletlen erdő visszavonás mutatja.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

GYÁNE = 0.891209218139

R-sqr = 0.759661334921

Cella fölötti végrehajtani készített idő: 49.21 másodpercig


### <a name="gradient-boosting-trees-regression"></a>Színátmenet teljesítményfokozó fák regressziós

Ebben a szakaszban a kód képzése, felmérése és mentése a színátmenet teljesítményfokozó fák modell, amely = esetben a tipp összege a következőt: taxi utazás adatok előrejelzése mutatja.

**Képzése és felmérése**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

GYÁNE = 0.908473148639

R-sqr = 0.753835096681

Cella fölötti végrehajtani készített idő: 34.52 másodpercig

**Ábrázolása**

az előző cellára struktúra táblázatként *tmp_results* regisztrált. A táblázat eredménye keretbe *sqlResults* adatok-megrajzolásához az eredményt ad. Az alábbiakban a kódot.

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Az alábbiakban a kódot, és rajzolja meg az adatokat a Jupyter kiszolgálót használ.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)
    

**KIMENET:**

![Tényleges – és – előre jelzett-tipp-lépésekben](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

    
## <a name="clean-up-objects-from-memory"></a>A memória objektumok karbantartása

Használat `unpersist()` a gyorsítótárban tárolt objektumok törlése.
        
    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Rekord tárolási helye a felhasználás és pontozási modellek

Felhasználása és egy független adatkészlet ismertetett pontszám az [pontszámhoz és a külső beépített gépi tanulási modellek értékeli](machine-learning-data-science-spark-model-consumption.md) témakör kell másolása és beillesztése a mentett modellek itt létrehozott be a felhasználás Jupyter jegyzetfüzetet tartalmazó fájlnevekben. Az alábbiakban a kódot, és nyomtassa ki van szüksége modellfájlt elérési útvonalát.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**KIMENET**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"


## <a name="whats-next"></a>Mi az következő?

Most, hogy a regressziós és besorolását modellek a külső MlLib hozott létre, készen áll pontszám, és ezek a modellek felmérése című témakörből. Speciális adatok feltárása és modellezése a jegyzetfüzet dives mélyebb határokon érvényesítés, hyper paraméter abszolút, beleértve azokat, és kiértékelés modell. 

**Modell felhasználás:** Pontszám és az osztályozás és a regressziós létrehozott modellek ebben a témakörben felmérése című témakörben talál [pontszámhoz és a külső beépített gépi tanulási modellek kiértékelése](machine-learning-data-science-spark-model-consumption.md).

**Idegen-ellenőrzési és hyperparameter abszolút**: lásd: az [adatok feltárása és modellezése a külső speciális](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) meg, hogyan lehet a modellek képzett abszolút határokon-ellenőrzési és hyper paraméter használatával



