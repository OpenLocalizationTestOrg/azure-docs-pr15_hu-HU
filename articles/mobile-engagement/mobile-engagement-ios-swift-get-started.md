<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet a Swift iOS használatának első lépései |} Microsoft Azure"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések for iOS alkalmazások Azure Mobile tetszés szerint elmélyedhet használatához."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Azure Mobile tetszés szerint elmélyedhet iOS Swift az alkalmazások használatának első lépései

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használatával megérteni az alkalmazás használatát, és küldje el a leküldéses értesítések szegmentált felhasználók iOS az alkalmazás.
Ebben az oktatóanyagban hozzon létre egy üres iOS-alkalmazás által gyűjtött adatokat, és az Apple leküldéses értesítést rendszer (APNS) használata a leküldéses értesítések fogadása.

Ebben az oktatóprogramban az alábbiakra van szükség:

+ A MAC App Store telepítheti XCode 8
+ a [mobil tetszés szerint elmélyedhet iOS SDK]
+ Leküldéses értesítést tanúsítvány (.p12) szerezhet be az Apple fejlesztői központ

> [AZURE.NOTE] Ebben az oktatóanyagban Swift 3.0-s verzióját használja. 

Ebben az oktatóanyagban befejezése rendszer minden más Mobile tetszés szerint elmélyedhet oktatóanyagok az iOS-alkalmazások.

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Mobil tetszés szerint elmélyedhet beállítása az iOS-alkalmazás

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció", amely a minimálisan szükséges adatok összegyűjtése és küldje el a leküldéses értesítést jeleníti meg. A teljes integrációs dokumentációt megtalálható a [Mobile tetszés szerint elmélyedhet iOS SDK integrációja](mobile-engagement-ios-sdk-overview.md)

Egy egyszerű alkalmazás XCode keresztül mutatjuk integrálása a hozunk létre:

###<a name="create-a-new-ios-project"></a>IOS új projekt létrehozása

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása Mobile tetszés szerint elmélyedhet kódmentes

1. A [mobil tetszés szerint elmélyedhet iOS SDK] letöltése
2. Bontsa ki a. tar.gz fájl egy mappába a számítógépre
3. Kattintson a jobb gombbal a projektet, és válassza a "Fájlok hozzáadása..."

    ![][1]

4. Nyissa meg azt a mappát, ahová a SDK, és válassza a `EngagementSDK` mappára, majd nyomja le az OK gombra.

    ![][2]

5. Nyissa meg a `Build Phases` lap és a `Link Binary With Libraries` menü hozzáadása a keretek alább látható módon:

    ![][3]

8. Engedélyezni szeretné a fájlt a SDK cél C API-k használata áthidalás fejléc létrehozása > Új > Fájl > iOS > forrás > élőfej fájlt.

    ![][4]

9. Szerkessze az összekötő élőfej fájlt elérhetővé tenni a gyors kód Mobile tetszés szerint elmélyedhet cél-C kódot, és adja meg a következő import:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Összeállítása beállítások csoportban állítsa be Swift fordító - kód létrehozása a beállítás célja-C hidat élőfej összeállítása van ezt a fejlécet elérési útját. Íme egy példa a elérési útja: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (attól függően, hogy az elérési út)**

    ![][6]

11. Térjen vissza az alkalmazást *a kapcsolat adatai* lapon az Azure-portálra, és másolja a vágólapra a kapcsolati karakterlánc

    ![][5]

12. Most illessze be a kapcsolati karakterláncot az `didFinishLaunchingWithOptions` delegálása

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Valós idejű ellenőrzése engedélyezése

Adatok küldése, és annak biztosítására, hogy a felhasználók aktív indításához legalább egy képernyővel (a tevékenység) kell küldenie a Mobile tetszés szerint elmélyedhet kódmentes.

1. Nyissa meg a **ViewController.swift** fájlt, és az alap osztály **ViewController** **EngagementViewController**kell a csere:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Mobile tetszés szerint elmélyedhet segítségével interaktív módon, és ELÉRHETI a leküldéses értesítések és az alkalmazás üzenetküldés kampányok környezetében a felhasználókkal. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
A következőkben lesz beállítási is kapni az alkalmazás.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Csendes leküldéses értesítést szeretne kapni az alkalmazás engedélyezése

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>A gyermekektől tár hozzáadása a projekthez

1. Kattintson a jobb gombbal a projekthez
2. Válassza a`Add file to ...`
3. Nyissa meg azt a mappát, ahová a SDK
4. Jelölje ki a `EngagementReach` mappa
5. Kattintson a Hozzáadás
6. A fejlécek Mobile tetszés szerint elmélyedhet cél-C elérni, és adja hozzá a következő import összekötő élőfej-fájl szerkesztése:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Az alkalmazás meghatalmazott módosítása

1. Belül a `didFinishLaunchingWithOptions` - hozzon létre egy vannak modult, és a meglévő tetszés szerint elmélyedhet inicializálni sorban adja át:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>APN leküldéses értesítést szeretne kapni az alkalmazás engedélyezése
1. Az alábbi sor hozzáadása a `didFinishLaunchingWithOptions` módszer:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Adja hozzá a `didRegisterForRemoteNotificationsWithDeviceToken` módszer az alábbiak szerint:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Adja hozzá a `didReceiveRemoteNotification:fetchCompletionHandler:` módszer az alábbiak szerint:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobil tetszés szerint elmélyedhet iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
