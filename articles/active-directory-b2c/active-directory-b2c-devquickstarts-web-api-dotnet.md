<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Egy webalkalmazás, amely felhívja a webes API segítségével az Azure Active Directory B2C létrehozásának módját."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure Active Directory B2C: A webes API hívja a .NET-webalkalmazásból

Azure Active Directory (Azure Active Directory) B2C használatával hatékony önkiszolgáló identitás kezelése szolgáltatások hozzáadása a web Apps alkalmazások és a webes API-khoz néhány rövid lépéseket. Ez a cikk fog megvitatása hogyan hozhat létre, amely felhívja a webes API segítségével bearer tokenek .NET Model-vezérlő (MVC) "Feladatlista" webalkalmazást

Ez a cikk nem tárgyalja a bejelentkezési, előfizetési megvalósításáról és az Azure Active Directory B2C profilok kezelése. A felhasználó már hitelesítése után összpontosít hívó webes API-khoz. Ha még nem tette meg, érdemes együtt kezd [.NET web app bevezetés oktatóprogram](active-directory-b2c-devquickstarts-web-dotnet.md) Ha többet szeretne tudni az Azure Active Directory B2C alapjai.

## <a name="get-an-azure-ad-b2c-directory"></a>Ismerkedés az Azure Active Directory B2C címtár

Azure Active Directory B2C használata előtt hozzon létre egy könyvtárat, vagy az bérlői.  A könyvtár (az összes felhasználó, alkalmazások, csoportok és az egyéb tároló).  Ha egy már nem rendelkezik, a [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) előtt ebből az útmutatóból továbbra is.

## <a name="create-an-application"></a>Alkalmazás létrehozása

Ezután kell B2C címtárában-alkalmazás létrehozása. Ezzel kap, hogy szüksége van az alkalmazás biztonságos kommunikáció Azure AD-információkat. Ebben az esetben a web app és a webes API fog képviseli egyetlen **Azonosítója**, mert egy logikai alkalmazás tartalmazzák. Az alkalmazás létrehozásához kövesse az [alábbi utasításokat](active-directory-b2c-app-registration.md). Győződjön meg arról, hogy:

- A **web app/webes API** bele az alkalmazást.
- Adja meg `https://localhost:44316/` **Válasz URL**-címként. Célszerű a kódot a következő példában az alapértelmezett URL-CÍMÉT.
- Másolja az alkalmazás rendelt **Azonosítója** . Is szüksége lesz a később.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>A házirendek létrehozása

Azure Active Directory B2C minden felhasználói felület [házirend](active-directory-b2c-reference-policies.md)van megadva. Ez a webalkalmazás tartalmaz három identitást szolgáltatások: regisztráció, jelentkezzen be, és a profil szerkesztése elemre. Kell minden típusú több házirend létrehozása a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)leírt módon. Amikor a három házirendek hoz létre, ne felejtse el:

- A **megjelenítendő név** és egyéb előfizetési attribútumai válassza az előfizetés házirend.
- Válassza a **megjelenítendő név** és a **Objektumazonosító** alkalmazás jogcímalapú minden házirend. Megadhatja, hogy más igények mellé.
- Létrehozásuk után, másolja a vágólapra a minden szabály **nevét** . Az előtag rendelkeznie kell `b2c_1_`. Házirend neveket újabb mobiltelefon szükséges.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a három házirendek, készen áll az alkalmazás összeállítása.

Megjegyzés: Ez a cikk nem tárgyalja használatáról az imént létrehozott házirendeket. Hogyan működnek a házirendek az Azure Active Directory B2C kapcsolatos további tudnivalókért kezdje az [oktatóprogram .NET web app az első lépések](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Töltse le a kódot.

Ezen oktatóprogram [a GitHub karbantartása](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet)kódját. A minta létrehozásához a menet közben, akkor [Töltse le a skeleton projekt .zip fájlként](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Átmásolhatja a skeleton is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

A kész alkalmazás [.zip fájlként](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) is érhető el vagy a `complete` ugyanazt a tárházba részlege.

Miután letöltötte a példakódot, nyissa meg a Visual Studio .sln fájlt a kezdéshez.

## <a name="configure-the-task-web-app"></a>A tevékenység web app konfigurálása

Az első `TaskWebApp` kommunikálni az Azure Active Directory B2C, meg kell adnia néhány gyakori paramétereket. Az a `TaskWebApp` projekt, nyissa meg a `web.config` a legfelső szintű a projekt fájl- és az értékek lecserélése a `<appSettings>` szakaszban. Hagyhatja, a `AadInstance`, `RedirectUri`, és `TaskServiceUrl` értékek változatlanok maradnak.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Első hozzáférési jogkivonat, és hívja fel a tevékenység API

Ez a szakasz bemutatja, hogyan lehet a jogkivonat, jelentkezzen be Azure Active Directory B2C érkezett használata a webes API-val, amely titkosítani is az Azure Active Directory B2C elérése érdekében.

Ez a cikk nem tárgyalja a API védelme részleteit. Megtudhatja, hogyan webes API biztonságosan hitelesíti kérések Azure Active Directory B2C használatával, tanulmányozása: a [webes API első lépések a cikk](active-directory-b2c-devquickstarts-api-dotnet.md)

### <a name="save-the-sign-in-token"></a>A bejelentkezési jogkivonat mentése

Először hitelesítését (használatával a házirendek közül bármelyik), és a bejelentkezési jogkivonat kapott Azure Active Directory B2C.  Ha nem biztos abban, hogy hogyan házirendek hajtható végre, térjen vissza, és próbálkozzon a [.NET web app bevezetés oktatóprogram](active-directory-b2c-devquickstarts-web-dotnet.md) Ha többet szeretne tudni az Azure Active Directory B2C alapjai.

Nyissa meg a fájlt `App_Start\Startup.Auth.cs`.  Egy fontos változás következik kell tenni a `OpenIdConnectAuthenticationOptions` -Önnek kell beállítania `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Kapcsolatfelvétel egy token vezérlők

A `TasksController` a felelős a kommunikáció a webes API-t, a HTTP-kérések küld az API olvasása, létrehozása és törölheti a tevékenységeket.  Azure Active Directory B2C védi az API-e Becuase, kell először beolvasni a jogkivonat, a fenti lépést a mentett.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

A `BootstrapContext` jogkivonat beszerzett hajtja végre a B2C házirendek egyikét a bejelentkezési tartalmazza.

### <a name="read-tasks-from-the-web-api"></a>Olvassa el a tevékenységek API-t az internetről

Ha egy jogkivonat, akkor csatolhat azt a HTTP `GET` kérheti a `Authorization` biztonságosan hívja fel az élőfej `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Hozzon létre, és törölheti a tevékenységeket a webes API

Hajtsa végre az azonos típusúak küldésekor `POST` és `DELETE` kérelmek a webes API, használata a `BootstrapContext` beolvasásához jogkivonat a bejelentkezés. A létrehozás művelet meg végrehajtani azt. Próbálja meg a törlési művelet befejezése `TasksController.cs`.

## <a name="run-the-sample-app"></a>A minta alkalmazás futtatása

Végezetül össze, és futtassa az alkalmazást. Jelentkezzen, és jelentkezzen be, és a bejelentkezett felhasználó létrehozása. Jelentkezzen ki, és egy másik felhasználó, jelentkezzen be. Az adott felhasználó-feladatok létrehozása. Hogyan a feladatok tárolt felhasználónkénti az API-t a program, mert az API-t a felhasználói azonosító olvas a token kap.

Kész példa [.zip fájlként megadva](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip)hivatkozás. Is átmásolhatja azt a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
