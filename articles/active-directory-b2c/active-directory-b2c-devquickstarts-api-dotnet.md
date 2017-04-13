<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Hogyan lehet egy .NET webes API készíthet az Azure Active Directory B2C, biztonságos OAuth 2.0-s hozzáférési jogkivonat hitelesítés használatával használatával."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: .NET webes API összeállítása

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Az Azure Active Directory (Azure Active Directory) B2C biztonságossá tétele a webes API OAuth 2.0-s hozzáférési jogkivonat alkalmazásával. Tokenek lehetővé teszi, hogy az ügyfél-alkalmazások által használt Azure Active Directory B2C az API-nak hitelesítést végezni. Ez a cikk bemutatja, hogyan hozhat létre egy .NET Model-vezérlő (MVC) "Feladatlista" API-val, amely lehetővé teszi a felhasználóknak CRUD feladatokat. A webes API-val védett Azure Active Directory B2C használ, és csak lehetővé teszi a felhasználóknak, hogy a feladatlista egyszerűbben kezelhető hitelesített.

## <a name="create-an-azure-ad-b2c-directory"></a>Az Azure Active Directory B2C könyvtár létrehozása

Azure Active Directory B2C használata előtt hozzon létre egy könyvtárat, vagy az bérlői. A könyvtár (az összes a felhasználók, alkalmazások, csoportok és az egyéb tároló). Ha egy már nem rendelkezik, a [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) előtt ebből az útmutatóból továbbra is.

## <a name="create-an-application"></a>Alkalmazás létrehozása

Ezután kell B2C címtárában-alkalmazás létrehozása. Ezzel kap, hogy szüksége van az alkalmazás biztonságos kommunikáció Azure AD-információkat. Az alkalmazás létrehozásához kövesse az [alábbi utasításokat](active-directory-b2c-app-registration.md). Győződjön meg arról, hogy:

- A **web App alkalmazást** vagy a **webes API** bele az alkalmazást.
- **Egységes erőforrás-azonosító átirányítás** használata `https://localhost:44316/` a webalkalmazásban. Ez a kódot a következő példában a web app ügyfélprogram alapértelmezett helye.
- Másolja az alkalmazás rendelt **azonosítója** . Újabb mobiltelefon szükséges.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>A házirendek létrehozása

Azure Active Directory B2C minden felhasználói felület [házirend](active-directory-b2c-reference-policies.md)van megadva. Az ügyfél, a kód mintában három identitás szolgáltatások tartalmazza: regisztráció, jelentkezzen be, és a profil szerkesztése elemre. Szüksége lesz az egyes:, házirend létrehozása a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)leírt módon. Amikor a három házirendek hoz létre, ne felejtse el:

- Válassza az identitás-szolgáltatók lap **Előfizetési felhasználói azonosító** és a **regisztrációs e-mailben** .
- Válassza ki a **megjelenítendő név** és a más előfizetési attribútumok a regisztrációs házirend.
- Minden házirend alkalmazás követelések, válassza a **megjelenítendő név** és a **Objektumazonosító** követelések. Megadhatja, hogy más igények mellé.
- Létrehozásuk után, másolja a vágólapra a minden szabály **nevét** . A házirend nevek újabb mobiltelefon szükséges.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a három házirendek sikeresen, készen áll az alkalmazás összeállítása.

## <a name="download-the-code"></a>Töltse le a kódot.

