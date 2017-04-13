<properties
    pageTitle="Első lépések Azure Active Directory AngularJS |} Microsoft Azure"
    description="Egy szög JS egyoldalas alkalmazás, amely integrálódik az Azure Active Directory való bejelentkezéshez és Azure Active Directory felhívja létrehozásának API-k használata OAuth védett."
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


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>AngularJS egyetlen lap alkalmazások az Azure Active Directory biztonságossá tétele

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure Active Directory teszi egyszerű és egyszerű aláírás hozzáadása a Kijelentkezés parancsra, és a biztonságos az egy oldalra alkalmazásait OAuth API-hívások.  Az Active Directory-fiókokkal rendelkező felhasználók hitelesítő és bármely webes API védi az Azure Active Directory, például az Office 365 API-k és az Azure API felhasználása az alkalmazás lehetővé teszi.

Javascript-alkalmazások a böngészőben futó, az Azure Active Directory itt adhatja meg, az Active Directory Authentication Library vagy a adal.js.  Adal.js meg kizárólagos életben célja, hogy az alkalmazás, hogy a hozzáférési jogkivonat könnyen.  Annak bemutatják, hogy milyen egyszerűen egyszerű, itt azt fogja hozza létre a AngularJS teendőlista alkalmazás, amely:

- A felhasználó bejelentkezik az Azure Active Directory az identitásszolgáltató használata az alkalmazásba.
- A felhasználó bizonyos információkat jelenít meg.
- Biztonságos hívja fel az alkalmazást a tegye lista API Bearer tokenek AAD a használata.
- Az alkalmazás ki a felhasználó bejelentkezik.

A teljes munkaidő-alkalmazás létrehozására kell:

2. Az alkalmazás regisztrálása Azure AD.
3. ADAL telepítse és állítsa be a biztonságos jelszó-hitelesítés.
5. ADAL segítségével biztonságos a biztonságos jelszó-hitelesítés lapjait.

Az első lépések, [Töltse le az alkalmazás skeleton](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) vagy [a kész minta letöltése](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Az Azure Active Directory bérlői, ahol felhasználók létrehozása és egy alkalmazás regisztrálása is szüksége lesz.  Ha még nem rendelkezik a bérlői, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. a DirectorySearcher alkalmazás regisztrálása*
Ahhoz, hogy a felhasználók hitelesítő és tokenek az alkalmazást, először kell a Azure AD-bérlő rögzítheti azt:

-   Jelentkezzen be az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com)
-   A bal oldali navigációs sávon kattintson **Az Active** Directory
-   Jelölje ki a bérlői webhelyet, ahol az alkalmazás regisztrálni.
-   Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra az alsó fiókban.
-   Az útmutatást követve hozzon létre egy új **webalkalmazás és/vagy WebAPI**.
    -   Az alkalmazás **neve** leírja a végfelhasználók számára alkalmazást.
    -   Az **Átirányítás Uri** hely, amelyre AAD tokenek ad eredményül.  Ez a példa az alapértelmezett helye`https://localhost:44326/`
-   Regisztráció végrehajtása után, AAD rendel az alkalmazás egy egyedi **Ügyfél-azonosító**.  Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a **beállítás** lapon.
- Adal.js kommunikálni az Azure Active Directory a OAuth implicit folyamat használja.  Az alkalmazás által engedélyeznie kell a implicit folyamat:
    - Töltse le az alkalmazás jegyzék **Cikkét kezelése**gombra kattintva.
    - Nyissa meg a jegyzék, és keresse meg a `oauth2AllowImplicitFlow` tulajdonság. Állítsa az értékét `true`.
    - Mentés, és töltse fel az alkalmazás jegyzék gombra kattintva **Kezelheti a cikkét** még egyszer.

## <a name="2-install-adal--configure-the-spa"></a>*2. a ADAL telepítse és állítsa be a biztonságos jelszó-hitelesítés*
Most, hogy az alkalmazások az Azure Active Directory, adal.js telepítése, és írja be a kapcsolódó azonosító kódot.

