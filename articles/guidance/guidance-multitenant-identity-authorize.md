<properties
   pageTitle="Engedély multitenant alkalmazásokban |} Microsoft Azure"
   description="Hogyan végezhetők el engedélyezési multitenant alkalmazásban"
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
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Szerepkör- és erőforrás-alapú hitelesítés multitenant alkalmazásokban

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ez a cikk a [sorozat]része. Egy teljes [minta alkalmazás] sorozat olvashatja el is van.

A [hivatkozás végrehajtása] ASP.NET Core 1.0 alkalmazásként jelenik meg. Ebben a cikkben áttekintjük engedélyezési, általános két megközelítés az engedély ASP.NET Core 1.0 leírt API-k használata.

-   **Szerepköralapú engedélyt**. A felhasználó szerepköreit alapján művelet engedélyezése. Ha például néhány műveletet szükség rendszergazdai szerepkör.
-   **Erőforrás-alapú hitelesítés**. Egy adott erőforrás alapján művelet engedélyezése. Ha például minden erőforrás van tulajdonosa. A tulajdonos törölheti az erőforrás; más felhasználók nem.

Egy tipikus alkalmazás fog alkalmazhat egy kettő vegyesen is. Ha például egy erőforrás törléséhez a felhasználónak kell lennie az erőforrás tulajdonos _vagy_ rendszergazda? lehetőséget.


## <a name="role-based-authorization"></a>Szerepkör-alapú hitelesítés

[Dejójáték Kft felmérések] [ Tailspin] alkalmazás a következő szerepkörök határozza meg:

- Rendszergazdaként. Az összes CRUD műveletek végezhetők a minden olyan felmérést, amelyhez az adott bérlői tartozik.
- Készítő. Új felmérések hozhat létre.
- Olvasó. Erről bármilyen felmérések által tartoznak

Az alkalmazás _felhasználói_ szerepkörök vonatkoznak. A felmérés alkalmazásban a felhasználó-rendszergazda, a létrehozó vagy a olvasó.

Vitafórum meghatározása és szerepkörök kezeléséhez az az [alkalmazás szerepkörök]című témakör tartalmaz.

Hogyan kezeli a szerepkörök, függetlenül a engedélyezési kód hasonló fog megjelenni. ASP.NET Core 1.0 vezet be [engedélyezési házirendek]nevű absztrakciója[policies]. Ezzel a szolgáltatással engedélyezési házirendek meghatározása kódot, és kattintson a vezérlő műveletek házirendekhez alkalmazása. A házirend az adatkezelő van leválasztott.

### <a name="create-policies"></a>Házirendek létrehozása

Szabályzat kialakítása, először hozzon létre egy osztály, amely `IAuthorizationRequirement`. Legcélszerűbb, ha származtatása `AuthorizationHandler`. Az a `Handle` módszer, vizsgálja meg a megfelelő claim(s).

Íme egy példa az Dejójáték Kft felmérések alkalmazásból:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Lásd: [SurveyCreatorRequirement.cs]

Az osztály a felhasználót, hogy hozzon létre egy új felmérés kötelező határozza meg. A felhasználó a SurveyAdmin vagy SurveyCreator szerepkör kell lennie.

Az indítási osztályához szabályzat kialakítása elnevezett, amely tartalmaz egy vagy több követelményeknek. Ha több követelményeket, a felhasználó kell teljesítenie _minden_ is engedélyezni kell. A következő kódot két házirendek határozza meg:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Lásd: [Startup.cs]

Ez a kód is ez az információ, mely hitelesítési köztes fusson, ha nem sikerül egy engedélyezési ASP.NET hitelesítési séma állítja be. Ebben az esetben azt adja meg a cookie-k hitelesítési köztes, mivel a cookie-k hitelesítési köztes a felhasználó átirányítása a "Tiltott" lap. A tiltott lap helyét a cookie-k köztes; a AccessDeniedPath beállításban van állítva. Lásd: [a hitelesítési köztes beállítása].

### <a name="authorize-controller-actions"></a>Engedélyezi a vezérlő műveletek

Végül egy MVC vezérlő művelet engedélyezése, beállítása a házirendet a `Authorize` attribútum:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

ASP.NET korábbi verzióiban a **szerepkörök** tulajdonság volna meg az attribútum:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

ASP.NET Core 1.0 továbbra is támogatja ezt, de van néhány hátrányai engedélyezési házirendek hasonlítani:

