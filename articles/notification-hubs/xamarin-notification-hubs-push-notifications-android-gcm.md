<properties
    pageTitle="Értesítés hubok Xamarin.Android alkalmazások használatának első lépései |} Microsoft Azure"
    description="Ebből az oktatóanyagból megismerheti, hogyan Azure értesítés hubok használni a leküldéses értesítéseket küldeni egy Xamarin Android alkalmazást."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Értesítés hubok Xamarin az Android-alapú használatának első lépései

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy miként, Azure értesítés hubok segítségével küldje el a leküldéses értesítések Xamarin.Android alkalmazás.
Leküldéses értesítések fogadása a Google Cloud üzenetküldés (GCM) egy üres Xamarin.Android alkalmazással létre fogja hozni. Ha végzett, is használhatja az értesítési központi szétküldése fut, az alkalmazás az összes eszközök leküldéses értesítéseket. A kész kódot érhető el az [alkalmazás NotificationHubs] [ GitHub] minta.

Ebben az oktatóanyagban értesítés hubok használata egyszerű közvetítési forgatókönyv mutatja be.


## <a name="before-you-begin"></a>Első lépések

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Ebben az oktatóanyagban bejegyzett kódját GitHub találhatók [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban az alábbiakra van szükség:

+ Visual Studio környezetben Xamarin a Windows vagy Mac OS x teljes telepítési utasításokat a Xamarin Studio a [telepítő, és telepítse az Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx)vannak.
+ Aktív Google-fiók
+ [Azure üzenetküldés komponens]
+ [A Google Cloud üzenetküldés ügyfél-összetevő]

Ebben az oktatóanyagban befejezése rendszer minden más értesítés hubok oktatóanyagok-Xamarin.Android alkalmazást.

> [AZURE.IMPORTANT] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>A Google Cloud üzenetküldés engedélyezése

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Az értesítés központi konfigurálása

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Kattintson a <b>beállítás</b> fülre a képernyő tetején, és adja meg az <b>API -ja kulcs</b> értékét szerezte be az előző szakaszban kattintson a <b>Mentés</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Az értesítési központi most már be van állítva GCM dolgozhat, és a kapcsolat karakterláncok mindkét regisztrálni az alkalmazást, ha értesítést szeretne kapni, és küldje el a leküldéses értesítések van.

##<a name="connect-your-app-to-the-notification-hub"></a>Az alkalmazás csatlakoztatása az értesítési-központban

###<a name="create-a-new-project"></a>Új projekt létrehozása

1. A Xamarin Studióban kattintson az **Új megoldás**, kattintson az **Alkalmazás az Android**, és kattintson a **Tovább**gombra.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Adja meg az **Alkalmazás neve** és **azonosítója**. Kattintson a **Cél Plaforms** támogatja, és kattintson a **Tovább gombra** , és a **Létrehozás**gombra.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Ez a Android új projektet hoz létre.

2. Kattintson a jobb gombbal a megoldást nézetben az új projekten, és válassza a **Beállítások**, nyissa meg a projekt tulajdonságait. A **Szerkesztés** csoportban jelölje ki az **Android alkalmazás** elemet.

    Győződjön meg arról, hogy a **csomag nevére** az első betűje kisbetűre cseréli le.

    > [AZURE.IMPORTANT] A csomag nevére az első betűje kisbetűre cseréli le kell lennie. Ellenkező esetben nyilvánvalóan alkalmazáshibák fog kapni, amikor a **BroadcastReceiver** és **IntentFilter** regisztrál az alábbi leküldéses értesítéseket.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Tetszés szerint állítsa a **Minimum Android verzió** egy másik API szintű.

4. Tetszés szerint állítsa a **cél Android verzió** célba, amelyet más API-verziót (kell lennie API szintű 8-as vagy újabb).

Kattintson az **OK gombra** , és a Project beállításai párbeszédpanel bezárásához.


###<a name="add-the-required-components-to-your-project"></a>A szükséges összetevők hozzáadása a projekthez

A Google Cloud üzenetküldés ügyfél elérhető Xamarin összetevő áruházból leegyszerűsíti a leküldéses értesítések támogatása Xamarin.Android.

1. Kattintson a jobb gombbal a összetevők mappa Xamarin.Android alkalmazásban, és válassza a **További összetevők első**.

2. Az **Azure Messaging** összetevőt kereshet, és adja hozzá a projekthez.

3. A **Google Cloud üzenetküldés ügyfél** összetevő kereshet, és adja hozzá a projekthez.


###<a name="set-up-notification-hubs-in-your-project"></a>A projekt bejelentés hubok beállítása

1. Gyűjtse össze a Android alkalmazást és az értesítési-központját az alábbi információkat:

    - **GoogleProjectNumber**: a projekt száma érték beolvasása a az alkalmazást a Google Developer Portal áttekintése. Megjegyzés: Ez az érték korábban elvégzett az alkalmazást a portálon létrehozásakor.
    - **Kapcsolati karakterlánc meghallgatása**: az irányítópulton a [Klasszikus Azure-portálon]kattintson a **Nézet kapcsolati karakterláncot**. Másolja a vágólapra a *DefaultListenSharedAccessSignature* kapcsolati karakterláncot, ehhez az értékhez.
    - **Központi neve**: Ez az a központi az [Azure klasszikus portál]nevét. Ha például *mynotificationhub2*.

    A Xamarin projekthez **Constants.cs** osztály létrehozása, és a következő állandó értékek megadása az osztály. A helyőrzők cseréje az értékeket.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Adja hozzá a következő **MainActivity.cs**kimutatások használatával:

        using Android.Util;
        using Gcm.Client;

3. Egy példány változó hozzáadása a `MainActivity` osztály-értesítés párbeszédpanel megjelenítése, ha az alkalmazás fut használt:

        public static MainActivity instance;


3. A következő módszerrel a **MainActivity** osztály létrehozása:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. Az a `OnCreate` **MainActivity.cs**, metódusát inicializálni a `instance` változó és hívást kezdeményez hozzáadása `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Hozzon létre egy új osztály **MyBroadcastReceiver**.

    > [AZURE.NOTE] Fog végigvezetjük az alapoktól az alábbi **BroadcastReceiver** osztály létrehozása. Egy rövid ahelyett, hogy manuálisan **MyBroadcastReceiver.cs** viszont ha nézni szeretné a minta Xamarin.Android projektben, a [NotificationHubs minták]részét képező **GcmService.cs** fájlra[GitHub]. **GcmService.cs** másolása és módosítása tartalmazza az remek kiindulási is lehet.

5. Adja hozzá a következő **MyBroadcastReceiver.cs** (a összetevő és összeállítás, amely a korábban hozzáadott hivatkozó) kimutatások használatával:

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. **MyBroadcastReceiver.cs**adja hozzá a következő jogosultsági kérelmek a **használata** kimutatásokban és a **névtér** nyilatkozat között:

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. **MyBroadcastReceiver.cs**módosítsa a **MyBroadcastReceiver** osztály megfelelően a következőket:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Adjon egy másik osztály **MyBroadcastReceiver.cs** nevű **PushHandlerService**, amely **GcmServiceBase**származik. Győződjön meg arról, hogy a **szolgáltatás** attribútum alkalmazni az osztály:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** módszerek **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**és **OnError()**hajtja végre. A **PushHandlerService** végrehajtása osztály kell felülbírálása ezeket a módszereket, és ezeket a módszereket fog fire válaszként az értesítési-központban személlyel.


9. A **OnRegistered()** módszer **PushHandlerService** felülbírálása az alábbi kód használatával:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] A fenti **OnRegistered()** kódot vegye figyelembe az azt jelenti, hogy adja meg a címkék regisztrálhatja adott üzenetben csatornák.

10. A **OnMessage** módszer **PushHandlerService** felülbírálása az alábbi kód használatával:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Az alábbi módszerek **createNotification** és **dialogNotify** felvétele **PushHandlerService** a felhasználók értesítésével arról, hogy amikor értesítést kap.

    >[AZURE.NOTE] Értesítés tervezés 5.0-s vagy újabb Android verzióban a korábbi verziók, amelyek egy jelentős indító jelöli. Ha tesztelni ez Android 5.0-s vagy újabb, az alkalmazás kell futnia, amely az értesítéseket. További információ [Az Android értesítés](http://go.microsoft.com/fwlink/?LinkId=615880)jelenik meg.

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Absztrakt tagok **OnUnRegistered()** **OnRecoverableError()**és **OnError()** bírálja felül az, hogy a kód lefordítja:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Az alkalmazás a irányító futtatása

Ha az alkalmazás a irányító futtatja, ellenőrizze az Android virtuális eszköz (AVD), amely támogatja a Google API-k használata.

> [AZURE.IMPORTANT] Annak érdekében, hogy a leküldéses értesítéseket, be kell állítania a Google-fiók Android-eszközön virtuális. (A irányító, keresse meg a **beállításai** és **A fiók hozzáadása**gombra.) Győződjön meg arról, hogy a irányító csatlakozik az internethez.

>[AZURE.NOTE] Értesítés tervezés 5.0-s vagy újabb Android verzióban a korábbi verziók, amelyek egy jelentős indító jelöli. További információ [Az Android értesítés](http://go.microsoft.com/fwlink/?LinkId=615880)jelenik meg.


1. Az **eszközök**menüben kattintson a **Megnyitás Android irányító Manager**, válassza ki az eszközét, és kattintson a **Szerkesztés**parancsra.

    ![][18]

2. Jelölje be a **Google API -khoz** be a **cél**, és kattintson **az OK**gombra.

    ![][19]

3. A felső eszköztáron kattintson a **Futtatás**parancsra, és válassza ki az alkalmazást. Ez a irányító elindul, és fut, az alkalmazás.

  Az alkalmazás a *registrationId* GCM és az regisztrálja az értesítési-központban.

##<a name="send-notifications-from-your-backend"></a>Értesítés küldése a kódmentes


Ellenőrizheti, hogy értesítésekkel az alkalmazásban értesítést küld az [Azure klasszikus portál] hibakeresési lapján kattintson az értesítési-központban keresztül az alábbi képernyőképen látható módon.

![][30]


Leküldéses értesítések általában küldjük kompatibilis tár például a mobil szolgáltatások vagy ASP.NET kódmentes szolgáltatás. A REST API közvetlenül az értesítési küldésére, ha egy tárban nem érhető el a kódmentes is használhatja.

Az alábbiakban néhány egyéb, előfordulhat, hogy ellenőrizni kívánt küldött értesítések oktatóanyagok listája:

- ASP.NET: Lásd: [Használata értesítés hubok a felhasználóknak a leküldéses értesítéseket].
- Azure értesítés hubok Java SDK: megtudhatja, [hogy miként Java az értesítési hubok használata](notification-hubs-java-push-notification-tutorial.md) az értesítéseket küldeni Java. Ez tesztelte a Holdas Android fejlesztéséhez.
- PHP: Lásd: [értesítés hubok a PHP használatáról](notification-hubs-php-push-notification-tutorial.md).


Az oktatóprogram kapcsolatban a következő részek a küldött értesítések egy .NET konzol alkalmazás használatával, és egy mobilszolgáltatás keresztül csomópont parancsfájl használatával.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Nem kötelező) Értesítés küldése a .NET-alkalmazás segítségével

Ebben a részben azt értesítést küld egy .NET konzol alkalmazás használatával

1. Új képi C# console-alkalmazás létrehozása:

    ![][20]

2. A Visual Studióban kattintson az **eszközök** **NuGet csomag Manager**kattintson, és válassza a **Csomag kezelője konzol**.

    A csomag kezelője konzol a Visual Studióban jeleníti meg.

3. Csomag kezelője konzol ablakában a **projekt alapértelmezett** beállítása a konzol alkalmazás új projekt, és majd konzol ablakában végre az alábbi parancsot:

        Install-Package Microsoft.Azure.NotificationHubs

    Ez a lehetőség a <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification hubok NuGet csomag</a>használata Azure-értesítés hubok SDK mutató hivatkozás.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Nyissa meg a Program.cs fájlt, és adja hozzá a következő `using` utasítás:

        using Microsoft.Azure.NotificationHubs;

5. Az a `Program` osztály, és adja meg a következő módszerrel. A helyőrző szöveg frissítse a *DefaultFullSharedAccessSignature* csatlakozási karakterlánc és a központi neve az [Azure klasszikus portálon].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Adja meg a következő sort a **fő** mód:

         SendNotificationAsync();
         Console.ReadLine();

7. Nyomja le az F5 billentyűvel futtassa az alkalmazást. Értesítés a alkalmazásban kell kapnia.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Nem kötelező) Értesítés küldése egy mobilszolgáltatás segítségével

1. Hajtsa végre az [első lépések a mobil szolgáltatások].

1. Jelentkezzen be a [Klasszikus Azure-portálra], és jelölje ki a mobil szolgáltatást.

2. Jelölje ki a **Feladatütemező** fülre a képernyő tetején.

    ![][22]

3. Hozzon létre egy új ütemezett feladat, helyezze be a nevét, és jelölje ki **az igény**.

    ![][23]

4. Ha a feladat jön létre, kattintson a projekt nevére. Kattintson a felső sávon a **parancsfájl** -fülre.

5. Szúrjon be a következőt a Feladatütemező függvény belül. Ellenőrizze, hogy a értesítés központi neve és a kapcsolati karakterlánc a helyőrzők cseréje *DefaultFullSharedAccessSignature* korábbi szerezte be. Kattintson a **Mentés**gombra.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Kattintson **Egyszer futtassa** az alsó sávján. Egy értesítőjére kell kapnia.

##<a name="next-steps"></a>Következő lépések

Egyszerű ebben a példában akkor az Android rendszerű eszközökre értesítések rendelkezésére bocsát. Annak érdekében, hogy a célként megadott felhasználók, olvassa el az oktatóprogram [Használata értesítés hubok a felhasználóknak a leküldéses értesítéseket]. Oszthatja fel a felhasználók kamat csoportok szerint szeretné, ha erről [Használata értesítés hubok küldése legfrissebb híreket]. További tájékoztatást értesítés hubok [Értesítés hubok útmutatást] és az [Értesítési hubok ennek az Android-alapú].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Mobil szolgáltatások – első lépések]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure klasszikus portál]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Értesítés hubok útmutató]: http://msdn.microsoft.com/library/jj927170.aspx
[Értesítés hubok ennek androidra]: http://msdn.microsoft.com/library/dn282661.aspx

[Értesítés hubok segítségével felhasználók a leküldéses értesítések]: /manage/services/notification-hubs/notify-users-aspnet
[Legfrissebb hírek küldése értesítés hubok használatával]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[A Google Cloud üzenetküldés ügyfél-összetevő]: http://components.xamarin.com/view/GCMClient/
[Azure üzenetküldés komponens]: http://components.xamarin.com/view/azure-messaging
