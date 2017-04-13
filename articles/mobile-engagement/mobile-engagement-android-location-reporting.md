<properties
    pageTitle="Jelentéskészítési Android SDK Azure mobil tetszés szerint elmélyedhet a hely"
    description="Megtudhatja, hogy miként jelentéskészítési Azure Mobile tetszés szerint elmélyedhet Android SDK a hely beállítása"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Jelentéskészítési Android SDK Azure mobil tetszés szerint elmélyedhet a hely

> [AZURE.SELECTOR]
- [Android-alapú](mobile-engagement-android-integrate-engagement.md)

Ez a témakör ismerteti az Android-alkalmazás jelentése helyét.

## <a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Hely jelentése

Ha azt szeretné, hogy a helyekre kell jelenteni, konfiguráció néhány sorok hozzáadásához szüksége (között a `<application>` és `</application>` címkék).

### <a name="lazy-area-location-reporting"></a>Lusta terület hely jelentése

Lehetővé teszi, hogy az ország, régió és eszközök társított településen jelentéskészítés jelentéskészítés Lusta terület helye. A következő típusú hely jelentése csak az (a cella azonosító vagy WIFI alapuló) a hálózati helyeken használja. Munkamenet minden legfeljebb egyszer jelenteni az eszköz terület. Soha nem használják a GPS, és így helyet a jelentés a következő típusú alacsony hatással van az elem.

A bejelentett területek számítja ki a felhasználókat, munkamenetek, események és hibák földrajzi statisztikájának szolgálnak. Is felhasználhatók vannak kampányok feltételként.

Engedélyezi a korábban, ez az eljárás a konfigurációval jelentéskészítés Lusta terület helye:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Meg kell adja meg a hely jogosultsági. Ez a kód használja ``COARSE`` jogosultsági:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Ha az alkalmazás megkívánja, akkor ``ACCESS_FINE_LOCATION`` helyette.

### <a name="real-time-location-reporting"></a>Valós idejű hely jelentése

Valós idejű hely jelentéskészítés lehetővé teszi, hogy a szélesség és hosszúság eszközök társított jelentéskészítés. Alapértelmezés szerint a megfelelő helyre jelentéskészítés csak hálózati helyek cella azonosító vagy WIFI alapján használja. A jelentési esetén csak az aktív (például egy munkamenetben) az előtérben fut. az alkalmazást.

Valós idejű helyei *nem* statisztika kiszámítására használható. A csak célja, hogy valós időben geo-kerítés használhatók \<vannak-célközönség-geofencing\> vannak kampányok feltételt.

Ahhoz, hogy a jelentési valós idejű helyét, a kód sor hozzáadása hol beállítja a tetszés szerint elmélyedhet kapcsolati karakterláncot az alkalmazásindító tevékenység. Az eredmény formátumban, például a következőket:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS alapú jelentése

Alapértelmezés szerint valós idejű hely jelentéskészítés csak használja hálózati helyek. A konfigurációs objektum segítségével GPS-alapú helyekre, amelyek sokkal pontos, használatának engedélyezése:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Meg kell hozzáadása a következő engedélyt, ha hiányzó:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Háttér jelentése

Alapértelmezés szerint valós idejű hely jelentéskészítés esetén csak az aktív (például egy munkamenetben) az előtérben fut. az alkalmazás. Ha engedélyezni szeretné a jelentési is a háttérben, használja a konfigurációs objektum:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Amikor az alkalmazás a háttérben fut csak a hálózati helyek jelentik, még akkor is, ha engedélyezte a GPS.

Ha a felhasználó az eszköze újraindul, a háttérben hely jelentés leállt. Szeretné tenni a betöltés közben automatikusan indítani, adja hozzá a kódot.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Meg kell hozzáadása a következő engedélyt, ha hiányzó:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M engedélyek

Android M kezdve, néhány futásidőben kezelhetők és engedélyek szükséges felhasználói jóváhagyási.

Ha a célként Android API szintű 23, a futtatókörnyezet engedélyek ki vannak kapcsolva alapértelmezés szerint az új alkalmazás telepítése. Egyéb esetben vannak kapcsolva alapértelmezés szerint.

Engedélyezhetik/letilthatják az eszköz beállítások menüjének ezeket az engedélyeket. Kapcsolja ki a rendszer menüből engedélyek űrlapokat állítjuk leáll a háttérfolyamatok az alkalmazás, amely a rendszer működését, és nincs hatással van a azt jelenti, hogy a leküldéses kapni a háttérben.

Jelentéskészítés Mobile tetszés szerint elmélyedhet hely környezetében futásidőben jóváhagyás megkövetelése engedélyeket a következők:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Kérjen engedélyt a felhasználónak, használja a szabványos rendszer párbeszédpanel megjelenítéséhez. A felhasználó jóváhagyja, ha ``EngagementAgent`` figyelembe módosítások valós idejű állapotba. Egyéb esetben a változás feldolgozása a következő alkalommal, amikor a felhasználó elindítja az alkalmazást.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
