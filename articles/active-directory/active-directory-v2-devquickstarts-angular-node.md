<properties
    pageTitle="Első lépések Azure AD-2.0-s verzió AngularJS |} Microsoft Azure"
    description="Hogyan hozhat létre, amely a két személyes Microsoft-fiókkal rendelkező felhasználók bejelentkezik szög JS egyoldalas alkalmazás és a munkahelyi vagy iskolai fiókokat."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a>Bejelentkezési AngularJS egyoldalas alkalmazás - NodeJS hozzáadása

Ebben a cikkben azt be kell jelentkeznie hajtott Microsoft-fiók használata az Azure Active Directory 2.0-s verzió végpontot AngularJS alkalmazás jegyezze fel. a 2.0-s verzió végpont lehetővé teszi hajtsa végre a egyetlen integráció az alkalmazást, és a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók hitelesítési.

Ez a példa egy egyszerű feladatlista egyoldalas alkalmazás, amely egy kódmentes REST API-t, NodeJS nyelven íródott és OAuth bearer tokenek az Azure Active Directory használata biztonságos feladatokat tárolja.  A AngularJS alkalmazásban a Megnyitás JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) segítségével a teljes bejelentkezési folyamat kezelni, és hívja fel a REST API-tokenek szerezheti be.  Az azonos mintát más REST API-khoz, például a [Microsoft Graph](https://graph.microsoft.com) - vagy az Azure erőforrás-kezelő API-k hitelesíteni alkalmazhatók.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="download"></a>Letöltés

Első lépésként meg kell letöltése és telepítése [node.js](https://nodejs.org).  Átmásolhatja, majd [Töltse le](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) egy skeleton alkalmazás vagy:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

A skeleton alkalmazás tartalmaz egy egyszerű AngularJS alkalmazás újrafelhasználható programkódot, de az összes az identitás kapcsolatos darab hiányzik.  Ha nem szeretné követheti, akkor ehelyett klónozhatja vagy [Töltse le](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) a bejegyzett minta.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Regisztráljon az alkalmazás

Első lépésként az [Alkalmazás regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)-alkalmazás létrehozása, vagy kövesse az alábbi [lépések részletes leírását](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- A **webes** platform az alkalmazás hozzáadása
- Írja be a megfelelő **URI irányítja**. Ez a példa az alapértelmezett értéke `http://localhost:8080`.
- Kilépés a engedélyezett **Implicit Flow engedélyezése** jelölőnégyzetet. 

Másolás lefelé az **Azonosítója** , hogy az alkalmazás hozzá van rendelve, szüksége van rá hamarosan. 

## <a name="install-adaljs"></a>Adal.js telepítése
Szeretné kezdeni, nyissa meg a projekt letöltött, és telepítse a adal.js.  Ha [bower](http://bower.io/) telepítve van, ez a parancs csak futtathatja.  Bármilyen típusú függés verzió eltérések egyszerűen válassza a magasabb verziószámú.
```
bower install adal-angular#experimental
```

Másik lehetőségként manuálisan letöltheti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) és az [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Mindkét fájlok hozzáadása a `app/lib/adal-angular-experimental/dist` címtár.

Most már a kedvenc szövegszerkesztő programban, nyissa meg a projektet, és töltse be a Laptörzs végén adal.js:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Állítsa be a REST API-val

Azt is beállítások megadását, miközben lehetővé teszi, kódmentes REST API-működéséhez.  A parancssorba telepítse a szükséges csomagokat futtatásával (Győződjön meg arról, hogy a projekt a legfelső szintű címtárban):

```
npm install
```

Most már nyissa meg a `config.js` , és cserélje le a `audience` érték:

```js
exports.creds = {
     
     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',
     
     ...
}
```

A REST API AJAX-kérelmek szög alkalmazásból kap tokenek érvényesítése ezt az értéket kell használni.  Megjegyzés: Ez az egyszerű REST API tárolja az adatokat a memóriában – tehát minden alkalommal, amikor leállítja a kiszolgálót, akkor elvesznek a korábban létrehozott feladatok.

Ez a mindig megyünk, hogy mennyit beszél meg, hogyan működik a REST API-t.  Nyugodtan a kód poke, de ha azt szeretné, ha többet szeretne tudni a biztonságossá tétele a webes API-khoz az Azure Active Directory, olvassa el [Ez a cikk](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>A bejelentkezési felhasználók
Néhány azonosító kódírás időt.  Akkor lehet, hogy már láthatta, hogy adal.js AngularJS szolgáltató megfelelően legyenek rendezve lejátszását szög útválasztási mechanizmusok tartalmazza.  Indítsa el az alkalmazást az adal modul hozzáadásával:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Most már meg is inicializálni a `adalProvider` az alkalmazás azonosítója:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Nagy adal.js összegyűjtötte szüksége van az alkalmazás biztonságos, és jelentkezzen be a felhasználók az összes információt.  Hogy az alkalmazás egy adott útvonalon bejelentkezés, az összes szükséges időt kód egy sor:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Most már amikor a felhasználó kattint a `TodoList` hivatkozás, adal.js automatikusan átirányítja az Azure Active Directory, a bejelentkezési szükség esetén.  Bejelentkezési és kijelentkezési kéréseket küldéséhez, a vezérlők adal.js meghívása explicit módon is:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Felhasználói adatok megjelenítése
Most, hogy a felhasználónak van bejelentkezve, valószínűleg kell elérni az alkalmazás az aláírt a felhasználói hitelesítés adatait.  Adal.js ezt az információt nyissa meg a elérhetővé teszi a `userInfo` objektumot.  Az objektum egy nézet eléréséhez először hozzáadása adal.js a legfelső szintű hatókör a megfelelő vezérlő:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Meg közvetlenül cím, majd a `userInfo` objektum a nézetben: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Is használhatja a `userInfo` objektum határozza meg, ha a felhasználó jelentkezett be, vagy sem.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Hívja fel a REST API-val
Végül pedig első néhány tokenek, és hívja fel a REST API-t létrehozás, Olvasás, frissítés, és törölheti a tevékenységeket.  Jól találat mi?  Nem kell *egy*funkciójú.  Adal.js automatikusan az első, a gyorsítótár és a tokenek frissítése gondoskodik meg.  Fog is gondoskodik a e tokenek csatolása kimenő AJAX-kérelmek küld a REST API-hoz.  

Pontosan hogyan működik? Az összes a mágikus [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), amely lehetővé teszi a kimenő és bejövő HTTP-üzenetek átalakításához adal.js a több van.  Ezenkívül adal.js feltételezi, hogy minden olyan kérelmek a Küldés ugyanahhoz a tartományhoz az ablak tokenek szánt alkalmazás azonosítója megegyezik a AngularJS alkalmazást kell használnia.  Ez az az oka annak, hogy az azonos azonosítója használt mindkét szög letölthető és a NodeJS REST API.  Természetesen ez a működési módjának felülbírálására és a tokenek beszerzése más REST API-k, ha szükséges - adal.js megállapítani, de ez az egyszerű forgatókönyv az alapértelmezett hajt végre.

Íme egy kódtöredékének, amely mutatja, hogy milyen könnyen célszerű bearer tokenek kérések küldeni az Azure Active Directory:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Gratulálok!  Az Azure Active Directory-integrált egyoldalas alkalmazás befejeződött.  Lépjen tovább, készíthet egy masniba.  Azt is hitelesíti a felhasználókat, biztonságosan hívja fel a kódmentes OpenID összekapcsolás funkcióval REST API-t, és a felhasználó alapvető adatainak.  Beépített egy személyes Microsoft-Account vagy Azure AD a munkahelyi vagy iskolai fiókkal rendelkező felhasználók támogat.  Próbálja a app futtatásával:

```
node server.js
```

A böngészőben nyissa meg `http://localhost:8080`.  Jelentkezzen be egy személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiók segítségével.  Tevékenységek hozzáadása a felhasználó feladatlista, és jelentkezzen ki.  Próbáljon meg bejelentkezni az egyéb típusú fiókot. Ha szüksége van egy Azure AD-bérlői hozhat létre a munkahelyi vagy iskolai felhasználó, [megtudhatja, hogyan hozhatja ki egyet az alábbi](active-directory-howto-tenant.md) (Ez az ingyenes).

Továbbra is kapcsolatban a a 2.0-s verzió végpont vissza címjegyzékemben [2.0-s verzió Fejlesztőeszközök útmutató](active-directory-appmodel-v2-overview.md).  További források olvassa el:

- [Azure-minták a GitHub >>](https://github.com/Azure-Samples)
- [Egymást fedő túlfolyás Azure Active Directory >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure Active Directory dokumentációja [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések beszerzése termékei

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók az [erre a lapra](https://technet.microsoft.com/security/dd252948) , és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
