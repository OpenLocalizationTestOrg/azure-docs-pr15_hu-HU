<properties
    pageTitle="Hogyan használata IOS-en SDK Azure Mobile-appokról"
    description="Hogyan használata IOS-en SDK Azure Mobile-appokról"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Hogyan használata iOS ügyfél tár Azure Mobile-appokról

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ez az útmutató útmutatást ad a legújabb [Azure Mobile-alkalmazások iOS SDK]használatával esetei végezheti el[1]. Ha még kezdő az első teljes [Azure Mobile alkalmazások Quick Start] hozzon létre egy a kódmentes Azure Mobile-alkalmazások hozzon létre egy táblázatot, és töltse le az iOS beépített Xcode projekt. Ebből az útmutatóból azt kiemelése az ügyféloldali iOS SDK csomagjában talál. A kiszolgálóoldali SDK a kódmentes kapcsolatos további információért lásd: a kiszolgáló SDK HOWTOs.

## <a name="reference-documentation"></a>Hivatkozási dokumentáció

Az iOS-ügyfél SDK dokumentációját található itt: [Azure Mobile-alkalmazások iOS ügyfél hivatkozás][2].

## <a name="supported-platforms"></a>Támogatott platformok

Az iOS SDK támogatja a cél-C projektek, Swift 2.2 projektek és Swift 2.3 projektek IOS 8.0-s vagy újabb verziók.

A "server-adatfolyam" hitelesítés a felhasználói felület bemutatott egy webnézet használja.  Ha az eszköz nem tudja bemutatására webnézet felhasználói Felületet, majd egy másik hitelesítési módszer szükség, akkor a termék hatókörén kívüli.  
Ez az SDK így nem alkalmas megtekintés típusú vagy hasonló módon korlátozott eszközöket.

##<a name="Setup"></a>A telepítő és vonatkozó követelmények

Ez az útmutató feltételezi, hogy egy kódmentes létrehozott egy táblát. Ez az útmutató feltételezi, hogy a táblázat ugyanarra a sémára, mint a tábla rendelkezik-e oktatóanyagok. Ez az útmutató is feltételezi, hogy a kódban hivatkozik `MicrosoftAzureMobile.framework` , és importálja a `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Útmutató: ügyfél létrehozása

A projekt-Azure Mobile-alkalmazások kódmentes eléréséhez hozzon létre egy `MSClient`. Csere `AppUrl` app URL-címét. Hagyhatja, `gatewayURLString` és `applicationKey` üres. Beállította a hitelesítést az átjárók, ha feltöltéséhez `gatewayURLString` az átjáró URL-címmel.

**Cél-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Útmutató: táblázat hivatkozást hoz létre.

Az access vagy a frissítés adatokat hozzon létre hivatkozást a kódmentes táblában. Csere `TodoItem` annak a táblának a nevére a

**Cél-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Útmutató: adatok lekérdezése

Egy adatbázis-lekérdezés létrehozásához a lekérdezés a `MSTable` objektumot. Az alábbi lekérdezés megkapja a naptárelemekhez `TodoItem` és a szöveg minden elem naplózza.

**Cél-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Útmutató: szűrés visszaadott adatok

A szűrést, sokféleképpen érhető el.

Egy predikátumok használatával szűréséhez használni egy `NSPredicate` és `readWithPredicate`. A következő szűrők csak hiányos teendők elemekre kereshet adatokat adja vissza.

**Cél-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Útmutató: MSQuery használata

Végezze el a komplex lekérdezést (például a rendezést és lapozási), hozzon létre egy `MSQuery` objektum, vagy közvetlenül egy predikátumok használatával:

**Cél-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`több lekérdezés viselkedése szabályozhatja teszi lehetővé.

* Az eredmények sorrend meghatározásához
* Mely mezők való visszatéréshez korlátozása
* Való visszatéréshez rekordok számának korlátozása
* Adja meg, a válasz darabszáma
* Egyéni lekérdezési karakterlánc paramétert-összehívásban
* További funkciók alkalmazása

Hajtsa végre az `MSQuery` hívja fel a lekérdezés `readWithCompletion` az objektumon.

## <a name="sorting"></a>Útmutató: MSQuery az adatok rendezése

Találatok rendezése lássunk egy példát. Mező "szöveg" növekvő, majd a teljes"csökkenő rendezéséhez meghívása `MSQuery` következőhöz hasonlóan:

**Cél-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Útmutató: mezők korlátozni, és bontsa ki a lekérdezési karakterlánc paramétereket MSQuery

