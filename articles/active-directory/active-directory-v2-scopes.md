<properties
    pageTitle="Azure Active Directory 2.0-s verzió hatókörök és engedélyek, és a felhasználó hozzájárul ahhoz |} Microsoft Azure"
    description="Az Azure Active Directory 2.0-s verzió végpontot, például tartományok, az engedélyek és a felhasználó hozzájárul ahhoz az engedélyezés leírását."
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

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Keresési tartományok és engedélyek, és a 2.0-s verzió végpont jóváhagyása

Integrálása az Azure Active Directory-alkalmazások hajtsa végre az adott engedélyezési modell, amely lehetővé teszi a felhasználóknak szabályozhatja, hogy az alkalmazás hogyan is hozzáférhetnek az adataikhoz.  Frissítette a engedélyezési modell 2.0-s verzió végrehajtásának, módosítása, hogy az alkalmazás kell és együttműködését Azure AD.  Ez a témakör ismerteti a engedélyezési modell, beleértve a hatókörök, az engedélyek és a felhasználó hozzájárul ahhoz alapfogalmak.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="scopes--permissions"></a>Keresési tartományok és engedélyek

Azure AD a [OAuth 2.0-s](active-directory-v2-protocols.md) engedélyezési protokoll, amely lehetővé teszi a 3 fél app hozzáférésének webes által üzemeltetett erőforrások felhasználó nevében módszer hajtja végre.  Bármely webes által üzemeltetett erőforrás, amely integrálódik az Azure Active Directory egy erőforrás-azonosító vagy az **Alkalmazás azonosítója URI**fog rendelkezni.  Például a Microsoft webes által üzemeltetett erőforrások közé tartoznak:

- Az Office 365 levelezési API egyesített:`https://outlook.office.com`
- Az Azure Active Directory-grafikon API:`https://graph.windows.net`
- A Microsoft Graph-hoz:`https://graph.microsoft.com`

Ugyanez igaz integrálva van az Azure Active Directory 3 fél erőforrásokat.  Ezek az erőforrások közül is megadhatja, hogy egy jogosultságkészletet kisebb szövegadattömb az adott erőforrás működésének felosztása használható.  Példaként a Microsoft Graph definiálva van néhány engedélyekkel:

