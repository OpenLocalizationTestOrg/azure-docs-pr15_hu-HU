<properties
    pageTitle="Azure értesítés hubok értesíti a felhasználók az Android-alapú .NET kódmentes"
    description="Útmutató: a leküldéses értesítéseket küldeni a felhasználónak Azure-ban. Az Android-alapú Java nyelven írt mintakódok"
    documentationCenter="android"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a>Azure értesítés hubok értesíti a felhasználók az Android-alapú .NET kódmentes


[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##<a name="overview"></a>– Áttekintés

Leküldéses értesítést támogatása az Azure lehetővé teszi, hogy egy könnyen használható multiplatform és méretezett kivételének leküldéses infrastruktúra eleme nagyban egyszerűsíti a leküldéses értesítések egyszerre fogyasztói és vállalati alkalmazások mobil platformokon történő elérésére. Ebből az oktatóanyagból megtudhatja, Azure értesítés hubok használatáról a leküldéses értesítéseket küldeni egy adott alkalmazás felhasználó egy adott eszközön. Egy ASP.NET WebAPI kódmentes hitelesítést végezni az ügyfelek és szolgál értesítéseket, az útmutatást a témakör [az alkalmazás kódmentes a regisztrálás](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)látható módon. Ebben az oktatóprogramban az értesítési-központban, az [Értesítési hubok (Android) – első lépések](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyagban hoztunk épül.

> [AZURE.NOTE] Ebben az oktatóanyagban feltételezi, hogy létrehozása és beállítása a értesítés központi, [Értesítés hubok (Android) – első lépések](notification-hubs-android-push-notification-google-gcm-get-started.md)leírt módon.

[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>Az Android projekt létrehozása

A következő lépésként az Android-alkalmazás létrehozása.

1. Kövesse az [Értesítési hubok (Android) – első lépések](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóprogram hozhat létre, és állítsa be az alkalmazás való GCM a leküldéses értesítéseket.

2. Nyissa meg a **res/layout/activity_main.xml** fájlt, és cserélje le a fel a következő tartalom definíciók.

    Ezzel felveszi az új EditText vezérlők felhasználónak bejelentkezés. Is egy mezőt, amely küldött értesítések része lesz felhasználónév címke ad hozzá:

        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
            android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

        <EditText
            android:id="@+id/usernameText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/usernameHint"
            android:layout_above="@+id/passwordText"
            android:layout_alignParentEnd="true" />
        <EditText
            android:id="@+id/passwordText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/passwordHint"
            android:inputType="textPassword"
            android:layout_above="@+id/buttonLogin"
            android:layout_alignParentEnd="true" />
        <Button
            android:id="@+id/buttonLogin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/loginButton"
            android:onClick="login"
            android:layout_above="@+id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:layout_marginBottom="24dp" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="WNS on"
            android:textOff="WNS off"
            android:id="@+id/toggleButtonWNS"
            android:layout_toLeftOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="GCM on"
            android:textOff="GCM off"
            android:id="@+id/toggleButtonGCM"
            android:checked="true"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="APNS on"
            android:textOff="APNS off"
            android:id="@+id/toggleButtonAPNS"
            android:layout_toRightOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessageTag"
            android:layout_below="@id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_tag_hint" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessage"
            android:layout_below="@+id/editTextNotificationMessageTag"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_hint" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/send_button"
            android:id="@+id/sendbutton"
            android:onClick="sendNotificationButtonOnClick"
            android:layout_below="@+id/editTextNotificationMessage"
            android:layout_centerHorizontal="true" />
        </RelativeLayout>



3. Nyissa meg a **res/values/strings.xml** fájlt, és cserélje le a `send_button` újra a karakterláncot, az alábbi vonallal meghatározása a `send_button` és karakterláncok számára a további vezérlők hozzáadása:

        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>

    A main_activity.xml grafikus elrendezés így most néz ki:

    ![][A1]

4. Hozzon létre egy új osztály **RegisterClient** nevű fájlt egy csomagban a `MainActivity` osztály. Használja a alatt az új osztály fájl.

        import java.io.IOException;
        import java.io.UnsupportedEncodingException;
        import java.util.Set;

        import org.apache.http.HttpResponse;
        import org.apache.http.HttpStatus;
        import org.apache.http.client.ClientProtocolException;
        import org.apache.http.client.HttpClient;
        import org.apache.http.client.methods.HttpPost;
        import org.apache.http.client.methods.HttpPut;
        import org.apache.http.client.methods.HttpUriRequest;
        import org.apache.http.entity.StringEntity;
        import org.apache.http.impl.client.DefaultHttpClient;
        import org.apache.http.util.EntityUtils;
        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.util.Log;

        public class RegisterClient {
            private static final String PREFS_NAME = "ANHSettings";
            private static final String REGID_SETTING_NAME = "ANHRegistrationId";
            private String Backend_Endpoint;
            SharedPreferences settings;
            protected HttpClient httpClient;
            private String authorizationHeader;

            public RegisterClient(Context context, String backendEnpoint) {
                super();
                this.settings = context.getSharedPreferences(PREFS_NAME, 0);
                httpClient =  new DefaultHttpClient();
                Backend_Endpoint = backendEnpoint + "/api/register";
            }

            public String getAuthorizationHeader() {
                return authorizationHeader;
            }

            public void setAuthorizationHeader(String authorizationHeader) {
                this.authorizationHeader = authorizationHeader;
            }

            public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
                String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

                JSONObject deviceInfo = new JSONObject();
                deviceInfo.put("Platform", "gcm");
                deviceInfo.put("Handle", handle);
                deviceInfo.put("Tags", new JSONArray(tags));

                int statusCode = upsertRegistration(registrationId, deviceInfo);

                if (statusCode == HttpStatus.SC_OK) {
                    return;
                } else if (statusCode == HttpStatus.SC_GONE){
                    settings.edit().remove(REGID_SETTING_NAME).commit();
                    registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                    statusCode = upsertRegistration(registrationId, deviceInfo);
                    if (statusCode != HttpStatus.SC_OK) {
                        Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                        throw new RuntimeException("Error upserting registration");
                    }
                } else {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            }

            private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                    throws UnsupportedEncodingException, IOException,
                    ClientProtocolException {
                HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
                request.setEntity(new StringEntity(deviceInfo.toString()));
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                request.addHeader("Content-Type", "application/json");
                HttpResponse response = httpClient.execute(request);
                int statusCode = response.getStatusLine().getStatusCode();
                return statusCode;
            }

            private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
                if (settings.contains(REGID_SETTING_NAME))
                    return settings.getString(REGID_SETTING_NAME, null);

                HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                HttpResponse response = httpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                    throw new RuntimeException("Error creating Notification Hubs registrationId");
                }
                String registrationId = EntityUtils.toString(response.getEntity());
                registrationId = registrationId.substring(1, registrationId.length()-1);

                settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

                return registrationId;
            }
        }

    Ez az összetevő alkalmazza a többi hívások, lépjen kapcsolatba az alkalmazás kódmentes annak érdekében, hogy a leküldéses értesítések regisztrálása szükséges. Az értesítés-központban, [az alkalmazás kódmentes a regisztrálás](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)ismertetett módon által létrehozott *registrationIds* helyileg is tárol. Ne feledje, hogy használja-e egy jóváhagyási jogkivonat helyi tárolója tárolja a **Bejelentkezés** gombra.

5. Az a `MainActivity` osztály meg a private mező, a Megjegyzés hozzáadása vagy eltávolítása `NotificationHub`, és a mező felvétele a `RegisterClient` osztály és a ASP.NET kódmentes végpont karakterlánc. Helyére ne felejtse el `<Enter Your Backend Endpoint>` , az a tényleges kódmentes végpont korábban kapott. Ha például `http://mybackend.azurewebsites.net`.


        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


6. Az a `MainActivity` az osztály a `onCreate` módszer, távolítsa el vagy ki a inicializálni megjegyzések hozzáfűzése a `hub` mező és a hívást a `registerWithNotificationHubs` módszer. Vegye fel egy példányának inicializálni kódot a `RegisterClient` osztály. A módszer tartalmaznia kell a következő sort:

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            MyHandler.mainActivity = this;
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
            gcm = GoogleCloudMessaging.getInstance(this);

            //hub = new NotificationHub(HubName, HubListenConnectionString, this);
            //registerWithNotificationHubs();

            registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

            setContentView(R.layout.activity_main);
        }

7. Az a `MainActivity` osztály, törlése vagy ki a teljes megjegyzés `registerWithNotificationHubs` módot. Nem lesz ebben az oktatóanyagban.

8. Adja hozzá a következő `import` utasítások a **MainActivity.java** fájlhoz.

        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;


9. Adja hozzá a következő módszerekkel kezelheti a **Bejelentkezés** gombra kattintson az esemény és a küldő leküldéses értesítéseket.

        @Override
        protected void onStart() {
            super.onStart();
            Button sendPush = (Button) findViewById(R.id.sendbutton);
            sendPush.setEnabled(false);
        }

        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());

            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(SENDER_ID);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }

        /**
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {

                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;

                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");

                        HttpResponse response = new DefaultHttpClient().execute(request);

                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }

                    return null;
                }
            }.execute(null, null, null);
        }


    A `login` a **Bejelentkezés** gombra a kezelő létrehoz egy alapszintű hitelesítés token beviteli felhasználónév és jelszó (figyelje meg, hogy ez képviseli bármely jogkivonat, használja a hitelesítési módot) használ, akkor használja `RegisterClient` hívja fel a kódmentes regisztráció.

    A `sendPush` módszer felhívja a kódmentes szeretne elindítani egy biztonságos értesítésben, ahol a felhasználó a felhasználó címke alapján. A platform értesítési szolgáltatás `sendPush` célok attól függ, a `pns` átadott karakterlánc.

10. Az a `MainActivity` osztály, frissítse a `sendNotificationButtonOnClick` módszer, ha fel szeretne hívni a `sendPush` módszer a felhasználó a következőképpen kijelölt platform értesítési szolgáltatások.

        /**
         * Send Notification button click handler. This method sends the push notification
         * message to each platform selected.
         *
         * @param v The view
         */
        public void sendNotificationButtonOnClick(View v)
                throws ClientProtocolException, IOException {

            String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                    .getText().toString();
            String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                    .getText().toString();

            // JSON String
            nhMessage = "\"" + nhMessage + "\"";

            if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
            {
                sendPush("wns", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
            {
                sendPush("gcm", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
            {
                sendPush("apns", nhMessageTag, nhMessage);
            }
        }



## <a name="run-the-application"></a>Futtassa az alkalmazást


1. Futtassa az alkalmazást egy eszközt, vagy egy irányító Android Studio segítségével.

2. Az Android alkalmazásban adja meg a felhasználónevet és jelszót. Mindkét feleljen meg az azonos karakterláncérték, és ezek nem tartalmazhat szóközt és különleges karaktereket.

3. Az Android alkalmazásban kattintson a **Bejelentkezés**elemre. Várakozás az értesítőben szereplő üzenet arról, hogy **a naplózott és regisztrált**. Ezzel engedélyezi az **Értesítés küldése** gombra.

    ![][A2]

4. Kattintson a váltása gombokkal ahhoz, hogy hol van az összes platformokon az alkalmazás és a felhasználó regisztrált.
5. Írja be a felhasználó nevét, amely a értesítést kap. A cél eszközökön értesítések regisztrálni kell a felhasználó számára.

6. Adjon meg egy üzenetet a felhasználó számára, mint a leküldéses értesítést kap.
7. Kattintson az **Értesítés küldése**gombra.  Egyes eszközök, amelynek az egyező felhasználónév címkével ellátott regisztráció a leküldéses értesítést kap.


[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
