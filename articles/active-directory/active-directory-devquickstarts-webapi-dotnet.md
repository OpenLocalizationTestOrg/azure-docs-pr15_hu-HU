<properties
    pageTitle="Első lépések Azure AD-.NET |} Microsoft Azure"
    description="A .NET MVC webes API-val, amely integrálódik az Azure Active Directory-hitelesítés és az engedélyezés létrehozásának módját."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>A webes API Bearer tokenek az Azure Active Directory használata védelme

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ha meg kell, hogy ezek az erőforrások védelmét a jogosulatlan hozzáférés védett erőforrásokhoz való hozzáférést biztosító alkalmazás szeretne összeállítani.
Azure Active Directory teszi egyszerű és védelmét a webes API csak néhány kódsorokat OAuth Bearer 2.0-s hozzáférési jogkivonat használata egyszerű.

Asp.NET-webalkalmazások esetében az ennek használatával elvégezhető a Microsoft .NET-keretrendszer 4.5 szereplő közösségi-alapú OWIN köztes végrehajtása.  Itt használjuk, hogy OWIN összeállítása "Teendőlista" webes API-val, amely:
-   Jelöl ki, mely API-e védelemmel ellátva.
-   Ellenőrzi, hogy érvényes hozzáférési jogkivonat tartalmaz-e a webes API-hívások.

Annak érdekében, hogy ehhez kell:

1. Azure AD-alkalmazás regisztrálása
2. Állítsa be az alkalmazás a OWIN hitelesítési folyamat.
3. Ha fel szeretne hívni a kattintva tegye listában webes API ügyfélalkalmazás konfigurálása

Az első lépések, [Töltse le az alkalmazás skeleton](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) vagy [a kész minta letöltése](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Minden egyes a Visual Studio 2013 megoldást.  Egy Azure AD-bérlő, amelyben az alkalmazás rögzítése is szüksége lesz.  Ha nincs telepítve egyik már, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. regisztrálja Azure AD-alkalmazás*
Az alkalmazás biztonságos, először kell-alkalmazás létrehozása a bérlői webhelyén, és Azure Active Directory biztosítson néhány kulcsfontosságú adatokat.

-   Jelentkezzen be az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com)
-   A bal oldali navigációs sávon kattintson **Az Active** Directory
-   Jelölje ki a bérlői webhelyet, ahol az alkalmazás regisztrálni.
-   Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra az alsó fiókban.
-   Az útmutatást követve hozzon létre egy új **webalkalmazás és/vagy WebAPI**.
    -   Az alkalmazás **neve** leírja a végfelhasználók számára alkalmazást.  Írja be a "Teendőlista szolgáltatás".
    -   Az **Átirányítás Uri** az Office-téma és karakterlánc kombinációja, amellyel Azure AD vissza bármely tokenek az alkalmazást kérte használható. Adja meg `https://localhost:44321/` ehhez az értékhez.
-   Regisztrációs végrehajtása után, nyissa meg azt a **beállítás** lapon, és keresse meg az **Alkalmazás azonosítója URI** mezőt.  Írja be például bérlői-specifikus azonosítóját ezt az értéket`https://contoso.onmicrosoft.com/TodoListService`
- A konfiguráció mentéséhez.  Hagyja nyitva a portálon – hamarosan regisztráljon az ügyfélalkalmazás is kell.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. Állítsa be az alkalmazás a OWIN hitelesítési folyamat*

Most, hogy az alkalmazás Azure AD a regisztráció után, be kell állítania az Azure Active Directory annak érdekében, hogy a bejövő kérések és válaszok tokenek érvényesítése kommunikáció alkalmazás.

