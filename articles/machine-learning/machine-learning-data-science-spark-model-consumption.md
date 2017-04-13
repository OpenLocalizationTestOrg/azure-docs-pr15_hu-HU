<properties
    pageTitle="A külső beépített gépi tanulási modellek pontszám |} Microsoft Azure"
    description="Hogyan lehet az Azure Blob-tároló (WASB) tárolt pontszámhoz tanulási modellek."
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

# <a name="score-spark-built-machine-learning-models"></a>A külső beépített gépi tanulási modellek pontszám 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ez a témakör azt ismerteti, hogyan gépi tanulási (Machine Learning) modellek, amely a külső MLlib használatával beépített betöltése, és Azure Blob-tároló (WASB), és hogyan pontszám őket az adatkészleteket, amely WASB is megtalálható tárolja. Azt mutatja, hogyan előre folyamat a bemeneti adatok, a MLlib eszközkészlet indexelési és kódolási függvényeivel szolgáltatások átalakítása, és hogyan hozhat létre a címkével ellátott pont adatobjektumok bemeneteként a Machine Learning modellekkel pontozási használható. A pontozási használt modellek lineáris regressziós, logisztikus regresszió, véletlen erdő modellek és átmenetes szolgáló lehetőségekről fa modellek tartalmazzák.


## <a name="prerequisites"></a>Előfeltételek

1. Azure-fiók és egy HDInsight külső meg kell egy HDInsight 3.4 külső 1,6 fürthöz az útmutató elvégzéséhez szükséges. Nézze meg az utasításokat a [Áttekintése adatok tudományos Azure hdinsight szolgáltatáshoz a külső használatával](machine-learning-data-science-spark-overview.md) való ezeknek a követelményeknek. Témakör is itt használt Taxi a következőt: 2013 adatokat és a külső fürt Jupyter jegyzetfüzet hajt végre kódot útmutatást leírását tartalmazza. Az ebben a témakörben mintakódok tartalmazó **pySpark-machine-learning-data-science-spark-model-consumption.ipynb** Jegyzetfüzet [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)érhető el.

2. A gépi tanulási modellek munka – az [adatok feltárása és modellezése a külső](machine-learning-data-science-spark-data-exploration-modeling.md) témakör itt kiértékelhető is létre kell hoznia.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Telepítés: tárolási helye, tárak és az előre beállított külső környezetben

A külső el tudja olvasni és írni az Azure tároló Blob (WASB). Tárolt valamelyik meglévő adatait, nincs dolgozható külső és az eredmények tárolt újra WASB használatával.

Ahhoz, hogy WASB modellek vagy fájlok mentése, az elérési utat kell megfelelő módon kell megadni. Az alapértelmezett tároló a külső fürthöz csatolt egy elérési utat kezdődő hivatkozhat: *"wasb / /"*. A következő kódot minta olvasható az adatok és a modell tároló könyvtár, amelyhez a modell kimenet mentésekor elérési helyét adja meg. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Tárolási helye a könyvtár elérési WASB beállítása

Menti a modellek: "wasb: / / / felhasználó/remoteuser/NYCTaxi/modellek". Ha az elérési út nem megfelelően van beállítva, modellek nem töltődnek a pontozási.

A scored eredmények mentette a: "wasb: / / / felhasználó/remoteuser/NYCTaxi/ScoredResults". Ha nem a megfelelő mappa elérési útját, eredmények nem menti abban a mappában.   


>[AZURE.NOTE] A fájl elérési útja helyeken másolható és a helyőrzőket, a **machine-learning-data-science-spark-data-exploration-modeling.ipynb** jegyzetfüzet utolsó cellájának eredményéből kód a beillesztett.   


Az alábbiakban a kód útjainak beállítása: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**KIMENET:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Tárak importálása

A külső környezet beállítása, és importálja a kódot a következő szükséges tárak

    #IMPORT LIBRARIES
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

A PySpark mag Jupyter jegyzetfüzetek által biztosított van egy előre definiált környezetben. Így nem kell beállítania a külső vagy struktúra környezetek kifejezetten az alkalmazás használata megkezdése előtt fejlesztéséhez. Ezek alapértelmezés szerint elérhető. Ezek a környezetek a következők:

- sc – a külső 
- sqlContext - struktúra számára

A PySpark kernel tartalmaz néhány előre definiált "magics", melyek, amelyek a felhívhatja speciális parancsok %%. Nincsenek két olyan parancsokat, az alábbi mintakódok használt.

- **%% helyi** A megadott helyileg hajtja végre a következő sorok kódot. Kód érvényes Python kódot kell lennie.
- **%% sql -o<variable name>** 
- A sqlContext ellen struktúra lekérdezés végrehajtása. Ha a -o paramétert a lekérdezés eredményének állandó-e a a %% helyi Python környezetben, mint egy Pandas dataframe.
 

