<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Hogyan hozhat létre, amelynek előfizetési, a bejelentkezési webes alkalmazáshoz, és a jelszó alaphelyzetbe állításához az Azure Active Directory B2C használatával."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure Active Directory B2C: Előfizetési és a bejelentkezéskor ASP.NET webalkalmazást

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure Active Directory) B2C használatával hatékony önkiszolgáló identitás-felügyeleti funkciókat is adhat a web App alkalmazásban a rövid néhány lépést. Ez a cikk fog megvitatása hogyan hozhat létre, amely magában foglalja a felhasználó-előfizetési bejelentkezési ASP.NET webes alkalmazást, és a jelszó alaphelyzetbe állítása. Az alkalmazás támogatni fogja előfizetési és a bejelentkezési felhasználónév vagy e-mailek használatával, és a közösségi fiókból, például a Facebook- és a Google segítségével.

Ebben az oktatóanyagban különbözik [a többi .NET webes oktatóprogram](active-directory-b2c-devquickstarts-web-dotnet.md) , hogy a felhasználó regisztrálásának biztosítására és a bejelentkezés egy-egy gomb, használja a két helyett használja a [regisztráció vagy bejelentkezés házirend](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) (egy előfizetési és egy bejelentkezési).  Az egy nutshell a feliratkozás vagy házirend bejelentkezés lehetővé teszi a felhasználóknak, hogy a bejelentkezés egy meglévő fiókkal van, vagy hozzon létre egy újat, ha az azok az alkalmazás használata első alkalommal.

## <a name="get-an-azure-ad-b2c-directory"></a>Ismerkedés az Azure Active Directory B2C címtár

Azure Active Directory B2C használata előtt hozzon létre egy könyvtárat, vagy az bérlői. A könyvtár (az összes a felhasználók, alkalmazások, csoportok és az egyéb tároló).  Ha egy már nem rendelkezik, a [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) előtt ebből az útmutatóból továbbra is.

## <a name="create-an-application"></a>Alkalmazás létrehozása

Ezután kell B2C címtárában-alkalmazás létrehozása. Ezzel kap, hogy szüksége van az alkalmazás biztonságos kommunikáció Azure AD-információkat. Az alkalmazás létrehozásához kövesse az [alábbi utasításokat](active-directory-b2c-app-registration.md).  Győződjön meg arról, hogy:

- A **web app/webes API** bele az alkalmazást.
- Adja meg `https://localhost:44316/` mint **URI átirányítása**. Célszerű a kódot a következő példában az alapértelmezett URL-CÍMÉT.
- Másolja le az alkalmazás rendelt **Azonosítója** .  Később lesz szüksége.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>A házirendek létrehozása

Azure Active Directory B2C minden felhasználói felület [házirend](active-directory-b2c-reference-policies.md)van megadva. A kód minta tartalmaz két identitás-változat: **előfizetési és a bejelentkezési**, és a **jelszó alaphelyzetbe állítása**.  Meg kell hozzon létre egy házirendet, minden típusú [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md)ismertetett módon. A két házirendek létrehozásakor ügyeljen arra, hogy:

- Válassza a **Felhasználói azonosító regisztráció** vagy a **regisztrációs e-mailek** az identitás-szolgáltatók lap.
- A **megjelenítendő név** és egyéb előfizetési attribútumai válassza az előfizetés és a bejelentkezési házirend.
- Válassza ki a **megjelenítendő név** állítást alkalmazásként minden házirend állítást. Megadhatja, valamint egyéb követelések.
- Létrehozásuk után, másolja a vágólapra a minden szabály **nevét** . Címzett számára házirend újabb mobiltelefon szükséges.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a két házirendek, készen áll az alkalmazás összeállítása.

## <a name="download-the-code-and-configure-authentication"></a>Töltse le a kódot, és a hitelesítés konfigurálása

