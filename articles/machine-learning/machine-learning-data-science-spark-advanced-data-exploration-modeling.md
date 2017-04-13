<properties
    pageTitle="Adatok feltárása és modellezése a külső speciális |} Microsoft Azure"
    description="Használja a HDInsight a külső adatok feltárása és láthatja el ismeretekkel a bináris osztályozás és a regressziós modellek határokon-ellenőrzési és hyperparameter optimalizálási használatával."
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

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Speciális adatok feltárása és modellezése a külső 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

A forgatókönyv HDInsight a külső adatok feltárása és láthatja el ismeretekkel a bináris osztályozás és a regressziós modellek határokon-ellenőrzés használatával elemre, és hyperparameter optimalizálása mintájából a következőt: utazás taxiköltség 2013 adatkészlet fizetési. Azt végigvezeti a az [Adatok tudományos folyamat](http://aka.ms/datascienceprocess), a végpontok közötti, használja az HDInsight külső fürt feldolgozásra és Azure BLOB-tárolja az adatokat, és az adatmodellek. A folyamat tallózása, és megjeleníti az adatokat egy tároló Azure Blob vett fel, és majd előkészíti a cserélendő modellek össze az adatokat. A megoldás kód és kattintva jelenítse meg a megfelelő felvétel Python használatban van. Ezek a modellek a külső MLlib eszközkészlet használata bináris osztályozás és a regressziós modellezési feladatok elvégzésére build. 

- A **bináris besorolás** tevékenység-e a tipp az utazás kifizetett előrejelzésére. 
- Az egyéb tipp jellemzők alapján tipp mennyiségét előrejelzésére, akkor a **regressziós** feladatot. 

A modellezési lépéseket is tartalmazhatnak képzése, felmérése és mentése a különböző típusú modell ábrázoló kódot. A témakör ismerteti az azonos alapokról témaként az [adatok feltárása és modellezése a külső](machine-learning-data-science-spark-data-exploration-modeling.md) részét. De rá az "összetettebb", hogy is használja határokon-ellenőrzési együtt az optimális pontos osztályozás és a regressziós modellek betanítása abszolút hyperparameter. 

**Idegen-ellenőrzési (KtgE)** a, hogy mennyire képzett adatok ismert csoportja a modell generalizes az adatkészleteket, amikor azt még nem lett képzett funkciók előrejelzésére értékeli technika. Az általános mögött Ez a módszer lényege modell van képzett a ismert adatok adatkészletet, és kattintson annak az előrejelzések pontosságának egy független adatkészlet van tesztelése. A közös megvalósított, hogy egy adathalmaz K éleket oszthatja, és ezután képzése a modell összes, de az éleket közül ciklikus módon. 

**Hyperparameter optimalizálási** általában az a cél optimalizálja a algoritmus teljesítmény-független adatkészlet történő változásának mértéke egy tanulási algoritmus hyperparameters készletének kiválasztása problémája. **Hyperparameters** értékeket meg kell adni a modell oktatás eljárás kívül. Ezek az értékek feltételezéseket hatással lehet a rugalmas és a pontosság modellek. Döntési fák hyperparameters, például a kívánt magassági és a fában Falevelek például van. Támogatás vektoros gépek (SVMs) szükség beállítás téves besorolás büntetés kifejezés. 

Hajtsa végre az itt használt hyperparameter optimalizálás gyakori módja a rács keresés, vagy egy **paraméter takarítás**. Ez egy tanuló algoritmus keresztül az értékek egy teljes körű keresést meghatározott részhalmazát hyperparameter térköz elvégzéséhez áll. Több érvényességi is adja meg a teljesítmény metrikus rendezése a rács keresési algoritmus készített optimális eredményeit. Használt segít korlát problémákat, például oktatóanyag adatok modell overfitting az, hogy a modell megtartja kapacitása ahhoz, hogy az adatok, ahonnan az oktatóanyag adatok kibontása lett általános csoportjának alkalmazása abszolút hyperparameter Önéletrajz.

A modellek használjuk logisztikus és lineáris regressziós, véletlen erdők és átmenetes csillapítja fák tartalmazza:

- [A SGD lineáris regressziós](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) egy lineáris regressziós modell Stochastic színátmenet süllyedési (SGD) módszer használó és optimalizáló és szolgáltatás méretezés tipp összegek előre kifizetett. 
- [A LBFGS logisztikus regresszió](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) vagy a "logit" regressziós regressziós modell, amely is használható, amikor a függő változó adatok besorolás végezze el a kockák. LBFGS egy kvázi Newton optimalizálási algoritmus, amely megközelíti a Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmus memóriát korlátozott mennyiségű használatával és gépi tanulási körben használható.
- [Véletlen erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes.  Overfitting kockázata csökkentheti hány döntési fák egyesítheti őket. Véletlen erdők segítségével regressziós és besorolását képes kezelni a kockák funkciók és kiterjeszthetők a multiclass besorolás beállítása. Ezeket a szolgáltatás méretezése nincs szükség, és képesek nemlinearitás rögzítése és a kapcsolati funkciót. Véletlen erdők a legtöbb sikeres gépi tanulási a modellek osztályozás és a regressziós közül.
- [Színátmenet csillapítja fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) döntési fák együttes. GBTs láthatja el ismeretekkel a döntési fák ismételt minimalizálásához veszteség függvényt. GBTs segítségével regressziós és besorolását kockák szolgáltatások képes kezelni, szolgáltatás méretezése nincs szükség, és nemlinearitás rögzítése és a kapcsolati funkció. Azok is használható a multiclass-besorolás beállítása.

Példák a KtgE és Hyperparameter modellezése takarítás jelennek meg a bináris besorolás probléma. Egyszerűbb példák (nélkül paraméter halmokat) jelennek meg a fő témakörben regressziós feladatokhoz. De a függelékben verzióval rugalmas nettó lineáris regressziós és KtgE és paramétert a takarítás véletlen erdő regressziós használata érvényességi is mutatnak. A **nettó rugalmas** a regressziós rendeződik módszer lineáris regressziós modellek illesztésére kombináló lineárisan a L1 és 2 mértékek, a [Szabadkézi kijelölés](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) és [peremmel](https://en.wikipedia.org/wiki/Tikhonov_regularization) módszerek következményekkel.   



>[AZURE.NOTE] Bár a külső MLlib eszközkészlet tervezve a nagy adathalmazok, viszonylag kis minta (170K sorokat, az eredeti a következőt: adatkészlet körülbelül 0,1 %-át használatával ~ 30 Mb) használatos itt kényelmesebbé. Az itt megadott torna fut hatékony (KB) egy HDInsight fürt 2 dolgozó csomópontokkal. A ugyanazt kódot, kisebb módosításokkal nagyobb-készletek, megfelelő módosításával az adatok a memóriában gyorsítótár és fürt méretének módosítása feldolgozása használható.


## <a name="prerequisites"></a>Előfeltételek

Azure-fiók és egy HDInsight külső meg kell egy HDInsight 3.4 külső 1,6 fürthöz az útmutató elvégzéséhez szükséges. Nézze meg az utasításokat a [Áttekintése adatok tudományos Azure hdinsight szolgáltatáshoz a külső használatával](machine-learning-data-science-spark-overview.md) való ezeknek a követelményeknek. Témakör is itt használt Taxi a következőt: 2013 adatokat és a külső fürt Jupyter jegyzetfüzet hajt végre kódot útmutatást leírását tartalmazza. Az ebben a témakörben mintakódok tartalmazó **pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** Jegyzetfüzet [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)érhető el.


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
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**KIMENET**

DateTime.DateTime (2016, 4, a 18, a 17, a 36, a 27-es, a 832799)


### <a name="import-libraries"></a>Tárak importálása

Importálás szükséges tárak kódot a következő:

    # LOAD PYSPARK LIBRARIES
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


## <a name="data-ingestion-from-public-blob"></a>A nyilvános blob bevitel adatokat: 

Az első lépés a adatok tudományos folyamat is ingest forrásokból, amely tartalmazza az adatok feltárása és adatmodellezést az elemezni kívánt adatokat. Ez a környezete külső az útmutató. Ebben a szakaszban a kódot, és fejezze be a tevékenységek sorozatának tartalmazza:

- az adatok minta modellezni ingest
- olvassa el a beviteli adathalmazban (tárolt .tsv-fájlként)
- formázhatja, és az adatok letisztázásának
- Hozzon létre és objektumok (RDDs vagy adatok-keretek) a memóriában gyorsítótár
- Regisztráljon az SQL-környezetben temp-táblázatként azt.

Az alábbiakban adatok szempontjából a kódot.


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
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET**

Cella fölötti végrehajtani készített idő: 276.62 másodpercig


## <a name="data-exploration--visualization"></a>Adatok feltárása és a képi megjelenítés 

A külső adatok üzembe, ha az adatok tudományos folyamat lépés, az adatok feltárása és a Megjelenítés alaposabb ismereteket szerezhet. Ebben a részben azt vizsgálja meg az SQL lekérdezésekkel taxi adatokat, így a cél változók és a leendő szolgáltatások vizuális ellenőrzésre. Kifejezetten azt a taxi utakat, a gyakoriság tipp összegek és hogyan tippek típusától függően változnak kifizetés összege, és írja be a személy megszámolja gyakoriságának ábrázolni.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Rajzolja meg a személy darab gyakoriságok taxi utakat minta hisztogram

A kód és a későbbi kódrészletek a SQL mágikus használatával lekérdezheti a minta és a helyi mágikus az adatok ábrázolásához.

- **SQL mágikus (`%%sql`)** A HDInsight PySpark kernel támogatja a sqlContext egyszerűen beágyazott HiveQL lekérdezéseket. A (-o VARIABLE_NAME) argumentumot is fennáll, az SQL-lekérdezés eredményét egy Pandas DataFrame Jupyter kiszolgálói szerint. Ez azt jelenti, hogy a helyi módban érhető el.
- A ** `%%local` mágikus** futtatásához használt kód helyi meghajtóra, amely a headnode a HDInsight fürt Jupyter kiszolgálón. Általában használni `%%local` színű együtt a `%%sql` színű -o paraméterrel. A -o paraméter szeretné továbbra is fennáll helyileg az SQL-lekérdezés eredményét majd %% helyi mágikus kódtöredék szemben a helyi meghajtóra megőrződnek SQL-lekérdezések kimenetét helyi futtatásához a következő készlete szeretne elindítani.

A kimenet automatikusan formájában jelenik meg a kód futtatása után.

A lekérdezés olvassa be a személy darabszám szerint utakat. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Ez a kód hoz létre egy helyi adatok keret a lekérdezés eredménye, és rajzolja meg az adatokat. A `%%local` mágikus létrehoz egy helyi adatok-keretben, `sqlResults`, a matplotlib megrajzolásához az használható. 

>[AZURE.NOTE] A PySpark mágikus többször szerepel az útmutató. Ha az adatok mennyiségét túl nagy, a kell példa a elférő adatok-keret létrehozása a helyi memóriában.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Az alábbiakban a kódot, és rajzolja meg a utakat személy száma szerint

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**KIMENET**

![Személy darabszám szerint utakat gyakorisága](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Választhat számos különböző típusú megjelenítések (táblázatot, kör, sor, terület vagy sáv) között található gombokkal **típus** menü a jegyzetfüzet. A sáv ábra itt jelenik meg.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Tipp: összegek, és hogyan tipp összeg személy darab és a jegy ára összegek változik hisztogram ábrázolni.

SQL-lekérdezés a mintaadatok használja.
    
    # SQL SQUERY
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

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**KIMENET:** 

![Tipp: az összeg felosztása](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tipp: az összeg személy darabszám szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tipp: az összeg jegy ára összeg szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Ez a funkció modellezési műszaki, átalakítási és adatok előkészítése

Ez a szakasz ismerteti és a kód menetét előkészíthetik az adatokat a Machine Learning modellezési való használatra. Azt szemlélteti, hogyan lehet hajtsa végre az alábbi műveleteket:

- Hozzon létre egy új szolgáltatást binning órával a forgalom idő időszakok
- Index és a meleg kódolását kockák funkciók
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

**KIMENET**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Tárgymutató- és egy gyorsbillentyűk használatát kódolását kockák funkciók

Ebben a szakaszban a tárgymutató- vagy a modellezési funkciók bevitt kockák funkcióinak kódolását mutatja. Modellezési és MLlib funkcióit igénylő szolgáltatások, amelyeknek az indexelt és kódolt használata előtt kockák bemeneti adatok előrejelzésére. 

Attól függően, hogy a modellt kell indexelni vagy különböző módokon kódolását őket. Például Logistic és a regressziós modellek igényelnek kódolást egy gyorsbillentyűk használatát, ahol, például egy szolgáltatást a három kategóriába kibontható három szolgáltatás oszlopba, az egyes tartalmazó 0 vagy 1, attól függően, hogy egy megfigyelés kategóriájára. MLlib Itt [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) függvényét kódolást egy gyorsbillentyűk használatát. A kódoló címke indexek oszlopa bináris módszereket, értékű legfeljebb egyetlen egy-egy oszlopban rendeli hozzá. A kódolás lehetővé teszi, hogy a várt numerikus értéket tartalmazó funkciókat, például a logisztikus regresszió kockák funkcióinak alkalmazza az algoritmusok.

Az alábbiakban a kódot az index és a kockák szolgáltatások kódolását:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
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


**KIMENET**

Cella fölötti végrehajtani készített idő: 3.14 másodpercig


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Az Machine Learning függvények bemeneti címkézett pont-objektumok létrehozása

Ez a témakör a kódot, amely bemutatja, hogyan tárgymutató-kockák szöveges adatok címkézett pont adattípusként és kódolását azt, hogy képzése és MLlib logisztikus regresszió és egyéb besorolás modellek tesztelése szolgál. Címkével ellátott pont objektumai rugalmassá elosztott adatkészleteket (RDD) formázott oly módon, mint a bemeneti adatok Machine Learning algoritmusok MLlib a legtöbb van szükség. [Címkével ellátott pont](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektoros, vagy a sűrű, vagy a ritka, társított címke/választ.

Az alábbiakban a kódot az index és a bináris osztályozási text funkcióinak kódolását.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
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
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
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

**KIMENET**

Cella fölötti végrehajtani készített idő: 0.31 másodpercig


### <a name="feature-scaling"></a>Ez a funkció méretezése

Szolgáltatás beosztását, más néven adatok normalizálás adatblokkok, hogy szolgáltatások, amelyeknek körben folyósított értékek vannak nem adott fölösleges összehasonlítani a objektív függvény. A szolgáltatás méretezés kódját a [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) méretezni a szolgáltatásokat az egység eltérése használja. Azt által biztosított MLlib a lineáris regressziós a Stochastic színátmenetes süllyedési (SGD), a népszerű algoritmus képzési számos más gépi tanulási modellek például rendeződik hátrányait vagy a támogatási vektoros gépek (SVM) való használatra.   

>[AZURE.TIP] A LinearRegressionWithSGD algoritmus bizalmas funkció a méretezés kell rendelkeznie talált.   

Az alábbiakban a kód skála változók regularized lineáris SGD algoritmus való használatra.

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

**KIMENET**

Cella fölötti végrehajtani készített idő: 11.67 másodpercig


### <a name="cache-objects-in-memory"></a>A memóriában gyorsítótár-objektumok

Tanfolyamok és algoritmusok Machine Learning kipróbálása idő csökkenthető a bemeneti adatok keret objektumok osztályozásához regressziós használt, és szolgáltatások méretezett gyorsítótárazás.

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

**KIMENET** 

Cella fölötti végrehajtani készített idő: 0.13 másodpercig


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>E vagy sem ezt a tippet bináris besorolás modellekkel kifizetett előrejelzésére

Ebből a szakaszból megtudhatja, hogyan három használata modellek a bináris besorolás tevékenység előrejelzésére, attól függetlenül, hogy ezt a tippet kifizetett taxi utazása. A modell mutatják be a következő:

- Logisztikus regresszió 
- Véletlen erdő
- Színátmenet teljesítményfokozó fák

Minden egyes kódot tartalmazó szakaszban épület modell oszlik lépéseket: 

1. **Modell oktatás** adatokat egy paraméter megadása
2. A mértékek próba adatkészlet **modell értékelése**
3. A **modell mentése** a blob-jövőbeli felhasználás

Idegen-ellenőrzési (KtgE) módjáról kétféleképpen abszolút paraméterrel bemutatjuk:

1. A algoritmus bármely algoritmus az MLlib, és minden olyan paraméternek alkalmazható **Általános** egyéni kód használatával állítja be. 
1. A **pySpark CrossValidator folyamat függvény**használatával. Ne feledje, hogy annak ellenére kényelmes és a használat érdekében alapján CrossValidator a külső 1.5.0 néhány korlátozás: 

    - Az adatmodellek folyamat nem lehet a jövőbeli felhasználás mentett/állandó.
    - Egy a modellben szereplő összes paraméterhez nem használhatók.
    - Minden MLlib algoritmus nem használhatók.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Általános érvényességi és hyperparameter abszolút logisztikus regresszió algoritmus használható bináris besorolás

Ebben a szakaszban a kód láthatja el ismeretekkel a felmérése és mentse egy logisztikus regresszió modell = esetben előrejelzése, vagy sem a tipp a következőt: taxi utazás és jegy ára adathalmazban utazása kifizetett [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) mutatja. A modell van képzett érvényességi (KtgE) és a hyperparameter abszolút valamelyik, az oktatóközpont algoritmusok MLlib az alkalmazott egyéni kóddal végrehajtott használatával.   

>[AZURE.NOTE] Egyéni KtgE kód végrehajtását is eltarthat néhány percig, amíg.

**A KtgE és hyperparameter abszolút logisztikus regresszió modell képzése**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET**

Együtthatók: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

METSZ:-0.0111216486893

Cella fölötti végrehajtani készített idő: 14.43 másodpercig


**A szokásos mértékek bináris besorolás modell felmérése**

Ebben a szakaszban a kódot a próba-adatkészlet, beleértve a ROC görbe egy ábrázolása ellen logisztikus regresszió modell ki szeretné számítani mutatja.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
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
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET**

ÁR területen = 0.985336538462

ROC területen = 0.983383274312

Összefoglaló stat

Pontosság = 0.984174341679

Visszahívása = 0.984174341679

Az F1 Pontszám = 0.984174341679

Cella fölötti végrehajtani készített idő: 2.67 másodpercig


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
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
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
    

**KIMENET**

![Az általános megközelítés logisztikus regresszió ROC görbe](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**A probléma továbbra is fennáll a jövőbeli felhasználás blob mintával**

Ebben a szakaszban a kódot a logisztikus regresszió modell fogyasztási mentése mutatja.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**KIMENET**

Cella fölötti végrehajtani készített idő: 34.57 másodpercig


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>Logisztikus regresszió (rugalmas regressziós) modell MLlib's CrossValidator folyamat függvény használata

Ebben a szakaszban a kód láthatja el ismeretekkel a felmérése és mentse egy logisztikus regresszió modell = esetben előrejelzése, vagy sem a tipp a következőt: taxi utazás és jegy ára adathalmazban utazása kifizetett [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) mutatja. A modell van képzett érvényességi (KtgE) és a hyperparameter abszolút végrehajtani a MLlib CrossValidator folyamat függvénnyel a KtgE takarítás paraméter használatával.   


>[AZURE.NOTE] MLlib Önéletrajz kód végrehajtását is eltarthat néhány percig, amíg.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**KIMENET**

Cella fölötti végrehajtani készített idő: 107.98 másodpercig


**A ROC görbe ábrázolni.**

A *predictionAndLabelsDF* táblázatként *tmp_results*az előző cellában van regisztrálva. *tmp_results* használható do lekérdezések és a kimeneti eredmények keretbe sqlResults adatok-megrajzolásához. Az alábbiakban a kódot.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Az alábbiakban a ROC görbe ábrázolandó a kódot.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
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


**KIMENET**

![Logisztikus regresszió ROC görbe MLlib's CrossValidator használatával](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Véletlen erdő besorolás

Ebben a szakaszban a kód képzése, felmérése és mentése = esetben előrejelzése, vagy sem a tipp a következőt: taxi utazás és jegy ára adathalmazban utazása kifizetett véletlen erdő visszavonás mutatja.


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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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


**KIMENET**

ROC területen = 0.985336538462

Cella fölötti végrehajtani készített idő: 26.72 másodpercig


### <a name="gradient-boosting-trees-classification"></a>Színátmenet teljesítményfokozó fák besorolás

Ebben a szakaszban a kód láthatja el ismeretekkel a, értékelni, és mentse a színátmenet teljesítményfokozó fák modell, amely = esetben előrejelzése, vagy sem ezt a tippet be a következőt: taxi utazás utazása kifizetett és adatkészlet fizetési mutatja.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
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

**KIMENET**

ROC területen = 0.985336538462

Cella fölötti végrehajtani készített idő: 28.13 másodpercig


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Tipp: összeg előrejelzésére regressziós modellekkel (nem használ KtgE)

Ebből a szakaszból megtudhatja, hogyan használata három modell a regressziós tevékenység előrejelzésére mennyiségét a tipp: a kifizetett egyéb tipp jellemzők alapján taxi utazás. A modell mutatják be a következő:

- Lineáris regressziós rendeződik
- Véletlen erdő
- Színátmenet teljesítményfokozó fák

Ezek a modellek szerepelnek a bevezetőben. Minden egyes kódot tartalmazó szakaszban épület modell oszlik lépéseket: 

1. **Modell oktatás** adatokat egy paraméter megadása
2. A mértékek próba adatkészlet **modell értékelése**
3. A **modell mentése** a blob-jövőbeli felhasználás   


>AZURE Megjegyzés: Keresztté-ellenőrzési nem használatos ebben a szakaszban a három regressziós modellekkel mivel ez volt látható logisztikus regresszió modellek részletesen. Példa használata a KtgE rugalmas nettó lineáris regressziós ábrázoló kapja: Ez a témakör a függelékben.


>AZURE Megjegyzés: A saját környezete szakaszban lehet LinearRegressionWithSGD modellek közelítését kapcsolatos problémákat, és a paraméterek kell megváltozott/optimalizálva gondosan beszerzése érvényes modell. Méretezés változót jelentősen konvergencia megkeresheti. Ez a témakör mellékletének látható rugalmas nettó regressziós is LinearRegressionWithSGD helyett használható.


### <a name="linear-regression-with-sgd"></a>Lineáris regressziós SGD együtt

Ebben a szakaszban a kód egy lineáris regressziós optimalizálása stochastic színátmenet süllyedési (SGD) használó betanítása méretezett lehetőségeinek haszná, és hogyan pontszám felmérése és mentse a modellt az Azure Blob-tároló (WASB) látható.

>[AZURE.TIP] A saját környezete szakaszban lehet a LinearRegressionWithSGD modellek közelítését kapcsolatos problémák, és a paraméterek kell megváltozott/optimalizálva gondosan érvényes modell beszerzése. Méretezés változót jelentősen konvergencia megkeresheti.


    # LINEAR REGRESSION WITH SGD 

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
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET**

Együtthatók: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

METSZ: 0.854507624459

GYÁNE = 1.23485131376

R-sqr = 0.597963951127

Cella fölötti végrehajtani készített idő: 38.62 másodpercig


### <a name="random-forest-regression"></a>Véletlen erdő regressziós

Ebben a szakaszban a kód képzése, felmérése és mentése a véletlen erdő modell, amely = esetben a tipp összege a következőt: taxi utazás adatok előrejelzése mutatja.   

>[AZURE.NOTE] Idegen-ellenőrzési paraméterrel abszolút egyéni kód használatával a függelékben megadva.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
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

**KIMENET**

GYÁNE = 0.931981967875

R-sqr = 0.733445485802

Cella fölötti végrehajtani készített idő: 25.98 másodpercig


### <a name="gradient-boosting-trees-regression"></a>Színátmenet teljesítményfokozó fák regressziós

Ebben a szakaszban a kód képzése, felmérése és mentése a színátmenet teljesítményfokozó fák modell, amely = esetben a tipp összege a következőt: taxi utazás adatok előrejelzése mutatja.

**Képzése és felmérése**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET**

GYÁNE = 0.928172197114

R-sqr = 0.732680354389

Cella fölötti végrehajtani készített idő: 20.9 másodpercig


**Ábrázolása**
    
az előző cellára struktúra táblázatként *tmp_results* regisztrált. A táblázat eredménye keretbe *sqlResults* adatok-megrajzolásához az eredményt ad. Az alábbiakban a kódot.

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Az alábbiakban a kódot, és rajzolja meg az adatokat a Jupyter kiszolgálót használ.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Tényleges – és – előre jelzett-tipp-lépésekben](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>. Melléklet: A kiegészítő regressziós tevékenységek közötti érvényességi használata paraméter el

A függelék rugalmas nettó használatának lineáris regressziós KtgE módjáról és az egyéni kód használatával a véletlen erdő regressziós paraméter takarítás KtgE módjáról kódot tartalmazza.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Lineáris regressziós nettó elasztikus verzióval érvényességi Cross

Ebben a szakaszban a kód jeleníti meg, hogyan cross rugalmas nettó használatának lineáris regressziós érvényességi és hogyan tesztadatokat szemben a modellhez ki szeretné számítani.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET**

Cella fölötti végrehajtani készített idő: 161.21 másodpercig

**Az R-SQR metrikus felmérése**

az előző cellára struktúra táblázatként *tmp_results* regisztrált. A táblázat eredménye keretbe *sqlResults* adatok-megrajzolásához az eredményt ad. Az alábbiakban a kódot.

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Az alábbiakban a kódot, R – sqr számítja ki.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**KIMENET**

R-sqr = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Adatérvényesítési Cross a paraméter takarítás véletlen erdő regressziós az egyéni kód használatával

Ebben a szakaszban a kód jeleníti meg, hogyan cross érvényesítése az egyéni kód használatával a véletlen erdő regressziós paraméter takarítás és hogyan tesztadatokat szemben a modellhez ki szeretné számítani.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET**

GYÁNE = 0.906972198262

R-sqr = 0.740751197012

Cella fölötti végrehajtani készített idő: 69.17 másodpercig


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Objektumok memória és a nyomtatási modell helyekről karbantartása

Használat `unpersist()` a gyorsítótárban tárolt objektumok törlése.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
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


**KIMENET**

PythonRDD [122], a PythonRDD.scala RDD: 43


**Nyomat modellfájlt felhasználási jegyzetfüzetben használandó elérési útvonalát.** Felhasználása, és egy független adatkészlet pontszám szüksége másolja és illessze be a fájlnevekben "felhasználási jegyzetfüzetben".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**KIMENET**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Mi az következő?

Most, hogy a regressziós és besorolását modellek a külső MlLib hozott létre, készen áll pontszám, és ezek a modellek felmérése című témakörből.

**Modell felhasználás:** Pontszám és az osztályozás és a regressziós létrehozott modellek ebben a témakörben felmérése című témakörben talál [pontszámhoz és a külső beépített gépi tanulási modellek kiértékelése](machine-learning-data-science-spark-model-consumption.md).