További információt a mag Jupyter jegyzetfüzetekben és az előre definiált "magics", amely a biztosítanak, lásd: [mag HDInsight HDInsight külső Linux fürt Jupyter jegyzetfüzetek érhető el](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Adatok ingest és érintett adatok keret létrehozása

Ez a témakör az adatok kiértékelhető ingest szükséges tevékenységek sorozatának a kódját. Olvassa el az illesztett 0,1 % minta taxi utazás és jegy ára (tárolt fájl .tsv-fájlként), formátum az adatokat, és ekkor létrehoz egy könnyen áttekinthető adatok keret.

A taxi utazás és jegy ára fájlokat volt az illesztés alapján meghatározott az eljárás a: [a csapat adatok tudományos folyamat működését: HDInsight Hadoop fürt használatával](machine-learning-data-science-process-hive-walkthrough.md) témakört.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
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
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

Cella fölötti végrehajtani készített idő: 46.37 másodpercig


## <a name="prepare-data-for-scoring-in-spark"></a>Pontozás a külső adatok előkészítése 

Ez a szakasz megtudhatja, hogy miként index, kódolását és előkészítheti őket a való használatra felügyelt MLlib tanulási algoritmusok osztályozás és a regressziós kockák funkciók méretezni.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Ez a funkció átalakítása: index, és kódolását modellekkel pontozási bevitt kockák szolgáltatásai 

Ebből a szakaszból megtudhatja, hogyan tárgymutató-kockák az adatoknak a `StringIndexer` és szolgáltatások, amelyeknek kódolását `OneHotEncoder` be a modellek bemeneti.

A [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kódolja karakterlánc oszlop címkék címke indexek oszlopra. Az indexek gyakoriságok címke szerint vannak rendezve. 

A [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) címke indexek oszlopa bináris módszereket, értékű legfeljebb egyetlen egy-egy oszlopban rendeli hozzá. A kódolás lehetővé teszi, hogy a várt folyamatos értékelni funkciókat, például a logisztikus regresszió kockák funkcióinak alkalmazza az algoritmusok.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
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
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

Cella fölötti végrehajtani készített idő: 5.37 másodpercig


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Szolgáltatás tömbök modellek az adatbevitelt RDD-objektumok létrehozása

Ez a témakör a kódot, amely bemutatja, hogyan tárgymutató-kockák szöveges adatok RDD objektumként és egy gyorsbillentyűk használatát kódolását, így láthatja el ismeretekkel és MLlib logisztikus regresszió és az adatmodellek fa-alapú tesztelése használható. Az indexelt adatokat [Rugalmas elosztott adatkészlet (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objektumok vannak tárolva. Ezek a külső egyszerű kivételére. Az objektum RDD elemeket, amelyek a külső párhuzamosan is üzemeltetett megváltoztatható, particionált gyűjteménye jelöli.

Is tartalmaz, kódot, amely mutatja az adatok a `StandardScalar` MLlib nyújtotta a lineáris regressziós a Stochastic színátmenetes süllyedési (SGD), a népszerű algoritmus sokféle gépi tanulási modellek képzési való használatra. Ha át kívánja méretezni a szolgáltatásokat az egység eltérése a [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) használják. Szolgáltatás beosztását, más néven adatok normalizálás adatblokkok, hogy szolgáltatások, amelyeknek körben folyósított értékek vannak nem adott fölösleges összehasonlítani a objektív függvény. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**KIMENET:**

Cella fölötti végrehajtani készített idő: 11.72 másodpercig


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>A regressziós logisztikus modellel pontszám, és mentse a kimenet blob-

Ebben a szakaszban a kód Azure blob-tárolóban lévő mentett logisztikus regressziós modell betöltése, és annak segítségével előre vagy sem ezt a tippet taxi utazás kell fizetni, pontszám azt a szabványos besorolás mértékek, és mentse, majd rajzolja meg az eredmények blob-tárolóhoz mutatja. A scored eredmények RDD objektumok tárolja. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**KIMENET:**

Cella fölötti végrehajtani készített idő: 19.22 másodpercig


## <a name="score-a-linear-regression-model"></a>Egy lineáris regressziós modell pontszám

[LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) használatával képzése egy lineáris regressziós modell Stochastic színátmenet süllyedési (SGD) optimalizálása használatával előre kifizetett tipp mennyiségét. 

Ebben a szakaszban a kód egy lineáris regressziós modell betöltése Azure blob-tárolóhoz, pontszám méretezett változók használatával, és mentse az eredmények vissza az blob mutatja.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**KIMENET:**

Cella fölötti végrehajtani készített idő: 16.63 másodpercig


## <a name="score-classification-and-regression-random-forest-models"></a>Osztályozás és a regressziós véletlen erdő modellek pontszám

Ebben a szakaszban a kód szemlélteti, hogyan lehet a mentett besorolás betöltése és Azure blob-tárolóhoz mentett regressziós véletlen erdő modellek, a teljesítmény szabványos besorolás és a regressziós mértékek pontszám, és mentse az eredmények vissza blob-tárolóhoz.

[Véletlen erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes.  Overfitting kockázata csökkentheti hány döntési fák egyesítheti őket. Véletlen erdők képes kezelni a kockák funkcióiról, használatának kiterjesztése a multiclass besorolás beállítása, nincs szükség a szolgáltatás méretezése és képesek nemlinearitás rögzítése és a kapcsolati funkció. Véletlen erdők a legtöbb sikeres gépi tanulási a modellek osztályozás és a regressziós közül.

[Spark.mllib](http://spark.apache.org/mllib/) véletlen erdők támogatja a bináris és multiclass osztályozás és a regressziós, a folyamatos és az kockák funkciókkal. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**KIMENET:**

Cella fölötti végrehajtani készített idő: 31.07 másodpercig


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Osztályozás és a regressziós színátmenet szolgáló lehetőségekről fa modellek pontszám

Ebben a szakaszban a kód osztályozás és a regressziós színátmenet szolgáló lehetőségekről fa modellek betöltése Azure blob-tárolóhoz, a teljesítmény szabványos besorolás és a regressziós mértékek pontszám, és mentse az eredmények vissza blob-tárolóhoz mutatja. 

**Spark.mllib** GBTs támogatja a bináris osztályozás és a regressziós, a folyamatos és az kockák funkciókkal. 

[Színátmenet teljesítményfokozó fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) döntési fák együttes. GBTs láthatja el ismeretekkel a döntési fák ismételt minimalizálásához veszteség függvényt. GBTs kockák szolgáltatások képes kezelni, szolgáltatás méretezése nincs szükség, és nemlinearitás rögzítése és a kapcsolati funkciót. Azok is használható a multiclass-besorolás beállítása.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**KIMENET:**

Cella fölötti végrehajtani készített idő: 14.6 másodpercig


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Jelenjenek meg a memóriában objektumok és pontszáma helye nyomtatása

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**KIMENET:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>A külső modellek felhasználása egy webes felületén keresztül

A külső lehetővé teszi a köteg feladatok és a többi kapcsolaton keresztül interaktív lekérdezések távolról elküldése az Livius összetevőt. A HDInsight külső fürt alapértelmezés szerint engedélyezve van a Livius. Livius további információkat lásd: [segítségével távolról Livius küldése külső feladatokat](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Segítségével Livius távolról a feladat, amely a köteg értékek elküldése az Azure blob tárolja, és kattintson az eredmények ír egy másik blob fájl. Ehhez a Python parancsfájl feltöltése  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) a blob a külső fürt. Például a **Microsoft Azure tároló Explorer** vagy **AzCopy** eszköz segítségével másolja a parancsfájlt a fürt blob. Abban az esetben ***wasb:///example/python/ConsumeGBNYCReg.py***a parancsfájl feltölteni azt.   


>[AZURE.NOTE] A hívóbetűk arról, hogy meg kell is található külső fürthöz társított tároló fióknak a portálon. 


Miután feltöltött erre a helyre, a parancsfájl belül a külső fürt elosztott környezetben fut. A modell betölti és a előrejelzések futtatása a bemeneti fájlok a modellek alapján.  

Ez a parancsfájl távolról meghívása kérés egy egyszerű HTTPS/többi a Livius.  Az alábbiakban a HTTP-kérés távolról a Python parancsfájl meghívandó Egyenletszerkesztővel curl parancsot. Cserélje ki a megfelelő értékeket a külső fürt CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

A távoli rendszer bármely nyelvre segítségével a külső feladat keresztül Livius meghívásához az alapszintű hitelesítés egy egyszerű HTTPS-hívást.   


>[AZURE.NOTE] A Python kérések könyvtár használata, ha a HTTP-hívást kényelmes lenne, de alapértelmezés szerint a Azure funkciók jelenleg nincs telepítve. Így régebbi HTTP-tárak használja helyette.   


Az alábbiakban a HTTP-híváshoz a Python kódot:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Python kód szeretne elindítani egy külső feladat elküldése, hogy értékek blob alapján különböző eseményeket egy időzítő, létrehozása és frissítése blob- [Azure függvények](https://azure.microsoft.com/documentation/services/functions/) is hozzáadhat. 

Ha inkább a kód ingyenes ügyfél használhatóság, a HTTP-művelet definiáló **Logika alkalmazások Designer** , és állítsa a paramétereken pontozási külső köteg indítása az [Azure logika alkalmazások](https://azure.microsoft.com/documentation/services/app-service/logic/) használatával. 

- Azure portálról választva hozhat létre új logika alkalmazás **+ Új** -> **webes + Mobile** -> **Logika alkalmazást**. 
- **Logika alkalmazások Designer**bármelyikére, adja meg nevét, valamint a logika alkalmazás alkalmazás szolgáltatás megtervezése.
- Jelölje ki a HTTP-művelet, és adja meg a paramétereket, az alábbi ábrán látható:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Mi az következő? 

**Idegen-ellenőrzési és hyperparameter abszolút**: lásd: az [adatok feltárása és modellezése a külső speciális](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) meg, hogyan lehet a modellek képzett abszolút határokon-ellenőrzési és hyper paraméter használatával.
