 <properties
    pageTitle="Bevezetés a hadoop - Mi az a HDInsight Hadoop? | Microsoft Azure"
    description="Ismertetés egy Hadoop elosztott nagy adatfeldolgozás és elemzés és Hadoop ökológiai összetevői HDInsight meg a felhőben."
    keywords="nagy adatelemzés, hadoop, mi az hadoop, hadoop-technológia Papírhalom, hadoop ökológiai – bevezetés"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Azure hdinsight szolgáltatáshoz a Hadoop ökológiai bemutatása

 Ez a cikk bevezető hadoop Azure hdinsight szolgáltatáshoz, annak ökológiai és nagy adatokat. Tudjon meg többet a Hadoop összetevők, a közös kifejezések és a nagy adatelemzés forgatókönyvek.

## <a name="what-is-hadoop-on-hdinsight"></a>Mi az a HDInsight Hadoop?

Hadoop elosztott kezelését, tárolását keretét Megnyitás-forrás szoftver-ökológiai és elemzése a számítógép fürt nagy adatkészletek hivatkozik. Azure hdinsight szolgáltatáshoz elérhetővé teszi a **Hortonworks adatok Platform (HDP)** eloszlás Hadoop összetevőinek a felhőben, és üzembe helyezése, és felügyelt fürt rendelkezések magas megbízhatósága és elérhetőségét.  

