<properties
    pageTitle="Azure használatával Scala és a külső adatok tudományos |} Microsoft Azure"
    description="Hogyan használhatók Scala felügyelt gépi tanulási tevékenységek a külső méretezhető MLlib és a külső Machine Learning csomagok egy Azure hdinsight szolgáltatáshoz külső fürt."  
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
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Azure használatával Scala és a külső adatok tudományos

Ez a cikk bemutatja, hogyan Scala használni a külső méretezhető MLlib és a külső Machine Learning csomagok egy Azure hdinsight szolgáltatáshoz külső fürt felügyelt gépi tanulási tevékenységek. Azt bemutatja a műveleteket, az [adatok tudományos folyamata](http://aka.ms/datascienceprocess)felépítő: adatok bevitel és adatfeltáró, ábrázoló, szolgáltatás műszaki, modellezési és modell felhasználás. A modellek című témakörben logisztikus és lineáris regressziós, véletlen erdők és színátmenet csillapítja fák (GBTs), többek között két gyakori felügyelt gépi tanulási feladatokon kívül:

- A probléma regressziós: szövegbevitel taxi utazás tipp összege ($)
- Bináris besorolás: tipp vagy nincs ötlet (1 és 0) taxi utazása előrejelzése

A modellezési folyamat képzési és értékelési egy próba adatkészlet és a pontosság vonatkozó mérőszámok szükséges. Ebben a cikkben megtudhatja, hogyan kell tárolni, ezek a modellek Azure Blob-tárolóban lévő és pontszám és a prediktív teljesítmény felmérése. Ez a cikk bemutatja a Speciális témakörök arról, hogy hogyan optimalizálhatja az adatmodellek abszolút határokon-ellenőrzési és hyper paraméter használatával is. A használt adata egy mintája szerepel az 2013 következőt: taxi utazás és jegy ára adatkészlet GitHub elérhető.

[Scala](http://www.scala-lang.org/), egy nyelvet a Java virtuális gép alapján objektumorientált és funkcionális nyelvi fogalmak egyesíti. Egy méretezhető nyelvet, hogy jól illeszkedik elosztott feldolgozás a felhőben, és Azure külső fürt futtatható.

[A külső](http://spark.apache.org/) egy Megnyitás-forrás párhuzamos feldolgozási keretrendszer, amely támogatja az adatok nagy analytics-alkalmazások a teljesítmény a memóriában feldolgozása. A külső feldolgozás motor sebesség, használni, és összetett analytics Kezeléstechnikai épül. Külső meg a memóriában elosztott számítási funkciók tehető közelítéses algoritmusok kinek ajánljuk gépi tanulási és graph számítások. A [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) csomag egységes magas szintű API-hoz, amelyek segíthetnek keretek létrehozása és finomhangolása gyakorlati gépi tanulási folyamatok adatai fölött beépített biztosít. [MLlib](http://spark.apache.org/mllib/) külső 's méretezhető gépi tanulási tár, amely életre adatmodellezési funkciókat kínál a elosztott környezetben.

[HDInsight külső](../hdinsight/hdinsight-apache-spark-overview.md) az Azure tárolt ajánlja fel a külső forrásból Megnyitás. Azt is tartalmazza a külső fürt Jupyter Scala jegyzetfüzetek támogatását, és futtatását is lehetővé teszi az átalakítás, szűrése és Azure Blob-tárolóban lévő adatok ábrázolása interaktív külső SQL-lekérdezések. A Scala kódtöredék ebben a cikkben a megoldást, és megjelenítheti az adatok ábrázolásához vonatkozó felvétel telepítve van a külső fürt a Jupyter jegyzetfüzetek futnak. Az alábbi témakörök modellezési lépéseiben kódot, amely bemutatja, hogyan képzése, felmérése, mentése és modell hibatípusonként felhasználni.

A beállítási lépéseket, és a kódot a jelen cikkben szereplő, az Azure hdinsight szolgáltatáshoz 3.4 külső 1,6. Jó helyen jár a kód, a jelen cikkben és [Scala Jupyter jegyzetfüzet](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) általános és bármely külső fürt kell dolgozni. Lehet, hogy a fürt beállítása és kezelése a lépések némileg eltér az látható ez a cikk Ha nem használ HDInsight külső.

> [AZURE.NOTE] Ez a témakör bemutatja, hogyan Scala, hanem Python használata egy végpont adatok tudományos folyamat feladatok elvégzésére [Használja az Azure hdinsight szolgáltatáshoz a külső adatok tudományos](machine-learning-data-science-spark-overview.md)című témakör tartalmaz.


## <a name="prerequisites"></a>Előfeltételek

-   Az Azure előfizetéssel kell rendelkeznie. Ha még nem rendelkezik egy, az [Ismerkedés az Azure ingyenes próbaverziójára](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   Az Azure hdinsight szolgáltatáshoz 3.4 külső 1,6 fürt, az alábbi eljárás elvégzéséhez szükséges. Az útmutatást találhat fürt létrehozásához [első lépések: Apache külső létrehozása Azure hdinsight szolgáltatáshoz a](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Fürt típusa és a verzió beállítása a **Fürt típusa** menü.

![HDInsight fürt típusának beállítása](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


A következőt: taxi utazás adatok és az útmutatást hajt végre a külső fürt Jupyter jegyzetfüzet kódot leírását lásd: a vonatkozó szakaszokat az [Áttekintés adatok tudományos Azure hdinsight szolgáltatáshoz a külső használatával](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>A külső fürt Jupyter jegyzetfüzet hajt végre Scala kódot

Az Azure portálról Jupyter jegyzetfüzet indíthatja el. Keresse meg a külső fürt az irányítópulton, és rákattintva a kezelése lapon adja meg a fürt. Ezután kattintson **Fürt irányítópultok**, majd nyissa meg a jegyzetfüzetet a külső fürt társított **Jupyter jegyzetfüzetet** .

![Fürt irányítópult és Jupyter jegyzetfüzetek](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Akkor is Jupyter jegyzetfüzetek eléréséhez a https://&lt;clustername&gt;.azurehdinsight.net/jupyter. *Clustername* cserélje le a csoport nevére. Szüksége van a Jupyter jegyzetfüzetek eléréséhez a rendszergazdai fiók jelszava.

![Nyissa meg a jegyzetfüzetek Jupyter megfelelő fürt nevet](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Jelölje ki a **Scala** , amelynek a PySpark API-t használó előre csomagolt jegyzetfüzetek néhány példa könyvtárában megjelenítéséhez. Modellezési feltárása és pontozás, amely tartalmazza a külső témakörök a csomagja mintakódok Scala.ipynb jegyzetfüzetet használó [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala)található.


A külső fürt feltöltheti a Jupyter jegyzetfüzet kiszolgálóra GitHub közvetlenül a jegyzetfüzetet. A Jupyter kezdőlap lapján kattintson a **Feltöltés** gombra. A Fájlkezelőben illessze be a Scala jegyzetfüzet GitHub (nyers tartalom) URL-CÍMÉT, és kattintson a **Megnyitás**gombra. A Scala jegyzetfüzetet a következő URL-címen érhető el:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Telepítés: Beállított külső és struktúra környezetek, a külső magics és a külső tárak

### <a name="preset-spark-and-hive-contexts"></a>Előre definiált külső és struktúra környezetek

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


A külső mag Jupyter jegyzetfüzetek által biztosított van az előre beállított környezetek. Nem kell a külső explicit módon beállított, vagy az alkalmazás használata előtt struktúra környezetek fejlesztéséhez. Az előre beállított környezetekben a következők:

- `sc`a SparkContext
- `sqlContext`a HiveContext


### <a name="spark-magics"></a>A külső magics

A külső kernel tartalmaz néhány előre definiált "magics", melyek, amelyek a felhívhatja speciális parancsok `%%`. A következő mintakódok használt két ezek a parancsok.

- `%%local`Itt adhatja meg, hogy a kód, a további vonalak helyileg futtatható. A kód érvényes Scala kódot kell lennie.
- `%%sql -o <variable name>`végrehajtja a struktúra lekérdezése `sqlContext`. Ha a `-o` paramétert, a lekérdezés eredményét a megőrződnek a `%%local` a külső adatok keretként Scala környezetben.

A további információt a mag Jupyter jegyzetfüzetek és a előre definiált "magics", amely felhívja a `%%` (például `%%local`), lásd: [mag HDInsight HDInsight külső Linux fürt Jupyter jegyzetfüzetek érhető el](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Tárak importálása

A külső MLlib és más szüksége lesz az alábbi kód használatával tárak importálni.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Adatok bevitel

Az első lépés a adatok tudományos folyamat is ingest az elemezni kívánt adatokat. Az adatok feltárása és modellezése környezetbe külső források vagy rendszerek, amely tartalmazza a tárolt adatokat. Ebben a cikkben, ingest adatai illesztett 0,1 % minta taxi utazás és jegy ára-fájl (tárolt .tsv-fájlként). Az adatok feltárása és modellezése környezete külső. Ebben a szakaszban a kódot, és fejezze be a következő tevékenységek sorozatának tartalmazza:

1. Az adatok és modell tároló könyvtár elérési beállítása.
2. Olvassa el a beviteli adatok megadása (a számítógépen tárolt .tsv-fájlként).
3. Az adatok séma definiálása, és az adatok letisztázásának.
4. Hozzon létre egy érintett adatok keretet, és a memóriában a gyorsítótárban.
5. Regisztráljon az adatok SQLContext ideiglenes táblázatként.
6. A lekérdezés a táblázatot, és importálja az eredmények adatok keret.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Tárolási helye a könyvtár elérési beállítása a Azure Blob-tárolóhoz

A külső elolvashatja és Azure Blob-tárolóhoz írni. Külső használva dolgozza fel a meglévő adatok közül, és kattintson az eredmények tárolására újra Blob-tárolóban lévő.

Blob-tárolóhoz modellek vagy a fájl mentéséhez kell megfelelően adja meg, hogy az elérési útját. Hivatkozás a alapértelmezett tároló kezdődő elérési út segítségével a külső fürt csatolt `wasb:///`. Hivatkozás más helyekre használatával `wasb://`.

A következő kódot minta megadja a modell mentési helyének kell olvasni a bemeneti adatok és csatolva van a külső fürt Blob-tárolóhoz elérési helyét.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Adatok importálása, hozzon létre egy RDD és a séma alapján adatok keret meghatározása

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Kimenet:**

A cella futtatásának: 8 másodpercig.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>A lekérdezés a táblázatot, és importálja a adatok keret eredménye

A tábla jegy ára személy, adatok, és ezt a tippet; ezután lekérdezés sérült, és a külső adatok kiszűrése és nyomtassa ki több sort.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Kimenet:**

fare_amount|passenger_count|tip_amount|Formabontó
-----------|---------------|----------|------
       13.5|            1.0|       2.9|   1.0
       16,0|            2.0-s verziója|       3.4.|   1.0
       10.5|            2.0-s verziója|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Adatok feltárása és megjelenítésekben

Az adatok összhangba külső, miután a adatok tudományos folyamat lépés, mélyebb ismertetése, az adatok feltárása és a Megjelenítés eléréséhez. Ebben a részben, megtekintésével taxi SQL-lekérdezések használatával. Ezután importálja az eredmények a cél változók és vizuális vizsgálati leendő funkcióinak ábrázolandó Jupyter automatikus-megjelenítés funkcióval adatok keretbe.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Adatok ábrázolása helyi és SQL mágikus használatával

Alapértelmezés szerint minden futtatásakor kapott Jupyter jegyzetfüzet kódtöredék kimenetét érhető el, amely a dolgozó csomóponton megőrződnek környezetén belül. Ha minden kiszámítása dolgozó csomópontjait utazás menteni szeretné, és a számítási szükséges összes adatot helyileg a csomóponton Jupyter server (amely a fő csomópont) érhető el, ha a `%%local` mágikus Jupyter kiszolgálói a kódrészletet futtatásához.

- **SQL-mágikus** (`%%sql`). A HDInsight külső kernel egyszerűen beágyazott HiveQL lekérdezéseket SQLContext támogatja. A (`-o VARIABLE_NAME`) argumentumot is fennáll, az SQL-lekérdezés eredményét Jupyter kiszolgálói Pandas adatok keretként. Ez azt jelenti, hogy a helyi módban elérhető lesz.
- `%%local`**mágikus**. A `%%local` mágikus a kód helyileg a kiszolgálón fut Jupyter, amely a központi csomópontot a HDInsight fürt. Általában használni `%%local` színű együtt a `%%sql` színű és a `-o` paraméter. A `-o` paraméter marad volna meg a helyi meghajtóra, az SQL-lekérdezés eredményét majd `%%local` mágikus kódtöredék szemben a helyi meghajtóra megőrződnek SQL-lekérdezések kimenetét helyi futtatásához a következő készlete szeretne elindítani.

### <a name="query-the-data-by-using-sql"></a>A lekérdezést SQL használatával
A lekérdezés olvassa be a taxi utakat jegy ára összeg, darab személy és tipp összege.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

A következő kódrészlet a `%%local` mágikus létrehoz egy helyi adatok keretben, sqlResults. SqlResults segítségével matplotlib ábrázolni.

> [AZURE.TIP] Ez a cikk többször helyi mágikus használják. Ha az adatkészletet túl nagy, kérjük, minta elférő adatok keret létrehozása a helyi memóriában.

### <a name="plot-the-data"></a>Az adatok ábrázolása

Ábrázolni tud Python kóddal után az adatok keret Pandas adatok keretként helyi környezetben.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 A külső kernel automatikusan megjeleníti a kimenet (HiveQL) SQL-lekérdezések, miután futtatja a kódot. Különböző típusú megjelenítések közül választhat:
 
- Táblázat
- Kördiagram
- Sor
- Terület
- Sáv

Az alábbiakban a kódot, és az adatok ábrázolása:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Kimenet:**

![Tipp: az összeg hisztogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tipp: az összeg személy darabszám szerint](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tipp: az összeg jegy ára összeg](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Funkciók létrehozása és szolgáltatások átalakítása, és ezután előkészítése az adatokat a bevitt modellezési funkciók

A fastruktúrájú-alapú modellezési funkciók a külső Machine Learning és MLlib kell készítenie cél és szolgáltatások különféle technikákat, például binning, indexelés, egy gyorsbillentyűk használatát kódolási és vectorization használatával. Az alábbiakban kövesse az ebben a szakaszban a eljárások:

1. Hozzon létre egy új szolgáltatást **binning** órával forgalom idő időszakok.
2. **Az indexelés és egy gyorsbillentyűk használatát kódolást** alkalmazása kockák funkciók.
3. A **minta és a felosztott az adatkészlet** képzés és próba tört alakra is.
4. **Adja meg a tanfolyam változó és szolgáltatások**, majd hozzon létre indexelt vagy egy gyorsbillentyűk használatát képzési kódolt és adatkészleteket (RDDs) vagy az adatok képkocka rugalmassá beviteli címkézett pont tesztelése elosztott.
5. Automatikus **Kategorizálás és szolgáltatások és -célokat vectorize** szeretné alkalmazni gépi tanulási modellek bemeneti adatok alapján.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Hozzon létre egy új szolgáltatást binning órával a forgalom idő időszakok

Ez a kód megtudhatja, hogyan hozhat létre egy új szolgáltatást binning óra forgalom idő időszakok be, és az eredményül kapott adatok keret a memóriában gyorsítótárazásának. Hol RDDs és az adatok keretek használt többször, gyorsítótár-továbbfejlesztett végrehajtási időket eredményez. Ennek megfelelően RDDs és adatok keretek fogja gyorsítótár több szakaszában az alábbi eljárások valamelyikét.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indexelés és egy gyorsbillentyűk használatát kódolást kockák funkciói

Modellezési és MLlib funkcióit igénylő szolgáltatások, amelyeknek az indexelt és kódolt használata előtt kockák bemeneti adatok előrejelzésére. Ez a szakasz bemutatja, hogyan index vagy az modellezési funkciók bevitt kockák funkcióinak kódolását.

Kell indexelni vagy a modellek kódolását különböző módokon, attól függően, hogy a modellt. Például logisztikus és lineáris regressziós modellek szükség kódolást egy gyorsbillentyűk használatát. Ha például egy szolgáltatást a három kategóriába kibontható három szolgáltatás oszlopba. Minden egyes oszlopát 0 vagy 1, attól függően, hogy egy megfigyelés kategóriájú tartalmaz. MLlib nyújt a [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) függvény a kódolás egy gyorsbillentyűk használatát. A kódoló címke indexek oszlopa bináris módszereket értékű legfeljebb egyetlen egy-egy oszlopban rendeli hozzá. A kódolással várt numerikus értéket tartalmazó funkciókat, például a logisztikus regresszió, algoritmusok kockák funkcióinak vonatkozni.

Az alábbi példák, amelyek karakterláncok megjelenítéséhez csak négy változók fogja átalakítani. Is lehet index további változót, például hét.napja, numerikus értékeket vehet fel, mint a kockák változók jelöli.

Az indexelés, használja a `StringIndexer()`, és egy gyorsbillentyűk használatát kódolást használja a `OneHotEncoder()` MLlib függvények. Az alábbiakban a kódot az index és a kockák szolgáltatások kódolását:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Kimenet:**

A cella futtatásának: 4 másodperc.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Minta és a képzés és Próba törtek az adatkészlet felosztása

Ez a kód véletlen példákat talál arra az adatok (Ez a példa a 25 %) hoz létre. Bár mintavételnél nem szükséges ebben a példában az adatkészlet miatt méretét, a cikk bemutatja, hogyan, hogy tudja, hogyan kell használni a saját problémákat, szükség esetén mintát is. Nagy minták esetén ez jelentős időt takaríthat modellek láthatja el ismeretekkel közben. A minta felosztása egy oktatás (ebben a példában a 75 %) és a tesztelés részét (ebben a példában a 25 %) használata osztályozás és a regressziós modellezési a Tovább gombra.

(A 0 és 1) közötti véletlen szám hozzáadása a oszlop mindegyik sorában (a "VÉL") jelölje ki az idegen-ellenőrzési éleket képzés során használható.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Kimenet:**

A cella futtatásának: 2 másodpercig.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Adja meg a tanfolyam változó és szolgáltatások, és hozza létre az indexelt vagy egy gyorsbillentyűk használatát kódolva, képzést, és a bemeneti adatok vagy pont RDDs keretek címkézett vizsgálata

Ez a szakasz megtudhatja, hogy miként tárgymutató-kockák szöveges adatok címkézett pont adattípusként, és kódolását azt, így láthatja el ismeretekkel és MLlib logisztikus regresszió és egyéb besorolás modellek tesztelése felhasználhatja kódot tartalmazza. Címkével ellátott pont objektumai RDDs, mint a bemeneti adatok gépi tanulási algoritmusok MLlib a legtöbb van szükség, ha vannak formázva. [Címkével ellátott pont](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektoros, vagy a sűrű, vagy a ritka, társított címke/választ.

A kód állítsa be a cél (függő) változó és a szolgáltatások képzése az adatmodellek használatával. Ezután létrehozhat indexelt vagy egy gyorsbillentyűk használatát képzést, és tesztelje a bemeneti adatok vagy pont RDDs keretek címkézett kódolt.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Kimenet:**

A cella futtatásának: 4 másodperc.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automatikus kategorizálás és vectorize szolgáltatások és -célokat szeretné alkalmazni gépi tanulási modellek bemeneti adatok alapján

A külső Machine Learning segítségével kategorizálás a cél és a szolgáltatások használata a fastruktúrájú-alapú modellezési funkciók. A kód két feladat befejezése:

-   Hoz létre, osztályozási bináris cél értéke 0 vagy 1 hozzárendelése egyes pontjai 0 és 1 közötti 0,5 küszöbérték használatával.
- Automatikusan kategorizálja funkciók. Ha bármely szolgáltatás különböző numerikus értékek száma 32-nél kisebb, akkor ezt a szolgáltatást kategóriába esik.

Az alábbiakban a következő két feladat kódot.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Bináris besorolás modell: előrejelzésére, hogy ezt a tippet kell fizetni

Ebben a részben háromféle előre, vagy sem ezt a tippet kell fizetni bináris besorolás modellek létrehozása:

- A külső Machine Learning használatával **logisztikus regresszió modell** `LogisticRegression()` függvény
- A külső Machine Learning használatával **véletlen erdő besorolás modell** `RandomForestClassifier()` függvény
- A **Színátmenet teljesítményfokozó fa besorolás modell** a MLlib használatával `GradientBoostedTrees()` függvény

### <a name="create-a-logistic-regression-model"></a>Hozzon létre egy logisztikus regresszió modell

Ezután hozzon létre egy logisztikus regresszió modell a külső Machine Learning használatával `LogisticRegression()` függvény. A modell épület lépést sorozat kód hozza létre:

1. Egy paraméterrel **vonaton a modell** adatainak megadása
2. **A modell kiértékelés** a mértékek próba adatkészlet.
3. **Mentse a modellt** jövőbeli felhasználás Blob-tárolóhoz.
4. **A modell pontszám** tesztadatokat szemben.
5. A címzett jellemző (ROC) görbék működő a **rajzolja meg az eredményeket** .

Az alábbiakban a kódját ezeket a műveleteket:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Betöltése, pontszám, és mentse az eredményeket.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Kimenet:**

A vizsgált adatokat ROC = 0.9827381497557599


A helyi Pandas adatok keretek Python segítségével a ROC görbe ábrázolni.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
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


**Kimenet:**

![Tipp: vagy nincs tipp ROC görbe](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Véletlen erdő besorolás modell létrehozása

Ezután hozzon létre egy véletlen erdő besorolás modell a külső Machine Learning használatával `RandomForestClassifier()` működik, és majd logikai értéket a vizsgált adatokat a modellt.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Kimenet:**

A vizsgált adatokat ROC = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>Hozzon létre egy GBT besorolás modell

Ezután hozzon létre egy GBT besorolás modell MLlib's használatával `GradientBoostedTrees()` működik, és majd logikai értéket a vizsgált adatokat a modellt.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Kimenet:**

ROC görbéhez terület: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Regressziós modell: ezt a tippet összeg előrejelzésére

Ebben a részben kétféle tipp összeg előrejelzésére regressziós modellek létrehozása:

- Egy **lineáris regressziós modell rendeződik** a külső Machine Learning használatával `LinearRegression()` függvény. A modellt, és, valamint logikai értéket a vizsgált adatokat a modellt.
- A **fastruktúrájú regressziós modell színátmenetes szolgáló lehetőségekről** a külső Machine Learning használatával `GBTRegressor()` függvény.


### <a name="create-a-regularized-linear-regression-model"></a>Hozzon létre egy lineáris regressziós rendeződik modell

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Kimenet:**

A cella futtatásának: 13 másodperces.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Kimenet:**

R-sqr a tesztadatokat = 0.5960320470835743


Ezután a vizsgálati eredmények lekérdezés adatok keretként és AutoVizWidget és matplotlib segítségével ábrázolása azt.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

A kód a lekérdezés eredménye a helyi adatok keret hoz létre, és rajzolja meg az adatokat. A `%%local` mágikus létrehoz egy helyi adatok keretben, `sqlResults`, amelynek matplotlib az ábrázolandó használatával.

>[AZURE.NOTE] A külső mágikus többször szerepel ez a cikk. Ha az adatok mennyiségét túl nagy, a kell példa a elférő adatok keret létrehozása a helyi memóriában.

Hozzon létre a felvétel Python matplotlib használatával.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Kimenet:**

![Tipp: az összeg: tényleges előre jelzett és](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Hozzon létre egy GBT regressziós modell

A külső Machine Learning használatával GBT regressziós modell létrehozása `GBTRegressor()` működik, és majd logikai értéket a vizsgált adatokat a modellt.

[Színátmenet csillapítja fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) döntési fák együttes. GBTs láthatja el ismeretekkel a döntési fák ismételt minimalizálásához veszteség függvényt. A regressziós és besorolását GBTs is használhatja. Azok a kockák szolgáltatások képes kezelni, szolgáltatás méretezése nincs szükség, és nonlinearities és szolgáltatás kapcsolati rögzítheti. Is használhatja őket egy multiclass-besorolás beállítással.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Kimenet:**

Próba-R-sqr van: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Speciális modellezési segédprogramok optimalizálása

Ebben a részben gépi tanulási segédprogramok fejlesztők gyakran használt modell optimalizálása használja. Pontosabban gépi tanulási modellek három módon tudja optimalizálni paraméter abszolút és idegen-ellenőrzés használatával:

-   Az adatok felosztása vonaton és érvényességi beállítása, az adatmodell optimalizálása alkalmazással hyper paraméter abszolút oktatás meg és alapján érvényességi (lineáris regressziós) felmérése
-   A modell optimalizálása határokon-ellenőrzési és függvénnyel külső Machine Learning CrossValidator (bináris osztályozás) abszolút a hyper-paraméter használatával
-   Az adatmodell optimalizálása gépi tanulási függvény és a paraméterek beállítása (lineáris regressziós) használata egyéni határokon-ellenőrzési és paraméter abszolút kód használatával


**Idegen-ellenőrzés** a, hogy mennyire képzett adatok ismert csoportja a modell fog generalize, előre, amikor azt még nem lett képzett adatkészletek funkciók értékeli technika. Ez a módszer mögött az általános arról, hogy modell van képzett ismert adatok adatok csoportja a, de kattintson annak az előrejelzések pontosságának egy független adatkészlet van tesztelése. A közös megvalósítás is oszthatja egy adathalmaz *k*-éleket, és kattintson az a modell összes, de az éleket közül ciklikus módon képzése.

**Optimalizálás a Hyper-paraméter** okozza a problémát, az általában az a cél optimalizálja a algoritmus teljesítmény-független adatkészlet történő változásának mértéke a hyper-paramétereinek egy tanuló algoritmus készletének kiválasztása. A hyper-paraméter értéke meg kell adnia a modell oktatás eljárás kívül. Hyper paraméterértékeket feltételezéseket hatással lehet a rugalmas és a modell pontosságát. Döntési fák a hyper-paraméterek, például a kívánt magassági és a fában Falevelek például van. Önnek kell beállítania a támogatási vektoros géphez (SVM) téves besorolás büntetés kifejezés.

Végezze el a hyper paraméter optimalizálás gyakori módja a rács keresés, más néven egy **paraméter takarítás**környezetbe. A rács keresés egy teljes körű keresést történik, az értékek egy adott részének a hyper paraméter egy tanuló algoritmus hely keresztül. Idegen-ellenőrzési is adja meg a teljesítmény metrikus rendezése a rács keresési algoritmus készített optimális eredményeit. Idegen-ellenőrzési hyper paraméter abszolút használata esetén segítséget nyújthat korlát problémákat, például oktatóanyag adatok modell overfitting. Ezzel a módszerrel a modell megtartja a kapacitása ahhoz, hogy az adatok, ahonnan az oktatóanyag adatok kibontása lett általános csoportjának vonatkozik.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Egy lineáris regressziós modell hyper paraméter abszolút optimalizálása

Adatok ezután feloszthatja vonaton és érvényességi készleteket használata a hyper-paraméter abszolút az adatmodell optimalizálása, és egy adatérvényesítési beállítása (lineáris regressziós) a kiértékelendő oktatás alapján.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Kimenet:**

Próba-R-sqr van: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>A bináris besorolás modell optimalizálása abszolút határokon-ellenőrzési és hyper paraméter használatával

Ez a szakasz bemutatja, hogyan optimalizálhatja a bináris besorolás modell abszolút határokon-ellenőrzési és hyper paraméter használatával. Ez az használ a külső Machine Learning `CrossValidator` függvény.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Kimenet:**

A cella futtatásának: 33 másodperc.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>A regressziós modell optimalizálása egyéni határokon-ellenőrzési és paraméter abszolút kód használatával

Ezután az adatmodell optimalizálása egyéni kód használatával, és a legjobb model paraméterek azonosítsa a legmagasabb pontosság feltétel használatával. Hozzon létre a végleges modell, logikai értéket a vizsgált adatokat a modellt, és mentse a modellt Blob-tárolóban lévő. Végezetül betöltése a modellt, tesztadatokat pontszám és pontosságának felmérése.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Kimenet:**

A cella futtatásának: 61 másodperc.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>A külső beépített gépi tanulási modellek automatikusan Scala felhasználása

Témakörök, amelyek végigvezetik Önt a feladatokat foglalja az Azure-ban az adatok tudományos folyamat áttekintése látható [Csapat tudományos adatfeldolgozás](http://aka.ms/datascienceprocess).

[Csoportwebhely adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) más végpontok közötti forgatókönyvek, amelyek bemutatják a lépéseket a csapat adatok tudományos folyamat a különböző forgatókönyvekben ismerteti. A forgatókönyvek is bemutatják, hogyan lehet a felhőben, és a helyszíni eszközök és szolgáltatások egyesítése egy munkafolyamat vagy a folyamat hozhat létre új intelligens alkalmazást.

[Pontszámhoz külső beépített gépi tanulási modellek](machine-learning-data-science-spark-model-consumption.md) bemutatja, hogyan Scala kód használatával automatikusan betöltése és új adatkészletek pontszám gépi tanulási modellekkel külső beépített és Azure Blob-tárolóhoz menti. A megjelenő utasításokat követve van, és egyszerűen a Python kódot cserélje ki a jelen cikkben automatizált fogyasztási Scala kódot.
