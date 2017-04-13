<properties 
    pageTitle="Külső csomagok használata HDInsight Apache külső fürt Jupyter jegyzetfüzetek |} Azure"
    description="Lépésenkénti útmutató Jupyter jegyzetfüzetek elérhető állítható be az HDInsight külső fürt külső külső csomagok használata." 
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


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Külső csomagok használata Apache külső fürt HDInsight Linux Jupyter jegyzetfüzetek

>[AZURE.NOTE] Ez a cikk olyan külső 1.5.2 a HDInsight 3.3 és a külső 1.6.2 a HDInsight 3.4 alkalmazható. 

Megtudhatja, hogy miként Jupyter jegyzetfüzet konfigurálása a HDInsight (Linux) használata a külső Apache külső fürt, közösségi járult csomagokat, amelyek nem szerepel a fürt ki-be a. 

A teljes listáját a rendelkezésre álló csomagok [maven tesztelése tárházba](http://search.maven.org/) is kereshet. Rendelkezésre álló csomagok listája is megnyithatja a más forrásokból. Például teljes listáját a közösségi járult csomagok érhető el a [Külső csomagokat](http://spark-packages.org/).

Ebben a cikkben megtanulhatja a [külső csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -csomag használata a Jupyter jegyzetfüzetet.

##<a name="prerequisites"></a>Előfeltételek

Rendelkeznie kell a következőket:

- Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Egy HDInsight Linux Apache külső fürthöz. Című cikkben olvashat [létrehozása Apache külső fürt az Azure hdinsight szolgáltatásból lehetőségre](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Külső csomagok Jupyter jegyzetfüzeteket használata 

1. Az [Azure-portált](https://portal.azure.com/)a a startboard kattintson a csempére a külső fürt (Ha a startboard a kiemelt). Az **Összes böngészése**a fürthöz is navigálhat > **Fürt hdinsight szolgáltatásból lehetőségre**.   

2. A külső fürt lap, a **Tartalom**gombra, és a **Fürt az irányítópult** lap az kattintson **Jupyter jegyzetfüzet**. Ha a rendszer kéri, írja be a fürt a rendszergazdai hitelesítő adataival.

    > [AZURE.NOTE] Előfordulhat, hogy is elér Jupyter jegyzetfüzet a fürt számára a böngészőben nyissa meg az alábbi URL-címet. __CLUSTERNAME__ cserélje ki a fürt neve:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Hozzon létre új jegyzetfüzetet. Kattintson az **Új**gombra, és kattintson a **külső**.

    ![Új Jupyter jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Új Jupyter jegyzetfüzet létrehozása")

3. Egy új jegyzetfüzetet létre, és a megnyitott Untitled.pynb nevű. Kattintson a jegyzetfüzet nevére a képernyő tetején, és írjon be egy rövid nevet.

    ![Adjon egy nevet a jegyzetfüzetnek] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Adjon egy nevet a jegyzetfüzetnek")

4. Akkor használja a `%%configure` mágikus konfigurálása a jegyzetfüzetet egy külső csomag használata. A jegyzetfüzetek által használt külső csomagok gondoskodjon arról, hogy felhívja a `%%configure` színű kód első cellájára. Ezzel biztosíthatja, hogy a kernel van konfigurálva a csomag a munkamenet indítása előtt.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Ha elfelejti a kernel beállítása első cellájára, használhatja a `%%configure` együtt a `-f` paraméter, de fog indítsa újra a munkamenetet, és az összes folyamatban el fog veszni.

5. A fenti, a kódtöredék `packages` vár maven tesztelése koordináták maven tesztelése központi adattárban listáját. Az ebben a kódtöredék `com.databricks:spark-csv_2.10:1.4.0` van a **külső csv** -csomag maven tesztelése koordináta. Az alábbiakban hogyan állítja össze a koordináták a csomagnak.

    egy. Keresse meg a csomagot a maven tesztelése tárat. Ebben az oktatóanyagban [külső csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar)azt használja.
    
    b. A tárházba, a gyűjtse össze az értékek **Csoportazonosító**, **ArtifactId**és **verzióját**.

    ![Jupyter Jegyzetfüzet használata a külső csomagok] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Jupyter Jegyzetfüzet használata a külső csomagok")

    c billentyűkombinációt. ÖSSZEFŰZ a három értéket, a elválasztva (****:).

        com.databricks:spark-csv_2.10:1.4.0

6. Futtassa a kódot tartalmazó cellát a `%%configure` mágikus. Ez az alapul szolgáló Livius munkamenet megadott előkészítés beállítja. A jegyzetfüzet későbbi cellájában most már használhatja a csomagot, alább látható módon.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Ezt követően futtathatja a kódrészletek, például alábbi ábrán látható, a dataframe az előző lépésben létrehozott adatainak megtekintéséhez.

        df.show()

        df.select("Time").count()


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

* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények

* [Létrehozása és elküldése külső Scala alkalmazást IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin.md)

* [A külső alkalmazások távolról hibáinak IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [A HDInsight külső fürt Zeppelin jegyzetfüzetek használata](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Elérhető az HDInsight-külső fürthöz Jupyter jegyzetfüzet mag](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Jupyter telepítése a számítógépen, és csatlakozzon az HDInsight külső fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése

* [A Apache külső fürt Azure hdinsight szolgáltatáshoz a források kezelése](hdinsight-apache-spark-resource-manager.md)

* [A a HDInsight-Apache külső fürthöz nyomon követése és hibakeresési feladatok](hdinsight-apache-spark-job-debugging.md)
