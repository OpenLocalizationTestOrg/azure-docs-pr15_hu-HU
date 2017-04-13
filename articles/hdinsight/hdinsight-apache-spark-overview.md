<properties 
    pageTitle="A HDInsight Apache külső áttekintése |} Microsoft Azure" 
    description="A HDInsight Apache külső és -esetek HDInsight külső használata az alkalmazásokban használandó bemutatása." 
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
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Áttekintés: A külső Apache HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache külső</a> egy megnyitott-forrás párhuzamos, amely támogatja a memóriában feldolgozás a teljesítmény nagy adatok analitikus alkalmazások keretrendszer feldolgozása. A külső feldolgozás motor sebesség, használni, és összetett analytics Kezeléstechnikai épül. A külső meg a memóriában kiszámítása funkciók tehető közelítéses algoritmusok kinek ajánljuk gépi tanulási és graph számítások. A külső egyben kompatibilis Azure Blob-tárolóhoz (WASB), a meglévő adatok Azure-ban tárolt egyszerűen feldolgozhatók keresztül külső.

Amikor a külső fürtre HDInsight hoz létre, Azure számítási erőforrások telepítette és beállította a külső hoz létre. Csak tart körülbelül tíz perccel létrehozása külső fürt hdinsight szolgáltatásból lehetőségre. Az adatok feldolgoztatni Azure Blob-tárolóhoz vannak tárolva. Lásd: [Azure Blob-tárolóhoz használata a HDInsight][hdinsight-storage].

![A külső Apache a Azure hdinsight szolgáltatáshoz] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "A külső Apache a Azure hdinsight szolgáltatáshoz")


