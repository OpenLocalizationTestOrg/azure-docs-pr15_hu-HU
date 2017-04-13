<properties
    pageTitle="Azure Active Directory 2.0-s verzió – webalkalmazás .NET |} Microsoft Azure"
    description="A .NET MVC webalkalmazásból, amelyeket a felhasználók be mindkét személyes Microsoft-Account bejelentkezik létrehozásának és munkahelyi vagy iskolai fiókokat."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Bejelentkezési felvétele egy .NET MVC web App alkalmazásba

A 2.0-s verzió végponttal hitelesítési a web Apps alkalmazások támogatásával az mindkét személyes Microsoft-fiók és a munkahelyi vagy iskolai fiók gyorsan adhat hozzá.  ASP.NET-webalkalmazások esetében az ennek használatával elvégezhető a Microsoft OWIN köztes .NET-keretrendszer 4.5 szerepel.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

 Itt azt fogja a felhasználónak bejelentkezés, megjelenítése a felhasználó információkat, és jelentkezzen ki az alkalmazás a felhasználó OWIN használó webes alkalmazás összeállítása.
 
 ## <a name="download"></a>Letöltés
Ebben az oktatóanyagban kódját [a GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet)karbantartása.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) is vagy a skeleton klónozhatja:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

A kész alkalmazást is ez az oktatóanyag végén megadva.

## <a name="register-an-app"></a>Regisztráljon az alkalmazás
Új alkalmazás létrehozása a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [lépések részletes leírását](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- Másolás lefelé **Azonosítója** rendelt az alkalmazást, szüksége van rá hamarosan.
- A **webes** platform az alkalmazás hozzáadása
- Írja be a megfelelő **URI irányítja**. Az átirányítás uri azt jelzi, hogy hol kell irányítja a hitelesítési válaszok - ebben az oktatóanyagban alapértelmezés szerint Azure Active Directory `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Telepítse és OWIN hitelesítés konfigurálása
Ebben az esetben azt fogja állítsa be a OWIN köztes a csatlakozás OpenID hitelesítési protokoll használatára.  OWIN bejelentkezési és kijelentkezési kérések hiba kezelése munkamenetet a felhasználó és a felhasználó számára, többek között adatainak használatával.

-   Első lépésként nyissa meg a `web.config` a legfelső szintű a projekt fájlt, és adja meg az alkalmazás konfigurációs értékét a `<appSettings>` szakaszban.
    -   A `ida:ClientId` az alkalmazás a regisztrációs portálon rendelt **Azonosítója** .
    -   A `ida:RedirectUri` a portálon megadott **Átirányítás Uri** van.

-   Ezután a OWIN köztes NuGet csomagok hozzáadása a projekthez, a csomag Manager konzolon.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Egy "OWIN indítási osztály" a projekt neve hozzáadása `Startup.cs` megfelelő, kattintson a Projekt--> **Hozzáadás** --> **Új elem** keresése "OWIN"-->.  A OWIN köztes meghívják a `Configuration(...)` módszer, amikor elindítja az alkalmazást.
-   Az osztály nyilatkozat módosítása `public partial class Startup` -azt korábban már szerepelni fog az osztály részét meg másik fájlra.  Az a `Configuration(...)` módszer, legyen ConfigureAuth(...) hívást a webalkalmazás hitelesítés beállítása  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítása a `ConfigureAuth(...)` módot.  Az Ön megadja a paraméterek `OpenIdConnectAuthenticationOptions` fog kommunikálni az Azure Active Directory az alkalmazás koordináták lesz.  Állítsa be a cookie-hitelesítés is kell – a OpenID csatlakozás köztes cookie-kat, a fedőlap alatt használ.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Hitelesítési kérések küldése
Az alkalmazás letöltése megfelelően konfigurálva a hitelesítési OpenID csatlakozás protokollal 2.0-s verzió végpont kommunikálni.  OWIN van készített érdekli, az összes hitelesítési üzeneteket létrehozásával, a érvényesítése token származhat Azure Active Directory és a felhasználói munkamenet fenntartása szép részleteit.  Az összes, hogy továbbra is a felhasználók számára a bejelentkezés és kijelentkezés lehetőséget.

- Használhatja a vezérlők csak egy bizonyos lap megnyitása előtt, hogy a felhasználó bejelentkezik, a címkék engedélyezése.  Nyissa meg `Controllers\HomeController.cs`, és adja hozzá a `[Authorize]` a névjegy vezérlőhöz címkét.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   OWIN közvetlenül kibocsátása érkező hitelesítési kérések a kód belül is használhatja.  Nyissa meg `Controllers\AccountController.cs`.  A SignIn() és SignOut() műveletek adnak a OpenID csatlakozás kérdés és kijelentkezési kérelmeket, ki.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Ezután nyissa meg a `Views\Shared\_LoginPartial.cshtml`.  Az, ahol fogja megjeleníteni a felhasználó bejelentkezési és kijelentkezési hivatkozások az alkalmazást, és nyomtassa ki a felhasználó nevét egy nézetben.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
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

## <a name="display-user-information"></a>Felhasználói adatok megjelenítése
Csatlakozás OpenID rendelkező felhasználók hitelesítő, a 2.0-s verzió végpont az alkalmazást, amely tartalmazza a [követelések](active-directory-v2-tokens.md#id_tokens)és a felhasználó kapcsolatos előfeltételek egy id_token visszatér.  Ezek követelések is használhatja az alkalmazás testreszabása:

- Nyissa meg a `Controllers\HomeController.cs` fájlt.  A vezérlők keresztül érheti el a felhasználó követelések a `ClaimsPrincipal.Current` rendszerbiztonsági.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Futtatása

Végezetül továbbépítése és az alkalmazás futtatásához!   Jelentkezzen be akár személyes Microsoft-Account, akár egy munkahelyi vagy iskolai fiókjával, és figyelje meg, hogyan a felhasználói azonosító tükröződik a felső navigációs sávon.  Most már védett iparágban szabványos protokollokkal hitelesíthet a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók webalkalmazást.

Hivatkozások [formájában érhető el az alábbi egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip)kész példa (nélkül a konfigurációs értékek), illetve klónozhatja, a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Következő lépések

Most már áthelyezheti Speciális témakörök alakzatot.  Előfordulhat, hogy szeretne kipróbálni:

[A webes API a biztonságos a 2.0-s verzió végpont >>](active-directory-devquickstarts-webapi-dotnet.md)

További források olvassa el:
- [A 2.0-s verzió Fejlesztőeszközök útmutató >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active Directoryval" címke >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések beszerzése termékei

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók az [erre a lapra](https://technet.microsoft.com/security/dd252948) , és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