Ezen oktatóprogram [a GitHub karbantartása](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet)kódját. A minta készítéséhez menet közben is [.zip fájlként skeleton projekt letöltése](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Átmásolhatja a skeleton is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

A kész alkalmazás [.zip fájlként](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) is érhető el vagy a `complete` ugyanazt a tárházba részlege.

Miután letöltötte a példakódot, nyissa meg a Visual Studio .sln fájlt a kezdéshez. A megoldásfájlt két projektet tartalmaz: `TaskWebApp` és `TaskService`. `TaskWebApp`van egy olyan MVC webalkalmazást, amelyek a felhasználói. `TaskService`van az alkalmazás háttéradatbázist webes API-val, amely az egyes felhasználók teendőlista tárolja.

## <a name="configure-the-task-web-app"></a>A tevékenység web app konfigurálása

Amikor a felhasználók hogyan kommunikáljon a `TaskWebApp`, az ügyfél kérelem küld az Azure Active Directory és kapja vissza tokenek felhívására használható a `TaskService` webes API-val. Jelentkezzen be a felhasználó és tokenek, meg kell adnia `TaskWebApp` néhány információkkal, az alkalmazás. Az a `TaskWebApp` projekt, nyissa meg a `web.config` a legfelső szintű a projekt fájl- és az értékek lecserélése a `<appSettings>` szakaszban.  Hagyhatja, a `AadInstance`, `RedirectUri`, és `TaskServiceUrl` értékeinek-van.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Ez a cikk nem tárgyalja a épület a `TaskWebApp` ügyfél.  Megtudhatja, hogyan hozhat létre az Azure Active Directory B2C használatával webalkalmazást, nézze [meg a .NET web app](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Biztonságos az API

Amikor egy ügyfél, amely felhívja a API felhasználók nevében, biztonságos `TaskService` 2.0-s OAuth bearer tokenek használatával. A API elfogadása és a tokenek érvényesíteni a Microsoft Open Web Interface .NET (OWIN) tár használatával.

### <a name="install-owin"></a>OWIN telepítése
Első lépésként telepítenie kell a OWIN OAuth hitelesítési folyamat:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Adja meg a B2C adatait
Nyissa meg a `web.config` fájlt a legfelső szintű a `TaskService` project és a értékek cseréje a `<appSettings>` szakaszban. Ezeket az értékeket az API-val és a OWIN tár egész lesz.  Hagyhatja, a `AadInstance` érték változatlan marad.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Egy OWIN indítási osztály hozzáadása
Egy OWIN indítási osztály hozzáadása a `TaskService` nevű projekt `Startup.cs`.  Kattintson a jobb gombbal a projekten, válassza a **Hozzáadás gombra** , és **Új elem**, és keresse meg a OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0-s hitelesítés konfigurálása
Nyissa meg a fájlt `App_Start\Startup.Auth.cs`, és megvalósítása a `ConfigureAuth(...)` módszer:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>A tevékenység vezérlő biztonságos
Miután az alkalmazás OAuth 2.0-hitelesítés használatára van beállítva, biztonságossá teheti a webes API hozzáadásával egy `[Authorize]` a tevékenység vezérlőhöz címkét. Ez egy, a vezérlő, ahol minden Tennivaló lista kezelő történik, így kell biztonságossá tenni a teljes vezérlő osztály szintre. Is hozzáadhat a `[Authorize]` címkét a külön több szabályozási egyéni műveletek.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Felhasználói adatok beolvasása a token
`TasksController`Ha rendelkezik az egyes tevékenységek a egy társított felhasználójának "" a tevékenység adatbázisban tárolja. A tulajdonos azonosítja a felhasználó **Objektumazonosító**. (Emiatt a Objektumazonosító hozzáadása alkalmazásként van szüksége az összes a házirendek állítást.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>A minta alkalmazás futtatása

Végezetül készítése és futtatása mindkét `TaskWebApp` és `TaskService`. Regisztráció az alkalmazáshoz adott e-mail címről vagy felhasználónév használatával. Bizonyos feladatok létrehozása a felhasználó feladatlista, és figyelje meg, hogyan azok csoportosítások megmaradnak a API az ügyfél újraindítása után is.

## <a name="edit-your-policies"></a>A házirendek szerkesztése

Azure Active Directory B2C használatával van védelmének beállítása egy API-t, után meg az alkalmazás házirendek kísérletezni és az effektusok (vagy hiányzik, annak) az API. Kezelheti a házirendek az alkalmazás követelések, és módosítsa a felhasználói adatokat a webes API elérhető. Bármely követelések fel a .NET MVC webes API elérhetők lesznek a `ClaimsPrincipal` objektum, a korábban a jelen cikkben leírt módon.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
