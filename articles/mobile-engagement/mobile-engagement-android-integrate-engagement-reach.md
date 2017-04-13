<properties
    pageTitle="Azure mobil tetszés szerint elmélyedhet Android SDK integrációja"
    description="Legújabb frissítésekkel és Azure Mobile tetszés szerint elmélyedhet Android SDK eljárások"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Hogyan integrálása tetszés szerint elmélyedhet vannak Android-eszközön

> [AZURE.IMPORTANT] Követniük kell a integrációs leírtakat tetszés szerint elmélyedhet integrálni szeretné a hogyan Android dokumentum Ez az útmutató következő előtt.

##<a name="standard-integration"></a>Szabványos integrációja

A elérjen SDK csomagjában talál van szükség az **Android támogatási library (v4)**.

A leggyorsabb módja a tár hozzáadása a projekthez, a **Holdas** `Right click on your project -> Android Tools -> Add Support Library...`.

Ha Ön nem használja az Holdas, erről a képernyőn megjelenő utasításokat [Itt].

A projektben a SDK vannak erőforrásfájl másolja.

-   Fájlok másolása a `res/layout` mappa szállított a SDK be a `res/layout` mappát az alkalmazás.
-   Fájlok másolása a `res/drawable` mappa szállított a SDK be a `res/drawable` mappát az alkalmazás.

Szerkesztés a `AndroidManifest.xml` fájl:

-   Az alábbi szakasz hozzáadása (között a `<application>` és `</application>` címkék):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Ezzel az engedéllyel ismétlődő lejátszásra van állítva, amely nem rendszerindításkor rendszerüzenetek szükséges (ellenkező folyamatosan lemezen, de nem eltűnt jelenik meg, valójában kell tartalmazzák a).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Adja meg az ikon értesítések (mind az alkalmazás és a rendszer egyesre) másolásakor és szerkesztésekor a következő szakasz által használt (között a `<application>` és `</application>` címkék):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Ez a szakasz **kötelező** , ha vannak kampányok létrehozásakor használatáról a rendszer értesítést. Android megakadályozza, hogy a rendszer értesítés nélkül ikonok alatt látható. Így ez a szakasz elhagyása esetén a végfelhasználók nem lesznek érkeznek őket.

