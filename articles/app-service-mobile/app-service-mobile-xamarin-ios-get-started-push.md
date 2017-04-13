<properties
    pageTitle="Leküldéses értesítések hozzáadása a Xamarin.iOS alkalmazás Azure alkalmazás szolgáltatással"
    description="Azure alkalmazás szolgáltatás használata a leküldéses értesítéseket küldeni az Xamarin.iOS alkalmazás"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Leküldéses értesítések felvétele a Xamarin.iOS alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban ad hozzá leküldéses értesítéseket a [Xamarin.iOS gyors indítása](app-service-mobile-xamarin-ios-get-started.md) projekthez, hogy a leküldéses értesítést küld az eszköz minden alkalommal, amikor a program beszúrja a rekordot.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, szüksége lesz a leküldéses értesítést bővítmény csomag. [Munka a .NET kódmentes kiszolgálóval Azure Mobile-appokról SDK csomagjában talál](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt talál.

##<a name="prerequisites"></a>Előfeltételek

* Az [Xamarin.iOS quickstart útmutató](app-service-mobile-xamarin-ios-get-started.md) oktatóprogram elvégzéséhez.

* A fizikai iOS-eszközökön. Leküldéses értesítések az iOS simulatort használja által nem támogatott.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Regisztráljon az alkalmazást az Apple developer Portal segítségével a leküldéses értesítések

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>A leküldéses értesítések küldésére mobilalkalmazás beállítása

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Leküldéses értesítések küldésére a kiszolgáló projekt frissítése

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>A Xamarin.iOS projekt konfigurálása

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Leküldéses értesítések felvétele az alkalmazásba

1. Adja hozzá a következő tulajdonság **QSTodoService**, így **AppDelegate** szerezheti be a mobil ügyfélprogram:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Adja hozzá a következő `using` utasítás **AppDelegate.cs** fájl tetején.

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. A **AppDelegate**felülbírálása az **FinishedLaunching** esemény:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Ugyanannak a fájlnak a felülbírálása az **RegisteredForRemoteNotifications** eseményt. A kód regisztrál egy egyszerű sablon értesítést, amely a kiszolgáló által támogatott platformon elküldjük.

    Értesítés hubok sablonokat kapcsolatos további tudnivalókért olvassa el a [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)című témakört.


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Ezután felülbírálása az **DidReceivedRemoteNotification** esemény:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Az alkalmazás letöltése frissül a leküldéses értesítések támogatása.

## <a name="test"></a>Teszt leküldéses értesítéseket, az alkalmazásban

1. A **Futtatás** gombra kattintva hozza létre a projekt, és indítsa el az alkalmazás képes iOS-eszközön, majd kattintson **az OK gombra** koppintva fogadja el a leküldéses értesítéseket.

    > [AZURE.NOTE] Az alkalmazás kifejezetten el kell fogadnia leküldéses értesítéseket. A kérés csak az első alkalommal a alkalmazást futtató fordul elő.

2. Abban az alkalmazásban, írjon be egy feladatot, és kattintson a plusz (**+**) ikonra.

3. Győződjön meg arról, hogy a értesítés érkezik, majd kattintson **az OK gombra** kattintva zárja be az értesítésre.

4. Ismételje meg a 2 azonnal bezárja az alkalmazást, majd ellenőrizze, hogy látható-e értesítést.

Ebben az oktatóanyagban sikeresen befejeződött.

<!-- Images. -->

<!-- URLs. -->



