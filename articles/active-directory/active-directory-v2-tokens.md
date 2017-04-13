<properties
    pageTitle="Azure Active Directory 2.0-s verzió jogkivonat-hivatkozást |} Microsoft Azure"
    description="Milyen típusú tokenek és követelések a 2.0-s verzió végpont által kibocsátott"
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

# <a name="v20-token-reference"></a>jogkivonat-hivatkozást 2.0-s verzió

A 2.0-s verzió végpont biztonsági tokenek feldolgozása minden [hitelesítési folyamat](active-directory-v2-flows.md)során különböző típusú bocsát ki. A dokumentum formázása, biztonsági jellemzők, és minden jogkivonat típusú tartalmának ismerteti.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="types-of-tokens"></a>Tokenek típusai

A 2.0-s verzió végpont támogatja a [OAuth 2.0-s engedélyezési protokoll](active-directory-v2-protocols.md), access_tokens és a refresh_tokens él.  Hitelesítés és a bejelentkezési [OpenID csatlakozni](active-directory-v2-protocols.md#openid-connect-sign-in-flow), amely bemutatja egy harmadik típusú jogkivonat, a id_token keresztül is támogat.  Minden egyes tokenek megfelelője "bearer jogkivonathoz".

Egy bearer jogkivonathoz a könnyű biztonsági jogkivonat, amely a "bearer" hozzáférést biztosít a védett erőforrás. Ebben az értelemben "bearer" bármely fél, amely a token bemutathatja. Bár kell fél Azure AD az bearer jogkivonathoz fogadásához először hitelesíteni, ha a szükséges lépéseket nem vesznek biztonságossá tétele a token továbbítása, tárterület, hozzá, és egy várt fél használják. Néhány biztonsági tokenek van a beépített kisalkalmazások megakadályozza, hogy a jogosulatlan felek használja őket, miközben a bearer tokenek Ez az eljárás nem rendelkezik, és egy biztonságos csatornát, például átviteli réteg biztonsági (HTTPS) kell lennie szállított. Egy bearer jogkivonathoz törlése küldött a rendszer, egy férfi a a középső név kezdőbetűjét homonimaszerű webcímmel használható rosszindulatú fél szerezheti be a token és használni szeretné az illetéktelen hozzáférést egy védett erőforrásra. Azonos biztonsági elvek alkalmazása tárolásáról vagy gyorsítótárazásának bearer tokenek későbbi felhasználás céljából. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon bearer tokenek tárolja. További biztonsági megfontolások a bearer tokenek [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750)című témakör tartalmaz.

A tokenek a 2.0-s verzió végpont által kibocsátott számos Json webes tokenek vagy JWTs végrehajtását.  Egy JWT egy tömör, URL-biztonságos módon információk két fél között.  JWTs tárolt adatok nevezik "követelések" vagy az információ Előfeltételek kapcsolatos bearer és a token tárgyát.  A JWTs követelések objektumai JSON kódolva, és a továbbítás szerializált.  Mivel a JWTs a 2.0-s verzió végpont által kibocsátott jelentkezve, de nem titkosított, egyszerűen megvizsgálhatja a JWT tartalmának hibakeresés céljából. További információt a JWTs a [JWT specifikációja](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)is hivatkozhat.

## <a name="idtokens"></a>Id_tokens

Id_tokens is megjeleníthető a bejelentkezési biztonsági jogkivonat, az alkalmazás [OpenID csatlakozás](active-directory-v2-protocols.md#openid-connect-sign-in-flow)hitelesítéssel végrehajtásakor kapja.  [JWTs](#types-of-tokens)ábrázolva vannak, és követelések, hogy a felhasználónak bejelentkezés az alkalmazásba-et is tartalmaz.  A is használhatja a jogcímalapú egy id_token, tetszés szerint - azok is a gyakran használt fiók adatainak megjelenítéséhez vagy access vezérlő döntések-alkalmazásokban.  A 2.0-s verzió végpont csak hibák egy adott típusú id_token, amely a felhasználót, hogy rendelkezik-e bejelentkezve függetlenül követelések konzisztens mellett.  Ez az, hogy a formátum és a id_tokens tartalmát lesznek a azonos mindkét személyes Microsoft-Account felhasználóknak és a munkahelyi vagy iskolai fiók mondandóját.

Id_tokens jelentkezve, de jelenleg nem titkosítva.  Amikor az alkalmazás-id_token kap, kell [az aláírás ellenőrzése](#validating-tokens) a token hitelesség, és ellenőrizze a token néhány jogcímalapú érvényességi szemléltetéséhez.  Az alkalmazás érvényesíteni követelések forgatókönyv követelmények függően változnak, de néhány [Gyakori állítást ellenőrzések](#validating-tokens) az alkalmazás minden esetben kell végrehajtania.

Teljes részletes tudnivalókat a id_tokens követelések pontjait az alábbiakban, és egy minta id_token.  Látható, hogy a id_tokens követelések nem meghatározott sorrendben követik egymást adja vissza.  Ezeken kívül új jogcímeken lehessen bevinni bármely pontján id_tokens időben – az alkalmazás nem kell oldaltörések, mint bevezetett új jogcímeken.  Az alábbi lista tartalmazza az alkalmazás megbízható értelmezni tud írásakor időben követelések.  Ha szükséges, még részletesebben az [OpenID csatlakozás specifikációja](http://openid.net/specs/openid-connect-core-1_0.html)találhatók.

#### <a name="sample-idtoken"></a>Minta id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Gyakorlat próbálkozzon a minta id_token a követelések vizsgálata [calebb.net](https://calebb.net)való beillesztésével.

#### <a name="claims-in-idtokens"></a>A id_tokens követelések
| név | Igénylése | Példa érték | Leírás |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| A célközönség | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | A címzett, a jogkivonat azonosítja.  A id_tokens a közönség megegyezik az alkalmazás azonosítója, az alkalmazás az alkalmazás regisztrációs portálon rendelve.  Az alkalmazás kell érvényesítése ezt az értéket, és a token elvetése, ha nem egyezik meg. |
| Kibocsátó | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Azonosítja a biztonsági jogkivonat szolgáltatást (STS), amely szerkezetek és a jogkivonat, valamint a Azure AD-bérlő, amelyben a felhasználó hitelesített adja eredményül.  Az alkalmazás ellenőrizni kell a kibocsátó állítást annak érdekében, hogy a jogkivonat, a telepítésük az 2.0-s verzió végpontot.  Azt is érdemes használnia a kérelmet globálisan egyedi azonosítója részét korlátozhatja a bérlők jelentkezhet be az alkalmazás áll rendelkezésre.  A globálisan egyedi azonosítója, amely jelzi a felhasználónak a Microsoft-fiók fogyasztói felhasználó `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| A kiadott | `iat` | `1452285331` | Az idő, amelynél a token bocsátotta, alapidőpont időben jelöli. |
| Lejárati idő | `exp` | `1452289231` | Az idő, amelynél a token válik érvénytelen, alapidőpont időben jelöli.  Az alkalmazás állítását segítségével kell az élettartam érvényességének ellenőrzése.  |
| Nem előtt | `nbf` | `1452285331` |  Az idő, amelynél a token lesz érvényes, alapidőpont időben jelöli. Érdemes általában ugyanaz, mint a kiállítási időt.  Az alkalmazás állítását segítségével kell az élettartam érvényességének ellenőrzése.  |
| Verzió | `ver` | `2.0` | A id_token Azure Active Directory által meghatározott verziója.  Az érték lesz a 2.0-s verzió végpont `2.0`. |
| Bérlői azonosító | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Egy globálisan egyedi azonosítója, amely az Azure Active Directory bérlői, amelyet a felhasználó a.  Munkahelyi és iskolai fiókokat a szervezet, amely a felhasználó tagja megváltoztatható bérlői azonosítója a globálisan egyedi azonosítója lesz.  Személyes fiókok esetén az érték lesz `9188040d-6c67-4c5b-b112-36a304b66dad`.  A `profile` hatókör való állítását fogadása szükséges. |
| Kód kivonat | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | A kód kivonat csak akkor, amikor a id_token kibocsátott mellett egy OAuth 2.0-s engedélyezési kód id_tokens szerepel.  Egy engedélyezési kód hitelességére érvényesítéséhez használható.  További információt a [Csatlakozás OpenID specifikációja](http://openid.net/specs/openid-connect-core-1_0.html) nézze meg az ellenőrzés hajt végre. |
| Hozzáférési jogkivonat kivonat | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | A hozzáférési jogkivonat kivonat csak akkor, amikor a id_token kibocsátott mellett egy OAuth 2.0-s hozzáférési jogkivonat id_tokens szerepel.  Egy hozzáférési jogkivonat hitelességére érvényesítéséhez használható.  További információt a [Csatlakozás OpenID specifikációja](http://openid.net/specs/openid-connect-core-1_0.html) nézze meg az ellenőrzés hajt végre. |
| Nonce | `nonce` | `12345` | A nonce jogkivonat ismétléses enyhítő stratégia.  Az alkalmazás megadhatja a egy nonce engedélyezési felkérés használatával a `nonce` paraméteres lekérdezés.  Az adja meg, ha a kérelem érték fog kibocsátott, a id_token a `nonce` változatlanul állítást.  Ezzel az alkalmazás azt a kérelmet, amelyet egy adott id_token az alkalmazás munkamenet társít a megadott érték alapján értékét megerősítéséhez.  Az alkalmazás végrehajtsa a id_token ellenőrzési folyamat során az ellenőrzés. |
| név | `name` | `Babe Ruth` | A név állítást emberi olvasható érték, amely azonosítja a token tárgya biztosít. Ennek az értéknek egyedinek kell lennie nem garantálja, változtatható, és célja, hogy csak a Megjelenítés célokra használhatók.  A `profile` hatókör való állítását fogadása szükséges. |
| E-mailben | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Az elsődleges e-mail címét, a fiókhoz társított, ha van ilyen.  Az érték változtatható, és az adott felhasználó számára idő után megváltoznak.  A `email` hatókör való állítását fogadása szükséges. |
| Elsődleges felhasználónév | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Az elsődleges felhasználónevét, hogy a felhasználó a 2.0-s verzió végpont megjelenítésére szolgál.  E-mail címre, telefonszámát vagy egy általános felhasználónév meghatározott formázással nem lehet.  Az érték változtatható, és az adott felhasználó számára idő után megváltoznak.  A `profile` hatókör való állítását fogadása szükséges. |
| Tárgy | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | A tőketörlesztés mértékét arról, hogy melyik a token asserts információkat, például a felhasználót az alkalmazás. Ez az érték megváltoztatható, és nem rendelhető vagy újrafelhasználható, így szolgál ellenőrzi engedélyt biztonságosan, például egy erőforrás eléréséhez a token használatakor. A tárgy mindig megtalálható a tokenek az Azure Active Directory problémák, mert ez az érték egy általános célú engedélyezési rendszerben használata ajánlott. |
| Objektumazonosító | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Az objektum azonosítója a Azure AD a rendszer a munkahelyi vagy iskolai fiókjával.  Személyes Microsoft-fiókok nem állítják állítását.  A `profile` hatókör való állítását fogadása szükséges. |


## <a name="access-tokens"></a>Hozzáférési jogkivonat

Csak a Microsoft Services felhasználható ezen a ponton a egyszerre a hozzáférési jogkivonat, a 2.0-s verzió végpont által kibocsátott.  Az alkalmazások nem kell végezze el az érvényesítési vagy a hozzáférési jogkivonat vizsgálata bármely jelenleg támogatott esetet tárgyal.  Akkor is tekinti hozzáférési jogkivonat teljesen átlátszatlanná válik – ezek csak karakterláncok, amelyeket az alkalmazás a Microsoftnak a HTTP-kérések továbbíthatja.

A közeljövőben a 2.0-s verzió végpont fog mutassa be az alkalmazás hozzáférési jogkivonat fogadni más ügyfelek lehetősége.  Adott időpontban ezt az információt frissülnek a az alkalmazás hozzáférési jogkivonat érvényességi és más hasonló feladatok végrehajtásához szükséges információkat.

Egy hozzáférési jogkivonat kér a 2.0-s verzió végpontot, amikor az 2.0-s verzió végpontot, az is a hozzáférési jogkivonat, az alkalmazás fogyasztási néhány metaadatait adja eredményül.  Ezt az információt tartalmaz a hozzáférési jogkivonat, és a hatókörök, amelynek érvénytelen lejárati idejét.  Ez a hozzáférési jogkivonat, magát lehetővé végrehajtásához intelligens gyorsítótárba helyezése hozzáférési jogkivonat anélkül, hogy elemezni, nyissa meg az alkalmazást.

## <a name="refresh-tokens"></a>Tokenek frissítése

Frissítse a tokenek biztonsági tokenek, amely az alkalmazás segítségével szerezheti be az új elérni egy OAuth 2.0-s folyamat tokenek.  Lehetővé teszi a felhasználó nevében erőforrások hosszú távú elérését anélkül, hogy a felhasználó által kapcsolati eléréséhez az alkalmazást.

Frissítés tokenek több erőforrás.  Tehát, hogy egy erőforrás jogkivonat kérés során kapott frissítés token is lehet beváltott hozzáférési jogkivonat, az teljesen más erőforrás.

Annak érdekében, hogy a frissítés jogkivonat választ kap, az alkalmazás kell kérnie, és jogosultsággal a `offline_acesss` hatókörét.   További tudnivalók a `offline_access` hatókör, [Itt a felhasználó hozzájárul ahhoz & hatókörök cikk](active-directory-v2-scopes.md)nyújt útmutatást.

Frissítse a tokenek vannak, és a program mindig, teljesen átlátszatlanná válik, hogy az alkalmazás.  Azok az Azure Active Directory 2.0-s verzió végpontot által kibocsátott és is csak ellenőrzött & értelmezi az 2.0-s verzió végpontot.  Élettartamú, de az alkalmazás nem kell írni számíthat, hogy a frissítés jogkivonat bármely ideig fog tartani.  Frissítés tokenek időpillanatban bármilyen okból sokféle érvénytelenített lehet.  Az alkalmazás érvénytelen, ha a frissítés jogkivonat tudnivalók csak úgy, próbálja meg jogkivonat kérelmet, így a 2.0-s verzió végpont beválthatja azt.

Amikor egy új hozzáférési jogkivonat frissítés token lennem? (és, ha megadták az alkalmazás a `offline_access` hatókör), a token válasz kap egy új frissítés token.  Mentse az újonnan kiállított frissítés jogkivonathoz cseréje a kérelem használtat.  Ez garantálja, hogy a frissítés tokenek marad, amíg érvényes.

## <a name="validating-tokens"></a>Tokenek érvényesítése

Ezen a ponton a egyszerre az alkalmazások kell végrehajtani a csak jogkivonat érvényességi érvényesítése id_tokens.  Annak érdekében, hogy egy id_token érvényesíteni, az alkalmazás ellenőrizni kell a id_token aláírás és a id_token a követelések is.

<!-- TODO: Link -->
Elvégezheti a szükséges tárak és mintakódok hö jogkivonat ellenőrzés – egyszerűen kezelése a alatti információ egyszerűen megadva azoknak, akik az alapul szolgáló folyamatának megértéséhez szeretne.  Program is több 3 fél Megnyitás tárak elérhető JWT adatérvényesítéshez – szinte minden platform & nyelvi ott legalább egy lehetőség van.

#### <a name="validating-the-signature"></a>Az aláírás érvényesítése
Egy JWT vannak elválasztva három szegmensre tartalmazza a `.` karaktert.  Az első szakasz a **fejléc**, a második másként **törzsébe**, és a harmadik az **aláírás**van neve.  Az aláírás szakaszában, hogy az alkalmazás megbízhatónak tekinthetők meg a id_token hitelességére érvényesítéséhez használható.

Id_Tokens bejelentkezve iparágban szabványos aszimmetrikus titkosítást algoritmusok, például RSA 256 használatával. A fejlécen a id_token, jelentkezzen be a token használt billentyűt, és titkosítási módszer ismerteti:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

A `alg` állítást jelzi a algoritmus, jelentkezzen be a token, míg használt a `kid` állítást jelzi az adott nyilvános kulcs, jelentkezzen be a token használt.

Bármely pontján adott időben a 2.0-s verzió végpont előfordulhat, hogy jelentkezzen be egy nyilvános-titkos kulcs párban részhalmazát bármelyikével id_token.  A 2.0-s verzió végpont elforgatása billentyűk rendszeres időközönként, a lehetséges készlete, így kell írni az alkalmazás automatikusan kezelheti a fontosabb változásokról.  Lehetővé teszi gyakoriság, az a nyilvános kulcshoz a 2.0-s verzió végpont által használt frissítések keresése körülbelül 24 óra van.

A csatlakozás OpenID metaadat-dokumentum található használatával aláírás érvényesítéséhez szükséges aláírási kulcs adatok szerezheti be:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Ez az URL a böngészőben próbálja!

A metaadat-dokumentum a JSON-objektumra, amely több hasznos információkat vehet, például a hely, a különböző végpontok OpenID csatlakozás hitelesítése szükséges.  

Is tartalmaz egy `jwks_uri`, amely ad a tokenek aláírásához használt nyilvános kulcs készlete helyét.  A JSON-dokumentum található a `jwks_uri` összes használja-e adott időpillanatban nyilvános kulcs információkat tartalmazza.  Az alkalmazás használata a `kid` formál, jelölje ki, melyik nyilvános kulcs a dokumentumban használt jelentkezzen be az egy adott token JWT fejlécében.  Ezután műveleteket hajthat végre a megfelelő nyilvános kulcs és a megjelölt algoritmus aláírás érvényesítése.

Aláírás elvégzéséhez a dokumentum hatókörén kívüli - sok Megnyitás tárak áll rendelkezésre segít teheti, ha szükséges.

#### <a name="validating-the-claims"></a>A jogcímalapú érvényesítése
Amikor az alkalmazás-felhasználónak bejelentkezés után id_token kap, a id_token meg kell végezni néhány ellenőrzések szemben.  Az ilyen tartalmazzák, de nem:

- A **célközönség** állítást - ellenőrzéséhez, hogy a id_token célja fordítani az alkalmazást.
- A **Nem előtti** és a **Lejárati idő** jogcímalapú - ellenőrizze, hogy a id_token nem járt le.
- A **kibocsátó** állítást - ellenőrzéséhez, hogy a token valóban bocsátotta szeretné az alkalmazást a 2.0-s verzió végpont.
- A **Nonce** - megegyezik egy jogkivonat ismétléses homonimaszerű webcímmel kezelési.
- és így tovább...

Az alkalmazás végrehajtsa állítást ellenőrzések teljes listáját tekintse át [specifikációja OpenID csatlakozni](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Ezek követelések várható értékének részleteit [id_token szakaszban](#id_tokens)szereplő felett.


## <a name="token-lifetimes"></a>Jogkivonat élettartama

A következő jogkivonat élettartama áll rendelkezésre pusztán a ismertetése, mint fejlesztése, az alkalmazások hibakeresési segítik.  Az alkalmazások nem kell írni számíthat az e állandó maradjon élettartama közül – is, és a megadott bármikor idő változik.

| Jogkivonat | Leírási idő | Leírás |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (munkahelyi vagy iskolai fiókokat) | 1 óra értéket | Id_Tokens érvényesek általában egy óra.  A web app használata a azonos élettartama Ez esetben megtartják saját munkamenet a felhasználóval (ajánlott), vagy válasszon egy teljesen más munkamenet élettartam.  Ha az alkalmazás első egy új id_token, egyszerűen szüksége van, hogy az új bejelentkezési kérelmének a 2.0-s verzió engedélyezheti a végpontot.  Ha a felhasználónak van egy érvényes böngésző-munkamenetet a 2.0-s verzió végpontot, hogy nincs szükség újra megadnia a hitelesítő adatait. |
| Id_Tokens (személyes fiókok esetén) | 24 óra | Személyes fiókok Id_Tokens érvényesek a szokásos 24 órát.  A web app használata a azonos élettartama Ez esetben megtartják saját munkamenet a felhasználóval (ajánlott), vagy válasszon egy teljesen más munkamenet élettartam.  Ha az alkalmazás első egy új id_token, egyszerűen szüksége van, hogy az új bejelentkezési kérelmének a 2.0-s verzió engedélyezheti a végpontot.  Ha a felhasználónak van egy érvényes böngésző-munkamenetet a 2.0-s verzió végpontot, hogy nincs szükség újra megadnia a hitelesítő adatait. |
| Hozzáférési jogkivonat (munkahelyi vagy iskolai fiókokat) | 1 óra értéket | Megjelölt jogkivonat válaszokat a token metaadatok részeként. |
| Hozzáférési jogkivonat (személyes fiókok esetén) | 1 óra értéket | Megjelölt jogkivonat válaszokat a token metaadatok részeként.  Személyes fiókok nevében kiadott Access_tokens lehet beállítani egy másik élettartam, de egy órával a helyzet, általában. |
| Frissítse a tokenek (munkahelyi vagy iskolai fiók) | Legfeljebb 14 nappal | Egy egyetlen frissítés token 14 nappal legfeljebb érvényes.  Azonban a frissítés jogkivonat válhat érvénytelen okokból tetszőleges számú tetszőleges időpontban, az alkalmazás továbbra is próbálja és a frissítési jogkivonat mindaddig, amíg meg nem sikerül vagy mindaddig, amíg az alkalmazás kicseréli egy új frissítés token.  A frissítés jogkivonat is érvényét veszti, ha 90 napon vált, mivel a felhasználó beírta a hitelesítő adatait. |
| Frissítse a tokenek (személyes fiókok esetén) | Legfeljebb 1 év | Egy egyetlen frissítés token legfeljebb 1 év érvényes.  Azonban a frissítés jogkivonat válhat érvénytelen okokból tetszőleges számú tetszőleges időpontban, az alkalmazás továbbra is próbálja ki és használja a frissítés jogkivonat, mindaddig, amíg meg nem sikerült. |
| Engedély kódok (munkahelyi vagy iskolai fiókokat) | 10 perc | Engedély kódok szándékosan rövid életű, és kell azonnal váltható access_tokens és refresh_tokens beérkezéskor. |
| Engedély kódok (személyes fiókok esetén) | 5 percig tart | Engedély kódok szándékosan rövid életű, és meg kell azonnal beváltott access_tokens és refresh_tokens beérkezéskor.  Személyes fiókok nevében kiállított engedélyezési kódok is egyszeri használata. |