-   Nagy kép használata rendszerüzenetek kampányok hoz létre, ha, hogy fel kell vennie a következő engedélyeket (után a `</application>` címke) Ha hiányzó:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Android M és a Ha az alkalmazás célként 23 vagy újabb Android API szint ``WRITE_EXTERNAL_STORAGE`` engedéllyel a felhasználói jóváhagyást igényel. Olvassa el [Ebben a szakaszban](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   A rendszer értesítések akkor is megadhatja a gyermekektől marketingkampány, ha az eszköz kell csengetés és/vagy a rezgést. Ahhoz, hogy működik, ellenőrizze, hogy, a következő engedélyt deklarálva van (után a `</application>` címke):

            <uses-permission android:name="android.permission.VIBRATE" />

    Ezzel az engedéllyel nélkül Android megakadályozza, hogy a rendszer értesítést nem látható, ha a gyűrű vagy a elérjen marketingkampány-kezelő az vibrate választógombot bejelölve.

-   Ha **ProGuard** segítségével az alkalmazás összeállítása és az Android támogatási tár vagy a tevékenységek üveg kapcsolatos hibákat, és adja meg a következő sorokat a `proguard.cfg` fájl:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Natív leküldéses

Most, hogy úgy állította be a modul vannak, akkor konfigurálnia kell natív leküldéses engedélyezni szeretné kapni a kampányok az eszközön.

Két szolgáltatás támogatjuk az Android-eszközön:

  - A Google Play eszközök: használja [A Google Cloud üzenetküldés] a [hogyan integráció GCM tetszés szerint elmélyedhet kalauz az](mobile-engagement-android-gcm-integrate.md) útmutató követve.
  - Amazon eszközök: használata [Amazon eszköz üzenetküldés] a [hogyan integráció ADM tetszés szerint elmélyedhet kalauz az](mobile-engagement-android-adm-integrate.md) útmutató követve.

Ha azt szeretné, célba Amazon és a Google Play eszközök, lehetséges, hogy az minden fejlesztési 1 AndroidManifest.xml/APK belül. De Amazon elküldésekor azok elutasíthatja az alkalmazás Ha az azok GCM kód megkeresése.

Ebben az esetben több APKs kell használni.

**Az alkalmazás készen áll a vezérlőn és vannak kampányok megjelenítése!**

##<a name="how-to-handle-data-push"></a>Adatok leküldéses kezelése

### <a name="integration"></a>Integráció

Ha engedélyezni szeretné a vannak adatok veremmutatót kapni az alkalmazás, az almenüből osztály létrehozására van `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` és a hivatkozást a `AndroidManifest.xml` fájl (között a `<application>` és/vagy `</application>` címkék):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Hagyhatja, majd a `onDataPushStringReceived` és `onDataPushBase64Received` visszahívást. Lássunk egy példát:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategória

A kategória paraméter egy adatok leküldéses marketingkampány létrehozásakor a lépés nem kötelező, és lehetővé teszi, hogy Ön szűrheti az adatokat verembe küldi. Ez akkor hasznos, ha több közvetítési vevők kezelése a különböző típusú adatok veremmutatót, vagy ha különböző leküldéses szeretne a `Base64` adatok és a típusának azonosítása elemzése őket előtt szeretné.

### <a name="callbacks-return-parameter"></a>A visszahívás visszatérési paramétere

Az alábbiakban útmutatást olvashat megfelelő a visszaadott paramétere kezeléséhez `onDataPushStringReceived` és `onDataPushBase64Received`:

-   Térjen vissza a közvetített fogadó `null` a visszahívás, ha nem tudja a leküldéses adatok kezelése a. A kategória határozza meg, hogy a közvetített címzett kezelje az adatok leküldéses, vagy nem kell használni.
-   Térjen vissza a közvetített címzett egyik `true` a visszahívás, ha az adatok leküldéses elfogadja a.
-   Térjen vissza a közvetített címzett egyik `false` a visszahívás, ha az adatok leküldéses felismer, de valamilyen okból figyelmen kívül hagyja a. Például visszatérési `false` érvénytelen kapott adatok esetén.
-   Ha egyik küldi el címzett eredménye `true` miközben egy másik egyikét adja vissza `false` azonos adatokat leküldéses, nem meghatározott működése, kell soha ne az ehhez szükséges lépéseket.

A visszatérési típus csak a gyermekektől statisztika használható:

-   `Replied`a közvetítési vevők egyikét adja vissza, vagy ha minden `true` vagy `false`.
-   `Actioned`csak akkor, ha a szórás vevők egyikét adja vissza az eggyel `true`.

##<a name="how-to-customize-campaigns"></a>Kampányok testreszabása

Kampányok testreszabásához az elrendezéseket a elérjen SDK leírt módosíthatók.

Kell tartani az elrendezések használt összes azonosító, és a nézettípusok azonosítót, különösen a kép és szöveg nézeteket használó megtartása. Egyes nézetek csak szolgálnak elrejtése vagy megjelenítése a területeket, így azok típus módosítható. Ha azt szeretné, a megadott elrendezés nézet módosítása, ellenőrizze a forráskódot.

### <a name="notifications"></a>Értesítések

Értesítés két típusa van: rendszer és az alkalmazás értesítésekre, amelyek különböző elrendezés fájlok használata.

#### <a name="system-notifications"></a>Rendszerüzenetek

Ha testre szeretné szabni a rendszer értesítéseit, akkor használja a **Kategóriák**. A [Kategóriák](#categories)ugorhat.

#### <a name="in-app-notifications"></a>Az alkalmazás

Alapértelmezés szerint ki az alkalmazás értesítjük dinamikusan hozzáadott a tevékenység aktuális felhasználói felület az Android módszer több nézet `addContentView()`. Egy értesítés átfedő Link. Átfedi értesítési kiválóan alkalmasak a gyors integráció, mert nem igényel, hogy az alkalmazás bármely elrendezésének módosítása.

Az értesítés átfedi megjelenésének módosításához egyszerűen módosíthatja a fájlt `engagement_notification_area.xml` igényeinek.

> [AZURE.NOTE] A fájl `engagement_notification_overlay.xml` egy értesítés átfedő létrehozásához használt egyetlen tartalmazza a fájl `engagement_notification_area.xml`. Testre is szabhatja azt az igényeinek megfelelően (például mint elhelyezése az értesítési területen belül az átfedő).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Egy tevékenység elrendezés részeként értesítés elrendezés tartalmazza.

Átfedi kiválóan alkalmasak a gyors integráció, de lehetnek kényelmesen vagy bizonyos esetekben egymás hatással. Az átfedő rendszer testre szabható egy tevékenység szinten, így egyszerűen speciális tevékenységek hatásai megakadályozása.

Eldöntheti, hogy a meglévő elrendezéseket a **tartalmazzák** az Android utasítás több az értesítési elrendezés felvenni. Az alábbi képen egy módosított `ListActivity` elrendezés tartalmazó csak egy `ListView`.

**Mielőtt tetszés szerint elmélyedhet integráció:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Tetszés szerint elmélyedhet integráció: után**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

Ebben a példában jelöltük szülő tároló óta használt listanézet az eredeti elrendezést a legfelső szintű elemet. Is hozzáadott `android:layout_weight="1"` engedélyezni szeretné a beállított listanézet alatti nézet `android:layout_height="fill_parent"`.

A tevékenységek elérjen SDK automatikusan észleli, hogy az értesítési elrendezést a tevékenység szerepel-e, és nem vesz fel egy további a tevékenységhez.

> [AZURE.TIP] Egy ListActivity az alkalmazás használatakor egy látható vannak átfedő nem teszik lehetővé a lista nézetben lévő elemek már rákattintott reagálni. Az ismert probléma. Ez a probléma megoldásához javasoljuk, hogy az értesítési elrendezés beágyazása saját lista tevékenység elrendezés, mint az előző minta.

##### <a name="disabling-application-notification-per-activity"></a>Alkalmazás értesítést / tevékenység letiltása

Ha nem szeretné, hogy az átfedő felvenni kívánt a tevékenységét, és ha nem az értesítési elrendezés be saját elrendezését, az ezt a tevékenységhez átfedés letilthatja a `AndroidManifest.xml` hozzáadásával egy `meta-data` szakasz, mint az alábbi példában:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>A kategóriák

Ha módosítja a megadott elrendezések, minden értesítést megjelenését módosíthatja. Kategóriák definiálása különböző célzott látványtervek (valószínűleg viselkedése) az értesítésekhez teszi lehetővé. Kategória egy vannak marketingkampány létrehozásakor adható meg. Ne feledje, hogy a kategóriák segítségével is testre szabhatja a hirdetmények és szavazásokra, a jelen dokumentum ismertetett.

Az értesítések kategória leíró regisztrálásához kell adja hozzá a hívást, ha az alkalmazás inicializált van.

> [AZURE.IMPORTANT] Olvassa el a figyelmeztetést az Android-alapú: folyamat attribútum \<android-sdk-előjegyzéssel-process\> való integráció részvételét a továbblépés előtt Android témakör hogyan.

Ez a példa feltételezi, hogy ismerni az előző figyelmeztetés, és használja az almenüből osztály `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

A `MyNotifier` objektum a értesítés kategória kezelő végrehajtását. Bármelyik megvalósítása a `EngagementNotifier` felület vagy az alapértelmezett végrehajtásának sub osztály: `EngagementDefaultNotifier`.

Tartsa szem előtt, hogy az azonos bejelentő számos kategóriába képes kezelni, rögzítheti őket jelennek meg:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Az alapértelmezett kategória megvalósítás cserélni, regisztrálhat a példányába, például az alábbi példában:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Az aktuális kategória egy kezelő használt felülbírálhatja a legtöbb módszer paraméterként átadott `EngagementDefaultNotifier`.

Van haladt akár egy `String` paraméter vagy közvetve egy `EngagementReachContent` objektumra, amely mellett egy `getCategory()` módot.

Módosíthatja a értesítés létrehozásának folyamata a legtöbb újradefiniálás módszerek a `EngagementDefaultNotifier`, további lehetőségein nyugodtan nézze meg a műszaki dokumentáció andras@contoso.com forráskódot.

##### <a name="in-app-notifications"></a>Az alkalmazás

Ha csak szeretne váltakozó elrendezések használható egy adott kategóriát, alkalmazhat Ez az alábbi példának megfelelően:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Példa `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Amint látható, az átfedő nézetben azonosító eltér a szabványos egy. Fontos, hogy egyes elrendezések átfedi használja egy egyedi azonosítót.

**Példa `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Amint látható, az értesítési területen nézet azonosító eltér a szabványos egy. Fontos, hogy egyes elrendezések használja-e az értesítési területen egy egyedi azonosítót.

Ez a kategória egyszerű példa lehetővé teszi az alkalmazás (vagy az alkalmazás) értesítések, a képernyő felső részén látható. Azt nem módosította az értesítési területen, saját használt szabványos azonosítóját.

Módosíthatja, hogy szeretne, esetén újra a `EngagementDefaultNotifier.prepareInAppArea` módot. Keresse meg a műszaki dokumentáció andras@contoso.com forráskódot az ajánlott `EngagementNotifier` és `EngagementDefaultNotifier` Ha azt szeretné, ez lehetőségein szintje.

##### <a name="system-notifications"></a>Rendszerüzenetek

Időtartamának `EngagementDefaultNotifier`, felülbírálhatja `onNotificationPrepared` megváltoztathatja az ablakokat, amelyek az alapértelmezett megvalósítása készült.

Példa:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Ebben a példában a "folyamatos" kategória segítségével folyamatban lévő esemény jelenik meg tartalom rendszer értesítést teszi.

Ha össze szeretne a `Notification` objektum bármikor visszatérhet az elejétől, `false` módszer és hívás `notify` a saját maga a `NotificationManager`. Ebben az esetben fontos, hogy őrizze meg egy `contentIntent`, egy `deleteIntent` és az értesítési azonosító által használt `EngagementReachReceiver`.

Íme egy megfelelő példa egy végrehajtása:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Értesítés csak hirdetmények

Kattintson a csak hirdetmények testre szabható által felülbírálása értesítést irányításának `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` módosítása az elkészített `Intent`. Ezzel a módszerrel lehetővé teszi a jelölők egyszerűen finomhangolása.

Példa hozzáadása a `SINGLE_TOP` jelző:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

A régi tetszés szerint elmélyedhet felhasználók Felhívjuk a figyelmét arra, hogy rendszerüzenetek nélkül művelet URL-cím most elindítja az alkalmazást, ha volt a háttérben fut, így ez a módszer hívható művelet URL-címe nélkül tartalmat. Vegye figyelembe, hogy a szándék elérését segítő testreszabásakor.

Is alkalmazhat `EngagementNotifier.executeNotifAnnouncementAction` nulláról.

##### <a name="notification-life-cycle"></a>Értesítés életciklus

Az alapértelmezett kategória használatakor egyes életciklusának módszerek felkért a `EngagementReachInteractiveContent` jelentés statisztika az objektum, és frissítse a marketingkampány állapot:

-   Ha az értesítés jelenik meg az alkalmazás vagy helyezze az állapotsorban a `displayNotification` módszer neve (amely a statisztikai jelentések) szerint `EngagementReachAgent` Ha `handleNotification` adja eredményül `true`.
-   Ha meg van nyitva az értesítésre, a `exitNotification` módszer neve, statisztikai jelentést, és a következő kampányok most feldolgozhatók.
-   Ha az értesítés kattintott, `actionNotification` van neve, statisztikai jelenteni, és szándékának kapcsolódó elindul.

Ha a példányába `EngagementNotifier` figyelmen kívül hagyja a az alapértelmezett működés életciklusának módszerekben hívásakor saját maga által szükséges. Az alábbi példák bemutatják, bizonyos esetekben, ahol az alapértelmezett működés elmarad:

-   Kijelölés méretének növelése nem `EngagementDefaultNotifier`, például kategória kezelése az alapoktól szerepelni fog.
-   A rendszer értesítést, overrode a `onNotificationPrepared` és az Ön által módosított `contentIntent` vagy `deleteIntent` a a `Notification` objektum.
-   Az alkalmazás az értesítésekhez overrode `prepareInAppArea`, ne felejtse el a térkép legalább `actionNotification` az egyik a U.I szabályozza.

> [AZURE.NOTE] Ha `handleNotification` eredményez, a program törli a kivétel, a tartalom és `dropContent` nevezik. A jelentett statisztikai, és most is dolgozza fel a következő kampányok.

### <a name="announcements-and-polls"></a>Hirdetmények és szavazásokra

#### <a name="layouts"></a>Elrendezések

Módosíthatja a `engagement_text_announcement.xml`, `engagement_web_announcement.xml` és `engagement_poll.xml` fájlok szöveg hirdetmények, a webhely hirdetmények és a szavazásokra testreszabásához.

Ezeket a fájlokat oszthat meg a cím és a gomb terület két gyakori elrendezések. Az elrendezés, a cím `engagement_content_title.xml` és a városról drawable fájlt használja a háttérben. Az elrendezés, a művelet- és kilépési gombok `engagement_button_bar.xml` és a városról drawable fájlt használja a háttérben.

A szavazás, a kérdés elrendezés és a választási lehetőségek vannak dinamikusan felfújni többször használatával a `engagement_question.xml` elrendezés fájl azokra a kérdésekre, és a `engagement_choice.xml` választási fájlt.

#### <a name="categories"></a>A kategóriák

##### <a name="alternate-layouts"></a>Alternatív elrendezések

Értesítések, például a marketingkampány kategória, hogy az alternatív elrendezések a hirdetmények és szavazásokra használható.

Ha például a szöveg közlemény kategória létrehozásához bővítheti `EngagementTextAnnouncementActivity` hivatkoznak rá, és a `AndroidManifest.xml` fájl:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Figyelje meg, hogy a kategória a leképezési szűrés a különbség az alapértelmezett bejelentése tevékenységgel létesítéséhez használatos.

A elérjen SDK leképezési rendszert használ, a jobb oldali tevékenység egy adott kategória feloldásához és esik vissza az alapértelmezett kategória, ha a megoldás sikertelen volt.

Majd végrehajtásához van `MyCustomTextAnnouncementActivity`, csak szeretne elrendezésének módosítása (de meg szeretné tartani az azonos nézet azonosítók), csak esetén például az osztály határozhat meg a következő példa:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Az alapértelmezett kategória a szöveg hirdetmények cseréjéhez egyszerűen cseréje `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` a megvalósítás.

Webhely hirdetmények és szavazásokra hasonló módon testre szabható.

A webhely hirdetmények kiterjesztése `EngagementWebAnnouncementActivity` és a tevékenység a `AndroidManifest.xml` , mint az alábbi példában:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

A szavazásokra kiterjesztése `EngagementPollActivity` történő deklarálását és a a a `AndroidManifest.xml` , mint az alábbi példában:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Bevezetés az alapoktól

Alkalmazhat kategóriák a bejelentése (és a szavazás) tevékenységek valamelyikét bővítése nélkül a `Engagement*Activity` a elérjen SDK által biztosított osztályok. Ez akkor hasznos, ha szeretne összeállítani, mely nem használja az azonos nézetek, a normál elrendezések elrendezés.

Hasonló speciális értesítés testreszabáshoz ajánlott tekintse meg a forráskódot szabványos végrehajtása.

Íme néhány dolog, amit érdemes észben tartania: vannak elindítja a tevékenység az egy adott leképezés (a leképezési szűrő megfelelő) plusz egy további paramétert, amely a tartalom azonosítója.

A tartalom objektumot tartalmazó mezőket az marketingkampány létrehozása a webhelyen a megadott beolvasásához ezt megteheti:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Statisztikai, jelentse a tartalom jelenik meg a `onResume` esemény:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Ezután, ne felejtse el vagy hívás `actionContent(this)` vagy `exitContent(this)` a tevékenység működésbe háttér előtt tartalom objektumon.

Nem hívásakor vagy `actionContent` vagy `exitContent`, statisztikai adatokat nem küldi (tehát nem analytics a a marketingkampány) és a fontosabb, a következő kampányok nem jelenik meg értesítés az alkalmazás folyamat újraindításáig.

Tájolás vagy más konfigurációs módosítást végezhet a kód körlevelekben határozza meg, hogy a tevékenység működésbe háttér-e, illetve a szabványos végrehajtása ellenőrzi, a tartalom készként való kilép, ha a felhasználó elhagyja a tevékenység (vagy billentyűkombináció lenyomásával `HOME` vagy `BACK`), de nem, ha megváltozik a tájolását.

Az alábbiakban a végrehajtása érdekes részét:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Amint látható, ha a hívott `actionContent(this)` , majd a tevékenység elkészült `exitContent(this)` biztonságosan hívható anélkül, hogy semmilyen hatása.

[itt]:http://developer.android.com/tools/extras/support-library.html#Downloading
[A Google Cloud üzenetküldés]:http://developer.android.com/guide/google/gcm/index.html
[Amazon eszköz üzenetküldés]:https://developer.amazon.com/sdk/adm.html
