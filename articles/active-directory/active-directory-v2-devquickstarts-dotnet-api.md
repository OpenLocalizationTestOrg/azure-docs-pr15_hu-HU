<properties
    pageTitle="Azure Active Directory 2.0-s verzió .NET webes API |} Microsoft Azure"
    description="A .NET MVC webes API-val, amely a két személyes Microsoft-Account tokenek elfogadja létrehozásának és munkahelyi vagy iskolai fiókokat."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Biztonságos egy MVC webes API

Az Azure Active Directory a 2.0-s verzió végpont védheti a webes API segítségével [OAuth 2.0-s](active-directory-v2-protocols.md#oauth2-authorization-code-flow) hozzáférési jogkivonat, engedélyezése a felhasználók személyes Microsoft-fiók és munkahelyi vagy iskolai fiók az biztonságos hozzáférés a webes API.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

ASP.NET webes API-hoz, az Ez használatával elvégezhető a Microsoft OWIN köztes .NET-keretrendszer 4.5 szerepel.  Itt használjuk OWIN össze egy "Teendőlista" MVC webes API-val, amely lehetővé teszi az ügyfelek hozhat létre, és olvassa el a feladatokat a felhasználó teendők listájából.  A webes API ellenőrzi, hogy bejövő felkérést tartalmazza az egy érvényes hozzáférési jogkivonat, és, amely nem megfelelnek az ellenőrzés olyan védett útvonalon visszautasítása.  Ez a példa készült Visual Studio 2015 használatával.

## <a name="download"></a>Letöltés
Ebben az oktatóanyagban kódját [a GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet)karbantartása.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) is vagy a skeleton klónozhatja:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

A skeleton alkalmazás egy egyszerű API az összes újrafelhasználható kódot tartalmazza, de az összes az identitás kapcsolatos darab hiányzik. Ha nem szeretné követheti, akkor ehelyett klónozhatja vagy [a kész minta letöltése](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Regisztráljon az alkalmazás
Új alkalmazás létrehozása a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [lépések részletes leírását](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- Másolás lefelé **Azonosítója** rendelt az alkalmazást, szüksége van rá hamarosan.

A visual studio megoldás is tartalmaz, a "TodoListClient", amely egy egyszerű WPF alkalmazást.  A TodoListClient bemutatják, hogyan felhasználó jelek bővítményekkel telepített és hogyan ügyfél állíthatnak kérelmek a webes API-hoz használatos.  Ebben az esetben a TodoListClient, mind a TodoListService jelölik az azonos alkalmazást.  Ha szeretné beállítani a TodoListClient, tegye a következőt is:

- A **mobil** platform az alkalmazás hozzáadása


## <a name="install-owin"></a>OWIN telepítése

Most, hogy az alkalmazás a regisztráció után, be kell állítania az alkalmazást a 2.0-s verzió végpont annak érdekében, hogy a bejövő kérések és válaszok tokenek érvényesítése kommunikáció.

- Első lépésként nyissa meg azt a megoldást, és adja hozzá a OWIN köztes NuGet csomagok a csomag Manager konzolon TodoListService projekthez.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuth hitelesítés konfigurálása

- Egy OWIN indítási osztály hozzáadása a TodoListService projekt nevű `Startup.cs`.  Kattintson a Projekt--> **Hozzáadás**jobb --> **Új elem** keresése "OWIN"-->.  A OWIN köztes meghívják a `Configuration(…)` módszer, amikor elindítja az alkalmazást.
- Az osztály nyilatkozat módosítása `public partial class Startup` -azt korábban már szerepelni fog az osztály részét meg másik fájlra.  Az a `Configuration(…)` módszer, a webalkalmazás hitelesítés beállítása ConfgureAuth(...) hívást legyen.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítása a `ConfigureAuth(…)` módszer, fogadja el a token származhat a 2.0-s verzió végpont fog beállítása a webes API-t.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Most használható `[Authorize]` védheti a vezérlők, és a műveletek 2.0-s OAuth bearer hitelesítéssel attribútumok.  Díszítése a `Controllers\TodoListController.cs` osztály-engedélyezése címkével.  Ekkor a felhasználót, hogy jelentkezzen be az adott lap megnyitása előtt.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Amikor egy meghatalmazott hívó sikeresen elindítja az egyik a `TodoListController` API-hoz, a művelet lehet szükségük a hívó adatait.  OWIN belül az bearer jogkivonathoz keresztül követelések hozzáférést biztosít a `ClaimsPrincpal` objektumot.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Végül, nyissa meg a `web.config` a legfelső szintű a TodoListService projekt fájlt, és adja meg a konfigurációs értékét a `<appSettings>` szakaszban.
  - A `ida:Audience` a portálon megadott az alkalmazás **Azonosítója** .

## <a name="configure-the-client-app"></a>Az ügyfél app konfigurálása
Megjelenik a Teendők lista szolgáltatás működését, mielőtt kell a Teendők lista ügyfélprogram konfigurálását, így tokenek kinyerése a 2.0-s verzió végpont és a hívásokat a szolgáltatás.

- Nyissa meg a TodoListClient projekt `App.config` , és adja meg a konfigurációs értékét a `<appSettings>` szakaszban.
  - A `ida:ClientId` másolta a portálról azonosítója.

Végezetül törlése, összeállítása, és futtassa az egyes projektek!  Ekkor a .NET MVC webes API-val, amely token származhat mindkét személyes Microsoft-fiók és a munkahelyi vagy iskolai fiók elfogadja.  Jelentkezzen be a TodoListClient, és hívja fel a webes API-t a felhasználó Teendőlista vehet fel tevékenységeket.

Hivatkozások [formájában érhető el az alábbi egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip)kész példa (nélkül a konfigurációs értékek), illetve klónozhatja, a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Következő lépések
További témakörök alakzatot most léphet.  Előfordulhat, hogy szeretne kipróbálni:

[A webes API mobilról webalkalmazást >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

További források olvassa el:
- [A 2.0-s verzió Fejlesztőeszközök útmutató >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active Directoryval" címke >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések beszerzése termékei

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók az [erre a lapra](https://technet.microsoft.com/security/dd252948) , és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