**Első lépések az Azure hdinsight szolgáltatáshoz a Apache külső szeretne?** Lásd: [quickstart Útmutató: hozzon létre egy külső fürt HDInsight Linux, és futtassa a minta alkalmazások Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Ismert hibákat és korlátozásokat az aktuális verziójának megjelenése listáját olvassa el a [ismert problémák a Apache külső az Azure hdinsight szolgáltatáshoz (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md)című témakört.


## <a name="why-use-spark-on-azure-hdinsight"></a>Miért érdemes használni a külső Azure hdinsight szolgáltatáshoz a? 

Azure hdinsight szolgáltatáshoz a külső szolgáltatás teljes körű felügyelt kínál. A HDInsight külső előnyei vannak:

| A szolgáltatás                             | Leírás       |
|-------------------------------------|-------------------|
| Kezeléstechnikai fürt létrehozása            | Létrehozhat egy új külső fürt HDInsight perc alatt az Azure adatkezelési portált, Azure PowerShell vagy a HDInsight .NET SDK csomagjában talál. Lásd: a [külső fürt HDInsight az első lépések](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Egyszerű használat érdekében                     | A külső HDInsight fürt előre beállított Jupyter jegyzetfüzetek tartalmazza. Ezek az interaktív adatfeldolgozás és a képi megjelenítések használhatja. Az URL-ek a a https://CLUSTERNAME.azurehdinsight.net/jupyter van. __CLUSTERNAME__ cserélje le a külső HDInsight fürt nevét.|
| REST API-hoz                       | A HDInsight külső [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)tartalmazza, a REST API-alapú külső feladat kiszolgáló távolról nyújt, és figyelemmel követheti a futó feladatok. |
| Azure tó adattár támogatása | A HDInsight külső beállítható úgy, hogy Azure tó adattár használni egy további tárterületet. Tó adattár kapcsolatos további tudnivalókért olvassa el a [Áttekintése az Azure tó adattár](../data-lake-store/data-lake-store-overview.md)című témakört.
| Integrálása az Azure szolgáltatások | A HDInsight külső az Azure esemény hubok összekötő megtalálható. Ügyfelek nyújthat adatfolyam alkalmazások használata az esemény hubok kívül [Kafka](http://kafka.apache.org/), amely már elérhető külső részeként. |
| R Server támogatása  | A HDInsight külső fürthöz R kiszolgáló elosztott R számítások futtatása a sebesség egy külső fürthöz megígért beállíthatja. További tudnivalókért olvassa el a [R Server HDInsight használatának első lépései](hdinsight-hadoop-r-server-get-started.md)című témakört.   |
| Integráció a IntelliJ arról  | A HDInsight beépülő modul IntelliJ hozhat létre, és egy külső HDInsight fürthöz kérelmeiket is használhatja. További információt a [Használata HDInsight eszközök beépülő modul IntelliJ KIDOLGOZNIUK külső HDInsight külső Linux fürt alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)témakörben talál. |
| Egyidejű lekérdezések              | A HDInsight külső egyidejű lekérdezések támogatja. Több lekérdezés egy felhasználónak vagy a különböző felhasználók és az alkalmazások megosztani az fürt ugyanazokat az erőforrásokat a több lekérdezés lehetővé teszi. |
| SSD gyorsítótárazás                 | Megadhatja, hogy a memória vagy a csatolt fürt csomópontok SSD gyorsítótár adatokhoz. A memóriában gyorsítótárazás biztosít a legjobb lekérdezési teljesítményt, de lehet, hogy drága; SSD gyorsítótárazás itt nincs szükség méretének igazítása a teljes adatkészlet a memóriában szükséges fürt létrehozása a lekérdezési teljesítmény javítására nagyszerű lehetőséget.|
| Integráció a Üzletiintelligencia-eszközeiről       | HDInsight-külső adatok elemzéséhez például a [Power BI](http://www.powerbi.com/) és [Tableau](http://www.tableau.com/products/desktop) Üzletiintelligencia-eszközeiről összekötők nyújt.|
| Előre elkészített Anaconda tárak        | A külső HDInsight fürt beépített előtelepítve Anaconda tárak. [Anaconda](http://docs.continuum.io/anaconda/) biztosít közelébe 200 tárak gépi tanulási, adatelemzés, képi és stb.|
| Méretezhetőség:                     | Bár a fürt csomópontok számának létrehozása során adhatja meg, érdemes a nagyobb vagy kisebb a fürt terhelést megfelelően. Az összes HDInsight lehetővé teszi, hogy a fürt található csomópontok számának módosítása. Is a külső fürt húzható az adatvesztés nélkül, mivel az Azure Blob-tárolóban lévő összes adatot tárolja. |
| 24/7 támogatás                    | A külső a HDInsight-SLA 99,9 % up – idő és a vállalati szintű 24/7 támogatás megtalálható.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Mik azok a külső a HDInsight-használati eset

A HDInsight Apache külső lehetővé teszi, hogy az alábbi főbb jelenik meg.

### <a name="interactive-data-analysis-and-bi"></a>Interaktív adatelemzés és üzleti Intelligencia

[Tekintse meg a oktatóprogram](hdinsight-apache-spark-use-bi-tools.md)

A HDInsight Apache külső Azure BLOB tárolja az adatokat. Üzleti szakértők és a fő döntés készítők elemzésével és jelentések készítése fölé, hogy az adatok és elemzése az adatokból interaktív jelentések készítése a Microsoft Power BI segítségével. Elemzők Azure-tárolóban lévő strukturált strukturálatlan/félig adatokból elindíthatja, meghatározása a séma használata jegyzetfüzetek adatokhoz, és ezután létrehozhatja a Microsoft Power BI használata adatmodellek. A HDInsight külső támogatja a külső Üzletiintelligencia-eszközöket, például a Tableau Qlikview és az adatok elemzők, üzleti szakértők és fő döntés készítők egy ideális platform így SAP Lumira számos is.

### <a name="iterative-machine-learning"></a>Iteratív gépi tanulási

[Tekintse meg a oktatóprogram: előrejelzésére épület átlaghőmérsékletűek uisng fűtés-és Légtechnikai adatok](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Tekintse meg a oktatóprogram: élelmiszer vizsgálati eredmények előrejelzésére](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

A külső Apache [MLlib](http://spark.apache.org/mllib/), a gépi tanulási külső épülő tár megtalálható. Mindezeken kívül a HDInsight külső Anaconda, gépi tanulási csomagjai különféle Python eloszlás is tartalmaz. Ez egy beépített támogatása Jupyter jegyzetfüzetek, be- és gépi tanulási alkalmazások hozhat létre egy felső – vonal a környezetben.  

### <a name="streaming-and-real-time-data-analysis"></a>Adatfolyam- és a valós idejű adatok elemzése

[Tekintse meg a oktatóprogram](hdinsight-apache-spark-eventhub-streaming.md)

Valós idejű adatok elemzése idő csökkentése az adatok betekintést által adatok feldolgozása, ahogy azt hajtanak végre, a folyamatos átvitelű megoldás igaz létrehozásába kezdve esetek szolgál. A HDInsight külső valós idejű analytics-megoldások fejlesztésére gazdag támogatása kínál. Külső adatok több forrásból, például Kafka, a Flume, a Twitteren, a ZeroMQ vagy a TCP sockets ingest összekötők már, miközben a HDInsight a külső hozzáadása Azure esemény hubok adatainak ingesting osztályú támogatása. Esemény hubok a legtöbb elterjedt Azure várólista szolgáltatást. Egy ki-be a támogatási problémákat esemény hubok hoz létre dokumentuma a HDInsight-ideális platform valós idejű analytics folyamat készítéséhez.

##<a name="next-steps"></a>Milyen összetevői egy külső fürt mellékelve?

A HDInsight külső, amely alapértelmezés szerint a fürt érhetők el az alábbiakat tartalmazza.

- [A külső Core](https://spark.apache.org/docs/1.5.1/). Tartalmazza a külső Core, a külső SQL, a folyamatos átvitelű API-hoz, GraphX és MLlib külső.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter jegyzetfüzet](https://jupyter.org)

HDInsight a külső is biztosít egy [ODBC-illesztőprogram](http://go.microsoft.com/fwlink/?LinkId=616229) kapcsolat külső fürt a HDInsight az Üzletiintelligencia-eszközöket, például a Microsoft Power BI és Tableau.

## <a name="where-do-i-start"></a>Hol kezdhetem meg?

Nyisson meg egy külső fürt létrehozása HDInsight Linux. Lásd: [quickstart Útmutató: hozzon létre egy külső fürt HDInsight Linux, és futtassa a minta alkalmazások Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Következő lépések

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

* [Jupyter telepítése a számítógépen, és csatlakozzon az HDInsight külső fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése

* [A Apache külső fürt Azure hdinsight szolgáltatáshoz a források kezelése](hdinsight-apache-spark-resource-manager.md)

* [A a HDInsight-Apache külső fürthöz nyomon követése és hibakeresési feladatok](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
