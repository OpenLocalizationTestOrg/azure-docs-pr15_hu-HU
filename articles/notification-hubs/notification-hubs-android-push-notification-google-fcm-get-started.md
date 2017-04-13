<properties
    pageTitle="Leküldéses értesítések küld az Androidhoz készült Azure értesítés hubok és az Firebase felhő |} Microsoft Azure"
    description="Ebből az oktatóanyagból megismerheti, hogyan Azure értesítés hubok és az Firebase felhő használatával Android-eszközön való leküldéses értesítéseket."
    services="notification-hubs"
    documentationCenter="android"
    keywords="leküldéses értesítések, leküldéses értesítést, android leküldéses értesítést, fcm, firebase cloud üzenetküldés"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Az Androidhoz készült Azure értesítés hubok küld a leküldéses értesítések

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

> [AZURE.IMPORTANT] Ez a témakör bemutatja, hogyan leküldéses értesítéseket a Google Firebase felhő üzenetküldés (FCM). Ha továbbra is használja a Google Cloud üzenetküldés (GCM), [az Androidhoz készült Azure értesítés hubok és GCM a küldése leküldéses értesítés](notification-hubs-android-push-notification-google-gcm-get-started.md)jelenik meg.

Ebből az oktatóanyagból megtudhatja, hogy miként, Azure értesítés hubok és az Firebase felhő segítségével küldje el a leküldéses értesítések Android az alkalmazás.
Kell létrehoznia, hogy a leküldéses értesítések fogadása Firebase felhő üzenetküldés (FCM) használatával egy üres Android alkalmazást.



