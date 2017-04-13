<properties
    pageTitle="Platformok értesítés küldése a felhasználóknak az értesítési hubok (ASP.NET)"
    description="Ismerje meg, hogy egyetlen kérelem, az összes platformokon függvénynél platform-felismerése nélkül értesítést küldhet értesítést hubok-sablonok használatáról."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Értesítés hubok rendelkező felhasználók platformok értesítések küldése


Az előző oktatóanyagban [értesítés hubok felhasználók értesítése]szüksége a tanultakhoz módjáról a leküldéses értesítések regisztrált egy bizonyos hitelesített felhasználó összes eszközöket. Adott oktatóanyagból több kérések kellett egy értesítést küld a támogatott ügyfél platformokon. Értesítés hubok támogatja a sablonok, amelyekkel adja meg, hogyan egy adott eszköz azt szeretné, ha értesítést szeretne kapni. Ez egyszerűbbé teszi-platformok értesítést küld. Ez a témakör bemutatja, hogyan lehet egyetlen kérelem, az összes platformokon függvénynél platform-felismerése nélkül értesítést küldhet sablonok előnyeit. Részletes információt a sablonok, [Áttekintése]című témakörben találhat Azure értesítés hubok[Templates].

> [AZURE.NOTE] Értesítés hubok lehetővé teszi, hogy több sablonok regisztrálása az adott címkét szeretne egy eszközt. Egy bejövő üzenet, amely a címke kiválasztásával ebben az esetben az eszközt, egy, az egyes sablonok kézbesítve több értesítések eredményez. Lehetővé teszi, ugyanezt a hibaüzenetet jeleníthet meg több vizuális értesítések, például mindkét jelvény és a Windows áruházból letölthető bejelentési értesítés.

Sablonok használata platformok értesítés küldése az alábbi lépések elvégzésével:

1. A Visual Studio megoldás Explorer bontsa ki a **vezérlők** mappát, majd nyissa meg a RegisterController.cs fájlt.

2. Keresse meg a kód Tiltás hoz létre egy új regisztrációs csere **bejegyzés** módszert a `switch` tartalom és a a következő kódot:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Ez a kód helyett a regisztráció natív regisztráció sablon létrehozása a platform-specifikus módszer hívások. Meglévő regisztrációk nem kell módosítani, mert a sablon regisztrációk natív regisztrációk származhat.

3. Az **értesítések** vezérlő cserélje ki a **sendNotification** módszer a következő kódot:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Ez a kód értesítést küld az összes platformokon egyszerre, és adja meg a natív tartalom nélkül. Értesítés elosztók hoz létre, és a megfelelő tartalom biztosítja a megadott _címke_ értéket az ajánlott sablonok meghatározott minden eszközre.

4. Tegye közzé újra a WebApi háttéradatbázist projekt.

5. Futtassa újra az ügyfél-alkalmazást, és ellenőrizze, hogy tud-e a regisztráció.

6. (Nem kötelező) Az ügyfél-alkalmazások terjesztése egy másik eszközre, és futtassa az alkalmazást.

    Figyelje meg, hogy látható-e értesítést az egyes.

## <a name="next-steps"></a>Következő lépések

Most, hogy ebben az oktatóanyagban befejezése részletesebben értesítés hubok és sablonok a következő témaköröket:

+ **[Legfrissebb hírek küldése értesítés hubok használatával]** <br/>Bemutatja, sablonok használata másik forgatókönyve

+  **[Azure értesítés hubok – áttekintés][Templates]**<br/>Van – áttekintés részletesebb információt a sablonok.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Legfrissebb hírek küldése értesítés hubok használatával]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Az értesítés hubok felhasználók értesítése]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
