<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Webalkalmazás létrehozása az Azure Active Directory végrehajtása a csatlakozás OpenID hitelesítési protokoll használatával."
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

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Webes jelentkezzen be OpenID csatlakoztatása

A hitelesítési protokollt, fölött OAuth 2.0-s biztonságosan felhasználók bejelentkezni a webalkalmazások használható beépített OpenID csatlakozás.  Az Azure Active Directory (Azure Active Directory) B2C végrehajtása OpenID csatlakozás használatával is erőforrás kihelyezése előfizetési, a bejelentkezési, és más Identitáskezelés az Azure Active Directory a webalkalmazások találkozik. Ez az útmutató kell követnie erre nyelvi beállítástól független módon. Az üzenetek küldése és fogadása HTTP nélkül, a Megnyitás-forrás tárak közül fog ismertetik.

[Csatlakozás OpenID](http://openid.net/specs/openid-connect-core-1_0.html) kiterjeszti az OAuth 2.0-s *engedélyezési* protokoll használatát a *hitelesítési* protokollt. Lehetővé teszi az egyszeri bejelentkezés OAuth használatával végezheti el. Azt fogalmának egy `id_token`, vagyis egy olyan biztonsági jogkivonat lehetővé teszi, hogy az ügyfél annak a felhasználónak az identitás ellenőrzése és a felhasználói profil alapvető információhoz juthat.

Mivel ez elég OAuth 2.0-s, is lehetővé teszi alkalmazások biztonságos módon szerezheti be **access_tokens**. [Engedély kiszolgáló](active-directory-b2c-reference-protocols.md#the-basics)által biztosított erőforrások eléréséhez access_tokens is használhatja. Azt javasoljuk OpenID kapcsolódni, ha egy webalkalmazás, amely egy kiszolgálón tárolt és a böngészőn keresztül érhető el szeretne összeállítani. Ha azt szeretné, az Identitáskezelés hozzáadása a mobil vagy az asztali alkalmazások Azure Active Directory B2C, [OAuth 2.0-s](active-directory-b2c-reference-oauth-code.md) , hanem OpenID csatlakozás kell használnia.

Azure Active Directory B2C a szabványos OpenID csatlakozás protokoll végezze el a több, mint az egyszerű hitelesítési és engedélyezési kiterjed. A [**házirend-paraméter**](active-directory-b2c-reference-policies.md), amely lehetővé teszi, hogy a felhasználói élmény felvétele az alkalmazás – például a OpenID csatlakozás használatával bevezetett-előfizetés, bejelentkezés, és profilok kezelése. Itt bemutatjuk, OpenID csatlakozás és a házirendek használatáról a webalkalmazások az alábbi szolgáltatások mindegyike végrehajtásához. Is bemutatjuk, access_tokens beszerzése a webes API-khoz eléréséhez.

Az alábbi példa HTTP-kérelmek a minta B2C címtár, **fabrikamb2c.onmicrosoft.com**, valamint a minta alkalmazás **https://aadb2cplayground.azurewebsites.net** és házirendek fogja használni. Szabadon kipróbálni a kérések saját maga használatával ezeket az értékeket, vagy lecserélheti őket a saját.
Megtudhatja, hogyan hozhatja ki [a saját B2C bérlői, az alkalmazást, és a házirendek](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Hitelesítési kérések küldése
A web App alkalmazásban kell hitelesíteni a felhasználót, majd hajtsa végre a házirendet, amikor azt milyen módokon kérheti fel a felhasználót, hogy a `/authorize` végpontot. Ez az interaktív részét a folyamat, ahol a felhasználó ténylegesen járnak, attól függően, hogy a házirend.

A kérést, az ügyfél szerezheti be a felhasználó elől a szükséges engedélyeket azt mutatja, a `scope` paraméterre és a házirendet, amely a végrehajtása a `p` paraméter. Három példa pontjait az alábbiakban (sortöréssel olvashatóság érdekében), minden más házirenddel. Megtudhatja, minden egyes kérelem működése, hogy a kérelem beillesztése a böngészőben, és futtatásával.

#### <a name="use-a-sign-in-policy"></a>Bejelentkezési-házirend használata

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Regisztráció-házirend használata

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Profil szerkesztése-házirend használata

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Paraméter | Szükség? | Leírás |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Szükséges | Az alkalmazás azonosítója rendelt az [Azure portál](https://portal.azure.com/) az alkalmazás. |
| response_type | Szükséges | A tartalmaznia kell választípust `id_token` az OpenID csatlakozni. Ha a web App alkalmazásban a webes API hívásához is tokenek szükség van, akkor `code+id_token`, elvégzettként azt már itt. |
| redirect_uri | Ajánlott | Az alkalmazást, ahol küldött és megkapta az alkalmazás a hitelesítési válaszokkal redirect_uri. Azt pontosan egyeznie kell az portálon regisztrált a redirect_uris közül azzal a különbséggel, hogy az URL-címként kódolandó kell lennie. |
| hatókör | Szükséges | Keresési tartományok terület tagolt listáját. Egyetlen hatókör érték, akkor az Azure AD mindkét kérnek engedélyt. A `openid` hatókör engedélyt a felhasználónak bejelentkezés, és a felhasználó **id_tokens** (további új funkciók hamarosan meg ez a cikk későbbi részében) formájában kapcsolatos adatok beolvasása jelzi. A `offline_access` hatóköre web Apps alkalmazások nem kötelező. Azt jelzi, hogy az alkalmazás egy **refresh_token** lesz szüksége erőforrások élettartamú elérését. |
| response_mode | Ajánlott | A módszer, amellyel az eredményül kapott authorization_code küldenek vissza az alkalmazást. "Lekérdezés", "form_post" vagy "fragment" közül lehet.  a nagyobb biztonság "form_post" ajánlott. |
| állam | Ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték. Lehet, hogy a minden olyan tartalom, amelyben karakterlánc. Véletlenszerűen generált egyedi érték webhelyközi kérelem hamisítása támadások megakadályozása általában szolgál. Az állapot kódolását az alkalmazás a felhasználó állapotával kapcsolatos információ, mielőtt a hitelesítési kérelem történt, például az oldal, mintha is szolgál. |
| nonce | Szükséges | A kérelem (a által generált) fog szerepelni az eredményül kapott id_token igény szerint szereplő érték. Az alkalmazás majd ellenőrizze, hogy ez az érték jogkivonat ismétléses csökkentésében. Az érték általában véletlenszerűen, egyedi karakterlánc, amely azonosítja a kérelmet forrását is használható. |
| p | Szükséges | A házirend végrehajtott. Egy szabály a B2C bérlői webhelyén létrehozott neve. A házirend-Név oszlopban látható értéket kell kezdődnie "b2c\_1\_". További tudnivalók a házirendek az [bővíthető keretet](active-directory-b2c-reference-policies.md). |
| figyelmeztető üzenet | Nem kötelező. | A szükséges felhasználói beavatkozás típusát. Jelenleg csak érvényes érték "bejelentkezés", amely a felhasználó megadnia a hitelesítő adatait, hogy kérésre kezd. Egyszeri bejelentkezés lép érvénybe. |

Ezen a ponton a felhasználót a rendszer kéri a házirendet a munkafolyamat végrehajtásához. Ez a felhasználó a felhasználónév és jelszó megadására bejelentkezés egy közösségi identitás a címtárban vagy más száma lépései attól függően, hogy hogyan definiálva a házirend feliratkozna állhat.

A felhasználó a házirend befejeződése után Azure Active Directory ad vissza az alkalmazást, a megjelölt adott válasz `redirect_uri`, a megadott módszerrel a `response_mode` paraméter. A válasz pontosan azonos lesz az egyes független lett végrehajtva házirendet a fenti esetek.

A sikeres választ használatával `response_mode=fragment` jelenne meg:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| id_token | A id_token, amely kéri az alkalmazás. Ellenőrizze a felhasználói azonosító, és kezdje el a felhasználó munkamenetet a id_token is használhatja. Id_tokens és tartalmukat további tájékoztatást az [Azure Active Directory B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md)tartalmaz. |
| kód | A kért az alkalmazást, ha korábban authorization_code `response_type=code+id_token`. Az alkalmazás segítségével az engedélyezési kód kérése a cél erőforrás-access_token. Authorization_codes nagyon rövid élettartamú. Ezek általában körülbelül 10 perc múlva lejár. |
| állam | Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy az állapot értékeket a kérelem és a válasz azonos. |

Hibaüzenetek is lehet küldeni a `redirect_uri` , hogy az alkalmazás képes kezelni őket megfelelően:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba. |
| állam | Lásd a teljes körű leírását az előző táblázatra. Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy az állapot értékeket a kérelem és a válasz azonos. |


## <a name="validate-the-idtoken"></a>A id_token ellenőrzése
Csak a egy id_token fogadása nem elég csatlakozó felhasználók hitelesítését – kell a id_token aláírás ellenőrzése, és ellenőrizze a token szerint az alkalmazás – követelmények követelések. Azure Active Directory B2C tokenek jelentkezzen, és győződjön meg róla, hogy azok érvényes [JSON webes tokenek (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) és a nyilvános kulcs titkosítás használja.

Nem lehet sok Megnyitás-forrás elérhető érvényesítése JWTs, attól függően, hogy a nyelvi preferencia. Javasoljuk, hogy felfedezése ezeket a beállításokat, és nem saját érvényességi logikájának végrehajtása. Az alábbi információk hasznosak, tudja, hogy miként megfelelően alkalmazza a tárak lesz.

Azure Active Directory B2C OpenID csatlakozás metaadat-alapú zárólap, amely lehetővé teszi, hogy az alkalmazás-ból az Azure Active Directory B2C futásidőben információkat tartalmaz. Ezt az információt a végpontok, jogkivonat tartalma és jogkivonat-aláíró billentyűk tartalmazza. Van egy JSON metaadat-dokumentum minden házirend a B2C bérlői webhelyén. Példa a metaadat-alapú dokumentumot a `b2c_1_sign_in` házirend a `fabrikamb2c.onmicrosoft.com` helyen található:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

A konfigurációs dokumentum tulajdonságainak egyik a `jwks_uri`, amelynek értéke ugyanazt az házirendet a következő lesz:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

A rendelés határozza meg, milyen házirend-id_token aláírásokkal használt (és a hol-ból a metaadatok), két lehetőség közül választhat. Első lépésként a házirend neve szerepel a `acr` a id_token lévő igényt. Kapcsolatos tudnivalókat az igényeit egy id_token elemezni, akkor olvassa el az [Azure Active Directory B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md). A többi azt javasoljuk, hogy a házirend értéke kódolását a `state` hiba a kérést, és ezután dekódolását azt határozza meg, milyen házirend használt paraméter. Mindkét módszer akkor tökéletesen érvényes.

Miután a metaadat-alapú dokumentumok a metaadat-alapú OpenID csatlakozás végpont szerzett használhatja-e RSA 256 nyilvános kulcsok (találhatók a végpont) a id_token aláírás érvényesítéséhez. Több billentyűk szerepel a megadott bármikor végpont idő lehetnek, minden egyes jelölt egy `kid`. A fejléc, a id_token is tartalmaz egy `kid` formál, ami azt jelenti, hogy melyik billentyűk használt jelentkezzen be a id_token. A további tudnivalók és szakaszok [tokenek ellenőrzése](active-directory-b2c-reference-tokens.md#validating-tokens) és a [fontos információkat a fő átváltási aláírási](active-directory-b2c-reference-tokens.md#validating-tokens) [Azure Active Directory B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md) talál.
<!--TODO: Improve the information on this-->

A id_token aláírása érvényesítése után vannak több követelések, hogy szüksége lesz, ha ellenőrizni szeretné, például:

- Meg kell erősítenie a `nonce` formál jogkivonat ismétléses megakadályozása érdekében. Az érték kell a bejelentkezési kérésben megadott.
- Meg kell erősítenie a `aud` állítják annak érdekében, hogy az alkalmazás a id_token lett-e ki. Az érték az alkalmazás azonosítója kell lennie.
- Meg kell erősítenie a `iat` és `exp` követelések annak érdekében, hogy a id_token nem járt le.

Is számos további ellenőrzések, amely akkor kell végrehajtani, a [OpenID csatlakozás Core specifikáció](http://openid.net/specs/openid-connect-core-1_0.html)részletes ismertetése.  Érdemes azt is, további követelések, az esete érvényesítéséhez. Néhány általános ellenőrzések a következők:

- Annak ellenőrzése, hogy a felhasználó szervezet regisztrált a alkalmazáshoz.
- Annak ellenőrzése, hogy a felhasználó rendelkezik-e a megfelelő engedélyezés vagy jogokkal.
- Annak ellenőrzése, hogy egy bizonyos erőssége hitelesítési történt, például az Azure többtényezős hitelesítés.

Az egy id_token követelések a további tudnivalókért lásd: az [Azure Active Directory B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md).

Teljesen érvényesítése a id_token után kezdje el a felhasználó munkamenetet, és a id_token követelések használatával megjelenítheti az alkalmazás a felhasználó adatait. Ez az információ használható megjelenítési, rekordokat, engedélyezési és így tovább.

## <a name="get-a-token"></a>Jogkivonat beszerzése
Ha, hogy web app igényeinek megfelelően hajtsa végre a házirendek végrehajtása, kihagyhatja a következő néhány szakaszokkal. Ezek a szakaszok a webes alkalmazásai szükséges webes API hívások hitelesített és Azure Active Directory B2C is védettek csak vonatkoznak.

A authorization_code beszerzett, felhasználói (használatával `response_type=code+id_token`) a kívánt erőforrás küldésével jogkivonat egy `POST` kérése a `/token` végpontot. Csak az erőforrás, kérheti egy token jelenleg az alkalmazás saját kódmentes webes API-val. A használni kívánt saját magához egy token kérése az alkalmazás ügyfél-azonosító tartományként a hatókör:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Paraméter | Szükség? | Leírás |
| ----------------------- | ------------------------------- | --------------------- |
| p | Szükséges | A házirend szerezheti be az engedélyezés kódot használt. A kérés egy másik házirend nem használhatók. Megjegyzés: Ez a paraméter hozzáadása a **lekérdezési karakterlánc**nem bejegyzés törzsében. |
| client_id | Szükséges | Az alkalmazás azonosítója rendelt az [Azure portál](https://portal.azure.com/) az alkalmazás. |
| grant_type | Szükséges | Engedélyek biztosítása, amely lehet a típusú `authorization_code` az engedélyezés kód továbbításhoz. |
| hatókör | Ajánlott | Keresési tartományok terület tagolt listáját. Egyetlen hatókör érték, akkor az Azure AD mindkét kérnek engedélyt. A `openid` hatókör engedélyt a felhasználónak bejelentkezés, és a felhasználó **id_tokens**formájában kapcsolatos adatok beolvasása jelzi. Az alkalmazás saját kódmentes webes API-val, amely az alkalmazás azonosítója megegyezik az ügyfél által jelölt tokenek eléréséhez használható. A `offline_access` hatókör azt jelzi, hogy az alkalmazás egy **refresh_token** lesz szüksége erőforrások élettartamú elérését. |
| kód | Szükséges | A authorization_code, amely a, a folyamat első láb vásárolta meg. |
| redirect_uri | Szükséges | Az alkalmazás, ahol a authorization_code kapott redirect_uri. |
| client_secret | Szükséges | Az alkalmazás titkos az [Azure portál](https://portal.azure.com/)létrehozott. Ez az alkalmazás titkos kulcs egy fontos biztonsági eltérés. Meg kell azt biztonságosan tárolhatja a kiszolgálón. Meg kell is gondoskodik elforgatása a ügyfél titkos rendszeres időközönként. |

A sikeres jogkivonat választ fog kinézni:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| not_before | Az idő, amelynél a token érvényes, alapidőpont időben. |
| token_type | A token típusú érték. Az egyetlen típus, amely támogatja az Azure Active Directory Bearer. |
| access_token | Az aláírt JWT token kért. |
| hatókör | A tartományok, amely a token érvényes, későbbi felhasználás céljából tokenek gyorsítótárazás használható. |
| expires_in | Amely a access_token (másodpercben) érvényes idejének hosszát. |
| refresh_token | Egy OAuth 2.0-s refresh_token. Az alkalmazás szerez további tokenek, az aktuális jogkivonathoz lejárta után a token használhatja.  Refresh_tokens vannak életben ideig volt, és amely megőrzi az erőforrásokhoz hosszabb ideig is használható. További részleteket tekintse át a [B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md). Megjegyzés: kell használt a hatókör `offline_access` engedélyezési és annak érdekében, hogy egy refresh_token fogadása jogkivonat összehívásokat is. |

Hibaüzenetek fog kinézni:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba. |

## <a name="use-the-token"></a>A token használata
Most, hogy sikeresen vásárolta az `access_token`, használhatja a token kérelmek a kódmentes a webes API-khoz meg felvételével a `Authorization` élőfej:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>A token frissítése
A id_tokens olyan rövid élettartamú. Frissítenie kell azokat után továbbra is, nem tudnak hozzáférni az erőforrások járnak. Tehetné küldése egy másik `POST` kérése a `/token` végpontot. Ebben az esetben adja meg a `refresh_token` helyett a `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Paraméter | Szükséges | Leírás |
| ----------------------- | ------------------------------- | -------- |
| p | Szükséges | A házirend szerezheti be az eredeti refresh_token használt. A kérés egy másik házirend nem használhatók. Megjegyzés: Ez a paraméter hozzáadása a **lekérdezési karakterlánc**nem bejegyzés törzsében. |
| client_id | Szükséges | Az alkalmazás azonosítója rendelt az [Azure portál](https://portal.azure.com/) az alkalmazás. |
| grant_type | Szükséges | Engedélyek biztosítása, amely lehet a típusú `refresh_token` Ez a kód engedélyezési folyamat láb. |
| hatókör | Ajánlott | Keresési tartományok terület tagolt listáját. Egyetlen hatókör érték, akkor az Azure AD mindkét kérnek engedélyt. A `openid` hatókör engedélyt a felhasználónak bejelentkezés, és a felhasználó **id_tokens**formájában kapcsolatos adatok beolvasása jelzi. Az alkalmazás saját kódmentes webes API-val, amely az alkalmazás azonosítója megegyezik az ügyfél által jelölt tokenek eléréséhez használható. A `offline_access` hatókör azt jelzi, hogy az alkalmazás egy **refresh_token** lesz szüksége erőforrások élettartamú elérését. |
| redirect_uri | Ajánlott | Az alkalmazás, ahol a authorization_code kapott redirect_uri. |
| refresh_token | Szükséges | A második láb, az áramlás vásárolta meg eredeti refresh_token. Megjegyzés kell használt a hatókör `offline_access` engedélyezési és annak érdekében, hogy egy refresh_token fogadása jogkivonat kérések is. |
| client_secret | Szükséges | Az alkalmazás titkos az [Azure portál](https://portal.azure.com/)létrehozott. Ez az alkalmazás titkos kulcs egy fontos biztonsági eltérés. Meg kell azt biztonságosan tárolhatja a kiszolgálón. Meg kell is gondoskodik elforgatása a ügyfél titkos rendszeres időközönként. |

A sikeres jogkivonat választ fog kinézni:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| not_before | Az idő, amelynél a token érvényes, alapidőpont időben. |
| token_type | A token típusú érték. Az egyetlen típus, amely támogatja az Azure Active Directory Bearer. |
| access_token | Az aláírt JWT token kért. |
| hatókör | A tartományok, amely a token érvényes, későbbi felhasználás céljából tokenek gyorsítótárazás használható. |
| expires_in | Amely a access_token (másodpercben) érvényes idejének hosszát. |
| refresh_token | Egy OAuth 2.0-s refresh_token. Az alkalmazás szerez további tokenek, az aktuális jogkivonathoz lejárta után a token használhatja.  Refresh_tokens vannak életben ideig volt, és amely megőrzi az erőforrásokhoz hosszabb ideig is használható. További részleteket tekintse át a [B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md). |

Hibaüzenetek fog kinézni:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba. |


## <a name="send-a-sign-out-request"></a>Kijelentkezési-összehívás küldése

Ha meg szeretné jelentkezzen ki a letölthető a felhasználót, még nem elég vagy törölje a jelet az alkalmazás cookie-kat, különben a program befejezése a felhasználó a munkamenetet. Azure AD ki szeretne jelentkezni a felhasználó is átirányítást kell beállítania. Ha nem sikerül kéri, a felhasználó előfordulhat, hogy tudja ír be újra a hitelesítő adatait a alkalmazásba hitelesítését. Ennek az oka egy érvényes bejelentkezéses munkamenetben az Azure Active Directory rendelkeznek.

Egyszerűen irányíthatja át a felhasználót, hogy a `end_session_endpoint` , amely szerepel-e a csatlakozás OpenID metaadatok dokumentumon "Ellenőrzése a id_token" a korábbi szakaszban leírt:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Paraméter | Szükség? | Leírás |
| ----------------------- | ------------------------------- | ------------ |
| p | Szükséges | A házirendet, amelyet szeretne használatával jelentkezhetek be a felhasználó ki az alkalmazást. |
| post_logout_redirect_uri | Ajánlott | Az URL-címet, amely a felhasználó e irányítva a után sikeres kijelentkezési. Ha még nem tartalmaz, a felhasználó által Azure Active Directory B2C általános üzenet fog megjelenni.  |

> [AZURE.NOTE]
    A felhasználót, hogy mely irányítja, miközben a `end_session_endpoint` törölje a felhasználó egyszeri bejelentkezéses állapota az Azure Active Directory B2C részét, azt nem fog jelentkezzen ki a felhasználó közösségi identitás szolgáltató (IDP) munkamenet a felhasználó. Ha a felhasználó ugyanazt a IDP kijelöli a későbbi bejelentkezés során, azok fog kell hitelesíteni, saját hitelesítő adatok beírása nélkül. Ha egy felhasználó szeretne jelentkezzen ki a B2C alkalmazásból, azt nem feltétlenül jelenti szeretnének teljesen jelentkezzen ki a Facebook-fiókját. Azonban helyi fiókok, amíg a felhasználó a munkamenet befejeződik megfelelően.

## <a name="use-your-own-b2c-tenant"></a>A saját B2C bérlői használata

Ha szeretne e kérelmek meg magának, előbb végre ezeket a lépéseket, és az majd cserélje ki a példát feletti saját:

- [B2C bérlő létrehozása](active-directory-b2c-get-started.md), és a bérlő a összehívásokban be.
- [-Alkalmazás létrehozása](active-directory-b2c-app-registration.md) egy azonosítója és egy redirect_uri juthat. Szerepeltetni kívánt egy **web app/webes api** az alkalmazást, és tetszés szerint hozzon létre egy **alkalmazás titkos**.
- [A házirendek létrehozása](active-directory-b2c-reference-policies.md) beszerzése a szabály nevét.
