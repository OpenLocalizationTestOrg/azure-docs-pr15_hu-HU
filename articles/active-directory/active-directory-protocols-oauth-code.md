<properties
    pageTitle="Azure Active Directory .NET protokoll – áttekintés |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan webalkalmazások és a webes API-hoz a bérlői webhelyén, az Azure Active Directory és OAuth 2.0-s való hozzáférés engedélyezése a HTTP-üzenetek használatával."
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


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Webalkalmazás az OAuth 2.0-s és Azure Active Directory számára a hozzáférés engedélyezése

Azure Active Directory (Azure Active Directory) használ, OAuth 2.0-s lehetővé teszi a webalkalmazások és a Azure AD-bérlő webes API-hoz való hozzáférés engedélyezése. Ez az útmutató független nyelvet, és ismerteti, hogyan lehet az üzenetek küldése és fogadása HTTP nélkül, a Megnyitás-forrás tárak közül.

A OAuth 2.0-s engedélyezési kód folyamat [az OAuth 2.0 specifikáció 4.1](https://tools.ietf.org/html/rfc6749#section-4.1) szakaszban leírt. A legtöbb alkalmazás típusú, beleértve a web Apps alkalmazások és a natív módon telepített alkalmazások hitelesítési és engedélyezési végrehajtásához használatos.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0-s engedélyezési folyamat

Magas szintű a teljes engedélyezési folyamat, az alkalmazás néz ki egy kicsit:

![OAuth Auth kód továbbításához](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Egy engedélyezési kód kérése

A kód engedélyezési folyamat kezdődik az ügyfél, a felhasználót, hogy mely irányítja a `/authorize` végpontot. Az ügyfél a kérést, azt mutatja, a szerezheti be a felhasználó a szükséges engedélyekkel. Azure klasszikus portál, az alkalmazás lapról a **Nézet végpontok** gombra az alsó fiókban OAuth 2.0-s végpontjait elérheti.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------- |
| Bérlői webhelyen | szükséges | A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték bérlői azonosítók, például `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` vagy `common` a bérlői beállítástól független tokenek |
| client_id | szükséges | Az alkalmazás azonosítója rendelt az alkalmazást, az Azure Active Directory regisztrálásakor. Az adatkezelési portálon Azure megtalálhatja. Kattintson **Az Active**Directory kattintson a címtár, kattintson az alkalmazásra, kattintson a **Configure** |
| response_type | szükséges | Tartalmaznia kell `code` az engedélyezés kód továbbításhoz. |
| redirect_uri | ajánlott | Az alkalmazást, ahol küldött és megkapta az alkalmazás a hitelesítési válaszok redirect_uri.  Azt pontosan egyeznie kell a regisztrálta a portálon, kivéve akkor kell lennie az URL-címként kódolandó redirect_uris közül.  A natív és mobil alkalmazás, az alapértelmezett értéket kell használni `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | ajánlott | Adja meg a módszert, amellyel az eredményül kapott jogkivonat vissza küldje el az alkalmazást.  Lehet `query` vagy `form_post`.  |
| állam | ajánlott | A kérelmet, amely a token válasz is lesznek visszaadva szereplő érték. Véletlenszerűen generált egyedi érték [webhelyközi kérelem hamisítása támadásokkal letiltom](http://tools.ietf.org/html/rfc6749#section-10.12)a szokásos szolgál.  Az állapot kódolás információt az alkalmazás a felhasználó állapotát, mielőtt a hitelesítési kérelem történt, például az oldal vagy a nézet, mintha is szolgál. |
| erőforrás | nem kötelező. | Az alkalmazás azonosítója URI a webes API-val (védett erőforrás). Az adatkezelési portálon Azure a webes API-val, az alkalmazás azonosítója URI-megkereséséhez kattintson **Az Active Directory**, kattintson a címtár, jelölje be az alkalmazás, és kattintson a **beállítás**. |
| figyelmeztető üzenet | nem kötelező. |  Adja meg a szükséges felhasználói beavatkozás típusát.<p> Az érvényes értékek a következők: <p> *Bejelentkezés*: A felhasználó arra kéri, hogy újra hitelesítést végezni. <p> *beleegyezés*: felhasználó hozzájárulása megadták, de frissítésre szorul. A felhasználó beleegyezés, kéri. <p> *admin_consent*: egy rendszergazdának arra kéri, hogy a nevében a szervezet minden tagjának beleegyezés |
| login_hint | nem kötelező. | Használható előre töltse ki a username/e-mail cím mezőbe, a bejelentkezési lapon a felhasználó számára, ha tudja, hogy a felhasználónév időszakokra.  Gyakran alkalmazásokat fogja használni a paraméter újbóli hitelesítést problémákat már kiolvasott felhasználónév egy előző bejelentkezési használata közben a `preferred_username` formál. |
| domain_hint | nem kötelező. | A bérlő vagy tartományt, a felhasználónak kell használatával jelentkezhetek be az emlékeztető biztosít. A domain_hint értéke a bérlő regisztrált tartományt. Ha a bérlő összevont legyen-e egy helyszíni Directoryhoz, AAD átirányítja az adott bérlői összevonási kiszolgáló. |

> [AZURE.NOTE] A felhasználók a szervezet része, ha a szervezet rendszergazdája beleegyezés vagy elutasíthatják a felhasználó nevében, vagy lehetővé teszik a felhasználó számára beleegyezés. A felhasználó kap a beállítás csak akkor, ha a rendszergazda lehetővé teszi, hogy beleegyezés.

Ezen a ponton a felhasználót a rendszer kéri a megadnia a hitelesítő adatait, és az engedélyeket a megjelölt beleegyezés az `scope` paraméteres lekérdezés. Amikor a felhasználó hitelesíti, és adja meg a felhasználó hozzájárul ahhoz Azure AD az alkalmazás, a választ küld a `redirect_uri` kérését címet.

### <a name="successful-response"></a>A sikeres válasz

A sikeres választ ábrája ilyen:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Paraméter | Leírás |
| -----------------------| --------------- |
| admin_consent | Az érték értéke igaz, ha a rendszergazda átadni kívánt hozzájárult e jóváhagyási kérelem kérdést.|
| kód | A engedélyezési kódot, amely kéri az alkalmazás. Az alkalmazás segítségével az engedélyezési kód kérése a cél erőforrás-jogkivonat. |
| session_state | Egy egyedi érték, amely azonosítja az aktuális felhasználói munkamenetet. Ez az érték egy globálisan egyedi azonosítója, de egy áttetsző vizsgálat nélkül átadott érték kell kezelni. |
| állam | Ha az Állapot paraméter szerepel a kérelmet, az azonos értékkel jelenjen meg a választ. Tanácsos az alkalmazáshoz, és ellenőrizze, hogy az állapot értékeket a kérelem és a válasz azonos a kérdésre adott választ használata előtt. Ezzel az elemzéssel feltárása a [Webhelyközi kérése hamisítása (CSRF) támadások](https://tools.ietf.org/html/rfc6749#section-10.12) elleni az ügyfél.  

### <a name="error-response"></a>Hiba válasz

Hibaüzenetek is küld a `redirect_uri` , hogy az alkalmazás képes kezelni őket megfelelően.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
|-----------|-------------|
| hibaüzenet | Kód hibaértéket szakasz 5,2 [OAuth 2.0-s engedélyezési keretrendszer](http://tools.ietf.org/html/rfc6749)definiálva. A következő táblázat az Azure AD eredményül adó hibakódok. |
| error_description | A hiba részletes leírását. Ez az üzenet nem célja, hogy kell végfelhasználói rövid. |
| állam | Az állapot értéke nem újrafelhasználható egy véletlenszerűen létrehozott érték, amely a kérelem küld, és a válasz, hogy a webhelyközi kérelem hamisítása (CSRF) támadások. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Használja a engedély kérése egy hozzáférési jogkivonat

Most, hogy már szerezte be az engedélyezés kód és a kapott jogosultságot a felhasználó által, felhasználói egy hozzáférési jogkivonat, a megfelelő erőforrásnézethez kódját bejegyzés kérelem küld a `/token` végpont:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Paraméter | | Leírás |
| ----------------------- | ------------------------------- | --------------------- |
| Bérlői webhelyen | szükséges |  A `{tenant}` elérési útját a kérelem értéke lehet bejelentkezni az alkalmazást, hogy kik használható.  A megengedett érték bérlői azonosítók, például `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` vagy `common` a bérlői beállítástól független tokenek |
| client_id | szükséges | Az alkalmazás azonosítója rendelt az alkalmazást, az Azure Active Directory regisztrálásakor. Az Azure klasszikus portálon megtalálhatja. Kattintson **Az Active**Directory kattintson a címtár, kattintson az alkalmazásra, kattintson a **Configure** |
| grant_type | szükséges | Meg kell `authorization_code` az engedélyezés kód továbbításhoz. |
| kód | szükséges | A `authorization_code` , amely vásárolta meg az előző szakaszban   |
| redirect_uri | szükséges | Azonos `redirect_uri` szerezheti be használt érték a `authorization_code`. |
| client_secret | web Apps alkalmazások szükséges | Az alkalmazás titkos az alkalmazást az app regisztrációs portálon létrehozásához.  Azt nem használandó natív alkalmazásban, mert client_secrets biztos, hogy nem tárolhatók eszközökön.  A web Apps alkalmazások és a webes API-hoz, amelyeket tárolni lehetősége van szükség a `client_secret` biztonságosan a kiszolgálóoldali. |
| erőforrás | Ha a megadott engedélyezési kódot kér, még nem kötelező szükséges | Az alkalmazás azonosítója URI a webes API-val (védett erőforrás).
Az adatkezelési portálon Azure az alkalmazás azonosítója URI megkereséséhez kattintson **Az Active Directory**, kattintson a címtár, jelölje be az alkalmazás, és kattintson a **beállítás**.

### <a name="successful-response"></a>A sikeres válasz

Azure AD egy hozzáférési jogkivonat, sikeres választ ad eredményül. Az ügyfélalkalmazás és a kapcsolódó késés hálózati hívásait minimalizálásához az ügyfélalkalmazás kell gyorsítótárba hozzáférési jogkivonat, amely az OAuth 2.0-s válasz megadott az élettartam. A következőképpen határozza meg az élettartam, a `expires_in` vagy `expires_on` paraméterértékeket.

Ha egy webes API-erőforrás eredménye egy `invalid_token` kódszámú hiba jelenik meg, ezzel jelezheti, hogy az erőforrás van határozza meg, hogy a token van-e le. Ha az ügyfél- és erőforrás-óra időpontokat különböző (az úgynevezett "egyszerre ferdeség"), az erőforrás fontolja meg a jogkivonat, mielőtt a token nincs bejelölve, az ügyfél gyorsítótárból lejárt. Ilyen esetben törölje a token a gyorsítótárból esetén is továbbra is a számított élettartama belül.

A sikeres választ ábrája ilyen:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| access_token | A kért hozzáférési jogkivonat. Az alkalmazás a token segítségével a védett erőforrás, például a webes API hitelesítést végezni. |
| token_type | Jogkivonat típusa értékét jelöli. Az egyetlen típus, amely támogatja az Azure Active Directory Bearer. Bearer tokenek kapcsolatos további tudnivalókért lásd: [OAuth2.0 engedélyezési keretrendszer: Bearer jogkivonat használatát (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Mennyi ideig a hozzáférési jogkivonat érvénytelen (másodpercben). |
| expires_on | A hozzáférési jogkivonat lejártakor idő. A dátum megfelelője másodpercek számát a 1970-01-01T0:0:0Z UTC addig, amíg a lejárati idő. Ez az érték használatos gyorsítótárazott tokenek időtartama határozza meg. |
| erőforrás | Az alkalmazás azonosítója URI a webes API-val (védett erőforrás).|
| hatókör | Megszemélyesítési engedélyeket az ügyfélalkalmazás. Az alapértelmezett engedély `user_impersonation`. A védett erőforrás tulajdonosa Azure Active Directory regisztrálhatja további értékeket.|
| refresh_token |  OAuth 2.0-s frissítés token. Az alkalmazás szerez további hozzáférési jogkivonat, az aktuális jogkivonat lejárta után a token használhatja.  Frissítés tokenek élettartamú, és amely megőrzi az erőforrásokhoz hosszabb ideig is használható. |
| id_token | Egy alá nem írt JSON webes jogkivonat (JWT). Az alkalmazás lehet base64Url dekódolását bejelentkezve felhasználó kérelem információt a token egyes részeit. Az alkalmazás, gyorsítótár-értékeket, és megjelenítheti őket, de azt kell támaszkodik őket bármely engedélyezés vagy a biztonsági határai. |

### <a name="jwt-token-claims"></a>JWT jogkivonat követelések
A JWT jogkivonat értékét a `id_token` paraméter is dekódolható a következő jogcímalapú:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

A `id_token` paraméter tartalmazza a következő típusú állítást. JSON webes tokenek kapcsolatos további tudnivalókért lásd [JWT IETF piszkozat specifikációja](http://go.microsoft.com/fwlink/?LinkId=392344). Többet szeretne tudni a típusok jogkivonat és követelések olvassa el az [jogkivonat támogatott és állítást típusai](active-directory-token-and-claims.md)

| Formál típusa | Leírás |
|------------|-------------|
| és | A célközönség a jogkivonat. A token egy ügyfélalkalmazás elküldésekor van-e a közönség a `client_id` az ügyfél.
| Exp | Lejárati idő. Az idő, a token lejártakor. A token lesz érvényes, az aktuális dátum/idő csak akkor kisebb vagy egyenlő a `exp` értéket. Az idő megfelelője másodpercek számát a 1970 január 1 (1970-01-01T0:0:0Z) UTC a időpontig a token adta-e ki. |
| family_name | Felhasználó Vezetéknév vagy vezetékneve. Az alkalmazás jeleníthet meg ezt az értéket. |
| given_name | Első felhasználónév. Az alkalmazás jeleníthet meg ezt az értéket. |
| IAT | Kiállított időben. Az idő, amikor a JWT adta ki. Az idő megfelelője másodpercek számát a 1970 január 1 (1970-01-01T0:0:0Z) UTC a időpontig a token adta-e ki. |
| iss | Azonosítja a token kibocsátó |
| NBF | Nem előtt ideje. Az idő, amikor a token változik hatékony. A jogkivonat lesz érvényes az aktuális dátum/idő nagyobb vagy egyenlő Nbf értékre kell lennie. Az idő megfelelője másodpercek számát a 1970 január 1 (1970-01-01T0:0:0Z) UTC a időpontig a token adta-e ki. |
| objektumazonosító | Objektum (azonosító) a user objektumban, az Azure Active Directory azonosítója. |
| Sub | Jogkivonat tárgy azonosítója. Ez a egy állandó és megváltoztatható azonosítót a token ismerteti, hogy a felhasználó számára. Ezt az értéket a gyorsítótár-logika használni. |
| TID | Azonosító () a az Azure AD-bérlő a token kiadott bérlői. |
| unique_name | Egy egyedi azonosítót, amely a felhasználó jeleníthetők meg. Ez általában az egyszerű felhasználónév (UDN). |
| (UPN) | A felhasználó egyszerű felhasználóneve. |
| ver | Verzió. A JWT jogkivonat, a szokásos 1.0 verziója. |

### <a name="error-response"></a>Hiba válasz

Jogkivonat kiállítási végpont olyan HTTP-hibakódok, mert a felhívja a token kiállítási végpont közvetlenül. A HTTP-állapotkód mellett az Azure Active Directory jogkivonat kiállítási végpontot is a JSON dokumentumokat olyan objektumok, amelyek leírják a hibát adja eredményül.

Egy minta hibaüzenetet ábrája ilyen:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba.  |
| error_codes | Diagnosztikai segítő STS adott hibakódok listája. |
| időbélyeg | Az idő, a hiba történt. |
| trace_id | A kérés diagnosztika segítő egyedi azonosítója  |
| correlation_id | A kérelem diagnosztika keresztül összetevők segítő egyedi azonosítója|

#### <a name="http-status-codes"></a>HTTP állapot kódok

Az alábbi táblázat a HTTP-állapot kódokat, amely a token kiadási végpont ad vissza. Bizonyos esetekben a hibakód: ahhoz, a válasz elegendő, de abban az esetben, ha hibák, meg kell elemzi a mellékelt JSON-dokumentumot, és vizsgálja meg a kódszámú hiba jelenik meg.

| HTTP-kód | Leírás |
|-----------|-------------|
| 400       | Alapértelmezett HTTP kódot. A legtöbb esetben használt, és a szokásos oka az, hogy a hibás kérés. Javítsa ki, és küldje el az összehívást. |
| 401       | Hitelesítés sikertelen volt. A kérés például a client_secret paraméter nincs megadva.|
| 403       | Nem sikerült engedélyezése. Például a felhasználónak nincs engedélye az erőforrás eléréséhez. |
| 500       | Belső hiba történt, a szolgáltatás. Ismételje meg a kérelmet. |

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

## <a name="use-the-access-token-to-access-the-resource"></a>A hozzáférési jogkivonat az erőforrás eléréséhez használt

Most, hogy sikeresen vásárolta az `access_token`, meg felvételével a webes API-khoz kérelmek a token is használhatja a `Authorization` fejléc. A [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) specifikációja HTTP-kérések bearer tokenek segítségével védett forrásokat ismerteti.

### <a name="sample-request"></a>Minta kérés

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Hiba válasz

Védett információforrások, amelyek a RFC 6750 probléma HTTP állapot kódok végrehajtása. Ha a kérelem nem része a hitelesítő vagy hiányzik a jogkivonat, a válasz tartalmazza-e egy `WWW-Authenticate` fejléc. Ha nem sikerül egy kérelmet, az erőforrás-kiszolgáló válaszol a egy HTTP-állapotkód és egy kódszámú hiba jelenik meg.

Az alábbi képen egy sikertelen válasz során az ügyfél-összehívás nem tartalmaz az bearer jogkivonathoz:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Hiba paraméterei

| Paraméter | Leírás |
|-----------|-------------|
| authorization_uri | A URI (fizikai végpont) engedélyezési kiszolgáló. Ez az érték is szolgál keresési kulcsként további információt a kiszolgáló kinyerése egy feltárás végpontot. <p><p> Az ügyfél ellenőriznie kell, hogy megbízható-e a engedélyezési kiszolgáló nem. Azure Active Directory védi az erőforrás, esetén elegendő, ellenőrizze, hogy az URL-cím https://login.windows.net vagy egy másik hostname (állomásnév), amely támogatja az Azure Active Directory kezdődik. Bérlői-specifikus erőforrás mindig térjen vissza a bérlői webhely-specifikus engedély URI. |
| hibaüzenet | Kód hibaértéket szakasz 5,2 [OAuth 2.0-s engedélyezési keretrendszer](http://tools.ietf.org/html/rfc6749)definiálva.|
| error_description | A hiba részletes leírását. Ez az üzenet nem célja, hogy kell végfelhasználói rövid.|
| erőforrás_azonosítója | Az egyedi azonosító az erőforrás lekérdezése Az ügyfélalkalmazás használata a az azonosító értékét a `resource` paraméter egy jogkivonat, az erőforrás kérelem során. <p><p> Nagyon fontos a ügyfélalkalmazáshoz ellenőrzéséhez ezt az értéket, egyéb rosszindulatú szolgáltatás lehet tudja **illetéktelen a jogosultság a** támadások jutva <p><p> A javasolt stratégiát létrehozását megakadályozó támadások annak ellenőrzésére, hogy a `resource_id` egyezik-e a webes API URL-cím alapjának használatban. Ha https://service.contoso.com/data használatban van, például a `resource_id` htttps://service.contoso.com/ is lehet. Az ügyfélalkalmazás kell utasítani egy `resource_id` , hogy ne kezdődjön az alap URL-cím kivéve, ha van egy megbízható alternatív módszer a azonosító ellenőrzéséhez. |

#### <a name="bearer-scheme-error-codes"></a>Bearer séma hibakódok esetén

A RFC 6750 specifikációja határozza meg, az alábbi hibák erőforrásokat, használja a WWW-hitelesítést végezni élőfej- és Bearer színséma használata a kérdésre adott választ.

| HTTP-állapotkód | Kódszámú hiba jelenik meg | Leírás | Ügyfél művelet |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | A kérés formátuma nem megfelelő. Például, előfordulhat, hogy paraméter hiányzó vagy kétszer az azonos paraméter használatával. | A hiba kijavításához, és ismételje meg a kérelmet. Hibaüzenet a következő típusú mi történjen, csak a fejlesztés alatt, és a kezdeti tesztelés az általuk észlelt. |
| 401 | invalid_token   | A hozzáférési jogkivonat hiányzik, érvénytelen és visszavonva. Az érték paraméter error_description további információt tartalmaz. |  Egy új token kérése a engedélyezési kiszolgálóról. Ha nem sikerül az új jogkivonat, a váratlan hiba történt. Hiba üzenet küldése a felhasználót, majd próbálkozzon újra véletlen késések után. |
| 403 | insufficient_scope | A hozzáférési jogkivonat nem tartalmaz az erőforrás eléréséhez szükséges engedélyek megszemélyesítési. | Egy új engedélyezési kérelmet küldhet az engedélyezés végpontot. Ha a válasz a hatókör paraméter tartalmazza, írja be az értekezlet-összehívást az erőforrás a a hatókör értéket. |
| 403 | insufficient_access | A tárgy a jogkivonat nem rendelkezik az erőforrás eléréséhez szükséges engedélyekről. | A felhasználótól másik fiókkal, vagy kérje az engedélyeket az adott erőforrás. |

## <a name="refreshing-the-access-tokens"></a>A hozzáférési jogkivonat frissítése

Hozzáférési jogkivonat rövid életű és frissíteni kell után továbbra is az erőforrások elérése járnak. Frissíthet a `access_token` által küldése egy másik `POST` kérése a `/token` végpontot, de ez alkalommal kezeléséről a `refresh_token` helyett a `code`.

Frissítés tokenek nem rendelkezik a megadott élettartama. A frissítés tokenek élettartamának általában viszonylag hosszú. Azonban bizonyos esetekben frissítés tokenek lejár, van függesztve vagy nem rendelkezik a megfelelő jogosultsággal a kívánt műveletet. Az alkalmazás a várt és kezelheti a hibát az jogkivonat kiállítási végpont helyesen kell.

Választ jogkivonat frissítési hiba jelenik meg, amikor a jelenlegi frissítés jogkivonat elvetése, és új engedélyezési kód kérése vagy jogkivonat eléréséhez. Különösen akkor, amikor frissítés használatával token a engedélyezési kód támogatási folyamat a Ha jelenik meg a választ a `interaction_required` vagy `invalid_grant` hibakódok vetni a frissítés jogkivonat, és új engedélyezési kód kérése.

Minta kéri a **bérlői webhely-specifikus** végpont (is használhatja az **közös** végpontot) első egy új hozzáférési jogkivonat használata egy frissítés token így néz ki:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Paraméter | Leírás |
|-----------|-------------|
| access_token | Az új jogkivonat kért.|
| expires_in   | A jogkivonat hátralévő élettartam másodpercben. Egy tipikus értéke 3600 (egy órával). |
| expires_on   | Dátum és idő a token jár le. A dátum megfelelője másodpercek számát a 1970-01-01T0:0:0Z UTC addig, amíg a lejárati idő. |
| refresh_token | Új OAuth 2.0-s refresh_token kérhet új hozzáférési jogkivonat, a válasz a lejártakor használható. |
| erőforrás     | A védett erőforrás, amely a hozzáférési jogkivonat azonosítja eléréséhez használt. |
| hatókör        | Megszemélyesítési engedélyeket a natív ügyfélalkalmazás. Az alapértelmezett engedély **user_impersonation**. A cél erőforrás tulajdonosa alternatív értékek regisztrálhatja Azure AD. |
| token_type   | A jogkivonat típusa. Az egyetlen támogatott érték **bearer**. |

### <a name="successful-response"></a>A sikeres válasz

A sikeres jogkivonat választ fog kinézni:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Hiba válasz

Egy minta hibaüzenetet ábrája ilyen:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Paraméter | Leírás |
| ----------------------- | ------------------------------- |
| hibaüzenet | Egy hiba kód karakterláncot, így a hibák típusú osztályozásához használható hibák reagálni is használható. |
| error_description | Egy adott hibaüzenet, amely segít a fejlesztők okának legfelső szintű hitelesítési hiba.  |
| error_codes | Diagnosztikai segítő STS adott hibakódok listája. |
| időbélyeg | Az idő, a hiba történt. |
| trace_id | A kérés diagnosztika segítő egyedi azonosítója  |
| correlation_id | A kérelem diagnosztika keresztül összetevők segítő egyedi azonosítója|

Hibakódok és a javasolt művelet leírását olvassa el [hibakódok jogkivonat végpont hibákat](#error-codes-for-token-endpoint-errors).
