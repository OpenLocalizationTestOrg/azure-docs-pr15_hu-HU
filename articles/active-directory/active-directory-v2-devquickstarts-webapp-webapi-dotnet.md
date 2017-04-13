<properties
    pageTitle="Azure Active Directory 2.0-s verzió – webalkalmazás .NET |} Microsoft Azure"
    description="Létrehozásának .NET MVC webalkalmazást a hívások web services használja személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókokat bejelentkezés."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Hívja fel a webes API .NET-webalkalmazásból

A 2.0-s verzió végponttal gyorsan a web Apps alkalmazások hitelesítési hozzáadása és webes API-khoz támogatása mindkét személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókokat.  Ebben az esetben azt fogja, hogy a felhasználók OpenID csatlakozni, használata a Microsoft OWIN köztes néhány közreműködése bejelentkezik MVC webes alkalmazás összeállítása.  A web app OAuth 2.0-s hozzáférési jogkivonat beszerzése a webes api OAuth 2.0-s verziója, amely lehetővé teszi, hogy védett létrehozása, olvasása, és törlése az adott felhasználó "teendőlista".

Ebben az oktatóanyagban összpontosít elsősorban MSAL használatával hozzáférési jogkivonat szerezheti be és a teljes [Itt](active-directory-v2-flows.md#web-apps)ismertetett webalkalmazást.  Előfeltételek, mint az első megtudhatja, érdemes hozzáadásuk [egyszerű jelentkezzen be az - webalkalmazást](active-directory-v2-devquickstarts-dotnet-web.md) , vagy hogyan [megfelelően a webes API biztonságos](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="download-sample-code"></a>Példakódot letöltése

Ebben az oktatóanyagban kódját [a GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet)megmarad.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) is vagy a skeleton klónozhatja:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Azt is megteheti, akkor [Töltse le az elvégzett alkalmazást egy .zip,](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) vagy a kész alkalmazás klónozhatja:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Regisztráljon az alkalmazás
Új alkalmazás létrehozása a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [lépések részletes leírását](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- Másolás lefelé **Azonosítója** rendelt az alkalmazást, szüksége van rá hamarosan.
- Az **Alkalmazás titkos** **jelszó** -típus létrehozása és későbbre lefelé értékének másolása
- A **webes** platform az alkalmazás hozzáadása
- Írja be a megfelelő **URI irányítja**. Az átirányítási uri azt jelzi, hogy hol kell irányítja a hitelesítési válaszok - ebben az oktatóanyagban alapértelmezés szerint Azure Active Directory `https://localhost:44326/`.


## <a name="install-owin"></a>OWIN telepítése
Adja hozzá az OWIN köztes NuGet csomagokat, hogy a `TodoList-WebApp` csomag Manager konzol használata a project.  A OWIN köztes használt bejelentkezési és kijelentkezési kérések hiba kezelése munkamenetet a felhasználó és adatainak megtekintése a felhasználó számára, többek között.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>A felhasználónak bejelentkezés
Most már állítsa be a OWIN köztes a [Csatlakozás OpenID hitelesítési protokoll](active-directory-v2-protocols.md#openid-connect-sign-in-flow)használatára.  

-   Nyissa meg a `web.config` fájlt a legfelső szintű a `TodoList-WebApp` project, és adja meg az alkalmazás konfigurációs értékét a `<appSettings>` szakaszban.
    -   A `ida:ClientId` az alkalmazás a regisztrációs portálon rendelt **Azonosítója** .
    - A `ida:ClientSecret` hozta létre a regisztrációs portál **Alkalmazás titkos kulcs** van.
    -   A `ida:RedirectUri` a portálon megadott **Átirányítás Uri** van.
- Nyissa meg a `web.config` fájlt a legfelső szintű a `TodoList-Service` project, és cserélje le a `ida:Audience` fentivel azonos **Azonosítója** .


- Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és vegyen fel `using` azon tárakhoz, a fenti utasításokat.
- Ugyanannak a fájlnak a végrehajtja a `ConfigureAuth(...)` módot.  Az Ön megadja a paraméterek `OpenIDConnectAuthenticationOptions` fog kommunikálni az Azure Active Directory az alkalmazás koordináták lesz.

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
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Hozzáférési jogkivonat MSAL használata
Az a `AuthorizationCodeReceived` értesítés, szeretnénk a authorization_code egy hozzáférési jogkivonat, a teendősáv lista szolgáltatás-beváltásához [együtt OpenID csatlakozás OAuth 2.0-s](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) használatával.  MSAL teheti ezt a folyamatot könnyen meg:

- Első lépésként MSAL előzetes verziójának telepítése:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- És vegyen fel egy másik `using` utasítást a `App_Start\Startup.Auth.cs` MSAL fájlt.
- Most adjon hozzá egy új módszer a `OnAuthorizationCodeReceived` eseménykezelő.  A kezelő fogja használni a MSAL szerezheti be egy hozzáférési jogkivonat, a teendősáv lista API-hoz, és tárolja a token MSAL's jogkivonat gyorsítótárban későbbre:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- Web Apps alkalmazások MSAL tartalmaz egy bővíthető jogkivonat gyorsítótár tokenek tárolásához használható.  Ez a példa végrehajtja a `NaiveSessionCache` http munkamenet tároló gyorsítótár tokenek használó.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Hívja fel a webes API
Most pedig a 3 vásárolta meg az access_token ténylegesen használni.  Nyissa meg a web app `Controllers\TodoListController.cs` fájl, amely lehetővé teszi a CRUD-kérelmek a teendősáv lista API-hoz.

- Később MSAL újra felhasználhatja itt-ból access_tokens a MSAL gyorsítótárból.  Első lépésként vegyen fel egy `using` MSAL nyilatkozata a fájlt.

    `using Microsoft.Identity.Client;`

- Az a `Index` művelet, használata MSAL `AcquireTokenSilentAsync` módszert get-access_token adatok beolvasása a feladatlista szolgáltatás használható:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- A minta hozzáadja az eredményül kapott jogkivonat a HTTP GET kérés, mint a `Authorization` fejléc, a feladatlista szolgáltatást használó hitelesítést végezni a kérést.
- Ha a feladatlista szolgáltatás eredménye egy `401 Unauthorized` válasz, a access_tokens a MSAL váltak érvénytelen valamilyen okból.  Ebben az esetben célszerű bármely access_tokens húzhatja a MSAL gyorsítótárból és megjelenítése a felhasználó, jelentkezzen be újra, amelyek újraindul a token WIA folyamat szükségük üzenet.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Hasonlóképpen ha MSAL kérdezhetők le egy access_token valamilyen okból, kell kérje meg a felhasználót, hogy jelentkezzen be újra.  Ez a egyszerűek, mint bármelyik kifogására `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- A pontos azonos `AcquireTokenSilentAsync` implementd a hívás a `Create` és `Delete` műveletek.  A web Apps alkalmazások Ez a módszer MSAL access_tokens megszerezni, amikor az alkalmazásban kell is használhatja.  MSAL beszerzésekor, gyorsítótár és frissítése a tokenek meg fog gondoskodik.

Végezetül továbbépítése és az alkalmazás futtatásához!  Jelentkezzen be Microsoft-Account vagy az Azure Active Directory-fiókjába, és figyelje meg, hogyan a felhasználói azonosító tükröződik a felső navigációs sávon.  Elemek hozzáadása és törlése néhány megtekintéséhez az OAuth 2.0 védett API-hívások működését a felhasználó teendők listájából.  Ekkor rendelkezésére áll egy web app és a webes API-val, mindkét védett iparágban szabványos protokollok, a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók hitelesíthet használatával.

A hivatkozás, az [itt megadott](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip)kész példa (nélkül a konfigurációs értékeket).  

## <a name="next-steps"></a>Következő lépések

További források olvassa el:
- [A 2.0-s verzió Fejlesztőeszközök útmutató >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active Directoryval" címke >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések beszerzése termékei

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók az [erre a lapra](https://technet.microsoft.com/security/dd252948) , és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
