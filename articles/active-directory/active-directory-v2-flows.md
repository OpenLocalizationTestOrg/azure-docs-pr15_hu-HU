<properties
    pageTitle="A 2.0-s verzió végpont típusú |} Microsoft Azure"
    description="Az alkalmazások és -esetek az Azure Active Directory 2.0-s verzió végpontot által támogatott típusú."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>A 2.0-s verzió végpont-alkalmazások típusai
A 2.0-s verzió végpont támogatja a régi alkalmazás architektúrákban, ami a iparágban szabványos protokollok [OAuth 2.0-s verzióját](active-directory-v2-protocols.md#oauth2-authorization-code-flow) és/vagy [OpenID csatlakozás](active-directory-v2-protocols.md#openid-connect-sign-in-flow)alapuló sokféle hitelesítést.  A dokumentum röviden készíthet alkalmazások típusú, a nyelv vagy platform függetlenül is előnyben részesíti.  Azt segít megérteni a magas szintű forgatókönyvek előtt, hogy [belevesse be a kódot](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="the-basics"></a>Az alapvető műveletei
Minden alkalmazást a 2.0-s verzió végpont használó kell a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)regisztrálását.  Az alkalmazás regisztrációs folyamat fog összegyűjtése és több érték hozzárendelése az alkalmazást:

- Egy egyedi módon azonosító az alkalmazás **Azonosítója**
- Irányítsa át a válaszokat visszatérhet az alkalmazás használható **URI átirányítása**
- Néhány egyéb forgatókönyv kötött értékek.  Ismerje meg részletesebben, hogyan lehet [rögzíteni az alkalmazás](active-directory-v2-app-registration.md).

