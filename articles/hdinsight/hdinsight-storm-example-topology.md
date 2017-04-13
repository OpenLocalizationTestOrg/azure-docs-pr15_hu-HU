<properties
 pageTitle="Példa Apache vihar topológiák a HDInsight |} Microsoft Azure"
 description="Példa vihar topológiák listájának létrehozott és Apache vihar HDInsight egyszerű C# és Java topológiák együtt, és esemény hubok használata a vizsgált."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="08/23/2016"
 ms.author="larryfr"/>

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Példa vihar toplogies és Apache vihar a HDInsight-összetevők

Az alábbiakban példákat Apache vihar HDInsight a programmal a Microsoft által díjtábláinak létrehozása és listáját. Ezek a példák számos témakört, ne hozzon létre egyszerű C# és Azure szolgáltatások, például esemény hubok, DocumentDB, a Power BI, SQL-adatbázissal, HBase HDInsight és Azure tároló használt Java topológiák terjed ki. Néhány példa is bemutatják, hogyan dolgozhat nem Azure, vagy még nem Microsoft-technológiák, például SignalR és Socket.IO

| Leírás                                                                                             | Bemutató                                         | Nyelvi/keretrendszer         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Azure tó adattár Apache vihar írják](hdinsight-storm-write-data-lake-store.md) | Azure tó adattár írása | Java |
| [Esemény központi Spout és a rögzített forrás](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Az esemény központi Spout és a rögzített forrás | Java |
| [Fejleszthet olyan Java-alapú topológiák Apache vihar a hdinsight szolgáltatáshoz][5797064f]                                 | Maven tesztelése                                                | Java                       |
| [Fejleszthet olyan C# topológiák Apache vihar a Visual Studio segítségével hdinsight szolgáltatáshoz][16fce2d1]                     | HDInsight Tools for Visual Studio                    | C#, Java                   |
| [Hozzon létre több adatfolyamok C# vihar topológiában][ec5a4064]                                         | Több adatfolyam                                     | C#                         |
| [A HDInsight vihar a Twitteren trendfigyelésre témakörök meghatározása][3c86c7c8]                                   | Trident                                              | Java Trident              |
| [Azure esemény hubok vihar HDInsight (C#) kattintson a folyamat eseményeinek][844d1d81]                                | Esemény hubok                                           | C# és Java                |
| [Azure esemény hubok vihar HDInsight (Java) kattintson a folyamat eseményeinek](hdinsight-storm-develop-java-event-hub-topology.md) | Esemény hubok | Java |
| [Egy vihar topológia adatainak ábrázolása a Power Bi segítségével][94d15238]                              | A Power BI                                             | C#                         |
| [Vihar és a HDInsight HBase érzékelő adatok elemzése][ab894747]                                     | Esemény hubok HBase, Socket.IO, webes irányítópult          | C#, Java-, JavaScript, HTML |
| [A folyamat gépjármű szenzoradatokat az esemény hubok HDInsight vihar használata][246ee964]                        | Esemény hubok DocumentDb, tároló Azure Blob (WASB)    | C#, Java                   |
| [Venni, az átalakítás és a betöltés (ETL) az Azure esemény hubok HBase, vihar használata hdinsight szolgáltatásból lehetőségre kattintva][b4b68194] | Esemény hubok HBase                                    | C#                         |
| [Sablon C# vihar topológia project a Azure hdinsight szolgáltatáshoz a vihar szolgáltatásai][ce0c02a2]  | Esemény hubok DocumentDb, az SQL-adatbázisban, HBase, SignalR | C#, Java                   |
| [Méretezhetőség szintek az Azure esemény hubok vihar használata HDInsight olvasása][d6c540e3]           | Üzenet átviteli, esemény-hubok SQL-adatbázis         | C#, Java                   |
| [Események vihar és a HBase használata HDInsight összehangolására.](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [A HDInsight vihar Python használata](hdinsight-storm-develop-python-topology.md) | Java és Clojure Python összetevők vihar topológiák alapján | Python |

## <a name="next-steps"></a>Következő lépések

* [A HDInsight Apache vihar – első lépések][2b8c3488]

* [Megtudhatja, hogy miként üzembe helyezéséhez és kezeléséhez, a HDInsight a vihar vihar topológiák][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Megtudhatja, hogy miként hozzon létre egy vihar HDInsight csoportját, és példa topológiák vihar irányítópult használatával."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Megtudhatja, hogy miként üzembe helyezéséhez és kezeléséhez használja a webes vihar irányítópult és vihar felhasználói felület vagy a HDInsight eszközök for Visual Studio topológiák."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Megtudhatja, hogyan hozhat létre a C# vihar topológiák for Visual Studio HDInsight eszközeivel."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Megtudhatja, hogyan hozhat létre vihar topológiák Java maven tesztelése, használatával: hozzon létre egy egyszerű wordcount topológia."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Bemutatja, hogyan C# topológia Power BI írásával, majd a diagram- és irányítópult létrehozása az adatokból."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Bemutatja, hogyan egy egyszerű vihar topológia, amely egy wordcount szerepelni fog C# hajt végre. Ez is bemutatja, hogyan hozhat létre több adatfolyamok C# topológia belül."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Megtudhatja, hogy miként adatainak olvasása és írása az Azure esemény hubok a vihar a hdinsight szolgáltatásból lehetőségre."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Megtudhatja, hogy miként Apache vihar használandó HDInsight Azure esemény hubok érzékelő adatainak feldolgozása, ábrázolása D3.js használatával, és (ha szükséges,) HBase tárolja azt."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "A Twitter megtudhatja, hogy miként Trident használatával hozzon létre egy vihar topológia határozza meg, hogy trendfigyelésre témakörök (hashtags, alapján)."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Egy vihar topológia használata üzenetek olvasása az Azure-esemény hubok dokumentumokat olvasni az Azure DocumentDB az adatok hivatkozik, és mentse az adatokat Azure tárolóhoz."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Több topológiák átviteli szemlélteti az Azure esemény hubok olvasása és tárolását HDInsight Apache vihar használata SQL-adatbázishoz."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Megtudhatja, hogy miként Azure esemény hubok összesített adatok olvassák és az adatokat a, majd tárolja azt a HDInsight HBase szeretne."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "A projekt spouts, csapszegek és különböző Azure szolgáltatásokkal, például az esemény hubokba, DocumentDB és SQL-adatbázis vezérléséhez topológiák sablonokat tartalmaz."
 
