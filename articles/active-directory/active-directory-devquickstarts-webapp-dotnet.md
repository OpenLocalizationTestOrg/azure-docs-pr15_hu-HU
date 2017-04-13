<properties
    pageTitle="Első lépések Azure AD-.NET |} Microsoft Azure"
    description="A .NET MVC Web App alkalmazásban, amely integrálódik az Azure Active Directory való bejelentkezéshez létrehozásának módját."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.NET Web App bejelentkezés és kijelentkezés az Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure Active Directory teszi egyszerű és a webalkalmazás az Identitáskezelés, kihelyező egyszerű nyújtó egyetlen bejelentkezési és a csak néhány kódsorokat kijelentkezési.  Asp.NET-webalkalmazások esetében az ennek használatával elvégezhető a Microsoft .NET-keretrendszer 4.5 szereplő közösségi-alapú OWIN köztes végrehajtása.  Itt a OWIN be:
-   Jelentkezzen be a felhasználó Azure AD a identitásszolgáltató használata az alkalmazásba.
-   A felhasználó információkat megjeleníteni.
-   Jelentkezzen be a felhasználó ki az alkalmazást.

Annak érdekében, hogy ehhez kell:

1. Az alkalmazások regisztrálhatja az Azure Active Directory
2. Állítsa be az alkalmazás a OWIN hitelesítési folyamat.
3. OWIN segítségével bejelentkezési és kijelentkezési kérések Azure AD ki.
4. Nyomtassa ki a felhasználó adatait.