Ez [a GitHub karbantartása](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI)minta kódját. A minta létrehozásához a menet közben, akkor [Töltse le a skeleton projekt .zip fájlként](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Átmásolhatja a skeleton is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

A kész minta [.zip fájlként](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) is érhető el vagy a `complete` ugyanazt a tárházba részlege.

Miután letöltötte a példakódot, nyissa meg a Visual Studio .sln fájlt a kezdéshez.

Az alkalmazás kommunikál Azure Active Directory B2C küld a HTTP-hitelesítési kérések, adja meg a házirend kíván végrehajtani a kérelem részeként. .NET webalkalmazások esetében is használhatja a Microsoft OWIN tár OpenID csatlakozás hitelesítési kérések küldése, hajtsa végre a házirendek, felhasználói munkamenetek stb.

A kezdéshez a OWIN köztes NuGet csomagok hozzáadása a project által a Visual Studio csomag Manager konzolon.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Ezután nyissa meg a `web.config` a legfelső szintű a projekt fájl, és adja meg az alkalmazás konfigurációs értékét a `<appSettings>` alá eső értékek cseréje a saját csoportjában.  Hagyhatja, a `ida:RedirectUri` és a `ida:AadInstance` értékek hasonlóan változatlan marad.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Ezután egy OWIN indítási osztály hozzáadása a projekthez, nevű `Startup.cs`. Kattintson a jobb gombbal a projekten, válassza a **Hozzáadás gombra** , és **Új elem**, és keresse meg a "OWIN." Az osztály nyilatkozat módosítása `public partial class Startup`. Azt egy másik fájlt meg végrehajtani az osztály részét. A OWIN köztes meghívják a `Configuration(...)` módszer, amikor elindítja az alkalmazást. Ez a módszer a hívás `ConfigureAuth(...)`, ahol beállítása hitelesítési az alkalmazás.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítása a `ConfigureAuth(...)` módot.  Az Ön megadja a paraméterek `OpenIdConnectAuthenticationOptions` Azure Active Directory kommunikálni az alkalmazás koordináták lesz. Akkor is be kell állítania cookie-hitelesítés. A csatlakozás OpenID köztes cookie-kat felhasználói esetében, többek között megőrzéséhez használ.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Azure Active Directory hitelesítési kérések küldése
Az alkalmazás letöltése megfelelően konfigurálva Azure Active Directory B2C kommunikálni az OpenID csatlakozás hitelesítési protokoll használatával.  OWIN van készített érdekli, az összes hitelesítési üzeneteket létrehozásával, a érvényesítése token származhat Azure Active Directory és a felhasználói munkamenet fenntartása részleteit.  Továbbra is, hogy minden felhasználó munkafolyamat indítására.

Ha egy felhasználó kijelöli a **bejelentkezési** vagy **Elfelejtette jelszavát?** a web App alkalmazásban, a kapcsolódó művelet a meghívott `Controllers\AccountController.cs`. Minden esetben indíthatja el a megfelelő házirend beépített OWIN módszereket használhatja:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

A feliratkozás végrehajtása során vagy jelentkezzen be a házirendet, a felhasználó rendelkezik-e lehetőség a kattintson egy **Elfelejtette jelszavát?** hivatkozásra.  Ebben az esetben az Azure Active Directory B2C küldi el az alkalmazást, jelezve, hogy egy adott hibaüzenet akkor kell végrehajtani jelszó alaphelyzetbe állítása házirend.  Ezt a hibát az rögzítheti `Startup.Auth.cs` használata a `AuthenticationFailed` értesítési:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Explicit módon meghívása egy házirendet, mellett használhatja egy `[Authorize]` házirend hajtja végre, ha a felhasználó nincs bejelentkezve a vezérlők címkére. Nyissa meg `Controllers\HomeController.cs` és felvétele a `[Authorize]` a követelések vezérlőhöz címkét.  Az utoljára kiválasztott OWIN beállítva a házirend a `[Authorize]` címke találat van.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

OWIN segítségével jelentkezzen ki a felhasználót a alkalmazásból. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Felhasználói adatok megjelenítése
OpenID kapcsolódás használatával hitelesíti a felhasználókat, amikor a Azure AD-azonosító token az alkalmazást, amely tartalmazza a **követelések**adja eredményül. Ezek a felhasználó kapcsolatos előfeltételek. Az alkalmazás testreszabásához követelések is használhatja.  

Nyissa meg a `Controllers\HomeController.cs` fájlt. A vezérlők keresztül érheti el felhasználói követelések a `ClaimsPrincipal.Current` rendszerbiztonsági.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Bármely állítás, hogy az alkalmazás napokon megegyező módon érheti el.  Kap az alkalmazás az összes jogcímeken listájának érhető el, a **Jogcímeken** lapon.

## <a name="run-the-sample-app"></a>A minta alkalmazás futtatása

Végül össze, és futtassa az alkalmazást. Fizessen elő az alkalmazás használatával egy e-mail címet vagy a felhasználó nevére. Jelentkezzen ki, és jelentkezzen be újra az azonos felhasználóként. A felhasználói profil szerkesztése elemre. Jelentkezzen ki, más néven regisztráció. Figyelje meg, hogy a **igények** lapon megjelenő információ felel meg az információkat a házirendek konfigurált.

## <a name="add-social-idps"></a>Közösségi IDPs hozzáadása

Az alkalmazás jelenleg csak az előfizetést, és a bejelentkezési felhasználó támogatja **helyi fiókok**használatával. Ezek a fiók B2C címtárában tárolt egy felhasználónevet és jelszót. Azure Active Directory B2C használatával más **Identitásszolgáltatók** (IDPs) támogatása bármelyik a kód megváltoztatása nélkül is hozzáadhat.

Közösségi IDPs hozzáadása az alkalmazáshoz, kezdje el az alábbi cikkekben részletes utasításait követve. Minden egyes IDP támogatja a kívánt frissítenie kell, hogy a rendszer egy alkalmazás regisztrálása, és egy ügyfél-azonosító beszerzése

- [Facebook-IDP beállítása](active-directory-b2c-setup-fb-app.md)
- [Google-IDP beállítása](active-directory-b2c-setup-goog-app.md)
- [Egy IDP Amazon beállítása](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn-IDP beállítása](active-directory-b2c-setup-li-app.md)

Miután felvette a Identitásszolgáltatók a B2C Directoryhoz, kell szerkesztése a három házirendek, amelyet fel szeretne venni az új IDPs mindegyike a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md)leírt módon. Miután mentette a házirendek, futtassa újra az alkalmazást.  Meg kell jelennie a bejelentkezési összegként új IDPs, és az egyes az identitás-előfizetési beállítások találkozik.

A házirendek kísérletezni, és tekintse meg az effektust a minta alkalmazásba. Hozzáadása vagy eltávolítása IDPs, alkalmazás követelések módosítására és -előfizetési attribútumok módosítása. Ki láthatja, hogy milyen házirendek, hitelesítési kérések és OWIN kötik össze kísérletezhet.

Kész példa (nélkül a konfigurációs értékek) [.zip fájlként megadva](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip)hivatkozás. Is átmásolhatja azt a GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
