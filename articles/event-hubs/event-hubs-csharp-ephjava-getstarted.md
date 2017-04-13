<properties
    pageTitle="Első lépések az esemény hubok C# |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből Azure esemény csomópontok, használatának első lépései események C# válna őket a EventProcessorHost használatával Java-ban."
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
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok szolgáltatás, amely a folyamatok nagy mennyiségű eseményadatok (telemetriai) a csatlakoztatott eszközök és alkalmazások. Adatgyűjtés esemény hubra, miután tárolja az adatokat tároló fürt használatával, vagy átalakításhoz valós idejű analytics-szolgáltató használata. A nagy rendezvény összegyűjtése és feldolgozása képesség modern alkalmazás architektúrákban, beleértve az dolgot Internet (IoT) fő része.

Ebből az oktatóanyagból megtudhatja, hogy miként az Azure klasszikus portal segítségével hozzon létre egy esemény hubhoz. Azt is megtudhatja, hogyan gyűjthetők össze az üzenetek egy esemény elosztóhoz írt C# konzol alkalmazást használ, és hogyan beolvasni a párhuzamos használata a Java esemény processzor Host tárat.

Oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Azure active fiók. <br/>Ha nincs telepítve egyik, létrehozhat egy ingyenes fiókra, mindössze néhány perc az. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A **címzett** projekt futtatásához.

    ![][21]

2.  A **Feladó** projekt futtatásához.

    ![][22]

## <a name="next-steps"></a>Következő lépések

Most, hogy már készített korábban a munkaidő-alkalmazások, hogy létrehoz egy eseményt hubhoz és adatokat küld és fogad, áthelyezhetők a következő esetekben:

- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.
- [Esemény hubok – áttekintés][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
