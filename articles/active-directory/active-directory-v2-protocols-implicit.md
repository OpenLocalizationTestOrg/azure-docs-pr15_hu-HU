<properties
    pageTitle="Azure Active Directory 2.0-s verzió Implicit folyamat |} Microsoft Azure"
    description="Azure Active Directory 2.0-s verzió végrehajtása a implicit folyamat használatának egyoldalas alkalmazások épület webalkalmazások."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>2.0-s verzió protokollok - gyógyfürdőhelyek a implicit folyamat használatával
A 2.0-s verzió végponttal is jelentkezzen be az egyoldalas alkalmazás a Microsoft a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók.  Egyetlen lap és a többi JavaScript-alkalmazás, amely futtatása elsősorban a böngésző arc érdekes néhány lekérdezi, amikor hitelesítés:

- Az alkalmazások biztonsági jellemzői jelentősen eltérnek a hagyományos kiszolgáló-alapú webes alkalmazásokat.
- Sok engedélyezési kiszolgálók és Identitásszolgáltatók nem támogatja a CORS kéréseket.
- Teljes lapos böngésző átirányítja az alkalmazást, különösen invazív, hogy a felhasználói felület válnak hagyta.

Ezeket az alkalmazásokat az (számításba: AngularJS, Ember.js, React.js, stb) Azure Active Directory támogatja a OAuth 2.0-s Implicit támogatási folyamat.  A implicit folyamat az [OAuth 2.0-s specifikációja](http://tools.ietf.org/html/rfc6749#section-4.2)ismertetése.  A fő előnye, hogy engedélyezi-e az alkalmazás lekérése tokenek Azure Active Directory kódmentes kiszolgáló végrehajtása nélkül exchange hitelesítőadat.  Lehetővé teszi a felhasználónak bejelentkezés, kezelése munkamenetet, és az ügyfél belül minden más webes API-khoz tokenek JavaScript-kód beolvasása az alkalmazást.  Létezik néhány fontos biztonsági megfontolások, figyelembe véve a implicit folyamat – kifejezetten az [ügyfél](http://tools.ietf.org/html/rfc6749#section-10.3) és a [felhasználó megszemélyesítési](http://tools.ietf.org/html/rfc6749#section-10.3)körül használata esetén.

Az implicit folyamat és Azure Active Directory authentication felvétele a JavaScript-alkalmazás használatával szeretné, ha azt javasoljuk, a Megnyitás JavaScript tár [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js)használ.  Néhány AngularJS oktatóanyagok áll rendelkezésre [az alábbi](active-directory-appmodel-v2-overview.md#getting-started) segítünk az első lépések.  

Ha inkább nem egy tárba, az egy oldalra alkalmazást használja, és a saját maga Protocol (protokoll) üzeneteket küldeni, kövesse az alábbi általános lépésekkel.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.
    
## <a name="protocol-diagram"></a>Protocol (protokoll) diagram
A teljes implicit bejelentkezési folyamat néz ki – minden egyes lépés részletesen ismertetik.

![OpenId csatlakozás sávok](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>A bejelentkezési kérelem küldése

A felhasználó kezdetben bejelentkezni az appba, hogy [OpenID csatlakozás](active-directory-v2-protocols-oidc.md) engedély-összehívás küldése és get- `id_token` a 2.0-s verzió végpont a:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Kattintson a kérelem végrehajtásához az alábbi hivatkozásra. Bejelentkezés után a böngészőben e irányítva a `https://localhost/myapp/` az egy `id_token` a böngésző címsorába.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| Bérlői webhelyen | szükséges | A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték `common`, `organizations`, `consumers`, és a bérlői azonosítóját.  További részleteket [protokoll alapjai](active-directory-v2-protocols.md#endpoints)című cikkben. |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| response_type | szükséges | Tartalmaznia kell `id_token` az OpenID csatlakozás bejelentkezés.  A response_type is tartalmazhat `token`. Használatával `token` itt lehetővé teszi, hogy a kapott egy hozzáférési jogkivonat közvetlenül az engedélyezés végpontot anélkül, hogy a második kérelem az engedélyezés végpontot, az alkalmazás.  Ha a `token` response_type, a `scope` paraméter egy erőforrás-a-jogkivonat kibocsátása jelző hatókör kell tartalmaznia. |
| redirect_uri | ajánlott | Az alkalmazást, ahol küldött és megkapta az alkalmazás a hitelesítési válaszokkal redirect_uri.  Azt pontosan egyeznie kell a regisztrálta a portálon, kivéve akkor kell lennie az URL-címként kódolandó redirect_uris közül. |
| hatókör | szükséges | Keresési tartományok terület tagolt listáját.  A OpenID csatlakozni, tartalmaznia kell a hatókör `openid`, amely a "Jelentkeztetni" engedéllyel a felhasználói felület hozzájárulása megfelelője.  Tetszés szerint érdemes felvenni a `email` vagy `profile` [hatókörök](active-directory-v2-scopes.md) további felhasználói adatok elérésekor.  Más hatókörök vonatkozó kérő különböző erőforrások jóváhagyását adja a is tartalmazhat.  |
| response_mode | ajánlott | Adja meg a módszert, amellyel az eredményül kapott jogkivonat vissza küldje el az alkalmazást.  Kell `fragment` az implicit továbbításhoz.  |
| állam | ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték.  Lehet, hogy a minden olyan tartalom, a kívánt karakterlánc.  Véletlenszerűen generált egyedi érték [webhelyközi kérelem hamisítása támadások megakadályozása](http://tools.ietf.org/html/rfc6749#section-10.12)általában szolgál.  Az állapot kódolását az alkalmazás a felhasználó állapotával kapcsolatos információk, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha is szolgál. |
| nonce | szükséges | Egy érték szerepel a kérelmet, az alkalmazást, az eredményül kapott id_token igény szerint szerepeltetni hozott létre.  Az alkalmazás majd ellenőrizze, hogy ez az érték jogkivonat ismétléses csökkentésében.  Az érték általában véletlenszerűen, egyedi karakterlánc, amely azonosítja a kérelmet forrását is használható.  |
| figyelmeztető üzenet | nem kötelező. | A szükséges felhasználói beavatkozás típusáról.  Jelenleg csak érvényes értékek "bejelentkezés", "nincs", "beleegyezés és.  `prompt=login`a felhasználó megadnia a hitelesítő adatait, hogy kérésre negating egyszeri bejelentkezés a program lehetséges.  `prompt=none`Ellenkező - biztosítja, hogy a felhasználó nem jelennek a minden interaktív bármilyen kérdés.  A kérés csendes egyszeri bejelentkezés a keresztül nem fejeződhet, ha a 2.0-s verzió végpont hibát jelez.  `prompt=consent`Elindítja a OAuth hozzájárulása párbeszédpanel után a felhasználó bejelentkezik a, a felhasználót a alkalmazásba engedélyek kéri. |
| login_hint | nem kötelező. | Használható előre töltse ki a username/e-mail cím mezőbe, a bejelentkezési lapon a felhasználó számára, ha tudja, hogy a felhasználónév időszakokra.  Gyakran alkalmazásokat fogja használni a paraméter újbóli hitelesítést problémákat már kiolvasott felhasználónév egy előző bejelentkezési használata közben a `preferred_username` formál. |
| domain_hint | nem kötelező. | Egyike lehet `consumers` vagy `organizations`.  Tartalmaz, ha azt kihagyja az e-mailes keresési folyamata a felhasználó halad át a 2.0-s verzió bejelentkezési lapján, amelyek hatékonyabbnak kissé felhasználói élmény.  Gyakran alkalmazásokat fogja használni a paraméter újbóli hitelesítés során kivonja a `tid` a id_token a formál.  Ha a `tid` formál érték `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`.  Egyéb esetben használata `domain_hint=organizations`. |

Ezen a ponton a felhasználót a rendszer kéri a megadnia a hitelesítő adatait, és a hitelesítési befejezéséhez.  A 2.0-s verzió végpont biztosítja azt is, hogy a felhasználó az engedélyeket a megjelölt hozzájárult a `scope` paraméteres lekérdezés.  Ha a felhasználó nem járult hozzá bármilyen ezeket az engedélyeket, a program kéri a felhasználó hozzájárul az ehhez szükséges engedélyekkel.  [Engedélyek, a felhasználó hozzájárul ahhoz, és a több elem bérlői alkalmazások itt megadott](active-directory-v2-scopes.md)részleteit.

Amikor a felhasználó hitelesíti, és adja meg a felhasználó hozzájárul ahhoz, a 2.0-s verzió végpont ad vissza az alkalmazást, a megjelölt adott válasz `redirect_uri`, a megadott módszerrel a `response_mode` paraméter.

#### <a name="successful-response"></a>A sikeres válasz

A sikeres választ használatával `response_mode=fragment` és `response_type=id_token+token` hasonlít a következő olvashatóságát sortöréssel:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| access_token | Ha része `response_type` tartalmaz `token`. Az alkalmazás kéri, ebben az esetben a Microsoft Graph jogkivonat.  A hozzáférési jogkivonat nem kell dekódolva vagy más módon ellenőrzött, átlátszó karakterláncként kezelhető. |
| token_type | Ha része `response_type` tartalmaz `token`.  Mindig legyen `Bearer`. |
| expires_in | Ha része `response_type` tartalmaz `token`.  Azt jelzi, hogy hány másodperc token érvényes, gyorsítótárazás céljából. |
| hatókör | Ha része `response_type` tartalmaz `token`.  Azt jelzi, amelynek a access_token lesz érvényes e keresési tartomány(ok). |
| id_token | A id_token, amely kéri az alkalmazás. Ellenőrizze a felhasználói azonosító, és kezdje el a felhasználó munkamenetet a id_token is használhatja.  Id_tokens és tartalmukat további tájékoztatást a [2.0-s verzió végpont jogkivonat-hivatkozást](active-directory-v2-tokens.md)is tartalmazni fogja.  |
| állam | Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy az állapot értékeket a kérelem és a válasz azonos. |


#### <a name="error-response"></a>Hiba válasz
Hibaüzenetek is küld a `redirect_uri` , az alkalmazás képes kezelni őket megfelelően:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba.  |

## <a name="validate-the-idtoken"></a>A id_token ellenőrzése
Csak a egy id_token fogadása nem elegendő csatlakozó felhasználók hitelesítését; a id_token aláírás érvényesítése kell, és ellenőrizze a token az alkalmazás – követelmények per követelések.  A 2.0-s verzió végpont tokenek jelentkezzen, és győződjön meg róla, hogy azok érvényes [JSON webes tokenek (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) és a nyilvános kulcs titkosítás használja.

Lehetősége van arra, hogy ellenőrizze a `id_token` ügyfél kódot, de általános gyakorlat az, hogy küldése a `id_token` kódmentes kiszolgálón, és hajtsa végre az érvényesítési van.  Miután a id_token aláírása érvényesítése vannak néhány követelések ellenőrzéséhez szükséges.  További információt, például [Érvényesítése tokenek](active-directory-v2-tokens.md#validating-tokens) és [Fontos információk kapcsolatos aláírási kulcs átváltási](active-directory-v2-tokens.md#validating-tokens) [token hivatkozás 2.0-s verzió](active-directory-v2-tokens.md) talál.  Javasoljuk, hogy az elemzés és érvényesítése tár alkalmazó jogkivonatok - nincs közül legalább az egyik elérhető a legtöbb nyelvek és platformokon.
<!--TODO: Improve the information on this-->

Érdemes az esete további követelések érvényesítése is.  Néhány általános ellenőrzések a következők:

- A felhasználói vagy szervezeti biztosítva van regisztrált az alkalmazást.
- A felhasználó biztosítása jogosultságai megfelelő engedélyezés /
- Egy bizonyos erőssége hitelesítési biztosítása történt, például többtényezős hitelesítés.

Az egy id_token követelések kapcsolatos további tudnivalókért olvassa el a a [2.0-s verzió végpont jogkivonat-hivatkozást](active-directory-v2-tokens.md)című témakört.

Miután teljesen érvényesítése a id_token kezdje el a felhasználó munkamenetet, és a id_token követelések segítségével megjelenítheti az alkalmazás a felhasználó adatait.  Ez az információ használható megjelenítési, a rekordok, az engedélyek, a stb.

## <a name="get-access-tokens"></a>Hozzáférési jogkivonat beszerzése

Most, hogy a felhasználó már bejelentkezett a egyoldalas alkalmazás, a hívó webes API-khoz védi az Azure Active Directory, például a [Microsoft Graph](https://graph.microsoft.io)hozzáférési jogkivonat elérheti.  Akkor is, ha már kapott jogkivonat használata a `token` response_type, használhatja ezt a módszert anélkül, hogy a felhasználó, jelentkezzen be újra az átirányítás szerezheti be a token további forrásokra.

A normál OpenID csatlakozás/OAuth folyamat, volna tegye a kérést, így a 2.0-s verzió `/token` végpontot.  Azonban a 2.0-s verzió végpont nem támogatja a CORS kérelmeket, ezért AJAX híváskezdeményezési gyűjtéséhez és frissítése a tokenek ki a kérdés.  Ehelyett a is használhatja a implicit folyamat egy rejtett iframe új tokenek beszerzése más webes API-hoz: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Próbálja ki a másolás és beillesztés a kérelem be egy böngészőlapon alatti! (Ne felejtse el cseréje a `domain_hint` és a `login_hint` értéket a felhasználó esetében a megfelelő értékekkel)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| Bérlői webhelyen | szükséges | A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték `common`, `organizations`, `consumers`, és a bérlői azonosítóját.  További részleteket [protokoll alapjai](active-directory-v2-protocols.md#endpoints)című cikkben. |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| response_type | szükséges | Tartalmaznia kell `id_token` az OpenID csatlakozás bejelentkezés.  Például: más response_types, is tartalmazhat `code`. |
| redirect_uri | ajánlott | Az alkalmazást, ahol küldött és megkapta az alkalmazás a hitelesítési válaszokkal redirect_uri.  Azt pontosan egyeznie kell a regisztrálta a portálon, kivéve akkor kell lennie az URL-címként kódolandó redirect_uris közül. |
| hatókör | szükséges | Keresési tartományok terület tagolt listáját.  Számára beolvasása tokenek az érdeklődésre számot tartó erőforrás van szüksége az összes [hatókörök](active-directory-v2-scopes.md) tartalmazza.  |
| response_mode | ajánlott | Adja meg a módszert, amellyel az eredményül kapott jogkivonat vissza küldje el az alkalmazást.  Egyike lehet `query`, `form_post`, vagy `fragment`.  |
| állam | ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték.  Lehet, hogy a minden olyan tartalom, a kívánt karakterlánc.  Véletlenszerűen generált egyedi érték webhelyközi kérelem hamisítása támadások megakadályozása általában szolgál.  Az állapot kódolását az alkalmazás a felhasználó állapotával kapcsolatos információk, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha is szolgál. |
| nonce | szükséges | Egy érték szerepel a kérelmet, az alkalmazást, az eredményül kapott id_token igény szerint szerepeltetni hozott létre.  Az alkalmazás majd ellenőrizze, hogy ez az érték jogkivonat ismétléses csökkentésében.  Az érték általában véletlenszerűen, egyedi karakterlánc, amely azonosítja a kérelmet forrását is használható.  |
| figyelmeztető üzenet | szükséges | Frissítés & tokenek első az egy rejtett iframe, célszerű használni `prompt=none` meggyőződni arról, hogy az iframe nem lefagy a 2.0-s verzió bejelentkezési lapján, és azonnal adja eredményül. |
| login_hint | szükséges | Frissítése & tokenek első az egy rejtett iframe, szerepelnie kell a felhasználónév annak a felhasználónak a tipp annak érdekében, hogy a felhasználó rendelkezik egy adott pontján időben több munkamenetek megkülönböztetni. A felhasználónév kinyerése egy előző bejelentkezési használata a `preferred_username` formál. |
| domain_hint | szükséges | Egyike lehet `consumers` vagy `organizations`.  A frissítés és a tokenek első az egy rejtett iframe szerepelnie kell a domain_hint a kérést.  Kell kinyerte az `tid` a id_token egy előző bejelentkezés határozza meg, melyik értéket kell használnia: a formál.  Ha a `tid` formál érték `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`.  Egyéb esetben használata `domain_hint=organizations`. |

Köszönet a következőknek a `prompt=none` paraméter, a kérelem fog vagy sikeres vagy sikertelen azonnal, és térjen vissza az alkalmazást.  A sikeres választ küld az alkalmazás, a megjelölt `redirect_uri`, a megadott módszerrel a `response_mode` paraméter.

#### <a name="successful-response"></a>A sikeres válasz
A sikeres választ használatával `response_mode=fragment` hasonlít:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| access_token | Az alkalmazás kért jogkivonata |
| token_type | Mindig legyen `Bearer`. |
| állam | Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy az állapot értékeket a kérelem és a válasz azonos. |
| expires_in | Mennyi ideig a hozzáférési jogkivonat érvénytelen (másodpercben). |
| hatókör | A tartományok, amely a hozzáférési jogkivonat érvényes. |

#### <a name="error-response"></a>Hiba válasz
Hibaüzenetek is küld a `redirect_uri` , az alkalmazás képes kezelni őket megfelelően.  Abban az esetben, `prompt=none`, várható hiba lesz:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba.  |

Ha ez a hibaüzenet jelenik meg az iframe-összehívási ablak, a felhasználónak kell interaktív jelentkezzen be újra egy új token beolvasásához.  Megadhatja, ebben az esetben a valamilyen módon van ilyesmire lehetőség, az alkalmazás kezelheti.

## <a name="refreshing-tokens"></a>Tokenek frissítése

Mindkét `id_token`s és `access_token`s lejár, miután egy rövid ideig, így az alkalmazás frissítéséhez ezek kell készíteni jogkivonatok rendszeres időközönként.  Bármelyik jogkivonat típusú frissítéséhez végezheti el a rejtett iframe kérésben feletti használata a `prompt=none` paraméter Azure AD-viselkedésének szabályozásához.  Ha meg szeretné kapni egy új `id_token`, feltétlenül `response_type=id_token` és `scope=openid`, valamint egy `nonce` paraméter.


## <a name="send-a-sign-out-request"></a>A Kijelentkezés kérelem küldése

A OpenIdConnect `end_session_endpoint` jelenleg nem támogatja az 2.0-s verzió végpontot. Ez azt jelenti, hogy az alkalmazás nem-összehívás küldése egy felhasználó munkamenet befejezése, és törölje a cookie-kat, a 2.0-s verzió végpont beállítása a 2.0-s verzió végpontot.
Jelentkezzen ki a felhasználó, hogy az alkalmazás is egyszerűen saját befejezése a felhasználóval, erre mezőt pedig hagyja a felhasználó-munkamenetet a 2.0-s verzió végpont az-tact.  A következő alkalommal, amikor a felhasználó ideje próbál bejelentkezni, megjelenik számukra a "Válassza ki a fiókot" lap, a saját aktívan aláírt fiókok szerepel a listában.
A lapon a felhasználó választhatja jelentkezzen ki a minden fiók a 2.0-s verzió végponttal a munkamenet befejezése.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->