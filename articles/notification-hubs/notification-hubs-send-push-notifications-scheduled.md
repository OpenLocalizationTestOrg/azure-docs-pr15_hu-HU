<properties
    pageTitle="Ütemezett értesítések üzenetküldés |} Microsoft Azure"
    description="Ez a témakör ismerteti az Azure értesítés hubok ütemezett értesítések használata."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="leküldéses értesítések, a leküldéses értesítést, ütemezés a leküldéses értesítések"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Útmutató: Ütemezett értesítések küldésére


##<a name="overview"></a>– Áttekintés

Ha van olyan eset, ahol szeretné valamikor a jövőben értesítés küldése, de ugyanígy felébresztése nem rendelkezik az értesítés küldése a háttéradatbázist kód. Szabványos réteg értesítés hubok támogatja a szolgáltatása, amely lehetővé teszi, hogy az ütemezést értesítések be, hogy a 7 nappal későbbi.

Ha értesítést küld, egyszerűen hajtsa végre a [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) osztály a értesítés hubok SDK csomagjában talál a következő példában látható módon:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Is használja a notificationId korábban ütemezett értesítést lemondhatja:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

A megadott elküldheti ütemezett értesítések nincs korlátozás van érvényben.