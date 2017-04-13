<properties
    pageTitle="Első lépések a C és Apache vihar esemény hubok |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből Azure esemény csomópontok, használatának első lépései események c válna őket egy Apache vihar fürthöz a."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok rendszer erősen méretezhető bevitel, több millió bevitelt is események másodpercenként, folyamat és elemzése a nagy mennyiségű adatot-alkalmazás engedélyezése készített a csatlakoztatott eszközök és az alkalmazások. Esemény hubra gyűjtött, miután átalakítás pontra, és tárolja az adatokat bármely valós idejű analytics-szolgáltatója vagy a tárhely fürt segítségével.

További tudnivalókért lásd a [esemény hubok – áttekintés].

Ebből az oktatóanyagból megtanulhatja, miként üzenetek ingest konzol alkalmazást használ, a C esemény-elosztóhoz, illetve Apache vihar használatával párhuzamosan lekérdezni.

Oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ A C fejlesztői környezet. Ebben az oktatóanyagban feltételezzük a ÖET Papírhalom az [Azure Linux virtuális](../virtual-machines/virtual-machines-linux-quick-create-cli.md) Ubuntu 14.04, a program. Más környezetekkel utasításokat fog kapni a külső hivatkozások.

+ [Maven tesztelése](http://maven.apache.org/)futtatására beállított Java fejlesztői környezet. Ebben az oktatóanyagban feltételezzük [Holdas](https://www.eclipse.org/)lesz.

+ Azure active fiók. Nem rendelkeznek fiókkal, ha csak néhány perc egy ingyenes fiókot is létrehozhat. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A **LogTopology** osztály lebonyolítása Holdas, majd megvárja, amíg az összes partíciót a vevők kezdődik.

2.  Futtassa a **Feladó** programot, de az események jelenjenek meg a címzett ablakban láthatók.

    ![][23]

> [AZURE.NOTE] Ebben az oktatóanyagban csak használata vihar helyi módban fejlesztési célra. Olvassa el a [HDInsight vihar – áttekintés] és hivatalos [Apache vihar] dokumentációjában találhat további információt a vihar telepítések és mintázatok.

## <a name="next-steps"></a>Következő lépések

Esemény hubok és vihar integrálása alkalmazások fejlesztésével a következő forrásokból érhetők el.

- Egy teljes forgatókönyv oktatóprogram esemény hubokba, vihar és HBase használatával Hadoop fürt szenzoradatokat ingest [szenzoradatokat vihar és HDInsight elemzése][] .
- [A folyamatos átvitelű adatfeldolgozás SCP.NET a és C# vihar és HDInsight-alkalmazások fejlesztése][] a hatékony szövegalkotás használatával C# vihar folyamatok oktatóanyagot.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Esemény hubok – áttekintés]: event-hubs-overview.md

[Apache vihar]: https://storm.incubator.apache.org
[HDInsight vihar – áttekintés]: ../hdinsight/hdinsight-storm-overview.md/
[Vihar és HDInsight érzékelő adatok elemzése]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[A folyamatos átvitelű adatfeldolgozás SCP.NET a és C# vihar és HDInsight-alkalmazások fejlesztése]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
