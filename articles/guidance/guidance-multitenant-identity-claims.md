<properties
   pageTitle="Állítást-alapú identitások multitenant alkalmazásokban használata |} Microsoft Azure"
   description="Hogyan állítja a use kibocsátó érvényességi és engedélyezési"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Jogcímalapú identitások multitenant alkalmazások használata

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ez a cikk a [sorozat]része. Egy teljes [minta alkalmazás] sorozat olvashatja el is van.

## <a name="claims-in-azure-ad"></a>Az Azure Active Directory követelések

Amikor a felhasználó bejelentkezik, az Azure AD-azonosító jogkivonat, amelyben a felhasználó kapcsolatos igények küld. Igényt egyszerűen valamilyen információra, kulcs/érték párban kifejezett értéke. Ha például `email` = `bob@contoso.com`.  Követelések van kibocsátó &mdash; ebben az esetben az Azure Active Directory &mdash; Ez az a személy hitelesíti a felhasználót, majd a jogcímalapú létrehoz. Mivel a kibocsátó megbízható megbízik követelések. (Viszont a kibocsátó nem bízik meg, ha nem megbízható követelések!)

Magas szintű:

1.  A felhasználó hitelesíti.
2.  A IDP küld egy sor olyan követelések.
3.  Az alkalmazás normalizálja vagy augments a jogcímalapú (nem kötelező).
4.  Az alkalmazás a jogcímalapú engedélyezési döntéseket használja.

A OpenID csatlakozni a [hatókör paraméter] : a hitelesítési kérelmet, amely el követelések megadása határozza meg. Azonban az Azure Active Directory problémák korlátozta követelések keresztül OpenID kapcsolódás; [támogatott jogkivonat és állítást típusok]témakörben talál. Ha azt szeretné, hogy a felhasználó kapcsolatos további tudnivalók, kell az Azure Active Directory Graph API-t használja.

Íme néhány az, hogy az alkalmazás a szokásos előfordulhat, hogy érdeklő AAD követelések:

Írjon be azonosító token formál |    Leírás
-----------------------|--------------
és | Ki a jogkivonat adta ki. Ez lesz az alkalmazás ügyfél-azonosítóval. Általánosságban elmondható ne kell aggódnia amiatt, hogy ezt a kérelmet, mert a köztes automatikusan ellenőrzi. Példa:`"91464657-d17a-4327-91f3-2ed99386406f"`
csoportok   | Amelynek tagja a felhasználó AAD csoportok listája. Példa:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
iss  | A [kibocsátó] a OIDC jogkivonat. Példa:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
név    | A felhasználó megjelenítendő név. Példa:`"Alice A."`
objektumazonosító | A felhasználó AAD objektumazonosító. Ezt az értéket a felhasználó a megváltoztatható és zárolási azonosítója lesz. A érték, nem az e-mail használni egy egyedi azonosítót felhasználók; módosíthatja az e-mail címét. Ha az alkalmazás az Azure Active Directory Graph API-t használ, Objektumazonosító értéke, hogy használt lekérdezés profil adatait. Példa:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
szerepkörök   | A felhasználó alkalmazás szerepkörének felsorolása. Példa:`["SurveyCreator"]`
TID | Bérlői azonosítójával. Ez az érték egy egyedi azonosítót a bérlő Azure Active Directory. Példa:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | A felhasználó emberi olvasható megjelenítendő neve. Példa:`"alice@contoso.com"`
(UPN) | Egyszerű felhasználónév. Példa:`"alice@contoso.com"`

A táblázat felsorolja az igénylése a azonosító token megjelenő. ASP.NET Core 1.0 a csatlakozás OpenID köztes alakít át. állítást típusokat, amikor azt feltölti a követelések gyűjtemény a felhasználó egyszerű:

-   objektumazonosító >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   egyszerű felhasználónévi >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Követelések átalakítások

A hitelesítési folyamat során, előfordulhat, hogy módosítani kívánja, amely a IDP el követelések. ASP.NET Core 1.0 végezhető el a csatlakozás OpenID köztes követelések átalakítása **AuthenticationValidated** esemény belül. (Lásd: a [hitelesítési eseményeket].)

A munkamenet hitelesítési cookie **AuthenticationValidated** során felvett igényt tárolja. Ezek nem első tolódik vissza Azure AD.

Íme néhány példa a követelések átalakítása:

-   **Követelések normalizálás**, vagy a projekten követelések egységes felhasználók keresztül. Ez a különösen fontos, ha a több IDPs, előfordulhat, hogy felhasználhatja típusai eltérő állítást hasonló követelések.
Azure AD például a "upn" igényt, amely tartalmazza a felhasználó e-mail küld. Más IDPs egy "e" kérelmet küldhet. A következő kódot az "upn" állítást alakítja egy "e-mail" felelős:

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Az **alapértelmezett értékek formál** követelések, amelyek nem tartalmazzák az hozzáadása &mdash; például hozzárendelése egy felhasználóhoz alapértelmezett szerepkörbe. Bizonyos esetekben ez egyszerűsítheti engedélyezési összefüggés.
- **Egyéni állítást típusok** információkkal, az alkalmazás-specifikus a felhasználó hozzáadása A felhasználó információkat tartalmazhatnak például adatbázisban. Ezeket az adatokat egy egyéni igénylése a hitelesítési jegy hozzáadhat. A kérelmet csak akkor van szüksége az adatbázis bejelentkezési folyamat minden egyszer megtudhatja, hogy a cookie-k vannak tárolva. Kézzel is szeretné elkerülni túlzottan nagy cookie-k létrehozása, így a cookie-k méretét és az adatbázis-keresések között a csökkentés figyelembe kell.   

