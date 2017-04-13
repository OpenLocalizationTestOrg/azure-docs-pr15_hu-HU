<properties
    pageTitle="Első lépések az esemény hubok C# |} Microsoft Azure"
    description="Kövesse az ebben az oktatóanyagban Ismerkedés az Azure esemény hubok és C# és a EventProcessorHost használatával."
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Esemény hubok – első lépések

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>– Bevezetés

Esemény hubok szolgáltatás, amely a folyamatok nagy mennyiségű eseményadatok (telemetriai) a csatlakoztatott eszközök és alkalmazások. Adatgyűjtés esemény hubra, miután tárolja az adatokat tároló fürt használatával, vagy átalakításhoz valós idejű analytics-szolgáltató használata. A nagy rendezvény összegyűjtése és feldolgozása képesség modern alkalmazás architektúrákban, beleértve az dolgot Internet (IoT) fő része.

Ebből az oktatóanyagból megtudhatja, hogy miként az Azure klasszikus portal segítségével hozzon létre egy esemény hubhoz. Azt is megtudhatja, hogyan gyűjthetők össze az üzenetek egy esemény elosztóhoz írt C# konzol alkalmazást használ, és hogyan beolvasni a párhuzamos használata a C# [Esemény processzor Host][] tárat.

Oktatóprogram elvégzéséhez a következőkre lesz szüksége:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Azure active fiók. Ha nincs telepítve egyik, létrehozhat egy ingyenes fiókra, mindössze néhány perc az. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. Visual Studio, nyissa meg a korábban létrehozott **címzett** projektet.

2. Kattintson a jobb gombbal a **címzett** megoldást, majd kattintson a **Hozzáadás**gombra, és válassza a **Meglévő projekt**.
 
3. Keresse meg a meglévő Sender.csproj fájlt, majd kattintson duplán kattintva adhatja azt hozzá a megoldáshoz.
 
4. Ismét kattintson a jobb gombbal a **címzett** megoldást, és válassza a **Tulajdonságok parancsot**. A **címzett** tulajdonságlapja jelenik meg.

5. Kattintson a **Projekt indítása**gombra, majd kattintson a **több indítási projekt** gombra. Állítsa a **művelet** mezőbe a **címzett** és a **Feladó** projektekhez **indításához**.

    ![][19]

6. Kattintson a **Projekt függőségeket**. A **projektek** mezőben kattintson a **Feladó**gombra. A **Jelszófüggő** párbeszédpanelen győződjön meg arról, **címzett** be van jelölve.

    ![][20]

7. Kattintson **az OK gombra** kattintva zárja be az a **tulajdonságokat** tartalmazó párbeszédpanel.

1.  Nyomja le az F5 futtassa a **címzett** projektnek a Visual Studióban, és várja meg, hogy az összes partíciót a vevők indítása.

    ![][21]

2.  A **Feladó** a project automatikusan fog futni. Nyomja le az **ENTER billentyűt** a konzol ablakban, és az események jelenjenek meg a címzett ablakban látható.

    ![][22]

Nyomja le a **Ctrl + C billentyűkombinációt** a **Feladó** ablakban befejezheti a feladó alkalmazást, majd nyomja le az **ENTER billentyűt** a címzett ablakban állítsa le az alkalmazást.

## <a name="next-steps"></a>Következő lépések

Most, hogy már készített korábban a munkaidő-alkalmazások, hogy létrehoz egy eseményt hubhoz és adatokat küld és fogad, áthelyezhetők a következő esetekben:

- Egy teljes [minta alkalmazást használó esemény hubok][].
- A [Méretezés meg az esemény hubok feldolgozása esemény][] minta.
- [Esemény hubok – áttekintés][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Esemény processzor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Esemény feldolgozása az esemény hubok méretezése]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
