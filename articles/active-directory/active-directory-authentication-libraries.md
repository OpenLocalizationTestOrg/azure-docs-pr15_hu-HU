<properties
   pageTitle="Azure Active Directory Authentication tárak |} Microsoft Azure"
   description="Az Azure Active Directory Authentication Library (ADAL) lehetővé teszi, hogy a ügyfél alkalmazások fejlesztői számára egyszerűen hitelesítést végezni a felhasználóknak, hogy az a felhő vagy a helyszíni Active Directory (AD), és beszereznie hozzáférési jogkivonat biztonságossá tehető az API-hívások."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory Authentication tárak

Az Azure Active Directory authentication Library (ADAL) egyszerűen hitelesítést végezni a felhőbe felhasználók ügyfél alkalmazások fejlesztői számára lehetővé teszi, hogy a helyszíni Active Directory (AD), illetve beszereznie hozzáférési jogkivonat biztonságossá tehető az API-hívások. ADAL van sok olyan szolgáltatást, hogy ellenőrizze a hitelesítés könnyebben aszinkron támogatását, konfigurálható jogkivonat gyorsítótárat tároló hozzáférési jogkivonat, és a frissítési tokenek, automatikus jogkivonat frissítést, amikor egy hozzáférési jogkivonat lejár, és-e a frissítés jogkivonat, például a fejlesztők és az egyéb. A legtöbb összetettségétől kezelésével, ADAL logikájának az alkalmazás fejlesztője összpontosít súgó, és egyszerűen biztonságos erőforrások anélkül, hogy a biztonsági szakértő.

ADAL platformokhoz készült különböző érhető el.

