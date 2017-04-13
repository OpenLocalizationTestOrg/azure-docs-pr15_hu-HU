<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Az alkalmazások típusú készíthet az Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Típusú alkalmazások

Azure Active Directory (Azure Active Directory) B2C támogatja a régi alkalmazás architektúrákban sokféle hitelesítést. Az üzleti szabványos protokollok [OAuth 2.0-s](active-directory-b2c-reference-protocols.md) vagy [OpenID csatlakozás](active-directory-b2c-reference-protocols.md)mindegyikük alapulnak. A dokumentum röviden alkalmazások úgy, hogy milyen típusú, a nyelv vagy platform függetlenül is előnyben részesíti. Segít a magas szintű forgatókönyvek [alkalmazások létrehozásába megkezdése](active-directory-b2c-overview.md#getting-started)előtt megismerését.

## <a name="the-basics"></a>Az alapvető műveletei
Azure Active Directory B2C használó minden app regisztrálni kell a [B2C címtár](active-directory-b2c-get-started.md) az [Azure-portálon](https://portal.azure.com/)keresztül. Az alkalmazás regisztrációs folyamat gyűjti össze, és több érték rendel az alkalmazást:

- **Azonosítója** egyedileg azonosító az alkalmazást.
- **Átirányítás URI** irányítsa át a válaszokat vissza az alkalmazás a használható.
- Minden más esetben-specifikus érték. További részletekért megtudhatja, hogyan lehet [rögzíteni az alkalmazás](active-directory-b2c-app-registration.md).

Miután regisztrálta az alkalmazást, akkor kommunikál az Azure Active Directory kérelem küld az Azure Active Directory 2.0-s verzió végpontot:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Azure Active Directory B2C küldött minden kérés adja meg a **házirend**. A házirend Azure Active Directory viselkedését határozza meg. Ezeket a végpontokat használatával hozza létre felhasználói élményt erősen testre szabható csoportját. Gyakori házirendek olyan előfizetési, bejelentkezés, és felhasználóiprofil-Szerkesztés házirendek. Ha nem ismeri a házirendek, olvassa el az Azure Active Directory B2C [bővíthető keretet](active-directory-b2c-reference-policies.md) a folytatás előtt.

A minden alkalmazás interakció egy 2.0-s verzió végpontot hasonló magas szintű mintát követi:

1. Az alkalmazást a 2.0-s verzió végpont [házirend](active-directory-b2c-reference-policies.md)végrehajtani a felhasználó irányítja.
2. A felhasználó a házirendet a házirend-definícióhoz szerint befejezi.
4. Az alkalmazás biztonsági jogkivonat bizonyos típusú fogad a 2.0-s verzió végpontot.
5. Az alkalmazás hozzáférést, illetve védett védett erőforrás a biztonsági jogkivonat használja.
6. Az erőforrás-kiszolgáló ellenőrzi a biztonsági jogkivonat, ellenőrizze, hogy is hozzáférést.
7. Az alkalmazás rendszeresen frissíti a biztonsági jogkivonat.

<!-- TODO: Need a page for libraries to link to -->
Ezek a lépések némileg alapján szeretne összeállítani alkalmazástípust eltérő. Megnyitás tárakat a részleteket is címe.

## <a name="web-apps"></a>Web Apps alkalmazások
Web Apps alkalmazások (a .NET, PHP, Java, fonetikus, Python és Node.js is beleértve), amely egy kiszolgálón tárolt és a böngészőn keresztül érhető el Azure Active Directory B2C támogatja a [OpenID csatlakozás](active-directory-b2c-reference-protocols.md) összes felhasználói élményt. Ide tartoznak a bejelentkezési,-előfizetés, és a profilok kezelése. Azure Active Directory B2C végrehajtásának OpenID csatlakozni a web app által hitelesítési kérések kiadását Azure AD a felhasználói élmény kezdeményez. A kérés eredménye egy `id_token`. Ez a biztonsági jogkivonat a felhasználói azonosító jelöli. Azt is megtudhatja a felhasználó követelések formájában:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

További tudnivalók az típusú tokenek és -alkalmazás [B2C jogkivonat referencia a](active-directory-b2c-reference-tokens.md)rendelkezésre álló jogcímalapú.

Egy webalkalmazás minden végrehajtás [házirend](active-directory-b2c-reference-policies.md) esetén magas szintű lépéseket:

![Web App sávok képe](./media/active-directory-b2c-apps/webapp.png)

Az adatérvényesítési a `id_token` használatával egy nyilvános aláírási kulcs az Azure AD-e a fogadott elegendő szeretné ellenőrizni a annak a felhasználónak. Ez a munkamenet cookie-k azonosítja a felhasználó a későbbi lap kérések használható is állítja be.

Lásd: Ebben az esetben a művelet, próbálkozzon a web app bejelentkezési mintakódok a [szakasz az első lépések](active-directory-b2c-overview.md#getting-started)a.

Kívül megkönnyítése egyszerű bejelentkezés, a kiszolgáló webalkalmazást előfordulhat, hogy is kell tudni férnie háttéradatbázist webszolgáltatás. Ebben az esetben a web app is némileg eltérő [OpenID csatlakozási folyamat](active-directory-b2c-reference-oidc.md) végrehajtása és szerezheti be a token engedélyezési kódok használatával, és frissítse tokenek. Ebben az esetben olyan téglalapként ábrázol, az alábbi [webes API -k csoportban](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Webes API-hoz
Azure Active Directory B2C biztonságossá tétele webes szolgáltatások, például az alkalmazás RESTful webes API is használhatja. Webes API-khoz OAuth 2.0-s segítségével adataikat, biztonságos hitelesíti tokenek használata bejövő HTTP felkérést. A hívó az weblapon API hozzáfűzi a HTTP-kérelem engedélyezési fejlécében jogkivonat:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

A webes API használhatja a token a API hívó személyazonosságának ellenőrzése és a hívó adatait kinyerése a token kódolt követelések. További tudnivalók az típusú tokenek és -alkalmazás [Azure Active Directory B2C jogkivonat referencia a](active-directory-b2c-reference-tokens.md)rendelkezésre álló jogcímalapú.

> [AZURE.NOTE]
    Azure Active Directory B2C jelenleg csak a webes API-hoz, amelyek a saját ismert ügyfelek érhetők el. Például a teljes alkalmazás iOS-alkalmazás, a Android alkalmazás és a háttéradatbázist webes API tartalmazhat. Ez a felépítés teljesen támogatott. Lehetővé teszi a partner ügyfélprogram, például egy másik iOS-alkalmazás, a azonos webes API jelenleg nem támogatott eléréséhez. Minden összetevő a teljes alkalmazás meg kell osztania egy egyetlen alkalmazásból.

A webes API kaphatnak a tokenek számos típusú ügyféleszközre is kiterjedhet, ideértve a web Apps alkalmazások, asztali és mobilalkalmazások, egyetlen oldalon alkalmazások, kiszolgálóoldali démonok és más webes API-khoz. Íme egy példa a teljes folyamat, amely felhívja a webes API webalkalmazások számára:

![Web App webes API sávok kép](./media/active-directory-b2c-apps/webapi.png)

Ha többet szeretne megtudni a engedélyezési kódok frissítés tokenek és tokenek első lépései, ismerje meg az [OAuth 2.0-s Protocol (protokoll)](active-directory-b2c-reference-oauth-code.md).

A webes API biztonságos Azure Active Directory B2C használatával című témakörben olvashat, nézze meg a webes API oktatóanyagok a [az első lépések szakaszában](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Mobile és a saját alkalmazások
Eszközök, például a mobil és az asztali alkalmazások telepített alkalmazások gyakran kell tudni férnie a háttéradatbázist services vagy a webes API-khoz felhasználók nevében. Vehet testre szabott identitás kezelése élményt natív alkalmazásait és biztonságos módon hívás háttéradatbázist szolgáltatások Azure Active Directory B2C és az [OAuth 2.0-s engedélyezési kód továbbításához](active-directory-b2c-reference-oauth-code.md).  

Ez a folyamat az alkalmazás végrehajtja a [házirendek](active-directory-b2c-reference-policies.md) és kap egy `authorization_code` az Azure Active Directory, a házirend kiegészítette. A `authorization_code` háttéradatbázist szolgáltatások jelenleg bejelentkezett felhasználó nevében hívja fel az alkalmazás jogosultsági jelöli. Az alkalmazás tudja majd exchange a `authorization_code` esetében a háttérben egy `id_token` és egy `refresh_token`.  Az alkalmazás használata a `id_token` HTTP-összehívásokban háttéradatbázist webes API hitelesítést végezni. Azt is használhatja a `refresh_token` egy új megszerezni `id_token` régebbi egy lejártakor.

> [AZURE.NOTE]
    Azure Active Directory B2C jelenleg csak az alkalmazás saját háttéradatbázist webszolgáltatás eléréséhez használt tokenek támogatja. Például a teljes alkalmazás iOS-alkalmazás, a Android alkalmazás és a háttéradatbázist webes API tartalmazhat. Ez a felépítés teljesen támogatott. Az iOS-alkalmazás használatával történő elérésére partnerek webes API OAuth 2.0-s hozzáférési jogkivonat engedélyezésének jelenleg nem támogatott. Minden összetevő a teljes alkalmazás meg kell osztania egy egyetlen alkalmazásból.

![Natív alkalmazás sávok képe](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuális korlátozások
Azure Active Directory B2C jelenleg nem támogatja az alkalmazások az alábbi típusú, de vannak a roadmapy. További korlátozásokat és Azure Active Directory B2C kapcsolatos korlátozások [korlátai és korlátozásokat](active-directory-b2c-limitations.md)ismertetjük.

### <a name="single-page-apps-javascript"></a>Egyetlen lap alkalmazások (JavaScript)
Sok modern alkalmazások van az egyoldalas elrendezésű oldalakat alkalmazás előtér elsősorban a JavaScript nyelven íródott. Gyakran alkalmaznak például AngularJS, Ember.js vagy Durandal keretrendszert. A rendelkezésre álló általában Azure AD-szolgáltatás az alkalmazások támogatja a OAuth 2.0-s implicit folyamat használatával. Azonban a folyamat még nem érhető el az Azure Active Directory B2C.

### <a name="daemonsserver-side-apps"></a>Démonok/kiszolgálóoldali alkalmazások
Alkalmazások tartalmazó, hosszú ideig futó folyamatok vagy anélkül, hogy a felhasználó jelenlétét, amelyek működnek kell védett erőforrások, például a webes API-khoz elérése oly módon is. Az alkalmazások hitelesítheti és beszerzése tokenek használ, az alkalmazás identitást (helyett, hogy a felhasználó delegált identitást) és az OAuth 2.0 ügyfél segítségével hitelesítő adatok továbbításához.

Ez a folyamat jelenleg nem támogatott Azure Active Directory B2C szerint. Az alkalmazások csak azt követően egy interaktív felhasználó folyamat történt tokenek szerezhet be.

### <a name="web-api-chains-on-behalf-of-flow"></a>Webes API láncának (a nevében – az áramlás)
Sok architektúrákban webes API hívás másik lefelé irányuló webes API, ahol mindkét Azure Active Directory B2C védi kell, hogy a tartalmazza. Ebben az esetben a webes API háttéradatbázist rendelkező natív ügyfelek közös. Ez a Microsoft online tárhelyre, például az Azure Active Directory Graph API majd szükségessé teszi.

Ebben az esetben láncolt webes API OAuth 2.0-s JWT bearer hitelesítő adatok megadását, a nevében – az áramlás néven is ismert használatával nem használható.  Azonban a nevében – az folyamat nem jelenleg használható az Azure Active Directory B2C.