A hitelesítési folyamat befejezése után a jogcímalapú érhetők el a `HttpContext.User`. Ezen a ponton kezelje őket, csak olvasható gyűjtemény &mdash; például a segítségükkel engedélyezési döntéseket.

## <a name="issuer-validation"></a>Kibocsátó érvényesítése
A OpenID csatlakozni a kibocsátó igényt ("iss") a kiállító az azonosító jogkivonathoz IDP azonosítja. A OIDC hitelesítési folyamat részét képezi, ellenőrizze, hogy a kibocsátó állítást megfelel-e a tényleges kibocsátó. A OIDC köztes kezeli, ezt meg.

Az Azure Active Directory, a kibocsátó értéke egyedi AD bérlőnként (`https://sts.windows.net/<tenantID>`). Az alkalmazások emiatt ellenőrizze, hogy a kibocsátó jelöl, amely jogosult a bejelentkezés az alkalmazásba bérlői webhelyre, egy további ellenőrzése kell megtennie.

Egy egyetlen-bérlői alkalmazáshoz csak ellenőrizheti, hogy a kibocsátó-e a saját bérlői. Erre valójában a OIDC köztes automatikusan elvégzi ezt alapértelmezés szerint. Több elem bérlői alkalmazásban engedélyeznie kell a több kibocsátó, a különböző bérlők megfelelő. Az alábbiakban az általános megközelítése használata:

-   Az OIDC köztes beállításainak megadása **ValidateIssuer** hamis értékre. Ez kikapcsolja az automatikus jelölőnégyzetet.
-   Bérlői webhelyre feliratkozik, amikor a felhasználó DB tárolni a bérlő és a kibocsátó.
-   Valahányszor a felhasználó bejelentkezik, keresse meg a kibocsátó az adatbázisban. Ha a kibocsátó nem található, az azt jelenti, azt nem regisztrált. Őket a megfelelő bejelentkezési oldal irányíthatja át.
-  Akkor is sikerült blacklist bizonyos bérlők; például az ügyfeleknek, hogy az előfizetése nem fizet.

Erről részletesen tárgyalja, [előfizetési és multitenant alkalmazásban bevezetési bérlői][signup].

## <a name="using-claims-for-authorization"></a>Engedély iránti kérelmek használata

Követelések a felhasználói azonosító már nem egy egységes személyhez. A felhasználó lehet például az e-mail címét, telefonszámát, Születésnapi, gender stb. A felhasználó IDP esetleg összes ezeket az adatokat tárolja. De amikor a felhasználó azonosítására, általában kapja meg követelések ezek egy részét. Ebben a modellben a felhasználói azonosító egyszerűen a az első lépésekhez követelések. Engedélyezési kapcsolatos döntések felhasználó, akkor fog keresni követelések adott készleteket. Más szóval, a kérdés végül "Nem felhasználói X van igény Z" "X felhasználó műveleteket végezhet Y" lesz.

Íme néhány alapvető mintázatok ellenőrzésének claims.

-  Ellenőrzése, hogy a felhasználó rendelkezik-e egy adott értéket az egy adott állítást:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Ez a kód ellenőrzi, hogy a felhasználó rendelkezik-e a "Rendszergazdai" értékű szerepkör igényt. Helyesen kezeli az esetben, ha a felhasználónak nincs szerepkör állítást vagy több szerepkör követelések.

    A **ClaimTypes** osztály a gyakran használt állítást típusnál állandók határozza meg. Azonban a állítást típus bármely karakterláncérték is használhatja.

-   Egy adott értéket legfeljebb tudjanak várt állítást típusra, lehető egyetlen értéket:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Az összes érték beszerzése állítást típus:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

További tudnivalókért lásd: a [szerepkör - és erőforrás-alapú hitelesítés multitenant alkalmazásokban][authorization].

## <a name="next-steps"></a>Következő lépések

- Olvassa el a sorozat következő cikkét: [előfizetési és multitenant alkalmazásban bevezetési bérlői][signup]
- További tudnivalók [Jogcímalapú hitelesítés] ASP.NET Core 1.0 dokumentációjában

<!-- Links -->
[sorozaton kívüli]: guidance-multitenant-identity.md
[hatókör paraméter]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Támogatott jogkivonat, és formál típusai]: ../active-directory/active-directory-token-and-claims.md
[kibocsátó]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Hitelesítési események]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Jogcímalapú hitelesítés]: https://docs.asp.net/en/latest/security/authorization/claims.html
[minta alkalmazás]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
