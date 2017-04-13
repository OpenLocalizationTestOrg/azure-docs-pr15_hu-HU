<properties
    pageTitle="Első lépések az esemény hubok Apache vihar a Java-ban |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből Azure esemény csomópontok, használatának első lépései Java események válna őket egy Apache vihar fürt."
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok rendszer erősen méretezhető bevitel, több millió bevitelt is események másodpercenként, folyamat és elemzése a nagy mennyiségű adatot-alkalmazás engedélyezése készített a csatlakoztatott eszközök és az alkalmazások. Esemény hubra gyűjtött, miután átalakítás pontra, és tárolja az adatokat bármely valós idejű analytics-szolgáltatója vagy a tárhely fürt segítségével.

További tudnivalókért lásd: az [Esemény hubok áttekintése][].

Ebből az oktatóanyagból megtudhatja, hogy miként Java egy konzol alkalmazással esemény-elosztóhoz üzenetek összegyűjtéséhez és Apache vihar használatával párhuzamosan lekérdezni.

Oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ [Maven tesztelése](http://maven.apache.org/)futtatására beállított Java fejlesztői környezet. Az ebben az oktatóanyagban feltételezzük [Holdas](https://www.eclipse.org/).

+ Azure active fiók. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A **LogTopology** osztály lebonyolítása Holdas, majd megvárja, amíg az összes partíciót a vevők kezdődik.

2.  A **Feladó** projekt futtatásához, és nyomja le az **ENTER billentyűt** a konzol ablakban az események jelenjenek meg a címzett ablakban látható.

    ![][22]

> [AZURE.NOTE] Ebben az oktatóanyagban csak használata vihar helyi módban fejlesztési célra. A [HDInsight vihar – áttekintés][] és hivatalos [Apache vihar][] dokumentációjában találhat további információt a vihar telepítések és mintázatok témakörben talál.

## <a name="next-steps"></a>Következő lépések

Esemény hubok és vihar integrálása alkalmazások fejlesztésével a következő forrásokból érhetők el.

- Egy teljes forgatókönyv oktatóprogram esemény hubokba, vihar és HBase használatával Hadoop fürt szenzoradatokat ingest [szenzoradatokat vihar és HDInsight elemzése] .
- [A folyamatos átvitelű adatfeldolgozás SCP.NET a és C# vihar és HDInsight-alkalmazások fejlesztése][] a hatékony szövegalkotás használatával C# vihar folyamatok oktatóanyagot.

<!-- Images. -->
[22]: ./media/event-hubs-java-storm-getstarted/receive-storm2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Esemény hubok – áttekintés]: event-hubs-overview.md

[Apache vihar]: https://storm.incubator.apache.org
[HDInsight vihar – áttekintés]: ../hdinsight/hdinsight-storm-overview.md
[Vihar és HDInsight érzékelő adatok elemzése]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[A folyamatos átvitelű adatfeldolgozás SCP.NET a és C# vihar és HDInsight-alkalmazások fejlesztése]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
 