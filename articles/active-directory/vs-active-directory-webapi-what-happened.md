<properties
    pageTitle="Mi történt a WebApi projekteket (Visual Studio Azure Active Directory csatlakoztatva szolgáltatás) |} Microsoft Azure "
    description="Mi történik a MVC projekt WebApi Visual Studio segítségével csatlakoztatása Azure Active Directory ismertetése"
  services="active-directory"
    documentationCenter=""
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

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Mi történt a WebApi projekteket (Visual Studio Azure Active Directory csatlakoztatva service)

> [AZURE.SELECTOR]
> - [Első lépések](vs-active-directory-webapi-getting-started.md)
> - [mi történt](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Hivatkozások felvétele

###<a name="nuget-package-references"></a>NuGet csomag hivatkozások

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.NET-hivatkozások

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Kód módosításai

###<a name="code-files-were-added-to-your-project"></a>Kód fájlok lettek hozzáadva a projekthez

Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** hozzá lett adva a projektet, amely tartalmazza az indítási logika a Azure AD-hitelesítést.

###<a name="startup-code-was-added-to-your-project"></a>A projekthez való hozzáadásának indítási kódot.

Ha már indítási osztály a projektben, a **konfigurációs** módszer frissült-e hívást kezdeményez felvenni `ConfigureAuth(app)`. Indítási osztály ellenkező esetben a projekthez való hozzáadásának.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Az app.config vagy web.config fájlhoz új konfigurációs értékeket.

A következő konfigurációs bejegyzések van hozzáadva.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Az Azure Active Directory-alkalmazásokban készült

Az Azure Active Directory-alkalmazás a kijelölt varázsló címtárban hozták létre.

[További tudnivalók az Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Ha tudom ellenőrizni, hogy *egyes felhasználói fiókok hitelesítés letiltása*, milyen további változtak a projektbe?
NuGet csomag hivatkozások eltávolítva, és a fájlok voltak eltávolítja, és a biztonsági mentésben. Attól függően, hogy a projekt állapotát előfordulhat, hogy akkor manuálisan távolítsa el a további hivatkozások vagy a fájlok, vagy módosítsa a kódot szükség szerint.

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet csomag hivatkozások (a jelenlegi) eltávolítása

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>A biztonsági mentésben és eltávolítani (a jelenlegi) fájlok kód

A következő fájlok minden egyes a biztonsági mentésben és megszűnt a projekt. Biztonsági másolatok a legfelső szintű a projekt címtár "Biztonsági másolat" mappában található.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Kód biztonsági mentést (azok számára, bemutató)

A következő fájlok minden egyes mentést előtt cserél. Biztonsági másolatok a legfelső szintű a projekt címtár "Biztonsági másolat" mappában található.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Ha be van jelölve *a címtár-adatok olvasása*, e milyen további változtak a projekt?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>További módosításokat végzett a app.config vagy web.config

A következő további beállítási bejegyzések van hozzáadva.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Az Azure Active Directory-alkalmazás frissült.
Az Azure Active Directory-alkalmazás frissült a *címtár-adatok olvasása* engedélyt tartalmazza, és egy további kulcs jött létre, amely majd használták a *Ida: jelszó* a `web.config` fájlt.

[További tudnivalók az Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
