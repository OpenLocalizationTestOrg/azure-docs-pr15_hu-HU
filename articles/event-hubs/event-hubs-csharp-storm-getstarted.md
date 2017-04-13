<properties
    pageTitle="Első lépések a C# Apache vihar az esemény hubok |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből Azure esemény csomópontok, használatának első lépései a C# és érkeznek be őket egy Apache vihar fürthöz az eseményeket küldött."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok rendszer erősen méretezhető bevitel, több millió bevitelt is események másodpercenként, folyamat és elemzése a nagy mennyiségű adatot-alkalmazás engedélyezése készített a csatlakoztatott eszközök és az alkalmazások. Esemény hubra gyűjtött, miután átalakítás pontra, és tárolja az adatokat bármely valós idejű analytics-szolgáltatója vagy a tárhely fürt segítségével.

További információt talál az [esemény hubok áttekintése].

Ebből az oktatóanyagból megtanulhatja, miként üzenetek ingest konzol alkalmazást használ, a C# esemény-elosztóhoz, illetve Apache vihar használatával párhuzamosan lekérdezni.

Annak érdekében, hogy az oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ [Maven tesztelése](http://maven.apache.org/)futtatására beállított Java fejlesztői környezet. Az ebben az oktatóanyagban feltételezzük fog [Holdas](https://www.eclipse.org/)

+ Azure active fiók. <br/>Nem rendelkeznek fiókkal, ha csak néhány perc egy ingyenes fiókot is létrehozhat. A részletekért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure ingyenes próbaverziót</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A **LogTopology** osztály lebonyolítása Holdas, majd megvárja, amíg az összes partíciót a vevők kezdődik.

2.  A **Feladó** projekt futtatásához, és nyomja le az **ENTER billentyűt** a konzol ablakban az események jelenjenek meg a címzett ablakban látható.

    ![][22]

## <a name="next-steps"></a>Következő lépések

Most, hogy már készített korábban a munkaidő-alkalmazások, hogy létrehoz egy eseményt hubhoz és adatokat küld és fogad, áthelyezhetők a következő esetekben:

- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 