|Platform|Csomag neve|Ügyfél-kiszolgáló|Letöltés|Forráskód|Dokumentáció és a minták|
|---|---|---|---|---|---|
|.NET ügyfél, a Windows áruházból, UWP, Xamarin iOS és az Android|.NET v3 az Active Directory Authentication Library (ADAL) |Ügyfél|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL a .NET rendszerhez (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Dokumentáció](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET ügyfél, a Windows áruházból, Windows Phone 8.1 |.NET v2 az Active Directory Authentication Library (ADAL) |Ügyfél|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL a .NET rendszerhez (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Dokumentáció](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|A JavaScript|A JavaScript az Active Directory Authentication Library (ADAL)|Ügyfél|[ADAL-JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL-JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Minta: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|OS X, az iOS|Az Active Directory Authentication Library (ADAL) célkitűzés-C|Ügyfél|[ADAL (CocoaPods) c-cél](http://cocoadocs.org/docsets/ADAL/)|[ADAL (Github) c-cél](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Minta: [NativeClient – iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android-alapú|Az Active Directory Authentication Library (ADAL) androidra|Ügyfél|[ADAL az Android-alapú (a központi adattárban)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL az Android-alapú (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Minta: [NativeClient Android rendszerű (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|NODE.js|Az Active Directory Authentication Library (ADAL) Node.js|Ügyfél|[ADAL-Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL-Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Minta: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|NODE.js|Microsoft Azure Active Directory Passport hitelesítési köztes csomóponthoz|Ügyfél|[Azure Active Directory Passport az Node.js (npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory-Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Az Active Directory Authentication Library (ADAL) Java|Ügyfél|[ADAL Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|A Microsoft .NET-keretrendszer 4.5-identitás protokoll bővítmények|Kiszolgáló|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure Active Directory identitás modell kiterjesztéseinek .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|JSON webes jogkivonat kezelő a Microsoft .net-keretrendszer 4.5-ös verzió|Kiszolgáló|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure Active Directory identitás modell kiterjesztéseinek .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|OWIN köztes, amely lehetővé teszi a Microsoft technológiai hitelesítéshez használt kérelmet.|Kiszolgáló|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|OWIN köztes, amely lehetővé teszi, hogy az alkalmazás OpenIDConnect hitelesítéshez.|Kiszolgáló|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Minta: [Webappban-OpenIDConnecty DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN köztes, amely lehetővé teszi, hogy az alkalmazás Webszolgáltatás-összevonás hitelesítéshez.|Kiszolgáló|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Minta: [Webappban-WSFederation DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Felhasználási területei

Íme három esetei, amelyben ADAL hitelesítéshez használt.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Távoli erőforráshoz ügyfélalkalmazás felhasználója hitelesítés

Ebben az esetben a fejlesztők tartalmaz egy ügyfélprogram, például egy WPF alkalmazást, Azure Active Directory, például a webes API által biztosított távoli erőforráshoz eléréséhez szükséges. Őt az Azure előfizetéssel rendelkezik, tudja, hogyan kell a későbbi webes API meghívása és tudja, hogy a Azure AD-bérlő, amely a webes API-t használja. Emiatt ő használatával ADAL megkönnyítheti a hitelesítést az Azure Active Directory, a hitelesítési funkcióit ADAL teljesen delegálása vagy kifejezetten kezelése a felhasználó hitelesítő adatait. ADAL tesz csatlakozó felhasználók hitelesítését, Azure AD-hozzáférési jogkivonat, és a frissítési jogkivonat beszerezni, és a hozzáférési jogkivonat, hogy egyszerűen kéri, hogy a webes API-val.

Kód, amely bemutatja az Azure Active Directory-hitelesítés használatával ebben az esetben minta [Natív WPF ügyfélalkalmazás webes API](https://github.com/azureadsamples/nativeclient-dotnet)című témakör tartalmaz.

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Távoli erőforráshoz kiszolgálói alkalmazás hitelesítő

Ebben az esetben a fejlesztők védi az Azure Active Directory, például a webes API távoli erőforrás eléréséhez szükséges kiszolgálón futó alkalmazás tartalmaz. Ő az Azure előfizetéssel rendelkezik, tudja, hogyan meghívni a későbbi szolgáltatást, és tudja, hogy a Azure AD-bérlő a webes API-t használja. Eredményt adja ő használatával ADAL megkönnyítheti az Azure Active Directory authentication kifejezetten kezelésével az alkalmazás hitelesítő adatokat. ADAL tesz jogkivonat lekérése Azure AD az alkalmazás ügyfél hitelesítő adatok használatával és adott jogkivonat, hogy egyszerűen kéri, hogy a webes API-val. ADAL is kezeli, akkor gyorsítótárazás és megújítása rá szükség szerint az jogkivonat élettartamának kezelése. Kód minta, mely szemlélteti, ebben az esetben a [Webes API-nak New](https://github.com/AzureADSamples/Daemon-DotNet)című témakör tartalmaz.

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>A kiszolgáló alkalmazás felhasználó nevében erőforrás távoli eléréséhez hitelesítés

Ebben az esetben a fejlesztők Azure Active Directory, például a webes API által biztosított távoli erőforráshoz eléréséhez szükséges kiszolgálón futó alkalmazás tartalmaz. A kérés kell Azure Active Directory tenni a felhasználók nevében. Őt az Azure előfizetéssel rendelkezik, tudja, hogyan kell a későbbi webes API meghívása, és tudja, hogy a szolgáltatás által használt bérlői az Azure AD. A felhasználó a webalkalmazás hitelesítését követően az alkalmazás-engedélyezési kód a felhasználó az szerezhet Azure AD. A webes alkalmazás segítségével ADAL szerezzen be egy hozzáférési jogkivonat, és frissítse a token engedélyezési kódot és az ügyfél hitelesítő adataival Azure AD az alkalmazás használatával felhasználó nevében. Miután a webalkalmazás birtokában a hozzáférési jogkivonat, akkor a webes API felhívhatja mindaddig, amíg a token lejár. A token lejár, a webalkalmazás ADAL megszerezni egy új jogkivonat a frissítés használatával jogkivonat, amely a korábban érkezett használható.


## <a name="see-also"></a>Lásd még:

[Az Azure Active Directory fejlesztői útmutató](active-directory-developers-guide.md)

[Az Azure Active directory Authentication esetek](active-directory-authentication-scenarios.md)

[Azure Active Directory-mintakódok](active-directory-code-samples.md)