Az első lépések, [Töltse le az alkalmazás skeleton](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) vagy [a kész minta letöltése](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Egy Azure AD-bérlő, amelyben az alkalmazás rögzítése is szüksége lesz.  Ha nincs telepítve egyik már, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. regisztrálja Azure AD-alkalmazás*
Ahhoz, hogy a felhasználók hitelesítést végezni az alkalmazást, először kell egy új alkalmazás regisztrálhatja a bérlői webhelyén.

- Jelentkezzen be az Azure adatkezelési portálra.
- A bal oldali navigációs sávon kattintson **Az Active Directory**.
- Jelölje ki a bérlő, ha ki szeretne regisztrálni az alkalmazást.
- Kattintson az **alkalmazások** fülre, és kattintson a Hozzáadás a alsó fiókban.
- Az útmutatást követve hozzon létre egy új **webalkalmazás és/vagy WebAPI**.
    - Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   A **Bejelentkezési URL** az alkalmazás alap URL-CÍMÉT.  A skeleton alapértelmezett `https://localhost:44320/`.
    - Az **Alkalmazás azonosítója URI** az alkalmazás egy egyedi azonosítót.  A kiállítás környezetbe `https://<tenant-domain>/<app-name>`, például`https://contoso.onmicrosoft.com/my-first-aad-app`
- Ha befejezte a regisztrációs, AAD rendel az alkalmazás egy egyedi ügyfél-azonosító.  Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a beállítás lapon.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. Állítsa be az alkalmazás a OWIN hitelesítési folyamat*
Ebben az esetben azt fogja állítsa be a OWIN köztes a csatlakozás OpenID hitelesítési protokoll használatára.  OWIN bejelentkezési és kijelentkezési kérések hiba kezelése munkamenetet a felhasználó és a felhasználó számára, többek között adatainak használatával.

-   A kezdéshez a OWIN köztes NuGet csomagok hozzáadása a projekthez, a csomag Manager konzolon.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Egy OWIN indítási osztály hozzáadása a projekthez című `Startup.cs` megfelelő, kattintson a Projekt--> **Hozzáadás** --> **Új elem** keresése "OWIN"-->.  A OWIN köztes meghívják a `Configuration(...)` módszer, amikor elindítja az alkalmazást.
-   Az osztály nyilatkozat módosítása `public partial class Startup` -azt korábban már szerepelni fog az osztály részét meg másik fájlra.  Az a `Configuration(...)` módszer, legyen ConfgureAuth(...) hívást a webalkalmazás hitelesítés beállítása  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítása a `ConfigureAuth(...)` módot.  Az Ön megadja a paraméterek `OpenIDConnectAuthenticationOptions` fog kommunikálni az Azure Active Directory az alkalmazás koordináták lesz.  Állítsa be a cookie-hitelesítés is kell – a OpenID csatlakozás köztes cookie-kat, a fedőlap alatt használ.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Végül, nyissa meg a `web.config` a legfelső szintű a projekt fájlt, és adja meg a konfigurációs értékét a `<appSettings>` szakaszban.
    -   Az alkalmazás `ida:ClientId` a másolt az Azure portálról, az 1 GUID.
    -   A `ida:Tenant` a Azure AD-bérlő, például "contoso.onmicrosoft.com" neve.
    -   A `ida:PostLogoutRedirectUri` jelzi az Azure AD, amikor a felhasználó szeretné irányítani kijelentkezési kérés tud befejeződése után.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. Kattintson OWIN bejelentkezési és kijelentkezési kérések kiadását az Azure Active Directory*
Az alkalmazás a hitelesítési OpenID csatlakozás protokollal Azure Active Directory kommunikáció most megfelelően beállítva.  OWIN van készített érdekli, az összes hitelesítési üzeneteket létrehozásával, a érvényesítése token származhat Azure Active Directory és a felhasználói munkamenet fenntartása szép részleteit.  Az összes, hogy továbbra is a felhasználók számára a bejelentkezés és kijelentkezés lehetőséget.

- Használhatja a vezérlők csak egy bizonyos lap megnyitása előtt, hogy a felhasználó bejelentkezik, a címkék engedélyezése.  Nyissa meg `Controllers\HomeController.cs`, és adja hozzá a `[Authorize]` a névjegy vezérlőhöz címkét.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   OWIN közvetlenül kibocsátása érkező hitelesítési kérések a kód belül is használhatja.  Nyissa meg `Controllers\AccountController.cs`.  A SignIn() és SignOut() műveletek adnak a kérdés OpenID csatlakozás és kijelentkezési kérelmeket, ki.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Ezután nyissa meg a `Views\Shared\_LoginPartial.cshtml`.  Az, ahol fogja megjeleníteni a felhasználó bejelentkezési és kijelentkezési hivatkozások az alkalmazást, és nyomtassa ki a felhasználó nevét egy nézetben.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="4--display-user-information"></a>*4. a felhasználói adatok megjelenítése*
Csatlakozás OpenID rendelkező felhasználók hitelesítő, amikor Azure AD-id_token az alkalmazás, amely tartalmazza a "követelések", és a felhasználó kapcsolatos előfeltételek adja eredményül.  Ezek követelések is használhatja az alkalmazás testreszabása:

- Nyissa meg a `Controllers\HomeController.cs` fájlt.  A vezérlők keresztül érheti el a felhasználó követelések a `ClaimsPrincipal.Current` rendszerbiztonsági.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Végezetül továbbépítése és az alkalmazás futtatásához!  Ha még nem tette meg, ezt követően hozzon létre egy új felhasználót a bérlői webhelyén, az idő a *. onmicrosoft.com végződésű tartományt.  Jelentkezzen be a felhasználó számára, és figyelje meg, hogyan a felhasználói azonosító tükröződik a felső navigációs sávon.  Jelentkezzen ki, és jelentkezzen be újra a bérlői webhelyemen más felhasználó.  Ha kifejezetten ambiciózus esetén kicsit, regisztrálni, és működésben alkalmazással (a saját clientId), és a Megtekintés lásd: egyszeri bejelentkezés egy másik példányát futtatni.

Az [itt megadott](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip)kész példa (nélkül a konfigurációs értékek) hivatkozást.  

Most már áthelyezheti Speciális témakörök alakzatot.  Előfordulhat, hogy szeretne kipróbálni:

[A webes API az Azure Active Directory biztonságos >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
