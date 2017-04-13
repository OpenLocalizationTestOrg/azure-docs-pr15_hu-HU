

<properties
    pageTitle="Azure Active Directory 2.0-s verzió OAuth engedélyezési folyamat kód |} Microsoft Azure"
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>2.0-s verzió protokollok - OAuth 2.0-s engedélyezési kód Flow

Védett erőforrások, például a webes API-khoz eléréséhez szükséges eszközön telepített alkalmazások OAuth 2.0-s engedélyezési kód megadása használható.  Az alkalmazás modell 2.0-s verzió féle végrehajtásának OAuth 2.0-s verzióját használja, felveheti a jelentkezzen be, és az API-mobil és az asztali alkalmazások való hozzáférést.  Ez az útmutató nyelvi beállítástól független, és ismerteti, hogyan lehet az üzenetek küldése és fogadása HTTP nélkül, a Megnyitás-forrás tárak közül.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

A OAuth 2.0-s engedélyezési kód folyamat a [4.1 az OAuth 2.0 specifikáció](http://tools.ietf.org/html/rfc6749)ismertetett.  A legtöbb alkalmazás típusú, beleértve a [web Apps alkalmazások](active-directory-v2-flows.md#web-apps) és a [natív módon telepített alkalmazások](active-directory-v2-flows.md#mobile-and-native-apps)hitelesítési és engedélyezési végrehajtásához használatos.  Biztonságos módon szerezheti be access_tokens információforrások, amelyek használatával a 2.0-s verzió végpont biztosított eléréséhez használt alkalmazások lehetővé teszi.  

## <a name="protocol-diagram"></a>Protocol (protokoll) diagram
Magas szintű a teljes hitelesítési folyamat egy natív és mobile alkalmazáshoz néz ki egy kicsit:

![OAuth Auth kód továbbításához](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Egy engedélyezési kód kérése
A kód engedélyezési folyamat kezdődik az ügyfél, a felhasználót, hogy mely irányítja a `/authorize` végpontot.  Az ügyfél a kérést, az azt jelzi, a szerezheti be a felhasználó a szükséges engedélyekkel:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Kattintson a kérelem végrehajtásához az alábbi hivatkozásra. Bejelentkezve, a böngésző e irányítva a `https://localhost/myapp/` az egy `code` a böngésző címsorába.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| Bérlői webhelyen | szükséges | A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték `common`, `organizations`, `consumers`, és a bérlői azonosítóját.  További részleteket [protokoll alapjai](active-directory-v2-protocols.md#endpoints)című cikkben. |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| response_type | szükséges | Tartalmaznia kell `code` az engedélyezés kód továbbításhoz. |
| redirect_uri | ajánlott | Az alkalmazást, ahol küldött és megkapta az alkalmazás a hitelesítési válaszok redirect_uri.  Azt pontosan egyeznie kell a regisztrálta a portálon, kivéve akkor kell lennie az URL-címként kódolandó redirect_uris közül.  A natív és mobil alkalmazás, az alapértelmezett értéket kell használni `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| hatókör | szükséges | [Keresési tartományok](active-directory-v2-scopes.md) , amelyet a felhasználó hozzájárul terület tagolt listáját.  |
| response_mode | ajánlott | Adja meg a módszert, amellyel az eredményül kapott jogkivonat vissza küldje el az alkalmazást.  Lehet `query` vagy `form_post`.  |
| állam | ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték.  Lehet, hogy a minden olyan tartalom, a kívánt karakterlánc.  Véletlenszerűen generált egyedi érték [webhelyközi kérelem hamisítása támadásokkal letiltom](http://tools.ietf.org/html/rfc6749#section-10.12)a szokásos szolgál.  Az állapot kódolás információt az alkalmazás a felhasználó állapotát, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha is szolgál. |
| figyelmeztető üzenet | nem kötelező. | A szükséges felhasználói beavatkozás típusáról.  Jelenleg csak érvényes értékek "bejelentkezés", "nincs", "beleegyezés és.  `prompt=login`a felhasználó megadnia a hitelesítő adatait, hogy kérésre negating egyszeri bejelentkezés a program kényszerítse.  `prompt=none`Ellenkező - biztosítja, hogy a felhasználó nem jelennek a minden interaktív bármilyen kérdés.  A kérés csendes egyszeri bejelentkezés a keresztül nem fejeződhet, ha a 2.0-s verzió végpont hibát jelez.  `prompt=consent`Elindítja a OAuth hozzájárulása párbeszédpanel után a felhasználó bejelentkezik a, a felhasználót a alkalmazásba engedélyek kéri. |
| login_hint | nem kötelező. | Használható előre töltse ki a username/e-mail cím mezőbe, a bejelentkezési lapon a felhasználó számára, ha tudja, hogy a felhasználónév időszakokra.  Gyakran alkalmazásokat fogja használni a paraméter újbóli hitelesítést problémákat már kiolvasott felhasználónév egy előző bejelentkezési használata közben a `preferred_username` formál. |
| domain_hint | nem kötelező. | Egyike lehet `consumers` vagy `organizations`.  Tartalmaz, ha azt kihagyja az e-mailes keresési folyamata a felhasználó halad át a 2.0-s verzió bejelentkezési lapján, amelyek hatékonyabbnak kissé felhasználói élmény.  Gyakran alkalmazásokat fogja használni a paraméter újbóli hitelesítés során kivonja a `tid` az előző bejelentkezés.  Ha a `tid` formál érték `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`.  Egyéb esetben használata `domain_hint=organizations`. |

Ezen a ponton a felhasználót a rendszer kéri a megadnia a hitelesítő adatait, és a hitelesítési befejezéséhez.  A 2.0-s verzió végpont biztosítja azt is, hogy a felhasználó az engedélyeket a megjelölt hozzájárult a `scope` paraméteres lekérdezés.  Ha a felhasználó nem járult hozzá bármilyen ezeket az engedélyeket, a program kéri a felhasználó hozzájárul az ehhez szükséges engedélyekkel.  [Engedélyek, a felhasználó hozzájárul ahhoz, és a több elem bérlői alkalmazások itt megadott](active-directory-v2-scopes.md)részleteit.

Amikor a felhasználó hitelesíti, és adja meg a felhasználó hozzájárul ahhoz, a 2.0-s verzió végpont ad vissza az alkalmazást, a megjelölt adott válasz `redirect_uri`, a megadott módszerrel a `response_mode` paraméter.

#### <a name="successful-response"></a>A sikeres válasz
A sikeres választ használatával `response_mode=query` hasonlít:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| kód | A authorization_code, amely kéri az alkalmazás. Az alkalmazás segítségével az engedélyezési kód kérése a cél erőforrás-jogkivonat.  Authorization_codes nagyon rövid élettartamú, általában körülbelül 10 perc múlva járnak. |
| állam | Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy az állapot értékek a kérelem és a válasz azonos. |

#### <a name="error-response"></a>Hiba válasz
Hibaüzenetek is küld a `redirect_uri` , az alkalmazás képes kezelni őket megfelelően:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Hibakódok engedélyezési végpont hibák

Az alábbi táblázat ismerteti a különböző hibakódok, amely a visszaadható a `error` paramétere: a hibaüzenetet.

| Kódszámú hiba jelenik meg | Leírás | Ügyfél művelet |
|------------|-------------|---------------|
| invalid_request | Protocol (protokoll) hiba, például a hiányzó szükséges paramétert. | Javítsa ki, és küldje el az összehívást. Ez a fejlesztés hibaüzenet általában kiszűri kezdeti a tesztelés során.|
| unauthorized_client | Az ügyfélalkalmazás-engedélyezési kód kéréséhez nem engedélyezett. | Ez általában fordul elő, ha az ügyfélalkalmazás az Azure Active Directory nem regisztrált, vagy nem adja hozzá a felhasználó Azure AD-bérlő. Az alkalmazás a felhasználótól is, az alkalmazás telepítése és Azure Active Directory felveszi utasításkészlettel. |
| ACCESS_DENIED | Erőforrás-tulajdonos hozzájárulása megtagadva | Az ügyfélalkalmazás is értesíti a felhasználót, hogy azt nem folytatható, kivéve, ha a felhasználó. |
| unsupported_response_type | Az engedély kiszolgáló nem támogatja a választípust a kérést. | Javítsa ki, és küldje el az összehívást. Ez a fejlesztés hibaüzenet általában kiszűri kezdeti a tesztelés során.|
|server_error | A kiszolgáló váratlan hiba történt. | Ismételje meg a kérelmet. A hibák ideiglenes feltételek származhat. Az ügyfélalkalmazás előfordulhat, hogy magyarázza el, a felhasználónak, hogy a válasz a határidő egy ideiglenes hiba késik. |
| temporarily_unavailable | A kiszolgáló átmenetileg nem elfoglalt, és meg szeretné kezelni a kérést. | Ismételje meg a kérelmet. Az ügyfélalkalmazás előfordulhat, hogy magyarázza el, a felhasználónak, hogy a válasz a határidő egy ideiglenes feltétel késik. |
| invalid_resource |A target erőforrás érvénytelen, mert nem létezik, az Azure Active Directory nem találja, vagy nincs megfelelően beállítva.| Ez azt jelzi, hogy az erőforrás, ha létezik, nincs beállítva a bérlőhöz. Az alkalmazás a felhasználótól is, az alkalmazás telepítése és Azure Active Directory felveszi utasításkészlettel. |

## <a name="request-an-access-token"></a>Egy hozzáférési jogkivonat kérése
Most, hogy egy authorization_code által megvásárolt és a kapott jogosultságot a felhasználó által beváltása a `code` az egy `access_token` a megfelelő erőforrásnézethez küldésével egy `POST` kérése a `/token` végpont:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Próbálja ki a kérelem végrehajtása a Postman! (Ne felejtse el cseréje a `code`)     [ ![Futtatja a Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------------- |
| Bérlői webhelyen | szükséges | A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték `common`, `organizations`, `consumers`, és a bérlői azonosítóját.  További részleteket [protokoll alapjai](active-directory-v2-protocols.md#endpoints)című cikkben. |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| grant_type | szükséges | Meg kell `authorization_code` az engedélyezés kód továbbításhoz. |
| hatókör | szükséges | Keresési tartományok terület tagolt listáját.  A tartományok, ez a szakasz kért egyenértékű vagy a kért az első szakasz hatókörök részhalmazát kell lennie.  Ha a megadott a kérelem hatókörök több erőforrás-kiszolgálók időtartamát a 2.0-s verzió végpont az erőforráshoz megadott az első hatókört token ad vissza.  Keresési tartományok részletes leírást olvassa el az [engedélyek, felhasználó hozzájárul ahhoz, és hatókörök](active-directory-v2-scopes.md).  |
| kód | szükséges | A authorization_code, amely a, a folyamat első láb vásárolta meg.   |
| redirect_uri | szükséges | Az azonos redirect_uri érték szerezheti be a authorization_code használt. |
| client_secret | web Apps alkalmazások szükséges | Az alkalmazás titkos az alkalmazást az app regisztrációs portálon létrehozásához.  Azt nem használandó natív alkalmazásban, mert client_secrets biztos, hogy nem tárolhatók eszközökön.  A web Apps alkalmazások és a webes API-hoz, amelyek a client_secret biztonságos tárolása a kiszolgálóoldalon szükség. |

#### <a name="successful-response"></a>A sikeres válasz
A sikeres jogkivonat visszajelzés fog kinézni:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| access_token | A kért hozzáférési jogkivonat. Az alkalmazás a token segítségével a védett erőforrás, például a webes API hitelesítést végezni. |
| token_type | Jogkivonat típusa értékét jelöli. Az egyetlen típus, amely támogatja az Azure Active Directory Bearer  |
| expires_in | Mennyi ideig a hozzáférési jogkivonat érvénytelen (másodpercben). |
| hatókör | A tartományok, amely a access_token érvényes. |
| refresh_token |  OAuth 2.0-s frissítés token. Az alkalmazás használata a token szerezheti be további hozzáférési jogkivonat, az aktuális jogkivonat lejárta után.  Refresh_tokens élettartamú, és amely megőrzi az erőforrásokhoz hosszabb ideig is használható.  További részleteket tekintse át a [2.0-s verzió jogkivonat-hivatkozást](active-directory-v2-tokens.md).  |
| id_token | Egy alá nem írt JSON webes jogkivonat (JWT). Az alkalmazás lehet base64Url dekódolását bejelentkezve felhasználó kérelem információt a token egyes részeit. Az alkalmazás, a gyorsítótárban értékeket, és megjelenítheti őket, de azt kell támaszkodik őket bármely engedélyezés vagy a biztonsági határai.  Id_tokens kapcsolatban további tudnivalókat talál a [2.0-s verzió végpont jogkivonat-hivatkozást](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Hiba válasz
Hibaüzenetek fog kinézni:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| correlation_id | A kérés diagnosztika keresztül összetevők segítő egyedi azonosítója |

#### <a name="error-codes-for-token-endpoint-errors"></a>Hibakódok jogkivonat végpont hibaellenőrzése

| Kódszámú hiba jelenik meg | Leírás | Ügyfél művelet |
|------------|-------------|---------------|
| invalid_request | Protocol (protokoll) hiba, például a hiányzó szükséges paramétert. | Javítsa ki, és küldje el az összehívást |
| invalid_grant | A engedélyezési kód érvénytelen vagy lejárt. | Próbálja ki az új kérelmének a `/authorize` végpont |
| unauthorized_client | A hitelesített ügyfél nem jogosult engedély megadása típus. | Ez általában fordul elő, ha az ügyfélalkalmazás az Azure Active Directory nem regisztrált, vagy nem adja hozzá a felhasználó Azure AD-bérlő. Az alkalmazás a felhasználótól is, az alkalmazás telepítése és Azure Active Directory felveszi utasításkészlettel. |
| invalid_client | Ügyfél-hitelesítés sikertelen volt. | Az ügyfél hitelesítő adatait, amelyek nem érvényesek. Az alkalmazás rendszergazdájának elhárításához frissíti a hitelesítő adatait. |
| unsupported_grant_type | Az engedély kiszolgáló nem támogatja az engedélyezési támogatás típusát. | Módosítsa a támogatás típusát a kérést. Hibaüzenet a következő típusú mi történjen, csak a fejlesztés alatt, és a kezdeti a tesztelés során az általuk észlelt. |
| invalid_resource | A target erőforrás érvénytelen, mert nem létezik, az Azure Active Directory nem találja, vagy nincs megfelelően beállítva. | Ez azt jelzi, hogy az erőforrás, ha létezik, nincs beállítva a bérlőhöz. Az alkalmazás a felhasználótól is, az alkalmazás telepítése és Azure Active Directory felveszi utasításkészlettel. |
| interaction_required | A kérés felhasználói beavatkozás igényel. Ha például egy további hitelesítési lépés szükség. | Ismételje meg a ugyanaz az erőforrás a kérelmet. |
| temporarily_unavailable | A kiszolgáló átmenetileg nem elfoglalt, és meg szeretné kezelni a kérést. | Ismételje meg a kérelmet. Az ügyfélalkalmazás előfordulhat, hogy magyarázza el, a felhasználónak, hogy a válasz a határidő egy ideiglenes feltétel késik.|

## <a name="use-the-access-token"></a>A hozzáférési jogkivonat használata
Most, hogy sikeresen vásárolta az `access_token`, használhatja a token webes API-összehívásokban meg felvételével a `Authorization` élőfej:

> [AZURE.TIP] A kérés végrehajtása Postman! (Cserélje le a `Authorization` Élőfej első)     [ ![Futtatja a Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>A hozzáférési jogkivonat frissítése
Access_tokens rövid életben volt, és frissítenie kell azokat után továbbra is az erőforrások elérése.  Tehetné küldése egy másik `POST` kérése a `/token` végpontot, ezúttal nyújtó a `refresh_token` helyett a `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Próbálja ki a kérelem végrehajtása a Postman! (Ne felejtse el cseréje a `refresh_token`)     [ ![Futtatja a Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | -------- |
| Bérlői webhelyen | szükséges | A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték `common`, `organizations`, `consumers`, és a bérlői azonosítóját.  További részleteket [protokoll alapjai](active-directory-v2-protocols.md#endpoints)című cikkben. |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| grant_type | szükséges | Meg kell `refresh_token` a kód engedélyezési folyamat a láb. |
| hatókör | szükséges | Keresési tartományok terület tagolt listáját.  A tartományok, ez a szakasz kért egyenértékű vagy a kért az eredeti authorization_code kérelem láb hatókörök részhalmazát kell lennie.  Ha a megadott a kérelem hatókörök több erőforrás-kiszolgálók időtartamát a 2.0-s verzió végpont az erőforráshoz megadott az első hatókört token ad vissza.  Keresési tartományok részletes leírást olvassa el az [engedélyek, felhasználó hozzájárul ahhoz, és hatókörök](active-directory-v2-scopes.md).  |
| refresh_token | szükséges | A refresh_token, amelyek a folyamat, a második láb vásárolta meg.   |
| redirect_uri | szükséges | Az azonos redirect_uri érték szerezheti be a authorization_code használt. |
| client_secret | web Apps alkalmazások szükséges | Az alkalmazás titkos az alkalmazást az app regisztrációs portálon létrehozásához.  Azt nem használandó natív alkalmazásban, mert client_secrets biztos, hogy nem tárolhatók eszközökön.  A web Apps alkalmazások és a webes API-hoz, amelyek a client_secret biztonságos tárolása a kiszolgálóoldalon szükség. |

#### <a name="successful-response"></a>A sikeres válasz
A sikeres jogkivonat választ fog kinézni:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| access_token | A kért hozzáférési jogkivonat. Az alkalmazás a token segítségével a védett erőforrás, például a webes API hitelesítést végezni. |
| token_type | Jogkivonat típusa értékét jelöli. Az egyetlen típus, amely támogatja az Azure Active Directory Bearer  |
| expires_in | Mennyi ideig a hozzáférési jogkivonat érvénytelen (másodpercben). |
| hatókör | A tartományok, amely a access_token érvényes. |
| refresh_token |  Új OAuth 2.0-s frissítés token. A frissítés újonnan megvásárolt token annak érdekében, hogy a frissítés tokenek továbbra is érvényes, amíg a régi frissítés jogkivonat kell cserélni.  |
| id_token | Egy alá nem írt JSON webes jogkivonat (JWT). Az alkalmazás lehet base64Url dekódolását bejelentkezve felhasználó kérelem információt a token egyes részeit. Az alkalmazás, gyorsítótár-értékeket, és megjelenítheti őket, de azt kell támaszkodik őket bármely engedélyezés vagy a biztonsági határai.  Id_tokens kapcsolatban további tudnivalókat talál a [2.0-s verzió végpont jogkivonat-hivatkozást](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Hiba válasz
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

Hibakódok és a javasolt művelet leírását olvassa el [hibakódok jogkivonat végpont hibákat](#error-codes-for-token-endpoint-errors).
