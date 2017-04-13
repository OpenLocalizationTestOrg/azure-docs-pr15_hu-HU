<properties
    pageTitle="Azure értesítés hubok Rich leküldéses"
    description="Megtudhatja, hogy miként gazdag leküldéses értesítéseket küldeni az iOS-alkalmazás az Azure. A cél-C a és C# írt mintakódok."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure értesítés hubok Rich leküldéses


##<a name="overview"></a>– Áttekintés

Annak érdekében, hogy a felhasználók azonnali multimédiás tartalom folytatni, az alkalmazások érdemes leküldéses túl az egyszerű szöveg. Az értesítések felhasználói kommunikációt és tartalom bemutatása, például az URL-címeit, hangok, képek vagy kuponokat, és az egyéb előléptetése Ebben az oktatóanyagban épül a [Felhasználók értesítést](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) témakört, és bemutatja, hogyan küldhet a leküldéses értesítéseket, amelyek beépítése tartalmak számára (például képet).


Ebben az oktatóprogramban az iOS 7 és 8 kompatibilis.

  ![][IOS1]

Magas szintű:

1. Az alkalmazás kódmentes:
    - Tárolja a multimédiás tartalom (ebben az esetben kép) az adatbázis/helyi kódmentes tárolására
    - Ez az értesítés gazdag azonosítója küld az eszközön
2. Alkalmazás az eszközön:
    - A multimédiás tartalom kap azonosítójú kérő háttér-kiszolgálói kapcsolatot létesít
    - Felhasználók értesítést küld az eszközön adatbeolvasást befejeződött, és megjeleníti a tartalom azonnal felhasználók koppintson a további


## <a name="webapi-project"></a>WebAPI projekt

1. A Visual Studióban nyissa meg a [Felhasználók értesítést](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyagban hoztunk **AppBackend** projekt.
2. Szerezze be a képet szeretné, hogy a felhasználók értesítése, és helyezze a project- **img** mappát.
3. Kattintson a **Minden fájl megjelenítése** a megoldást Intézőt, és kattintson a jobb gombbal a mappa **felvenni a**projektbe.
4. A kijelölt képpel módosítása a Tulajdonságok ablak összeállítása lépéséről **Beágyazott**erőforrásnézethez.

    ![][IOS2]

5. Az **Notifications.cs**, adja meg a következő utasítással:

        using System.Reflection;

6. A teljes **értesítések** osztály frissítse a következő kódot. Ügyeljen arra, hogy a helyőrzők cseréje a értesítés központi hitelesítő adatait, és a kép fájl nevét.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (nem kötelező) Olvassa el az [beágyazása és erőforrások eléréséhez vizuális C# használatával](http://support.microsoft.com/kb/319292) hogyan vehet fel, és szerezze be a projekt erőforrásait további információt.

7. A **NotificationsController.cs**újra **NotificationsController** , a következő kódrészletek. Ez az első csendes gazdag értesítés azonosító küld az eszköz, és lehetővé teszi, hogy a kép beolvasása ügyféloldali:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Azt fogja újra telepítheti egy Azure webhelyre az alkalmazás minden eszközökről megkönnyítése érdekében. Kattintson a jobb gombbal a **AppBackend** projekten, és válassza a **Közzététel**.

9. Jelölje be a publish cél Azure webhelyet. Jelentkezzen be a Azure-fiókjával, és jelölje ki egy meglévő vagy új webhelyet, és jegyezze fel a **célhely URL-CÍMÉT** tulajdonság a **kapcsolat** lapon. Hivatkozik Ez az URL a *kódmentes végpontot* , később az oktatóprogram. Kattintson a **Közzététel**gombra.

## <a name="modify-the-ios-project"></a>Az iOS projekt módosítása

Most, hogy az alkalmazás kódmentes csak *azonosító* értesítés küldése módosította, akkor az iOS-alkalmazás azonosítóval kezelni, valamint a multimédiás üzenet a kódmentes a változik.

1. Nyissa meg az iOS-projektet, és nyissa meg a fő alkalmazás célba a **célok** területen engedélyezése a távoli értesítéseket.

2. Kattintson a **funkciókat**, **Háttér mód**bekapcsolása és a **Távoli értesítések** jelölőnégyzetet.

    ![][IOS3]

3. Nyissa meg a **Main.storyboard**, és győződjön meg arról, hogy a [Felhasználó értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóprogram nézet vezérlő (említett az otthoni nézet tartományvezérlőnek ebben az oktatóanyagban).

4. **Navigációs vezérlő** hozzáadása a történet, és a Kezdőlap nézetben vezérlő, hogy legyen a **legfelső szintű megtekintése** a navigációs vezérlőelem-húzza. Ellenőrizze, hogy az **Eredeti nézetben van vezérlő** attribútumok felügyelő ki van jelölve a navigációs-vezérlő.

5. Adja hozzá a történet és egy **Képnézet** **Nézet vezérlő** . Ez az, hogy a felhasználók láthatják, miután válassza a további kattintson a notifiication lapot. A történet a következőképpen kell kinéznie:

    ![][IOS4]

6. Kattintson a **Kezdőlap nézetben vezérlő** a történet, és ellenőrizze, hogy **homeViewController** , valamint a **Osztály egyéni** **Történet azonosító** az identitás felügyelő csoportban.

7. Tegye ugyanezt a kép megjelenítése vezérlő **imageViewController**szerint.

8. Ezután hozzon létre egy új nézet vezérlő osztály **imageViewController** az imént létrehozott felhasználói felület kezelése című.

9. A **imageViewController.h**adja hozzá a következő a vezérlő felület deklarációs. Győződjön meg róla, hogy a vezérlőelem-húzza a történet kép nézetben ezeket a tulajdonságokat a két hivatkozás:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. Adja hozzá a következő **imageViewController.m**, **viewDidload**végén:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. A **AppDelegate.m**a kép vezérlő létrehozott importálása:

        #import "imageViewController.h"

12. A következő nyilatkozat-felület szakaszának hozzáadása:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. **AppDelegate**, győződjön meg arról, hogy az alkalmazás a csendes értesítések regisztrálja **alkalmazás: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Parancs az **alkalmazás: didRegisterForRemoteNotificationsWithDeviceToken** következő végrehajtása a történet állapotba felhasználói felület módosítja a fiókba:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Az alábbi módszerek majd hozzá **AppDelegate.m** a kép beolvasása a végpontot, és a helyi értesítés küldése, amikor befejeződött a lekérés. Ügyeljen arra, hogy a helyőrző helyettesítő `{backend endpoint}` a kódmentes végponttal:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Kezelheti a fent helyi értesítés a kép megjelenítése vezérlő a **AppDelegate.m** az alábbi módszerek megnyitása:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Futtassa az alkalmazást

1. XCode futtassa az alkalmazást a fizikai iOS-eszközön (a leküldéses értesítések nem működnek a simulatort használja).

2. Az iOS alkalmazás felhasználói felület adja meg a felhasználónevet és jelszót az azonos értékű hitelesítéshez, és kattintson a **Log In**.

3. Kattintson a **Küldés leküldéses** gombra, és meg kell jelennie a alkalmazás értesítés. Ha a **több**gombra kattint, tudomására a képet úgy döntött, hogy az alkalmazás kódmentes foglalni.

4. **Leküldéses küldése** gombra, és azonnal nyomja meg a Kezdőlap gomb az eszköz. Néhány percet, a leküldéses értesítést kap. Koppintson rá, és kattintson az Egyebek gombra, ha tudomására az alkalmazás és a kép multimédiás tartalom.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
