<properties
    pageTitle="Értesítés hubok legfrissebb híreket oktatóanyag – iOS"
    description="Megtudhatja, hogy miként legfrissebb híreket értesítést küld az iOS-eszközök Azure Service Bus értesítés hubok használatával."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Legfrissebb hírek küldése értesítés hubok használatával

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan legfrissebb híreket értesítések iOS-alkalmazás közvetítése Azure értesítés hubok használatával. Amikor elkészült, használni tudja regisztrálni érdeklik hírek kategóriák megszakítása, és csak a kategóriákhoz tartozó leküldéses értesítések fogadása. Ebben az esetben az sok alkalmazások közös mintájának, ahol értesítések kell el lehet küldeni a csoportok a felhasználók, amely korábban már van deklarálva érdekes őket, például az RSS-olvasó, az alkalmazások zenét ventillátortól stb.

Egy vagy több _címkék_ felvételével, az értesítési-központban regisztráció létrehozásakor közvetítési esetek engedélyezett. Értesítések fogadására egy címkét, ha az összes eszközök regisztrálta a címke a értesítést kap. Mivel a címkék egyszerűen karakterláncok, nem rendelkeznek, hogy előre be kell építenie. Címkékkel kapcsolatos további tudnivalókért olvassa el az [értesítési hubok Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Előfeltételek

Ez a témakör az alkalmazást, az [első lépések az értesítési hubok]létrehozott épül[get-started]. Ebben az oktatóanyagban indításához kell már befejezése [első lépések az értesítési hubok][get-started].

##<a name="add-category-selection-to-the-app"></a>Az alkalmazás hozzáadása a kategória kiválasztása

Első lépésként a Felhasználóifelület-elemek hozzáadása a meglévő történet, amelyek lehetővé teszik a felhasználó számára, válassza a kategóriák regisztrálni. A kategóriák a felhasználó által választott tárolják az eszközön. Az alkalmazás indításakor eszköz regisztráció címkékként a értesítés központ a kijelölt kategóriával jön létre.

1. Adja hozzá a MainStoryboard_iPhone.storyboard az objektum könyvtárból az alábbi összetevőket:
    + Címke "Megszakítása hírek" szöveggel
    + "Nemzetközi", "Politikában", "Üzleti", "Technológia", "Tudományos", "Sports", a kategória szövegeket tartalmazó címkék
    + Hat kapcsolók, egy kategóriát, adja meg mindegyik kapcsolót **állam** alapértelmezés szerint **ki** kell.
    + Egy gomb címkézett "Előfizetés"

    A történet a következőképpen kell kinéznie:

    ![][3]

2. Segéd szerkesztőben létrehozása az összes kapcsolót csatlakozó aljzatokkal, és hívja őket "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. A "előfizetés" nevű gombhoz tartozó művelet létrehozása. A ViewController.h tartalmaznia kell a következő:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Hozzon létre egy új néven **Kakaó érintéses osztály** `Notifications`. Másolja a következő kódot a fájl Notifications.h felület szakaszában.

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Adja hozzá a következő importálása irányelvet Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Másolja a következő kódot Notifications.m fájl végrehajtás szakaszához.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Az osztály használja a helyi tároló tárolni, valamint hírt, amelyhez ez az eszköz fog kapni a kategóriába. Emellett az ezekben a kategóriákban, egy [sablon](notification-hubs-templates-cross-platform-push-messages.md) regisztráció segítségével regisztrálhatja a módszer tartalmaz.

7. A AppDelegate.h fájlban adja hozzá az importálás utasítás Notifications.h, és az értesítések osztály egy példányának tulajdonság hozzáadása:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. Az a AppDelegate.m **didFinishLaunchingWithOptions** módszer adja meg az értesítések példányt az elejére a módszer inicializálni a kódot.  
 
    `HUBNAME`és `HUBLISTENACCESS` (hubinfo.h megadva) kell már rendelkezik a `<hub name>` és `<connection string with listen access>` *DefaultListenSharedAccessSignature* korábbi szerezte be az értesítési központi felhasználónevét és a kapcsolati karakterlánc helyett helyőrzők

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Mivel a hitelesítő adatait, amelyek egy ügyfél alkalmazással nem általában biztonságos, csak kell terjesztése meghallgatását access kulcsa az ügyfél-alkalmazást. Meghallgathatja access lehetővé teszi, hogy az értesítések, de a meglévő regisztrációk regisztrálhatja az alkalmazás nem módosíthatók, és nem lehet küldeni a értesítések. A teljes hívóbetű értesítések küldésére, és módosítása meglévő regisztrációk védett kódmentes szolgáltatás használják.


9. A AppDelegate.m **didRegisterForRemoteNotificationsWithDeviceToken** módszer cserélje ki az értesítések osztály a eszköz jogkivonat átadni a következő kódot módszer a kódot. Az értesítések osztály végez regisztrálása a kategóriával az értesítésekhez. Ha a felhasználó megváltoztatja a kategória beállításokat, hogy hívja fel a `subscribeWithCategories` módszer válaszként frissíteni őket, hogy a **Feliratkozás** gombra.

    > [AZURE.NOTE] Az eszköz jogkivonathoz által az Apple leküldéses értesítést szolgáltatás (APNS) rendelt bármikor is lehetőséget, mert regisztrálnia kell gyakran az értesítési hibák elkerülése érdekében az értesítésekhez. Ez a példa regisztrál az értesítés, valahányszor elindítja az alkalmazást. Gyakran futtatott alkalmazásai többször nap, valószínűleg kihagyhatja regisztrációs sávszélesség megőrzése, ha az előző regisztrációhoz óta eltelt kisebb, mint nap.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Figyelje meg, hogy ezen a ponton nem kell nincs más kód **didRegisterForRemoteNotificationsWithDeviceToken** módszer.

10. Az alábbi módszerek már az [első lépések az értesítési hubok] befejezését AppDelegate.m jelen kell lenniük[ get-started] oktatóprogram.  Ha nem, vegye fel őket.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Ez a módszer az alkalmazás egy egyszerű **UIAlert**megjelenítésével futtatásakor kapott értesítések kezeli.

11. Adja hozzá egy importálási utasítás AppDelegate.h ViewController.m, és másolja be az alábbi kódot a **Feliratkozás** XCode által generált módszer. Ez a kód frissíti az értesítési regisztrációs használni az új kategória címkéket a felhasználó úgy döntött, a felhasználói felületen.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Ez a módszer létrehoz egy **NSMutableArray** kategóriát, és a listában a helyi tároló tárolására használja az **értesítések** osztály, és a megfelelő címkék regisztrálja az értesítési központi. A kategóriák megváltozásakor a rendszer az új kategóriák újra a regisztráció.

12. Az ViewController.m adja meg a **viewDidLoad** módszer beállítása a felhasználói felület a korábban mentett kategóriák alapján a következő kódot.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Az alkalmazás letöltése tárolhatja kategóriák halmazának regisztrálása az értesítési-központban, amikor elindítja az alkalmazást használt eszköz helyi tárolására.  A felhasználó a kijelölés futásidőben kategóriák módosíthatja, és kattintson az **előfizetés** mód az eszköz a regisztráció frissítése. Ezután a legfrissebb híreket értesítéseket küldeni közvetlenül az alkalmazásban megjelenik az alkalmazás frissíti.


##<a name="optional-sending-tagged-notifications"></a>(nem kötelező) Címkézett értesítések küldése

Ha nincs Visual Studio való hozzáférést, ugorjon a következő részre, és értesítések küldése magát az alkalmazásból. Is küldhet a megfelelő sablon értesítés az [Azure klasszikus portál] hibakeresési lapján a értesítés-központját. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(nem kötelező) Értesítés küldése az eszközről

A szokásos módon értesítéseket szeretne küldeni kódmentes szolgáltatás által, de a alkalmazásból közvetlenül is elküldheti a legfrissebb híreket értesítéseket. Ehhez frissíteni fogjuk a `SendNotificationRESTAPI` módszer a az [első lépések az értesítési hubok] definiált[ get-started] oktatóanyagból.


1. Frissítés a ViewController.m a `SendNotificationRESTAPI` módszerrel történik, mint követi, hogy elfogadja a kategória címke paraméter és a megfelelő [sablon](notification-hubs-templates-cross-platform-push-messages.md) értesítést küld.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }



