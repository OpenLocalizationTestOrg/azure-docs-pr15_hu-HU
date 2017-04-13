<properties
    pageTitle="Első lépések az esemény hubok Java-ban |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből Azure esemény csomópontok, használatának első lépései Java események válna őket a C# a EventProcessorHost használatával."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok rendszer erősen méretezhető bevitel, több millió bevitelt is események másodpercenként, folyamat és elemzése a nagy mennyiségű adatot-alkalmazás engedélyezése készített a csatlakoztatott eszközök és az alkalmazások. Esemény hubra gyűjtött, miután átalakítás pontra, és tárolja az adatokat bármely valós idejű analytics-szolgáltatója vagy a tárhely fürt segítségével.

További tudnivalókért lásd: az [esemény hubok áttekintése][].

Ebből az oktatóanyagból megtudhatja, hogy miként, üzenetek ingest Java egy konzol alkalmazással esemény-elosztóhoz, és a párhuzamos használatával a C# [Esemény processzor Host][] tár lekérdezni.

Ebben az oktatóanyagban befejezéséhez a következőkre lesz szüksége:

+ Java-fejlesztői környezet. Ebben az oktatóanyagban feltételezzük [Holdas](https://www.eclipse.org/)lesz.

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Azure active fiók. <br/>Nem rendelkeznek fiókkal, ha csak néhány perc egy ingyenes fiókot is létrehozhat. A részletekért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure ingyenes próbaverziót</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A **címzett** projekt lebonyolítása Visual Studio, és várja meg, hogy az összes partíciót a vevők indítása gombra.

    ![][21]

2.  A **Feladó** projekt futtatásához.

    ![][22]

## <a name="next-steps"></a>Következő lépések

Most, hogy már készített korábban a munkaidő-alkalmazások, hogy létrehoz egy eseményt hubhoz és adatokat küld és fogad, áthelyezhetők a következő esetekben:

- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.

További tudnivalókért lásd: a [Java Developer Center](/develop/java/).

<!-- Images. -->
[21]: ./media/event-hubs-java-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-java-ephcs-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Esemény processzor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 