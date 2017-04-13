<properties
    pageTitle="Azure Active Directory 2.0-s verzió iOS alkalmazás |} Microsoft Azure"
    description="Az iOS-alkalmazás, amely a felhasználók mindkét személyes Microsoft-fiókjával bejelentkezik létrehozásának és munkahelyi vagy iskolai fiók külső tárak használatával."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Külső tár használata Graph API segítségével a 2.0-s verzió végpont iOS-alkalmazás bejelentkezési hozzáadása

A Microsoft identitás platform használja, például oauth2 hitelesítési mód és a csatlakozás OpenID megnyitott szabványoknak. A fejlesztők használhatják a szolgáltatások integrálása szeretnének tártípusból. Hogy a fejlesztők az tárakat a platformot használja, néhány forgatókönyvek keresztül mutatjuk konfigurálása a Microsoft identitás platform csatlakozhat külső tárak alábbihoz hasonló írt azt. A legtöbb tárakat, amely [a RFC6749 oauth2 hitelesítési mód specifikáció](https://tools.ietf.org/html/rfc6749) végrehajtja a Microsoft identitás platform csatlakozhat.

Ez a forgatókönyv hoz létre az alkalmazással felhasználók jelentkezzen be szervezete, és keresse meg a mások számára a szervezet a Graph API segítségével.

Ha most használja először a oauth2 hitelesítési mód vagy a OpenID csatlakozni, a minta konfigurálási előfordulhat, hogy nem hozzárendelésével. Javasoljuk, hogy [2.0-s verzió protokollok - OAuth 2.0-s engedélyezési kód Flow](active-directory-v2-protocols-oauth-code.md) háttér olvassa el.


> [AZURE.NOTE]
    Egyes szolgáltatások, hogy van-e egy kifejezést az oauth2 hitelesítési mód vagy OpenID csatlakozás előírásoknak, feltételes Access és az Intune házirendkezelő, például a platform megkövetelése, hogy a Megnyitás a Microsoft Azure identitás-tárak használata.

A 2.0-s verzió végpont esetek Azure Active Directory és a szolgáltatások nem támogatja.

> [AZURE.NOTE]
    Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="download-code-from-github"></a>Töltse le a kód GitHub
Ebben az oktatóanyagban kódját [a GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2)karbantartása.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) is vagy a skeleton klónozhatja:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