2. A ViewController.m frissíti az **Értesítés küldése** művelet a következő kódot látható módon. Hogy azt az egyes címke egyenként használatával értesítések küldésére, és több platformokon küldeni.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. A projekt újjáépítése, és ellenőrizze, hogy build hibátlan van.


##<a name="run-the-app-and-generate-notifications"></a>Futtassa az alkalmazást, és értesítéseket

1. A Futtatás gombra kattintva hozza létre a projekt, és indítsa el az alkalmazást. Jelölje ki néhány legfrissebb híreket beállításokat előfizetés, és nyomja le az **előfizetés** gombra. Az értesítések előfizetett jelző üzenet jelenik meg.

    ![][1]

    Úgy dönt, hogy **az előfizetés**, amikor az alkalmazás a kijelölt kategóriák konvertálja a címkék, és a kijelölt címkék új eszköz regisztráció kéri a értesítés központból.

2. Írja be az üzenet küldése a legfrissebb híreket-ként, majd nyomja le a az **Értesítés küldése** gombra. Azt is megteheti futtassa a .NET konzol alkalmazás értesítések létrehozásához.

    ![][2]


3. Egyes eszközök fizetett elő, hírek megszüntetése a legfrissebb híreket értesítések csak küldött kap.



## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban legfrissebb híreket közvetítésével kategória szerint megtanulta azt. Vegye figyelembe az alábbi oktatóanyagok más speciális értesítés hubok esetben kiemelő egyik befejezése:

+ **[Értesítés hubok segítségével honosított legfrissebb híreket közvetítése]**

    Megtudhatja, hogy miként bontsa ki a legfrissebb híreket alkalmazás küldő honosított értesítések engedélyezése.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Értesítés hubok segítségével honosított legfrissebb híreket közvetítése]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure klasszikus portál]: https://manage.windowsazure.com
