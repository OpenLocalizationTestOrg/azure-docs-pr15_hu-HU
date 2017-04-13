 <properties
   pageTitle="Azure Active Directory jogkivonat hivatkozás |} Microsoft Azure"
   description="Útmutató ismertetése, és a SAML 2.0-s és JSON webes tokenek (JWT) tokenek kiadott által Azure Active Directory (AAD) a követelések értékelése"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure Active Directory jogkivonat hivatkozás

Azure Active Directory (Azure Active Directory) biztonsági tokenek feldolgozása minden hitelesítési folyamat során különböző típusú bocsát ki. A dokumentum formázása, biztonsági jellemzők, és minden jogkivonat típusú tartalmának ismerteti.

## <a name="types-of-tokens"></a>Tokenek típusai

Azure Active Directory támogatja a [OAuth 2.0-s engedélyezési Protocol (protokoll)](active-directory-protocols-oauth-code.md), amely él access_tokens és refresh_tokens is.  Hitelesítés és a bejelentkezési [OpenID csatlakozni](active-directory-protocols-openid-connect-code.md), amely bemutatja egy harmadik típusú jogkivonat, a id_token keresztül is támogat.  Minden egyes tokenek megfelelője "bearer jogkivonathoz".

Egy bearer jogkivonathoz a könnyű biztonsági jogkivonat, amely a "bearer" hozzáférést biztosít a védett erőforrás. Ebben az értelemben "bearer" bármely fél, amely a token bemutathatja. Bár az Azure Active Directory authentication való kap egy bearer jogkivonathoz szükséges, a jogkivonat, hogy egy várt fél hozzáférés biztonságossá tétele lépéseket kell tenni. Bearer tokenek nem rendelkezik beépített mechanizmusa, ezzel az illetéktelen felek megakadályozásához használja őket, mert azok a biztonságos csatornát, például átviteli réteg biztonsági (HTTPS) kell szállított. Egy bearer jogkivonathoz továbbították a törlése, ha egy férfi a a középső név kezdőbetűjét homonimaszerű webcímmel használható szerezheti be a token és védett erőforrásként illetéktelen hozzáférhetnek. Azonos biztonsági elvek alkalmazása tárolásáról vagy gyorsítótárazásának bearer tokenek későbbi felhasználás céljából. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon bearer tokenek tárolja. További biztonsági megfontolások a bearer tokenek [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750)című témakör tartalmaz.