Szeretné korlátozni a mezőket, hogy a lekérdezés által visszaadott, adja meg a mezők azoknak a **selectFields** tulajdonságban. Ebben a példában a csak a szöveget és a kész mezőket adja eredményül:

**Cél-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

További lekérdezési karakterlánc paraméterei szerepeltetni a kiszolgáló kérelem (például azért, mert egy egyéni kiszolgálóoldali parancsfájl használja őket), feltöltéséhez `query.parameters` következőhöz hasonlóan:

**Cél-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Útmutató: oldalméret konfigurálása

Azure Mobile alkalmazással az oldalméret szabályozza az kódmentes táblázatok egyszerre is lekért bejegyzések számát. Hívás `pull` adatok szeretné majd köteg másolatokat, ez az oldalméret alapján, amíg nem csoportosítani további rekordok is meg vannak.

Ajánlatos konfigurálása az Oldalméret **MSPullSettings** használatával az alább látható módon. Az alapértelmezett oldalméret 50, és az alábbi példában a 3-as változik.

Egy másik oldalméret teljesítmény okokból sikerült beállítani. Ha nagyszámú kis adatrekordokat, a magas az oldalméret csökkenti a kiszolgáló állapotát a körbejárásokhoz számát. 

Ez a beállítás csak az oldalméret ügyféloldali szabályozza. Ha az ügyfél kér nagyobb az oldalméret, mint a Mobile-alkalmazások kódmentes támogatja, a maximális van állítva a kódmentes támogatása határa a az oldalméret. 

A beállítás akkor is adatrekordokat, nem _bájt méretű_ _szám_ .

Ha az ügyfél oldalméretet, [az oldalméret a kiszolgálón is növelni kell](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size)növelése

**Cél-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Útmutató: adatok beillesztése

Új sor beszúrásához hozzon létre egy `NSDictionary` és meghívja `table insert`. [Dinamikus séma] engedélyezve van, ha az Azure alkalmazás szolgáltatás mobil kódmentes az új oszlopok alapján automatikusan létrehozza a `NSDictionary`.

Ha `id` nem feltéve, a kódmentes automatikusan létrehoz egy új egyedi azonosítót. Adja meg a saját `id` használata az e-mail címeket, felhasználónevét, vagy a saját egyéni értékeinek azonosítójával. Saját azonosító nyújtó illesztések és üzleti célú adatbázis logika előfordulhat, hogy érdekében.

A `result` az új elem beszúrt tartalmazza. Attól függően, hogy a kiszolgáló logika összehasonlítva milyen átadott a kiszolgálóhoz adatok további vagy módosított lehetnek.

**Cél-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Útmutató: adatok módosítása

Egy meglévő sor frissítéséhez módosítása egy elemet, és a hívás `update`:

**Cél-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Azt is megteheti adja meg a Sorazonosító és a frissített mező:

**Cél-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

A minimum a `id` attribútum is be kell állítani frissítések kezdeményezhet.

##<a name="deleting"></a>Útmutató: adatok törlése

Elem törléséhez meghívása `delete` elemhez:

**Cél-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Másik lehetőségként, mert a Sorazonosító törlése:

**Cél-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

A minimum a `id` attribútum is be kell állítani tétele törli.

##<a name="customapi"></a>Útmutató: egyéni API hívása

