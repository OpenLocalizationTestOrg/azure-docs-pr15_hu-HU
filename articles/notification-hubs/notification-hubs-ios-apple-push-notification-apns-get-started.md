<properties
    pageTitle="Leküldéses értesítések küld az Azure értesítés hubok iOS |} Microsoft Azure"
    description="Ebből az oktatóanyagból megismerheti, hogyan Azure értesítés hubok segítségével küldje el a leküldéses értesítések iOS az alkalmazás."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="leküldéses értesítések leküldéses értesítéseket, ios leküldéses értesítések"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Az Azure értesítés hubok iOS küld a leküldéses értesítések

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Ebben az oktatóanyagban bemutatja, hogyan Azure értesítés hubok segítségével küldje el a leküldéses értesítések iOS az alkalmazás. Kell létrehoznia egy üres iOS-alkalmazás, amely a leküldéses értesítések fogadása az [Apple leküldéses értesítéseket kezelő szolgáltatása (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)használatával. 

Ha végzett, is használhatja az értesítési központi szétküldése fut, az alkalmazás az összes eszközök leküldéses értesítéseket.

## <a name="before-you-begin"></a>Első lépések

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Ebben az oktatóanyagban bejegyzett kódját [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted)megtalálható. 

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban az alábbiakra van szükség:

+ [Mobil szolgáltatások iOS SDK 1.2.4 verziója]
+ [Xcode] legújabb verziója
+ IOS 8 (vagy újabb verzió)-kompatibilis eszköz
+ [Apple Fejlesztőeszközök Program](https://developer.apple.com/programs/) tagság.

   > [AZURE.NOTE] Leküldéses értesítések konfigurálása követelményei, mert üzembe helyezéséhez és tesztelje a fizikai iOS-eszközön (iPhone vagy iPad) a leküldéses értesítések helyett az iOS simulatort használja.

Ebben az oktatóanyagban befejezése rendszer minden más értesítés hubok oktatóanyagok az iOS-alkalmazások.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Az értesítés központi iOS leküldéses értesítések konfigurálása

Ez a szakasz részletes információt tartalmaz egy új értesítés hubhoz létrehozása és konfigurálása a hitelesítést az Ön által létrehozott **.p12** leküldéses tanúsítvány használatával APN keresztül. Ha már létrehozott egy értesítés központi használni kívánt, kihagyhatja az 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Kattintson a <b>Beállítások</b> lap <b>Értesítési szolgáltatások</b> gombra, majd jelölje be az <b>Apple (APNS)</b>. Kattintson a <b>Tanúsítvány tölthet fel</b> , és jelölje ki a korábban exportált <b>.p12</b> fájlt. Győződjön meg arról, hogy a helyes jelszót is meg.</p>
<p>Győződjön meg arról, jelölje be a <b>védőfalas</b> üzemmód, mivel ez a fejlesztés. A <b>gyártási</b> csak akkor használja, ha el szeretné küldeni a leküldéses értesítések azoknak a felhasználóknak, az alkalmazás a áruházból vásárló.</p>
</li>
</ol>
&emsp;&emsp;![Állítsa be APNS az Azure-portálon](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![APN-tanúsítvány beállítása az Azure-portálon](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Az értesítés központi most úgy van beállítva az APN való használatra, és a kapcsolat karakterláncok regisztrálhat az alkalmazást, és küldje el a leküldéses értesítések van.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Az iOS-alkalmazás csatlakoztatása értesítés hubok

1. IOS új projekt létrehozása Xcode, és válassza ki az **Alkalmazás megtekintése egy** sablont.

    ![Xcode - alkalmazás egyetlen megtekintése][8]

2. Az új projekt beállításokról, ügyeljen az használni, **Termék neve** és a **Szervezet azonosítója** , ha korábban állít be az első lépésekhez azonosító az Apple Developer portal használt.

    ![Xcode – a project beállításai][11]

3. **Cél**, csoportban kattintson a projekt nevére, kattintson a **Szerkesztés Beállítások** fülre és bontsa ki a **Kód aláírása identitás**és majd **hibakeresési**, területen adja meg a kód aláírása személyazonosság. Váltás a **szintek** **az** **összes**, és a **Kiépítési profil** beállítása a korábban létrehozott kiépítési profilhoz.

    Ha nem látja az új kiépítési profil Xcode létrehozott, frissítse a profilok az aláírási identitáshoz. A menüsávon kattintson a **Xcode** , kattintson a **Beállítások**gombra, kattintson a **fiók** fülre, kattintson a **Részletek** gombra, kattintson az aláíró identitás, és kattintson a frissítés gombra a jobb alsó sarokban.

    ![Xcode – kiépítési profil][9]

4. A [mobil szolgáltatások iOS SDK 1.2.4 verzió] letöltése, és bontsa ki a fájlt. Kattintson a jobb gombbal a projekt Xcode, és kattintson a **Fájlok hozzáadása** a **WindowsAzureMessaging.framework** mappa hozzáadása a Xcode projekt lehetőségre. Jelölje ki, **szükség esetén elemeket másolni**, és kattintson a **Hozzáadás**gombra.

    >[AZURE.NOTE] Az értesítés hubok SDK jelenleg nem támogatja bitcode Xcode 7.  Be kell **Engedélyezése Bitcode** **nincs** a **Összeállítása beállítások** beállítása a projekthez.

    ![Bontsa ki az Azure SDK][10]

5. Új fájl élőfej hozzáadása a projekthez, nevű `HubInfo.h`. Ez a fájl az állandók az értesítési központi az tartalmazni fogja.  Adja hozzá a következő definíciókat, és a karakterlánc konstans helyőrzők cseréje a *központi neve* és a korábban feljegyzett *DefaultListenSharedAccessSignature* .

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Nyissa meg a `AppDelegate.h` fájl hozzáadása a következő importálása irányelvek:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. Az a `AppDelegate.m file`, a következő kód hozzáadása a `didFinishLaunchingWithOptions` módszer az iOS verziójába alapján. Ez a kód APN regisztrálja az eszköz fogópont:

    Az iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    Az iOS-verzió 8 előtt:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. Ugyanannak a fájlnak vegye fel az alábbi módszerekkel. Ez a kód a kapcsolat adatai alapján HubInfo.h megadott értesítés-hubhoz csatlakozik. Majd lehetőséget kínál a eszköz jogkivonat értesítés-hubon keresztül csatlakozott, hogy az értesítési-központban értesítéseket küldheti:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. Ugyanannak a fájlnak adja hozzá a következő módszerrel, ha szeretne megjeleníteni egy **UIAlert** az értesítés érkezik, amíg az alkalmazás aktív:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Építse fel, és futtassa az alkalmazást az eszközön, és ellenőrizze, hogy nincs hibák.

## <a name="send-test-push-notifications"></a>Küldje el a próba leküldéses értesítések


Ellenőrizheti, hogy az alkalmazás értesítésekkel küld a leküldéses értesítések az [Azure-portálon] keresztül a **Hibaelhárítás** a központi lap (a *Teszt küldése* beállítást kell használni) szakaszt.

![Azure portál - próba küldése][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Nem kötelező) Küldje el a leküldéses értesítéseket a alkalmazásból

>[AZURE.IMPORTANT] Ebben a példában az értesítés küldése a ügyfél alkalmazásból hiányzik tanulási csak célokat. Mivel az ehhez szükséges a `DefaultFullSharedAccessSignature` az ügyfél-alkalmazás telepítve legyen a kockázat, hogy a felhasználó hozzáférhet jogosulatlan értesítést küld az ügyfelek a értesítés elosztót teszi lehetővé.

Az alkalmazás belül a leküldéses értesítéseket küldeni szeretné, ha ez a témakör ennek módjáról a többi csatolófelület, például.

1. Nyissa meg az Xcode, `Main.storyboard` és a következő felhasználói felület elemeinek felvétele a leküldéses értesítéseket küldeni az alkalmazás a felhasználó objektum-tárból:

    - Egy adatfeliratra, amelynek nem címke szövegét. Jelentés hibáinak értesítést küld lesz. A **sorok** tulajdonság **0** kell beállítani, hogy a program automatikusan méret korlátozódik jobbra és bal margó és a nézet tetején.
    - Egy szövegmező, ahová a **helyőrző** szöveg állítsa **Értesítő üzenet adja meg**. A felirat alatti mező lehetőségeket, alább látható módon. Állítsa be a nézet-vezérlőt kilépő meghatalmazottként.
    - **Értesítés küldése** című gomb közvetlenül a szövegmező alatt, és a vízszintes center korlátozódik.

    A nézet a következőképpen kell kinéznie:

    ![Xcode tervezőben][32]


2. [Hozzáadása csatlakozó aljzatokkal](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) a címke és a szövegmező mező csatlakozik a nézetben, és frissítse a `interface` meghatározása a támogatási `UITextFieldDelegate` és `NSXMLParserDelegate`. Adja hozzá a három tulajdonság nyilatkozatokat alább látható, hogy hívja fel a REST API-t, és a válasz elemzése támogatást.

    A ViewController.h fájlt a következőképpen kell kinéznie:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Nyissa meg `HubInfo.h` , és adja hozzá a következő állandók, amelyek értesítést küld a központi használható. A tényleges *DefaultFullSharedAccessSignature* kapcsolati karakterláncot cserélje le a helyőrző karakterláncot.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Adja hozzá a következő `#import` utasítások a `ViewController.h` fájlt.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. A `ViewController.m` a következő kód hozzáadása a felület végrehajtása. Ez a kód fog elemzi a *DefaultFullSharedAccessSignature* kapcsolati karakterlánc. A [REST API-hivatkozás](http://msdn.microsoft.com/library/azure/dn495627.aspx)említett, elemzett információk kattintva állítsa elő a **engedélyezési** kérelem fejléchez társítások token lesz.

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. A `ViewController.m`, frissítse a `viewDidLoad` módszer a kapcsolati karakterlánc elemzésére, a nézet betöltésekor. A segédprogram módszerek, a felület végrehajtásával alább is hozzáadhat.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. A `ViewController.m`, az alábbi kód hozzáadása a társítások engedélyezési jogkivonat **engedélyezési** fejlécen kapott létrehozásához a [REST API-hivatkozás](http://msdn.microsoft.com/library/azure/dn495627.aspx)említett kapcsolat végrehajtása.

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + a húzás **Értesítés küldése** gombra kattintva `ViewController.m` **SendNotificationMessage** nevű az **Érintéses lefelé** esemény művelet hozzáadása. Frissítse az módszer a REST API értesítés küldése a következő kódot.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. A `ViewController.m`, adja hozzá a következő meghatalmazott módszer a támogatási a szövegmező a billentyűzet bezárása. A CTRL + a szövegmezőbe a nézet vezérlő ikonra, a felület-szerkesztőben, a nézet vezérlő kilépő meghatalmazottként beállítandó egér húzásával.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. A `ViewController.m`, adja hozzá a következő meghatalmazott módszerekkel támogatja, a válasz elemzése használatával `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Hozza létre a projekt, és ellenőrizze, hogy nincsenek-e hibák.


> [AZURE.NOTE] Ha egy összeállítás hiba Xcode7 bitcode támogatásával kapcsolatos, módosítania kell a **Összeállítása beállítások** > **Engedélyezése Bitcode (ENABLE_BITCODE)** Xcode **nem** értékűre. Az értesítés hubok SDK jelenleg nem támogatja a bitcode. 

A lehetséges értesítés tartalmak számára az Apple [helyi és a leküldéses értesítést programozási útmutató]talál.


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Ha az alkalmazás kaphatnak a leküldéses értesítések ellenőrzése

Tesztelje a iOS leküldéses értesítéseket, telepítenie kell az alkalmazás fizikai iOS-eszközére. Az iOS simulatort használja használatával nem küldhet az Apple leküldéses értesítéseket.

1. Futtassa az alkalmazást, és ellenőrizze, hogy regisztrációs sikerül, majd kattintson az **OK gombra**.

    ![iOS alkalmazás leküldéses értesítést regisztrációs tesztelése][33]

2. Az [Azure-portálon]próba leküldéses értesítést küldhet, fentebb ismertetett. Ha hozzáadta az alkalmazásban a leküldéses értesítések küldésének kódot, érintse meg értesítő üzenet megadása szöveg mezőbe. Nyomja le a billentyűzet vagy az **Értesítés küldése** gomb az értesítő üzenet küldése nézetében kattintson a **Küldés** gombra.

    ![iOS alkalmazás leküldéses értesítést teszt küldése][34]

3. A leküldéses értesítést küld az összes eszközök regisztrált a értesítést kapni az adott értesítés-központban.

    ![iOS alkalmazás leküldéses értesítést kap tesztelése][35]


##<a name="next-steps"></a>Következő lépések

Egyszerű ebben a példában akkor a regisztrált iOS-eszközökhöz leküldéses értesítéseket rendelkezésére bocsát. A tanulási kattint, az [Azure értesítés hubok .NET-fájlok IOS értesíti a felhasználók] oktatóprogram azt ismerteti, hogy a címkék használata a leküldéses értesítések küldésére kódmentes létrehozásának folyamatát, következő lépésként elláthatja javasoljuk. 

Ha szeretne kamat csoportok a felhasználói szegmens, továbbá áthelyezhetők a [Használata értesítés hubok legfrissebb híreket küldése] oktatóanyagra mutató. 

Értesítés hubok, általános információt lásd: [Értesítés hubok útmutatást].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobil szolgáltatások iOS SDK 1.2.4 verziója]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Értesítés hubok útmutató]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure értesítés hubok értesíti a felhasználók az iOS .NET-fájlok]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Legfrissebb hírek küldése értesítés hubok használatával]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Helyi és a leküldéses értesítést programozás útmutató]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure portál]: https://portal.azure.com