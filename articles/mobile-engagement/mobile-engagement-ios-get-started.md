<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet a cél C iOS használatának első lépései |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure Mobile tetszés szerint elmélyedhet analitikai és a leküldéses értesítések for iOS-alkalmazások használatához."
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
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Azure Mobile tetszés szerint elmélyedhet a cél C iOS-alkalmazások használatának első lépései

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használatával megérteni az alkalmazás használatát, és küldje el a leküldéses értesítések szegmentált felhasználók iOS az alkalmazás.
Ebben az oktatóanyagban hozzon létre egy üres iOS-alkalmazás által gyűjtött adatokat, és az Apple leküldéses értesítést rendszer (APNS) használata a leküldéses értesítések fogadása.

Ebben az oktatóprogramban az alábbiakra van szükség:

+ A MAC App Store telepítheti XCode 8
+ a [mobil tetszés szerint elmélyedhet iOS SDK]

Ebben az oktatóanyagban befejezése rendszer minden más Mobile tetszés szerint elmélyedhet oktatóanyagok az iOS-alkalmazások.

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Mobil tetszés szerint elmélyedhet beállítása az iOS-alkalmazás

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció", amely a minimálisan szükséges adatok összegyűjtése és küldje el a leküldéses értesítést jeleníti meg. A teljes integrációs dokumentációt megtalálható a [Mobile tetszés szerint elmélyedhet iOS SDK integrációja](mobile-engagement-ios-sdk-overview.md)

Egy egyszerű app integrációja keresztül mutatjuk XCode azt hoz létre.

###<a name="create-a-new-ios-project"></a>IOS új projekt létrehozása

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

1. Töltse le a [mobil tetszés szerint elmélyedhet iOS SDK csomagjában talál].
2. Bontsa ki a. tar.gz fájl egy mappába a számítógépén.
3. Kattintson a jobb gombbal a projektet, és válassza a **Hozzáadás a fájlok**.

    ![][1]

4. Nyissa meg azt a mappát, ahová a SDK csomagjában talál, válassza ki a `EngagementSDK` mappát, és nyomja le az **OK gombra**.

    ![][2]

5. **Összeállítása fázisok** lapjának megnyitása, és kattintson a **Hivatkozás bináris a tárak** menü hozzáadása a keretek alább látható módon:

    ![][3]

6. Térjen vissza az alkalmazást **a kapcsolat adatai** lapon az Azure-portálra, és másolja a vágólapra a kapcsolati karakterlánc.

    ![][4]

7. A következő sort a kód hozzáadása a **AppDelegate.m** fájl.

        #import "EngagementAgent.h"

8. Most illessze be a kapcsolati karakterláncot az `didFinishLaunchingWithOptions` delegálása.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`van egy választható kimutatást, amely lehetővé teszi a SDK naplók kapcsolatos problémák azonosítása érdekében. 

##<a id="monitor"></a>Valós idejű ellenőrzése engedélyezése

Adatok küldése, és annak biztosítására, hogy a felhasználók aktív indításához legalább egy képernyővel (a tevékenység) kell küldenie a Mobile tetszés szerint elmélyedhet kódmentes.

1. Nyissa meg a **ViewController.h** fájlt, és importálja a **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Most már cseréje a felügyelő osztály által **ViewController** felületének `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Mobil tetszés szerint elmélyedhet lehetővé teszi, hogy a felhasználók kommunikálni, és a leküldéses értesítések és az-app kampányok környezetében üzenetküldés is ELÉRHETI. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
Az alábbi szakaszok is kapni az alkalmazás beállítása.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Csendes leküldéses értesítést szeretne kapni az alkalmazás engedélyezése

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>A gyermekektől tár hozzáadása a projekthez

1. Kattintson a jobb gombbal a projektben.
2. Jelölje ki **Hozzáadás gombra**.
3. Nyissa meg azt a mappát, ahová a SDK csomagjában talál.
4. Jelölje ki a `EngagementReach` mappát.
5. Kattintson a **hozzáadása**gombra.

### <a name="modify-your-application-delegate"></a>Az alkalmazás meghatalmazott módosítása

1. Vissza a **AppDeletegate.m** fájlban a tetszés szerint elmélyedhet elérje a modul importálása.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Belül a `application:didFinishLaunchingWithOptions` módszer, hozzon létre egy vannak modult, és a meglévő tetszés szerint elmélyedhet inicializálni sorban adja át:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>APN leküldéses értesítést szeretne kapni az alkalmazás engedélyezése

1. Az alábbi sor hozzáadása a `application:didFinishLaunchingWithOptions` módszer:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Adja hozzá a `application:didRegisterForRemoteNotificationsWithDeviceToken` módszer az alábbiak szerint:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Adja hozzá a `didFailToRegisterForRemoteNotificationsWithError` módszer az alábbiak szerint:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Adja hozzá a `didReceiveRemoteNotification:fetchCompletionHandler` módszer az alábbiak szerint:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobil tetszés szerint elmélyedhet iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