A minta is egyszerűen töltheti le, és kezdheti el azonnal:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Regisztráljon az alkalmazás
Új alkalmazás létrehozása, az [alkalmazás regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy a részletes lépéseket, [hogy miként regisztrálhatja egy alkalmazást a 2.0-s verzió végponttal](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- Másolja az alkalmazás van rendelve, mert hamarosan kell **Azonosítója** .
- A **mobil** platform az alkalmazás hozzáadása
- Az **Átirányítás URI** másolja a portálon. Az alapértelmezett értéket kell használnia `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Töltse le a külső NXOAuth2 tárat, és a munkaterület létrehozása

Ez a forgatókönyv a OAuth2Client GitHub, amely az oauth2 hitelesítési mód tárba, a Mac OS X és az iOS (kakaó és érintéses kakaó) a fogja használni. A tár a oauth2 hitelesítési mód specifikáció 10 vázlatát alapul. Alkalmazza a natív alkalmazás profil és a támogatja az engedélyezés végpontot, annak a felhasználónak. Az alábbiakban az összes integrálása a Microsoft identitás platform kell.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>A tár hozzáadása a projekthez CocoaPods

CocoaPods Xcode projektekhez függőség kezelője. Az előző telepítési lépéseket automatikusan kezeli.

```
$ vi Podfile
```
1. Ez a podfile adja hozzá a következő:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Töltse be a podfile CocoaPods használatával. Ez a hoz létre új Xcode munkaterület betöltése lesz.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>A projekt szerkezetének feltárása

Az alábbi szerkezet beállítása a projekthez a skeleton:

- Egy egyszerű Felhasználónévi keresést Diaminta nézet
- A kijelölt felhasználó kapcsolatos adatok adatait megjelenítő nézet
- Ha egy felhasználó lehet bejelentkezni az alkalmazásba az graph lekérdezése a bejelentkezési nézet

Azt helyezi át a skeleton különböző fájlok hozzáadása a hitelesítési. A kódot, például a vizuális kódot, más részei nem vonatkoznak identitás, de találhatók meg.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Állítsa be a settings.plst fájlt a tárban

-   Nyissa meg a quickstart útmutató projekt a `settings.plist` fájlt. Az elemek megfelelően az értékeket, akkor az Azure-portálon használt szakaszában az értékek lecserélése. A kód fog hivatkozni ezeket az értékeket, amikor csak akkor használja az Active Directory Authentication Library.
    -   A `clientId` az alkalmazás a portálról másolt ügyfél azonosítója.
    -   A `redirectUri` , a portálon megadott átirányítási URL-címe.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>A NXOAuth2Client tárba, a LoginViewController beállítása

A NXOAuth2Client tár bizonyos értékek beállításához szükséges. Miután megadta ezt a feladatot, hívja fel a Graph API a megvásárolt jogkivonat is használhatja. Mivel `LoginView` lesz hívta a hitelesítés szükséges bármikor, célszerű elhelyezni a konfigurációs értékek, amely a fájl.

- Az egyes értékek hozzáadása a `LoginViewController.m` fájl segítségével az hitelesítési és engedélyezési környezetben. Az értékek részleteket kövesse a kódot.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Nézzük meg, a kód olvashat.

Az első karakterlánc szolgál `scopes`.  A `User.Read` érték lehetővé teszi, hogy olvassa be az alap profilt bejelentkezett felhasználó.

További tudnivalók a [Microsoft Graph jogosultsági hatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes)elérhető hatóköreit talál.

A `authURL`, `loginURL`, `bhh`, és `tokenURL`, a korábban megadott értékeket kell használnia. A Megnyitás a Microsoft Azure identitás-tárak használata esetén azt lehúzható ezeket az adatokat meg a metaadat-alapú végpont használatával. A elkerülése kiolvasó ezeket az értékeket meg, de azt hozzáadott elemeket.

A `keychain` értéke a tároló, amelyekkel a NXOAuth2Client tár egy kulcslánc tárolni a tokenek létrehozásához. Ha szeretné, hogy eléréséhez használt alkalmazások számos, az egyszeri bejelentkezés (SSO), ugyanazt a kulcslánc adja meg az alkalmazás minden egyes, és kérheti az adott kulcsláncában használata a Xcode jogosultságok. Ez az Apple dokumentációjában magyarázza.

Ezeket az értékeket a többi szükség, és meg a helyi értékek elvégzéséhez helyek létrehozása a tárban.

### <a name="create-a-url-cache"></a>Hozzon létre egy URL-gyorsítótár

Belül `(void)viewDidLoad()`, amelyek mindig nevezzük a nézet betöltése után, az alábbi kódot primes gyorsítótárat a használatra.

Adja hozzá a következő kódot:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Hozzon létre egy bejelentkezési webnézet

Egy webnézet a felhasználótól további tényezők hasonlóan, SMS-ben (Ha be van állítva) vagy a visszatérési hibaüzenetek jelennek meg a felhasználó számára. Az alábbi beállításokat a webnézet be, és később írni a kódot, és kezelheti a visszahívás, az Identitáskezelés szolgáltatásokból webnézetben történik.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>A webnézet módszerek hitelesítés kezelésének felülbírálása

Állapítható meg, hogy a webnézet mi történik, ha egy felhasználónak kell jelentkezzen be a korábban tárgyalt, beillesztheti a következő kódot.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Írja be a kódot az eredmény az oauth2 hitelesítési mód kérelem kezelése

A következő kódot, amely a webnézet vissza az redirectURL fogja kezelni. Ha a hitelesítési nem lett sikeres, a kód újra próbálkozik. Eközben a tárat a hibát, amely a konzolban látható, vagy aszinkron kezeléséhez nyújt.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Állítsa be az OAuth környezetben (más néven fiók tár)

Itt felhívhatja `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` az egyes szolgáltatásokhoz, az alkalmazás tudja elérni kívánt megosztott fiók áruházból. A fióktípus egy olyan karakterlánc, egy bizonyos szolgáltatás azonosítóként használt. A diagram API férnek hozzá, mert a kód hivatkozik azonosítóként `"myGraphService"`. Ezután beállítása, hogy a program tájékoztatja, ha bármilyen változás a token a megfigyelő. Miután kap a jogkivonat, visszatérési a felhasználó biztonsági másolatot szeretne a `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Állítsa be a megkereséséhez, és a felhasználókat a Graph API megjelenítése a minta nézet

Alkalmazás a mesteralakzat-vezérlő (MVC), a visszaadott adatok megjelenítése a rács az útmutató túlra, és sok online oktatóanyagok bemutatják, hogyan hozhat létre egyet. Ez a kód skeleton fájl található. Azonban kell kezelni az néhány dolog, amit az MVC alkalmazásban:

* Ha egy felhasználó a Keresés mezőbe beírja metsz
* Biztosítása az adatok objektum vissza az MasterView úgy is megjeleníti az eredményt a rács

Azt fogja tegye azokat az alábbi.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Ha be van jelentkezve egy ellenőrzése hozzáadása

Az alkalmazás hajtja végre apró része, ha a felhasználó nem jelentkezett be, hogy az intelligens ellenőrzéséhez, ha már van egy token gyorsítótár. Ha nem, átirányítja a LoginView való bejelentkezéshez a felhasználó számára. A visszahívás, ha a műveletekhez nézet betöltődésekor legegyszerűbben használni, a `viewDidLoad()` biztosító Apple kapcsolatfelvételi módot.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>A táblázat nézet frissítése, adatok érkezésekor

A diagram API adatokat ad vissza, ha szeretné megjeleníteni az adatokat. Az egyszerűség kedvéért az alábbiakban a táblázat frissítése a kódot. Csak a MVC újrafelhasználható kódban illessze be a megfelelő értékeket.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Hívja fel a Graph API, amikor valaki a Keresés mezőbe beírja kínál

Ha egy felhasználó a Keresés a mezőben, kell, hogy a diagram API shove. A `GraphAPICaller` osztály, amely a következő kódrészlet rendszer generál, összekapcsolt szót a keresés funkcióval a bemutatóból. Fontos nézzük írja be a kódot, amely a keresési karakterek a Graph API-hírcsatornák. Hajtanak végre, mert a módszer nevű `lookupInGraph`, amely megnyitja a karakterlánc, amely azt meg szeretné keresni.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Írja be a diagram API eléréséhez segítő osztály

Ez az alkalmazás központi. Mivel a többi az Apple-től alapértelmezett MVC szerkezetében történt vonalkód, itt, kódírás az graph lekérdezése a beírás, és ezután térjen az adatokat. Az alábbiakban a kódot, és részletes magyarázatot áthelyeződik vagy törlődik.

### <a name="create-a-new-objective-c-header-file"></a>Hozzon létre egy új cél C élőfej fájlt

A fájl nevét `GraphAPICaller.h`, és adja hozzá a következő kódot.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Itt látni, hogy megnyitja a karakterlánc megadott módszer, és egy completionBlock adja eredményül. Ez a completionBlock, előfordulhat, hogy rendelkezik kitalálhatják, frissül a táblázatot, mert az objektum adatokkal feltöltött a valós idejű, a felhasználó keres.


### <a name="create-a-new-objective-c-file"></a>Hozzon létre egy új cél C-fájlt

A fájl nevét `GraphAPICaller.m`, és adja hozzá a következő módszerrel.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Ez a módszer részletesen folyamatát megkönnyítésére vegyük sorra.

Az alap Ez a kód szerepel a `NXOAuth2Request`, módszer, amely megnyitja a paramétereket, amely korábban már megadott a settings.plist fájlban.

Első lépésként összeállításához jobb oldali diagram API-hívást. Mivel a hívás `/users`, adja meg, hogy a diagram API erőforrás együtt a verzió hozzáfűzheti. Célszerű elhelyezni egy külső beállításfájl az mivel ezek módosíthatja, az API követi.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Következő lépésként meg kell is biztosít a Graph API hívásba paramétert. Azt is *nagyon fontos* , hogy nem helyezi el a paraméterek az erőforrás-végpontot, mert, amely az összes nem URI megfelelő karakterek helyére kerülő futásidőben törölte. Az összes lekérdezés kód meg kell adni a paraméterek.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Megfigyelheti, hogy a hívások a `convertParamsToDictionary` módszer, amely még nem írt. Vegyük tegye most végén található a fájlt:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Ezután használata a `NXOAuth2Request` módszert adatok beolvasása a vissza az API JSON formátumban.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Végezetül nézzük meg, hogy állítsa vissza az adatokat a MasterViewController. Az adatok szerializáltként adja eredményül, és igénybe vehet, a MainViewController objektum betöltött és deszerializálni kell. Erre a célra a skeleton van egy `User.m/h` fájlt, létrehozza a User objektumban. Elhelyezte a User objektumban, a diagram adataival.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>A minta futtatása

Ha a skeleton használt vagy követett az útmutató – az alkalmazás most már működik együtt. Indítsa el a simulatort használja, és kattintson a **Jelentkezzen be** az alkalmazás használatához.

## <a name="get-security-updates-for-our-product"></a>A termék biztonsági frissítések beszerzése

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók a [Biztonsági, TechCenter](https://technet.microsoft.com/security/dd252948) és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
