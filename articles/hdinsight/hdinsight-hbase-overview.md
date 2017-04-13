<properties
    pageTitle="Mi az a HDInsight HBase? | Microsoft Azure"
    description="A HDInsight Apache HBase bemutatása, NoSQL adatbázis Hadoop épülnek. Használati eset ismertetése, és a többi Hadoop fürt HBase hasonlítsa össze."
    keywords="bigtable, nosql, hbase újdonságai"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Mi az a HDInsight HBase: BigTable hasonló funkciókat nyújt a Hadoop A NoSQL adatbázishoz

Apache HBase, amely Hadoop-ra épülő és Google BigTable után modellezni Megnyitás-forrást, NoSQL adatbázissá. HBase elérésű és erős konzisztencia nagy mennyiségű oszlop családok rendszerezve schemaless adatbázisban semistructured strukturált és strukturálatlan adatokat tartalmaz.

Adatok táblázat sorai vannak tárolva, és a sor adatait csoportosított oszlop család. HBase, hogy az oszlopok és milyen típusú adatokat tárolja őket nem kell őket használata előtt definiálhatók értelemben schemaless adatbázis. A Megnyitás forráskód kezelésére vonatkozó adatok csomópontok ezer petabytes lineárisan méretezze át. Adatok redundancia parancsfájl és egyéb ökológiai Hadoop elosztott alkalmazások által biztosított szolgáltatások alapján.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Hogyan történik a HBase az Azure hdinsight szolgáltatáshoz?

A felügyelt fürtre Azure környezetbe integrált HDInsight HBase kínálják. Tárolja az adatokat közvetlenül az Azure Blob-tárolóhoz, amely magában foglalja a kis késés és a teljesítmény és a költség lehetőség a nagyobb rugalmasság a fürt vannak beállítva. Ez lehetővé teszi, hogy az ügyfelek, a nagy adatkészletek-szolgáltatások végpontjait milliónyi érzékelő és telemetriai adatokat tároló létrehozásához, és a Hadoop-feladatok az adatok elemzéséhez használható interaktív webhelyek létrehozására. HBase és Hadoop akkor célszerű pontok nagy adatok projekt kezdési Azure;-ban Mindenekelőtt tagjaik valós idejű alkalmazások nagy adathalmazok végezhető.

A HDInsight végrehajtása automatikus sharding, táblák, az Olvasás és írás és automatikusan átveszi erős konzisztencia megadására HBase méretezési architektúráját használja. Lesz a teljesítmény a memóriában gyorsítótárazás olvasása és írása a folyamatos átvitelű nagy átviteli szerint. Virtuális hálózati kiépítési érhető el is HBase hdinsight szolgáltatásból lehetőségre. További részleteket találhat a [rendelkezést HDInsight fürt Azure virtuális hálózaton] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Hogyan kezeli az HDInsight HBase adatok?

Adatok segítségével kezelhetők a HBase a `create`, `get`, `put`, és `scan` parancsok helye a HBase rendszerhéj. Adatok használata az adatbázis írják `put` , és olvassa el a `get`. A `scan` parancs segítségével adatokat olvas be a több tábla sorainak. Adatok is lehet kezelni a HBase C# API, amely fölött HBase REST API-t egy ügyfél tárat. Egy HBase adatbázist is lekérdezhető struktúra használatával. Ezek a programozási modellek bemutatása című [HBase Hadoop HDInsight a az első lépések][hbase-get-started]. Dokumentumok közös processzorok is elérhetők, amelyek lehetővé teszik a csomópontok adatkezelés az adatbázist tároló.


## <a name="scenarios-use-cases-for-hbase"></a>Alkalmazási helyzetek: HBase esetek használata
A kanonikus használatieset mely BigTable (és kiterjesztés, HBase) készült webes keresés volt. Keresőmotorok indexeket megfeleltetni feltételek az őket tartalmazó weblapok. Számos más HBase alkalmas használati eset azonban –, amelyek több vannak részletezve ebben a szakaszban.

- Kulcs-érték áruházból

    Egy kulcsot-értéket tároló HBase is alkalmazható, és alkalmas üzenet rendszerek kezelése. Facebook azok üzenetkezelő rendszer HBase használ, és tárolása és kezelése az internetes kommunikáció ideális gombra. WebTable HBase keresse meg és kezelése a táblákat, amelyeket a rendszer kiolvasott weblapokra használja.

- Szenzoradatokat

    HBase akkor lehet hasznos rögzítéséhez fokozatosan összegyűjtött különböző forrásokból származó adatok. Ide tartoznak a közösségi analytics, idősort, interaktív irányítópultok frissítése a trendek és a számláló és naplózási napló rendszerek kezelése. Többek között Bloomberg kereskedők Terminálszolgáltatások, és nyissa meg az idő sorozat adatbázist (OpenTSDB), amely tárolja, és a kiszolgáló rendszer állapotának gyűjtött mértékek hozzáférést biztosít.

- Valós idejű lekérdezés

    [Phoenix](http://phoenix.apache.org/) egy SQL-lekérdezés motort Apache HBase. Egy JDBC eszközillesztőt, hozzáférés, és lekérdezése és kezelése a HBase tábla SQL használatával lehetővé teszi.

- HBase, mint egy platform

    Alkalmazások futtatását is lehetővé teszi HBase fölött, egy adattárhoz használatával. Példák Phoenix, OpenTSDB, Kiji és Titan tartalmazza. Alkalmazások integrálhatja HBase. Többek között struktúra, malac, Solr, vihar, Flume, Impala, külső, idegdúcokat és funkciók.


##<a name="next-steps"></a>Következő lépések

- [Elkezdeni használni a HDInsight Hadoop HBase][hbase-get-started]
- [Virtuális hálózati Azure hdinsight szolgáltatáshoz fürt kiépítése] [hbase-provision-vnet]
- [A HDInsight HBase a replikáció konfigurálása](hdinsight-hbase-geo-replication.md)
- [A HDInsight HBase a Twitteren sentiment elemzése][hbase-twitter-sentiment]
- [A HDInsight (Hadoop) HBase használó Java-alkalmazások létrehozásához használandó maven tesztelése][hbase-build-java-maven]

##<a name="see-also"></a>Lásd még:

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: Elosztott tároló rendszer strukturált adatok](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