- Olvassa el a felhasználó naptár
- Írja be a felhasználó-naptárba
- E-mail küldése felhasználóként
- [+ További](https://graph.microsoft.io)

Az engedélyek megadásával az erőforrás lehetnek egyedi szabályozni kell az adatokat, és hogyan elérhetővé tett a külső világ.  A 3 fél-at is majd kérhet ezeket az engedélyeket a végfelhasználó - és a végfelhasználói jóvá kell hagynia az engedélyeket az alkalmazás működhetnek, akik a nevében előtt.  Az erőforrás-funkciókkal való kisebb engedélykészletek chunking, ez a 3 fél alkalmazások csak egyedi engedélyeket, hogy feladataik elvégzéséhez szükségük kérhet beépíthető.  Is lehetővé teszi a felhasználóknak, hogy pontosan hogyan az alkalmazásokban használandó adataikat, úgy, hogy több benne, hogy az alkalmazás nem viselkedik rosszindulatú legyenek.

Az Azure Active Directory és OAuth, ezeket az engedélyeket nevezik **hatókörök**.  Is megjelenhet **oAuth2Permissions**néven őket.  A hatókör az Azure Active Directory megfelelője szöveges értékként.  A Microsoft Graph példa folytatása, a hatókör minden engedélyt érték:

- Olvassa el a felhasználó naptár:`Calendar.Read`
- Írja be a felhasználó naptár:`Mail.ReadWrite`
- E-mail küldése felhasználóként:`Mail.Send`

Az alkalmazás kérhet ezeket az engedélyeket a 2.0-s verzió végpont kérelmek a hatókörök megadásával alábbiakban leírtak szerint.

## <a name="openid-connect-scopes"></a>Csatlakozás OpenId hatókörök

Csatlakozás OpenID 2.0-s verzió végrehajtásának van néhány pontosan meghatározott hatókörök, amely nem vonatkoznak bármely adott erőforrás - `openid`, `email`, `profile`, és `offline_access`.

#### <a name="openid"></a>OpenId

Ha az alkalmazás végrehajt bejelentkezési [OpenID összekapcsolás](active-directory-v2-protocols.md#openid-connect-sign-in-flow)funkcióval, meg kell kérnie a `openid` hatókörét.  A `openid` hatókör megjelenik a "Jelentkeztetni" engedélyezése a munkahelyi fiók hozzájárulása képernyőn, és a személyes Microsoft fiók hozzájárulása képernyőn, a "És alkalmazások és -szolgáltatások a Microsoft-fiókkal csatlakoztatása a saját profil megtekintése" engedéllyel.  Ezzel az engedéllyel lehetővé teszi, hogy az alkalmazás formájában egyedi azonosítót kaphat a felhasználó kap a `sub` formál.  A számára biztosítja a felhasználói adatok végpont alkalmazás hozzáférést is.  A `openid` hatókör is használható a 2.0-s verzió jogkivonat végpont szerezheti be id_tokens használt biztonságos HTTP hívások másik összetevői alkalmazás között.

#### <a name="email"></a>E-mailben

A `email` hatókör is szerepelni fog mentén a `openid` hatókör és a többi.  A felhasználók elsődleges e-mail címét formájában alkalmazás hozzáférést számára biztosítja, akkor a `email` formál.  A `email` állítást csak szerepelni fog a tokenek Ha e-mail címre társítva a felhasználói fiókot, amely nem mindig az esetet.  Ha használ a `email` hatókörét, az alkalmazás kell készíteni a kezelheti az esetben, amikor a `email` állítás nem létezik a token.

#### <a name="profile"></a>Profil

A `profile` hatókör is szerepelni fog mentén a `openid` hatókör és a többi.  Azt számára biztosítja a felhasználó adatai különböző alkalmazás hozzáférést.  Ez tartalmazza, de nem korlátozódik, a felhasználó Utónév, Vezetéknév, előnyben részesített felhasználónevét, Objektumazonosító, és így tovább.  Az adott felhasználó számára elérhető id_tokens profil követelések teljes listáját olvassa el a [2.0-s verzió jogkivonat-hivatkozást](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

A [ `offline_access` hatókör](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) lehetővé teszi, hogy az alkalmazás erőforrások eléréséhez a felhasználó nevében hosszabb ideig.  A munkahelyi fiók hozzájárulása képernyőn a hatókör megjelennek a "Hozzáférés az adatok bármikor" engedélyt.  A személyes Microsoft fiók hozzájárulása képernyőn mint a "Hozzáférés meg adatait bármikor" engedélyt fog megjelenni.  Amikor a felhasználó hagyja jóvá a `offline_access` hatókörét, az alkalmazást a 2.0-s verzió jogkivonat végpont frissítés tokenek fogadni engedélyezi.  Frissítés tokenek élettartamú, és lehetővé teszi a szerezheti be az új hozzáférési jogkivonat, a régebbi is jár le az alkalmazást.

Ha az alkalmazás nem kér a `offline_access` hatókörét, nem fog kapni refresh_tokens.  Ez azt jelenti, hogy az [OAuth 2.0-s engedélyezési kód adatfolyam](active-directory-v2-protocols.md#oauth2-authorization-code-flow)-authorization_code beváltása meg, amikor csak kap vissza egy, a access_token a `/token` végpontot.  Adott access_token egy rövid ideig (általában egy órával) érvényes marad, de végül lejár.  At pont időben, az alkalmazás a felhasználó átirányítása kell biztonsági másolatot a `/authorize` végpont egy új authorization_code beolvasásához.  Ez az átirányítási során a felhasználó előfordulhat, hogy, vagy előfordulhat, hogy nem kell újra megadnia a hitelesítő adatait, vagy újra hozzájárul engedélyek, attól függően, az alkalmazás típusát.

Beszerzése és használatáról további információt a tokenek frissítése, keresse meg a [2.0-s verzió Protocol (protokoll) hivatkozást](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Egyéni felhasználói hozzájárulása kérése

[OpenID csatlakozás vagy OAuth 2.0-s](active-directory-v2-protocols.md) engedélyezési kérelmet, az alkalmazás használatával szükséges engedélyeket kérhet a `scope` paraméteres lekérdezés.  Például a felhasználó bejelentkezik-alkalmazásba, amikor az alkalmazás volna összehívásokkal (ha az olvashatóság érdekében sortörést) a következőhöz hasonló:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

A `scope` paraméter az alkalmazás igénylő hatókörök terület tabulátorral tagolt listáját.  Minden egyes hatókör jelzi a hatókör értéket hozzáfűzi az erőforrás-azonosító (alkalmazás azonosítója URI).  A fenti kérés azt jelzi, hogy az alkalmazás engedéllyel a felhasználói naptár olvasása és küldése a Posta felhasználóként.

Után a felhasználó saját hitelesítő adatok bevitele, a 2.0-s verzió végpont **felhasználói beleegyezés**egyező rögzíteni keres.  Ha a felhasználó nem járult hozzá a kért engedélyek múltbeli, a 2.0-s verzió végpont megkérdezi, a felhasználót, hogy az engedélyeket a kért.  

![Munkahelyi fiók hozzájárulása képernyőképe](../media/active-directory-v2-flows/work_account_consent.png)

Amikor a felhasználó hagyja jóvá a engedélyt, a jóváhagyás lesz, hogy a felhasználó nem kell újra a későbbi bejelentkezési bővítmények beleegyezés.

## <a name="requesting-consent-for-an-entire-tenant"></a>Kérő felhasználó hozzájárul ahhoz a teljes bérlői webhelyen

Gyakran, ha a szervezet egy licenc vagy -előfizetésre az alkalmazás vásárol, kívánt teljesen hozhatók létre az alkalmazottak.  Ez a folyamat részeként a vállalati rendszergazda adhat a felhasználó hozzájárul ahhoz, hogy az alkalmazás bármely alkalmazott nevében eljáró.  Egy teljes bérlői webhelyen engedély megadása, a szervezet alkalmazottai nem fog tapasztalni a a felhasználó hozzájárul ahhoz képernyője az alkalmazást.

Az alkalmazás képviselőjé hozzájárulása az összes felhasználó számára, a bérlői webhelyemen, használhatja a **felügyeleti beleegyezés végpontot**, leírása alább.

## <a name="admin-restricted-scopes"></a>Rendszergazdai tiltott tartományok

A Microsoft ökológiai bizonyos magas-jogosultsággal engedélyek jelölhető **admin korlátozott**.  Az ilyen hatókörök példája:

- Egy organizaion directory adatok beolvasása:`Directory.Read`
- Adatok írt egy szervezet címtárára:`Directory.ReadWrite`
- A szervezet címtárára olvasási a biztonsági csoportok:`Groups.Read.All`

A fogyasztói felhasználó egy alkalmazás hozzáférést ezeket az adatokat biztosíthat, miközben ugyanazok a bizalmas vállalati adatok hozzáférés engedélyezése a szervezeti felhasználók korlátozott.  Ha az alkalmazás a következő engedélyeket egyikéhez hozzáférést kér egy szervezeti felhasználótól, a felhasználó kap-hiba üzenet jelzi, hogy azok a Alkalmazásengedélyek hozzájárul a nem engedélyezett.

Ha az alkalmazás a következő admin korlátozott hatókörök azoknak a szervezeteknek hozzáférésre van szüksége, közvetlenül a vállalati rendszergazda is használja a **rendszergazdai beleegyezés végpontot**, leírása alább kérjék őket.

Ha egy adminstrator ezeket az engedélyeket a felügyeleti hozzájárulása végpont keresztül biztosít, hozzájárulása megkapja minden felhasználónak a bérlői webhelyemen fentebb ismertetett.

## <a name="using-the-admin-consent-endpoint"></a>A felügyeleti hozzájárulása végpont használata

Ezeket a lépéseket követve az alkalmazás fogja tudni az adott bérlői webhelyre, többek között a felügyeleti tiltott tartományok az összes felhasználó engedélyeinek gyűjtse össze.  Mintaként szolgáló kódot, amely az alábbi lépésekkel desribed megtekintéséhez a [felügyeleti tiltott tartományok minta](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2)vonatkoznak.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Az engedélyek az alkalmazás regisztrációs portálon kérése

- Nyissa meg az alkalmazás [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)vagy [-alkalmazás létrehozása](active-directory-v2-app-registration.md) , ha még nem tette meg.
- Keresse meg a **Microsoft Graph engedélyek** szakaszt, és hozzáadása az alkalmazáshoz szükséges engedélyeket.
- Feltétlenül **menti** az alkalmazás regisztráció

#### <a name="recommended-sign-the-user-into-your-app"></a>Javasolt: a felhasználó jelentkezzen be az alkalmazásba

A szokásos felépíteni a felügyeleti használó alkalmazás járul végpontot, amikor az alkalmazás egy lap vagy nézet, amely lehetővé teszi a rendszergazdát, hogy az alkalmazás engedélyek jóváhagyása lesz szüksége.  Ez a lap lehet az alkalmazás regisztrációs folyamat, az alkalmazás beállításokban vagy egy dedikált "Csatlakozás" folyamat része.  Sok esetben célszerű kattintva jelenítse meg az alkalmazáshoz "Csatlakozás" nézet csak azt követően felhasználó munkahelyi vagy iskolai Microsoft-fiókkal van bejelentkezve.

A felhasználónak bejelentkezés az alkalmazásba azonosítását teszi a organziation, amelyhez a felügyeleti tartozik, mielőtt mintaüzenetet felhasználva jóváhagyni a szükséges engedélyekkel.  Miközben, nem feltétlenül szükséges módon tud segítséget nyújtani a szervezeti felhasználók számára a további intuitív használhatóság létrehozása.  Jelentkezzen be a felhasználó, hajtsa végre a [2.0-s verzió protokoll oktatóanyagok](active-directory-v2-protocols.md).

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
| Bérlői webhelyen | szükséges | A címtár-bérlő engedély kérése kívánt.  Biztosítható, hogy a globálisan egyedi azonosítója vagy a felhasználóbarát név formátumát. |
| client_id | szükséges | Az alkalmazás azonosítója, hogy a regisztrációs portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) rendelve az alkalmazást. |
| redirect_uri | szükséges | A redirect_uri, amelyre a választ, és kezelheti az alkalmazás elküldését.  Azt pontosan egyeznie kell a regisztrálta a portálon redirect_uris közül. |
| állam | ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték.  Lehet, hogy a minden olyan tartalom, a kívánt karakterlánc.  Az alkalmazás a felhasználó állapotával kapcsolatos információk kódolását, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha az állapot szolgál. |

Ezen a ponton Azure Active Directory hivatkozási fogja, hogy csak egy Bérlői rendszergazda jelentkezzen be a kérés végrehajtása.  A rendszergazda kérni fogja jóváhagyni az összes engedélyeket, hogy az alkalmazás a regisztrációs portálon kérte. 

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

Miután a rendszergazda hozzájárulása végpont sikeres választ korábban kapott, az alkalmazás szerzett meg a kért engedélyeket.  Most már áthelyezheti az alakzatot a kívánt erőforrás token kérése az alábbiakban leírtak szerint.

## <a name="using-permissions"></a>Engedélyek használata

Után a felhasználó hozzájárul engedélyeket az alkalmazás, az alkalmazás a megjelenítő egyes minőségben erőforrás hozzáférése az alkalmazás hozzáférési jogkivonat szerezheti be.  Egy adott jogkivonat csak egy egyetlen resorce használható, de minden engedélyt, amely az alkalmazás az adott erőforrás megadták belül a kijelöléséhez kódolt lesz.  Szerezheti be egy hozzáférési jogkivonat, az alkalmazás végezheti el egy kérelmet a 2.0-s verzió jogkivonat végpont:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Az eredményül kapott jogkivonat ezután használható HTTP-kérelmek az erőforrás - biztos, hogy az állapotsor jelzi az erőforráshoz, hogy az alkalmazás rendelkezik a megfelelő engedéllyel egy adott feladatnak a végrehajtásához.  

További részleteket az OAuth 2.0-s protokoll és hogyan szerezheti be a hozzáférési jogkivonat [2.0-s verzió végpont protokoll hivatkozást](active-directory-v2-protocols.md)talál.
