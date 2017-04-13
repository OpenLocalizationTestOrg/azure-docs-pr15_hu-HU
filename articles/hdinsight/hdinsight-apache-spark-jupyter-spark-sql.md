<properties
    pageTitle="Hozzon létre egy külső fürt HDInsight Linux, és használja a külső SQL Jupyter az interaktív elemzéshez |} Microsoft Azure"
    description="Lépésenkénti útmutató segítségével gyorsan létrehozhat egy Apache külső a HDInsight fürt, majd a külső SQL Jupyter jegyzetből interaktív lekérdezések futtatása."
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
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Első lépések: hozzon létre Apache külső fürt HDInsight Linux, és a külső SQL interaktív lekérdezések futtatása

Megtudhatja, hogy miként hozhat létre egy külső Apache fürthöz hdinsight szolgáltatásból lehetőségre, és [Jupyter](https://jupyter.org) jegyzetfüzet segítségével interaktív külső SQL-lekérdezések futtatása a külső fürt.

   ![A HDInsight Apache külső használatának első lépései] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Használatba Apache külső HDInsight oktatóprogram során. Bemutatott lépések:; tárterület-fiók létrehozása Hozzon létre egy fürt; a külső SQL-utasítások futtatása")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Előfeltételek

- **Az Azure-előfizetés**. Ebben az oktatóanyagban megkezdése előtt az Azure előfizetéssel kell rendelkeznie. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **A biztonságos rendszerhéj (SSH) ügyfél**: Linux, Unix és OS X rendszer paramétert egy SSH ügyfél – a `ssh` parancsot. A Windows rendszerhez [gitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)javasoljuk.
    
- **(Nem kötelező) biztonságos rendszerhéj (SSH) kulcsok**: csatlakozzon a jelszó vagy a nyilvános kulcs használatával fürthöz SSH fiók biztonságát. Jelszóval kap, gyorsan lépéseket, és akkor használja ezt a lehetőséget, ha szeretne gyorsan fürt létrehozása és futtatása egyes próba feladatok. Egy kulcsot használata biztosítani, azonban további beállítási igényel. Érdemes lehet használni ezt a módszert, egy gyártási fürt létrehozásakor. Ebben a cikkben a jelszó megközelítés használjuk. Hogyan hozhat létre és SSH billentyűk használata HDInsight útmutatásért olvassa el az alábbi cikkekben:

    -  A számítógépről Linux - [Használata SSH Linux-alapú HDInsight (Hadoop) Linux rendszerhez, a Unix, vagy az OS X együtt](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Egy Windows rendszerű számítógépen - [Használata SSH Linux-alapú HDInsight (Hadoop) a Windows együtt](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] Ez a cikk a [Azure tároló BLOB a fürt tárolására](hdinsight-hadoop-use-blob-storage.md)használó külső fürt létrehozása az Azure erőforrás manager sablont használja. A külső fürtre mellett az alapértelmezett tárolására Azure-tároló BLOB-tárhely, [Azure tó adattár](../data-lake-store/data-lake-store-overview.md) használó is létrehozhat. Című cikkben olvashat [létrehozása egy HDInsight fürthöz tó áruházzal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>A külső csoport létrehozása

Ebben a részben egy HDInsight verzió 3.4 fürthöz (külső verzió 1.6.1) létrehozása az Azure erőforrás manager sablon használatával. HDInsight-verziók és azok SLA kapcsolatos további tudnivalókért lásd [HDInsight-összetevő verziószámozás](hdinsight-component-versioning.md). Más fürt létrehozási módjai [létrehozása HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md)talál.

1. Kattintson az alábbi képen a az Azure-portálon nyissa meg a sablont.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    A sablon nyilvános blob tároló, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*található. 
   
2. A Paraméterek lap az adja meg az alábbiakat:

    - **Fürtnév**: Adja meg a létrehozandó Hadoop fürt nevét.
    - **Fürt felhasználónév és jelszó**: az alapértelmezett bejelentkezési neve rendszergazdaként.
    - **SSH felhasználónevet és jelszót**.
    
    Kérjük, jegyezze fel ezeket az értékeket.  Szüksége lesz rájuk az oktatóprogram belül.

    > [AZURE.NOTE] A HDInsight fürt használatával a parancssorban távoli eléréséhez SSH használják. A felhasználónév és jelszó itt adhat meg a fürt SSH keresztül való csatlakozáskor használják. Is a SSH felhasználónév egyedinek kell lennie, mint hoz létre felhasználói fiókot a HDInsight fürt csomóponton. Az alábbi néhány a fürt szolgáltatások számára fenntartva fiók nevét, és nem használhatók a SSH felhasználónévvel:
    >
    > legfelső szintű, hdiuser, vihar, hbase, ubuntu, zookeeper, fájlrendszerhez, fonal, mapred, hbase, struktúra, oozie, sólyom, sqoop, felügyeleti, tez, hcat, hdinsight-zookeeper.

    > A HDInsight SSH használja a további tudnivalókért lásd: az alábbi cikkekben:

    > * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3.a mentéséhez kattintson **az OK** gombra a paraméterek.

4. az **Egyéni telepítés** lap, kattintson az **erőforrás csoport** legördülő listában, és kattintson az **Új** hozhat létre új erőforráscsoport. Az erőforráscsoport, amely a fürt, a függő tárterület-fiók és más csatolt erőforrás csoportosítja tároló.

5.a **jogi feltételek**gombra, és kattintson a **Létrehozás**gombra.

6. Kattintson **létrehozása**parancsra. Új sablon telepítéshez Submitting telepítési című csempe jelenik meg. Szükséges időt körülbelül 20 perc kapcsolatban a fürt és SQL-adatbázis létrehozása.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Használt Jupyter jegyzetfüzetek külső SQL-lekérdezések futtatása

Ebben a részben használatával Jupyter jegyzetfüzet külső SQL-lekérdezések futtatása a külső fürt szemben. HDInsight külső fürt adja meg a két mag Jupyter jegyzetfüzet használható. A következők:

* **PySpark** (az alkalmazásai Python)
* **A külső** (az alkalmazásai Scala)

Ebben a cikkben a PySpark kernel fogja használni. A következő cikkben: [a külső HDInsight fürt Jupyter jegyzetfüzetek mag](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) erről részletesen ismerni a PySpark kernelt előnyeit. Néhány legfontosabb előnye a PySpark kernelt azonban a következők:

* Nem kell beállítania a környezetek külső és a struktúra. Ezek vannak automatikusan beállítva.
* Cella magics, például: használható `%%sql`, hogy közvetlenül a struktúra vagy az SQL-lekérdezések futtatása, minden olyan megelőző kódtöredék nélkül.
* A kimenet a struktúra vagy az SQL-lekérdezések automatikusan formájában jelenik meg.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>PySpark kernel Jupyter jegyzetfüzet létrehozása 

1. Az [Azure-portált](https://portal.azure.com/)a a startboard kattintson a csempére a külső fürt (Ha a startboard a kiemelt). Az **Összes böngészése**a fürthöz is navigálhat > **Fürt hdinsight szolgáltatásból lehetőségre**.   

2. A külső fürt lap **Fürt irányítópult**elemre, és kattintson a **Jupyter jegyzetfüzet**elemre. Ha a rendszer kéri, írja be a fürt a rendszergazdai hitelesítő adataival.

    > [AZURE.NOTE] Előfordulhat, hogy is elér Jupyter jegyzetfüzet a fürt számára a böngészőben nyissa meg az alábbi URL-címet. __CLUSTERNAME__ cserélje ki a fürt neve:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Hozzon létre új jegyzetfüzetet. Kattintson az **Új**gombra, és kattintson a **PySpark**.

    ![Új Jupyter jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Új Jupyter jegyzetfüzet létrehozása")

3. Egy új jegyzetfüzetet létre, és a megnyitott Untitled.pynb nevű. Kattintson a jegyzetfüzet nevére a képernyő tetején, és írjon be egy rövid nevet.

    ![Adjon egy nevet a jegyzetfüzetnek] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Adjon egy nevet a jegyzetfüzetnek")

4. Mivel a PySpark kernelt jegyzetfüzet hozta létre, nem kell bármely környezetek kifejezetten létrehozása. A külső és struktúra környezetek automatikusan létrejön, az első kód cellát futtatásakor. Ebben az esetben az szükséges fájltípusok importálásával elindíthatja. Ehhez illessze be a következő kódrészletet a cellában, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.

        from pyspark.sql.types import *
        
    Minden alkalommal fut, a feladat Jupyter, a webhely böngészőben ablak címének jelennek meg a jegyzetfüzet cím együtt **(elfoglalt)** állapotban. Egyszínű kör mellett a jobb felső sarokban lévő **PySpark** szöveget is látni fogja. A feladat befejezése után meg egy üres körre változik.

     ![A jegyzetfüzet Jupyter feladat állapota] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "A jegyzetfüzet Jupyter feladat állapota")

4. Mintaadatok betöltése ideiglenes táblába. Amikor a külső fürtre HDInsight hoz létre, az adatfájl – minta, **hvac.csv**, másolja az alkalmazás a fiókhoz társított tároló **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Egy üres cellába illessze be az alábbi példa, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**. A példa az adatok regisztrál az ideiglenes **Fűtés-és légtechnikai**nevű táblába.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Mivel a PySpark kernel használ, most közvetlenül futtatható SQL-lekérdezés használatával létrehozott ideiglenes tábla **Fűtés-és légtechnikai** a `%%sql` mágikus. További információt a `%%sql` mágikus, valamint egyéb találhatók meg a PySpark kernel magics lásd: [a külső HDInsight fürt Jupyter jegyzetfüzetek mag](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Miután a feladat sikeresen befejeződött, a következő táblázatos kimenet alapértelmezés szerint megjelenik.

    ![Táblázat kimenetét lekérdezés eredménye] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Táblázat kimenetét lekérdezés eredménye")

    Megtekintheti a többi megjelenítésen, valamint az eredmények is. Ha például egy területdiagram ugyanazt a kimeneti lenne az alábbi.

    ![A lekérdezés eredményét területdiagram] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "A lekérdezés eredményét területdiagram")


6. Miután végzett az alkalmazást futtató, tegye a következőt: leállítás engedje fel az erőforrások jegyzetfüzetet. Ehhez a jegyzetfüzetet a **fájl** menüben kattintson a **bezárása és leállíthatja**. Ez lesz Leállítás és a jegyzetfüzet bezárása.

##<a name="delete-the-cluster"></a>A csoport törlése

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Lásd még:


* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: használata külső a HDInsight épület hőmérsékleti fűtés-és Légtechnikai adatok elemzéséhez](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

* [Webhely napló analysis HDInsight külső használata](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Alkalmazás betekintést telemetriai adatelemzés HDInsight külső használata](hdinsight-spark-analyze-application-insight-logs.md)

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

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
