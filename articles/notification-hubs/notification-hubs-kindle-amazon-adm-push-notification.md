<properties
    pageTitle="Azure értesítés hubok Kindle alkalmazások használatának első lépései |} Microsoft Azure"
    description="Ebből az oktatóanyagból megismerheti, hogyan Azure értesítés hubok segítségével küldje el a leküldéses értesítések Kindle alkalmazás."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Értesítés hubok Kindle alkalmazások használatának első lépései

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy miként, Azure értesítés hubok segítségével küldje el a leküldéses értesítések Kindle alkalmazás.
Kell létrehoznia egy üres Kindle alkalmazást, hogy a leküldéses értesítések fogadása Amazon eszköz üzenetküldés (OPA) használatával.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban az alábbiakra van szükség:

+ Az Android SDK (feltételezzük, hogy Holdas használandó) letölthető az <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-webhelyet</a>.
+ Kövesse a <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Beállítás be a fejlesztői környezet</a> a fejlesztői környezet beállítása Kindle.

##<a name="add-a-new-app-to-the-developer-portal"></a>Új alkalmazás hozzáadása a developer Portal segítségével

1. Először hozza létre az alkalmazás az [Amazon developer Portal segítségével].

    ![][0]

2. Másolja a vágólapra a **helyi menüt megjelenítő billentyűt**.

    ![][1]

3. A portálon kattintson az alkalmazás nevére, és kattintson az **Eszköz üzenetküldés** fülre.

    ![][2]

4. Kattintson a **Létrehozás egy új biztonsági profilt**, és hozza létre az új biztonsági profilt (például a **TestAdm biztonsági profilt**). Ezután kattintson a **Mentés**gombra.

    ![][3]

5. Kattintson a **Biztonsági profilt** az imént létrehozott biztonsági profil megtekintése gombra. Másolja az **Ügyfél-azonosító** , és az **Ügyfél titkos** értékeket későbbi felhasználás céljából.

    ![][4]

## <a name="create-an-api-key"></a>Hozzon létre egy API-ja kulcs

1. Nyisson meg egy parancssorablakot rendszergazdaként.
2. Nyissa meg azt a mappát, Android SDK csomagjában talál.
3. Írja be a következő parancsot:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  A **keystore** jelszót írja be az **android**.

5.  Másolja a vágólapra a **MD5** ujjlenyomat.
6.  Vissza a developer Portal segítségével az **üzenetek** lapon az **Android/Kindle** és adja meg az alkalmazást (például a **com.sample.notificationhubtest**) és **MD5** értékét a csomag nevét, gombmenü **API -ja kulcs generálása**.

## <a name="add-credentials-to-the-hub"></a>A központi hitelesítő adatok hozzáadása

A portálon adja hozzá az ügyfél titkos és az ügyfél-azonosító az értesítési központi **konfigurálása** lapja.

## <a name="set-up-your-application"></a>Az alkalmazás beállítása

> [AZURE.NOTE] Az alkalmazás létrehozásakor használjon legalább API szint 17.

A ADM tárak hozzáadása a Holdas projekthez:

1. A ADM tárat, [Töltse le a SDK]juthat. Bontsa ki a SDK zip-fájl.
2. Holdas kattintson a jobb gombbal a projekt, és válassza a **Tulajdonságok parancsot**. A bal oldalon válassza a **Java összeállítása elérési utat** , és válassza a a **tárak **lap tetején. Kattintson a **Külső Jar hozzáadása**, és válassza a fájl `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` a címtárból, amelyben az Amazon SDK kibontása.
3. Töltse le a NotificationHubs Android SDK (csatolás).
4. Bontsa ki a csomagot, és húzza a fájlt `notification-hubs-sdk.jar` be a `libs` Holdas mappájában.

Szerkessze a alkalmazás jegyzék ADM támogatása:

1. Adja hozzá az Amazon névtér a nyilvánvalóan gyökérelemben:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Engedélyek megadása a nyilvánvalóan elem alatt az első elemet. A csomag, az alkalmazás létrehozására használt helyett a **[Csomag név]** .

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Helyezze be a következő elem az alkalmazás elemet az első gyermekként. Ne feledje, hogy **[: A szolgáltatás neve]** helyettesítése a ADM üzenet kezelő (beleértve a csomag), a következő szakaszban létrehozott a nevet, és **[Csomag név]** cserélje le a csomag nevére, amellyel hozta létre az alkalmazását.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>A ADM üzenet eseménykezelő létrehozása

1. Hozzon létre egy új osztály öröklő `com.amazon.device.messaging.ADMMessageHandlerBase` , és nevezze el `MyADMMessageHandler`, az alábbi ábrán látható módon:

    ![][6]

2. Adja hozzá a következő `import` kimutatások:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Adja hozzá a következő kódot az Ön által létrehozott osztály. Ne feledje, hogy helyettesítése a központi nevét és a kapcsolati karakterlánc (Figyelés):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Adja hozzá a következő kódot a `OnMessage()` módszer:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Adja hozzá a következő kódot a `OnRegistered` módszer:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Adja hozzá a következő kódot a `OnUnregistered` módszer:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. Az a `MainActivity` módszer, az alábbi importálása utasítás hozzáadása:

        import com.amazon.device.messaging.ADM;

8. A következő kód hozzáadása a végén a `OnCreate` módszer:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Az alkalmazás az API-ja kulcs hozzáadása

1. A Holdas, **api_key.txt** a címtár-eszközök a projekt nevű új fájl létrehozása.
2. Nyissa meg a fájlt, és az API billentyűt az Amazon developer Portal segítségével létrehozott másolja.

## <a name="run-the-app"></a>Futtassa az alkalmazást

1. Indítsa el a irányító.
2. A irányító, a pöccintsen a képernyő tetején kattintson a **Beállítások**elemre, és kattintson **a fiók** és érvényes Amazon fiók regisztrálása.
3. Holdas futtassa az alkalmazást.

> [AZURE.NOTE] Ha hiba történik, ellenőrizze az időpont irányító (vagy eszközt). Az idő érték pontos kell lennie. Ha módosítani szeretné a Kindle irányító időtartamát, az Android SDK platform-eszközök címtárból futtathatja a következő parancsot:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Üzenet küldése

Üzenet küldése .NET használatával:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon developer Portal segítségével]: https://developer.amazon.com/home.html
[a SDK letöltése]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
