<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet iOS SDK elérjen integrációs |} Microsoft Azure"
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

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Hogyan tetszés szerint elmélyedhet elérjen integráció az iOS operációs rendszeren

Követniük kell a integrációs leírtakat [tetszés szerint elmélyedhet integráció iOS dokumentumot módjáról](mobile-engagement-ios-integrate-engagement.md) a következő útmutatóban előtt.

Ez a dokumentáció XCode 8 szükséges. Ha valóban függenek XCode 7 majd használhatja az [iOS tetszés szerint elmélyedhet SDK v3.2.4](https://aka.ms/r6oouh). Van egy ismert hibát előző verziójában közben iOS 10 rendszerű eszközökön: nincsenek actioned rendszerüzenetek. Elhárításához választania kell az elavult API megvalósítása `application:didReceiveRemoteNotification:` az alkalmazásban delegálása az alábbi képlettel történik:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Nem javasoljuk ezt a kerülő megoldást** a jelenség, módosíthatja a bármely közelgő (akár kisebb) iOS verziófrissítés mivel ez iOS API elavult. Meg kell váltani XCode 8 minél korábban beállítást.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Csendes leküldéses értesítést szeretne kapni az alkalmazás engedélyezése

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Integráció lépések

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>A tevékenységek elérjen SDK beágyazása a az iOS Project programban

-   A Xcode projekt hozzá a gyermekektől SDK csomagjában talál. Xcode, válassza a **projekt \> hozzáadása a projekthez** , és válassza a `EngagementReach` mappát.

### <a name="modify-your-application-delegate"></a>Az alkalmazás meghatalmazott módosítása

-   A végrehajtás fájl tetején a tetszés szerint elmélyedhet elérje a modul importálása:

        [...]
        #import "AEReachModule.h"

-   Belül módszer `applicationDidFinishLaunching:` vagy `application:didFinishLaunchingWithOptions:`, hozzon létre egy vannak modult, és a meglévő tetszés szerint elmélyedhet inicializálni sorban adja át:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Módosítsa a kép neve nevet, amelyet az értesítési ikon **"icon.png"** karakterláncot.
-   Ha a *frissítés jelvény érték* beállítást vannak kampányok használni kívánt vagy natív leküldéses használni kívánt \<szoftver/vannak API/marketingkampány formátum/natív leküldéses\> kampányokkal kell hagyja, hogy a modul kezelése a jelvény ikonra magát (akkor fog automatikusan törölje a jelet az alkalmazás jelvény és is alaphelyzetbe állítása az érték tetszés szerint elmélyedhet tárolt minden alkalommal, amikor az alkalmazás lépések vagy foregrounded) vannak. Ez történik, vannak modul inicializálni után a következő sort hozzáadásával:

        [reach setAutoBadgeEnabled:YES];

-   Ha azt szeretné, miként kezelje a vannak adatok leküldéses, kell hagyja, hogy az alkalmazás meghatalmazott felel meg a `AEReachDataPushDelegate` Protocol (protokoll). Adja hozzá a következő sort vannak modul inicializálni után:

        [reach setDataPushDelegate:self];

-   A módszereket is alkalmazhat, majd `onDataPushStringReceived:` és `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` az alkalmazás meghatalmazott a:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Kategória

A kategória paraméter egy adatok leküldéses marketingkampány létrehozásakor a lépés nem kötelező, és lehetővé teszi, hogy Ön szűrheti az adatokat verembe küldi. Ez akkor hasznos, ha különböző leküldéses szeretne a `Base64` adatokat, és azok típusának azonosítása elemzése őket előtt szeretne.

**Az alkalmazás készen áll a vezérlőn és vannak tartalom megjelenítéséhez!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Hogyan hirdetmények és bármikor szavazásokra

Tetszés szerint elmélyedhet vannak értesítéseket küldheti a végfelhasználói bármikor az Apple leküldéses értesítéseket kezelő szolgáltatás használatával.

Ez a funkció engedélyezéséhez fognak rendelkezésére állni előkészítése az alkalmazást az Apple leküldéses értesítéseket, és az alkalmazás meghatalmazott módosítására.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Felkészülés az alkalmazást az Apple leküldéses értesítések

Hajtsa végre az útmutató: [az Apple leküldéses értesítések alkalmazás előkészítése](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>A szükséges ügyfélprogram-kód hozzáadása

*Az alkalmazás ezen a ponton a tetszés szerint elmélyedhet frontend kell rendelkezniük regisztrált Apple leküldéses tanúsítvány.*

Ha ez nem tette, meg kell regisztrálhatja a leküldéses értesítést szeretne kapni az alkalmazás.

* Importálás a `User Notification` keretrendszer:

        #import <UserNotifications/UserNotifications.h>

* Az alkalmazás indításakor, vegye fel a következő sort (általában a `application:didFinishLaunchingWithOptions:`):

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

Ezután szüksége ahhoz, hogy az eszköz jogkivonathoz Apple kiszolgáló által visszaadott részvételét. Ezt a módszert nevű `application:didRegisterForRemoteNotificationsWithDeviceToken:` az alkalmazás meghatalmazott a:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Végül hogy tájékoztatja a tetszés szerint elmélyedhet SDK csomagjában talál, amikor az alkalmazás távoli értesítést kap. Megtennie, hívja fel a módszerrel `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` az alkalmazás meghatalmazott a:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] A fenti módszer iOS 7 verziójában jelent meg. Ha iOS céloz < 7, ügyeljen arra, hogy módszer megvalósítása `application:didReceiveRemoteNotification:` a meghatalmazott alkalmazás és a hívás `applicationDidReceiveRemoteNotification` a EngagementAgent megkerülhetők, ha nem üres a a `handler` argumentum:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Alapértelmezés szerint a completionHandler tetszés szerint elmélyedhet elérjen szabályozza. Ha manuálisan válaszolni szeretne a `handler` a kód mezőjében, nulla átviheti a `handler` argumentum és befejezése vezérlő letiltása saját magának. Lásd: a `UIBackgroundFetchResult` lehetséges értékek típusú listát.


### <a name="full-example"></a>Példa teljes

Az alábbiakban integráció a teljes példa:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Ha van saját UNUserNotificationCenterDelegate végrehajtása

A SDK szintén saját UNUserNotificationCenterDelegate protokollt végrehajtását. Akkor használja a SDK csomagjában talál az életciklusának tetszés szerint elmélyedhet értesítés az eszközön iOS 10 vagy újabb rendszeren futó Lync. Ha a SDK észleli a meghatalmazott azt nem fogja használni a saját végrehajtása mivel egy alkalmazás csak egy UNUserNotificationCenter meghatalmazott láthatja. Ez azt jelenti, hogy be kell a tetszés szerint elmélyedhet logika hozzáadása a saját meghatalmazott.

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

##<a name="how-to-customize-campaigns"></a>Kampányok testreszabása

### <a name="notifications"></a>Értesítések

Értesítés két típusa van: rendszer és az alkalmazás értesítést.

Rendszerüzenetek iOS kezeli, és nem szabható testre.

Az alkalmazás értesítéseket az aktuális alkalmazásablak dinamikusan hozzáadott nézet. Egy értesítés átfedő Link. Átfedi értesítési kiválóan alkalmasak a gyors integráció, mivel ezek nem szükséges, hogy az alkalmazás bármely megjelenítésének módosítása.

#### <a name="layout"></a>Elrendezés

Az alkalmazás az értesítések megjelenésének módosításához egyszerűen módosíthatja a fájlt `AENotificationView.xib` igényeinek, amíg a címke értékek és a meglévő subviews típusú megtartani.

Alapértelmezés szerint a képernyő alján az alkalmazás jelennek meg. Ha inkább megjelenítheti őket a képernyő felső részén, a megadott szerkesztése `AENotificationView.xib` , és módosíthatja a `AutoSizing` tulajdonság a fő nézet úgy is kell tartani a superview tetején.

#### <a name="categories"></a>A kategóriák

Ha módosítja a megadott elrendezést, minden értesítést megjelenését módosíthatja. Kategóriák definiálása különböző célzott látványtervek (valószínűleg viselkedése) az értesítésekhez teszi lehetővé. Kategória egy vannak marketingkampány létrehozásakor adható meg. Ne feledje, hogy a kategóriák segítségével is testre szabhatja a hirdetmények és szavazásokra, a jelen dokumentum ismertetett.

Az értesítések kategória leíró regisztrálásához kell hozzáadnia hívás után a gyermekektől modul inicializált van.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`kell lennie, amely megfelel a protokoll objektum egy példányának `AENotifier`.

A protokoll módszerek alkalmazhat saját maga által, vagy választhatja a meglévő osztály reimplement `AEDefaultNotifier` amely már végzi el a munkát.

Ha például az értesítések nézet egy adott kategória újra szeretné, ha, kövesse ebben a példában:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Ez a kategória egyszerű példa tegyük fel, hogy egy nevű fájlt `MyNotificationView.xib` a fő alkalmazás kötegben. Ha a módszerrel nem tudja megkeresni egy megfelelő `.xib`, az értesítés nem jelenik meg, és tetszés szerint elmélyedhet a konzolban üzenet fog kimeneti.

A megadott nib fájl kell tartaniuk a következő szabályokat:

-   Csak tartalmaznia kell egy nézetet.
-   Subviews kell lennie a azonos típusú, a megadott nib fájlban szerint a lehetőségekből nevű`AENotificationView.xib`
-   Subviews van, hogy ugyanazt a címkét, mint a lehetőségekből belül a megadott nevű nib fájl`AENotificationView.xib`

> [AZURE.TIP] Csak másolja a megadott nib nevű fájlt `AENotificationView.xib`, és onnan munka megkezdése. De ügyeljen arra, hogy, a nézet belül a nib fájl társítva az osztály `AENotificationView`. Az osztály újra a módszerrel `layoutSubViews` lehet áthelyezni és átméretezni a környezettől subviews. Előfordulhat, hogy szeretne cserélni egy `UIView` vagy egyéni nézet osztály.

Az értesítések testreszabása mélyebb igény (Ha azt szeretné, ha például való betöltéséhez közvetlenül a nézetben a kód), nézze meg a megadott forrás kódot és osztály dokumentáció ajánlott `Protocol ReferencesDefaultNotifier` és `AENotifier`.

Figyelje meg, hogy az azonos bejelentő használható több kategória.

Az alapértelmezett bejelentő jelennek meg újra is definiálva:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Értesítés kezelése

Az alapértelmezett kategória használatakor egyes életciklusának módszerek felkért a `AEReachContent` jelentés statisztika az objektum, és frissítse a marketingkampány állapot:

-   Az értesítés jelenik meg az alkalmazást, amikor a `displayNotification` módszer neve (amely a statisztikai jelentések) által `AEReachModule` Ha `handleNotification:` eredménye `YES`.
-   Ha meg van nyitva az értesítésre, a `exitNotification` módszer neve, statisztikai jelentést, és a következő kampányok most feldolgozhatók.
-   Ha az értesítés kattintott, `actionNotification` van neve, statisztikai jelenteni, és a kapcsolódó művelet végrehajtását.

Ha a példányába `AENotifier` figyelmen kívül hagyja a az alapértelmezett működés életciklusának módszerekben hívásakor saját maga által szükséges. Az alábbi példák bemutatják, bizonyos esetekben, ahol az alapértelmezett működés elmarad:

-   Kijelölés méretének növelése nem `AEDefaultNotifier`, például kategória kezelése az alapoktól szerepelni fog.
-   Akkor overrode `prepareNotificationView:forContent:`, ne felejtse el a térkép legalább `onNotificationActioned` vagy `onNotificationExited` az egyik a U.I szabályozza.

> [AZURE.WARNING] Ha `handleNotification:` eredményez, a program törli a kivétel, a tartalom és `drop` van nevezik, ez jelentett statisztikai és következő kampányok most dolgozható fel.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Meglévő nézet részeként értesítés tartalmazza.

Átfedi kiválóan alkalmasak a gyors integráció, de is előfordul, hogy nem lehet hasznos, vagy nem kívánt oldal hatást.

Ha nem elégedett az átfedő rendszer egyes a nézetek, testre szabhatja a következő nézetekben.

Eldöntheti, hogy a meglévő nézetek az értesítési elrendezés felvenni. Ehhez két végrehajtása stílusok van:

1.  A felület-szerkesztő használata értesítés nézet felvétele

    -   Nyissa meg *a szerkesztő felület*
    -   Helyezze a 320 x 60 (vagy ha iPad 768 x 60) `UIView` , amelyre az értesítés jelenik meg
    -   Ez a nézet címke értékének beállítása: **36822491**

2.  Az értesítés nézet hozzáadása a programozás útján. Amikor a nézet inicializálni csak hozzá a következő kódot:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`makró megtalálható a `AEDefaultNotifier.h`.

> [AZURE.NOTE] Az alapértelmezett bejelentő automatikusan észleli, hogy az értesítési elrendezés szerepel ebben a nézetben, és nem vesz fel egy további azt.

### <a name="announcements-and-polls"></a>Hirdetmények és szavazásokra

#### <a name="layouts"></a>Elrendezések

Módosíthatja a fájlok `AEDefaultAnnouncementView.xib` és `AEDefaultPollView.xib` mindaddig, amíg a címke értékek és a meglévő subviews típusú megtartani.

#### <a name="categories"></a>A kategóriák

##### <a name="alternate-layouts"></a>Alternatív elrendezések

Értesítések, például a marketingkampány kategória, hogy az alternatív elrendezések a hirdetmények és szavazásokra használható.

Hozzon létre egy a tartalmat, **AEAnnouncementViewController** kiterjesztése, és rögzítheti azt a gyermekektől modul van már inicializált után:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Minden alkalommal, amikor a felhasználó a kategóriával tartalmat az értesítést fog kattintson "a\_kategória", a regisztrált nézet vezérlő (ebben az esetben `MyCustomAnnouncementViewController`) fog inicializálni, hívja fel a módszerrel `initWithAnnouncement:` és a nézet az aktuális alkalmazásablak lehet hozzáadni.

A végrehajtása során az `AEAnnouncementViewController` be kell olvasni a(z) osztály `announcement` a subviews inicializálni. Vegye figyelembe az alábbi példában, ahol két címkék használatával is inicializált `title` és `body` tulajdonságait a `AEReachAnnouncement` osztály:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Ha nem szeretné a nézetek betöltése keresnie őket, de csak felhasználni kívánt az alapértelmezett bejelentése nézet elrendezés, egyszerűen teheti az egyéni nézet vezérlő bővíti a megadott osztály `AEDefaultAnnouncementViewController`. Ebben az esetben a nib fájl másolása `AEDefaultAnnouncementView.xib` , és nevezze át, így az egyéni nézet vezérlő tölthető (egy vezérlő nevű `CustomAnnouncementViewController`, meg kell hívnia nib fájljait `CustomAnnouncementView.xib`).

Hirdetmények alapértelmezett kategóriájú cseréjéhez egyszerűen regisztrálhatja a megadott kategória egyéni nézet vezérlő `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Szavazásokra testre szabható ugyanígy néznek ki:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Ez az idő, a megadott `MyCustomPollViewController` kell kiterjesztése `AEPollViewController`. Vagy lehetősége van arra, hogy az alapértelmezett vezérlő terjed ki: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Ne felejtse el vagy hívás `action` (`submitAnswers:` egyéni szavazás nézet vezérlők) vagy `exit` módszer, mielőtt a nézet vezérlő meg van nyitva. Ellenkező esetben nem statisztikai adatokat küldi (tehát nem analytics a marketingkampány meg), és további fontosabb következő kampányok nem jelenik meg értesítés az alkalmazás folyamat újraindításáig.

##### <a name="implementation-example"></a>Példa végrehajtása

Ez a változat az egyéni bejelentése nézet betöltött külső xib fájlból.

Hasonló speciális értesítés testreszabáshoz ajánlott tekintse meg a forráskódot szabványos végrehajtása.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
