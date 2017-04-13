<properties 
    pageTitle="Gépi tanulási alkalmazások HDInsight létrehozásához használandó Apache külső |} Microsoft Azure" 
    description="Lépésenkénti útmutató gépi tanulási alkalmazások létrehozásához használandó Apache külső a jegyzetfüzetek" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Gépi tanulási alkalmazások futtatásához a HDInsight Linux Apache külső fürt

Megtudhatja, hogyan hozhat létre egy gépi tanulási alkalmazást egy Apache külső fürthöz hdinsight szolgáltatásból lehetőségre. Ez a cikk bemutatja, hogyan a rendelkezésre álló Jupyter jegyzetfüzetet létre, és a alkalmazásban tesztelje a fürthöz használni. Az alkalmazás minden fürt alapértelmezés szerint a HVAC.csv mintaadatok használja.

**Előfeltételek:**

Rendelkeznie kell a következőket:

- Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Egy Apache külső fürthöz Linux hdinsight szolgáltatásból lehetőségre. Című cikkben olvashat [létrehozása Apache külső fürt az Azure hdinsight szolgáltatásból lehetőségre](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>A adatainak megjelenítése

Az alkalmazás épület azt megkezdése előtt tudassa velünk értelmezésekor az adatokat, és mi az adatok elemzése típusú. 

Ebben a cikkben fogjuk kiszámolni az **HVAC.csv** adatfájl – minta elérhető a HDInsight fürt társított Azure tárterület-fiókjában. A tároló számla belüli fájl **\HdiSamples\HdiSamples\SensorSampleData\hvac**elemre. Töltse le és nyissa meg a CSV-fájlt az adatai pillanatképét eléréséhez.  

![Fűtés-és Légtechnikai adatok pillanatfelvétel] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "A fűtés-és Légtechnikai adatai pillanatképét")

Az adatokat a cél hőmérsékleti és a tényleges hőmérsékleti, amelyen telepítve fűtés-és Légtechnikai rendszerek épület látható. Tegyük fel, a **rendszer** oszlop a rendszer azonosító pedig a **SystemAge** oszlop helyén az épület volt a fűtés-és Légtechnikai rendszer évek száma.

Az adatok segítségével előrejelzést, hogy épület hotter vagy colder alapján a is a cél hőmérsékleti, a rendszer azonosító és a rendszer életkorát.

##<a name="app"></a>A külső MLlib használatával gépi tanulási alkalmazás

Az alkalmazás azt egy külső Machine Learning folyamat végrehajtásához használja a dokumentum besorolás. Az a folyamat azt ossza szét a dokumentum szavak, a szavakat konvertálása egy numerikus szolgáltatás vektoros, és végül hozza létre a szolgáltatás felhasználásával és a címkék előrejelzési modell. A következő lépésekkel hozhat létre az alkalmazást.