-   Kezdje el a csomag Manager konzolon TodoSPA projekthez adal.js hozzáadásával:
  - Töltse le a [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) , és vegye fel, hogy a `App/Scripts/` project címtár.
  - Töltse le a [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) , és vegye fel, hogy a `App/Scripts/` project címtár.
  - Minden egyes parancsfájl vége előtt betöltése a `</body>` a `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   A biztonságos jelszó-hitelesítés 's kódmentes kattintva tegye lista API fogadja el a token származhat a böngészőben a kódmentes kell adni a alkalmazás regisztráció konfigurációs adatait. Nyissa meg a TodoSPA projekt `web.config`.  Az elemek az értékek lecserélése a `<appSettings>` szakasz meg őket a Azure portálra bemeneti értékek megfelelően.  A kód minden alkalommal ADAL hivatkoznak ezeket az értékeket.
    -   A `ida:Tenant` a Azure AD-bérlő, például contoso.onmicrosoft.com tartománya
    -   A `ida:Audience` **Ügyfél-azonosító** az alkalmazás a portáljáról másolt kell lennie.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. ADAL használata a biztonságos jelszó-hitelesítés lapok biztonságossá tétele*
Adal.js épített AngularJS útvonal és a http-szolgáltatók integrálása lehetővé teszi, hogy a biztonságos jelszó-hitelesítés az egyéni nézetek biztonságos.

- A `App/Scripts/app.js`, jelenítse meg a adal.js modulban:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Most töltse a `adalProvider` konfigurációs értékekkel, az alkalmazás regisztráció is a `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Most, hogy biztosítsa a `TodoList` megtekintése az alkalmazásban, csak egy sort szükség – `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Ekkor az azt jelenti, hogy jelentkezzen be a felhasználók és az API kódmentes Bearer jogkivonat védett kérések ki egy biztonságos egyoldalas alkalmazást.  Amikor a felhasználó kattint a `TodoList` hivatkozás, adal.js automatikusan átirányítja az Azure Active Directory való bejelentkezéshez szükség esetén.  Ezeken kívül adal.js fog automatikusan csatolhat egy access_token bármely ajax-kérelmek az alkalmazás kódmentes küldött.  A fenti összeállítása a adal.js - egészében bA minimálisan szükség, de számos egyéb szolgáltatások, amelyek a gyógyfürdőhelyek hasznosak lehetnek:

- Explicit módon probléma bejelentkezési és kéréseket kijelentkezés definiálhatók adal.js meghívása a vezérlők függvények.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Érdemes azt is, az alkalmazás felhasználói felületén a felhasználói adatok bemutatására.  Az adal szolgáltatás már hozzá lett adva a `userDataCtrl` vezérlő, elérheti a `userInfo` a társított nézetben objektum `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Is sok olyan esetek, amelyek fog szeretné, hogy ha a felhasználónak van bejelentkezve.  Is használhatja a `userInfo` ezen információk összegyűjtése az objektumot.  Ha például a `index.html` jelenítheti meg a hitelesítési állapot alapján "Bejelentkezési" vagy "Kijelentkezés" gomb:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Gratulálok! Az Azure Active Directory integrált egyetlen lap alkalmazás befejeződött.  Azt is hitelesíti a felhasználókat, biztonságos hívja fel a kódmentes OAuth 2.0-s verzióját használja, és a felhasználó alapvető adatainak.  Ha még nem tette meg, ezt követően az idő az egyes felhasználók a bérlő kitöltéséhez.  Futtassa a kattintva tegye lista biztonságos jelszó-hitelesítés, és jelentkezzen be egy azoknak a felhasználóknak.  Tevékenységek hozzáadása a felhasználók a listában, jelentkezzen ki, és jelentkezzen be újra.

Adal.js megkönnyíti az alábbi közös identitás funkciók részévé tenni az alkalmazást.  Azt gondoskodik a dirty munkáját vonatkozó - gyorsítótár-kezelés, a OAuth protokoll támogatása, a felhasználó bemutató tartása a bejelentkezési felhasználói felülete frissítése a lejárt tokenek, és így tovább.

A kész minta (nélkül a konfigurációs értékek) megadva hivatkozás, az [alábbi](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Most már áthelyezheti további forgatókönyvek.  Előfordulhat, hogy szeretne kipróbálni:

[CORS webes API hívhat egészében >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