Apache Hadoop az eredeti Megnyitás-forrásprojektben nagy adatkezelési volt. Következő kapcsolódó szoftverek és -segédeszközök Apache struktúra, Apache HBase, Apache külső és számos más Hadoop technológia Papírhalom részének számít fejlesztésének volt. További információ a [HDInsight a Hadoop ökológiai áttekintése](#overview) című témakörben találhat.

## <a name="what-is-big-data"></a>Mi az nagy adatok?

Nagy adatok bármely nagy digitális adatait, a Twitteren hírcsatorna, a érzékelő információkhoz ipari berendezések böngészési ügyfél- és vásárlások információt egy webhelyen lévő szöveg törzsében ismerteti. Nagy adat lehet korábbi (azaz a gyorsítótárban tárolt adatokat) vagy valós idejű (tehát, hogy közvetlenül a forrásból származó folyamatosan lejátszott). Nagy adatokat gyűjtenek minden eddiginél escalating mennyiségű, egyre nagyobb sebességek, és egy Kibontás különböző formátumokban.

Az értekezletekre üzletiintelligencia- vagy betekintést nyújtanak nagy adatok vonatkozó adatok gyűjtése és a megfelelő kérdéseket. Is gondoskodnia kell arról, hogy az adatok érhető el, törölve, elemezheti és majd foglalt kényelmes. Ez a Hadoop HDInsight a nagy adatelemzés segítséget hol.

## <a name="overview"></a>A HDInsight Hadoop ökológiai áttekintése

HDInsight nagy adatelemzés gyorsan bővülő Apache Hadoop-technológia készlet a Microsoft Azure felhő terjesztési. Apache külső, HBase, vihar, malac, struktúra, Sqoop, Oozie, Ambari és így tovább példányait tartalmazza. HDInsight is integrálódik üzleti üzletiintelligencia-eszközöket, például a Power BI, az Excel, az SQL Server Analysis Services és az SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase, külső, vihar és testre szabott fürt

HDInsight fürt konfigurációk Apache Hadoop, külső, HBase vagy vihar biztosít. Vagy [parancsprogram-műveleteinek fürt](hdinsight-hadoop-customize-cluster-linux.md)testreszabhatja.

* **Hadoop**: megbízható adatok tárolására szolgál [Fájlrendszerhez](#hdfs), és egy egyszerű [MapReduce](#mapreduce) programozási modell folyamat, és a párhuzamos található adatok elemzése.

* **<a target="_blank" href="http://spark.apache.org/">A külső Apache</a>**: A párhuzamos feldolgozási keretrendszer, amely támogatja a memóriában feldolgozás nagy adatelemzés alkalmazások, a teljesítmény works dokumentuma SQL, adatfolyam, és a számítógép tanulási. Lásd: [áttekintése: Mi az a HDInsight Apache külső?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**:, amelybe elérésű és erős konzisztencia félig strukturált és strukturálatlan adatokat - esetleg a sorok milliárd nagy mennyiségű Hadoop-ra épülő NoSQL adatbázis időtúllépés oszlopok milliónyi. [A HDInsight a HBase áttekintése](hdinsight-hbase-overview.md)című témakörben találhat.

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache vihar</a>**: elosztott, valós idejű számítási rendszer nagy adatfolyamok az adatok gyors feldolgozása. A HDInsight felügyelt fürt vihar kínálják. Lásd: az [elemzés valós idejű szenzoradatokat vihar és Hadoop használatával](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Példa testreszabási parancsfájlok

Parancsfájl-műveletek parancsprogramok futtatása közben fürt kiépítési, és további összetevőit telepítése a fürt használható. Linux-alapú fürtre vonatkozóan ezek Bash parancsfájlok.

A következő példa parancsfájlok a HDInsight csoportja által nyújtott:

* [Szín](hdinsight-hadoop-hue-linux.md): fürt vezérléséhez használt webalkalmazások A csoportját. Csak Linux fürt.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): dolog, amit vagy a személyek modell kapcsolatának feldolgozás Graph.

* [R](hdinsight-hadoop-r-scripts-linux.md): egy megnyitott Forrásnyelv és gépi tanulási felhasznált statisztikai számítások környezet.

* [Solr](hdinsight-hadoop-solr-install-linux.md): egy vállalati szintű keresési platform, amely lehetővé teszi, hogy a teljes szöveges keresési adatok.

Egyéni parancsfájl-műveletek fejlesztésével kapcsolatos [parancsfájl műveletet fejlesztési HDInsight a](hdinsight-hadoop-script-actions-linux.md)témakörben talál.


## <a name="what-are-the-hadoop-components-and-utilities"></a>Mik azok a Hadoop-összetevők és a segédprogramok?

Az alábbi összetevőket és segédprogramok HDInsight fürt szerepelnek.

* **[Ambari](#ambari)**: fürt kiépítési, management, figyelésére és segédprogramot.

* **[Avro](#avro)** (Microsoft .NET Library Avro): a Microsoft .NET-környezetben adatszerializáció.

* **[Struktúra & HCatalog](#hive)**: Structured Query Language (SQL) – például lekérdezése és a táblázat és a tárhely kezelése réteg.

* **[Mahout](#mahout)**: méretezhető gépi tanulási a alkalmazásokat.

* **[MapReduce](#mapreduce)**: régi keretét Hadoop elosztott feldolgozás és erőforrás-kezelés. Lásd: [fonal](#yarn), a következő generációs erőforrás keretében.

* **[Oozie](#oozie)**: munkafolyamatok kezelése.

* **[Phoenix](#phoenix)**: relációs adatbázisból réteg HBase fölé.

* **[Malac](#pig)**: egyszerűbb parancsfájlok MapReduce átalakítások.

* **[Sqoop](#sqoop)**: adatok importálása és exportálása.

* **[Tez](#tez)**: lehetővé teszi az adatok igényű folyamatok hatékony futtatásához a méretezés.

* **[Fonal](#yarn)**: a Hadoop core könyvtár és a következő generációs MapReduce szoftver keretrendszer része.

* **[ZooKeeper](#zookeeper)**: elosztott rendszerek folyamatok összehangolására.

> [AZURE.NOTE] Az adott összetevők információkat és verzióadatok című témakörben talál [Hadoop-összetevők, verziószámozás, és a HDInsight szolgáltatásajánlatok][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari kiépítési, kezelése és Apache Hadoop fürt ellenőrzésére szolgál. Egy ötletes webhelycsoport operátor eszközök és Hadoop, fürt működésének egyszerűsítése komplexitását elrejtése API-k robusztus halmazából tartalmazza. Linux-alapú HDInsight fürt gondoskodik mind a Ambari webes felhasználói felület és a Ambari REST API-t, miközben Windows-alapú fürt adja meg a REST API-t egy részét. A HDInsight fürt Ambari nézetek lehetővé teszik a beépülő modul felhasználói felület funkciók.

Lásd: [Ambari használatával kezelheti a HDInsight fürt](hdinsight-hadoop-manage-ambari.md) (csak Linux), [a Ambari API HDInsight a Monitor Hadoop fürt](hdinsight-monitor-use-ambari-api.md)és <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API hivatkozást</a>.

### <a name="avro"></a>Avro (Microsoft .NET függvénytár-Avro)

A Microsoft .NET-tár Avro a végrehajtja a Apache Avro kompakt bináris data interchange formátum szerializáláshoz a Microsoft .NET-környezetben. Nyelvi interoperability köt nyelvi-felismerése nélkül sémára definiálása <a target="_blank" href="http://www.json.org/">JavaScript objektum jelölés (JSON)</a> használ, és több nyelv eredményezi jelentése adatokat elolvashatja a másik. Részletes információkat a formátum megtalálható a < cél = "üres" href _ = "http://avro.apache.org/docs/current/spec.html" > Apache Avro specifikációja</a>.
Avro fájlok formátumának támogatja a elosztott MapReduce programozási modell. A fájlok "táblázat felosztása", azaz a Keresés a fájl bármely pontjára, és olvasási indítása az egy adott szövegrészt. Megtudhatja, hogy miként, olvassa el a [Szerializált adatok Avro a Microsoft .NET-tárral](hdinsight-dotnet-avro-serialization.md)című témakört.


### <a name="hdfs"></a>FÁJLRENDSZERHEZ

Hadoop elosztott fájl rendszer (hdfs) lehetőségre, amely, MapReduce és fonal, az alapvető ökológiai Hadoop elosztott fájlrendszer. Fájlrendszerhez rendszer a szabványos Hadoop fürtre a HDInsight vonatkozóan.

### <a name="hive"></a>Struktúra & HCatalog

Adatok raktári szoftver, amely lehetővé teszi, hogy a lekérdezés és nagy adatkészletek elosztott tárolóban lévő kezelése HiveQL nevű SQL-hasonló nyelvek használatával Hadoop-ra épülő <a target="_blank" href="http://hive.apache.org/">Apache struktúra</a> . Malac, például a struktúra fölött MapReduce absztrakciója. Futtatásához struktúra MapReduce feladatok sorozata lekérdezések alakítja. Struktúra adatbázistáblákhoz közelebb a relációs adatbázis-kezelő rendszer malac-nál, és ezért megfelelő további strukturált adatok való használatra. Malac strukturálatlan adatokat, a legjobb választás. Ismerje meg [a Hadoop HDInsight a struktúra](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> Hadoop, amely bemutatja a felhasználók relációs adatok megjelenítése a táblázat és a tárhely kezelése réteg. A HCatalog olvassa el, és bármilyen struktúra SerDe (szerializáló-deszerializáló) írható formátumú fájlok írása.

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> gépi tanulási Hadoop futó algoritmusok méretezhető gyűjteményének. Gépi tanulási alkalmazások lapon statisztika alapelvei használ, a rendszerek adatokból és eredményük múltbeli használatával jövőbeli működés meghatározása. Lásd: a [Létrehozás film javaslatok a Hadoop Mahout használatával](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce Hadoop régi szoftver keretét, az alkalmazások folyamata nagy adatkészletek párhuzamosan köteg írásához. MapReduce feladat nagy adathalmazok kettéválaszthatja-e, és az adatok rendezi a feldolgozásra kulcs – érték párokká.

[A Hadoop következő generációs erőforrás-kezelő és az alkalmazás keretet, és nevezik MapReduce 2.0-s verziója.](#yarn) MapReduce feladatok fonal futtatható.

MapReduce kapcsolatos további tudnivalókért olvassa el a <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> a Hadoop Wiki című témakört.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> rendszer munkafolyamat effektusával Hadoop feladatok kezelő. A Hadoop Papírhalom integrálva van és a Hadoop feladatok MapReduce, malac, struktúra és Sqoop támogatja. Azt is használható a rendszer, például Java-programok vagy rendszerhéj parancsfájlok jellemző feladatok ütemezése. Lásd: [a Hadoop használata Oozie](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> a relációs adatbázisok réteg HBase fölé. Phoenix, amely lehetővé teszi a felhasználóknak, hogy a lekérdezés vagy SQL-táblák közvetlenül kezelése JDBC illesztőprogramot tartalmazza. Phoenix megfelelője lekérdezések és más kimutatások natív NoSQL API-hívások - helyett MapReduce használatával – így gyorsabban alkalmazások fölött NoSQL tárolja. Lásd: [használata Apache Phoenix és a HBase fürt erdeimókus](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Malac
<a  target="_blank" href="http://pig.apache.org/">Apache malac</a> olyan magas szintű platform, amely lehetővé teszi, hogy bonyolult MapReduce átalakítások végrehajtani a nagyon nagy adathalmazok malac Latin néven egyszerű parancsfájlok nyelvet használ. Malac a malac Latin parancsfájlok átalakítja, így fog Hadoop belül futnak. Felhasználó által definiált függvényeket (UDF), ha ki szeretné terjeszteni a malac Latin hozhat létre. Lásd: [Hadoop malac használatát](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> , hogy a átvitelek lehető leghatékonyabb adatok Hadoop és a relációs adatbázisok, például egy SQL között, vagy más strukturált adatokat tárolja tömeges eszközt. Lásd: [a Hadoop használata Sqoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> egy alkalmazás keret, amelyek összetett, aciklikus grafikonok általános adatfeldolgozás végrehajtja a Hadoop fonal épül. A a MapReduce keretrendszer, amely lehetővé teszi, hogy az adatok igényű folyamatok, például struktúra hatékonyabban futtatásához a skála rugalmas, és hatékony követő. Lásd: ["Használata Apache Tez nagyobb teljesítmény az" használata struktúra és HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>FONAL
Apache van a következő MapReduce (MapReduce 2.0-s vagy MRv2) létrehozása és adatfeldolgozási lehetőséget a nagyobb méretezhetőség és a valós idejű feldolgozás feldolgozása MapReduce köteg túl támogat. Erőforrás-kezelés és egy elosztott keretrendszer fonal ismertetése MapReduce feladatok fonal futtatható.

FONAL kapcsolatos további tudnivalókért olvassa el a <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (fonal)</a>című témakört.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> folyamatok nagy elosztott rendszerekben adatok nyilvántartások (znodes) megosztott hierarchikus névteret segítségével koordinálja. Znodes tartalmazzák a szükséges folyamatok koordinálása metaadatait, kis mennyiségű: állapot, hely, konfigurációs és így tovább.

## <a name="programming-languages-on-hdinsight"></a>A HDInsight programozási nyelven

HDInsight fürt – Hadoop, HBase vihar és a külső fürt – támogatási programnyelv számos, de néhány alapértelmezés szerint nincsenek telepítve. A tárak, modulok és csomagok alapértelmezés szerint nincs telepítve a parancsprogram művelettel telepítse az összetevőt. Lásd: [parancsfájl művelet fejlesztési a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Alapértelmezett programozási nyelvek támogatása

Alapértelmezés szerint a HDInsight fürtök támogatása:

* Java

* Python

További nyelvek parancsfájl-műveletek telepíthető: [parancsfájl művelet fejlesztési a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java virtuális gép (JVM) nyelvek

Java-től különböző több nyelvet (JVM); Java virtuális gépet futtatható azonban fut a következő nyelveken részét szükség lehet a fürt telepítendő további összetevőit.

Ezek JVM írást használó nyelvek HDInsight fürt támogatottak:

* Clojure

* Jython (Python Java)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-specifikus nyelvek

HDInsight fürt Hadoop ökológiai jellemző a következő nyelvet támogatja:

* Malac Latin malac feladatokhoz

* A struktúra feladatok és SparkSQL HiveQL


## <a name="advantage"></a>A felhőben Hadoop előnyei

Azure felhő ökológiai részeként a HDInsight Hadoop számos előnyökkel jár, ezek közül:

* Hadoop fürt automatikus kiépítése. HDInsight fürt jelentősen megkönnyíti, mint manuális beállítása Hadoop fürt létrehozásához. A részletekért olvassa [rendelkezést Hadoop fürt a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-provision-linux-clusters.md).

* Állapot-elemeket a Hadoop-összetevők. A részletekért olvassa [Hadoop-összetevők, verziószámozás, és a HDInsight szolgáltatásajánlatok][component-versioning].

* Magas rendelkezésre állásának és fürt megbízhatóságát. [Elérhetőség és a HDInsight Hadoop fürt megbízhatóságát](hdinsight-high-availability-linux.md) információt talál.

* Hatékonyak és gazdaságos adattárolás az Azure Blob-tárolóhoz, egy Hadoop-kompatibilis lehetőséget. [Azure blobtárolóhoz használata a Hadoop a HDInsight](hdinsight-hadoop-use-blob-storage.md) talál részleteket.

* Integráció az más Azure szolgáltatásaival, beleértve a [Web Apps alkalmazások](https://azure.microsoft.com/documentation/services/app-service/web/) és az [SQL-adatbázishoz](https://azure.microsoft.com/documentation/services/sql-database/).

* További virtuális méretét és a HDInsight fürt futó típusok. Lásd: [Hadoop-összetevők, verziószámozás, és a HDInsight szolgáltatásajánlatok] [ component-versioning] további információt.

* A fürt méretezését. Fürt méretezés lehetővé teszi, hogy egy futó HDInsight fürt csomópontok számának módosítása anélkül, hogy törlése, és hozza létre.

* Virtuális hálózati támogatása. HDInsight fürt kínál Azure virtuális hálózat támogatása a felhőben erőforrások vagy hibrid környezetek kialakításához, amelyek hivatkozás: cloud erőforrások azokkal a adatközpontban elkülönítési.

* Alacsony bejegyzés költség. Indítsa el az [ingyenes próbaverzióra](https://azure.microsoft.com/free/), vagy olvassa el a [HDInsight árak részleteket](https://azure.microsoft.com/pricing/details/hdinsight/).

További információ a Hadoop a HDInsight előnyei lásd: az [Azure-szolgáltatások lapon, a HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard és HDInsight-támogatás

HDInsight nyújt két kategóriában, normál és prémium nagy adatok felhőben szeretne rendelni. HDInsight Standard tartalmaz egy vállalati szintű fürthöz használó szervezetek a nagy adatok munkaterhelésekből futtatásához. HDInsight prémium bejegyzései, és hozza létre, amely a speciális analitikai és egy HDInsight fürt biztonsági funkciókat. További tudnivalókért lásd: [Azure hdinsight szolgáltatáshoz prémium verzió](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>További információ a nagy adatelemzés, Hadoop és HDInsight tanulási források

Az a felhő és az alábbi forrásokkal nagy adatelemzés Hadoop bevezetőből épülnek.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop dokumentációjában hdinsight szolgáltatáshoz

* [HDInsight dokumentáció](https://azure.microsoft.com/documentation/services/hdinsight/): Azure hdinsight szolgáltatáshoz a Hadoop dokumentáció lap cikkek, videók és további források mutató hivatkozásokkal.

* [A HDInsight Hadoop – első lépések](hdinsight-hadoop-linux-tutorial-get-started.md): a kiépítési HDInsight Hadoop fürt és minta struktúra lekérdezések futtatása rövid oktatóanyagot.

* [Külső HDInsight az első lépések](hdinsight-apache-spark-jupyter-spark-sql.md): a külső fürt létrehozása és interaktív külső SQL-lekérdezések futtatása rövid oktatóanyagot.

* [A HDInsight R-kiszolgáló használata](hdinsight-hadoop-r-server-get-started.md): Start HDInsight prémium R-kiszolgálóval.

* [Rendelkezést HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md): megtudhatja, hogy miként hozhatók létre egy HDInsight Hadoop fürthöz az Azure portál, az Azure CLI vagy az Azure PowerShell keresztül.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: További tudnivalók a Apache Hadoop szoftver tárban, amely lehetővé teszi, hogy a nagyméretű adatkészletek elosztott feldolgozás fürt számítógépek közötti keretrendszert.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">Fájlrendszerhez</a>: További információ a architektúrája és a Hadoop elosztott fájlrendszer, a Hadoop-alkalmazások által használt elsődleges tároló rendszer kialakítását.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce oktatóprogram</a>: További tudnivalók a Hadoop-alkalmazások gyors feldolgozását nagy mennyiségű adatot párhuzamosan a számítási csomópontok nagy fürt írásához programozási keretében.


### <a name="microsoft-business-intelligence"></a>A Microsoft üzleti intelligencia

A jól ismert üzleti Üzletiintelligencia-eszközök – például az Excel PowerPivot, SQL Server Analysis Services és az SQL Server Reporting Services - beolvasásához, elemzése és jelenteni az adatok HDInsight integrálódik a Power Query bővítmény vagy a a Microsoft struktúra ODBC-illesztőprogram használatával.

Ezek az Üzletiintelligencia-eszközeiről a nagy adatelemzésben segíthetnek:

* [Hadoop a Power Query az Excel csatlakozni](hdinsight-connect-excel-power-query.md): megtudhatja, hogy miként fiókhoz való csatlakozáshoz Excel az Azure tárhely, amely tárolja az adatokat a HDInsight fürt társított az Excelhez készült Microsoft Power Query használatával. A Windows munkaállomás szükséges. Windows - vagy Linux-alapú fürt működik.

* [Excel csatlakoztatása a Microsoft struktúra ODBC-illesztőprogram hadoop](hdinsight-connect-excel-hive-odbc-driver.md): Útmutató: adatok importálása a Microsoft struktúra ODBC-illesztőprogram hdinsight szolgáltatásból lehetőségre. A Windows munkaállomás szükséges. Windows - vagy Linux-alapú fürt működik.

* [Microsoft Cloud Platform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): tudnivalók a Power BI az Office 365-höz, az SQL Server próbaverzió letöltése és beállítása a SharePoint Server 2013 és az SQL Server üzleti Intelligencia.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
