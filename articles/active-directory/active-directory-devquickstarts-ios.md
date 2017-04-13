<properties
    pageTitle="Első lépések az Azure Active Directory iOS |} Microsoft Azure"
    description="Hogyan hozhat létre egy iOS-alkalmazás, amely integrálódik való bejelentkezéshez Azure Active Directory és hívások Azure Active Directory API-k használata OAuth védett."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrálhatja a Azure AD-iOS-alkalmazás

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure Active Directory iOS ügyfélprogramokat sorolja fel kell védett forrásokat az Active Directory Authentication Library vagy ADAL, biztosít.  ADAL-féle kizárólagos életben célja, hogy az alkalmazás, hogy a hozzáférési jogkivonat könnyen.  Annak bemutatják, hogy milyen egyszerűen egyszerű, itt azt fogja hozza létre a cél C teendőlista alkalmazás, amely:

-   Keresése access-tokenek a hívásához az Azure Active Directory Graph API [OAuth 2.0-s hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)használatával.
-   A megadott alias rendelkező felhasználóknak könyvtárában keres.

A teljes munkaidő-alkalmazás létrehozására kell:

2. Az alkalmazás regisztrálása Azure AD.
3. Telepítse & konfigurálása ADAL.
5. ADAL segítségével tokenek lekérése az Azure Active Directory.

