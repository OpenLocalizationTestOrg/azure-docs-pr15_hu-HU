<properties
    pageTitle="Leküldéses értesítések hozzáadása a Xamarin.Forms alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure szolgáltatások használatával több platformot leküldéses értesítéseket küldeni az Xamarin.Forms alkalmazásait."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Leküldéses értesítések felvétele a Xamarin.Forms alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban adja hozzá a leküldéses értesítések minden projekt, hogy a leküldéses értesítést küld az összes platformok ügyfelek minden alkalommal, amikor a program beszúrja a rekord [Xamarin.Forms gyors indítása](app-service-mobile-xamarin-forms-get-started.md) megszakításához.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, szüksége lesz a leküldéses értesítést bővítmény csomag. [Munka a .NET kódmentes kiszolgálóval Azure Mobile-appokról SDK csomagjában talál](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt talál.

##<a name="prerequisites"></a>Előfeltételek

* Az iOS, szüksége lesz az [Apple Fejlesztőeszközök Program tagság](https://developer.apple.com/programs/ios/) és a tényleges iOS-eszközön mivel [iOS simulatort használja nem támogatja a leküldéses értesítéseket](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Értesítés hubon konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Leküldéses értesítések küldésére a kiszolgáló projekt frissítése

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Nem kötelező) Állítsa be, és futtassa az Android-projekt

Töltse ki az Android-alapú a Xamarin.Forms Droid projekthez a leküldéses értesítések engedélyezése ebben a szakaszban.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Firebase felhő (FCM) üzenetküldés engedélyezése

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>A leküldéses kéréseket FCM használatával küldhet mobilalkalmazás kódmentes konfigurálása

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Leküldéses értesítések hozzáadása az Android-projekt

A háttér-kiszolgálói FCM konfigurált is hozzáadunk összetevők regisztrálni a FCM, az ügyfél-kódokat regisztrálhatja a mobilalkalmazásban kódmentes keresztül az Azure értesítés hubok leküldéses értesítéseket, és értesítéseket.

1. A **Droid** projektben kattintson a jobb gombbal a **összetevők** mappát, kattintson a **További összetevők az...**, a **Google Cloud üzenetküldés ügyfél** összetevő keresni, és adja hozzá a projekthez. Az összetevő Xamarin Android projekt támogatja a leküldéses értesítéseket.


2. Nyissa meg a MainActivity.cs projekt-fájlját, és adja hozzá a következő utasítással a fájl tetején:

        using Gcm.Client;

3. A **OnCreate** módszer a hívást **LoadApplication**után a következő kódot hozzáadása:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Egy új **CreateAndShowDialog** segítő mód hozzáadása a következőképpen:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. A következő kód hozzáadása a **MainActivity** osztály:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Ez az aktuális **MainActivity** példány közzététele, így azt a fő felhasználói felületi szálon hajthat végre.

6. Inicializálni a `instance`, az alábbiak szerint a **OnCreate** módszer az elején változó.

        // Set the current instance of MainActivity.
        instance = this;

2. Új osztály fájl hozzáadása a **Droid** projekt nevű `GcmService.cs`, és ellenőrizze, hogy a fájl tetején találhatók a következő kimutatások **használatával** :

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Adja hozzá a következő jogosultsági kérelmek a fájl tetején után a **használata** kimutatásokban és a **névtér** nyilatkozat előtt.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. A névtér adja hozzá a következő osztály-definíciót. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]**< PROJECT_NUMBER >** cserélje le a korábban feljegyzett projektszámot.   

11. Az üres **GcmService** osztály cserélje ki az alábbi kódot, amely használja az új közvetítési címzett:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. A következő kód hozzáadása a **GcmService** osztály, amely felülírja az **OnRegistered** eseménykezelő és **regisztrálhatja** a módszer hajtja végre.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Adja hozzá a következő kódot, amely **OnMessage**hajtja végre: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Ez a kezelje a bejövő értesítéseket, és küldje el nekik a értesítés kezelő megjeleníteni.

14. **GcmServiceBase** igényel, hogy végrehajtja a **OnUnRegistered** és **hibára** kezelő módszerek, amely az alábbiak szerint végezheti el:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Ettől kezdve készen áll a próba leküldéses értesítéseket, az alkalmazás az Android-eszközökre vagy a irányító rendszeren futó áll.

###<a name="test-push-notifications-in-your-android-app"></a>Az Android alkalmazásban a próba leküldéses értesítések

Az első két lépést szükség, csak kattintson egy irányító tesztelésekor.

1. Ellenőrizze, hogy bevezetéshez vagy egy virtuális eszközön, amelyen a Google API-hoz a célként beállítása az Android virtuális eszköz (AVD) manager alább látható módon hibakeresési vannak.

2. Google-fiók hozzáadása az Android-készülék **alkalmazások**kattintva > **Beállítások** > **fiók hozzáadása**, majd kövesse az útmutatást követve használata egy meglévő Google-fiókot hozzáadása az eszközre a hozzon létre egy újat.

1. A Visual Studio vagy Xamarin Studio kattintson a jobb gombbal a **Droid** projekt, és kattintson a **beállítás indítása projektként**.

2. A **Futtatás** gombra kattintva hozza létre a projekt, és indítsa el a alkalmazást Android-eszközzel vagy irányító.

3. Abban az alkalmazásban, írjon be egy feladatot, és kattintson a plusz (**+**) ikonra.

4. Győződjön meg arról, hogy értesítést kapott, elemek hozzáadásakor.


##<a name="optional-configure-and-run-the-ios-project"></a>(Nem kötelező) Állítsa be, és futtassa az iOS-projekt

