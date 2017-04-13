
<properties
    pageTitle="Azure Active Directory 2.0-s verzió OAuth ügyfél hitelesítő adatok folyamat |} Microsoft Azure"
    description="Építőelem webalkalmazások Azure Active Directory végrehajtása az OAuth 2.0-s hitelesítési protokoll használatával."
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>Áramlás 2.0-s verzió protokollok - OAuth 2.0 ügyfél hitelesítő adatok

Az [Adja meg az OAuth 2.0 ügyfél hitelesítő adatait](http://tools.ietf.org/html/rfc6749#section-4.4), néven is ismert "két Egyszárú OAuth", használható az alkalmazás azonosítója segítségével is webes forrásokat.  Általában arra használják, a kiszolgáló szolgáltatásokat foglalja össze, kell futás a háttérben az azonnali precense a végfelhasználók nélkül.  Ilyen típusú alkalmazások gyakran nevezik **démonok** vagy **szolgáltatás fiókok**.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

A további tipikus három "három Egyszárú OAuth," az ügyfélalkalmazás engedélyt egy erőforrás egy felhasználó nevében eléréséhez.  A jogosultsági a **meghatalmazott** a felhasználótól, általában a során [beleegyezés](active-directory-v2-scopes.md) az alkalmazás.  Jó helyen jár az ügyfél hitelesítő adatok folyamat engedélyekkel rendelkező **közvetlenül** az alkalmazás magát.  Amikor az alkalmazás bemutatja az erőforrás jogkivonat, az erőforrás kényszeríti a, hogy rendelkezik-e az alkalmazást, magát engedélyezési műveletet néhány – néhány felhasználónak engedélyezési nem.

## <a name="protocol-diagram"></a>Protocol (protokoll) diagram
A teljes ügyfél hitelesítő adatok folyamat néz ki – minden egyes lépés részletesen ismertetik.

![Ügyfél hitelesítő adatok továbbításához](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Közvetlen engedélyezési beszerzése 
Kétféle módon, hogy az alkalmazás általában kap egy erőforrás hozzáférés közvetlen engedélyezése: az erőforrás a hozzáférés-vezérlési lista, vagy alkalmazás jogosultsági hozzárendelés az Azure Active Directory keresztül.  Többféle módon más erőforrás dönthet úgy engedélyezheti az ügyfelek, és minden erőforrás-kiszolgáló dönthet a módszert, amellyel logikusnak alkalmazásához.  E két módszer a leggyakoribb az Azure Active Directory és az ügyfelek és információforrások, amelyek a ügyfél hitelesítő adatok folyamat végrehajtandó reccommended.

### <a name="access-control-lists"></a>Hozzáférés-vezérlési listák
Egy adott erőforrás-szolgáltató előfordulhat, hogy be egy alapuló Alkalmazásazonosítók, hogy tudja, és bizonyos adott szintű hozzáférést biztosít a engedélyezése jelölőnégyzetet.  Az erőforrás kap a 2.0-s verzió végpont jogkivonat, amikor a token dekódolását, és bontsa ki a az ügyfél azonosítója a a `appid` és `iss` követelések.  Ezután összehasonlíthatja, hogy az alkalmazást, néhány hozzáférési vezérlőelem-lista (vezérlés) alapján tárol.  A beállítási lehetőséget és a hozzáférési lista metódusát jelentősen kibővíti az erőforrás változhat.

Egy közös használatieset ilyen hozzáférés-vezérlési listák webalkalmazáshoz dugványok vizsgálat vagy a webes API-val.  A webes api csak adhatnak hozzá tartozó teljes engedélyek csak egy részhalmazát az egyes ügyfeleknek.  De annak érdekében, hogy az api végpont vizsgálatok futtatásához a próba-ügyfél jön létre, amely szerez token származhat a 2.0-s verzió végpontot, és elküldése az api-nak.  Az API-t is majd vezérlés az api teljes funkciók teljes eléréséhez a próba-ügyfél azonosítója.  Figyelje meg, hogy a szolgáltatás Ha ilyen listáját, látnia kell, hogy nem csak a hívó `appid`, de is ellenőrzése, hogy a `iss` a jogkivonat megbízható is.

Az ilyen típusú engedélyezési közös démonok és a személyes Microsoft-fiók fogyasztói felhasználók tulajdonában adatok eléréséhez szükséges szolgáltatás fiókoknak.  A szervezetek tulajdonában lévő adatokat javasolt, hogy az alkalmazás perimssions keresztül szükséges engedély finomítása.

### <a name="application-permissions"></a>Szolgáltatásalkalmazás jogosultságainak
Hozzáférés-vezérlési listák helyett API-khoz is fedheti fel az alkalmazás kaphatnak **Alkalmazásengedélyek** csoportja.  Az alkalmazás jogosultsági adható meg az alkalmazás egy szervezet rendszergazdája, és csak akkor használható, hogy a szervezet és az alkalmazottak adatainak eléréséhez.  Ha például a Microsoft Graph közzététele több Alkalmazásengedélyek:

- A levelek olvasása összes postaládákba
- Olvasásra és írásra mail összes postaládákba
- Olyan felhasználóként Levélküldés
- A címtár-adatok beolvasása
- [+ További](https://graph.microsoft.io)

Annak érdekében, hogy ezeket az engedélyeket az alkalmazás szerez, hajtsa végre az alábbi lépéseket.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Az engedélyek az alkalmazás regisztrációs portálon kérése

- Nyissa meg az alkalmazás [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)vagy [-alkalmazás létrehozása](active-directory-v2-app-registration.md) , ha még nem tette meg.  Meg kell győződjön meg arról, hogy az alkalmazás hozott létre legalább egy alkalmazás titkos.
- Keresse meg a **Közvetlen Alkalmazásengedélyek** szakaszt, és hozzáadása az alkalmazáshoz szükséges engedélyeket.
- Feltétlenül **menti** az alkalmazás regisztráció

#### <a name="recommended-sign-the-user-into-your-app"></a>Javasolt: a felhasználó jelentkezzen be az alkalmazásba

A szokásos arról, hogy az alkalmazás felépíteni használt Alkalmazásengedélyek, az alkalmazás egy lap vagy nézet, amely lehetővé teszi a rendszergazdát, hogy az alkalmazás engedélyek jóváhagyása lesz szüksége.  Ez a lap lehet az alkalmazás regisztrációs folyamat, az alkalmazás beállításokban vagy egy dedikált "Csatlakozás" folyamat része.  Sok esetben célszerű kattintva jelenítse meg az alkalmazáshoz "Csatlakozás" nézet csak azt követően felhasználó munkahelyi vagy iskolai Microsoft-fiókkal van bejelentkezve.

A felhasználónak bejelentkezés az alkalmazásba lehetővé teszi a organziation, amelyhez a felhasználó tagja előtt mintaüzenetet jóváhagyásához a szolgáltatásalkalmazás jogosultságainak azonosításához.  Miközben, nem feltétlenül szükséges módon tud segítséget nyújtani a szervezeti felhasználók számára a további intuitív használhatóság létrehozása.  Jelentkezzen be a felhasználó, hajtsa végre a [2.0-s verzió protokoll oktatóanyagok](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Az engedélyek kérése a címtár-rendszergazda

Ha készen áll engedélyt a vállalati rendszergazda az, a felhasználó irányíthatja át a 2.0-s verzió **admin beleegyezés végpontot**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| Bérlői webhelyen | szükséges | A címtár-bérlő engedély kérése kívánt.  Biztosítható, hogy a globálisan egyedi azonosítója vagy a felhasználóbarát név formátumát.  Ha nem tudja, melyik bérlői a felhasználó tagja, és nem szeretné az illetőnek, jelentkezzen be a bármely bérlői, használhatja a `common`. |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| redirect_uri | szükséges | A redirect_uri, amelyre a választ, és kezelheti az alkalmazás elküldését.  Azt pontosan egyeznie kell a regisztrálta a portálon, azzal a különbséggel URL-címként kódolandó kell lennie, és a beállíthatja, hogy további elérési út szegmensek redirect_uris közül. |
| állam | ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték.  Lehet, hogy a minden olyan tartalom, a kívánt karakterlánc.  Az alkalmazás a felhasználó állapotával kapcsolatos információk kódolását, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha az állapot szolgál. |

Ezen a ponton Azure Active Directory hivatkozási fogja, hogy csak egy Bérlői rendszergazda jelentkezzen be a kérés végrehajtása.  A rendszergazda kérni fogja jóváhagyni az összes alkalmazás közvetlen engedélyeket, hogy az alkalmazás a regisztrációs portálon kérte. 

##### <a name="successful-response"></a>A sikeres válasz
Ha a rendszergazda hagyja jóvá az engedélyeket az alkalmazás, a sikeres válasz lesz:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| Bérlői webhelyen | A címtár-bérlő, hogy az alkalmazás a engedélyekkel azt kért globálisan egyedi azonosítója formátumban. |
| állam | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték.  Lehet, hogy a minden olyan tartalom, a kívánt karakterlánc.  Az alkalmazás a felhasználó állapotával kapcsolatos információk kódolását, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha az állapot szolgál. |
| admin_consent | Állítja be `True`. |


##### <a name="error-response"></a>Hiba válasz
Ha a rendszergazda nem hagyja jóvá az engedélyeket az alkalmazás, a sikertelen válasz lesz:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők azonosítása: a hiba okát.  |

Miután sikeres válasz érkezett-e a kiépítési végpont alkalmazásból, az alkalmazás a közvetlen Alkalmazásengedélyek meg a kért szerzett.  Most már áthelyezheti az alakzatot a kívánt erőforrás token kérő.

## <a name="get-a-token"></a>Jogkivonat beszerzése
Amikor az alkalmazás már szerezte be a szükséges engedélyt, akkor folytathatja API-k hozzáférési jogkivonat beszerző.  Úgy juthat az ügyfélprogram jogkivonat hitelesítő adatok megadása, a bejegyzés kérelem küldése a `/token` 2.0-s verzió végpont:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| hatókör | szükséges | Az átadott érték a `scope` a kérelem paraméter az erőforrás-azonosító (URI a alkalmazás azonosító) kell lennie az elhelyezni kívánt erőforrás a `.default` utótag.  Így például a Microsoft Graph adni, az értéket kell `https://graph.microsoft.com/.default`.  Ezt az értéket, hogy az összes alkalmazás közvetlen engedélyeinek állította be, az alkalmazás, akkor kell egy jogkivonat kibocsátása a kívánt erőforrás kapcsolatos szerint a lehetőségekből for a 2.0-s verzió végpont tájékoztatást. |
| client_secret | szükséges | Az alkalmazás titkos, amely az alkalmazás létrehozta a regisztrációs portálon. |
| grant_type | szükséges | Meg kell `client_credentials`. | 

#### <a name="successful-response"></a>A sikeres válasz
A sikeres visszajelzés lép, hogy az űrlap:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| access_token | A kért hozzáférési jogkivonat. Az alkalmazás a token segítségével a védett erőforrás, például a webes API hitelesítést végezni. |
| token_type | Jogkivonat típusa értékét jelöli. Az egyetlen típus, amely támogatja az Azure Active Directory `Bearer`.  |
| expires_in | Mennyi ideig a hozzáférési jogkivonat érvénytelen (másodpercben). |

#### <a name="error-response"></a>Hiba válasz
Választ hiba lép, hogy az űrlapon:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba.  |
| error_codes | Diagnosztikai segítő STS adott hibakódok listája.  |
| időbélyeg | Az idő, a hiba történt. |
| trace_id | A kérés diagnosztika segítő egyedi azonosítója  |
| correlation_id | A kérelem diagnosztika keresztül összetevők segítő egyedi azonosítója |

## <a name="use-a-token"></a>Jogkivonat használata
Most, hogy egy token szerzett adott jogkivonat, hogy az erőforrás kéréseket is használhatja.  Ha lejár a jogkivonat, egyszerűen ismételje meg az értekezlet-összehívást a `/token` szerezheti be egy friss jogkivonat végpontot.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Példa a kódot.
Példa egy alkalmazást, hogy a client_credentials adni, használja a felügyeleti eszközök beleegyezés végpontot, tanulmányozza a [2.0-s verzió démon kód minta](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