Miután regisztrálta, az alkalmazás kommunikál Azure Active Directory kérelem küld az Azure Active Directory 2.0-s verzió végpontot.  Elvégezheti a szükséges Megnyitás keretek és tárat, amellyel gondoskodik a következő kéréseket részleteit, illetve alkalmazhat a hitelesítési logikájának saját maga ezeket a végpontokat kéréseket létrehozásával:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web Apps alkalmazások
Web Apps alkalmazások (.NET PHP, Java, fonetikus, Python, csomópontot, stb), amelyek érhetők el a böngészőn keresztül, végezze el felhasználói bejelentkezés [OpenID kapcsolódás](active-directory-v2-protocols.md#openid-connect-sign-in-flow)használatával.  A csatlakozás OpenID a web app kap egy `id_token`, a biztonsági jogkivonat, amely ellenőrzi a felhasználói azonosító, és a felhasználó követelések formájában információt tartalmaz:

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

Megismerheti az összes az típusú tokenek és jogcímalapú érhető el egy alkalmazást a [2.0-s verzió jogkivonat-hivatkozást](active-directory-v2-tokens.md).

Webalkalmazások kiszolgáló a bejelentkezési hitelesítési folyamat esetén magas szintű lépéseket:

![Web App sávok képe](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

A nyilvános kulccsal aláírási a 2.0-s verzió végpont kapott id_token érvényesítése is elegendő az biztosítása érdekében a felhasználói azonosító, és adja meg a munkamenet cookie-k azonosítja a felhasználó a következő oldal kérések használható.

Lásd: Ebben az esetben a művelet, próbálja ki egyet a web app bejelentkezési mintakódok az [Első lépések](active-directory-appmodel-v2-overview.md#getting-started) szakaszában.

Kívül egyszerű bejelentkezés a kiszolgáló webalkalmazást előfordulhat, hogy is kell tudni férnie néhány más webszolgáltatás, például a REST API-t.  A kiszolgáló webalkalmazás ebben az esetben egy kombinált OpenID csatlakozás és OAuth 2.0-s folyamat, az [áramlás OAuth 2.0-s engedélyezési kód](active-directory-v2-protocols.md#oauth2-authorization-code-flow)használatával is részt. Ebben az esetben a [webalkalmazás-WebAPI használatába témakör](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)foglalt alatt.

## <a name="web-apis"></a>Webes API-hoz
A 2.0-s verzió végpont biztonságossá tétele, valamint a webes szolgáltatások, például az alkalmazás RESTful webes API is használhatja.  Id_tokens és a munkamenet cookie-k használata helyett webes API segítségével OAuth 2.0-s access_tokens biztonságos adataikat, és a hitelesítési, bejövő felkérést.  A hívó egy webes API-access_token HTTP felkérés engedélyezési fejlécében hozzáfűzi:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

A webes API használhatja a access_token a API hívó személyazonosságának ellenőrzése és a hívó adatait kinyerése a access_token a kódolt követelések.  Megismerheti az összes az típusú tokenek és jogcímalapú érhető el egy alkalmazást a [2.0-s verzió jogkivonat-hivatkozást](active-directory-v2-tokens.md).

A webes API adhat felhasználók való csatlakozás – a/letiltásra egyes funkciók, illetve az adatok a power engedélyek, más néven [hatókörök](active-directory-v2-scopes.md)ki.  Egy hívó alkalmazás szerezheti be a hatókör engedélye a felhasználónak kell hozzájárul a hatókör egy folyamat során.  A 2.0-s verzió végpont fog gondoskodik kérjen engedélyt a felhasználónak, és a felvétel ezeket az engedélyeket az összes access_tokens, amely a webes API kapja.  A webes API kell aggódnia amiatt, hogy az összes az egyes hívó kap access_tokens ellenőrzése és a megfelelő engedélyezés ellenőrzések végrehajtásához.

A webes API is access_tokens fogadjanak bármilyen típusú alkalmazásokat, a kiszolgáló webalkalmazások, asztali és mobilalkalmazások, egyetlen oldalon alkalmazások, kiszolgáló egymás démonok és akár más webes API-khoz.  A magas szintű webes api-hitelesítés folyamatábrája az alábbi képlettel történik:

![Webes API sávok kép](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Authorization_codes, refresh_tokens és a részletes lépéseket az első access_tokens kapcsolatos további információért ismerje meg az [OAuth 2.0-s Protocol (protokoll)](active-directory-v2-protocols-oauth-code.md).

Megtudhatja, hogy miként biztonságossá tétele a webes api az oauth2 hitelesítési mód access_tokens, olvassa el a webes api mintakódok, az [első lépések szakaszában](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile és a saját alkalmazások
Mobil és az asztali alkalmazások, például egy eszközön telepített alkalmazások gyakran kell tudni férnie, kódmentes services vagy a webes API-k adatok tárolására, és hajtsa végre a különböző függvényekre felhasználó nevében.  Az alkalmazások vehet bejelentkezési és engedélyezési [folyamat OAuth 2.0-s engedélyezési kód](active-directory-v2-protocols-oauth-code.md)használatával kódmentes szolgáltatások.  

Ez a folyamat egy az alkalmazást a 2.0-s verzió végpont felhasználónak bejelentkezés, kódmentes szolgáltatások a jelenleg bejelentkezett felhasználó nevében hívja fel az alkalmazás jogosultsági jelöl, amely alapján egy authorization_code kap.  Az alkalmazás majd is exchange-OAuth 2.0-s access_token és egy refresh_token a háttérben a authoriztion_code.  Az alkalmazás segítségével a access_token webes API-khoz hitelesíteni a HTTP-kérelmeket, és használatával a refresh_token segíthet új access_tokens régebbi eszköz lejártakor.

![Natív alkalmazás sávok képe](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Egyetlen lap alkalmazások (javascript)
Modern alkalmazások sok van egy egyetlen lap alkalmazás (biztonságos jelszó-hitelesítés) előtér írt elsősorban a javascript és gyakran használata keretek például AngularJS, Ember.js, Durandal és stb.  Az Azure Active Directory 2.0-s verzió végpontot támogatja az alkalmazások használata a [OAuth 2.0-s Implicit Flow](active-directory-v2-protocols-implicit.md).

Ez a folyamat az alkalmazás a 2.0-s verziója kap tokenek végpont közvetlenül, engedélyezhetik bármely kódmentes kiszolgáló cseréje végrehajtása nélkül.  Ezzel a hitelesítési logika és a munkamenet-kezelési állapotba helyezze csupán a javascript-ügyfél, a felesleges oldalt átirányítások végrehajtása nélkül.

![Implicit folyamatábra sávok képe](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Lásd: Ebben az esetben működését, próbálja meg el az [Első lépések](active-directory-appmodel-v2-overview.md#getting-started) szakaszában egyoldalas alkalmazás mintakódok egyikét.

### <a name="daemonsserver-side-apps"></a>Démonok/kiszolgáló egymás alkalmazások
Alkalmazások tartalmazó, hosszú futó folyamatok vagy anélkül, hogy a felhasználó jelenlétét, amelyek működnek kell védett erőforrások, például a webes API-khoz elérése oly módon is.  Az alkalmazások hitelesítheti és az alkalmazás-azonosító (helyett egy felhasználói delegált azonosító) az OAuth 2.0 ügyfél használatával tokenek hitelesítő adatok folyamat első.

Ez a folyamat az alkalmazás tokenek beolvassa által műveletek közvetlen végzése a `/token` végpont:

![Démon alkalmazás sávok képe](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Egy démon alkalmazás összeállítása, lásd: az [Első lépések](active-directory-appmodel-v2-overview.md#getting-started) szakaszában az ügyfél hitelesítő adatok documeenation, vagy olvassa el [a .NET minta](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)alkalmazásba.

## <a name="current-limitations"></a>Aktuális korlátozások
Alkalmazások ilyen típusú jelenleg nem támogatott a 2.0-s verzió végpontot, de a az ütemterv.  További korlátozásokat és a 2.0-s verzió végpont korlátozások a [2.0-s verzió korlátozások cikk](active-directory-v2-limitations.md)témakörben olvashat.

### <a name="chained-web-apis-on-behalf-of"></a>Láncolt webes API-hoz (a-nevében-:)
Sok architektúrákban egy másik későbbi webes API-val, mind a 2.0-s verzió végpont által biztosított hívás kell, hogy a webes API tartalmazzák.  Ebben az esetben a webes API kódmentes, amelyik meghívja a Microsoft Online szolgáltatás, például az Office 365- vagy a diagram API rendelkező natív ügyfelek a közös.

Ebben az esetben láncolt webes API segítségével OAuth 2.0-s Jwt Bearer hitelesítő adatok megadását, más néven [On-Behalf-Of Flow](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)nem használható.  Azonban a On-nevében – a folyamat nem jelenleg használható az 2.0-s verzió végpontot.  Ez a munkafolyamat működése a általában elérhető Azure Active Directory megtekintéséhez szolgáltatás, olvassa el a [kódot a-nevében-: minta GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
