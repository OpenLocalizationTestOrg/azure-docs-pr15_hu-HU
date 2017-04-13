<properties 
    pageTitle="Használjon Zeppelin jegyzetfüzeteket külső fürt HDInsight Linux |} Microsoft Azure" 
    description="Lépésenkénti HDInsight Linux külső fürt Zeppelin jegyzetfüzetek használatára." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Használjon Zeppelin jegyzetfüzeteket HDInsight Linux Apache külső fürthöz

HDInsight külső fürt Zeppelin jegyzetfüzetek használó külső feladatok futtatása tartalmazzák. Ebben a cikkben megismerheti, hogyan a Zeppelin Jegyzetfüzet használata egy HDInsight fürt.


**Előfeltételek:**

* Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Egy külső Apache fürthöz. Című cikkben olvashat [létrehozása Apache külső fürt az Azure hdinsight szolgáltatásból lehetőségre](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Egy Zeppelin notebook alkalmazás elindítása

1. A külső fürt lap **Fürt irányítópult**elemre, és kattintson a **Zeppelin jegyzetfüzet**elemre. Ha a rendszer kéri, írja be a fürt a rendszergazdai hitelesítő adataival.

    > [AZURE.NOTE] Előfordulhat, hogy is elér Zeppelin jegyzetfüzet a fürt számára a böngészőben nyissa meg az alábbi URL-címet. __CLUSTERNAME__ cserélje ki a fürt neve:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Hozzon létre új jegyzetfüzetet. A fejléc ablakból a **Jegyzetfüzet**elemre, és kattintson az **Új feljegyzés létrehozása**.

    ![Új Zeppelin jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Új Zeppelin jegyzetfüzet létrehozása")

    Írjon be egy nevet a jegyzetfüzetnek, és kattintson a **Megjegyzés létrehozása**gombra.

3. Győződjön meg arról, hogy a jegyzetfüzet fejléc egy csatlakoztatott állapotot mutatja. Egy zöld pont jobb felső sarokban lévő helyén.

    ![Zeppelin jegyzetfüzet állapota] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin jegyzetfüzet állapota")

4. Mintaadatok betöltése ideiglenes táblába. Amikor a külső fürtre HDInsight hoz létre, az adatfájl – minta, **hvac.csv**, másolja az alkalmazás a fiókhoz társított tároló **\HdiSamples\SensorSampleData\hvac**.

    Az új jegyzetfüzetet az alapértelmezés szerint jön létre üres bekezdés illessze be az alábbi kódtöredékének.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT** , vagy kattintson a kódtöredék futtatásához a bekezdés a **Lejátszás** gombra. A bekezdés jobb-sarkában állapota Kész FÜGGŐBEN befejezett futtatása a kell haladnia. A kimenet ugyanahhoz a bekezdéshez alján jelenik meg. A képernyőképet formátumban, például a következőket:

    ![Ideiglenes nyers adatokból a tábla létrehozása] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Ideiglenes nyers adatokból a tábla létrehozása")

    A bekezdések címet is megadhat. A jobb sarkában kattintson a **Beállítások** ikonra, és kattintson a **cím megjelenítése**.

5. Most már a **Fűtés-és légtechnikai** táblázatban futtatását is lehetővé teszi a külső SQL-utasításait. Új bekezdés illessze be az alábbi lekérdezés. A lekérdezés által visszaadott az épület Azonosítóját, illetve a cél és egy adott dátum minden létrehozásának tényleges átlaghőmérsékletűek közötti különbség. Nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    Az elején **% sql** -utasítás azt jelenti, hogy a Livius Scala értelmező használni a jegyzetfüzetet.

    Az alábbi képernyőképen a kimenet.

    ![Egy külső SQL-utasítást a jegyzetfüzetet használó futtatása] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Egy külső SQL-utasítást a jegyzetfüzetet használó futtatása")

     A megjelenítési beállítások (téglalap kiemelve) kattintson a azonos kimeneti különböző megadott közötti váltáshoz. Kattintson a **Beállítások** a kulcsot és értékek kimeneti milyen consitutes választhatja ki. Fent, a képernyőfelvétel **buildingID** **temp_diff** átlagát, valamint a kulcs értékként használja.

    
6. Változók használata a lekérdezési külső SQL-utasítások is futtathatók. A következő kódtöredékének határozhat meg egy változó, **Temp**, a lekérdezés, amelynek a lehetséges értékek a lekérdezni kívánt mutatja. Amikor először futtatja a lekérdezést, egy legördülő automatikusan kitölti a változó megadott értékek.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Illessze be a kódtöredék új bekezdés, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**. Az alábbi képernyőképen a kimenet.

    ![Egy külső SQL-utasítást a jegyzetfüzetet használó futtatása] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Egy külső SQL-utasítást a jegyzetfüzetet használó futtatása")

    Későbbi lekérdezések esetén választhat egy új értéket a legördülő listában, és futtassa újra a lekérdezést. Kattintson a **Beállítások** a kulcsot és értékek kimeneti milyen consitutes választhatja ki. A fenti képernyőképet **buildingID** használja, mint a kulcsot, **temp_diff** értékként és **targettemp** csoportként átlaga.

7. Indítsa újra az Livius értelmező lépjen ki az alkalmazást. Ehhez értelmező beállítások megnyitása elemre kattintva a bejelentkezett felhasználó nevét a jobb felső sarkában, és válassza a **értelmező**.

    ![Indítsa el a értelmező] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Kimeneti struktúra")

2. Görgessen a Livius értelmező beállításokat, és válassza a **Indítsa újra**.

    ![Indítsa újra a Livius intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Indítsa újra a Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Hogyan külső csomagok használata a jegyzetfüzetet?

Beállíthatja a Zeppelin jegyzetfüzet Apache külső fürt a HDInsight (Linux), amelyek nem található meg a-kész a fürt külső, közösségi járult csomagok használata. A teljes listáját a rendelkezésre álló csomagok [maven tesztelése tárházba](http://search.maven.org/) is kereshet. Rendelkezésre álló csomagok listája is megnyithatja a más forrásokból. Például teljes listáját a közösségi járult csomagok érhető el a [Külső csomagok](http://spark-packages.org/).

Ebben a cikkben látni fogja a [külső csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -csomag használata a Jupyter jegyzetfüzetet.

1. Nyissa meg a értelmező beállításait. A jobb felső sarokban kattintson a bejelentkezett felhasználó nevét, és válassza a **értelmező**.

    ![Indítsa el a értelmező] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Kimeneti struktúra")

2. Görgessen a Livius értelmező beállítások, és kattintson a **Szerkesztés**gombra.

    ![Értelmező beállításainak módosítása] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Értelmező beállításainak módosítása")

3. Vegyen fel egy új kulcsot, **livy.spark.jars.packages** néven, és állítsa be az értéket a formátum `group:id:version`. Igen, ha szeretné használni a [külső csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -csomagot, be kell a billentyű lenyomásával értékének `com.databricks:spark-csv_2.10:1.4.0`.

    ![Értelmező beállításainak módosítása] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Értelmező beállításainak módosítása")

    Kattintson a **Mentés** gombra, és indítsa újra a Livius értelmező.

4. **Tipp**: Ha megtudhatja, mire eljut az üzenetem a hogyan szeretné a kulcs értékét a megadott fölött, az alábbiakban módját.

    egy. Keresse meg a csomagot a maven tesztelése tárat. Ebben az oktatóanyagban [külső csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar)azt használni.
    
    b. A tárházba, a gyűjtse össze az értékek **Csoportazonosító**, **ArtifactId**és **verzióját**.

    ![Jupyter Jegyzetfüzet használata a külső csomagok] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Jupyter Jegyzetfüzet használata a külső csomagok")

    c billentyűkombinációt. ÖSSZEFŰZ a három értéket, a elválasztva (****:).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>A Zeppelin jegyzetfüzetek mentési helyének?

A Zeppelin jegyzetfüzeteket a fürt headnodes menti. Igen ha törli a fürt, a jegyzetfüzetek törlődik, valamint. Ha meg szeretné őrizni a jegyzetfüzetek más fürt a későbbi felhasználás céljából, exportálnia kell őket a feladatok futtatása után. Exportálja a kívánt jegyzetfüzetet, kattintson az **Exportálás** ikonra az alábbi képen látható módon.

![Jegyzetfüzet letöltése] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "A jegyzetfüzet letöltése")

Ez a letöltési helye a JSON fájlként menti a jegyzetfüzet.

## <a name="livy-session-management"></a>Livius munkamenet-kezelés

Az első kód bekezdés futtatásakor a Zeppelin jegyzetfüzet egy új Livius-munkamenetet a HDInsight külső fürt jön létre. Ebben a munkamenetben meg van osztva, amely a későbbiekben létrehozott valamennyi Zeppelin jegyzetfüzetben. Ha valamilyen okból a munkamenet Livius (fürt indítsa újra a rendszert, stb.) levágni, nem tudja feladatokat lebonyolítása a Zeppelin jegyzetfüzetet.

Ebben az esetben az alábbi lépéseket kell elvégeznie, futó feladatok Zeppelin jegyzetfüzet a Kezdés előtt. 

1. Indítsa újra a Zeppelin jegyzetfüzetből a Livius értelmező. Ehhez értelmező beállítások megnyitása elemre kattintva a bejelentkezett felhasználó nevét a jobb felső sarkában, és válassza a **értelmező**.

    ![Indítsa el a értelmező] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Kimeneti struktúra")

2. Görgessen a Livius értelmező beállításokat, és válassza a **Indítsa újra**.

    ![Indítsa újra a Livius intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Indítsa újra a Zeppelin intepreter")

3. Futtassa a kód cella Zeppelin meglévő jegyzetfüzet. Ezzel létrehoz egy új Livius-munkamenetet a HDInsight fürt.

## <a name="seealso"></a>Lásd még:


* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: használata külső a HDInsight épület hőmérsékleti fűtés-és Légtechnikai adatok elemzéséhez](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

* [Webhely napló analysis HDInsight külső használata](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Létrehozása és futtatása alkalmazások

* [Scala használatával önálló-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)

* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények

* [Létrehozása és elküldése külső Scala alkalmazást IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin.md)

* [A külső alkalmazások távolról hibáinak IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

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