-   Azt feltételezi, hogy egy adott állítás típust. Házirendek állítást bármilyen ellenőrizni. Szerepkörök igénylése egy adott típusú.
-   A szerepkör neve csomagolásukkor a attribútum be. Egy helyről, van az engedélyezés logika házirendek egyszerűsítése frissítése vagy akár betöltése a beállításai.
-   Házirendek engedélyezése összetettebb engedélyezési döntéseket (például életkor > = 21), amely nem kell megadni egyszerű szerepkör-tagság szerint.

## <a name="resource-based-authorization"></a>Erőforrás-alapú hitelesítés

_Erőforrás-alapú hitelesítés_ következik be az engedélyezés attól függ, hogy egy adott erőforráson művelet érinti. Dejójáték Kft felmérések alkalmazásban a minden felmérés tulajdonos és nulla-a-többhöz munkatársak tartalmaz.

-   A tulajdonos olvashat, frissíthet, közzététele és közzétételének megszüntetése a felmérés.
-   A tulajdonos munkatársak rendelhet a felmérést.
-   Munkatársak tudja olvasni, és frissítse a felmérést.

Ne feledje, hogy "tulajdonos" és "közreműködői" nem alkalmazás szerepkörök; azok az adatbázis-alkalmazás felmérésekre tárolja. Annak ellenőrzéséhez, hogy a felhasználó törölhet felmérés, például az alkalmazás annak vizsgálata, hogy a felhasználó a tulajdonos, hogy a felmérés.

ASP.NET Core 1.0, az erőforrás-alapú hitelesítés súgójában **AuthorizationHandler** eredő és **kezelje a** módszer felülbírálása.

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Figyelje meg, hogy a az osztály erősen beírta felmérés objektumok.  Az osztály indításkor DI rögzítése:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Ellenőrzi engedélyt, használja a **IAuthorizationService** felületből, akkor a is a vezérlőket helyezhet el. A következő kódrészlet ellenőrzi, hogy a felhasználók olvashatják felmérés:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Mivel azt át a egy `Survey` objektum, a hívás meghívják a `SurveyAuthorizationHandler`.

A engedélyezési kód egy jó módszer, hogy az összesítő minden szerepkör- és erőforrás-alapú engedélyekkel a felhasználó, akkor az összesítés meg a kívánt művelet jelölőnégyzetet.
Íme egy példa a felmérések alkalmazásból. Az alkalmazás többféle jogosultsági határozza meg:

- Rendszergazda
- Közös munka
- Készítő
- Tulajdonos
- A képernyőolvasók

Az alkalmazás is készletét felmérések esetleges műveleteket:

- Hozzon létre
- Olvasás
- Frissítés
- Törlése
- Közzététel
- Unpublsh

A következő kódot hoz létre, ha egy adott felhasználói és a felmérés listáját. Figyelje meg, hogy ez a kód vizsgálja meg a felhasználó alkalmazás szerepkörök és a tulajdonos és közös munka a felmérés mezőket.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Lásd: [SurveyAuthorizationHandler.cs].

Több elem bérlői alkalmazásban gondoskodnia kell arról, hogy engedélyek nem "Előfordulhat" egy másik bérlői adatokat. A felmérés alkalmazásban a munkatárs jogosultsági lehetővé teszi a keresztül bérlők &mdash; , rendelhetnek valaki egy másik bérlői, mint egy contriubutor. A jogosultsági másfajta csak információforrások, amelyek az adott felhasználó bérlői tartoznak. Meg lehessen őrizni a követelmény, a kód előtt engedély ellenőrzi a bérlői azonosítója. (A `TenantId` a felmérés létrehozásakor hozzárendelt mező.)

A következő lépésként jelölje be az engedélyek elleni művelet (olvasás, frissítés, törlés, stb.). A felmérés alkalmazás ezt a lépést a keresési tábla függvények használatával hajtja végre:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Következő lépések

- Olvassa el a sorozat következő cikkét: [a webes API multitenant alkalmazásban egy kódmentes biztonságossá tétele][web-api]
- ASP.NET 1.0 Core az erőforrás-alapú hitelesítés kapcsolatos további tudnivalókért lásd: az [Erőforrás-alapú engedélyezési][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[sorozaton kívüli]: guidance-multitenant-identity.md
[Alkalmazás-szerepkörök]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[hivatkozás végrehajtása]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[A hitelesítés köztes konfigurálása]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[minta alkalmazás]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
