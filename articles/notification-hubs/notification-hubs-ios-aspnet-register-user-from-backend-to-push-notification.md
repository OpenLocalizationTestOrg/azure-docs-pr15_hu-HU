<properties
    pageTitle="Leküldéses értesítések az aktuális felhasználót regisztrálhatja a webes API segítségével |} Microsoft Azure"
    description="Útmutató: a leküldéses értesítést regisztrációs egy iOS-alkalmazásban az Azure értesítés hubok kérése registeration ASP.NET webes API hajtja végre."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Leküldéses értesítések az aktuális felhasználót regisztrálni ASP.NET használatával

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan kérhet leküldéses értesítést regisztráció Azure értesítés hubok, amikor regisztrációs ASP.NET webes API végzi. Ez a témakör az oktatóprogram [értesítés hubok felhasználók értesítése]kiterjed. Kell már befejezése a szükséges lépéseket, hogy a hitelesített mobilszolgáltatás létrehozása az oktatóanyagban. A felhasználók értesítése forgatókönyv a további tudnivalókért lásd: [értesítés hubok értesítés felhasználókat].

##<a name="update-your-app"></a>Az alkalmazás frissítése  

1. Adja hozzá a MainStoryboard_iPhone.storyboard, az alábbi összetevőket az objektum-tárból:

    + **Címke**: "Leküldéses értesítést hubok felhasználó számára"
    + **Címke**: "Végrehajtott"
    + **Címke**: "Felhasználó"
    + **Szöveges mező**: "Felhasználó"
    + **Címke**: "Jelszó"
    + **Szöveges mező**: "Jelszó"
    + **Gomb**: "Bejelentkezési"

    Ezen a ponton a történet formátumban, például a következőket:

    ![][0]

2. Segéd szerkesztőben létrehozása az kapcsolt vezérlők csatlakozó aljzatokkal, és hívja őket, a szöveg mezők a nézet vezérlővel (delegált), hozhat létre, és a **bejelentkezési** gombhoz tartozó **művelet** .

    ![][1]

    A BreakingNewsViewController.h fájl már neveket is tartalmazó a következő kódot:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Hozzon létre egy **DeviceInfo**nevű osztály, és másolja a vágólapra az alábbi kódot a felület szakaszba DeviceInfo.h fájl:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. DeviceInfo.m fájl végrehajtás szakaszához másolja a következő kódot:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. Adja hozzá a következő tulajdonság egyszeres PushToUserAppDelegate.h:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. A PushToUserAppDelegate.m **didFinishLaunchingWithOptions** módszer adja hozzá a következő kódot:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    Az első sor előkészíti a **DeviceInfo** egyszeres. A második sorban a regisztráció indítja el, a leküldéses értesítéseket, amely már bemutató már elvégezték az [Első lépések az értesítési hubok] oktatóprogram.

9. A módszer **didRegisterForRemoteNotificationsWithDeviceToken** megvalósítását a a AppDelegate PushToUserAppDelegate.m, és adja hozzá a következő kódot:

        self.deviceInfo.deviceToken = deviceToken;

    Az eszköz jogkivonathoz a kérelem állít be.

    > [AZURE.NOTE] Ezen a ponton nem kell bármely más kódot ezzel a módszerrel. Ha már van hívást kezdeményez, a program által az [Első lépések az értesítési hubok](/manage/services/notification-hubs/get-started-notification-hubs-ios/) oktatóprogram befejeződésekor **registerNativeWithDeviceToken** módszer, kell Megjegyzés meg, vagy távolítsa el a hívás.

10. Adja hozzá a következő kezelő módszer a PushToUserAppDelegate.m fájlt:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Ez a módszer értesítés felhasználói felület jeleníti meg, amikor az alkalmazás értesítést kap, futása közben is.

9. Nyissa meg a PushToUserViewController.m fájlt, és a billentyűzet vissza az alábbi végrehajtása:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. A **viewDidLoad** módszer a PushToUserViewController.m fájlban inicializálni végrehajtott felirat az alábbi képlettel történik:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Adja meg a következő tulajdonságokat PushToUserViewController.m felület:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Ezután adja hozzá a következő végrehajtása:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. A **bejelentkezési** kezelő módszer XCode által létrehozott másolja be a következő kódot:

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Ez a módszer egy Telepítésazonosítót és a csatorna kapja a leküldéses értesítések, és elküldi, az eszköz típusa, valamint a hitelesített webes API-módszerrel, amelyet a regisztrációkor létrehoz az értesítési hubok. A webes API [értesítés hubok felhasználók értesítése]lett megadva.

Most, hogy az ügyfél-alkalmazás frissítése térjen vissza az [értesítési hubok felhasználók értesítése] , és frissítse a mobilszolgáltatás értesítés küldése értesítés hubok használatával.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Az értesítés hubok felhasználók értesítése]: /manage/services/notification-hubs/notify-users-aspnet

[Értesítés hubok – első lépések]: /manage/services/notification-hubs/get-started-notification-hubs-ios
