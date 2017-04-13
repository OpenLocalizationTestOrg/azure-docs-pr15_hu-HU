<properties
    pageTitle="Mi történt a MVC projekteket (Visual Studio Azure Active Directory csatlakoztatva szolgáltatás) |} Microsoft Azure "
    description="Ismerteti, hogy mi történik a MVC projekthez való csatlakozáskor Azure Active Directory csatlakozik a Visual Studio services használatával"
    services="active-directory"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Mi történt a MVC projekteket (Visual Studio Azure Active Directory csatlakoztatva szolgáltatás)?

> [AZURE.SELECTOR]
> - [Első lépések](vs-active-directory-dotnet-getting-started.md)
> - [mi történt](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Hivatkozások felvétele

### <a name="nuget-package-references"></a>NuGet csomag hivatkozások

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET-hivatkozások

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Kód hozzá lett adva.

### <a name="code-files-were-added-to-your-project"></a>Kód fájlok lettek hozzáadva a projekthez

Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** hozzá lett adva a projektet, amely tartalmazza az indítási logika a Azure AD-hitelesítést. Ezenkívül egy vezérlő osztály, Controllers/AccountController.cs hozzá lett adva **SignIn()** és **SignOut()** módszerek, amely tartalmazza. Végül a részleges nézet, **Views/Shared/_LoginPartial.cshtml** hozzá lett adva egy művelet hivatkozást tartalmazó bejelentkezés/SignOut.

### <a name="startup-code-was-added-to-your-project"></a>A projekthez való hozzáadásának indítási kódot.

Ha már indítási osztály a projektben, a **konfigurációs** módszer frissült a hívást kezdeményez **ConfigureAuth(app)**felvenni. Indítási osztály ellenkező esetben a projekthez való hozzáadásának.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>A app.config vagy web.config értéke új konfiguráció

A következő konfigurációs bejegyzések van hozzáadva.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Azure Active Directory (AD) alkalmazás hozták létre
Az Azure Active Directory-alkalmazás a kijelölt varázsló címtárban hozták létre.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Ha tudom ellenőrizni, hogy *egyes felhasználói fiókok hitelesítés letiltása*, milyen további változtak a projektbe?
NuGet csomag hivatkozások eltávolítva, és a fájlok voltak eltávolítja, és a biztonsági mentésben. Attól függően, hogy a projekt állapotát előfordulhat, hogy akkor manuálisan távolítsa el a további hivatkozások vagy a fájlok, vagy módosítsa a kódot szükség szerint.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet csomag hivatkozások (a jelenlegi) eltávolítása

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>A biztonsági mentésben és eltávolítani (a jelenlegi) fájlok kód

A következő fájlok minden egyes a biztonsági mentésben és megszűnt a projekt. Biztonsági másolatok a legfelső szintű a projekt címtár "Biztonsági másolat" mappában található.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Kód biztonsági mentést (azok számára, bemutató)

A következő fájlok minden egyes mentést előtt cserél. Biztonsági másolatok a legfelső szintű a projekt címtár "Biztonsági másolat" mappában található.

- **Startup.cs**
- **App_Start\Startup.auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Ha be van jelölve *a címtár-adatok olvasása*, e milyen további változtak a projekt?

További hivatkozások felvétele

###<a name="additional-nuget-package-references"></a>További NuGet csomag hivatkozások

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>További .NET-hivatkozások

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>További kód fájlok lettek hozzáadva a projekthez

Két fájlok támogatása jogkivonat gyorsítótárazás lettek hozzáadva: **Models\ADALTokenCache.cs** és **Models\ApplicationDbContext.cs**.  Egy további vezérlő és a nézet hozzáadták elérésekor felhasználóiprofil-adatok használata Azure graph API-khoz szemlélteti.  Ezeket a fájlokat, hogy **Controllers\UserProfileController.cs** és **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>További indítási kódot hozzáadva a projekthez

A **startup.auth.cs** fájl egy új **OpenIdConnectAuthenticationNotifications** objektum hozzá lett adva a **OpenIdConnectAuthenticationOptions** **értesítések** tagja.  Ez a engedélyezése a OAuth kód fogadására és-hozzáférési jogkivonat, az exchange.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>További módosításokat végzett a app.config vagy web.config

A következő további beállítási bejegyzések van hozzáadva.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

A következőkben konfigurálása és a kapcsolati karakterlánc van hozzáadva.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Az Azure Active Directory-alkalmazás frissült.
Az Azure Active Directory-alkalmazás frissült a *címtár-adatok olvasása* engedélyt tartalmazza, és egy további kulcs jött létre, amelyek ezután használták a *ida: ClientSecret* **a fájlt** a.

[További tudnivalók az Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
