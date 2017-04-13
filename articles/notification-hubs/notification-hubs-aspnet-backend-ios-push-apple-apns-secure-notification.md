<properties
    pageTitle="Azure értesítés hubok biztonságos leküldéses"
    description="További információ az iOS-alkalmazás az Azure biztonságos leküldéses értesítéseket küldeni. A cél-C a és C# írt mintakódok."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure értesítés hubok biztonságos leküldéses

> [AZURE.SELECTOR]
- [A Windows univerzális](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-alapú](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>– Áttekintés

Leküldéses értesítést támogatása a Microsoft Azure lehetővé teszi, hogy könnyen használható, többplatformos, amelyekkel méretarányos telepítés leküldéses infrastruktúra nagyban egyszerűsíti a leküldéses értesítések egyszerre fogyasztói és vállalati alkalmazások mobil platformokon történő elérésére.

Szabályozási miatt vagy biztonsági kényszerek, előfordul, hogy az alkalmazás előfordulhat, hogy szeretne foglalni valamit, amit az ablakokat, amelyek nem kell továbbítani a szabványos leküldéses értesítést infrastruktúra keresztül. Ebben az oktatóanyagban az ugyanazon élmény elérése küld a bizalmas információkat az ügyféleszközről és az alkalmazás kódmentes közötti biztonságos, hitelesített kapcsolaton keresztül ismerteti.

Magas szintű a folyamat a következőképpen történik:

1. Az alkalmazás háttéradatbázist:
    - Tárolók biztonságos tartalom háttéradatbázist.
    - Ez az értesítés azonosítója küld az eszköz (nem biztonságos küld adatokat a).
2. Az alkalmazás az eszközön, amikor a értesítést kap:
    - Az eszköz csatlakozik a háttéradatbázist kérése a biztonságos tartalom.
    - Az alkalmazás megjelenítheti a tartalom, az eszközön értesítést.

Fontos tudni, hogy a fenti folyamat (és ebben az oktatóanyagban) feltételezzük, hogy az eszköz tárol egy hitelesítési jogkivonat helyi tároló után a felhasználó bejelentkezik a. Ez a teljesen zökkenőmentes változat, garantálja, mint az eszközre az értesítés biztonságos tartalom a token használatával meghallgathatja. Ha az alkalmazás nem tárolja hitelesítési tokenek az eszközre, vagy ha tokenek lejárt az eszköz alkalmazást, az értesítés fogadásakor megjelenjen-e egy általános értesítést a felhasználótól indítsa el az alkalmazást. Az alkalmazás hitelesíti a felhasználót, majd az értesítési tartalom jeleníti meg.

A biztonságos leküldéses szabhatja biztonságosan leküldéses értesítést küldhet. Az oktatóprogram, célszerű lépések elvégzése, hogy az oktatóanyagban először a [Felhasználók értesítést](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóprogram épül.

> [AZURE.NOTE] Ebben az oktatóanyagban feltételezi, hogy létrehozása és beállítása a értesítés központi, [Értesítés hubok (iOS) – első lépések](notification-hubs-ios-apple-push-notification-apns-get-started.md)leírt módon.

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Az iOS projekt módosítása

Most, hogy az alkalmazás háttéradatbázist csak *azonosító* értesítés küldése módosított kell kezelni az értesítés és visszahívhatja a háttéradatbázist beolvasni a biztonságos üzenet jelenik meg az iOS-alkalmazás módosítása.

A cél elérését van a tartalom biztonságáról beolvasni a alkalmazás háttéradatbázist logika írni.

1. **AppDelegate.m**győződjön meg arról, hogy az alkalmazás nyilvántartások csendes értesítéseket, így az értesítési azonosító feldolgozásával küldünk a kódmentes. Adja meg a **UIRemoteNotificationTypeNewsstandContentAvailability** beállítást didFinishLaunchingWithOptions:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. A **AppDelegate.m** adja hozzá egy végrehajtása szakaszt a következő nyilatkozat a tetején:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Majd ezt hozzáadhatja a végrehajtás szakaszában a következő kódot, helyettesítése a helyőrző `{back-end endpoint}` a végponttal esetében a háttéradatbázist korábban kapott:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Most van kezelje a bejövő értesítést, és a fenti módszer segítségével beolvashatja a tartalom megjelenítéséhez. Először is azt engedélyeznie kell az iOS-alkalmazás futtatásához a háttérben, amikor a leküldéses értesítést kap. **XCode**jelölje ki a bal oldali ablaktáblában kattintson az alkalmazás projektet, majd kattintson a fő alkalmazás cél a központi ablakból a **célok** területen.

5. Ezután kattintson a **Funkciók** fülre, a középső ablak tetején, és a **Távoli értesítések** jelölőnégyzetet.

    ![][IOS1]


6. **AppDelegate.m** adja hozzá a következő módszerrel a leküldéses értesítések kezelése:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Ne feledje, hogy hiányzó hitelesítési élőfej tulajdonság, illetve elutasításra eseteit kezelheti a háttéradatbázist által előnyben részesíteni. Ezekben az esetekben az adott kezelését attól függenek, főleg a cél felhasználói felület. Egy, hogy értesítés megjelenítése egy általános rákérdező hitelesítést végezni a felhasználó számára beolvasása a tényleges értesítés.

## <a name="run-the-application"></a>Futtassa az alkalmazást

Az alkalmazás futtatásához tegye a következőket:

1. XCode futtassa az alkalmazást a fizikai iOS-eszközön (a leküldéses értesítések nem működnek a simulatort használja).

2. Az iOS alkalmazás felhasználói felület adja meg egy felhasználónevet és jelszót. Ezek a karakterlánc lehet, de ugyanazt az értéket kell lenniük.

3. Kattintson az iOS alkalmazás felhasználói felület, **Jelentkezzen be**. Kattintson a **Küldés a leküldéses**. Meg kell jelennie a biztonságos értesítés az értesítési központban megjelenítését.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
