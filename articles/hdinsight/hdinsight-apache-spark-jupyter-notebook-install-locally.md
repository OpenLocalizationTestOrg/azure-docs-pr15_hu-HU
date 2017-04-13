<properties 
    pageTitle="Jupyter jegyzetfüzet telepítése a számítógépen, és kapcsolja egy HDInsight külső fürthöz |} Microsoft Azure" 
    description="Ismerje meg, Jupyter jegyzetfüzet helyileg telepítése a számítógépen, és csatlakoztassa a Azure HDInsight-Apache külső fürt kapcsolatban." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Jupyter jegyzetfüzet telepítése a számítógépen, és csatlakozzon a HDInsight Linux Apache külső fürthöz

Ez a cikk megtanulhatja, hogyan Jupyter jegyzetfüzetet, telepítse az egyéni PySpark (a Python) és a külső mágikus (a Scala) külső mag, és a jegyzetfüzet-HDInsight fürthöz csatlakozni. Számos Jupyter telepítéséhez a helyi számítógépen oka lehet, és néhány kihívásokkal kapcsolatban is lehet. Okok és problémáit listáját, [Miért célszerű telepíteni a számítógépen Jupyter](#why-should-i-install-jupyter-on-my-computer) Ez a cikk végén című.

Három fő lépésből áll részt Jupyter és a külső mágikus telepítése a számítógépen.

* Jupyter jegyzetfüzet telepítése
* A külső mágikus a PySpark és a külső mag telepíteni
* Állítsa be a külső mágikus külső fürt HDInsight elérése

Többet szeretne tudni az egyéni mag és a külső mágikus HDInsight fürt Jupyter jegyzetfüzetek érhető el olvassa el a [HDInsight Apache külső Linux fürt Jupyter jegyzetfüzetek elérhető mag](hdinsight-apache-spark-jupyter-notebook-kernels.md)című témakört.

##<a name="prerequisites"></a>Előfeltételek

Az itt felsorolt Előfeltételek sem Jupyter telepítéséhez. Ezek a Jupyter jegyzetfüzet csatlakozhat egy HDInsight fürthöz, a jegyzetfüzet telepítése után.

- Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Egy Apache külső fürthöz Linux hdinsight szolgáltatásból lehetőségre. Című cikkben olvashat [létrehozása Apache külső fürt az Azure hdinsight szolgáltatásból lehetőségre](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter jegyzetfüzet telepítése a számítógépen

Mielőtt telepíthetné az Jupyter jegyzetfüzetek telepíteni kell az Python. Python és Jupyter is rendelkezésre állnak a [Ananconda terjesztési](https://www.continuum.io/downloads)részeként. Anaconda telepítésekor Python eloszlásának ténylegesen telepítése. Miután telepítette az Anaconda, vegye fel a Jupyter telepítést parancs futtatásával. Ez a témakör hajtsa végre az utasításokat.

1. A [Anaconda installer](https://www.continuum.io/downloads) a platform letöltéséhez és a telepítést. Közben a beállítási varázsló, győződjön meg arról, hogy az elérési út változó Anaconda hozzáadása a beállítást választja.

2. A következő parancsot Jupyter telepítéséhez.

        conda install jupyter

    További tájékoztatást a installting Jupyter [Telepítése Jupyter Anaconda használatával](http://jupyter.readthedocs.io/en/latest/install.html)megtekintése

## <a name="install-the-kernels-and-spark-magic"></a>Telepítse a mag és a külső mágikus

A külső mágikus telepítése, PySpark és a külső mag, tanulmányozza a [sparkmagic dokumentáció](https://github.com/jupyter-incubator/sparkmagic#installation) GitHub a.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Konfigurálja a HDInsight külső fürt eléréséhez külső mágikus

Ebben a szakaszban a korábban telepített egy Apache külső fürthöz kell már létrehozott az Azure hdinsight szolgáltatáshoz való kapcsolódás külső mágikus konfigurálása.

1. A Jupyter konfigurációs adatok általában a felhasználók otthoni könyvtár vannak tárolva. Keresse meg a otthoni címtárban bármely OS platformon, írja be a következő parancsokat.

    Indítsa el a Python rendszerhéj. A parancs ablakban írja be a következőt:

        python

    Adja meg a Python rendszerhéj megtudhatja, hogy az otthoni címtár a következő parancsot.

        import os
        print(os.path.expanduser('~'))

2. Keresse meg a otthoni könyvtár, és hozzon létre egy nevű **.sparkmagic** , ha még nem létezik.

3. A mappába hozzon létre egy **config.json** nevű fájlt, és adja hozzá a következő JSON kódtöredékének belül a kijelöléséhez.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. **{USERNAME}**, **{CLUSTERDNSNAME}**és **{BASE64ENCODEDPASSWORD}** helyettesítése a megfelelő értéket. A kedvenc programnyelv vagy online segédeszközök számos actualy jelszavát kódolt base64 jelszó létrehozására is használhatja. Egy egyszerű Python kódtöredékének futtatásához a parancssorból a következő lesz:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Indítsa el a Jupyter. A parancssorból a következő parancsot használja.

        jupyter notebook

6. Győződjön meg arról, hogy tud csatlakozni a fürthöz a Jupyter jegyzetfüzetet használó és használható a külső mágikus érhető el, a mag. Hajtsa végre az alábbi lépéseket.

    1. Hozzon létre új jegyzetfüzetet. A jobb oldali sarokban kattintson az **Új**gombra. Az alapértelmezett kernel **Python2** és a két új mag telepített, **PySpark** és a **külső**láthatók.

        ![Új Jupyter jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Új Jupyter jegyzetfüzet létrehozása")

    
        Kattintson a **PySpark**.


    2. Futtassa a következő kódrészletet.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        A kimenet sikeresen meghallgathatja, ha a program a kapcsolat a HDInsight fürt a vizsgált.

    >[AZURE.TIP] Ha azt szeretné, egy másik fürthöz csatlakozni a jegyzetfüzet-beállítások frissítése, ahogy a fenti 3 értéket, az újonnan létrehozott frissítse a config.json. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Miért célszerű telepíteni a számítógépen Jupyter?

Számos oka, miért célszerű Jupyter telepítése a számítógépen, és csatlakoztassa a HDInsight külső fürtre lehet.

* Habár Jupyter jegyzetfüzetek már érhetők el a külső fürt az Azure hdinsight szolgáltatáshoz, azt a vezérlőt, amellyel létrehozhat jegyzetfüzeteket helyi meghajtóra, tesztelje a futó fürt szemben az alkalmazást, és töltse be a jegyzetfüzeteket a fürthöz Jupyter telepítése a számítógépen biztosít. Töltse fel a jegyzetfüzeteket a fürthöz, is töltse fel őket a Jupyter jegyzetfüzet futtató vagy a fürt, vagy mentse őket a /HdiNotebooks mappába fürthöz társított tárterület-fiókjában. További tájékoztatást a jegyzetfüzetek hogyan találhatók a fürt című témakört [Jupyter jegyzetfüzetek tároló](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* A rendelkezésre álló jegyzetfüzeteket helyi meghajtóra, csatlakozhat más külső fürt az alkalmazás követelmény alapján.
* Egy adatforrás-vezérlő rendszer megvalósítása, és a jegyzetfüzeteket a verziókövetés GitHub is használhatja. Egy olyan együttműködési környezet kialakítása, ahol több felhasználó ugyanazt a jegyzetfüzetet kezelnek is.
* Dolgozhat a jegyzetfüzetek helyi anélkül, hogy kellene fürt. Csak akkor van szükség tesztelése szemben, a jegyzetfüzetek nem szeretne manuális kezelése a jegyzetfüzetek és a fejlesztői környezet fürt.
* Lehet, hogy könnyebben saját helyi fejlesztői környezet beállítása, mint amilyen a Jupyter telepítés konfigurálása a fürt.  Használatba veheti a szoftver minden egy vagy több távoli fürtre beállítása nélkül helyileg telepített.

>[AZURE.WARNING] Jupyter a helyi számítógépen telepítve, a több felhasználó ugyanazt a jegyzetfüzetet a futtathatók az azonos külső fürt egy időben. Ebben az esetben több Livius munkamenetek jönnek létre. Ha szeretné hibakeresése, és belefutna egy problémába, mely felhasználói tartozik egy összetett feladat nyomon követéséhez melyik Livius munkamenetet lesz.




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

* [A HDInsight külső fürt Zeppelin jegyzetfüzetek használata](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Elérhető az HDInsight-külső fürthöz Jupyter jegyzetfüzet mag](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Külső csomagok Jupyter jegyzetfüzeteket használata](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Erőforrások kezelése

* [A Apache külső fürt Azure hdinsight szolgáltatáshoz a források kezelése](hdinsight-apache-spark-resource-manager.md)

* [A a HDInsight-Apache külső fürthöz nyomon követése és hibakeresési feladatok](hdinsight-apache-spark-job-debugging.md)
