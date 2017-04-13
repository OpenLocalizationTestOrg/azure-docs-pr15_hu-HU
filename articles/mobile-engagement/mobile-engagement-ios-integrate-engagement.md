<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet iOS SDK integrációs |} Microsoft Azure"
    description="Legújabb frissítéseket, és az iOS Azure Mobile tetszés szerint elmélyedhet SDK eljárások"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Hogyan tetszés szerint elmélyedhet integráció az iOS operációs rendszeren

> [AZURE.SELECTOR]
- [A Windows univerzális](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-alapú](mobile-engagement-android-integrate-engagement.md)

Ez az eljárás a legegyszerűbb módszer tetszés szerint elmélyedhet a analitikai és a függvények az iOS-alkalmazás figyelése aktiválása ismerteti.

A tevékenységek SDK és elő kell készítenie iOS6 + Xcode 8: az alkalmazás a telepítés helyének legalább kell iOS 6.

> [AZURE.NOTE]
> Ha valóban függenek XCode 7 majd használhatja az [iOS tetszés szerint elmélyedhet SDK v3.2.4](https://aka.ms/r6oouh). Van egy ismert hibát az előző verzióhoz vannak modulon közben iOS 10-eszközökön futó [vannak modul integrációja](mobile-engagement-ios-integrate-engagement-reach.md) további részleteket lásd:. Ha úgy dönt, hogy a SDK v3.2.4 használata, akkor csak ugorja át a `UserNotifications.framework` importálása a következő lépésben.

A következő lépések nem elég a jelentést a naplók szükség számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és Technicals kapcsolatos összes statisztika aktiválása. A jelentés naplók szükség más statisztikai adatokat, például eseményeket, a hibák és a feladatok számítja ki kell végezni, manuálisan segítségével tetszés szerint elmélyedhet API-val (lásd: [a speciális Mobile tetszés szerint elmélyedhet címkézés API-ja az iOS-alkalmazás használatához](mobile-engagement-ios-use-engagement-api.md) , mivel ezeket a statisztikákat függő alkalmazás.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>A tevékenységek SDK beágyazása a az iOS Project programban

- Töltse le az iOS SDK [Itt](http://aka.ms/qk2rnj).

- A tevékenységek SDK csomagjában talál az iOS projekthez hozzáadni: Xcode, kattintson a projekt a jobb gombbal, és jelölje be a **"Fájlok hozzáadása..."** , és válassza a `EngagementSDK` mappát.

- Tetszés szerint elmélyedhet további keretek a munkát igényel: a project explorer nyissa meg a projekt ablaktáblát, és válassza ki a megfelelő célját. Ezután nyissa meg a **"Szerkesztés fázisok"** fülre, és a **"hivatkozás bináris és-tárak"** menüben adja hozzá a következő keretek:

    -   `UserNotifications.framework`-a hivatkozás, mint beállítása`Optional`
    -   `AdSupport.framework`-a hivatkozás, mint beállítása`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] Az AdSupport keretrendszer eltávolíthatók. Tetszés szerint elmélyedhet a keret gyűjt a IDFA van szüksége. Azonban lehet letiltani IDFA webhelycsoport \<ios-sdk-előjegyzéssel-idfa\> felel meg az új Apple házirend kapcsolatban a azonosítójával.

##<a name="initialize-the-engagement-sdk"></a>A tevékenységek SDK inicializálni

Az alkalmazás meghatalmazott módosítani kell:

-   A végrehajtás fájl tetején importálása a tevékenységek agent:

        [...]
        #import "EngagementAgent.h"

-   Tetszés szerint elmélyedhet inicializálni belül a módszerrel "**applicationDidFinishLaunching:**"vagy"**alkalmazás: didFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Egyszerű jelentés

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Ajánlott módszer: túlterhelés a `UIViewController` osztályok

A jelentés tetszés szerint elmélyedhet számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és technikai statisztika által igényelt összes naplók aktiválásához egyszerűen teheti az összes a `UIViewController` alszint osztályok örökölhet a `EngagementViewController` osztályok (ugyanaz a szabály `UITableViewController`  - \> `EngagementTableViewController`).

**Tetszés szerint elmélyedhet: nélkül**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**A tevékenységek:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternatív módszer: hívás `startActivity()` manuálisan

Ha nem szeretne a többszörös vagy nem a `UIViewController` osztályok, hívja fel a tevékenységek inkább elindíthatja `EngagementAgent`meg közvetlenül módszerek.

> [AZURE.IMPORTANT] Az iOS SDK automatikusan felhívja a `endActivity()` módszer, ha az alkalmazás be van zárva. Így célszerű *erősen* ajánlott, ha fel szeretne hívni a `startActivity` módszer, amikor a tevékenység annak a felhasználónak módosítása, és *Soha* hívja a `endActivity` módszer, mivel ez a módszer hívásához kezd az aktuális munkamenet be kell fejezni.

##<a name="location-reporting"></a>Hely jelentése

Apple szolgáltatási szerződése helyének nyomon követése csak a statisztika célra használható alkalmazások esetén nem engedélyezik. Így ajánlott hely jelentések engedélyezése, csak akkor, ha az alkalmazás a a helyének nyomon követése egy másik okból is használhatja.

IOS 8 kezdve, meg kell adnia hogyan az alkalmazás hely szolgáltatásokat használ egy karakterlánc kulcsot [NSLocationWhenInUseUsageDescription] vagy [NSLocationAlwaysUsageDescription] beállítva az alkalmazás Info.plist fájl leírását. Ha azt szeretné, a háttérben tetszés szerint elmélyedhet a jelentés helyre, adja hozzá a NSLocationAlwaysUsageDescription billentyűt. Minden egyéb esetben az adja meg a NSLocationWhenInUseUsageDescription billentyűt.

### <a name="lazy-area-location-reporting"></a>Lusta terület hely jelentése

Lusta terület hely jelentéskészítés lehetővé teszi az ország, a terület és a kapcsolódó eszközök településen. A következő típusú hely jelentése csak az (a cella azonosító vagy WIFI alapuló) a hálózati helyeken használja. Munkamenet minden legfeljebb egyszer jelenteni az eszköz terület. Soha nem használják a GPS, és az így helyet a jelentés a következő típusú nagyon kevés (nem a fel, hogy nincs) az elem gyakorolt hatását.

A bejelentett területek számítja ki a felhasználókat, munkamenetek, események és hibák földrajzi statisztikájának szolgálnak. Is felhasználhatók vannak kampányok feltételként. Az utolsó ismert területen jelenteni az eszköz az [Eszköz API]több tudja visszaszerezni.

Lusta terület hely jelentéskészítés engedélyezéséhez helyezzen el a következő sort a tetszés szerint elmélyedhet agent inicializálása után:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Valós idejű hely jelentése

Valós idejű hely jelentéskészítés lehetővé teszi, hogy a szélesség és hosszúság eszközök társított jelentést. Alapértelmezés szerint a következő típusú hely jelentéskészítés csak akkor használja, hálózati helyek (cella azonosító vagy WIFI alapján), és a jelentés csak aktív futtatásakor az alkalmazás az előtérben (vagyis egy munkamenetben).

Valós idejű helyei *nem* statisztika kiszámítására használható. A csak célja, hogy valós időben geo-kerítés használhatók \<vannak-célközönség-geofencing\> vannak kampányok feltételt.

Ahhoz, hogy valós időben hely jelentéskészítés, helyezzen el a következő sort a tetszés szerint elmélyedhet agent inicializálása után:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS alapú jelentése

Alapértelmezés szerint valós idejű hely jelentéskészítés csak használja alapú hálózati helyek. (Amelyek sokkal pontos) alapú GPS helyek használatának engedélyezése, vegyen fel:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Háttér jelentése

Alapértelmezés szerint valós idejű hely jelentéskészítés esetén csak az aktív (vagyis egy munkamenetben) az előtérben fut. az alkalmazás. Ha engedélyezni szeretné a jelentési is a háttérben, felvétele:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Amikor az alkalmazás háttérben fut, csak a hálózati helyeken alapú kell jelenteni, még akkor is, ha engedélyezte a GPS.

Ez a funkció végrehajtásának visszahívja [startMonitoringSignificantLocationChanges] , amikor az alkalmazás a háttérben működésbe. Ne feledje, hogy akkor fog automatikusan indítsa újra az alkalmazás a háttérben, ha egy új helyre esemény megérkezik.

##<a name="advanced-reporting"></a>Speciális jelentése

Tetszés szerint szeretne alkalmazás bizonyos eseményeket, a hibák és a feladatok, ha szeretné használni a tetszés szerint elmélyedhet API keresztül módszerek a `EngagementAgent` osztály. Az osztály objektum tudja visszaszerezni hívja fel a `[EngagementAgent shared]` statikus módot.

A tevékenységek API-val lehetővé teszi, hogy az összes tetszés szerint elmélyedhet a speciális funkciókat használja, és hogyan részletes történő használatáról a tetszés szerint elmélyedhet API iOS (valamint műszaki dokumentációja az a `EngagementAgent` osztály).

##<a name="disable-idfa-collection"></a>A webhelycsoport IDFA letiltása

Alapértelmezés szerint tetszés szerint elmélyedhet fogja használni a [IDFA] egyedileg azonosító felhasználó. De ha nem használ a máshol hirdetési az alkalmazásban, akkor előfordulhat, hogy utasítani az App Store áruházban Véleményezés folyamat. IDFA webhelycsoport lehet letiltani a előfeldolgozó makró hozzáadásával `ENGAGEMENT_DISABLE_IDFA` a pch fájl (vagy a `Build Settings` az alkalmazás). Ezzel biztosíthatja, hogy van-e nem hivatkozik `ASIdentifierManager`, `advertisingIdentifier` vagy `isAdvertisingTrackingEnabled` alkalmazás összeállítása.

Integráció a **prefix.pch** fájl:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Ellenőrizheti, hogy a IDFA gyűjtése van megfelelően tiltva az alkalmazásban, jelölje be a tetszés szerint elmélyedhet próba naplókat. Lásd a integrációs tesztelése\<ios-sdk-előjegyzéssel-próba-idfa\> dokumentációjában találhat további információt.

##<a name="disable-log-reporting"></a>Tiltsa le a napló jelentése

### <a name="using-a-method-call"></a>Hívás kezdeményezése módszer használatával

Ha azt szeretné, hogy le szeretné állítani a naplók küldése részvételét, felhívhatja:

    [[EngagementAgent shared] setEnabled:NO];

A hívás állandó: használ `NSUserDefaults` tárolja az információkat.

Engedélyezheti újra jelentéskészítés az azonos függvény hívásával napló `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integráció a beállításokat az első lépésekhez

Hívja fel ezt a funkciót, hanem integrálhatja ezt a beállítást a közvetlenül a meglévő `Settings.bundle` fájlt. A karakterlánc `engagement_agent_enabled` kell használni egy az elsőbbségi azonosító, és meg kell társítani váltása kapcsoló (`PSToggleSwitchSpecifier`).

A következő példa `Settings.bundle` végrehajtásához szemlélteti:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Eszköz API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