Ez a szakasz nem fut az iOS-eszközökhöz Xamarin iOS projekt. Ez a szakasz átugorhatja, ha nem működik az iOS-eszközök.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Az értesítés-központban APN konfigurálása

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Ezután az iOS-projekt beállítása fog konfigurálni Xamarin Studio vagy Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Leküldéses értesítések hozzáadása az iOS-alkalmazás

1. Nyissa meg az **iOS** project AppDelegate.cs az alábbi **használatával** utasítás hozzáadása a kód fájl tetején.

        using Newtonsoft.Json.Linq;

4. A **AppDelegate** osztály felülírással értesítések regisztrálása az **RegisteredForRemoteNotifications** esemény hozzáadása:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. **AppDelegate**is adja hozzá a következő felülbírálása az **DidReceivedRemoteNotification** eseménykezelő:

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Ez a módszer kezeli a bejövő értesítéseket, az alkalmazás futása közben.

2. A **AppDelegate** osztály hozzáadása a **FinishedLaunching** módszerrel a következő kódot: 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Ez lehetővé teszi, hogy a távoli értesítések támogatása, és kéréseket leküldéses regisztrációs.

Az alkalmazás letöltése frissül a leküldéses értesítések támogatása.

####<a name="test-push-notifications-in-your-ios-app"></a>Teszt leküldéses értesítéseket, az iOS-alkalmazásban

1. Kattintson a jobb gombbal az iOS-projektet, majd kattintson a **StartPp projektként beállítása**.

2. Nyomja le a Visual Studio készítése a projekthez, és indítsa el az alkalmazást az iOS-eszközökön a **F5** vagy a **Futtatás** gombra, majd kattintson **az OK gombra** koppintva fogadja el a leküldéses értesítéseket.

    > [AZURE.NOTE] Az alkalmazás kifejezetten el kell fogadnia leküldéses értesítéseket. A kérés csak az első alkalommal a alkalmazást futtató fordul elő.

3. Abban az alkalmazásban, írjon be egy feladatot, és kattintson a plusz (**+**) ikonra.

4. Győződjön meg arról, hogy a értesítés érkezik, majd kattintson **az OK gombra** kattintva zárja be az értesítésre.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Nem kötelező) Állítsa be, és futtassa a Windows projektek

Ez a szakasz nem fut a Xamarin.Forms WinApp és a Windows rendszerű eszközökre WinPhone81 projektek. Ezeket a lépéseket is támogatni univerzális Windows platformon (UWP) projekteket. Ez a szakasz kihagyhatja, ha nem működik Windows eszközökkel.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>A leküldéses értesítések Windows-alkalmazás regisztrálása WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Az értesítés-központban WNS konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Leküldéses értesítések hozzáadása a Windows-alkalmazás

1. A Visual Studióban nyissa meg a **App.xaml.cs** Windows projekt, és adja hozzá a következő **használata** kimutatásokban.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Csere `<your_TodoItemManager_portable_class_namespace>` névtér a hordozható projekt, amely tartalmazza a `TodoItemManager` osztály.
 

2. Adja hozzá a következő **InitNotificationsAsync** módszer App.xaml.cs: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Ez a módszer a leküldéses értesítést csatorna kap, és a sablon értesítést kapni a értesítés központból sablon regisztrálja. Sablon értesítést, amely támogatja az *messageParam* érkeznek meg az ügyfélnek.

3. A App.xaml.cs, frissítse az **OnLaunched** esemény kezelő módszer definíciójának hozzáadásával a `async` módosító, majd adja hozzá a következő sort a kód végén található a módszer: 

        await InitNotificationsAsync();

    Ezzel biztosíthatja, hogy a leküldéses értesítést regisztráció létrejön vagy frissül, minden alkalommal, amikor az alkalmazást. Fontos, hogy ezt a WNS leküldéses csatorna értéke mindig aktív biztosítása érdekében.  

4. Megoldás Explorer for Visual Studio nyissa meg a **Package.appxmanifest** fájlt, és **Igen** a **értesítések**beállítása **Bejelentési alkalmas** .

5. Az alkalmazás összeállítása, és ellenőrizze a hibátlan van.  Ügyfél app most regisztráljanak a mobilalkalmazás kódmentes a sablon értesítésekhez. Ismételje meg ebben a szakaszban a megoldás minden Windows projektjére.


####<a name="test-push-notifications-in-your-windows-app"></a>Teszt leküldéses értesítéseket, a Windows-alkalmazásban

1. A Visual Studióban kattintson a jobb gombbal a Windows projekt, és kattintson a **beállítás indítása projektként**.

2. A **Futtatás** gombra kattintva hozza létre a projekt, és indítsa el az alkalmazást.

3. Abban az alkalmazásban, írja be egy új todoitem nevét, és kattintson a plusz (**+**) ikonra koppintva vegye fel.

4. Győződjön meg arról, hogy értesítést kapott, ha az elem felvétele.

##<a name="next-steps"></a>Következő lépések

További információ a leküldéses értesítések:

* [Leküldéses értesítést problémáinak diagnosztizálása](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Okból különböző miért értesítések kerülhetnek, vagy nem a végeredmény eszközökön. Ez a témakör bemutatja, hogyan elemzése és találnia, hogy a leküldéses értesítést hibák okát. 

Fontolja meg a Folytatás megjelenítheti az alábbi oktatóanyagok egyikét:

* [Hitelesítés felvétele az alkalmazásba](app-service-mobile-xamarin-forms-get-started-users.md)  
Megtudhatja, hogy miként tel az alkalmazás felhasználóinak hitelesítést végezni.

* [Az alkalmazás offline szinkronizálás engedélyezése](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Megtudhatja, hogy miként hozzáadása az offline munka támogatása az alkalmazás-mobilalkalmazás kódmentes használatával. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

