<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Az Azure Active Directory B2C kiadott tokenek típusú."
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>


# <a name="azure-ad-b2c-token-reference"></a>Azure Active Directory B2C: Jogkivonat hivatkozás

Azure Active Directory (Azure Active Directory) B2C biztonsági tokenek különböző típusú bocsát ki, mint feldolgozásával minden [hitelesítési folyamat](active-directory-b2c-apps.md). A dokumentum formázása, biztonsági jellemzők, és minden jogkivonat típusú tartalmának ismerteti.

## <a name="types-of-tokens"></a>Tokenek típusai

Azure Active Directory B2C támogatja a [OAuth 2.0-s engedélyezési Protocol (protokoll)](active-directory-b2c-reference-protocols.md), amely él hozzáférési jogkivonat és a frissítési tokenek. Is támogat hitelesítési és a bejelentkezési [OpenID csatlakozni](active-directory-b2c-reference-protocols.md), amely bemutatja a token egy harmadik típusú keresztül: az azonosító jogkivonathoz. Minden egyes tokenek egy bearer jogkivonathoz ábrázolva jelennek meg.

Egy bearer jogkivonathoz a könnyű biztonsági jogkivonat, amely a "bearer" hozzáférést biztosít a védett erőforrás. A bearer bármely fél, amely a token bemutathatja. Azure Active Directory kell először hitelesítést végezni a féltől előtt megkaphatja a bearer jogkivonathoz. De ha a szükséges lépéseket nem hoznak biztonságossá tétele a token továbbítása, tárterület, azt is hozzá és egy várt fél által használt. Néhány biztonsági tokenek van a beépített kisalkalmazások megakadályozza, hogy a jogosulatlan felek használja őket, de nincs bearer tokenek Ez az eljárás. Azok a biztonságos csatornát, például átviteli réteg biztonsági (HTTPS) kell szállított.

Ha egy bearer jogkivonathoz kívüli biztonságos csatornát, a kártékony féltől segítségével egy férfi középső szándékú szerezheti be a token, és ezzel az illetéktelen eléréséhez védett erőforrás használni. Azonos biztonsági elvek alkalmazni, a rendszer bearer tokenek tárolt vagy gyorsítótárazott későbbi felhasználás céljából. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon bearer tokenek tárolja.

További biztonsági megfontolások a bearer tokenek [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750)című témakör tartalmaz.