1. Az [Azure-portált](https://portal.azure.com/)a a startboard kattintson a csempére a külső fürt (Ha a startboard a kiemelt). Az **Összes böngészése**a fürthöz is navigálhat > **Fürt hdinsight szolgáltatásból lehetőségre**.   

2. A külső fürt lap **Fürt irányítópult**elemre, és kattintson a **Jupyter jegyzetfüzet**elemre. Ha a rendszer kéri, írja be a fürt a rendszergazdai hitelesítő adataival.

    > [AZURE.NOTE] Előfordulhat, hogy is elér Jupyter jegyzetfüzet a fürt számára a böngészőben nyissa meg az alábbi URL-címet. __CLUSTERNAME__ cserélje ki a fürt neve:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Hozzon létre új jegyzetfüzetet. Kattintson az **Új**gombra, és kattintson a **PySpark**.

    ![Új Jupyter jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Új Jupyter jegyzetfüzet létrehozása")

3. Új jegyzetfüzet létrejön, és Untitled.pynb nevű megnyitni. Kattintson a jegyzetfüzet nevére a képernyő tetején, és írjon be egy rövid nevet.

    ![Adjon egy nevet a jegyzetfüzetnek] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Adjon egy nevet a jegyzetfüzetnek")

3. Mivel a PySpark kernelt jegyzetfüzet hozta létre, nem kell bármely környezetek kifejezetten létrehozása. A külső és struktúra környezetek automatikusan létrejön, az első kód cellát futtatásakor. Ebben az esetben szükséges fájltípusok importálásával elindíthatja. Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Meg kell most betöltheti az adatokat (hvac.csv), elemzéséhez és a modell képzése használatával. Ezt egy funkció, amely annak vizsgálata, hogy a tényleges hőmérsékleti az épület nagyobb, mint a cél hőmérsékleti definiálhatók. Ha a tényleges hőmérsékleti értéke nagyobb, az épület az meleg, az érték **1.0**-lel. Ha a tényleges hőmérsékleti kisebb, az épület hideg, az érték **0.0**-lel. 

    Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Állítsa be a külső gépi tanulási folyamat, amely három szakaszban áll: tokenizáló hashingTF és lr. Erről további információt a Mi az, hogy egy folyamat, és hogy hogyan működik a <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">külső gépi tanulási a folyamat</a>.

    Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. A folyamat a oktatás dokumentumot illeszkedik. Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.

        model = pipeline.fit(training)

7. Ellenőrizze az ellenőrzés oktatás dokumentum állapotának az alkalmazással. Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.

        training.show()

    Meg kell adnia a kibocsátás, az alábbihoz hasonló:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Térjen vissza, és ellenőrizze a kimenet nyers CSV-fájl szemben. Ha például a CSV-fájl első sorában ezeket az adatokat foglalja magában:

    ![Fűtés-és Légtechnikai adatok pillanatfelvétel] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "A fűtés-és Légtechnikai adatai pillanatképét")

    Figyelje meg, hogyan a tényleges hőmérsékleti nem éri el a cél hőmérsékleti javaslat az épület hideg. Így oktatás kimeneti a **címkét** az első sor értéke **0.0**, ami azt jelenti, hogy az épület nem meleg.

8.  Készítse el adatok a képzett modell ellen futtatásához. Ehhez azt volna adják át egy azonosító és a rendszer az épület kora (jelölése **SystemInfo** oktatás kimeneti), és a modell előrejelzésére szeretné, hogy a rendszer azonosító és a rendszer kor épület hotter kell (1.0 lel) vagy hűvösebben (0.0 lel).

    Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Az előrejelzések végül kinagyíthatja a vizsgált adatokat. Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    Az első sorban a szövegbevitel, láthatja, hogy a azonosító 20 és a rendszer korát 25 év fűtés-és Légtechnikai rendszer esetén az épület lesz meleg (**előrejelzési = 1,0**). DenseVector (0.49999) az első értéke megfelel az előrejelzési 0.0, és a második értékig (0.5001) felel meg az előrejelzési 1.0. Az eredményben, annak ellenére, hogy a második értékig csak részben magasabb a modell megjelenítése **előrejelzési = 1,0**.

11. Miután végzett az alkalmazást futtató, tegye a következőt: leállítás engedje fel az erőforrások jegyzetfüzetet. Ehhez a jegyzetfüzetet a **fájl** menüben kattintson a **bezárása és leállíthatja**. Ez lesz leálljon, és a jegyzetfüzet bezárása.
           

##<a name="anaconda"></a>Használja a Anaconda scikit-tár gépi tanulási bemutatása

HDInsight Apache külső fürt Anaconda tárak tartalmazzák. Ez is tartalmazza a **scikit – ismerje meg,** gépi tanulási tárat. A tár a minta alkalmazások közvetlenül a Jupyter jegyzetfüzet létrehozásához használható különböző adatkészletek is tartalmaz. Példák a scikit használatáról – ismerje meg a tárat a [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html)találhat.

##<a name="seealso"></a>Lásd még:

* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

* [Webhely napló analysis HDInsight külső használata](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Létrehozása és futtatása alkalmazások

* [Scala használatával önálló-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)

* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények

* [Létrehozása és elküldése külső Scala alkalmazást IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin.md)

* [A külső alkalmazások távolról hibáinak IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [A HDInsight külső fürt Zeppelin jegyzetfüzetek használata](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Elérhető az HDInsight-külső fürthöz Jupyter jegyzetfüzet mag](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Külső csomagok Jupyter jegyzetfüzeteket használata](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter telepítése a számítógépen, és csatlakozzon az HDInsight külső fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése

* [A Apache külső fürt Azure hdinsight szolgáltatáshoz a források kezelése](hdinsight-apache-spark-resource-manager.md)

* [A a HDInsight-Apache külső fürthöz nyomon követése és hibakeresési feladatok](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
