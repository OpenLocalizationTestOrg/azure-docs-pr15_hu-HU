<properties
    pageTitle="Azure Active Directory B2C: A webes API hívhat fel egy harmadik fél tárak használata iOS-alkalmazás |} Microsoft Azure"
    description="Ez a cikk bemutatja, hogyan hozhat létre, amely felhívja Node.js webes API 2.0-s OAuth bearer tokenek használata egy harmadik fél tár használatával iOS "teendőlista" alkalmazást"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< ms.service= "az active directory, b2c" ms.workload="identity címkék" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "fő-cikk"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure Active Directory B2C: A webes API hívhat az iOS-alkalmazás egy harmadik fél tárban

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

A Microsoft identitás platform használja, például oauth2 hitelesítési mód és a csatlakozás OpenID megnyitott szabványoknak. Ez lehetővé teszi, hogy a fejlesztők számára, hogy a szolgáltatások integrálása kívánt tártípusból kihasználhatja. A fejlesztők támogatási a tárakat a platformot használja a demonstate az alábbihoz hasonló néhány forgatókönyvek írt azt konfigurálása a Microsoft identitás platform csatlakozhat külső tárak. A legtöbb tárakat, amely [a RFC6749 oauth2 hitelesítési mód specifikáció](https://tools.ietf.org/html/rfc6749) valósítja tud csatlakozni a Microsoft Identity platform lesz.


Ha most használja először a oauth2 hitelesítési mód vagy a OpenID csatlakozni a minta konfigurálási előfordulhat, hogy nem hozzárendelésével sok. Azt javasoljuk, tekintse meg egy rövid [Itt azt már dokumentált protokollt áttekintése](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Egyes szolgáltatások a platform, ezeket a szabványokat feltételes Access és az Intune házirendkezelő, például egy kifejezést tartalmazó csak a Megnyitás a Microsoft Azure identitás-tárak használata. 
   
Nem minden Azure Active Directory esetek és szolgáltatások támogatottak a B2C platform.  Annak megállapításához, ha a B2C platform kell használnia, [B2C korlátozások](active-directory-b2c-limitations.md)olvashat.


## <a name="get-an-azure-ad-b2c-directory"></a>Ismerkedés az Azure Active Directory B2C címtár

Azure Active Directory B2C használata előtt hozzon létre egy könyvtárat, vagy az bérlői. A könyvtár (az összes a felhasználók, alkalmazások, csoportok és az egyéb tároló). Ha nincs egy már, [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) előtt a Folytatás gombra.

## <a name="create-an-application"></a>Alkalmazás létrehozása

Ezután kell B2C címtárában-alkalmazás létrehozása. Ezzel kap, hogy szüksége van az alkalmazás biztonságos kommunikáció Azure AD-információkat. Az alkalmazás és a webes API-val is jeleníti meg egy egyetlen **Azonosítója** ebben az esetben, mert egy logikai alkalmazás tartalmazzák. Az alkalmazás létrehozásához kövesse az [alábbi utasításokat](active-directory-b2c-app-registration.md). Győződjön meg arról, hogy:

- A **mobileszköz** bele az alkalmazást.
- Másolja az alkalmazás rendelt **Azonosítója** . Is szüksége lesz a később.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>A házirendek létrehozása

Azure Active Directory B2C minden felhasználói felület [házirend](active-directory-b2c-reference-policies.md)van megadva. Az alkalmazás tartalmaz egy identitás élmény: a és a regisztrációs kombinált meg. Kell minden típusú a házirend létrehozása a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)leírt módon. A házirend létrehozásakor ügyeljen arra, hogy:

- Adja meg a **megjelenítendő név** és a regisztrációs attribútumok a házirend.
- Válassza a **megjelenítendő név** és a **Objektumazonosító** alkalmazás jogcímeken minden házirend. Megadhatja, hogy más igények is.
- Létrehozásuk után, másolja a vágólapra a minden szabály **nevét** . Az előtag rendelkeznie kell `b2c_1_`.  A házirend nevére újabb mobiltelefon szükséges.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a házirendek, készen áll az alkalmazás összeállítása.


## <a name="download-the-code"></a>Töltse le a kódot.

Ebben az oktatóanyagban kódját [a GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)karbantartása.  Követésre, [Töltse le az alkalmazást, mint egy .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip is) vagy klónozhatja azt:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Vagy csak töltse le a kész kódot, és azonnal első lépések: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Töltse le a harmadik féltől származó tár nxoauth2, és indítsa el a munkaterület

Ez a forgatókönyv fogjuk használni a OAuth2Client a GitHub, az oauth2 hitelesítési mód tárba, a Mac OS X és az iOS (kakaó és érintéses kakaó). A tár a oauth2 hitelesítési mód specifikáció 10 vázlatát alapul. Alkalmazza a natív alkalmazás profil és a támogatja a végfelhasználói engedélyezési végpontot. Az alábbiakban az összes sorrendben, a és a Microsoft identitás platform integrat lesz szükség.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>A tár hozzáadása a project CocoaPods

CocoaPods Xcode projektekhez függőség kezelője. A fenti lépéseket a telepítés automatikusan kezeli.

```
$ vi Podfile
```
Ez a podfile adja hozzá a következő:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Most töltse be a podfile cocoapods használatával. Ez hoz létre egy új XCode munkaterület betöltése lesz.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>A projekt szerkezetének

Van még a következő szerkezet beállítása a projekthez a skeleton:

* A **Minta nézet** munkaablak
* A kijelölt tevékenység adatait a **Nézet hozzáadása a tevékenységek**
* **Jelentkezzen be a nézet** , amely lehetővé teszi a felhasználónak bejelentkezés az alkalmazásba.

Azt ugrik az egyes fájlok hitelesítési hozzáadása a projekthez. A kód, például a vizuális kód más részei nem germane identitáshoz, és találhatók meg.

## <a name="create-the-settingsplist-file-for-your-application"></a>Hozzon létre a `settings.plist` az alkalmazás fájl

Célszerűbb konfigurálja az alkalmazást, ha van még egy központi lapon helyezze a konfigurációs értékeket. Azt is segít megérteni az egyes beállítások leírása az alkalmazásban. Egy mód, ha ezeket az értékeket az alkalmazásnak biztosítani, azt fogja kihasználhatja a *Tulajdonságlista* .

* Létrehozása/megnyitása a `settings.plist` a fájl `Supporting Files` az alkalmazás-munkaterületen

* Írja be az alábbi értékeket (később találhat végighalad rajtuk a részletek hamarosan)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Megkönnyítésére vegyük sorra Ugrás a következő részletesen.


A `authURL`, `loginURL`, `bhh`, `tokenURL` észre fogja venni, adja meg a bérlői nevét kell. Ez a bérlői nevét az Önhöz rendelt B2C bérlői webhelyen. Ha például `kidventusb2c.onmicrosoft.com`. Ha a Megnyitás a Microsoft Azure identitás tárak azt volna lehúzható ezeket az adatokat a metaadat-alapú végpont segítségével. A elkerülése kiolvasó ezeket az értékeket meg, de azt hozzáadott elemeket.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

A `keychain` értéke a tároló, amelyekkel a NXOAuth2Client tár egy kulcslánc tárolni a tokenek létrehozásához. Ha szeretné, hogy egyszeri bejelentkezés célba juttatni számos alkalmazások, is ugyanazt a kulcslánc adja meg az alkalmazás minden egyes, valamint a XCode entitements kérheti az adott kulcsláncában alkalmazását. Ez az Apple dokumentációjában vonatkozik.

A `<policy name>` egyes URL-cím végéről a helyek ugyanúgy hol helyezi el a fenti létrehozott házirend. Az alkalmazás visszahívja ezekről az irányelvekről attól függően, hogy a folyamat.

A `taskAPI` nevezzük a B2C jogkivonat vagy a többi végpont hozzáadása a tevékenységek vagy a meglévő tevékenységek lekérdezés. Meg van adva kifejezetten az alábbi példa a. Nem kell módosításáról a minta a munkát.

Ezeket az értékeket a többi egyszerűen a helyek meg a helyi értékek elvégzéséhez létrehozása és használata az a tár szükségesek.

Most, hogy elkészült a `settings.plist` alkalmazással létrehozott fájl szükséges elolvasni kódot.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>A beállítások olvasható AppData osztály beállítása

Most, hogy egyszerű fájl, amely csak elemzi a `settngs.plist` fájlt, hogy előbb létrehozott, és ellenőrizze ezen beállítások avaialble a jövőben minden osztály. Mivel minden alkalommal, amikor egy osztály kér, hozzon létre egy új példányát az adatok nem szeretnénk, azt egyszeres mintát használja, és csak a létrehozott minden alkalommal, amikor egy kérelem beállításainak példányt adja vissza

* Hozzon létre egy `AppData.h` fájl:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Hozzon létre egy `AppData.m` fájl:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Most, hogy könnyen elérheti, hogy adatait egyszerűen hívja fel `  AppData *data = [AppData getInstance];` az osztályok közben az alatt jelenik meg.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>A NXOAuth2Client tárba, a AppDelegate beállítása

A NXOAuthClient tár néhány érték beállításához szükséges. Egyszer vagyis teljes is használhatja a jogkivonat, amely aquired hívja fel a REST API-t. Mivel a tudjuk, hogy a `AppDelegate` bármikor azt betöltése, hogy az alkalmazás neve van ilyesmire lehetőség a fájlhoz, helyezze azt a konfigurációs értékein.
* Nyissa meg `AppDelegate.m` fájl

* Néhány később fogjuk használni az élőfej-fájlok importálása.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Adja hozzá a `setupOAuth2AccountStore` a AppDelegate módszer

Hozzon létre egy AccountStore és az adatokat, hogy csak olvasni a hírcsatorna szükség a `settings.plist` fájlt.

Néhány dolgot kell tudatában a B2C szolgáltatással kapcsolatban ezen a ponton, amely több érthető elvégzi ezt a kódot:


1. Azure Active Directory B2C használja a *házirendet* a lekérdezés paraméterei által biztosított szolgáltatás a kérést. Ezzel az Azure Active Directory használni kívánt csak az alkalmazás független szolgáltatásainak. Ezek a felesleges lekérdezés paraméterei kell adnia a biztosítása érdekében a `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` módszer az egyéni házirend paraméterrel. 

2. Azure Active Directory B2C használja sokkal hatókörök ugyanúgy más oauth2 hitelesítési mód kiszolgálókhoz. Azonban óta B2C használata annyi erőforrások elérése a felhasználó hitelesítő kapcsolatos néhány hatókörök feltétlenül szükséges ahhoz, hogy megfelelően működjön a folyamat. Ez a `openid` hatókörét. A Microsoft identitás SDK automatikusan megadja a `openid` , így nem látható, hogy a SDK konfigurációs hatókörét. Mivel a harmadik féltől származó tár használja azt, azonban azt kell adni a hatókör.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Ezután győződjön meg arról, hogy hívja meg a AppDelegate csoportban a `didFinishLaunchingWithOptions:` módot. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Hozzon létre egy `LoginViewController` osztály, amely fogjuk használni hitelesítési kérések kezelése

A fiók bejelentkezés egy webnézet fogjuk kiszámolni. Lehetővé teszi a felhasználótól további tényezők hasonlóan, SMS-ben (Ha be van állítva), vagy adjon vissza hibaüzenetek jelennek meg az felhasználónak us. Itt kell beállítani a webnézet be, és később írni a kódot, és kezelheti a visszahívás, amely a Microsoft identitás szolgáltatásból webnézetben történik.

* Hozzon létre egy `LoginViewController.h` osztály

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Hozzunk létre minden, az alábbi módszerek egyikét.

> [AZURE.NOTE] 
    Győződjön meg arról, hogy köt a `loginView` szeretne a tényleges webnézet, amely a történet belül. Egyéb esetben nem van egy webnézet, amely is felugró időpontjában hitelesítést végezni.

* Hozzon létre egy `LoginViewController.m` osztály

* Állapot elvégzéséhez, ahogy azt hitelesítést végezni néhány változók hozzáadása

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* A webnézet módszerek hitelesítés kezelésének felülbírálása

Szükség van a webnézet szeretnénk, ha egy felhasználónak kell bejelentkezni, a fent ismertetett módon viselkedését. Egyszerűen kivágása, és illessze be az alábbi kódot.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Írja be a kódot az eredmény az oauth2 hitelesítési mód kérelem kezelése

Azt a redirectURL vissza érkező a webnézet kezelő kódot kell rendelkeznie. Ha ez nem lett sikeres, hogy újra próbálkozik. Eközben a tárat a hiba, hogy a konzolban látható, vagy asyncronously kezeléséhez nyújt. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Állítsa be a értesítés gyárak.

Hozzunk létre, akkor visszavonhatja ugyanazzal a módszerrel a `AppDelegate` fent, de ez esetben néhány hozzáadunk `NSNotification`s közli velünk a Mi történik a a szolgáltatás. Fog mondja el nekünk mikor bármilyen változás az a token megfigyelő beállítása azt. Ha azt sikerült, a token azt lépjen vissza a felhasználó vissza az `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* A forrás szerkesztésével, amely a felhasználó kezeli, valahányszor a bejelentkezési natív kezdeményezése egy kérelmet

Hozzunk létre, amely hívható, valahányszor van még egy kérelemre hitelesítési módszer. Ez a módszer, amelyet a ténylegesen létrehoz egy webnézet lesz

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Végezetül vegyük hívás azt minden alkalommal feletti írt minden módszer a `LoginViewController` betölti. Hajtanak végre ezeket a módszereket, amelyekkel hozzáadásával a `viewDidLoad` kapcsolatfelvételi mód Apple adja vissza

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Most már végzett fő módja azt fogja másokkal való bejelentkezéshez az alkalmazás létrehozásával. Követően, hogy be van bejelentkezve, akkor kell használni a tokenek érkeztek. Az adott segítő kód, amely a tár használatával a REST API-khoz visszahívja létrehoznia azt.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Hozzon létre egy `GraphAPICaller` osztály kezelheti saját kérések REST API-hoz

A konfiguráció minden alkalommal, amikor azt töltse be a saját alkalmazás betöltött van. Most már azt kell elvégeznie az általa után egy token van. 

* Hozzon létre egy `GraphAPICaller.h` fájl

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Megjelenik a kód, hogy azt fog szervezni két módszer szerint lehetséges: egy-a tevékenységek kinyerése egy API-val, és a másik vehet fel tevékenységeket az API-t.

Most, hogy azt a beállított saját felület, helyezzen el a tényleges végrehajtása:

* Hozzon létre egy`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>A minta alkalmazás futtatása

Végezetül össze, és futtassa az alkalmazást a Xcode. Regisztráció vagy bejelentkezés az alkalmazásba, és a bejelentkezett felhasználó létrehozása. Jelentkezzen ki, és egy másik felhasználó, jelentkezzen be az adott felhasználó-feladatok létrehozása.

Figyelje meg, hogy a tevékenységek is tárolt felhasználónkénti az API, mivel az API-t a felhasználói azonosító olvas a hozzáférési jogkivonat kap.


## <a name="next-steps"></a>Következő lépések

Most már áthelyezheti speciális B2C témakörök alakzatot. Próbáljon meg:

[Hívja fel a Node.js webes API egy Node.js web app alkalmazásból]()

[Testre szabhatja a UX B2C-alkalmazásokban]()
