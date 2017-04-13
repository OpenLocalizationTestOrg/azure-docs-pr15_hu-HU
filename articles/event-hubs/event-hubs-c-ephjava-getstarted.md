<properties
    pageTitle="Első lépések az esemény hubok c |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből Azure esemény csomópontok, használatának első lépései események c válna őket az esemény processzor állomás használatával Java-ban."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok rendszer erősen méretezhető bevitel, több millió bevitelt is események másodpercenként, folyamat és elemzése a nagy mennyiségű adatot-alkalmazás engedélyezése készített a csatlakoztatott eszközök és az alkalmazások. Esemény hubra gyűjtött, miután átalakítás pontra, és tárolja az adatokat bármely valós idejű analytics-szolgáltatója vagy a tárhely fürt segítségével.

További információt talál az [esemény hubok áttekintése][].

Ebből az oktatóanyagból megtanulhatja, hogyan üzenetek ingest egy esemény elosztóhoz konzol alkalmazást használ, a C és a párhuzamos használatával a C# [Esemény processzor Host][] tár lekérdezni.

Annak érdekében, hogy az oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ A C fejlesztői környezet. Ebben az oktatóanyagban feltételezzük a ÖET Papírhalom az [Azure Linux virtuális](../virtual-machines/virtual-machines-linux-quick-create-cli.md) Ubuntu 14.04, a program. Más környezetekkel utasításokat fog kapni a külső hivatkozások.

+ A Windows Microsoft Visual Studio Express

+ Azure active fiók. <br/>Nem rendelkeznek fiókkal, ha csak néhány perc egy ingyenes fiókot is létrehozhat. A részletekért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure ingyenes próbaverziót</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A **címzett** projekt futtatásához.

    ![][21]

2.  Futtassa a **Feladó** programot, és figyelje meg, az események jelenjenek meg a címzett ablakban.

    ![][24]

## <a name="next-steps"></a>Következő lépések

Most, hogy már készített korábban a munkaidő-alkalmazások, hogy létrehoz egy eseményt hubhoz és adatokat küld és fogad, áthelyezhetők a következő esetekben:

- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.
- [Esemény hubok – áttekintés][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephjava-getstarted/ephjava.png
[24]: ./media/event-hubs-c-ephjava-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Esemény processzor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