Egy egyéni API-val bármely kódmentes funkció elérhetővé tenni. Egy táblázat művelet hozzárendelése nincsenek. Nemcsak tegye elérhetővé válnak további beállítási lehetőségekre üzenetküldés, még akkor is lehet olvasási/fejlécek és a válasz szervezet formátumának módosítása. Megtudhatja, hogy miként hozhat létre egy egyéni API a kódmentes, olvassa el az [Egyéni API-hoz](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Hívja fel szeretne hívni egy egyéni API, `MSClient.invokeAPI`. A kérés és -válasz tartalom JSON számít. Médiafájlok másfajta használandó [használja, a Túlterhelés `invokeAPI` ] [ 5].  Hogy egy `GET` helyett kérése egy `POST` kérjen egy, a paraméter megadása `HTTPMethod` való `"GET"` és paraméterrel `body` való `nil` (óta GET kérelmek nincs üzenettörzs.) Ha az egyéni API támogatja a HTTP Továbbiak, `HTTPMethod` megfelelően.

**Cél-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Útmutató: külső.FÜGGV leküldéses sablonok platformok értesítések küldésére

Sablonok regisztrálásához fázisban sablonok a **client.push registerDeviceToken** mód az ügyfél alkalmazásban.

**Cél-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

A sablonok NSDictionary típusú és tartalmazhatnak több sablonok a következő formátumban:

**Cél-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Az összes címke biztonsági összehívásokban vannak hivatkozásból.  Címkék hozzáadása telepítések vagy a sablonok telepítések belül, [használata]című rész tartalmaz a .NET kódmentes server Azure Mobile-appokról SDK[4].  Értesítések e regisztrált sablonok használatával küldhet [Értesítés hubok API -k]használata[3].

##<a name="errors"></a>Útmutató: hibák kezelése

Az Azure alkalmazás szolgáltatás mobil kódmentes hívásakor teljesítése Tiltás tartalmaz egy `NSError` paraméter. Hiba történik, ha ezt a paramétert a nem üres. A kód jelölje be ezt a paramétert és kezelni a hibát, ha szükséges, ahogyan az előző a kódtöredék.

A fájl [`<WindowsAzureMobileServices/MSError.h>`] [6] határozza meg az állandók `MSErrorResponseKey`, `MSErrorRequestKey`, és `MSErrorServerItemKey`. A hibára vonatkozó további adatok beolvasása:

**Cél-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

A fájl ezenkívül minden egyes hibakódú állandók határozza meg:

**Cél-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Útmutató: az Active Directory Authentication Library rendelkező felhasználók hitelesítő

Active Directory Authentication tár (ADAL) segítségével a felhasználók jelentkezzen be az Azure Active Directory segítségével az alkalmazás. Ügyfél továbbításához hitelesítéssel egy identitásszolgáltató SDK célszerű a használatával a `loginWithProvider:completion:` módot.  Ügyfél-adatfolyam hitelesítés egy további natív UX működés biztosít, és lehetővé teszi, hogy további testreszabási.

1. A mobilalkalmazásban kódmentes az AAD bejelentkezés konfigurálása [az Active Directory jelentkezzen be az alkalmazás szolgáltatás konfigurálása] a következő[ 7] oktatóprogram. Ellenőrizze, hogy a natív ügyfélalkalmazás vételének nem kötelező lépés befejezéséhez. Az iOS, akkor javasoljuk, hogy az átirányítás URI van az űrlap `<app-scheme>://<bundle-id>`. További tudnivalókért lásd: az [ADAL iOS quickstart útmutató][8].

2. Telepítse az ADAL Cocoapods használatával. Ha meg szeretné jeleníteni a következő definíciót, **A PROJEKTHEZ** helyettesítve a Xcode projekt nevét a Podfile szerkesztése:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   és a Pod:

        pod 'ADALiOS'

3. Futtassa a terminált, használatával `pod install` , a címtárból a projektet tartalmazó, és nyissa meg a létrehozott Xcode munkaterület (nem a projekt).

4. Adja hozzá a következő kódot az alkalmazás használata nyelvének megfelelően. Mindegyik ellenőrizze az alábbi javaslatok:

    * **Beszúrás-szervezet – itt** cserélje le a bérlő, amelyben az alkalmazás kiépítéstől nevét. A formátum https://login.windows.net/contoso.onmicrosoft.com kell lennie. Ezt az értéket a [Azure klasszikus portálon] az Azure Active Directory tartományi lapjának másolható.
    * **Beszúrás – erőforrás-azonosító – Itt** cserélje le a mobilalkalmazásban kódmentes az ügyfél-azonosító. Az ügyfél-azonosító a **Speciális** lapon az **Azure Active Directory-beállításai** a portálon szerezhet be.
    * **Beszúrás-ügyfél-azonosító – Itt** cserélje le az ügyfél-azonosító másolta a natív ügyfele alkalmazásból.
    * **Beszúrás – ÁTIRÁNYÍTÁS-URI-itt** cserélje le a webhely _/.auth/login/done_ végpontot, a HTTPS rendszert használ. Ezt az értéket kell _https://contoso.azurewebsites.net/.auth/login/done_hasonló.

**Cél-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Útmutató: a Facebook SDK csomagjában talál az iOS rendelkező felhasználók hitelesítő

A Facebook SDK csomagjában talál az iOS felhasználók jelentkezhet be az alkalmazás használatáról a Facebook szolgáltatásból is használhatja.  Egy ügyfél továbbításához hitelesítéssel célszerű való használatáról a `loginWithProvider:completion:` módot.  Az ügyfél-adatfolyam hitelesítés tartalmaz egy további natív UX működését, és lehetővé teszi, hogy további testreszabási.

1. Állítsa be a mobilalkalmazásban kódmentes a Facebook-bejelentkezés [beállításáról a Facebook jelentkezzen be az alkalmazás szolgáltatás] a következő[ 9] oktatóprogram.

2. A Facebook SDK csomagjában talál az iOS telepíteni a [Facebook-SDK IOS – első lépések] [ 10] dokumentációt. Helyett az alkalmazás, a iOS platform is adhat a meglévő regisztrációt. 

3. Facebook-féle dokumentáció cél-C kód szerepel a alkalmazás meghatalmazott. Ha **Swift**használata esetén használhatja a következő fordítások AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Beszúrásán `FBSDKCoreKit.framework` a projekthez, egy hivatkozást is meg kell adni `FBSDKLoginKit.framework` megegyező módon. 

4. Adja hozzá a következő kódot az alkalmazás használata nyelvének megfelelően. 

**Cél-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Útmutató: az iOS Twitter háló rendelkező felhasználók hitelesítő

Az iOS háló felhasználók jelentkezhet be az alkalmazást, a Twitteren is használhatja. Ügyfél-adatfolyam-hitelesítés használatával célszerű a `loginWithProvider:completion:` módszer, ahogy egy további natív UX működés tartalmaz, illetve további testreszabási tesz lehetővé.

1. A mobilalkalmazásban kódmentes, a Twitteren bejelentkezés beállítása a [Twitteren jelentkezzen be az alkalmazás szolgáltatás konfigurálása](app-service-mobile-how-to-configure-twitter-authentication.md) oktatóprogram követve.

2. Háló hozzáadása a projekthez át [az iOS – első lépések háló] követő, és állítsa be TwitterKit.

    > [AZURE.NOTE] Alapértelmezés szerint háló létrehoz egy Twitter alkalmazást. Elkerülheti a fogyasztói billentyűt, és a fogyasztói titkos létrehozott korábbi használatáról a következő kódtöredék regisztrálja alkalmazás létrehozása.  Lecserélheti azt is megteheti, Ön által alkalmazás szolgáltatás értékekkel, megjelenik a [Háló irányítópult]fogyasztói billentyűt, és a fogyasztói titkos értékeket. Ha ezt a beállítást választja, ne felejtse el a visszahívás URL-cím beállítása egy helyőrző értékére, például: `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Ha úgy dönt, hogy a korábban létrehozott titkos kulcsok használja, az alábbi kód hozzáadása a alkalmazás meghatalmazott:
    
    **Cél-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Adja hozzá a következő kódot az alkalmazás használata nyelvének megfelelően. 

**Cél-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Útmutató: a Google bejelentkezési SDK csomagjában talál az iOS rendelkező felhasználók hitelesítő

A Google bejelentkezési SDK csomagjában talál az iOS felhasználók jelentkezhet be az alkalmazást a Google-fiókot is használhatja.  A Google nemrégiben jelenti be a saját OAuth biztonsági házirendek módosításait.  Házirend-módosítások a későbbiekben a Google SDK felhasználása van szükség.

1. Állítsa be a mobilalkalmazásban kódmentes a Google bejelentkezési az alábbi az [alkalmazást a Google bejelentkezési szolgáltatás konfigurálása](app-service-mobile-how-to-configure-google-authentication.md) oktatóprogram.

2. A Google SDK csomagjában talál az iOS telepíteni a [Google bejelentkezési az IOS - indítása integrálása](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentációt. A "Hitelesíteni a a Kódmentes Server" szakasz kihagyhatja.

3. Adja hozzá a következő a meghatalmazott `signIn:didSignInForUser:withError:` módszert használja nyelvének megfelelően.

**Cél-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Győződjön meg arról is felvehet az alábbi módon `application:didFinishLaunchingWithOptions:` az alkalmazás a meghatalmazott, cseréje "SERVER_CLIENT_ID" azonosítójú használta az alkalmazás szolgáltatás konfigurálása a lépés: 1.

**Cél-C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. A következő kódot hozzáadni az alkalmazást egy UIViewController, amely a `GIDSignInUIDelegate` protokoll használata nyelvének megfelelően.  Újra aláírás előtt kijelentkezett, és bár nem kell újra írja be hitelesítő adatait, megjelenik a felhasználó hozzájárul ahhoz párbeszédpanel megjelenítéséhez.  Csak hívja fel ezt a módszert, ha a munkamenet jogkivonat lejárt.
 
 **Cél-C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure mobil alkalmazások első lépések]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dinamikus séma]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Háló irányítópult]: https://www.fabric.io/home
[Háló IOS – első lépések]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