[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Ebben az oktatóanyagban bejegyzett kódját letölthető GitHub [ide](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).


##<a name="prerequisites"></a>Előfeltételek

> [AZURE.IMPORTANT] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

- Nemcsak a fent említett aktív Azure fiók ebben az oktatóanyagban szükséges [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797)legújabb verzióját.
- Android 2.3-as vagy újabb Firebase felhő üzenetkezelés.
- Google tárházba verzió 27 vagy újabb szükség az Firebase felhő üzenetküldés használatára.
- A Google Play szolgáltatások 9.0.2 vagy újabb Firebase felhő üzenetkezelés.
- Ebben az oktatóanyagban befejezése rendszer minden más értesítés hubok oktatóanyagok az Android-alkalmazással.


##<a name="create-a-new-android-studio-project"></a>Android Studio új projekt létrehozása

1. Android Studio, az új projekt létrehozása Android Studio.

    ![Android Studio - új projekt](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)

2. Válassza ki a **telefonok és táblagépek** űrlap tényező és a **Minimum SDK** támogatja a használni kívánt. Kattintson a **Tovább gombra**.

    ![Android Studio - projekt-munkafolyamat létrehozása](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)

3. Válassza az **Üres tevékenység** a fő tevékenységet, kattintson a **Tovább**gombra, és kattintson a **Befejezés gombra**.


##<a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Hozzon létre egy projekt, amely támogatja a Firebase felhő üzenetküldés

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Állítsa be az új értesítés hubon

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. az értesítési központi, **Beállítások** lap, válassza **Értesítési szolgáltatások** és **Google (GCM)**. Adja meg a FCM kiszolgáló kulcsot a [Firebase konzol](https://firebase.google.com/console/) előbb másolt, és kattintson a **Mentés**gombra.

&emsp;&emsp;![Azure értesítés hubok - Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Az értesítési központi most úgy van beállítva Firebase felhő Messagin dolgozhat, és a kapcsolat karakterláncok őket, az alkalmazás a leküldéses értesítések küldésére és fogadására regisztrálni van.

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása az értesítési-központban

###<a name="add-google-play-services-to-the-project"></a>A Google Play szolgáltatások hozzáadása a projekthez

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Azure értesítés hubok tárak hozzáadása


1. Az a `Build.Gradle` az **alkalmazás**fájl, és adja meg a következő sort a **függőségeket** szakaszban.

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Adja hozzá a következő tárat a **függőségeket** szakasz után.

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>A AndroidManifest.xml frissíteni.


1. A támogatási FCM, azt kell végrehajtania egy példány azonosítója figyelő szolgáltatás, [regisztrációs tokenek beszerzése](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) használata [Google FirebaseInstanceId API -val](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId)használt a kód. Ebben az oktatóanyagban azt az osztály nevet `MyInstanceIDService`. 
 
    A következő definiált szolgáltatás hozzáadása a AndroidManifest.xml fájlt, benne a `<application>` címke. 

        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>



2. Miután a FCM regisztrációs jogkivonat kapott azt a FirebaseInstanceId API, fogjuk használni azt történő [regisztráláshoz az Azure-értesítés központban](notification-hubs-push-notification-registration-management.md). A háttér használata a regisztráció fog támogatjuk egy `IntentService` nevű `RegistrationIntentService`. Ez a szolgáltatás frissítését saját FCM regisztrációs jogkivonat felelős is.
 
    A következő definiált szolgáltatás hozzáadása a AndroidManifest.xml fájlt, benne a `<application>` címke. 

        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>



3. Azt is határozza meg a címzett, ha értesítést szeretne kapni. A következő címzettnek definíciót hozzáadása a AndroidManifest.xml fájlt, benne a `<application>` címke. Cserélje le a `<your package>` helyőrző az az a tényleges csomag nevét a felső részén látható a `AndroidManifest.xml` fájlt.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Adja hozzá a következő szükséges FCM kapcsolatos alábbi engedélyek a `</application>` címke. Győződjön meg arról, hogy le szeretné cserélni `<your package>` tetején látható nevű csomagot a `AndroidManifest.xml` fájlt.

    További tájékoztatást a következő engedélyeket [egy GCM ügyfél alkalmazás az Android-alapú telepítő](https://developers.google.com/cloud-messaging/android/client#manifest) és megtekintése [a GCM ügyfél alkalmazás az Android-alapú Firebase felhő üzenetküldéshez áttelepítése](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm)

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />


### <a name="adding-code"></a>Kód hozzáadása


1. A projekt nézetben bontsa ki az **alkalmazás** > **src** > **fő** > **java**. Kattintson a jobb gombbal a **java**csomag mappájában, kattintson az **Új**gombra, és válassza a **Java osztály**. Adja hozzá a nevű új osztály `NotificationSettings`. 

    ![Android Studio - új Java osztály](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)

    Ügyeljen arra, hogy a frissítés, az alábbi három helyőrzőket, a következő kódját a `NotificationSettings` osztály:
    * **SenderId**: az a **Felhő üzenetküldés** lapján a project beállításai a [Firebase konzol](https://firebase.google.com/console/)korábbi szerezte be a feladó azonosítója.
    * **HubListenConnectionString**: a központi **DefaultListenAccessSignature** kapcsolati karakterláncát. Kattintson a **Beállítások** lap, a központi az [Azure-portálon] **Hozzáférési** kattintva másolhatja a kapcsolati karakterlánc.
    * **HubName**: használja az értesítés központi a központi fel az [Azure-portálon]megjelenő neve.

    `NotificationSettings`kód:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }

2. A fenti lépésekkel hozzáadása egy másik új osztály nevű `MyInstanceIDService`. Ez a példány azonosítója figyelő szolgáltatás végrehajtása lesz.

    Az osztály kódját visszahívja a `IntentService` [a FCM jogkivonat frissítése](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) a háttérben.

        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;
        
        
        public class MyInstanceIDService extends FirebaseInstanceIdService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.d(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Egy másik új osztály hozzáadása a projekt neve, `RegistrationIntentService`. Ezt fogja végrehajtására a `IntentService` , amely [a FCM jogkivonat frissítése](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) , és [a értesítés hubhoz regisztrálása](notification-hubs-push-notification-registration-management.md)fogja kezelni.

    Használja a következő az osztály.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {
        
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
        
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. Az a `MainActivity` osztály, és adja meg a következő `import` kimutatások az osztályjegyzetfüzet nyilatkozat felett.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. A következő személyes tagok hozzáadása az osztály tetején. Ezek [a elérhetőségének megállapítása a Google Play szolgáltatások Google által javasolt](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk)azt fogja használni.

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. Az a `MainActivity` osztály, és adja meg a Google Play szolgáltatások elérhetőségét a következő módszerrel. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. Az a `MainActivity` osztály, és adja meg a következő kódrészlet ellenőrzi, hogy a Google Play szolgáltatások hívás előtt, hogy a `IntentService` és szeretné jobban a FCM regisztrációs jogkivonat regisztrálhatja a értesítés-központját.

        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. Az a `OnCreate` módszer a `MainActivity` osztály, és adja meg a következő kódot tevékenység létrehozásakor a regisztrációs folyamat indításához.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. További módszerek hozzáadása a `MainActivity` az alkalmazás alkalmazás állapotát és a jelentés állapotának ellenőrzéséhez.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. A `ToastNotify` módszer használja a *"Helló, világ"* `TextView` vezérlő állapotjelentést és értesítések folyamatosan az alkalmazásban. A activity_main.xml elrendezést adja meg a következő vezérlő azonosítója.

        android:id="@+id/text_hello"

11. Ezután hozzáadunk meg a címzett, a AndroidManifest.xml a definiált alosztálya. Egy másik új osztály hozzáadása a projekthez, nevű `MyHandler`.

12. Adja hozzá a következő importálása kimutatások tetején `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Adja hozzá a következő kódját a `MyHandler` osztály, így egy `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Ez a kód felülírja a `OnReceive` módszer, így a kezelő értesítést kapott jelentést. A kezelő is a leküldéses értesítést küld az Android értesítés manager használatával a `sendNotification()` módot. A `sendNotification()` módszert kell végrehajtani, amikor az alkalmazás nem fut, és értesítést kap.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Android Studio menüsávján, kattintson a **Szerkesztés** > **Újraépítéséhez projekt** ellenőrizze, hogy hibátlan szerepel a kódot.
15. Futtassa az alkalmazást az eszközön, és ellenőrizze, hogy sikeresen a értesítés hubhoz regisztrálja. 

    > [AZURE.NOTE] Regisztráció meghiúsulhat, amíg a kezdeti rovatok a `onTokenRefresh()` módszer Dokumentumazonosító szolgáltatás példányának neve. A frissítés sikeres regisztráció a értesítés hubhoz kezdeményezhet kell.

##<a name="sending-push-notifications"></a>Leküldéses értesítések küldése

Ellenőrizheti, hogy érkeznek be a leküldéses értesítéseket az alkalmazás az [Azure-portálon] – a központi lap **Hibaelhárítás** szakaszában keresse meg a via küldésével alább látható módon.

![Azure értesítés hubok - próba küldése](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Nem kötelező) Küldje el a leküldéses értesítések közvetlenül a alkalmazásból

>[AZURE.IMPORTANT] Ebben a példában az értesítés küldése a ügyfél alkalmazásból hiányzik tanulási csak célokat. Mivel az ehhez szükséges a `DefaultFullSharedAccessSignature` az ügyfél-alkalmazás telepítve legyen a kockázat, hogy a felhasználó hozzáférhet jogosulatlan értesítést küld az ügyfelek a értesítés elosztót teszi lehetővé.

A szokásos módon volna küldött értesítések kódmentes-kiszolgálóval. Egyes esetekben célszerű lehet küldeni a leküldéses értesítések közvetlenül az ügyfélalkalmazás. Ez a szakasz ismerteti, hogyan értesítéseket küldeni az ügyféltől a [Azure értesítés központi REST API -t](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Android Studio Project nézetben bontsa ki a **alkalmazás** > **src** > **fő** > **felbontás** > **Elrendezés**. Nyissa meg a `activity_main.xml` elrendezés fájlt, és kattintson a **szöveg** frissítése a fájl tartalmát a szöveg fülre. Frissíti a fájlt a kódot, az alábbi, amely új `Button` és `EditText` vezérlőelemek leküldéses értesítést üzeneteket küld az értesítési-központban. Ez a kód hozzáadása a képernyő alján csak előtt `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Android Studio Project nézetben bontsa ki a **alkalmazás** > **src** > **fő** > **felbontás** > **értékeket**. Nyissa meg a `strings.xml` fájlt, és adja meg a karakterlánc értéket, az új hivatkoznak `Button` és `EditText` vezérlők. Adja hozzá ezeket a képernyő alján a fájl csak előtt `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. Az a `NotificationSetting.java` fájlt, és adja meg a következő beállítást a `NotificationSettings` osztály.

    Frissítés `HubFullAccess` a központi **DefaultFullSharedAccessSignature** kapcsolati karakterlánccal. A másolt a kapcsolati karakterláncot az [Azure-portálon] kattintson a **Beállítások** lap az értesítési központi az **Access-házirendek** gombra kattintva.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";

4. Az a `MainActivity.java` fájlt, és adja meg a következő `import` a fenti utasításokat a `MainActivity` osztály.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. Az a `MainActivity.java` fájl, a következő tagok hozzáadása a képernyő tetején, a `MainActivity` osztály.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Létre kell hoznia egy szoftver Access aláírás (társítások) token üzeneteket küldeni az értesítési központi POST kérelem hitelesítést végezni. A kapcsolati karakterlánc kulcsfontosságú adatainak elemzése és a biztonsági jogkivonat, majd létrehozása, a [Közös fogalmak](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-hivatkozás említett végzi. A következő kód egy példa végrehajtása szolgál.

    A `MainActivity.java`, adja hozzá a következő módszer, hogy a `MainActivity` osztály a kapcsolati karakterlánc elemzésére.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. A `MainActivity.java`, adja hozzá a következő módszer, hogy a `MainActivity` osztály hitelesítési token társítások létrehozása.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. A `MainActivity.java`, adja hozzá a következő módszer, hogy a `MainActivity` osztály kezeléséhez kattintson a **Értesítés küldése** gombra, és küldje el a leküldéses értesítést a hubon keresztül csatlakozott a beépített REST API segítségével.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Az alkalmazás tesztelése

####<a name="push-notifications-in-the-emulator"></a>A irányító a leküldéses értesítések

Ha azt szeretné, tesztelje a leküldéses értesítések belül egy irányító, győződjön meg arról, hogy a irányító kép támogatja-e a Google API szintet, amelyhez az alkalmazás. Ha a kép nem támogatja natív Google API-hoz, akkor fog a végeredmény a **szolgáltatás\_nem\_elérhető** kivétel.

A fenti kívül biztosítása, hogy hozzáadta a Google-fiókot **a beállítások**csoportban a futó irányító > **fiókok**. A kísérletek GCM regisztrálása vonhat ellenkező esetben a **hitelesítési\_sikertelen** kivétel.

####<a name="running-the-application"></a>Az alkalmazás futtatása

1. Futtassa az alkalmazást, és figyelje meg, hogy a sikeres bejegyzési jelentett-e a regisztrációs Azonosítót.

    ![Az Android - csatorna regisztrációs tesztelése](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)

2. Írja be az összes Android rendszerű eszközön regisztrált a központi küldendő értesítő üzenet.

    ![Az Android - az üzenet elküldése tesztelése](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)

3. Nyomja meg az **Értesítés küldése**. Az operációs rendszert futtató alkalmazás bármely eszközök jelennek meg egy `AlertDialog` példány a leküldéses értesítést üzenettel. A leküldéses értesítéseket, amely nincs a alkalmazást futtató, de a korábban már regisztráltak eszközök szóló értesítést kap az Android-alapú értesítés kezelő. Azok a pöccintsen lefelé, a bal felső saroktól megtekinthetők.

    ![Az Android - értesítések tesztelése](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

##<a name="next-steps"></a>Következő lépések

A következő lépésben az [Használata értesítés hubok leküldéses értesítéseket, hogy a felhasználók az] oktatóprogram javasoljuk. Meg kell követnie-címkék használata a célként megadott felhasználók ASP.NET kódmentes az értesítéseket küldeni.

Ha szeretne kamat csoportok a felhasználói szegmens, olvassa el a [Használati értesítés hubok legfrissebb híreket küldése] oktatóprogram.

Értesítés hubok általánosabb információt című témakörben az [Értesítési hubok útmutatást].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Értesítés hubok útmutató]: notification-hubs-push-notification-overview.md
[Értesítés hubok segítségével felhasználók a leküldéses értesítések]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Legfrissebb hírek küldése értesítés hubok használatával]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure portál]: https://portal.azure.com
