<properties
    pageTitle="Az Azure értesítés hubok küldő biztonságos leküldéses értesítések"
    description="További információ az Azure biztonságos leküldéses értesítéseket küldeni az Android-alkalmazásokban. Java a és C# nyelven íródott mintakódok."
    documentationCenter="android"
    keywords="leküldéses értesítések leküldéses értesítéseket, az üzenetek android leküldéses értesítéseket leküldéses"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Az Azure értesítés hubok küldő biztonságos leküldéses értesítések

> [AZURE.SELECTOR]
- [A Windows univerzális](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-alapú](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>– Áttekintés

> [AZURE.IMPORTANT] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Leküldéses értesítést támogatása a Microsoft Azure lehetővé teszi, hogy az üzenet nagyban egyszerűsíti a leküldéses értesítések egyszerre fogyasztói és vállalati alkalmazások mobil platformokon könnyen használható, többplatformos, amelyekkel méretarányos telepítés leküldéses infrastruktúra érhető el.

Szabályozási miatt vagy biztonsági kényszerek, előfordul, hogy az alkalmazás előfordulhat, hogy szeretne foglalni valamit, amit az ablakokat, amelyek nem kell továbbítani a szabványos leküldéses értesítést infrastruktúra keresztül. Ebben az oktatóanyagban az ugyanazon élmény elérése küld a bizalmas információkat az ügyfél Android-eszközön, és az alkalmazás kódmentes közötti biztonságos, hitelesített kapcsolaton keresztül ismerteti.

Magas szintű a folyamat a következőképpen történik:

1. Az alkalmazás háttéradatbázist:
    - Tárolók biztonságos tartalom háttéradatbázist.
    - Ez az értesítés azonosítója küld az Android-készülék (nem biztonságos küld adatokat a).
2. Az alkalmazás az eszközön, amikor a értesítést kap:
    - Az Android-készülék lép kapcsolatba a háttéradatbázist kérése a biztonságos tartalom.
    - Az alkalmazás megjelenítheti a tartalom, az eszközön értesítést.

Fontos tudni, hogy a fenti folyamat (és ebben az oktatóanyagban) feltételezzük, hogy az eszköz tárol egy hitelesítési jogkivonat helyi tároló után a felhasználó bejelentkezik a. Ez a teljesen zökkenőmentes változat, garantálja, mint az eszközre az értesítés biztonságos tartalom a token használatával meghallgathatja. Ha az alkalmazás nem tárolja hitelesítési tokenek az eszközre, vagy ha tokenek lejárt az eszköz alkalmazást, a leküldéses értesítést kap megjelenjen-e egy általános értesítést a felhasználótól indítsa el az alkalmazást. Az alkalmazás hitelesíti a felhasználót, majd az értesítési tartalom jeleníti meg.

Ebből az oktatóanyagból megtudhatja, hogy miként küldhet biztonságos leküldéses értesítéseket. A [Felhasználók értesítést](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) oktatóprogram, célszerű lépések elvégzése, hogy az oktatóanyagban először Ha még nem tette meg épül.

> [AZURE.NOTE] Ebben az oktatóanyagban feltételezi, hogy létrehozása és beállítása a értesítés központi, [Értesítés hubok (Android) – első lépések](notification-hubs-android-push-notification-google-gcm-get-started.md)leírt módon.

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Az Android projekt módosítása

Most, hogy az alkalmazás háttéradatbázist csak *azonosító* leküldéses értesítést küldhet módosította, akkor módosítsa az Android alkalmazását az értesítés kezelni, visszahívhatja a háttéradatbázist beolvasni a biztonságos üzenet jelenik meg.
A cél elérését, akkor győződjön meg arról, hogy az Android alkalmazást tudja, hogyan és hitelesítse magát, a háttéradatbázist, amikor a leküldéses értesítést kap.

A Microsoft most módosítja a *bejelentkezési* folyamat annak érdekében, hogy mentse a hitelesítési élőfej értékének megosztott az alkalmazás beállításai. Hasonló mechanizmusok bármely hitelesítési jogkivonat (pl. OAuth tokenekre), amely az alkalmazást kell használnia, anélkül, hogy felhasználói hitelesítő adatokat tároló használható.

1. A Android alkalmazás projektben adja hozzá az alábbi állandók az **MainActivity** osztály tetején:

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Továbbra is a **MainActivity** osztály a frissíti a `getAuthorizationHeader()` használatával is tartalmaz, a következő kódot:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Adja hozzá a következő `import` kimutatások **MainActivity** fájl tetején:

        import android.content.SharedPreferences;

Most már azt, a kezelő, amikor az értesítés érkezik meghívott változik.

4. A **MyHandler** az osztályjegyzetfüzet-módosítása a `OnReceive()` használatával is tartalmazza:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Vegye fel a `retrieveNotification()` módszer, a helyőrző helyett `{back-end endpoint}` a háttéradatbázist végponttal kapott üzembe helyezése a háttéradatbázist közben:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Ez a módszer felhívja a alkalmazás háttéradatbázist a értesítés tartalomhoz megosztott a Preferences tárolt hitelesítő adatok beolvasásához, és szokásos értesítés formájában jeleníti meg. Az értesítés az alkalmazás felhasználónak pontosan például minden más leküldéses értesítést formátumban.

Ne feledje, hogy hiányzó hitelesítési élőfej tulajdonság, illetve elutasításra eseteit kezelheti a háttéradatbázist által előnyben részesíteni. Ezekben az esetekben az adott kezelését attól függenek, főleg a cél felhasználói felület. Egy, hogy értesítés megjelenítése egy általános rákérdező hitelesítést végezni a felhasználó számára beolvasása a tényleges értesítés.

## <a name="run-the-application"></a>Futtassa az alkalmazást

Az alkalmazás futtatásához tegye a következőket:

1. Ellenőrizze, hogy telepítve van a **AppBackend** Azure-ban. Ha használja a Visual Studióban, a **AppBackend** webes API alkalmazásnak a futtatására. ASP.NET-weblap jelenik meg.

2. Holdas futtassa az alkalmazást a fizikai Android-eszközzel vagy a irányító.

3. Felhasználói felület az Android alkalmazásban adja meg a felhasználónevet és jelszót. Ezek a karakterlánc lehet, de ugyanazt az értéket kell lenniük.

4. Felhasználói felület az Android alkalmazásban kattintson a **Bejelentkezés**. Kattintson a **Küldés a leküldéses**.
