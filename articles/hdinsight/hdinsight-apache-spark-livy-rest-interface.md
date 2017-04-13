<properties
    pageTitle="Külső feladatok segítségével távolról Livius elküldése |} Microsoft Azure"
    description="Megtudhatja, hogy miként HDInsight fürt külső feladatok távolról küldhetik Livius használata."
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
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>A külső feladatok távolról HDInsight Linux Livius használata egy Apache külső fürthöz kezdeményezése

Azure hdinsight szolgáltatáshoz a Apache külső fürt Livius, feladatokat távolról a külső fürtre elküldéséhez REST felület tartalmazza. Részletes leírást [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)című témakör tartalmaz.

Livius segítségével interaktív külső ismertetése, vagy az küldhetnek köteg feladatokat a külső futtatásához. Ez a cikk a köteg feladatok küldhetik Livius használatáról folytatott beszélgetést. Az alábbi szintaxist Curl használja a többi kezdeményezhetnek az Livius végpontot.

**Előfeltételek:**

Rendelkeznie kell a következőket:

- Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Egy HDInsight Linux Apache külső fürthöz. Című cikkben olvashat [létrehozása Apache külső fürt az Azure hdinsight szolgáltatásból lehetőségre](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>A fürt köteg feladat elküldése

Mielőtt elküld egy köteg feladatot, akkor fel kell töltenie az alkalmazás üveg fürthöz társított fürt tárolása a. [**AzCopy**](../storage/storage-use-azcopy.md), egy parancssori segédprogramot, ezt is használhatja. Vannak olyan sok más ügyfelek feltölteni az adatokat is használhatja. További információ a névjegyek őket az [adatok HDInsight Hadoop feladatok feltöltése](hdinsight-upload-data.md)is megkeresheti.

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Példa**:

* Ha a üveg fájl fürt tárolására (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Ha a használni kívánt bemeneti fájl részeként a üveg fájlnév- és a osztálynév átadni (ebben a példában az input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Információ a kötegekben történik a a fürthöz

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Példa**:

* Ha szeretné lekérni a a fürthöz kötegekben történik:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Ha egy adott tételt az egy adott batchId lekérdezni kívánt

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>A köteg feladat törlése

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Példa**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livius és a Max-elérhetősége

Livius nyújt a magas rendelkezésre állás külső a fürt futó feladatokat. Az alábbiakban néhány példát.

* Ha a Livius szolgáltatás megszakad, miután elküldte a feladat távolról a külső fürtre, a feladat továbbra is futtathatók a háttérben. Ha Livius készítsen biztonsági másolatot, visszaállítja a a feladatot, és a jelentések ismét állapotát.

* A HDInsight Jupyter jegyzetfüzetek a kódmentes a vannak hajtott Livius. Jegyzetfüzet egy külső feladat fut, és a Livius szolgáltatás újraindítása kapja, ha a jegyzetfüzet továbbra is a kód cellák futtatásához. 

## <a name="show-me-an-example"></a>Példa megjelenítése

Ebben a részben megnézi az példák Livius használatát egy külső kérelmet, az alkalmazás állapotának nyomon követéséhez, és törölje a feladat. Az ebben a példában használjuk alkalmazás egyike a fejlett [önálló Scala alkalmazás és a HDInsight külső fürthöz futtatásához létrehozása](hdinsight-apache-spark-create-standalone-application.md)című témakörben. Az alábbi lépésekkel tegyük fel, a következőket:

* A már másolt keresztül az alkalmazás üveg fürthöz társított tárterület-fiókjába.
* Ha CuRL telepítve a számítógépen, ahová próbálja ezeket a lépéseket.

Hajtsa végre az alábbi lépéseket.

1. Tudassa velünk először győződjön meg arról, hogy a Livius fut-e a fürt. Azt teheti az első kötegekben futó listáját. Ha ez az első alkalommal futtatja az feladatot Livius segítségével, ezzel térjen vissza a nulla.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Szerezheti be olyan eredménye az alábbihoz hasonló:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Figyelje meg, hogyan az utolsó sor az eredményben az **összes: 0**, amely nem futó kötegekben javasol olvasható.

2. Tudassa velünk most köteg feladat elküldése. Az alábbi kódtöredékének paraméterként bemeneti fájl (input.txt) történő átadásakor üveg nevét és az osztály nevét használja. Az a javasolt megközelítés, ha egy Windows rendszerű számítógépen futtatja a következő lépéseket.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    A fájl **input.txt** a paraméterek a következők:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Figyelje meg, hogyan a kimenet utolsó sora szerint **Állapot: indítása**. Azt is azt mondja, **azonosító: 0**. Ez az, hogy a köteg azonosítóját.

3. Most már meghallgathatja a állapotát a adott köteg, használja a köteg azonosítóját.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    A kimenet most látható **Állapot: sikeres**, amely felajánlja, hogy a feladat sikeresen befejeződött.

4. Ha azt szeretné, most törölheti a köteget.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    A kimenet utolsó sora jeleníti meg, hogy a köteg sikeresen törölték. Ha töröl egy feladat futása közben, azt a feladat lényegében fog törlése. Ha véletlenül töröl egy feladat, amely befejeződött, sikeresen vagy más módon teljesen törlése az adatok.

## <a name="seealso"></a>Lásd még:


* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: használata külső a HDInsight épület hőmérsékleti fűtés-és Légtechnikai adatok elemzéséhez](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

* [Webhely napló analysis HDInsight külső használata](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Létrehozása és futtatása alkalmazások

* [Scala használatával standalone-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)

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