-   Első lépésként nyissa meg azt a megoldást, és adja hozzá a OWIN köztes NuGet csomagok a csomag Manager konzolon TodoListService projekthez.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Egy OWIN indítási osztály hozzáadása a TodoListService projekt nevű `Startup.cs`.  Kattintson a Projekt--> **Hozzáadás**jobb --> **Új elem** keresése "OWIN"-->.  A OWIN köztes meghívják a `Configuration(…)` módszer, amikor elindítja az alkalmazást.
-   Az osztály nyilatkozat módosítása `public partial class Startup` -azt korábban már szerepelni fog az osztály részét meg másik fájlra.  Az a `Configuration(…)` módszer, a webalkalmazás hitelesítés beállítása ConfgureAuth(...) hívást legyen.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítása a `ConfigureAuth(…)` módot.  Az Ön megadja a paraméterek `WindowsAzureActiveDirectoryBearerAuthenticationOptions` fog kommunikálni az Azure Active Directory az alkalmazás koordináták lesz.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Most használható `[Authorize]` védheti a vezérlők, és a műveletek JWT bearer hitelesítéssel attribútumok.  Díszítése a `Controllers\TodoListController.cs` osztály-engedélyezése címkével.  Ekkor a felhasználót, hogy jelentkezzen be az adott lap megnyitása előtt.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Amikor egy meghatalmazott hívó sikeresen elindítja az egyik a `TodoListController` API-hoz, a művelet lehet szükségük a hívó adatait.  OWIN a jogcímeken belül az bearer jogkivonathoz keresztül hozzáférést biztosít a `ClaimsPrincpal` objektumot.  
- Webes API-khoz közös követelmény azt javasoljuk, hogy a "hatókörök" szerepel a token érvényesítése – Ez biztosítja, hogy a végfelhasználói hozzájárult a teendők listában szolgáltatás eléréséhez szükséges engedélyek:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Végül, nyissa meg a `web.config` a legfelső szintű a TodoListService projekt fájlt, és adja meg a konfigurációs értékét a `<appSettings>` szakaszban.
  - A `ida:Tenant` a Azure AD-bérlő, például "contoso.onmicrosoft.com" neve.
  - A `ida:Audience` van az alkalmazás azonosítója URI az alkalmazás megadott az Azure-portálon.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. a ügyfélalkalmazás konfigurálása és a szolgáltatás futtatása*
Megjelenik a Teendők lista szolgáltatás működését, mielőtt kell a Teendők lista ügyfélprogram konfigurálását, így tokenek beolvasása AAD és a hívásokat a szolgáltatás.

- Nyissa meg újra az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com)
- Új alkalmazás létrehozása az Azure Active Directory-ös bérlőben, és válassza a **Natív ügyfélalkalmazás** az eredményül kapott parancssorában.
    -   Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   Adja meg `http://TodoListClient/` az **Átirányítás Uri** érték.
- Regisztráció végrehajtása után, AAD rendel az alkalmazás egy egyedi **Ügyfél-azonosító**. Ez az érték van szüksége a következő lépésekben, így másolja a vágólapra a beállítás lapon.
- Is **konfigurálása** lapon keresse meg a "Engedélyeket szeretne egyéb alkalmazások" szakaszt. Kattintson a "Alkalmazás hozzáadása". Válassza a "Az összes alkalmazás" a "Megjelenítése" tartalmazó legördülő listára, és kattintson a felső pipára. Keresse meg és kattintson a a tegye listában szolgáltatás elemre, és kattintson az alsó jelölőnégyzet be van jelölve az alkalmazás hozzáadása lehetőséget. Jelölje ki a "Hozzáférés tegye lista szolgáltatás" a "Meghatalmazott engedélyeit" legördülő listából, és mentse a konfigurációt.


- A Visual Studióban, nyissa meg a `App.config` a TodoListClient a projekt, és adja meg a konfigurációs értékét a `<appSettings>` szakaszban.
  - A `ida:Tenant` a Azure AD-bérlő, például "contoso.onmicrosoft.com" neve.
  - A `ida:ClientId` az Azure portálról másolt alkalmazás azonosítója.
  - A `todo:TodoListResourceId` van az alkalmazás azonosítója URI való tegye lista szolgáltatásalkalmazás megadott az Azure-portálon.

Végezetül törlése, összeállítása, és futtassa az egyes projektek!  Ha még nem tette meg, ezt követően hozzon létre egy új felhasználót a bérlői webhelyén, az idő a *. onmicrosoft.com végződésű tartományt.  A teendőlista ügyfélnek adott felhasználónak bejelentkezés, és bizonyos feladatok hozzáadása a felhasználó Teendőlista.

A kész minta (nélkül a konfigurációs értékek) megadva hivatkozás, az [alábbi](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Most már áthelyezheti további további identitás esetek.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
