<properties
    pageTitle="Az Azure Mobile tetszés szerint elmélyedhet Android SDK speciális konfiguráció"
    description="A Speciális beállítások, beleértve az Azure Mobile tetszés szerint elmélyedhet Android SDK Android cikkét ismerteti."
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Az Azure Mobile tetszés szerint elmélyedhet Android SDK speciális konfiguráció

> [AZURE.SELECTOR]
- [Univerzális Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-alapú](mobile-engagement-android-advanced-configuration.md)

Ez az eljárás különböző konfigurációs beállításainak megadása Azure Mobile tetszés szerint elmélyedhet Android-alkalmazással ismerteti.

## <a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Jogosultsági követelményei
Bizonyos beállítások szükséges engedélyeket, ami listája megtalálható a hivatkozást és egysoros az adott szolgáltatásban. Az engedélyek hozzáadása a projekt a AndroidManifest.xml azonnal előtti vagy utáni a `<application>` címke.

A jogosultsági kódot kell megjelenésével megegyező kialakítással szeretné a következő, ahol töltse ki a megfelelő engedélye a táblázat, amely követi.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Engedély | Használatakor |
| ---------- | --------- |
| INTERNETES | Szükséges. Egyszerű tartalmazó jelentések készítéséhez |
| ACCESS_NETWORK_STATE | Szükséges. Egyszerű tartalmazó jelentések készítéséhez |
| RECEIVE_BOOT_COMPLETED | Szükséges. Az értesítések központ megjelennek az eszköz újraindítása után |
| WAKE_LOCK | Ajánlott. Lehetővé teszi, hogy adatgyűjtés WiFi használata esetén, vagy ha a képernyőn ki van kapcsolva |
| REZGÉS | Nem kötelező. Lehetővé teszi, hogy rezgő értesítések érkezésekor |
| DOWNLOAD_WITHOUT_NOTIFICATION | Nem kötelező. Lehetővé teszi, hogy a Android áttekintés értesítés |
| WRITE_EXTERNAL_STORAGE | Nem kötelező. Lehetővé teszi, hogy a Android áttekintés értesítés |
| ACCESS_COARSE_LOCATION | Nem kötelező. Lehetővé teszi, hogy valós idejű hely jelentése |
| ACCESS_FINE_LOCATION | Nem kötelező. Lehetővé teszi, hogy GPS-alapú hely jelentése |

Android M, [néhány engedélyek kezelése futásidőben](mobile-engagement-android-location-reporting.md#Android-M-Permissions)kezdve.

Ha már használja a ``ACCESS_FINE_LOCATION``, akkor nem kell is használhatja ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android nyilvánvalóan beállítási lehetőségek

### <a name="crash-report"></a>Jelentés összeomlik

Összeomlik jelentések letiltásához közötti kód hozzáadása a `<application>` és `</application>` címkék:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst küszöbérték

Alapértelmezés szerint a tetszés szerint elmélyedhet szolgáltatás jelentések naplózza valós időben. Ha az alkalmazás jelentés naplók gyakran változnak, akkor célszerűbb puffer a naplókat, és a jelentés őket egyszerre a egy alap rendszeres időközönként (más néven "burst mód"). Ehhez a közötti kód hozzáadása a `<application>` és `</application>` címkék:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Burst mód kissé növeli az elem leírási_idő, de hatással van az tetszés szerint elmélyedhet monitoron: minden munkamenetek és a feladatok időtartamának (így a munkamenetek és a dolgozók rövidebb, mint a burst küszöbértékét nem láthatják a feladatok) burst küszöbértéknél kerekítve. A burst küszöb legfeljebb 30000 (30s) kell lennie.

### <a name="session-timeout"></a>Munkamenet-időtúllépés

 Egy tevékenységet a **Home** vagy **vissza** billentyű lenyomásával, beállításával a telefon tétlen vagy más alkalmazásba átugorja leállítható. Alapértelmezés szerint a munkamenet véget az utolsó tevékenység befejezése után tíz másodperc. Ezzel elkerülhető a munkamenet felosztott minden alkalommal, amikor a felhasználó kijelentkezik, majd visszatér az alkalmazás gyorsan, amely is történik, amikor a felhasználó felveszi a képet, ellenőrzi a értesítés stb. Előfordulhat, hogy módosítani kívánt ezt a paramétert. Ehhez a közötti kód hozzáadása a `<application>` és `</application>` címkék:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Tiltsa le a napló jelentése

### <a name="using-a-method-call"></a>Hívás kezdeményezése módszer használatával

Ha azt szeretné, hogy le szeretné állítani a naplók küldése részvételét, felhívhatja:

    EngagementAgent.getInstance(context).setEnabled(false);

A hívás állandó: akkor használja a megosztott beállításokat tartalmazó fájlt.

Tetszés szerint elmélyedhet olyankor, amikor felhívja ezt a funkciót, ha a szolgáltatás leállítása egy percig is eltarthat. De azt nem indítsa el a szolgáltatás egyáltalán a későbbiekben indítsa el az alkalmazást.

Engedélyezheti újra jelentéskészítés az azonos függvény hívásával napló `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>A saját integrációja`PreferenceActivity`

Hívja fel ezt a funkciót, hanem integrálhatja ezt a beállítást a közvetlenül a meglévő `PreferenceActivity`.

Beállíthatja, tetszés szerint elmélyedhet a beállításokat tartalmazó fájlt (a kívánt módjával) használata a `AndroidManifest.xml` fájl `application meta-data`:

-   A `engagement:agent:settings:name` a határozza meg a megosztott beállítások fájl nevét használja.
-   A `engagement:agent:settings:mode` a megosztott beállítások fájl mód megadása a használja. Használja ugyanazt a módot, mint a `PreferenceActivity`. Számmá alakít át kell adni a mód: állandó jelölők kombinációi használatakor a kód jelölje be a teljes értékét.

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
