<properties
   pageTitle="Azure Active Directory 2.0-s verzió hitelesítési tárak |} Microsoft Azure"
   description="Kompatibilis ügyfél-tárak és a kiszolgáló köztes tárak, és a kapcsolódó tár, a forrás és a minták hivatkozásokat, az Azure Active Directory 2.0-s verzió végpontot."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory 2.0-s verzió hitelesítési tárak
Az Azure Active Directory (Azure Active Directory) 2.0-s verzió végpontot támogatja a szabványos OAuth 2.0-s és OpenID csatlakozás 1.0 protokollok. A Microsoft és más szervezetek különböző tárak is használhatja a 2.0-s verzió végponttal.

Ha egy alkalmazást a 2.0-s verzió végpont használó generál, azt javasoljuk, tárak, akik szintén követik biztonsági fejlesztési életciklus (SDL) módszer, például [a Microsoft követ]protokoll tartomány szakértők által írt használható[Microsoft-SDL]. Ha úgy dönt, hogy a protokollok kéz-kód támogatása, az azt javasoljuk, hajtsa végre az SDL módszertan, és a figyeljen biztonsági megfontolások az egyes protokoll szabványokat alkalmazásra vonatkozó technikai adatok.

## <a name="types-of-libraries"></a>A tárak típusai

Azure Active Directory 2.0-s verzió működik-e a két tártípus:

- **Ügyfél-tárakban**. Natív ügyfél- és kiszolgáló segítségével ügyfél tárak hozzáférési jogkivonat, a erőforrás, például Microsoft Graph hívásához.
- **Köztes server-tárak**. Web Apps alkalmazások használata kiszolgáló köztes tárak felhasználónak bejelentkezés. Webes API-khoz kiszolgáló köztes tárak használata natív ügyfelek vagy más kiszolgálók által küldött tokenek ellenőrzése.

## <a name="library-support"></a>Tár-támogatás
Megadhatja a szabványoknak megfelelő tártípusból a 2.0-s verzió végpont használatakor, mert fontos, hogy honnan lépjen a támogatás. Problémák és a tár kód frissítéseiről forduljon a tár tulajdonosa. Problémák és szolgáltatás mellett protokoll végrehajtásában frissítéseiről forduljon a Microsoft.

Tárak két támogatási kategóriában jár:

- A **Microsoft által támogatott**. A Microsoft javításokat nyújt a tárakban és SDL megállapított haladékot a tárakban.
- **Kompatibilis**. A Microsoft tesztelte a tárak az alapvető felhasználási helyzet, és arról, hogy dolgozhat a 2.0-s verzió végpont. A Microsoft nem nyújt a tárak javítások, és nem befejeződik a tárak ismerkedést. A tár Megnyitás forrásprojektben problémák és frissítéseiről kell irányítani.

Tárat, amellyel dolgozni a 2.0-s verzió végpont listájáért lásd: Ebben a cikkben a következő szakaszokkal.

## <a name="microsoft-supported-client-libraries"></a>A Microsoft által támogatott ügyfél-tárak
| Platform| Dokumentumtár neve| Letöltés | Forráskód | Minta |
| :-: | :-: | :-: | :-: | :-: |
| .NET, a Windows áruházból, Xamarin | A Microsoft Authentication Library (MSAL) a .NET rendszerhez | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [A .NET rendszerhez (GitHub) MSAL][ClientLib-NET-Repo] | [Windows asztali natív ügyfele minta][ClientLib-NET-Sample] |
| NODE.js | Microsoft Azure Active Directory Passport.js beépülő modul | [Passport Azure-ADFS-(npm)][ClientLib-Node-Lib] | [Passport Azure-ADFS-(GitHub)][ClientLib-Node-Repo] | hamarosan |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>A Microsoft által támogatott server köztes-tárak
| Platform| Dokumentumtár neve| Letöltés | Forráskód | Minta |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID köztes ASP.NET csatlakozni. | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana projekt (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Web app-minta][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | ASP.NET OWIN OAuth Bearer köztes | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Webes API-minta][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET core | OWIN OpenID köztes a .NET Core csatlakozni. | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.NET biztonsági (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Web app-minta][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET core | A .NET Core OWIN OAuth Bearer köztes | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.NET biztonsági (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | hamarosan |
| NODE.js | Microsoft Azure Active Directory Passport.js beépülő modul | [Passport Azure-ADFS-(npm)][ServerLib-Node-Lib] | [Passport Azure-ADFS-(GitHub)][ServerLib-Node-Repo] | [Web app-minta][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Kompatibilis ügyfél-tárak
| Platform| Dokumentumtár neve | Tesztelt verziója | Forráskód | Minta |
| :-: | :-: | :-: | :-: | :-: |
| Android-alapú | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Natív alkalmazás minta](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Natív alkalmazás minta](active-directory-v2-devquickstarts-ios.md)|
| A JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | hamarosan |
| Python-lombikot | [Lombikot-OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Lombikot-OAuthlib](https://github.com/lepture/flask-oauthlib) | hamarosan |
| Fonetikus | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth oauth2 hitelesítési mód](https://github.com/intridea/omniauth-oauth2) | hamarosan |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Kompatibilis kiszolgáló köztes tárak
hamarosan

## <a name="related-content"></a>Kapcsolódó tartalom
Többet szeretne tudni az Azure Active Directory 2.0-s verzió végpontot, az [Azure Active Directory alkalmazás modell 2.0-s verzió – áttekintés]című[AAD-App-Model-V2-Overview].

Segítse a szolgáltatás pontosítása és a tartalom alakzat, használja a Disqus megjegyzéskészítő funkciók Ez a cikk végén visszajelzést.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
