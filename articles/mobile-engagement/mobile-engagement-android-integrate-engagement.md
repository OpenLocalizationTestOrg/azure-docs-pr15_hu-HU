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

#<a name="how-to-integrate-engagement-on-android"></a>Hogyan integrálása tetszés szerint elmélyedhet Android-eszközön

> [AZURE.SELECTOR]
- [A Windows univerzális](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-alapú](mobile-engagement-android-integrate-engagement.md)

Ez az eljárás a legegyszerűbb módszer tetszés szerint elmélyedhet a analitikai és az Android alkalmazásban függvények figyelése aktiválása ismerteti.

> [AZURE.IMPORTANT] A minimális SDK API-val Android szintű kell lennie, 10 vagy újabb (Android 2.3.3 vagy újabb).

A következő lépések nem elég aktiválja a jelentés naplók szükség számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és Technicals kapcsolatos összes statisztikát. A jelentés naplók szükség más statisztikai adatokat, például eseményeket, a hibák és a feladatok számítja ki kell végezni, manuálisan segítségével tetszés szerint elmélyedhet API-val (lásd: [a speciális Mobile tetszés szerint elmélyedhet API-t az Android-címkézés használatáról](mobile-engagement-android-use-engagement-api.md) , mivel ezeket a statisztikákat függő alkalmazás.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>A tevékenységek SDK és a szolgáltatás beágyazása a Android projekt

Töltse le a az Android SDK [Itt](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` , és helyezze át őket az a `libs` mappát a Android projekt (létrehozásához a könyvtárral lenne mappát, ha még nem létezik).

> [AZURE.IMPORTANT]
> A alkalmazáscsomag a ProGuard hoz létre, ha szeretne rögzíteni néhány osztályok. A következő konfigurációs kódtöredékének használható:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Adja meg a tetszés szerint elmélyedhet kapcsolati karakterlánc megnyitó ikon a tevékenységben hívja fel az alábbi módon:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

A kapcsolati karakterláncot, az alkalmazás a Azure portálon jelenik meg.

-   Ha hiányzik, az alábbi Android-engedélyek hozzáadása (előtt a `<application>` címke):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Az alábbi szakasz hozzáadása (között a `<application>` és `</application>` címkék):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Módosítás `<Your application name>` szerint az alkalmazás nevét.

> [AZURE.TIP] A `android:label` attribútum lehetővé teszi a válassza ki a nevét a tetszés szerint elmélyedhet szolgáltatás, a végfelhasználók a "Services szolgáltatást futtató" a telefon képernyőjén megjelenő. Az attribútum beállításához ajánlott `"<Your application name>Service"` (pl. `"AcmeFunGameService"`).

Adja meg a `android:process` attribútum biztosítja, hogy a tevékenységek szolgáltatás fut, a saját folyamat (futtató tetszés szerint elmélyedhet ugyanezt az eljárást, az alkalmazás láthatóvá válik a fő/felhasználói felületi szálon esetleg kisebb válaszol).

> [AZURE.NOTE] Bármilyen kódot elhelyeztem `Application.onCreate()` és más alkalmazások visszahívást az alkalmazás folyamatok, beleértve a tetszés szerint elmélyedhet szolgáltatás fog futni. Előfordulhat, hogy a nem kívánt oldal effektusok (például a szükségtelen memória hozzárendelések és a beszélgetésekben a tetszés szerint elmélyedhet folyamat, ismétlődő közvetítési címzett vagy a szolgáltatások).

Ha Ön felülbírálása `Application.onCreate()`, adja hozzá a következő kódrészletet elején ajánlott a `Application.onCreate()` függvény:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Is ugyanezt a célt szolgálja `Application.onTerminate()`, `Application.onLowMemory()` és `Application.onConfigurationChanged(...)`.

Is bővítheti `EngagementApplication` helyett bővítése `Application`: a visszahívás `Application.onCreate()` jelent, a folyamat ellenőrzése és hívások `Application.onApplicationProcessCreate()` csak ha az aktuális folyamat nem az adott levő tetszés szerint elmélyedhet szolgáltatást, a többi visszahívást ugyanazokat a szabályokat alkalmazni.

##<a name="basic-reporting"></a>Egyszerű jelentés

### <a name="recommended-method-overload-your-activity-classes"></a>Ajánlott módszer: túlterhelés a `Activity` osztályok

A jelentés tetszés szerint elmélyedhet számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és technikai statisztika által igényelt összes naplók aktiválásához egyszerűen csak van, hogy az összes a `*Activity` alszint osztályok örökölhet a megfelelő `Engagement*Activity` osztályok (például ha a régebbi tevékenységét kiterjed `ListActivity`, kiterjed helyezése `EngagementListActivity`).

**Tetszés szerint elmélyedhet: nélkül**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**A tevékenységek:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Használata esetén `EngagementListActivity` vagy `EngagementExpandableListActivity`, győződjön meg arról, hogy bármelyik hívása `requestWindowFeature(...);` történik, mielőtt a hívást `super.onCreate(...);`, egyéb esetben e az összeomlást akkor fordul elő.

Ezek az osztályok megtalálhatja a `src` mappát, és bemásolhatja őket a projectbe. Az osztályok is szerepelnek a **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatív módszer: hívás `startActivity()` és `endActivity()` manuálisan

Ha nem szeretne a többszörös vagy nem a `Activity` osztályok, helyette indítása és a tevékenység befejezéséhez hívja fel `EngagementAgent`meg közvetlenül a módszereket.

> [AZURE.IMPORTANT] Az Android SDK soha nem felhívja a `endActivity()` módszer, akkor is, ha az alkalmazás be van zárva (Android-eszközön, alkalmazások nem ténylegesen soha nem zárul). Így célszerű *erősen* ajánlott, ha fel szeretne hívni a `startActivity()` módszer a `onResume` visszahívást az *összes* a tevékenységek, és a `endActivity()` módszer a `onPause()` *összes* a visszahívás a tevékenységek. Ez a csak úgy, hogy meggyőződhessen róla munkamenetek kiszivárogtatott nem lehet. A munkamenet kiszivárogtatott van, ha, a tetszés szerint elmélyedhet szolgáltatás a (mivel ez a szolgáltatás maradjon csatlakoztatott, mindaddig, amíg a munkamenet folyamatban) soha nem leválasztása a a tetszés szerint elmélyedhet kódmentes.

Lássunk egy példát:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

Ez a Példa nagyon hasonlít a `EngagementActivity` osztály és a változatok, amelynek forráskód van megadva, a `src` mappát.

##<a name="test"></a>Teszt

Most már ellenőrizze, hogy a integráció a mobilalkalmazásban futó irányító vagy eszközön, és hitelesítésével Monitor lapján a munkamenet regisztrálja.

A következő szakaszok nem kötelező.

##<a name="location-reporting"></a>Hely jelentése

Ha azt szeretné, hogy a helyekre kell jelenteni, konfiguráció néhány sorok hozzáadásához szüksége (között a `<application>` és `</application>` címkék).

### <a name="lazy-area-location-reporting"></a>Lusta terület hely jelentése

Lusta terület hely jelentéskészítés lehetővé teszi az ország, a terület és a kapcsolódó eszközök településen. A következő típusú hely jelentése csak az (a cella azonosító vagy WIFI alapuló) a hálózati helyeken használja. Munkamenet minden legfeljebb egyszer jelenteni az eszköz terület. Soha nem használják a GPS, és az így helyet a jelentés a következő típusú nagyon kevés (nem a fel, hogy nincs) az elem gyakorolt hatását.

A bejelentett területek számítja ki a felhasználókat, munkamenetek, események és hibák földrajzi statisztikájának szolgálnak. Is felhasználhatók vannak kampányok feltételként.

Lusta terület hely jelentéskészítés engedélyezéséhez is elvégezheti a konfiguráció korábban, hogy az eljárás használatával:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Meg kell hozzáadása a következő engedélyt, ha hiányzó:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Vagy, folytassa a ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazás.

### <a name="real-time-location-reporting"></a>Valós idejű hely jelentése

Valós idejű hely jelentéskészítés lehetővé teszi, hogy a szélesség és hosszúság eszközök társított jelentést. Alapértelmezés szerint a következő típusú hely jelentéskészítés csak akkor használja, hálózati helyek (cella azonosító vagy WIFI alapján), és a jelentés csak aktív futtatásakor az alkalmazás az előtérben (vagyis egy munkamenetben).

Valós idejű helyei *nem* statisztika kiszámítására használható. A csak célja, hogy valós időben geo-kerítés használhatók \<vannak-célközönség-geofencing\> vannak kampányok feltételt.

Ahhoz, hogy valós időben hely jelentéskészítés, is elvégezheti a konfiguráció korábban, hogy az eljárás használatával:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Meg kell hozzáadása a következő engedélyt, ha hiányzó:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Vagy, folytassa a ``ACCESS_FINE_LOCATION`` Ha már használja az alkalmazás.

#### <a name="gps-based-reporting"></a>GPS alapú jelentése

Alapértelmezés szerint valós idejű hely jelentéskészítés csak használja alapú hálózati helyek. (Amelyek sokkal pontos) alapú GPS helyek használatának engedélyezése, használja a konfigurációs objektum:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Meg kell hozzáadása a következő engedélyt, ha hiányzó:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Háttér jelentése

Alapértelmezés szerint valós idejű hely jelentéskészítés esetén csak az aktív (vagyis egy munkamenetben) az előtérben fut. az alkalmazás. Ha engedélyezni szeretné a jelentési is a háttérben, használja a konfigurációs objektum:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Amikor az alkalmazás háttérben fut, csak a hálózati helyeken alapú kell jelenteni, még akkor is, ha engedélyezte a GPS.

A háttér hely jelentés állítható le, ha a felhasználó újraindítja az eszközét, felveheti az Ez legyen az automatikusan indítani rendszerindításkor:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Meg kell hozzáadása a következő engedélyt, ha hiányzó:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M engedélyek

Android M kezdve, néhány futásidőben kezelhetők és engedélyek szükséges felhasználói jóváhagyási.

A futtatókörnyezet engedélyekkel fog ki kell kapcsolni alapértelmezés szerint az új alkalmazás telepítése, ha a célként Android API szintű 23. Egyéb esetben azt fogja be kell kapcsolni alapértelmezés szerint.

A felhasználó engedélyezhetik/letilthatják az eszköz beállítások menüjének ezeket az engedélyeket. Engedélyek rendszermenüjének kikapcsolása az űrlapokat állítjuk leáll háttérfolyamatok az alkalmazás, a rendszer működését, és azt jelenti, hogy a leküldéses kapni a háttérben nincs hatással van.

Mobil tetszés szerint elmélyedhet környezetében futásidőben jóváhagyás megkövetelése engedélyeket a következők:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(csak ha célba juttatása ezt az egy szinttel Android API 23)

A külső tároló csak vannak áttekintés funkció használható. Ha megtalálta helyezni a felhasználóknak, ezzel az engedéllyel kell zavaró, eltávolíthatja azt használt csak a mobil tetszés szerint elmélyedhet de átfogó képet funkció letiltásával.

A hely funkciók egy normál rendszer párbeszédpanel használatával felhasználói engedélyekkel kell kérhet. Ha a felhasználó jóváhagyja, szükség van ``EngagementAgent`` figyelembe módosítások érvénybe valós idejű (ellenkező módosítása feldolgozás a következő alkalommal, amikor a felhasználó elindítja az alkalmazást).

Íme a kód kérése az engedélyek, és továbbítja az eredményt, ha pozitív, hogy egy tevékenység, az alkalmazás segítségével minta ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Speciális jelentése

Tetszés szerint szeretne alkalmazás bizonyos eseményeket, a hibák és a feladatok, ha szeretné használni a tetszés szerint elmélyedhet API keresztül módszerek a `EngagementAgent` osztály. Az osztály objektum lehet retreived hívja fel a `EngagementAgent.getInstance()` statikus módot.

A tevékenységek API-val lehetővé teszi, hogy az összes tetszés szerint elmélyedhet a speciális funkciókat használja, és hogyan részletes a tetszés szerint elmélyedhet API használata Android-eszközön (valamint műszaki dokumentációja az a `EngagementAgent` osztály).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Speciális konfigurációs (a AndroidManifest.xml)

### <a name="wake-locks"></a>Ébresztési zárolás:

Ha azt szeretné, hogy meggyőződhessen róla, hogy a valós idejű Wifi használata esetén, vagy ha ki kapcsolva a képernyő statisztika kapnak, adja hozzá a következő opcionális engedélyt:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Jelentés összeomlik

Ha összeomlik jelentések letiltása, vegye fel a ez (között a `<application>` és `</application>` címkék):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst küszöbérték

Alapértelmezés szerint a tetszés szerint elmélyedhet szolgáltatás jelentések naplózza valós időben. Ha az alkalmazás jelentések naplók gyakran, akkor célszerűbb puffer a naplókat, és a jelentést őket egyszerre egy rendszeres időközönként alap (azaz "burst mód"). Hozzáadás ehhez (között a `<application>` és `</application>` címkék):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

A burst mód kissé növelése az elem leírási_idő, de hatással van az tetszés szerint elmélyedhet monitoron: minden munkamenetek és a feladatok időtartamának (így a munkamenetek és a dolgozók rövidebb, mint a burst küszöbértékét nem láthatják a feladatok) burst küszöbérték lesz kerekítve. Egy mint 30000 (30s) burst küszöbérték használata ajánlott.

### <a name="session-timeout"></a>Munkamenet-időtúllépés

Alapértelmezés szerint a munkamenet befejezése 10s (Ez általában akkor fordul elő, a kezdőlap vagy a vissza billentyű lenyomásával, beállításával a telefon tétlen vagy más alkalmazásba átugorja) utolsó tevékenységét végét követő. Ez a elkerülése érdekében a munkamenet felosztott minden alkalommal, amikor a felhasználó Kilépés és az alkalmazás nagyon gyorsan visszatérhet (ez történik ő felveszi egy képet, ha ellenőrizni egy értesítés stb.). Előfordulhat, hogy módosítani kívánt ezt a paramétert. Hozzáadás ehhez (között a `<application>` és `</application>` címkék):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Tiltsa le a napló jelentése

### <a name="using-a-method-call"></a>Hívás kezdeményezése módszer használatával

Ha azt szeretné, hogy le szeretné állítani a naplók küldése részvételét, felhívhatja:

            EngagementAgent.getInstance(context).setEnabled(false);

A hívás állandó: akkor használja a megosztott beállításokat tartalmazó fájlt.

Tetszés szerint elmélyedhet olyankor, amikor felhívja ezt a funkciót, ha le szeretné állítani a szolgáltatás 1 perc is eltarthat. De azt nem indítsa el a szolgáltatás egyáltalán a későbbiekben indítsa el az alkalmazást.

Engedélyezheti újra jelentéskészítés az azonos függvény hívásával napló `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>A saját integrációja`PreferenceActivity`

Hívja fel ezt a funkciót, hanem integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.

Beállíthatja, tetszés szerint elmélyedhet a beállításokat tartalmazó fájlt (a kívánt módjával) használata a `AndroidManifest.xml` fájl `application meta-data`:

-   A `engagement:agent:settings:name` a határozza meg a megosztott beállítások fájl nevét használja.
-   A `engagement:agent:settings:mode` a megosztott beállítások fájl mód megadása a használja, ugyanazt a módot, mint a kell használnia a `PreferenceActivity`. Számmá alakít át kell adni a mód: állandó jelölők kombinációi használatakor a kód jelölje be a teljes értékét.

Tetszés szerint elmélyedhet mindig használja a `engagement:key` ezt a beállítást kezelésére szolgáló beállítások fájlban logikai billentyűt.

A következő példa `AndroidManifest.xml` megjeleníti az alapértelmezett értékeket:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Akkor is hozzáadhat egy `CheckBoxPreference` a preferencia-sorrend elrendezésben, az alábbihoz hasonlóan:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
