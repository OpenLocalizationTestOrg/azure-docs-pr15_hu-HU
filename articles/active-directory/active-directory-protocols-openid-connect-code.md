<properties
    pageTitle="Azure Active Directory .NET protokoll – áttekintés |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan webalkalmazások és a webes API-hoz a bérlői webhelyén, az Azure Active Directory és a csatlakozás OpenID való hozzáférés engedélyezése a HTTP-üzenetek használatával."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Webalkalmazás csatlakoztatása OpenID és Azure Active Directory számára a hozzáférés engedélyezése

Az OAuth 2.0-s protokoll épülő egyszerű identitás réteg [OpenID csatlakozás](http://openid.net/specs/openid-connect-core-1_0.html) . OAuth 2.0-s beszerzése és **hozzáférési jogkivonat** segítségével védett forrásokat mechanizmusok határozza meg, de nem határozza meg személyes adatokat normál módszereket. Csatlakozás OpenID hajtja végre hitelesítési kiterjesztése a OAuth 2.0-s engedélyezési folyamat, és információt nyújt a végfelhasználó formájában egy `id_token` , hogy a felhasználó ellenőrzi, valamint a felhasználó egyszerű profil információt tartalmaz.

Az ajánlási OpenID csatlakozás akkor, ha a kiszolgálón található, és a böngészőben elérhető webalkalmazás készítésekor.

## <a name="authentication-flow-using-openid-connect"></a>Hitelesítési folyamat OpenID összekapcsolás funkcióval

A legalapvetőbb bejelentkezési folyamat az alábbi lépéseket tartalmazza – mindegyiket leírása részletesen.

![OpenId csatlakozás hitelesítési folyamat](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>A bejelentkezési kérelem küldése

A webes alkalmazás kell csatlakozó felhasználók hitelesítését, ha meg kell irányítsa át a felhasználót, hogy a `/authorize` végpontot. A kérés hasonlít a első láb, az [OAuth 2.0-s engedélyezési kód Flow](active-directory-protocols-oauth-code.md), néhány fontos különbség:

- A kérés tartalmaznia kell a hatókör `openid` a a `scope` paraméter.
- A `response_type` tartalmaznia kell a paraméter `id_token`.
- A kérés tartalmaznia kell a `nonce` paraméter.

Ezt a minta kérelem így néz ki:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| Bérlői webhelyen | szükséges | A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték bérlői azonosítók, például `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` vagy `common` a bérlői beállítástól független tokenek |
| client_id | szükséges | Az alkalmazás azonosítója rendelt az alkalmazást, az Azure Active Directory regisztrálásakor. Az Azure klasszikus portálon megtalálhatja. Kattintson **Az Active**Directory kattintson a címtár, kattintson az alkalmazásra, kattintson a **Configure** |
| response_type | szükséges | Tartalmaznia kell `id_token` az OpenID csatlakozás bejelentkezés.  Például: más response_types, is tartalmazhat `code`. |
| hatókör | szükséges | Keresési tartományok terület tagolt listáját.  A OpenID csatlakozni, tartalmaznia kell a hatókör `openid`, amely a "Jelentkeztetni" engedéllyel a felhasználói felület hozzájárulása megfelelője.  Más hatókörök vonatkozó kérő felhasználó hozzájárul ahhoz a is tartalmazhat. |
| nonce | szükséges | A kérelmet, az alkalmazást, az eredményül kapott szereplő által generált szereplő érték `id_token` igény szerint.  Az alkalmazás majd ellenőrizze, hogy ez az érték jogkivonat ismétléses csökkentésében.  Az érték általában véletlenszerűen, egyedi karakterlánc vagy globálisan egyedi azonosítója, a kérelem forrását azonosítása használható.  |
| redirect_uri | ajánlott | Az alkalmazást, ahol küldött és megkapta az alkalmazás a hitelesítési válaszok redirect_uri.  Azt pontosan egyeznie kell a regisztrálta a portálon, kivéve akkor kell lennie az URL-címként kódolandó redirect_uris közül. |
| response_mode | ajánlott | Adja meg a módszert, amellyel az eredményül kapott authorization_code küldenek vissza az alkalmazást.  Támogatott érték `form_post` a *HTTP űrlap közzé* vagy `fragment` az *URL-cím fragment*.  Webalkalmazások használata ajánlott `response_mode=form_post` ahhoz, hogy az alkalmazás a legbiztonságosabb továbbított tokenek.  
| állam | ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték.  Lehet, hogy a minden olyan tartalom, a kívánt karakterlánc.  Véletlenszerűen generált egyedi érték [webhelyközi kérelem hamisítása támadások letiltom](http://tools.ietf.org/html/rfc6749#section-10.12)általában szolgál.  Az állapot kódolását az alkalmazás a felhasználó állapotával kapcsolatos információk, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha is szolgál. |
| figyelmeztető üzenet | nem kötelező. | A szükséges felhasználói beavatkozás típusáról.  Jelenleg csak érvényes értékek "bejelentkezés", "nincs", "beleegyezés és.  `prompt=login`a felhasználó megadnia a hitelesítő adatait, hogy kérésre negating egyszeri bejelentkezés a program lehetséges.  `prompt=none`Ellenkező - biztosítja, hogy a felhasználó nem jelennek a minden interaktív bármilyen kérdés.  A kérés csendes egyszeri bejelentkezés a keresztül nem fejeződhet, ha a végpontok hibát jelez.  `prompt=consent`Elindítja a OAuth hozzájárulása párbeszédpanel után a felhasználó bejelentkezik a, a felhasználót a alkalmazásba engedélyek kéri. |
| login_hint | nem kötelező. | Használható előre töltse ki a username/e-mail cím mezőbe, a bejelentkezési lapon a felhasználó számára, ha tudja, hogy a felhasználónév időszakokra.  Gyakran alkalmazásokat fogja használni a paraméter újbóli hitelesítést problémákat már kiolvasott felhasználónév egy előző bejelentkezési használata közben a `preferred_username` formál. |


Ezen a ponton a felhasználót a rendszer kéri a megadnia a hitelesítő adatait, és a hitelesítési befejezéséhez.

### <a name="sample-response"></a>Minta válasz

Minta választ, a felhasználót hitelesítette, miután ábrája ilyen:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| id_token | A `id_token` , amely kéri az alkalmazás. Használhatja a `id_token` ellenőrizze a felhasználói azonosító, és kezdje el a felhasználó munkamenetet.  |
| állam | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték. Véletlenszerűen generált egyedi érték [webhelyközi kérelem hamisítása támadások letiltom](http://tools.ietf.org/html/rfc6749#section-10.12)általában szolgál.  Az állapot kódolását az alkalmazás a felhasználó állapotával kapcsolatos információk, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha is szolgál. |

### <a name="error-response"></a>Hiba válasz
Hibaüzenetek is küld a `redirect_uri` , az alkalmazás képes kezelni őket megfelelően:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
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
| invalid_resource |A cél erőforrás érvénytelen, mert nem létezik, az Azure Active Directory nem találja, vagy nincs megfelelően beállítva.| Ez azt jelzi, hogy az erőforrás, ha létezik, nincs beállítva a bérlőhöz. Az alkalmazás a felhasználótól is, az alkalmazás telepítése és Azure Active Directory felveszi utasításkészlettel. |

## <a name="validate-the-idtoken"></a>A id_token ellenőrzése

Fogadása csak egy `id_token` nem elegendő csatlakozó felhasználók hitelesítését; kell az aláírás ellenőrzése és a követelések ellenőrizze a `id_token` per az alkalmazás – követelmények. Az Azure Active Directory-végpontot tokenek jelentkezzen, és győződjön meg róla, hogy azok érvényes JSON webes tokenek (JWTs) és a nyilvános kulcs titkosítás használja.

Lehetősége van arra, hogy ellenőrizze a `id_token` ügyfél kódot, de általános gyakorlat az, hogy küldése a `id_token` kódmentes kiszolgálón, és hajtsa végre az érvényesítési van. Miután, amelyeket érvényesített aláírását a `id_token`, létezik néhány követelések ellenőrzéséhez szükséges.

Érdemes az esete további követelések érvényesítése is. Néhány általános ellenőrzések a következők:

- A felhasználói vagy szervezeti biztosítva van regisztrált az alkalmazást.
- A felhasználó biztosítása jogosultságai megfelelő engedélyezés /
- Egy bizonyos erőssége hitelesítési biztosítása történt, például többtényezős hitelesítés.

Miután van teljesen érvényesíteni a `id_token`, kezdje el a felhasználó munkamenetet, és használja a követelések a `id_token` az alkalmazás a felhasználó információt szeretne kapni. Ez az információ használható megjelenítési, a rekordok, az engedélyek, a stb. A típusok jogkivonat és követelések kapcsolatos további tudnivalókért olvassa el [jogkivonat támogatott és a felelős típusok](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>A Kijelentkezés kérelem küldése

Ha szeretne jelentkezzen ki az alkalmazás a felhasználó, akkor sem elegendő vagy törölje a jelet az alkalmazás cookie-kat, különben a program befejezése a felhasználó a munkamenetet.  Is átirányítást kell beállítania a felhasználót, hogy a `end_session_endpoint` a Kijelentkezés parancsra.  Ha nem sikerül kéri, a felhasználó tudják újból hitelesíteni a alkalmazásba adataikat újra, beírása nélkül, mert egy érvényes bejelentkezéses munkamenetben az Azure Active Directory végponttal rendelkeznek.

Egyszerűen irányíthatja át a felhasználót, hogy a `end_session_endpoint` szerepel a metaadat-alapú OpenID csatlakozás dokumentumot:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | ajánlott | Az URL-címet, amely a felhasználó e irányítva a után sikeres jelentkezzen ki.  Ha nem szerepel, a felhasználó megjelenik egy általános üzenetet.  |

## <a name="token-acquisition"></a>Jogkivonat megszerzése

Sok web Apps alkalmazások nem csak a felhasználó, de a is elérheti, hogy a felhasználó OAuth használatával nevében webszolgáltatás kell. Ebben az esetben összefűzi OpenID csatlakozás egyidejű beolvasása közben a felhasználói hitelesítés egy `authorization_code` megszerezni, amely használható `access_tokens` az OAuth engedélyezési kód Flow használatával.

## <a name="get-access-tokens"></a>Hozzáférési jogkivonat beszerzése

Szerezheti be a hozzáférési jogkivonat, kell a bejelentkezési kérelem a fentiektől módosítása:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Jogosultsági hatókörök elhelyezése a kérést, és `response_type=code+id_token`, a `authorize` végpont biztosítja, hogy a felhasználó az engedélyeket a megjelölt hozzájárult a `scope` paraméteres lekérdezést, és lépjen vissza az alkalmazás-engedélyezési kód az exchange-hozzáférési jogkivonat.

### <a name="successful-response"></a>A sikeres válasz

A sikeres választ használatával `response_mode=form_post` hasonlít:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| id_token | A `id_token` , amely kéri az alkalmazás. Használhatja a `id_token` ellenőrizze a felhasználói azonosító, és kezdje el a felhasználó munkamenetet. |
| kód | A authorization_code, amely kéri az alkalmazás. Az alkalmazás segítségével az engedélyezési kód kérése a cél erőforrás-jogkivonat. Authorization_codes nagyon rövid élettartamú, általában körülbelül 10 perc múlva járnak. |
| állam | Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy az állapot értékeket a kérelem és a válasz azonos. |

### <a name="error-response"></a>Hiba válasz

Hibaüzenetek is küld a `redirect_uri` , az alkalmazás képes kezelni őket megfelelően:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba.  |

A lehetséges hibakódok és a javasolt művelet leírását olvassa el [hibakódok engedélyezési végpont hibákat](#error-codes-for-authorization-endpoint-errors).

Miután, amelyeket néhányan engedély `code` és egy `id_token`, a felhasználónak bejelentkezés, és kérjen hozzáférési jogkivonat más nevében.  Ellenőriznie kell bejelentkezni, hogy a felhasználó, a `id_token` pontosan a fent leírt módon. Úgy juthat az hozzáférési jogkivonat, követheti a [OAuth protokoll dokumentációjában](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token)leírt lépésekkel.
