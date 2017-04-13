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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0-s engedélyezési kód továbbításához

Védett erőforrások, például a webes API-khoz eléréséhez szükséges eszközön telepített alkalmazások OAuth 2.0-s engedélyezési kód megadása használható. Az Azure Active Directory (Azure Active Directory) B2C végrehajtása OAuth 2.0-s verzióját használja, a mobil és az asztali alkalmazások előfizetési, bejelentkezés, és egyéb identitás teendőkről is adhat. Ez az útmutató független nyelv. Ismerteti, hogyan üzenetek küldése és fogadása HTTP nélkül, a Megnyitás-forrás tárak közül.

<!-- TODO: Need link to libraries -->

A OAuth 2.0-s engedélyezési kód folyamat [az OAuth 2.0 specifikáció 4.1](http://tools.ietf.org/html/rfc6749)szakaszban leírt. A legtöbb alkalmazás típusú, beleértve a [web Apps alkalmazások](active-directory-b2c-apps.md#web-apps) és a [natív módon telepített alkalmazások](active-directory-b2c-apps.md#mobile-and-native-apps)hitelesítési és engedélyezési végrehajtásához felhasználhatja azt. Biztonságos módon szerezheti be **access_tokens**, amely információforrások, amelyek egy [engedélyezési kiszolgáló](active-directory-b2c-reference-protocols.md#the-basics)által biztosított eléréséhez használható alkalmazások lehetővé teszi.

Ez az útmutató egy adott változat OAuth 2.0-s engedélyezési kód folyamat –**nyilvános ügyfelek**összpontosít. Nyilvános ügyfél, akkor bármelyik ügyfélalkalmazás, amely nem biztonságos megőrzéséhez titkos jelszó integritását megbízható. Ide tartoznak a mobilalkalmazások, az asztali alkalmazások és a meglehetősen sok bármely alkalmazásban, amely a eszközön fut, és meg kell kapnia access_tokens. Ha szeretné az Identitáskezelés hozzáadása webalkalmazást Azure Active Directory B2C használatával, [OpenID csatlakozni](active-directory-b2c-reference-oidc.md) , hanem OAuth 2.0-s kell használnia.

Azure Active Directory B2C bővíti a standard OAuth 2.0-s végezze el a több, mint az egyszerű hitelesítési és engedélyezési folyik át. A [**házirend-paraméter**](active-directory-b2c-reference-policies.md), amely lehetővé teszi, hogy a felhasználói élmény felvétele az alkalmazást, például a OAuth 2.0 használatával bevezetett-előfizetés, bejelentkezés, és profilok kezelése. Itt előadást OAuth 2.0-s és a házirendek használatáról a saját alkalmazások az alábbi szolgáltatások mindegyike végrehajtásához. Bemutatjuk access_tokens beszerzése eléréséhez a webes API-khoz is.

Az alábbi példa a HTTP-kérelmek a minta B2C directory, a **fabrikamb2c.onmicrosoft.com**, valamint a minta alkalmazás és a házirendek fogja használni. Szabadon próbálja meg saját maga a kérések használatával ezeket az értékeket, vagy lecserélheti őket a saját.
Megtudhatja, hogyan hozhatja ki [a saját B2C directory, alkalmazást, és házirendeket](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. a get-engedélyezési kód
A kód engedélyezési folyamat kezdődik az ügyfél, a felhasználót, hogy mely irányítja a `/authorize` végpontot. A folyamat, interaktív részét az, ahol a felhasználó ténylegesen járnak. A kérést, az ügyfél szerezheti be a felhasználó elől a szükséges engedélyeket azt mutatja, a `scope` paraméterre és a házirendet, amely a végrehajtása a `p` paraméter. Három példa pontjait az alábbiakban (sortöréssel olvashatóság érdekében), minden más házirenddel.

#### <a name="use-a-sign-in-policy"></a>Bejelentkezési-házirend használata

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Regisztráció-házirend használata

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Profil szerkesztése-házirend használata

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Paraméter | Szükség? | Leírás |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Szükséges | Az alkalmazás azonosítója rendelt az [Azure portál](https://portal.azure.com) az alkalmazást. |
| response_type | Szükséges | A tartalmaznia kell választípust `code` az engedélyezés kód továbbításhoz. |
| redirect_uri | Szükséges | Az alkalmazást, ahol küldött és megkapta az alkalmazás a hitelesítési válaszok redirect_uri. Azt pontosan egyeznie kell az portálon regisztrált a redirect_uris közül azzal a különbséggel, hogy az URL-címként kódolandó kell lennie. |
| hatókör | Szükséges | Keresési tartományok terület tagolt listáját. Egyetlen hatókör érték, akkor az Azure AD mindkét kérnek engedélyt. Az ügyfél-azonosító használ, mint a hatókör azt jelzi, hogy az alkalmazás szüksége van egy **jogkivonat elérése** szemben a saját szolgáltatás vagy a webes API-val használt, jelöli ugyanazt az ügyfél azonosítót.  A `offline_access` hatókör azt jelzi, hogy az alkalmazás egy **refresh_token** lesz szüksége erőforrások élettartamú elérését.  Is használhatja a `openid` hatókör- **id_token** kérni az Azure Active Directory B2C. |
| response_mode | Ajánlott | A módszer, amellyel az eredményül kapott authorization_code küldi vissza az alkalmazást. "Lekérdezés", "form_post" vagy "fragment" közül lehet.
| állam | Ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték. Lehet, hogy a minden olyan tartalom, amelyben karakterlánc. Véletlenszerűen generált egyedi érték webhelyközi kérelem hamisítása támadások megakadályozása általában szolgál. Az állapot kódolását az alkalmazás a felhasználó állapotával kapcsolatos információk, mielőtt a hitelesítési kérelem történt, például az oldal, mintha vagy a házirend végrehajtott is szolgál. |
| p | Szükséges | A házirend végrehajtandó. Egy B2C címtárában létrehozott szabály neve. A házirend-Név oszlopban látható értéket kell kezdődnie "b2c\_1\_". További tudnivalók a házirendek az [bővíthető keretet](active-directory-b2c-reference-policies.md). |
| figyelmeztető üzenet | Nem kötelező. | A szükséges felhasználói beavatkozás típusát. Jelenleg csak érvényes érték "login", amely a felhasználó megadnia a hitelesítő adatait, hogy kérésre kezd. Egyszeri bejelentkezés lép érvénybe. |

Ezen a ponton a felhasználót a rendszer kéri a házirend munkafolyamat végrehajtásához. Ez a felhasználó beírása saját felhasználónevét és jelszavát, bejelentkezés egy közösségi identitás a címtárban vagy más száma lépései attól függően, hogy hogyan definiálva a házirend feliratkozna állhat.

A felhasználó a házirend befejeződése után Azure Active Directory ad vissza az alkalmazást, a megjelölt adott válasz `redirect_uri`, a megadott módszerrel a `response_mode` paraméter. A válasz pontosan azonos lesz az egyes független lett végrehajtva házirendet a fenti esetek.

A sikeres választ használó `response_mode=query` hasonlít:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| kód | A authorization_code, amely kéri az alkalmazás. Az alkalmazás segítségével az engedélyezési kód kérése a cél erőforrás-access_token. Authorization_codes nagyon rövid élettartamú. Általában járnak KB után. |
| állam | Lásd a teljes körű leírását az előző táblázatra. Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy az állapot értékek a kérelem és a válasz azonos. |

Hibaüzenetek is küld a `redirect_uri` , hogy az alkalmazás képes kezelni őket megfelelően:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Meghatározott hibák üzenet, amely a kiváltóok-hitelesítés hiba azonosítása egy fejlesztő segítséget. |
| állam | Lásd az első tábla ebben a szakaszban a teljes körű leírását. Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Az alkalmazás kell győződjön meg róla, hogy a kérelem és a válasz állam értékein azonos. |


## <a name="2-get-a-token"></a>2. a jogkivonat beszerzése
Most, hogy egy authorization_code szerzett beváltása a `code` a kívánt erőforrás küldésével jogkivonat egy `POST` kérése a `/token` végpontot. Az Azure Active Directory B2C kérheti egy token csak erőforrás az alkalmazás saját kódmentes webes API-val. Az használni kívánt saját magához egy token kérő használt, ha az alkalmazás ügyfél-azonosító egy keresési tartományt használni:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Paraméter | Szükség? | Leírás |
| ----------------------- | ------------------------------- | --------------------- |
| p | Szükséges | A házirend szerezheti be az engedélyezés kódot használt. A kérés egy másik házirend nem használhatók. Megjegyzés: Ez a paraméter hozzáadása a *lekérdezési karakterlánc*nem bejegyzés szövegében. |
| client_id | Szükséges | Az alkalmazás azonosítója rendelt az [Azure portál](https://portal.azure.com) az alkalmazást. |
| grant_type | Szükséges | Engedélyek biztosítása, amely lehet a típusú `authorization_code` az engedélyezés kód továbbításhoz. |
| hatókör | Reccommended | Keresési tartományok terület tagolt listáját. Egyetlen hatókör érték, akkor az Azure AD mindkét kérnek engedélyt. Az ügyfél-azonosító használ, mint a hatókör azt jelzi, hogy az alkalmazás szüksége van egy **jogkivonat elérése** szemben a saját szolgáltatás vagy a webes API-val használt, jelöli ugyanazt az ügyfél azonosítót.  A `offline_access` hatókör azt jelzi, hogy az alkalmazás egy **refresh_token** lesz szüksége erőforrások élettartamú elérését.  Is használhatja a `openid` hatókör- **id_token** kérni az Azure Active Directory B2C. |
| kód | Szükséges | A authorization_code, amely a, a folyamat első láb vásárolta meg. |
| redirect_uri | Szükséges | Az alkalmazás, ahol a authorization_code kapott redirect_uri. |

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
| access_token | Az aláírt JSON webes jogkivonat (JWT) token kért. |
| hatókör | A tartományok, amely a token érvényes, későbbi felhasználás céljából tokenek gyorsítótárazás használható. |
| expires_in | Amely a token (másodpercben) érvényes idejének hosszát. |
| refresh_token | Egy OAuth 2.0-s refresh_token. Az alkalmazás szerez további tokenek, az aktuális jogkivonathoz lejárta után a token használhatja. Refresh_tokens vannak életben ideig volt, és amely megőrzi az erőforrásokhoz hosszabb ideig is használható. További részleteket tekintse át a [B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md). |

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
| error_description | Meghatározott hibák üzenet, amely a kiváltóok-hitelesítés hiba azonosítása egy fejlesztő segít. |

## <a name="3-use-the-token"></a>3. Kattintson a token
Most, hogy sikeresen vásárolta az `access_token`, használhatja a token kérelmek a kódmentes a webes API-khoz meg felvételével a `Authorization` élőfej:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. a token frissítése
Az Access & Id tokenek olyan rövid élettartamú. Frissítenie kell azokat után továbbra is, nem tudnak hozzáférni az erőforrások járnak. Tehetné küldése egy másik `POST` kérése a `/token` végpontot. Ez esetben adja meg a `refresh_token` helyett a `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Paraméter | Szükség? | Leírás |
| ----------------------- | ------------------------------- | -------- |
| p | Szükséges | A házirend szerezheti be az eredeti refresh_token használt. A kérés egy másik házirend nem használhatók. Megjegyzés: Ez a paraméter hozzáadása a *lekérdezési karakterlánc*nem bejegyzés szövegében. |
| client_id | Ajánlott | Az alkalmazás azonosítója rendelt az [Azure portál](https://portal.azure.com) az alkalmazást. |
| grant_type | Szükséges | Engedélyek biztosítása, amely lehet a típusú `refresh_token` a kód engedélyezési folyamat a láb. |
| hatókör | Ajánlott | Keresési tartományok terület tagolt listáját. Egyetlen hatókör érték, akkor az Azure AD mindkét kérnek engedélyt. Az ügyfél-azonosító használ, mint a hatókör azt jelzi, hogy az alkalmazás szüksége van egy **jogkivonat elérése** szemben a saját szolgáltatás vagy a webes API-val használt, jelöli ugyanazt az ügyfél azonosítót.  A `offline_access` hatókör azt jelzi, hogy az alkalmazás egy **refresh_token** lesz szüksége erőforrások élettartamú elérését.  Is használhatja a `openid` hatókör- **id_token** kérni az Azure Active Directory B2C. |
| redirect_uri | Nem kötelező. | Az alkalmazás, ahol a authorization_code kapott redirect_uri. |
| refresh_token | Szükséges | A második láb, az áramlás vásárolta meg eredeti refresh_token. |

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
| access_token | Az aláírt JSON webes jogkivonat (JWT) token kért. |
| hatókör | A tartományok, amely a token érvényes, későbbi felhasználás céljából tokenek gyorsítótárazás használható. |
| expires_in | Amely a token (másodpercben) érvényes idejének hosszát. |
| refresh_token | Egy OAuth 2.0-s refresh_token. Az alkalmazás szerez további tokenek, az aktuális jogkivonathoz lejárta után a token használhatja. Refresh_tokens vannak életben ideig volt, és amely megőrzi az erőforrásokhoz hosszabb ideig is használható. További részleteket tekintse át a [B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md). |

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

## <a name="use-your-own-b2c-directory"></a>A saját B2C könyvtár használata

Ha szeretne e kérelmek meg magának, előbb végre ezeket a lépéseket, és az majd cserélje ki a példát feletti saját:

- [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) és a kéréseket a címtárában be.
- [-Alkalmazás létrehozása](active-directory-b2c-app-registration.md) egy azonosítója és egy redirect_uri juthat. Az alkalmazás **natív ügyfele** szerepeltetni kívánt.
- [A házirendek létrehozása](active-directory-b2c-reference-policies.md) beszerzése a szabály nevét.
