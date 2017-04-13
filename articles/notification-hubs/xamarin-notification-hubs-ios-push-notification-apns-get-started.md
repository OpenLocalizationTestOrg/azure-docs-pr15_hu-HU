<properties
    pageTitle="iOS értesítés hubok a leküldéses értesítések Xamarin alkalmazásai |} Microsoft Azure"
    description="Ebből az oktatóanyagból megismerheti, hogyan Azure értesítés hubok segítségével küldje el a leküldéses értesítések Xamarin iOS-alkalmazás."
    services="notification-hubs"
    keywords="iOS leküldéses értesítéseket, az üzenetek leküldéses értesítést, leküldéses üzenet"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS értesítés hubok a leküldéses értesítések Xamarin alkalmazásai

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés
> [AZURE.IMPORTANT] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Ebben az oktatóanyagban bemutatja, hogyan Azure értesítés hubok segítségével küldje el a leküldéses értesítések iOS az alkalmazás.
Kell létrehoznia egy üres Xamarin.iOS alkalmazást, hogy a leküldéses értesítések fogadása az [Apple leküldéses értesítéseket kezelő szolgáltatása (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)használatával. Ha végzett, is használhatja az értesítési központi szétküldése fut, az alkalmazás az összes eszközök leküldéses értesítéseket. A kész kódot érhető el az [alkalmazás NotificationHubs] [ GitHub] minta.

Ebben az oktatóprogramban az egyszerű leküldéses üzenet közvetítési eset: a értesítés hubok mutatja be.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban az alábbiakra van szükség:

+ [Xcode 6.0 alkalmazásban][Install Xcode]
+ IOS 7.0-s (vagy újabb verzió) kompatibilis eszköz
+ iOS Fejlesztőeszközök Program tagság
+ [Xamarin Studio]

   > [AZURE.NOTE] Konfigurációs követelmények miatt for iOS leküldéses értesítéseket, meg kell üzembe helyezéséhez és tesztelje a fizikai iOS-eszközön (iPhone vagy iPad) a minta alkalmazás helyett a simulatort használja.

Ebben az oktatóanyagban befejezése rendszer minden más értesítés hubok oktatóanyagok az iOS-alkalmazások Xamarin.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Az értesítés központi konfigurálása

Ez a szakasz részletes információt tartalmaz egy új értesítés hubhoz létrehozása és konfigurálása a hitelesítést az Ön által létrehozott **.p12** leküldéses tanúsítvány használatával APN keresztül. Ha már létrehozott egy értesítés központi használni kívánt, kihagyhatja az 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Az APN kapcsolat beállítása a az Azure-portálon szeretnénk, nyissa meg a központi értesítési beállítások, ande <b>Értesítési szolgáltatások</b>parancsára, és válassza az <b>Apple (APNS)</b> elemet a listában. Kész, kattintson a <b>Tanúsítvány tölthet fel</b> , és válassza ki a <b>.p12</b> tanúsítványt, és a jelszót a tanúsítvány a korábban exportált.</p>
<p>Győződjön meg róla, hogy <b>a védőfalas</b> üzemmód óta küldi leküldéses üzenetek fejlesztői környezetben. Csak akkor használja a <b>gyártási</b> beállítást, ha a felhasználók, akik már vásárolt az alkalmazás a áruházból leküldéses értesítéseket küldeni szeretné.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Az értesítés központi most úgy van beállítva az APN való használatra, és a kapcsolat karakterláncok regisztrálhat az alkalmazást, és küldje el a leküldéses értesítések van.


##<a name="connect-your-app-to-the-notification-hub"></a>Az alkalmazás csatlakoztatása az értesítési-központban

#### <a name="create-a-new-project"></a>Új projekt létrehozása

1. Xamarin Studio, iOS új projekt létrehozása, és válassza ki az **Egyesített API** > **Egyetlen megjelenítő** sablont.

    ![Xamarin Studio - választó alkalmazás típusa][31]

2. Az Azure Messaging összetevőt mutató hivatkozás hozzáadása. A megoldás nézetben kattintson a jobb gombbal a **összetevők** mappa beállítása a projekthez, és válassza a **További összetevők első**. Keresse meg az **Azure Messaging** összetevőt, és az összetevő hozzáadása a projekthez.

3. Az **AppDelegate.cs**, adja meg a következő utasítással:

        using WindowsAzure.Messaging;

4. Egy példányának **SBNotificationHub**deklarálhatnak:

        private SBNotificationHub Hub { get; set; }

5. Hozzon létre egy **Constants.cs** osztály a következő változók:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. A **AppDelegate.cs**frissítse a **FinishedLaunching()** megfelelően a következőket:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. A **RegisteredForRemoteNotifications()** módszer **AppDelegate.cs**felülbírálása:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. A **ReceivedRemoteNotification()** módszer **AppDelegate.cs**felülbírálása:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. A következő **ProcessNotification()** módszer **AppDelegate.cs**létrehozása:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Megadhatja, hogy felülbírálása **FailedToRegisterForRemoteNotifications()** azokról a helyzetekről, például nincs hálózati kapcsolat kezelése. Ez akkor különösen fontos, ahol a felhasználó a előfordulhat, hogy az alkalmazás indítsa el a kapcsolat nélküli módban (pl. repülőgép), és szeretné kezelni az alkalmazás-specifikus esetek üzenetküldés leküldéses.


10. Futtassa az alkalmazást az eszközön.


## <a name="sending-push-notifications"></a>Leküldéses értesítések küldése


Ellenőrizheti, hogy érkeznek be a leküldéses értesítések az alkalmazás értesítést küld az [Azure-portálon] keresztül a a **Hibaelhárítás** toolset **Tesztelése küldése** lehetőséget, a jobb oldalon az értesítési központi lapon az alábbi képernyőképen látható módon.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Leküldéses értesítések általában küldjük például a mobil szolgáltatások vagy ASP.NET-kompatibilis tár használatával háttéradatbázist szolgáltatáson keresztül. A REST API közvetlenül az leküldéses küldésére, ha egy tárban nem érhető el az Ön esetében is használhatja. 

Ebben az oktatóanyagban azt fogja legyen egyszerű, és csak bemutatják, az ügyfél-alkalmazás teszteli a .NET SDK használata helyett kódmentes szolgáltatás konzol alkalmazásban értesítés hubok értesítést küld. A következő lépés az értesítéseket küldeni egy ASP.NET kódmentes javasoljuk az [Használata értesítés hubok leküldéses értesítéseket, hogy a felhasználók az](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóprogram. Az alábbi módszerekkel azonban küldött értesítések használható:

* **Többi Interface**: bármely, a [többi felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)kódmentes platformon is támogatja a leküldéses értesítést.

* **Microsoft Azure értesítés hubok .NET SDK**: A a Nuget csomag Manager for Visual Studio, futtassa a [Telepítés-csomag Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [Node.js az értesítési hubok használatáról](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile Appjai**:, hogyan küldhet értesítést a az Azure alkalmazás szolgáltatás Mobile-alkalmazások kódmentes értesítés hubok integrálva van, például egy [értesítés](../app-service-mobile/app-service-mobile-ios-get-started-push.md)jelenik meg, Hozzáadás leküldéses a mobil alkalmazásba.

* **Java / PHP**: Példa leküldéses értesítéseket küldeni a REST API-k segítségével, témakö "értesítés hubok a Java/PHP használata" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Nem kötelező) Küldje el a leküldéses értesítések .NET konzol alkalmazásból

Ebben a részben egy egyszerű .NET konzol alkalmazás használatával leküldéses értesítéseket elküldi azt. Ez a példa alkalmazásában azt váltson fog egy Windows fejlesztői környezet, amely tartalmaz Visual Studio már telepítve van.

1. A Visual Studióban hozzon létre egy új vizuális C# konzol alkalmazást:

    ![Visual Studio - konzol új alkalmazás létrehozása][213]

2. A Visual Studióban kattintson az **eszközök** **NuGet csomag Manager**kattintson, és válassza a **Csomag kezelője konzol**.

    A csomag kezelője konzol alján a Visual Studio munkaterület rögzített jelenjenek meg.

3. Csomag kezelője konzol ablakában a **projekt alapértelmezett** beállítása a konzol alkalmazás új projekt, és majd konzol ablakában végre az alábbi parancsot:

        Install-Package Microsoft.Azure.NotificationHubs

    Ez a lehetőség a <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification hubok NuGet csomag</a>használata Azure-értesítés hubok SDK mutató hivatkozás.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Nyissa meg a `Program.cs` fájlt, és adja hozzá a következő `using` utasítás, biztosítva, hogy ábrázolásakor Azure osztályok és a fő osztály belül funkciók:

        using Microsoft.Azure.NotificationHubs;

3. Az a `Program` osztály, és adja meg a következő módszerrel (ne felejtse el cserélje le a **kapcsolati karakterlánc** és a **központi neve**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Adja hozzá a következő sorokat a `Main` módszer:

         SendNotificationAsync();
         Console.ReadLine();

5. Nyomja le az F5 billentyűvel futtassa az alkalmazást. Másodpercek lásd: a leküldéses értesítést jelennek meg az eszközön. Wi-Fi vagy mobilinternet-hálózathoz használja, hogy győződjön meg arról, hogy az eszközön aktív internetkapcsolatra van-e.

A lehetséges tartalmak számára az Apple [helyi és a leküldéses értesítést programozási útmutató]talál.

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Nem kötelező) Értesítés küldése egy mobilszolgáltatás

Ebben a részben keresztül csomópont parancsfájl mobil szolgáltatást használ, a leküldéses értesítéseket küldeni fogunk.

Értesítés küldése a mobilszolgáltatás használatával, hajtsa végre az [első lépések a mobil szolgáltatások], majd:

1. Jelentkezzen be a [Klasszikus Azure-portálra], és jelölje ki a mobil szolgáltatást.

2. Jelölje ki a **Feladatütemező** fülre a képernyő tetején.

    ![Azure klasszikus portál - ütemező][215]

3. Hozzon létre egy új ütemezett feladat, helyezze be a nevét, és jelölje ki **az igény**.

    ![Azure klasszikus portál – új feladat létrehozása][216]

4. Ha a feladat jön létre, kattintson a projekt nevére. Kattintson a felső sávon a **parancsfájl** -fülre.

5. Szúrjon be a következőt a Feladatütemező függvény belül. Ellenőrizze, hogy a értesítés központi neve és a kapcsolati karakterlánc a helyőrzők cseréje *DefaultFullSharedAccessSignature* korábbi szerezte be. Kattintson a **Mentés**gombra.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Kattintson **Egyszer futtassa** az alsó sávján. Meg kell értesítést kapni, az eszközön.

##<a name="next-steps"></a>Következő lépések

Egyszerű ebben a példában akkor az iOS-eszközökhöz leküldéses értesítéseket rendelkezésére bocsát. Annak érdekében, hogy a célként megadott felhasználók, olvassa el az oktatóprogram [Használata értesítés hubok a felhasználóknak a leküldéses értesítéseket]. Oszthatja fel a felhasználók kamat csoportok szerint szeretné, ha erről [Használata értesítés hubok küldése legfrissebb híreket]. További tájékoztatást értesítés hubok [Értesítés hubok útmutatást] és az [Értesítési hubok ennek az iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Mobil szolgáltatások – első lépések]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure klasszikus portál]: https://manage.windowsazure.com/
[Értesítés hubok útmutató]: http://msdn.microsoft.com/library/jj927170.aspx
[IOS ennek hubok értesítés]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Értesítés hubok segítségével felhasználók a leküldéses értesítések]: /manage/services/notification-hubs/notify-users-aspnet
[Legfrissebb hírek küldése értesítés hubok használatával]: /manage/services/notification-hubs/breaking-news-dotnet

[Helyi és a leküldéses értesítést programozás útmutató]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure portál]: https://portal.azure.com