Az első lépések, [Töltse le az alkalmazás skeleton](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) vagy [a kész minta letöltése](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Az Azure Active Directory bérlői, ahol felhasználók létrehozása és az alkalmazások regisztráljon is szüksége lesz.  Ha még nem rendelkezik a bérlői, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

> [AZURE.TIP] Az Előnézet az új [developer Portal segítségével](https://identity.microsoft.com/Docs/iOS) , amely segít a kezdeti lépéseket az Azure Active Directory címtárral mindössze néhány perc múlva próbálkozzon!  A developer Portal segítségével végigvezeti a folyamaton, az alkalmazás rögzítése és Azure Active Directory integrálása a kódot.  Ha végzett, lehetősége van egy egyszerű alkalmazás, amely hitelesíthet a bérlő és egy kódmentes, amelyek képesek fogadja el a tokenek, és hajtsa végre az érvényesítési felhasználókat. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. meghatározható, milyen lesz az átirányítás URI-az iOS*

Annak érdekében, hogy biztonságosan indítsa el az alkalmazások az egyes egyszeri bejelentkezési forgatókönyvek szükséges **Átirányítás URI** létrehozhat egy bizonyos formátumban. Győződjön meg arról, hogy a tokenek visszatérhet adnia őket a megfelelő alkalmazás átirányítás URI szolgál.

Az iOS átirányítása URI esetén:

```
<app-scheme>://<bundle-id>
```

-   **aap sémához** – ez van regisztrálva a XCode projektben. Fontos, hogy milyen más alkalmazásokban is hívja Önt. Ez a Info.plist -> található URL-cím típusú -> URL-cím azonosítója. Ha még nem rendelkezik egy vagy több konfigurálni kell létrehozni egy.
-   **az első lépésekhez-azonosító** – Ez az első lépésekhez azonosító "azonosító" alatt található un XCode a projekt beállításait.

Példa a kód quickstart Útmutató: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. a DirectorySearcher alkalmazás regisztrálása*
Ahhoz, hogy az alkalmazás tokenek megszerezni, először kell regisztrálni a Azure AD-bérlő, és adja meg azt az Azure Active Directory Graph API eléréséhez szükséges engedély:

-   Jelentkezzen be a Azure Kezelőportálja segítségével
-   A bal oldali navigációs sávon kattintson **Az Active** Directory
-   Jelölje ki a bérlői webhelyet, ahol az alkalmazás regisztrálni.
-   Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra az alsó fiókban.
-   Az útmutatást követve hozzon létre egy új **Natív ügyfélalkalmazás**.
    -   Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   Az **Átirányítás Uri** , amelyekkel a Azure AD vissza jogkivonat válaszok az Office-téma és karakterlánc együttes.  Az alkalmazás, a fenti információk alapján adja meg az egy adott értéket.
-   Regisztráció végrehajtása után, AAD rendel az alkalmazás az ügyfél egyedi azonosítót.  Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a **beállítás** lapon.
- Is **konfigurálása** lapon keresse meg a "Engedélyeket szeretne egyéb alkalmazások" szakaszt.  A "Azure Active Directory" alkalmazás hozzáadása a **hozzáférést a szervezet címtárára** engedélyt a **Meghatalmazott engedélyeit**.  Ezzel engedélyezi az alkalmazás a Graph API-felhasználóknak lekérdezéséhez.

## <a name="3-install--configure-adal"></a>*3. a telepítés & konfigurálása ADAL*
Most, hogy az alkalmazások az Azure Active Directory, ADAL telepítése, és írja be a kapcsolódó azonosító kódot.  Az Azure Active Directory kommunikálni tudjanak ADAL sorrendben kell, küldje el az alkalmazás regisztrációs információkat.
-   Kezdje el a DirectorySearcher projekt Cocapods használatával ADAL hozzáadásával.

```
$ vi Podfile
```
Ez a podfile adja hozzá a következő:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Most töltse be a podfile cocoapods használatával. Ez hoz létre egy új XCode munkaterület betöltése lesz.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Nyissa meg a plist fájlt a quickstart útmutató projekt `settings.plist`.  Az elemek, az Azure-portálra a bemeneti értékek megfelelően szakaszában az értékek lecserélése.  A kód minden alkalommal ADAL hivatkoznak ezeket az értékeket.
    -   A `tenant` a Azure AD-bérlő, például contoso.onmicrosoft.com tartománya
    -   A `clientId` van az alkalmazás a portálról másolta a clientId.
    -   A `redirectUri` van az átirányítás regisztrálta a portál URL-címét.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. segítségével ADAL AAD tokenek beolvasása*
A alapelve ADAL mögött, hogy amikor az alkalmazás szüksége van egy hozzáférési jogkivonat, egyszerűen meghívja egy completionBlock `+(void) getToken : `, és az ADAL jelent, a többi.  

-   Az a `QuickStart` megnyitott projekt `GraphAPICaller.m` , és keresse meg a `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` Megjegyzés felső részén.  Ez a hol adják át az ADAL a koordináták a CompletionBlock kommunikálni az Azure Active Directory és a gyorsítótár-tokenek erről keresztül.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Most már van szüksége a token segítségével kereshet felhasználókat a diagramon. Keresse meg a `// TODO: implement SearchUsersList` fűzzön módszer az Azure Active Directory Graph API Query GET kérésekben teszi a felhasználóknak, akiknek (UPN) kezdődik a megadott keresési kifejezés.  Annak érdekében, hogy a diagram API lekérdezéséhez is használni kell a egy access_token, de a `Authorization` élőfej a kérelem – Ez az ADAL honnan.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Amikor az alkalmazás kér hívja fel a jogkivonat `getToken(...)`, ADAL megpróbálja jogkivonat vissza anélkül, hogy a felhasználó hitelesítő adatait kérő.  ADAL határozza meg, hogy a felhasználónak kell jelentkezzen be az első jogkivonat, ha azt fog egy bejelentkezési párbeszédpanel megjelenítése, a felhasználó hitelesítő adatok összegyűjtése és jogkivonat sikeres hitelesítés esetén vissza.  Ha ADAL kérdezhetők le egy token valamilyen okból, akkor throw egy `AdalException`.
- Figyelje meg, hogy a `AuthenticationResult` objektum tartalmaz egy `tokenCacheStoreItem` használható az alkalmazás szükséges információk összegyűjtése az objektumot.  A quickstart útmutató a `tokenCacheStoreItem` azt határozza meg, ha authenitcation már fordult elő.


## <a name="step-5-build-and-run-the-application"></a>5 lépés: Össze, és futtassa az alkalmazást



Gratulálok! Most egy munka iOS alkalmazást, amely tartalmazza az azt jelenti, hogy a felhasználók hitelesítő, biztonságos hívja a webes API-khoz OAuth 2.0-s verzióját használja, és a felhasználó alapvető adatainak.  Ha még nem tette meg, ezt követően az idő az egyes felhasználók a bérlő kitöltéséhez.  Futtassa a quickstart útmutató alkalmazást, és jelentkezzen be egy azoknak a felhasználóknak.  Más felhasználók UPN név alapján kereshet.  Zárja be az alkalmazást, és futtassa újra.  Figyelje meg, hogy a felhasználó munkamenet sértetlen marad.

ADAL megkönnyíti az alábbi közös identitás funkciók részévé tenni az alkalmazást.  Azt gondoskodik a dirty munkáját vonatkozó - gyorsítótár-kezelés, a OAuth protokoll támogatása, a felhasználó bemutató tartása a bejelentkezési felhasználói felülete frissítése lejárt tokenek, és így tovább.  Valójában kell tudnia, mindössze egyetlen API-hívás `getToken`.

A kész minta (nélkül a konfigurációs értékek) megadva hivatkozás, az [alábbi](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>További felhasználási területei
Most már áthelyezheti további forgatókönyvek.  Előfordulhat, hogy szeretne kipróbálni:

- [Biztonságos Node.JS webes API Azure AD a](active-directory-devquickstarts-webapi-nodejs.md)
- Megtudhatja, [hogy miként határokon-alkalmazás SSO iOS ADAL használata a engedélyezése](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