Az Azure Active Directory B2C alkotta tokenek számos használják, mint a JSON webes tokenek (JWTs). Egy JWT tömör, URL-cím vannak olyan információk átvitelére két fél között. Követelések neve JWTs tartalmazzák. Ezek a bearer adatai Előfeltételek és a token tárgya. A JWTs követelések JSON olyan objektumok, amelyek kódolt és a továbbítás szerializálásának. Az Azure Active Directory B2C által kibocsátott JWTs jelentkezve, de nem titkosított, mert szeretné hibakeresése azt egy JWT tartalmának egyszerűen vizsgálatára használható. Több eszközök állnak rendelkezésre, amely ehhez, például [calebb.net](http://calebb.net). JWTs kapcsolatos további tudnivalókért tekintse át a [JWT jellemzői](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Azonosító tokenek

Az azonosító token megjeleníthető a biztonsági jogkivonat, hogy az alkalmazás az Azure Active Directory B2C kap `authorize` és `token` végpontok pontra. Azonosító tokenek van ábrázolva [JWTs](#types-of-tokens), és a bennük követelések azonosítja a felhasználókat az alkalmazásban használható. Ha az azonosító tokenek szerezte be a `authorize` végpontot, ezeket gyakran használják webalkalmazások felhasználók bejelentkezni. Ha az azonosító tokenek szerezte be a `token` végpontot, azok lehet küldeni a HTTP-kérések során két összetevői az azonos alkalmazás vagy szolgáltatás közötti kommunikációt. Használhatja a jogcímalapú-azonosító jogkivonat, tetszés szerint. Gyakran használják fiók adatainak megjelenítéséhez vagy access vezérlő döntések-alkalmazásokban.  

Azonosító tokenek be van jelentkezve, de most nincsenek titkosítva. Amikor az alkalmazás vagy API kap egy azonosító jogkivonat, kell [érvényesítése az aláírást](#token-validation) , amellyel igazolhatja, hogy a token hiteles. Az alkalmazás vagy API is ellenőriznie kell a token néhány jogcímalapú szemléltetéséhez, hogy az érvényes. Attól függően, hogy a forgatókönyv követelményeknek az alkalmazás érvényesíteni követelések eltérőek lehetnek, de az alkalmazás végre kell hajtania néhány [Gyakori állítást ellenőrzések](#token-validation) minden esetben.

#### <a name="sample-id-token"></a>Minta azonosító token
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Hozzáférési jogkivonat

Egy hozzáférési jogkivonat található is megjeleníthető a biztonsági jogkivonat, hogy az alkalmazás az Azure Active Directory B2C fogad `authorize` és `token` végpontok pontra. Hozzáférési jogkivonat is ábrázolva [JWTs](#types-of-tokens), és a bennük jogcímeken, amely azonosítja a felhasználókat a web services és az API-khoz is használhatja.

Hozzáférési jogkivonat be van jelentkezve, de még nem adott időben - titkosított és hasonló módon azonosító tokenek.  Hozzáférési jogkivonat használandó web services és az API-hoz való hozzáférést biztosít, és a azonosítása és a szolgáltatások csatlakozó felhasználók hitelesítését.  Azonban nem biztosítanak bármely állítás engedély a azokat a szolgáltatásokat.  Azaz a `scp` hozzáférési jogkivonat lévő igényt nem korlátozza vagy más módon jelenítik meg az access, amely a token tárgya megadták.

[Az aláírás érvényesítése](#token-validation) , amellyel igazolhatja, hogy a token hiteles az API kap egy hozzáférési jogkivonat, kell. Az API-val is ellenőriznie kell a token néhány jogcímalapú szemléltetéséhez, hogy az érvényes. Attól függően, hogy az forgatókönyv követelményeket az alkalmazás érvényesíteni követelések eltérőek lehetnek, de az alkalmazás végre kell hajtania néhány [Gyakori állítást ellenőrzések](#token-validation) minden esetben.

### <a name="claims-in-id--access-tokens"></a>Az azonosító és a hozzáférési jogkivonat követelések

Ha az Azure Active Directory B2C használja, a tokenek tartalomban külön hozzáférése van. Beállíthatja, hogy [házirendek](active-directory-b2c-reference-policies.md) elküldése az alkalmazás igénylő műveleteihez jogcímeken egyes felhasználói adatok csoportját. Ezeket a követelések is elhelyezhet a szabványos tulajdonságait, például a felhasználó `displayName` és `emailAddress`. [Egyéni felhasználói attribútumok](active-directory-b2c-reference-custom-attr.md) B2C címtárában megadhatja azokat is tartalmazhatnak. Minden azonosító és a hozzáférési jogkivonat fogadott biztonsággal kapcsolatos igények részhalmazát fog tartalmazni. Az alkalmazások ezeket követelések segítségével biztonságosan hitelesíteni a felhasználók és kéréseket.

Látható, hogy az azonosító tokenek követelések nem meghatározott sorrendben követik egymást adja vissza. Ezeken kívül új követelések is hozni az azonosító tokenek bármikor. Az alkalmazás nem kell oldaltörés bevezetett új igények szerint. Az alábbiakban az Azure Active Directory B2C által kibocsátott azonosító és a hozzáférési tokenek olyan várt követelések. Bármilyen további követelések házirendek határozza meg. Gyakorlat próbálkozzon a minta azonosító token követelések vizsgálata [calebb.net](http://calebb.net)való beillesztésével. További részletek találhatók az [specifikációja OpenID csatlakozni](http://openid.net/specs/openid-connect-core-1_0.html).

| név | Igénylése | Példa érték | Leírás |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| A célközönség | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | A célközönség állítást azonosítja a címzett, a jogkivonat. Az Azure Active Directory B2C a közönség megegyezik az alkalmazás azonosítója, az alkalmazás az alkalmazás regisztrációs portálon rendelve. Az alkalmazás kell érvényesítése ezt az értéket, és a token elvetése, ha nem egyezik meg. |
| Kibocsátó | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Ezt az állítást azonosítja a biztonsági jogkivonat szolgáltatást (STS), amely szerkezetek és a token adja eredményül. Az Azure Active Directory könyvtár, amelyben a felhasználó hitelesített is azonosítja. Az alkalmazás ellenőrizni kell a kibocsátó állítást annak érdekében, hogy a jogkivonat, a telepítésük az 2.0-s verzió végpontot. |
| A kiadott | `iat` | `1438535543` | Állítását az az idő, amelynél a token bocsátotta, alapidőpont időben jelöli. |
| Lejárati idő | `exp` | `1438539443` | A lejárati idő állítást az az idő, amelynél a token válik érvénytelen, ábrázolt alapidőpont idő. Az alkalmazás állítását segítségével kell az élettartam érvényességének ellenőrzése.  |
| Nem előtt | `nbf` | `1438535543` | Állítását az az idő, amelynél a token lesz érvényes, alapidőpont idő programban. Ez a általában ugyanaz, mint az időt, a token adta-e ki. Az alkalmazás állítását segítségével kell az élettartam érvényességének ellenőrzése.  |
| Verzió | `ver` | `1.0` | Az azonosító jogkivonathoz verzióját az Azure Active Directory szerint meghatározott formátumban. |
| Kód kivonat | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | A kód kivonat szerepel egy azonosító jogkivonat csak elküldésekor a jogkivonat-OAuth 2.0-s engedélyezési kód együtt. A kód kivonat az eredetiséget igazoló engedély-kódot érvényesítéséhez használható. Nézze meg a [Csatlakozás OpenID specifikációja](http://openid.net/specs/openid-connect-core-1_0.html) további információt az ellenőrzés végrehajtásához. |
| Hozzáférési jogkivonat kivonat | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Csak elküldésekor a token együtt egy OAuth 2.0-s hozzáférési jogkivonat-azonosító jogkivonat-hozzáférési jogkivonat kivonat szerepel. A hozzáférési jogkivonat kivonat egy hozzáférési jogkivonat hitelességére érvényesítéséhez használható. Nézze meg a [Csatlakozás OpenID specifikációja](http://openid.net/specs/openid-connect-core-1_0.html) további információt az ellenőrzés végrehajtásához. |
| Nonce | `nonce` | `12345` | Egy nonce jogkivonat ismétléses csökkentésében használt stratégiát is. Az alkalmazás megadhatja a egy nonce engedélyezési felkérés használatával a `nonce` paraméteres lekérdezés. Az adja meg, ha a kérelem érték fog kibocsátott a változatlanul a `nonce` csak egy azonosító jogkivonat formál. Ezzel az alkalmazás azt a kérelmet, amelyet egy adott azonosító token az alkalmazás munkamenet társít a megadott érték alapján értékét megerősítéséhez. Az alkalmazás az azonosító jogkivonat ellenőrzési folyamat során az ellenőrzés kell végezni. |
| Tárgy | `sub` | `Not supported currently. Use oid claim.` | Ez az a rendszerbiztonsági tag arról, hogy melyik a token asserts információkat, például a felhasználót az alkalmazás. Az állandó értéke lehet máshoz és nem használja fel újra. Ellenőrzi engedélyt biztonságosan, például a token használatakor egy erőforrás eléréséhez használható. Jó helyen jár a tárgy állítást még nem használható a az Azure Active Directory B2C. Konfigurálja a házirendek az Objektumazonosító felvenni `oid` igényelheti és értékére segítségével a felhasználók azonosítása helyett használja a tárgy állítást engedély. |
| Hitelesítés helyi osztály hivatkozás | `acr` | `b2c_1_sign_in` | Ez a használt a azonosító jogkivonat szerezheti be a szabály neve.  |
| Hitelesítés közben | `auth_time` | `1438535543` | Állítását a, amikor egy felhasználó utolsó megadott hitelesítő adatait, ábrázolt alapidőpont idő időre szükség. |


### <a name="refresh-tokens"></a>Tokenek frissítése

Frissítés tokenek, amelyek az alkalmazás segítségével szerezheti be az új azonosító tokenek és a hozzáférés-OAuth 2.0-s folyamat tokenek biztonsági tokenek. Az alkalmazás felhasználók nevében erőforrások hosszú távú hozzáféréssel rendelkező, anélkül, hogy ezek a felhasználók interakció biztosítanak.

A frissítés fogadásához jogkivonat, a token választ, az alkalmazás kell kérnie a `offline_acesss` hatókörét. További tudnivalók a `offline_access` hatókör, olvassa el az [Azure Active Directory B2C Protocol (protokoll) hivatkozást](active-directory-b2c-reference-protocols.md).

Frissítse a tokenek vannak, és a program mindig, teljesen átlátszatlanná válik, hogy az alkalmazás. Azure Active Directory által kibocsátott és is ellenőrizni és Azure Active Directory csak értelmezi. Élettartamú, de az alkalmazás nem az elvárásoknak, hogy a frissítés jogkivonat egy konkrét ideig fog tartani a kell írni. Frissítés tokenek okok sokféle bármikor érvénytelenített lehet. Az alkalmazás érvénytelen, ha a frissítés jogkivonat tudnivalók csak úgy, próbálja meg beválthatja azt, így jogkivonat kérelmének Azure AD.

Amikor egy új jogkivonat frissítés token lennem? (, és ha megkapta az alkalmazás a `offline_access` hatókör), a token válasz kap egy új frissítés token. Mentse az újonnan kiállított frissítés jogkivonathoz. A frissítés jogkivonat, a kérésben használt azt váltja fel. Ez segít garantálni, hogy a frissítés tokenek marad, amíg érvényes.

## <a name="token-validation"></a>Jogkivonat érvényesítése

Az érvényesítés jogkivonat, az alkalmazás kell jelölje be az aláírás és a igények bejelentésére a token.

Sok Megnyitás tárak elérhetők érvényesítése JWTs, attól függően, hogy a használni kívánt nyelvet. Azt javasoljuk, hogy a saját érvényességi logikájának megvalósítása helyett inkább ismerje meg azokat a beállításokat. Az adatokat, ebből az útmutatóból segítséget nyújtanak a tárak megfelelően használata.

### <a name="validate-the-signature"></a>Az aláírás ellenőrzése
Egy JWT tartalmaz három szegmensek elválasztva a `.` karaktert. Az első szakasz a **fejléc**, a második pedig a **szervezet**, és a harmadik pedig az **aláírást**. Az aláírás szakaszában, hogy az alkalmazás megbízhatónak tekinthetők meg az eredetiséget igazoló a jogkivonat érvényesítéséhez használható.

Azure Active Directory B2C tokenek bejelentkezve szabványos aszimmetrikus titkosítás algoritmusok, például RSA 256 használatával. A fejlécen a jogkivonat jelentkezzen be a token használt billentyűt, és titkosítási módszer ismerteti:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

A `alg` állítást jelzi a algoritmus, jelentkezzen be a token használt. A `kid` állítást jelzi az adott nyilvános kulcs, jelentkezzen be a token használt.

Az adott időpontban Azure Active Directory előfordulhat, hogy jelentkezzen be a token nyilvános-titkos kulcs párban részhalmazát közül bármelyik használatával. Azure Active Directory rendszeres időközönként, elforgatása kulcsok lehetséges megadása, így kell írni az alkalmazás automatikusan kezelheti a fontosabb változásokról. Egy lehetővé teszi az Azure Active Directory által használt a nyilvános kulcshoz frissítések keresése gyakoriság 24 óránként van.

Azure Active Directory B2C OpenID csatlakozás metaadat-alapú zárólap tartalmaz. Ezzel az alkalmazások-ból az Azure Active Directory B2C futásidőben információt. Ezt az információt a végpontok, jogkivonat tartalma és jogkivonat-aláíró billentyűk tartalmazza. A B2C címtárban minden házirend JSON metaadatok dokumentumot tartalmaz. Például a metaadat-alapú kívánt dokumentumot a `b2c_1_sign_in` házirendet a `fabrikamb2c.onmicrosoft.com` helyen található:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`az B2C könyvtár hitelesíti a felhasználót, és `b2c_1_sign_in` szerezheti be a token használt szabályzat. Határozza meg, milyen házirend használt jelentkezzen be a token (és -ból a metaadatok információforrás), két lehetőség közül választhat. Első lépésként a házirend neve szerepel a `acr` a token lévő igényt. Alap-64 a szervezet dekódolását és deszerializálása a JSON-karakterlánc, amely szerint is elemzi a jogcímeken ki a JWT törzsében. A `acr` állítást lesz a jogkivonat kibocsátása használt a szabály neve.  A többi azt javasoljuk, hogy a házirend értéke kódolását a `state` hiba a kérést, és ezután dekódolását azt határozza meg, milyen házirend használt paraméter. Mindkét módszer érvényes.

A metaadat-dokumentum, amely tartalmazza a számos hasznos információkat vehet egy JSON-objektum. Ezek közé tartozik a végpontok OpenID csatlakozás hitelesítési elvégzéséhez szükséges helyét. Szintén tartalmazzák `jwks_uri`, jelentkezzen be a token helyének megadása és a nyilvános kulcsok ad amely szolgálnak. Hely biztosítjuk, de az célszerű helyét dinamikusan lehívása használatával a metaadat-dokumentumot, és ki elemzése `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

A JSON-dokumentum következő URL-címen található összes egy adott pillanatban használata nyilvános kulcs információkat tartalmazza. Az alkalmazás használata a `kid` formál, jelölje be a nyilvános kulcshoz a JSON-dokumentum, amely egy adott token jelentkezzen be a JWT fejlécében. Kattintson a megfelelő nyilvános kulcs és a megjelölt algoritmus műveleteket hajthat végre aláírás.

Aláírás módjáról leírását a dokumentum hatókörén kívüli van. Sok Megnyitás tárak segít az adatokkal, ha szüksége lenne rá érhetők el.

### <a name="validate-the-claims"></a>A jogcímalapú ellenőrzése
Amikor az alkalmazás vagy API kap egy azonosító jogkivonat, több ellenőrzések szemben is azt kell végeznie a azonosító jogkivonat. Ezek tartalmazzák, de nem korlátozódik:

- A **célközönség** állítást: Ez ellenőrzi, hogy a azonosító jogkivonat célja fordítani az alkalmazást.
- A **nem előtti** és a **lejárati idő** jogcímeken: ezek ellenőrizze, hogy a azonosító jogkivonat nem járt le.
- A **kibocsátó** állítást: Ez ellenőrzi, hogy a token bocsátotta az alkalmazást az Azure Active Directory.
- A **nonce**: Ez az ismétlés jogkivonat homonimaszerű webcímmel kezelési stratégia.

Az alkalmazás végrehajtsa ellenőrzések teljes listáját olvassa el a [Csatlakozás OpenID specifikációja](https://openid.net). A várt értékek, az alábbi követelések részleteit [jogkivonat szakasz](#types-of-tokens)előző szerepelnek.  

## <a name="token-lifetimes"></a>Jogkivonat élettartama

További a Tudásbázis következő jogkivonat élettartamának állnak rendelkezésre. Segíthetnek kidolgozása és alkalmazások hibakeresési. Figyelje meg, hogy az alkalmazás nem írható számíthat az e állandó maradjon élettartama közül. Képesek pedig módosítja.  Erről további kapcsolatban a testre szabott az Azure Active Directory B2C jogkivonat élettartama az [Itt](active-directory-b2c-token-session-sso.md).

| Jogkivonat | Leírási idő | Leírás |
| ----------------------- | ------------------------------- | ------------ |
| Azonosító tokenek | Egy órával | Azonosító tokenek érvényesek általában egy óra. A web App alkalmazásban ez az élettartam segítségével saját munkamenetek karbantartása (ajánlott) felhasználókkal. Egy másik munkamenet élettartamának is választhat. Ha az alkalmazás új azonosító beszerzése jogkivonat, egyszerűen szüksége van, hogy a Azure AD egy új bejelentkezési kérelmet. Ha egy felhasználó rendelkezik egy érvényes böngésző-munkamenetet a Azure Active Directory, a felhasználó nincs szükség újra megadnia a hitelesítő adatait. |
| Tokenek frissítése | Legfeljebb 14 nappal | Egy egyetlen frissítés token 14 nappal legfeljebb érvényes. Azonban a frissítés jogkivonat válhat érvénytelen okok tetszőleges számú tetszőleges időpontban. Az alkalmazás továbbra is próbálja használni a frissítés jogkivonat, amíg a kérelem sikertelen, vagy az alkalmazás a frissítés jogkivonat kicseréli egy újat.  A frissítés jogkivonat is is érvénytelenné válnak Ha 90 napon eltelte, mivel a felhasználó utoljára megadott hitelesítő adatokat. |
| Engedély kódok | Öt perc | Engedély kódokban különbözőnek szándékosan rövid életű. Azokat meg kell beváltott azonnal hozzáférési jogkivonat, a azonosító tokenek vagy a frissítés tokenek beérkezéskor. |
