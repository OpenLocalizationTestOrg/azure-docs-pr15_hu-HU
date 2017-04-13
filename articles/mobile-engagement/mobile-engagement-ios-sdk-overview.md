<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet iOS SDK áttekintése |} Microsoft Azure"
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

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS Azure Mobile tetszés szerint elmélyedhet SDK

Kezdje itt a részleteket az összes a integrálása az Azure Mobile tetszés szerint elmélyedhet a egy iOS-alkalmazás ismerteti. Ha szeretné tegyen egy próbát első, győződjön meg arról, hogy el kell végeznie a [15 perces oktatóprogram](mobile-engagement-ios-get-started.md).

Ide kattintva megtekintheti a [Tartalom SDK](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Integráció eljárások
1. Kezdje itt: [hogyan integrálja a Mobile tetszés szerint elmélyedhet a az iOS-alkalmazás](mobile-engagement-ios-integrate-engagement.md)

2. [Történő vannak (értesítések) az az iOS-alkalmazás integrálásáról](mobile-engagement-ios-integrate-engagement-reach.md) értesítések:

3. Terv végrehajtás nyomon követése: [a speciális Mobile tetszés szerint elmélyedhet címkézés API-ja az iOS-alkalmazás használata](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="400-09122016"></a>4.0.0 (09/12 vagy 2016)

-   Rögzített értesítés nem actioned iOS 10 rendszerű eszközökön.
-   Érvénytelenítése XCode 7.

Korábbi verzió talál a [teljes kibocsátási megjegyzések](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Frissítési eljárások

Ha Ön már rendelkezik integrált tetszés szerint elmélyedhet a régebbi verzióját az alkalmazásba, akkor a SDK frissítéskor, vegye figyelembe az alábbiakat.

Előfordulhat, hogy végre kell hajtani több eljárás, ha Ön kihagyott a SDK lásd: a teljes [Frissítése eljárások](mobile-engagement-ios-upgrade-procedure.md)különböző verzióiban.

Az egyes a SDK verzióját kell először lecserélnie (távolítsa el, és importálja újra xcode) a EngagementSDK és EngagementReach mappa.

###<a name="from-300-to-400"></a>A 3.0.0 való 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 kötelező kezdve a SDK 4.0.0 verziója fut.

> [AZURE.NOTE] Ha valóban függenek XCode 7 majd használhatja az [iOS tetszés szerint elmélyedhet SDK v3.2.4](https://aka.ms/r6oouh). Van egy ismert hibát előző verziójában vannak moduljának iOS 10 rendszerű eszközökön futtatása közben: nincsenek actioned rendszerüzenetek. Elhárításához választania kell az elavult API megvalósítása `application:didReceiveRemoteNotification:` az alkalmazásban delegálása az alábbi képlettel történik:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nem javasoljuk ezt a kerülő megoldást** a jelenség, módosíthatja a bármely közelgő (akár kisebb) iOS verziófrissítés mivel ez iOS API elavult. Meg kell váltani XCode 8 minél korábban beállítást.

#### <a name="usernotifications-framework"></a>UserNotifications keretrendszer
Be kell állítania a `UserNotifications` keretrendszer a összeállítása fázisaiban.

a project explorer nyissa meg a projekt ablaktáblát, és válassza ki a megfelelő célját. Ezután nyissa meg a **"Szerkesztés fázisok"** fülre, és a **"hivatkozás bináris és-tárak"** menüben adja hozzá a keret `UserNotifications.framework` -hivatkozás, mint beállítása`Optional`

#### <a name="application-push-capability"></a>Alkalmazás leküldéses lehetőség
XCode 8 előfordulhat, hogy állítsa alaphelyzetbe az alkalmazás képesség leküldéses, dupla ellenőrizze a `capability` lapja a kijelölt cél.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Az új iOS 10 értesítési regisztráció programkódjának hozzáadása
Az értesítések alkalmazás rögzítése a régebbi kódrészletet továbbra is működik, de a iOS 10 rendszerben elavult API-khoz használja. 

Importálás a `User Notification` keretrendszer:

        #import <UserNotifications/UserNotifications.h>

Az alkalmazás meghatalmazott a `application:didFinishLaunchingWithOptions` módszer csere:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

szerint:

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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Ha már rendelkezik saját UNUserNotificationCenterDelegate végrehajtása

A SDK saját UNUserNotificationCenterDelegate protokollt végrehajtásának tartalmaz. Akkor használja a SDK csomagjában talál az életciklusának tetszés szerint elmélyedhet értesítés az eszközön iOS 10 vagy újabb rendszeren futó Lync. Ha a SDK észleli a meghatalmazott azt nem fogja használni a saját végrehajtása mivel egy alkalmazás csak egy UNUserNotificationCenter meghatalmazott láthatja. Ez azt jelenti, hogy be kell a tetszés szerint elmélyedhet logika hozzáadása a saját meghatalmazott.

Kétféleképpen cél.

Egyszerűen, amelyet a meghatalmazott továbbítása felhívja a SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Vagy öröklő a `AEUserNotificationHandler` osztály

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Meghatározhatja, hogy értesítést tetszés szerint elmélyedhet vagy nem megkerülhetők megtalálható-e a `userInfo` a Agent szótár `isEngagementPushPayload:` módszer osztály.