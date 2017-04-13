<properties
 pageTitle="Gépjármű szenzoradatokat Apache vihar a hdinsight szolgáltatásból lehetőségre a folyamat |} Microsoft Azure"
 description="Megtudhatja, hogy miként feldolgozása gépjármű szenzoradatokat az esemény hubok Apache vihar használata hdinsight szolgáltatásból lehetőségre. Modell adatainak vehet fel DocumentDB, és a kimeneti tárolóhoz tárolja."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>A folyamat gépjármű szenzoradatokat az Azure esemény hubok HDInsight Apache vihar használata

Megtudhatja, hogy miként feldolgozása gépjármű szenzoradatokat az Azure esemény hubok Apache vihar használata hdinsight szolgáltatásból lehetőségre. Ebben a példában szenzoradatokat beolvassa az Azure esemény hubok, az adatok dúsítja hivatkozó Azure DocumentDB tárolt adatok alapján, és végül tárolja az adatokat az Azure tárterületet használ a Hadoop fájl rendszer (hdfs) lehetőségre.

![HDInsight és a dolog Internet (IoT) architektúra diagram](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>– Áttekintés

Érzékelők hozzáadása járműveket lehetővé teszi előrejelzésére berendezések problémák korábbi trendeket alapján, valamint a jövőbeli verzióiban használatát mintát elemzés alapján javítása. Az elemzés használható hagyományos MapReduce parancsfájl, miközben kell gyorsan és hatékonyan betöltése az adatokat az összes járműveket Hadoop akkor fordulhat elő, MapReduce feldolgozása előtt. Ezenkívül érdemes hiba kritikus út (motor hőmérsékleti, fékek stb.) valós idejű elemzéseket végezni.

Azure esemény hubok kialakításának a nagy mennyiségű érzékelők által generált adatok kezelése, és a HDInsight Apache vihar MapReduce további feldolgozásra használható betöltése és az adatok feldolgozása előtt be (Azure tároló biztonsági) Fájlrendszerhez tárolja.

##<a name="solution"></a>Megoldás

Telemetriai adatokat nyújt a motor hőmérsékleti környezeti hőmérséklet és gépjármű sebesség rögzítését érzékelők, majd küldött esemény hubok a autós gépjármű azonosító szám (VIN) és az idő szerint. Itt egy vihar topológia egy Apache vihar HDInsight fürt futó beolvassa az adatokat, folyamatok, és tárolja a Fájlrendszerhez be.

A feldolgozás során a VIN használt modell adatainak lekérése az Azure DocumentDB. A adatfolyam ez lett hozzáadva, mielőtt a rendszer tárolja.

A vihar topológiában használt elemeket a következők:

* **EventHubSpout** - Azure esemény hubok adatokat olvas be

* **TypeConversionBolt** – alakítja a JSON-karakterlánc az esemény hubok motor hőmérsékleti, környezeti hőmérséklet, sebessége, VIN és időbélyeg adatok egyedi értékeket tartalmazó rekordhoz

* **DataReferencBolt** – a használatával a VIN DocumentDB keres a gépjármű modell

* **WasbStoreBolt** - tárolja az adatokat (Azure-tároló) Fájlrendszerhez

Az alábbiakban a megoldás folyamatábrája látható:

![vihar topológiája](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Ez az egyszerűsített diagram, és előfordulhat, hogy a megoldás minden összetevő több példányon. Például a topológiában minden összetevő több példányát a vihar HDInsight fürt csomópontját elosztva vannak.

##<a name="implementation"></a>Végrehajtása

Egy olyan automatikus megoldás ebben az esetben áll rendelkezésre a [HDInsight-vihar – példák](https://github.com/hdinsight/hdinsight-storm-examples) tárházából GitHub részeként. Ez a példa használatához kövesse a a [IoTExample – fontos fájl. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Következő lépések

További példa vihar topológiák [például topológiák vihar a HDInsight-](hdinsight-storm-example-topology.md)című témakör tartalmaz.
