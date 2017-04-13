<properties
    pageTitle="Értesítés hubok megszakítása hírek oktatóanyag – Android"
    description="Megtudhatja, hogy miként küldhet Azure Service Bus értesítés hubok legfrissebb híreket értesítések a Android-eszközön."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Legfrissebb hírek küldése értesítés hubok használatával

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan Azure értesítés hubok használatával közvetítése legfrissebb híreket értesítések Android alkalmazást. Amikor elkészült, használni tudja regisztrálni érdeklik hírek kategóriák megszakítása, és csak a kategóriákhoz tartozó leküldéses értesítések fogadása. Ebben az esetben az sok alkalmazások közös mintájának, ahol értesítések kell el lehet küldeni a csoportok a felhasználók, amely korábban már van deklarálva érdekes őket, például az RSS-olvasó, az alkalmazások zenét ventillátortól stb.

Egy vagy több _címkék_ felvételével, az értesítési-központban regisztráció létrehozásakor közvetítési esetek engedélyezett. Értesítések fogadására egy címkét, ha az összes eszközök regisztrálta a címke a értesítést kap. Mivel a címkék egyszerűen karakterláncok, nem rendelkeznek, hogy előre be kell építenie. Címkékkel kapcsolatos további tudnivalókért olvassa el az [értesítési hubok Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Előfeltételek

Ez a témakör az alkalmazást, az [első lépések az értesítési hubok]létrehozott épül[get-started]. Ebben az oktatóanyagban indításához kell már befejezése [első lépések az értesítési hubok][get-started].

##<a name="add-category-selection-to-the-app"></a>Az alkalmazás hozzáadása a kategória kiválasztása

Első lépésként a Felhasználóifelület-elemek hozzáadása, amelyek lehetővé teszik a felhasználó számára, válassza a kategóriák regisztrálhatja a meglévő fő tevékenységét. A kategóriák a felhasználó által választott tárolják az eszközön. Az alkalmazás indításakor eszköz regisztráció címkékként a értesítés központ a kijelölt kategóriával jön létre.

1. Nyissa meg a res/layout/activity_main.xml fájlt, és a tartalom, a következő helyettesítő:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Nyissa meg a res/values/strings.xml fájlt, és adja hozzá a következő sort:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    A main_activity.xml grafikus elrendezés így most néz ki:

    ![][A1]

3. Most már létrehozhat **értesítések** osztály a **MainActivity** osztály egy csomagban.

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Az osztály a helyi tároló hírek, ez az eszköz tartalmazó fogadásához kategóriáinak tárolására használja. Ezekben a kategóriákban regisztrálni módszereket is tartalmaz.


4. Az **MainActivity** osztályához távolítsa el a saját mezők **NotificationHub** és **GoogleCloudMessaging**, és a mező felvétele **az értesítésekhez**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Ezután a **onCreate** módszer távolítsa el a **központi** mező és a **registerWithNotificationHubs** módszer a inicializálni. Ezután adja hozzá a következő vonalak, amelyek az **értesítések** osztály egy példányának inicializálni. 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`és `HubListenConnectionString` már meg kell a a `<hub name>` és `<connection string with listen access>` helyőrzők az értesítési központi nevű, és a kapcsolati karakterláncot az *DefaultListenSharedAccessSignature* korábbi szerezte be.

    > [AZURE.NOTE] Mivel a hitelesítő adatait, amelyek egy ügyfél alkalmazással nem általában biztonságos, csak kell terjesztése meghallgatását access kulcsa az ügyfél-alkalmazást. Meghallgathatja access lehetővé teszi, hogy az értesítések, de a meglévő regisztrációk regisztrálhatja az alkalmazás nem módosíthatók, és nem lehet küldeni a értesítések. A teljes hívóbetű értesítések küldésére, és módosítása meglévő regisztrációk védett kódmentes szolgáltatás használják.


6. Adja hozzá a következő import és `subscribe` módszer kezelése az előfizetés gombra az esemény gombra:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Ez a módszer hoz létre a kategóriák listájában, és az **értesítések** osztály használja a helyi tároló tárolni a listában, és a megfelelő címkék regisztrálása az értesítési központi. A kategóriák megváltozásakor a rendszer az új kategóriák újra a regisztráció.

Az alkalmazás már kategóriák halmazának tárolását helyi tároló az eszközre, és az értesítési-központban regisztrálása, valahányszor a felhasználó megváltoztatja a kijelölés kategóriát.

##<a name="register-for-notifications"></a>Regisztrálás az értesítésekhez

Ezeket a lépéseket az értesítési-központban helyi tároló tárolt a kategóriák használata indításkor regisztrálása.

> [AZURE.NOTE] A hozzárendelt által a Google Cloud üzenetküldés (GCM) registrationId bármikor módosíthatja, mert regisztrálnia kell gyakran az értesítési hibák elkerülése érdekében az értesítésekhez. Ez a példa regisztrál az értesítés, valahányszor elindítja az alkalmazást. Gyakran futtatott alkalmazásai többször nap, valószínűleg kihagyhatja regisztrációs sávszélesség megőrzése, ha az előző regisztrációhoz óta eltelt kisebb, mint nap.


1. A következő kód hozzáadása a **onCreate** módszer a **MainActivity** osztály végén:

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Ezzel biztosíthatja, hogy minden alkalommal, amikor elindítja az alkalmazást, a kategóriák helyi tároló és az ezekbe a kategóriákba tartozó egy registeration kéri. 

2. Frissítse a `onStart()` módszer a `MainActivity` osztály az alábbi képlettel történik:

    @Overridea védett érvénytelenítése onStart() {super.onStart();      isVisible = igaz;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Ezzel frissíti a fő tevékenysége korábban mentett kategóriák állapotának alapján.

Az alkalmazás elkészült, és tárolhatja a kategóriák halmazának regisztrálása az értesítési-központban, a kategóriák kijelölés megváltozásakor használt eszköz helyi tárolására. Ezután azt határozza meg, hogy a kategória értesítéseket küldheti el ehhez az alkalmazáshoz a háttér-kiszolgálói.

##<a name="sending-tagged-notifications"></a>Címkézett értesítések küldése

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Futtassa az alkalmazást, és értesítéseket

1. Android Studio az alkalmazás összeállítása és kezdése az eszköz vagy irányító.

    Jegyzet, hogy az alkalmazás felhasználói felület biztosít be-és kikapcsolása, amellyel válassza a kategóriák előfizetéséhez.

2. Egy vagy több kategóriák megjelenítése és elrejtése engedélyezése, majd kattintson az **előfizetés**gombra.

    Az alkalmazás a kijelölt kategóriák konvertálja a címkék, és a kijelölt címkék új eszköz regisztráció kéri a értesítés központból. A bejegyzett kategóriák adja vissza, és egy bejelentési értesítés jelenik meg.

4. Új értesítés küldése a .NET-konzol app futtatásával.  Azt is megteheti használja az értesítés központi hibakeresési lapot az [Azure klasszikus portál]címkézett sablon értesítést küldhet.

    A kijelölt kategória értesítések bejelentési értesítés jelenik meg.

##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban legfrissebb híreket közvetítésével kategória szerint megtanulta azt. Vegye figyelembe az alábbi oktatóanyagok más speciális értesítés hubok esetben kiemelő egyik befejezése:

+ [Értesítés hubok segítségével honosított legfrissebb híreket közvetítése]

    Megtudhatja, hogy miként bontsa ki a legfrissebb híreket alkalmazás küldő honosított értesítések engedélyezése.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Értesítés hubok segítségével honosított legfrissebb híreket közvetítése]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure klasszikus portál]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