Az Azure Active Directory által kibocsátott tokenek számos JSON webes tokenek vagy JWTs végrehajtását.  Egy JWT egy tömör, URL-biztonságos módon információk két fél között.  JWTs tárolt adatok nevezik "követelések" vagy az információ Előfeltételek kapcsolatos bearer és a token tárgyát.  A JWTs követelések objektumai JSON kódolva, és a továbbítás szerializált.  Mivel az Azure Active Directory által kibocsátott JWTs jelentkezve, de nem titkosított, egyszerűen megvizsgálhatja a JWT tartalmának hibakeresés céljából.  Nincsenek érhető el ehhez a művelethez, például [jwt.calebb.net](http://jwt.calebb.net)számos eszközt. További információt a JWTs a [JWT specifikációja](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)is hivatkozhat.

## <a name="idtokens"></a>Id_tokens

Id_tokens is megjeleníthető a bejelentkezési biztonsági jogkivonat, az alkalmazás [OpenID csatlakozás](active-directory-protocols-openid-connect-code.md)hitelesítéssel végrehajtásakor kapja.  [JWTs](#types-of-tokens)ábrázolva vannak, és követelések, hogy a felhasználónak bejelentkezés az alkalmazásba-et is tartalmaz.  A is használhatja a jogcímalapú egy id_token, tetszés szerint - azok is a gyakran használt fiók adatainak megjelenítéséhez vagy access vezérlő döntések-alkalmazásokban.

Id_tokens jelentkezve, de jelenleg nem titkosítva.  Amikor az alkalmazás-id_token kap, kell [az aláírás ellenőrzése](#validating-tokens) a token hitelesség, és ellenőrizze a token néhány jogcímalapú érvényességi szemléltetéséhez.  Az alkalmazás érvényesíteni követelések forgatókönyv követelmények függően változnak, de néhány [Gyakori állítást ellenőrzések](#validating-tokens) az alkalmazás minden esetben kell végrehajtania.

A következő részben információt id_tokens követelések, valamint egy minta id_token.  Látható, hogy a id_tokens követelések nem meghatározott sorrendben követik egymást adja vissza.  Ezeken kívül új jogcímeken lehessen bevinni bármely pontján id_tokens időben – az alkalmazás nem kell oldaltörések, mint bevezetett új jogcímeken.  Az alábbi lista tartalmazza az alkalmazás megbízható értelmezni tud írásakor időben követelések.  Ha szükséges, még részletesebben az [OpenID csatlakozás specifikációja](http://openid.net/specs/openid-connect-core-1_0.html)találhatók.

#### <a name="sample-idtoken"></a>Minta id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Gyakorlat próbálkozzon a minta id_token a követelések vizsgálata [calebb.net](http://jwt.calebb.net)való beillesztésével.

#### <a name="claims-in-idtokens"></a>A id_tokens követelések

| JWT igénylése | név | Leírás |
|-----------|------|-------------|
| `appid`| Azonosítója | Az alkalmazás a token egy erőforráshoz való hozzáféréshez használt azonosítja. Az alkalmazás működhetnek magát vagy a felhasználó nevében. Az alkalmazás azonosítója általában jelöl alkalmazás objektum, de az Azure Active Directory is jelenthet egy egyszerű-objektum. <br><br> **Példa JWT érték**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| A célközönség | A címzetthez a jogkivonat. Az alkalmazás, amely a token kapja kell ellenőrizze, hogy a közönség érték helyes-e bármilyen egy másik közönségnek szánt tokenek elvetése. <br><br> **Példa SAML érték**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Példa JWT érték**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Alkalmazás hitelesítési helyi osztály hivatkozás | Azt jelzi, hogy az ügyfél hitelesítésének módját. Egy nyilvános ügyfél értéke 0. Ügyfél-azonosító, és az ügyfél titkos használja, ha az érték 1. <br><br> **Példa JWT érték**: <br> `"appidacr": "0"`|
| `acr`| Hitelesítés helyi osztály hivatkozás | Azt jelzi, hogy hogyan a tárgy hitelesített, nem pedig az ügyfél az alkalmazás hitelesítési helyi osztály hivatkozás állítást. "0" érték azt jelzi, hogy a végfelhasználói hitelesítés nem felel meg az ISO/IEC 29115 követelményeinek. <br><br> **Példa JWT érték**: <br> `"acr": "0"`|
| | Azonnali hitelesítés | A dátum és idő mikor történt a hitelesítési rekordok. <br><br> **Példa SAML érték**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Hitelesítési módszer | Azonosítja a token tárgya hitelesítésének módját. <br><br> **Példa SAML érték**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Példa JWT érték**:`“amr”: ["pwd"]` |
| `given_name`| Utónév | Az első biztosít vagy a "" nevet kapnak a felhasználó az Azure Active Directory user objektumban vannak megadva. <br><br> **Példa SAML érték**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Példa JWT érték**: <br> `"given_name": "Frank"` |
| `groups`| Csoportok | A tárgy csoporttagság megjelenítő objektum azonosítók biztosít. Ezek az értékek egyedi (lásd: Objektumazonosító) és az access, például egy erőforrás hozzáférés engedélyezése kényszerítése kezelésére szolgáló biztonságosan használható. A csoportok, a csoportok állítást szereplő alkalmazásonként alapon – az alkalmazás jegyzék "groupMembershipClaims" tulajdonsága megtörténik. Null érték nem veszi figyelembe az összes csoport, csak az Active Directory-biztonsági csoport tagsági és egy értéket a "Minden" fog tartalmazzák a biztonsági csoportokat és az Office 365 terjesztési listák "SecurityGroup" értéket tartalmazza. <br><br> **Példa SAML érték**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Példa JWT érték**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Identitásszolgáltató | Rekordok az identitásszolgáltató, és amelyek hitelesítése a token tárgyát. Ez az érték megegyezik az kibocsátó állítást értékét kivéve, ha a felhasználói fiók egy másik bérlői webhelyen, mint a kibocsátó. <br><br> **Példa SAML érték**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Példa JWT érték**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Az idő, amelynél a token kiadásának tárolja. Mérje le a token frissessége gyakran használatos. <br><br> **Példa SAML érték**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Példa JWT érték**: <br> `"iat": 1390234181` |
| `iss` | Kibocsátó | A biztonsági jogkivonat-szolgáltatás szerkezetek, és a token visszaadó azonosítja. A tokenek Azure AD eredményül adó a kibocsátó sts.windows.net. A globálisan egyedi azonosítója, a kibocsátó állítást értéket az Azure Active directory bérlői azonosítója. A bérlői Azonosítóra a könyvtár megváltoztatható és megbízható azonosítót. <br><br> **Példa SAML érték**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Példa JWT érték**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Vezetéknév | Az utolsó nevet, Vezetéknév vagy a felhasználó nevét család nyújt az Azure Active Directory user objektumban definiált. <br><br> **Példa SAML érték**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Példa JWT érték**: <br> `"family_name": "Miller"` |
| `unique_name`| név | Itt emberi olvasható érték, amely azonosítja a token tárgyát. Az érték nem biztos, a bérlői belül egyedinek kell lennie, és célja, hogy csak a Megjelenítés célokra használhatók. <br><br> **Példa SAML érték**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Példa JWT érték**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Objektum azonosítója | Azure AD egy objektum egy egyedi azonosítót tartalmaz. Az állandó értéke lehet máshoz és nem használja fel újra. Használja a Objektumazonosító az Azure Active Directory-lekérdezésekben objektum azonosításához. <br><br> **Példa SAML érték**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Példa JWT érték**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Szerepkörök | Az összes alkalmazás szerepkörökkel a tárgy megadták közvetlenül és közvetve csoporttagság és szerepköralapú hozzáférés-vezérlés hivatkozási használható jelöli. Alkalmazás szerepkörök definiált alkalmazásonként alapon keresztül a `appRoles` az alkalmazás jegyzék tulajdonsága. A `value` minden alkalmazás szerepkör tulajdonság értéke, amely a szerepkörök kárigény jelenik meg. <br><br> **Példa SAML érték**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Példa JWT érték**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Hatókör | Azt jelzi, hogy a ügyfélalkalmazás megszemélyesítési engedélyeket. Az alapértelmezett engedély `user_impersonation`. A védett erőforrás tulajdonosa Azure Active Directory regisztrálhatja további értékeket. <br><br> **Példa JWT érték**: <br> `"scp": "user_impersonation"`|
| `sub` |Tárgy| A tőketörlesztés mértékét arról, hogy melyik a token asserts információkat, például az alkalmazás a felhasználó azonosítja. Ez az érték megváltoztatható, és nem rendelhető, vagy újrafelhasználható, így szolgál biztonságosan engedélyezési ellenőrzések végrehajtásához. A tárgy mindig megtalálható a tokenek az Azure Active Directory problémák, mert ez az érték egy általános célú engedélyezési rendszerben használata ajánlott. <br> `SubjectConfirmation`igény esetén nem. Azt ismerteti, hogyan a token tárgya ellenőrzött lesz. `Bearer`azt jelzi, hogy a tárgy megerősíti a jogkivonat birtokában. <br><br> **Példa SAML érték**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Példa JWT érték**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Bérlői azonosító | Megváltoztatható, zárolási azonosítót, amely azonosítja a címtár bérlő, amely a token ki. Ez az érték segítségével bérlői-specifikus címtár forrásokat több bérlői alkalmazásban. Például ez az érték segítségével azonosítani a bérlő, a diagram API hívás. <br><br> **Példa SAML érték**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Példa JWT érték**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Élettartam | Megadja az időszakot, amelyeken belül a token érvényes. A szolgáltatás, amely ellenőrzi a token kell bizonyosodjon meg arról, hogy az aktuális dátumot az élettartam, egyéb meg kell utasítania a token belül. A szolgáltatás lehetővé teheti a token élettartamának tartományon kívül legfeljebb öt percig fiókhoz történő bármely órája ("idő ferdeség") közötti különbségek közötti Azure AD és a szolgáltatást. <br><br> **Példa SAML érték**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Példa JWT érték**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Egyszerű felhasználónév | A felhasználó egyszerű felhasználóneve tárolja.<br><br> **Példa JWT érték**: <br> `"upn": frankm@contoso.com`|
| `ver`| Verzió | A token verziószámának tárolja. <br><br> **Példa JWT érték**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Hozzáférési jogkivonat

Csak a Microsoft Services felhasználható ezen a ponton a egyszerre a hozzáférési jogkivonat.  Az alkalmazások nem kell végezze el az érvényesítési vagy a hozzáférési jogkivonat vizsgálata bármely jelenleg támogatott esetet tárgyal.  Akkor is tekinti hozzáférési jogkivonat teljesen átlátszatlanná válik – ezek csak karakterláncok, amelyeket az alkalmazás a Microsoftnak a HTTP-kérések továbbíthatja.

Egy hozzáférési jogkivonat kérésekor Azure Active Directory, az is a hozzáférési jogkivonat, az alkalmazás fogyasztási néhány metaadatait adja eredményül.  Ezt az információt tartalmaz a hozzáférési jogkivonat, és a hatókörök, amelynek érvénytelen lejárati idejét.  Ez a hozzáférési jogkivonat, magát lehetővé végrehajtásához intelligens gyorsítótárba helyezése hozzáférési jogkivonat anélkül, hogy elemezni, nyissa meg az alkalmazást.

## <a name="refresh-tokens"></a>Tokenek frissítése

Frissítés tokenek biztonsági tokenek, amely az alkalmazás segítségével szerezheti be az új hozzáférési jogkivonat-OAuth 2.0-s folyamat.  Lehetővé teszi a felhasználó nevében erőforrások hosszú távú elérését anélkül, hogy a felhasználó által kapcsolati eléréséhez az alkalmazást.

Frissítés tokenek a többszörös erőforrás, ami azt jelenti, előfordulhat, hogy kell egy erőforrás jogkivonat kérés során kapott, hanem az teljesen más erőforrás hozzáférési jogkivonat beváltott. Több elem erőforrás megadásához állítsa a `resource` az értekezlet-összehívást a célként megadott erőforrás paramétert.

Frissítés tokenek teljesen átlátszatlanná válik, hogy az alkalmazás. Élettartamú, de az alkalmazás nem kell írni számíthat, hogy a frissítés jogkivonat bármely ideig fog tartani.  Frissítés tokenek időpillanatban bármilyen okból sokféle érvénytelenített lehet.  Az alkalmazás érvénytelen, ha a frissítés jogkivonat tudnivalók csak úgy, próbálja meg egy jogkivonat kérelmet jogkivonat Azure AD-végpontot, így beválthatja azt.

De veheti egy frissítés token egy új jogkivonat, amikor egy új frissítés token jogkivonat válasz kap.  Mentse az újonnan kiállított frissítés jogkivonathoz cseréje a kérelem használtat.  Ez garantálja, hogy a frissítés tokenek marad, amíg érvényes.

## <a name="validating-tokens"></a>Tokenek érvényesítése

Ezen a ponton a egyszerre az ügyfél-alkalmazás kell végrehajtani a csak jogkivonat érvényességi érvényesítése id_tokens.  Annak érdekében, hogy egy id_token érvényesíteni, az alkalmazás ellenőrizni kell a id_token aláírás és a id_token a követelések is.

Elvégezheti a szükséges tárak és mintakódok, amely jogkivonat érvényesítés, egyszerűen kezelése megjelenítése, ha másnak szeretné, hogy a mögöttes folyamat megértése.  Vannak is számos külső forrás megnyitása tárak rendelkezésre JWT érvényességi, szinte minden platform és nyelvek legalább egy lehetőséget. Azure Active Directory authentication tárak és mintakódok kapcsolatos további tudnivalókért lásd [Azure Active Directory authentication tárak](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Az aláírás érvényesítése

Egy JWT vannak elválasztva három szegmensre tartalmazza a `.` karaktert.  Az első szakasz a **fejléc**, a második másként **törzsébe**, és a harmadik az **aláírás**van neve.  Az aláírás szakaszában, hogy az alkalmazás megbízhatónak tekinthetők meg a id_token hitelességére érvényesítéséhez használható.

Id_Tokens bejelentkezve iparágban szabványos aszimmetrikus titkosítást algoritmusok, például RSA 256 használatával. A fejlécen a id_token, jelentkezzen be a token használt billentyűt, és titkosítási módszer ismerteti:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

A `alg` állítást jelzi a algoritmus, jelentkezzen be a token, míg használt a `x5t` állítást jelzi az adott nyilvános kulcs, jelentkezzen be a token használt.

Bármely pontján adott időben Azure Active Directory előfordulhat, hogy jelentkezzen be egy nyilvános-titkos kulcs párban részhalmazát bármelyikével id_token. Azure Active Directory elforgatása billentyűk rendszeres időközönként, a lehetséges készlete, így kell írni az alkalmazás automatikusan kezelheti a fontosabb változásokról.  Egy lehetővé teszi az Azure Active Directory által használt a nyilvános kulcshoz frissítések keresése gyakoriság 24 óránként van.

A csatlakozás OpenID metaadat-dokumentum található használatával aláírás érvényesítéséhez szükséges aláírási kulcs adatok szerezheti be:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Ez az URL a böngészőben próbálja!

A metaadat-dokumentum a JSON-objektumra, amely több hasznos információkat vehet, például a hely, a különböző végpontok OpenID csatlakozás hitelesítése szükséges.  

Is tartalmaz egy `jwks_uri`, amely ad a tokenek aláírásához használt nyilvános kulcs készlete helyét.  A JSON-dokumentum található a `jwks_uri` összes használja-e adott időpillanatban nyilvános kulcs információkat tartalmazza.  Az alkalmazás használata a `kid` formál, jelölje ki, melyik nyilvános kulcs a dokumentumban használt jelentkezzen be az egy adott token JWT fejlécében.  Ezután műveleteket hajthat végre a megfelelő nyilvános kulcs és a megjelölt algoritmus aláírás érvényesítése.

Aláírás elvégzéséhez a dokumentum hatókörén kívüli - sok Megnyitás tárak áll rendelkezésre segít teheti, ha szükséges.

#### <a name="validating-the-claims"></a>A jogcímalapú érvényesítése

Amikor az alkalmazás-felhasználónak bejelentkezés után id_token kap, a id_token meg kell végezni néhány ellenőrzések szemben.  Az ilyen tartalmazzák, de nem:

  - A **célközönség** állítást - ellenőrzéséhez, hogy a id_token célja fordítani az alkalmazást.
  - A **Nem előtti** és a **Lejárati idő** jogcímalapú - ellenőrizze, hogy a id_token nem járt le.
  - A **kibocsátó** állítást - ellenőrzéséhez, hogy a token valóban bocsátotta az alkalmazást az Azure Active Directory.
  - A **Nonce** – a token ismétléses támadások csökkentésében.
  - és így tovább...

Az alkalmazás végrehajtsa állítást ellenőrzések teljes listáját olvassa el a [Csatlakozás OpenID specifikációja](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Ezek jogcímeken várható értékének részleteit [id_token szakasz](#id-tokens) előző szakaszt tartalmaz.

## <a name="sample-tokens"></a>Minta tokenek

Ebben a szakaszban a SAML és JWT tokenek Azure AD eredményül adó minták jeleníti meg. Ezek a minták látványosan lásd: a jogcímalapú környezetben.
SAML-jogkivonat

Ez a egy mintája szerepel egy tipikus SAML token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT jogkivonat - felhasználó megszemélyesítési

Ez a minta egy tipikus JSON webes jogkivonat (JWT) egy engedélyezési kód támogatási folyamat használja.
Követelések mellett a token szerepel a verziószám **ver** és **appidacr**, a hitelesítési helyi osztály hivatkozást, amely azt jelzi, hogy az ügyfél hitelesítésének módját. Egy nyilvános ügyfél értéke 0. Ha egy ügyfél-azonosító vagy ügyfél titkos használt, az érték 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Kapcsolódó tartalom
- Az Azure Active Directory Graph [házirend műveletek](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) és a [házirend entitás](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity)jogkivonat élettartam házirend keresztül az Azure Active Directory Graph API kezelésével kapcsolatban további megtekintése
- Házirendjeivel keresztül PowerShell-parancsmagok minták, beleértve a minták és további információt talál [az Azure Active Directory konfigurálható jogkivonat élettartama](active-directory-configurable-token-lifetimes.md). 
