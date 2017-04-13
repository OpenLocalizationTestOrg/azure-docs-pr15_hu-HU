<properties 
    pageTitle="Egyéni tárak használata egy HDInsight külső fürthöz webhely naplók elemzéséhez |} Microsoft Azure" 
    description="Egyéni tárak használata az egy HDInsight külső fürthöz webhely naplók elemzéséhez" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Egyéni tár használata Apache külső fürt HDInsight Linux webhely naplók elemzése

A jegyzetfüzet bemutatja, hogyan egyéni tár használata a HDInsight külső napló adatok elemzése céljából. Az egyéni tár használjuk **iislogparser.py**nevű Python tárban.

> [AZURE.TIP] Ebben az oktatóanyagban érhető el, a külső (Linux) fürtre HDInsight létrehozott Jupyter jegyzetfüzet. A jegyzetfüzet felület teszi lehetővé a Python kódrészletek lebonyolítása a jegyzetfüzetet önmagában. A jegyzetfüzeten belül az oktatóprogram elvégzéséhez hozzon létre egy külső fürt, indítsa el a jegyzetfüzet Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), majd futtassa a Jegyzetfüzet- **elemzés használata egy egyéni library.ipynb külső jelentkezik** a **PySpark** mappában találja.

**Előfeltételek:**

Rendelkeznie kell a következőket:

- Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Egy Apache külső fürthöz Linux hdinsight szolgáltatásból lehetőségre. Című cikkben olvashat [létrehozása Apache külső fürt az Azure hdinsight szolgáltatásból lehetőségre](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Egy RDD nyers adatokból mentése

Ebben a szakaszban a HDInsight-Apache külső fürt társított [Jupyter](https://jupyter.org) jegyzetfüzet feladat, amely a nyers mintaadatok folyamat, és mentse a struktúra táblázatként futtatható használjuk. A mintaadatok egy .csv fájlt (hvac.csv) elérhető alapértelmezés szerint minden fürt.

Miután az adatok mentésének struktúra táblázatként, a következő szakaszban azt fog csatlakozni az struktúratáblával Üzletiintelligencia-eszközöket, például a Power BI és Tableau használatával.

1. Az [Azure-portált](https://portal.azure.com/)a a startboard kattintson a csempére a külső fürt (Ha a startboard a kiemelt). Az **Összes böngészése**a fürthöz is navigálhat > **Fürt hdinsight szolgáltatásból lehetőségre**.   

2. A külső fürt lap **Fürt irányítópult**elemre, és kattintson a **Jupyter jegyzetfüzet**elemre. Ha a rendszer kéri, írja be a fürt a rendszergazdai hitelesítő adataival.

    > [AZURE.NOTE] Előfordulhat, hogy is elér Jupyter jegyzetfüzet a fürt számára a böngészőben nyissa meg az alábbi URL-címet. __CLUSTERNAME__ cserélje ki a fürt neve:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Hozzon létre új jegyzetfüzetet. Kattintson az **Új**gombra, és kattintson a **PySpark**.

    ![Új Jupyter jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Új Jupyter jegyzetfüzet létrehozása")

3. Új jegyzetfüzet létrejön, és Untitled.pynb nevű megnyitni. Kattintson a jegyzetfüzet nevére a képernyő tetején, és írjon be egy rövid nevet.

    ![Adjon egy nevet a jegyzetfüzetnek] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Adjon egy nevet a jegyzetfüzetnek")

4. Mivel a PySpark kernelt jegyzetfüzet hozta létre, nem kell bármely környezetek kifejezetten létrehozása. A külső és struktúra környezetek automatikusan létrejön, az első kód cellát futtatásakor. Ebben az esetben szükséges fájltípusok importálásával elindíthatja. Illessze be az alábbi kódtöredékének egy üres cellába, és nyomja le a **SHIFT + ENTER BILLENTYŰKOMBINÁCIÓT**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Hozzon létre egy már elérhető minta naplóadatok használata a fürt RDD. Az alapértelmezett tárterület-fiók a fürthöz **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**társított adatainak érheti el.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Lekérdezi egy mintanézet beállítása annak ellenőrzésére, hogy az előző lépésben sikeresen befejeződött.

        logs.take(5)

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Adatelemzés napló segítségével egyéni Python tárban

7. A eredménye a fenti az első néhány sor, vegye fel a fejlécadatokat, és minden megmaradt sor megfelelő az adott élőfej ismertetett séma. Az ilyen naplók elemzése bonyolult lehet. Igen egyéni Python tár (**iislogparser.py**), amelyek jelentősen megkönnyíti az ilyen naplók elemzése használjuk. Alapértelmezés szerint a tár megtalálható a **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**a külső fürthöz a hdinsight szolgáltatásból lehetőségre.

    A tár viszont nem az az `PYTHONPATH` , például importálása utasítás segítségével nem használatának `import iislogparser`. A tár használatához azt kell juttatnia az összes dolgozó csomóponthoz. Futtassa a következő kódtöredékének.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`Itt függvény `parse_log_line` , amely visszaadja a `None` Ha napló vonal fejlécsor, és egy példányát adja vissza az `LogLine` osztály, ha, amelyekkel találkozik, a napló sor. Használja a `LogLine` csak azok a napló Sorok kinyerése a RDD osztály:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Lekérdezi kibontott napló sorban annak ellenőrzésére, hogy néhány a lépés sikeresen befejeződött.

        logLines.take(2)

    A kimenet kell lennie az alábbihoz hasonló:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. A `LogLine` osztálynak, viszont az például néhány hasznos módszer van `is_error()`, amely adja eredményül, hogy van-e a naplóbejegyzést hibakód. Ezzel a paranccsal a kibontott napló a sorokban lévő hibák számát számítja ki, és jelentkezzen be az összes hibát egy másik fájlba.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. **Matplotlib** létrehozni megjelenítésnek az adatok is használhatja. Például ha hosszú ideig futó kérések okának, érdemes lehet keresheti meg a fájlokat, amely a legtöbb átlagosan szolgáló időt vehet igénybe.
Az alábbi kódtöredékének olvassa be a felső 25 erőforrások, a legtöbb idő kérelmének szolgáló beállításikon.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Ezt az információt az űrlap ábra is adhat elő. Első lépésként hozzon létre egy ábra, tudassa velünk **AverageTime**ideiglenes táblázat első létrehozása. A táblázat a naplókat csoportosítása láthatja, hogy volt bármely szokatlan késés kiugrásainak megfelelő adott bármikor időt.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Futtassa az alábbi SQL-lekérdezés egyben az összes rekordot a **AverageTime** táblában.

        %%sql -o averagetime
        SELECT * FROM AverageTime

    A `%%sql` mágikus követ `-o averagetime` biztosítja, hogy a lekérdezés eredményét (általában a fürt headnode) Jupyter kiszolgálón helyileg állandó. A kimenet egy [Pandas](http://pandas.pydata.org/) dataframe a megadott név **averagetime**, mint az állandó.

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

    ![SQL-lekérdezés eredménye] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "SQL-lekérdezés eredménye")

    További információt a `%%sql` mágikus, valamint egyéb találhatók meg a PySpark kernel magics lásd: [a külső HDInsight fürt Jupyter jegyzetfüzetek mag](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Matplotlib, az adatok megjelenítési használt tár egy ábra létrehozása most már használhatja. Az ábra a helyben tárolt **averagetime** dataframe kell létrehozni, mert a kódrészletet kell kezdődnie a `%%local` mágikus. Ezzel biztosíthatja, hogy a kód Jupyter kiszolgálón helyileg futó.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

    ![Matplotlib kimeneti] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Matplotlib kimeneti")

16. Miután végzett az alkalmazást futtató, tegye a következőt: leállítás engedje fel az erőforrások jegyzetfüzetet. Ehhez a jegyzetfüzetet a **fájl** menüben kattintson a **bezárása és leállíthatja**. Ez lesz leálljon, és a jegyzetfüzet bezárása.
    

## <a name="seealso"></a>Lásd még:


* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: használata külső a HDInsight épület hőmérsékleti fűtés-és Légtechnikai adatok elemzéséhez](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

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
