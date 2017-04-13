<properties
    pageTitle="Első lépések az esemény hubok a C és C# |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből Azure esemény csomópontok, használatának első lépései események c válna alján szegély C# a EventProcessorHost használatával."
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
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok rendszer erősen méretezhető bevitel, amelyek bevitelt milliós események másodpercenként, folyamat és elemzése a nagy mennyiségű adatot-alkalmazás engedélyezése készített a csatlakoztatott eszközök és az alkalmazások. Esemény hubra gyűjtött, miután átalakítás pontra, és tárolja az adatokat bármely valós idejű analytics-szolgáltatója vagy a tárhely fürt segítségével.

További tudnivalókért lásd a [Esemény hubok – áttekintés][].

Ebből az oktatóanyagból megtanulhatja, hogyan üzenetek ingest egy esemény elosztóhoz konzol alkalmazást használ, a C és a párhuzamos használatával a C# [Esemény processzor Host][] tár lekérdezni.

Oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ A C fejlesztői környezet. Ebben az oktatóanyagban feltételezzük a ÖET Papírhalom az [Azure Linux virtuális](../virtual-machines/virtual-machines-linux-quick-create-cli.md) Ubuntu 14.04, a program. Más környezetekkel utasításokat fog kapni a külső hivatkozások.

+ A Windows Microsoft Visual Studio Express

+ Azure active fiók. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  Futtassa a **címzett** projektnek a Visual Studio, és várja meg, hogy az összes partíciót a vevők indítása.

    ![][21]

2.  Futtassa a **Feladó** programot, de az események jelenjenek meg a címzett ablakban láthatók.

    ![][24]

## <a name="next-steps"></a>Következő lépések

Most, hogy már készített korábban a munkaidő-alkalmazások, hogy létrehoz egy eseményt hubhoz és adatokat küld és fogad, áthelyezhetők a következő esetekben:

- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.
- [Esemény hubok – áttekintés][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Esemény processzor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
