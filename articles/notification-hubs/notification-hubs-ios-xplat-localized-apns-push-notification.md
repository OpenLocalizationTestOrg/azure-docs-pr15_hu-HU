<properties
    pageTitle="Értesítés hubok honosított megszakítása hírek oktatóprogram IOS"
    description="Megtudhatja, hogy miként Azure Service Bus értesítés hubok küldhet honosított legfrissebb híreket értesítések (iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Küldhet értesítést hubok honosított legfrissebb hírek a iOS-eszközökhöz

> [AZURE.SELECTOR]
- [A Windows áruházból C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan funkcióval a [sablonok](notification-hubs-templates-cross-platform-push-messages.md) az Azure értesítés hubok közvetítése, nyelvi és eszköz honosított legfrissebb híreket értesítések. Ebben az oktatóanyagban megkezdése az iOS alkalmazásban létrehozott [Használata értesítés hubok küldése legfrissebb híreket]. Amikor elkészült, fogja tudni érdeklik kategóriák regisztrálhat, adja meg a nyelvet, amelyen az értesítéseket, és csak a kijelölt kategóriák leküldéses értesítések fogadása a választott nyelven.


Ebben az esetben a két részből áll:

- iOS-alkalmazás lehetővé teszi, hogy ügyfél adjon meg egy nyelvet, és más legfrissebb híreket kategóriák; előfizetés eszközök

- a háttéradatbázist az értesítéseket, használja az Azure értesítés hubok **címke** és a **sablonok** feautres szórások.



##<a name="prerequisites"></a>Előfeltételek

Meg kell már elvégezték az [Használata értesítés hubok legfrissebb híreket küldése] oktatóprogram, és a kód érhető el, mivel ez az oktatóanyag közvetlenül épít kódot.

Visual Studio 2012 vagy újabb verziója nem kötelező.



##<a name="template-concepts"></a>Sablon fogalmak

[Használati értesítés hubok legfrissebb híreket küldése] a beépített különböző hírek kategóriák kapcsolatos értesítések előfizetését **címkék** használt alkalmazás.
Sok alkalmazásokat, azonban több piacokon Megcélozhat és honosítási szükséges. Ez azt jelenti, hogy a tartalom az értesítés maguk kell honosított és a megfelelő színkészletet eszközök kézbesítve.
Ebben a témakörben bemutatjuk a **sablon** funkció az értesítési hubok használatáról a könnyen előadásához honosított legfrissebb híreket értesítéseket.

Megjegyzés: egy honosított értesítések küldésére módja minden címke több verziójának létrehozásához. Például angol, francia és Mandarin támogatásához volna szükséges három különböző címkék az híreket: "world_en", "world_fr", és a "world_ch". Azt szeretné majd el kell küldenie a híreket honosított verzióját minden további címkéket. Ez a témakör a sablonok elkerülése érdekében a száma korlátok, címkék között, és több üzenetet küldeni kötelező használjuk.

Magas szintű sablonok hatékonyan megadhatja, hogy egy adott eszköz egy értesítést kell kapniuk. A sablon a pontos tartalom formátumát hivatkoznia kell az alkalmazás háttéradatbázist által küldött üzenet részét képező tulajdonságok adja meg. Ebben az esetben azt tartalmazó összes támogatott nyelvek területi-felismerése nélkül üzenetet küld el:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ezután azt fogja győződjön meg arról, hogy a megfelelő tulajdonságot hivatkozó sablon regisztrálja eszközök. Például az iOS-alkalmazás, amely regisztrálni szeretne francia híreket regisztrálja a következőket:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Sablonok olyan talál további információt a saját [sablonok](notification-hubs-templates-cross-platform-push-messages.md) cikkben nagyon nagy teljesítményű funkciót.

##<a name="the-app-user-interface"></a>Az alkalmazás felhasználói felületén

Most módosítja, akkor a megszakítása hírek alkalmazás a következő témakörben [Használata értesítés hubok legfrissebb híreket küldése] küldése honosított legfrissebb híreket sablonok használatával létrehozott.


A MainStoryboard_iPhone.storyboard, adja meg a három nyelven, amelyek a támogatott diagramtípusról szegmentált vezérlőelem: angol, francia és Mandarin.

![][13]

Majd győződjön meg arról, hogy a ViewController.h vehet fel egy IBOutlet alább látható módon:

![][14]

##<a name="building-the-ios-app"></a>Az iOS-alkalmazás létrehozása


1. A Notification.h vegye fel a *retrieveLocale* módszer, és az üzlet módosítása lehetőséget, és módszerek előfizetés az alább látható módon:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    A Notification.m módosítsa a *storeCategoriesAndSubscribe* módszer a területi paraméter hozzáadásával, és a felhasználói Alapértelmezések tárolja őket:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Ha meg szeretné jeleníteni a területi *előfizetés* módszer majd módosítása:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Fontos tudni, hogyan azt most használ módszer *registerTemplateWithDeviceToken* *registerNativeWithDeviceToken*helyett. Ha azt sablonként regisztrálhat felkínálunk adja meg a json-sablont, és a sablon nevét (mint az alkalmazás különböző sablont rögzítéséhez célszerű). Ellenőrizze a kategóriák regisztrálhatja a címkéket, mint, ügyeljen arra, hogy az adott híreket notifciations fogadása szeretnénk.

    Adja hozzá a módszer a területi beolvasásához a felhasználói alapértelmezett beállításait:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Most, hogy az értesítések osztály módosította azt, hogy kell győződjön meg arról, hogy a ViewController tesz az új UISegmentControl. Adja meg a következő sort a *viewDidLoad* módszer, hogy az éppen kiválasztott területi megjelenítéséhez:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Ezután a *Feliratkozás* mód módosítsa a hívás átirányítása a *storeCategoriesAndSubscribe* a következő:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Végül, frissítenie kell a AppDelegate.m, a *didRegisterForRemoteNotificationsWithDeviceToken* módszer, hogy helyesen frissíthet a regisztrációt az alkalmazás indításakor. A hívás átirányítása a *Feliratkozás* módja a következő értesítések módosítása:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(nem kötelező) .NET-konzol alkalmazásból honosított sablon értesítés küldése

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(nem kötelező) Honosított sablon értesítés küldése az eszközről

Ha nem fér hozzá a Visual Studióban, vagy csak szeretne tesztelje az honosított sablon értesítés küldése a alkalmazásból közvetlenül az eszközön.  Egyszerű is honosított sablon paramétereinek hozzáadásához a `SendNotificationRESTAPI` módszer az előző oktatóanyagban megadott.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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




## <a name="next-steps"></a>Következő lépések

Sablonok használatával kapcsolatos további tudnivalókért lásd:

- [Értesítés hubok a felhasználók értesítése: az ASP.NET]
- [Értesítés hubok a felhasználók értesítése: Mobile szolgáltatások]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Legfrissebb hírek küldése értesítés hubok használatával]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Értesítés hubok a felhasználók értesítése: az ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Értesítés hubok a felhasználók értesítése: Mobile szolgáltatások]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
