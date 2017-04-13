<properties
   pageTitle="Azure Active Directory mintakódok |} Microsoft Azure"
   description="Tárgymutató Azure Active Directory mintakódok forgatókönyv szerint rendezve."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Azure Active Directory mintakódok

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure Active Directory) hitelesítési és engedélyezési hozzáadása a webalkalmazások és a webes API-khoz is használhatja. Ez a szakasz hivatkozásainak mintakódok, amelyek bemutatják, hogyan történik és kódtöredék, amelyek segítségével használhatja az alkalmazásokban. A kód minta lapon találhatók részletes olvasható-én témakörök, amelyek segítségével követelményeket, a telepítési és a beállítási. És a kód van segít megérteni a kritikus szakaszokat.

Az egyszerű forgatókönyv az egyes minta:, című cikkben talál részletes hitelesítési esetek az Azure Active Directory.

A minták GitHub a bővíthessék: [Microsoft Azure Active Directory minták és dokumentációt](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Webböngésző webalkalmazáshoz

Ezek a példák bemutatják, amely arra utasítja az Azure Active Directory jelentkezzen be őket a felhasználó böngésző webalkalmazás szövegalkotás.



| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| C# / .NET | [Webalkalmazás-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | OpenID csatlakozás (ASP.Net OpenID csatlakozás OWIN köztes) segítségével hitelesíti az Azure AD-bérlő felhasználóit.
| C# / .NET | [Webalkalmazás-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Több elem bérlői .NET MVC használó webalkalmazások OpenID csatlakoztatása (ASP.Net OpenID csatlakozás OWIN köztes) hitelesítést végezni a felhasználók a több Azure AD-bérlők.
| C# / .NET | [Webalkalmazás-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Webszolgáltatás-összevonás (ASP.Net Webszolgáltatás-összevonás OWIN köztes) használata hitelesítést végezni a felhasználók az Azure AD-bérlő.






## <a name="single-page-application-spa"></a>Egyetlen lap alkalmazás (biztonságos jelszó-hitelesítés)

Ez a példa bemutatja, hogyan az Azure Active Directory védett egyoldalas alkalmazás.  

| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| A JavaScript, C# / .NET | [SinglePageApp-DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | A JavaScript és Azure Active Directory ADAL segítségével ASP.NET webes API vissza vége hajtanak végre egyetlen oldalon AngularJS-alapú alkalmazás biztonságos.


## <a name="native-application-to-web-api"></a>A webes API tartozó alkalmazásban

Ezek mintakódok bemutatják, hogyan hívja fel a webes API-hoz, amely az Azure Active Directory által biztosított natív ügyfélalkalmazásokban össze. [Azure Active Directory Authentication Library (ADAL)](active-directory-authentication-libraries.md) és [az Azure Active Directory OAuth 2.0-s](https://msdn.microsoft.com/library/azure/dn645545.aspx)használják.

| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| A JavaScript | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Az ADAL Apache Cordova beépülő modul, amely felhívja a webes API-val és a hitelesítéshez használja az Azure Active Directory Apache Cordova alkalmazás használatával.
| C# / .NET | [NativeClient-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | A .NET WPF alkalmazás, amely a webes API hív meg, amelyek használatával az Azure Active Directory védett.
| C# / .NET | [NativeClient-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Egy Windows Áruházbeli alkalmazás, amely a webes API hív meg, hogy az Azure Active Directory védett.
| C# / .NET | [NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | A Windows áruházból alkalmazás, hívja fel az Azure Active Directory védett API több bérlői webhelyet.
| C# / .NET | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Natív ügyfélalkalmazás, amely felhívja a webes API-val, amely az eredeti felhasználó nevében eljáró token kap, és a jogkivonat alapján egy másik webes API felhívni.
| C# / .NET | [NativeClient-WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Egy Windows Áruházbeli alkalmazás Windows Phone 8.1, amely a webes API hív meg, hogy az Azure Active Directory védi.
| ObjC | [Az iOS NativeClient](https://github.com/Azure-Samples/active-directory-ios) | Egy iOS-alkalmazás, amely a webes API hív meg, hogy Azure Active Directory hitelesítést igényel.
| C# / .NET | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Natív ügyfélalkalmazás, amely tartalmazza a logika a webes API-t, a JWT jogkivonat feldolgozása OWIN köztes használata helyett.
| C# / Xamarin | [NativeClient Xamarin Android rendszerű](https://github.com/Azure-Samples/active-directory-xamarin-android) | Xamarin kötést szeretne a natív Azure Active Directory Authentication tár (ADAL) a Android tárat.
| C# / Xamarin | [NativeClient-Xamarin – iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Xamarin kötést szeretne a natív Azure Active Directory Authentication tár (ADAL) az iOS.
| C# / Xamarin | [NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Azure Active Directory öt platformokon észlelő, és a webes API-val, amely felhívja Xamarin projekt védi.
| C# / .NET | [NativeClient távfelügyeletű DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | A natív alkalmazás, amely nem interaktív hitelesítését hajtja végre, és a webes API-val, amely felhívja védi Azure AD.



## <a name="web-application-to-web-api"></a>Webalkalmazás webes API

A Megjelenítés használatával hogyan [az Azure Active Directory OAuth 2.0-s](https://msdn.microsoft.com/library/azure/dn645545.aspx) felhívni webalkalmazások mintakódok webes API-hoz, amely az Azure Active Directory által biztosított.

| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| C# / .NET | [Webalkalmazás-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Hívja fel a webes API a bejelentkezett a felhasználók engedélyeit.
|  C# / .NET | [Webalkalmazás-WebAPI-oauth2 hitelesítési mód-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Hívja fel a webes API az alkalmazás engedélyekkel.
| C# / .NET | [Webalkalmazás-WebAPI-oauth2 hitelesítési mód-felhasználói identitáshoz-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | [Az Azure Active Directory OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) 2.0-s engedélyezési hozzáadása meglévő webalkalmazás így felhívhatja, hogy a webes API.
| A JavaScript | [WebAPI-Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Állítsa be a REST API-szolgáltatás, integrálva van az API védelmet Azure AD. A webes API-val Node.js kiszolgáló tartalmazza.
| C# / .NET | [Webalkalmazás-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Több elem bérlő MVC webalkalmazás használatával OpenID csatlakoztatása (ASP.Net OpenID csatlakozás OWIN köztes) hitelesíti a felhasználókat az Azure Active Directory-bérlőből. A diagram API meghívása egy engedélyezési kód használja.

## <a name="server-or-daemon-application-to-web-api"></a>Kiszolgáló vagy a webes API démonalkalmazásaihoz

Ezek a mintakódok bemutatják, hogyan a webes API [Azure Active Directory Authentication Library (ADAL)](active-directory-authentication-libraries.md) és [az Azure Active Directory OAuth 2.0-s](https://msdn.microsoft.com/library/azure/dn645545.aspx)erőforrások megszerzi démon vagy a kiszolgáló-alkalmazások létrehozásához.

| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| C# / .NET | [Démon-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | A New a webes API hívások. Az ügyfél hitelesítő adatokhoz a jelszóval.
| C# / .NET | [Démon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | A New, amely felhívja a webes API-val. Az ügyfél hitelesítő adatokhoz egy tanúsítványt.


## <a name="calling-azure-ad-graph-api"></a>Azure Active Directory-grafikon API hívása

A kód minta bemutatják, hogyan hívja fel a címtár-adatainak olvasása és írása az Azure Active Directory Graph API-alkalmazások létrehozásához.

| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| Java | [A Java GraphAPI Webappban](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Azure Active directory-adatok eléréséhez a diagram API-t használó webalkalmazást.
| PHP | [Webalkalmazás-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Azure Active directory-adatok eléréséhez a diagram API-t használó webalkalmazást.
| C# / .NET | [Webalkalmazás-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Azure Active directory-adatok eléréséhez a diagram API-t használó webalkalmazást.
| C# / .NET | [ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Konzol az alkalmazás a Graph API közös olvasási és írási hívásainak mutatja be, és bemutatja, hogyan hajtsa végre a felhasználói licencek hozzárendelését, és frissítse a felhasználó miniatűrt és hivatkozások.
| C# / .NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | A New, amelyből a megkülönböztető lekérdezést a diagram API periodikus Azure AD-ös bérlői felhasználói objektumok megváltoztatása.
| C# / .NET | [Webalkalmazás-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | MVC alkalmazás Graph API-lekérdezések használja a vállalati egyszerű szervezeti diagram létrehozása.
| PHP | [Webalkalmazás-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | A diagram API-bővítménye regisztrálása olvassa el, frissítése és törlése a bővítmény attribútum értékein PHP-alkalmazás.


## <a name="authorization"></a>Engedély

Ezek mintakódok bemutatják, hogyan Azure Active Directory használandó engedélyt.

| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| C# / .NET | [Webalkalmazás-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Szerepköralapú hozzáférés szerepalapú Azure Active Directory-csoportot követelések használata az Azure Active Directory integrált alkalmazások végrehajtása
| C# / .NET | [Webalkalmazás-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Szerepköralapú hozzáférés szerepalapú Azure Active Directory-alkalmazás szerepkörök használata az Azure Active Directory integrálva van alkalmazásokban végrehajtása


## <a name="legacy-walkthroughs"></a>Régi forgatókönyvek

Ezek a forgatókönyvek kissé régebbi technológiát használ, de továbbra is érdemes lehet.

| Nyelvi/Platform | Minta | Leírás
| ----------------- | ------ | -----------
| C# / .NET | [Szerepköralapú és vezérlés-alapú hitelesítés egy Microsoft Azure Active Directory-alkalmazásban](http://go.microsoft.com/fwlink/?LinkId=331694) | Szerepkör-alapú hitelesítés (RBAC) és a vezérlés-alapú hitelesítés hajtsa végre az Azure Active Directory integrálva van alkalmazásokban.
| C# / .NET |  [AAL – Windows Áruházbeli alkalmazás a többi szolgáltatás - hitelesítés](http://go.microsoft.com/fwlink/?LinkId=330605) |  Felhasználói hitelesítés funkciók felvétele a Windows áruházból letölthető [Azure Active Directory Authentication Library (ADAL)](active-directory-authentication-libraries.md) (korábbi nevén AAL) a Windows áruházból Beta segítségével.
| C# / .NET | [ADAL - natív alkalmazás a többi szolgáltatás – hitelesítéssel AAD keresztül böngésző párbeszédpanel](http://go.microsoft.com/fwlink/?LinkId=259814) |  Felhasználói hitelesítés funkciók felvétele egy WPF ügyfél [Azure Active Directory Authentication Library (ADAL)](active-directory-authentication-libraries.md) segítségével.
| C# / .NET | [ADAL - natív alkalmazás a többi szolgáltatás – hitelesítéssel ACS keresztül böngésző párbeszédpanel](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  [Azure Active Directory Authentication Library (ADAL)](active-directory-authentication-libraries.md) és az [Access vezérlő szolgáltatás 2.0-s (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) felvétele segítségével felhasználói hitelesítés lehetőségei WPF ügyfél.
| C# / .NET | [ADAL - kiszolgáló hitelesítés](http://go.microsoft.com/fwlink/?LinkId=259816) | [Azure Active Directory Authentication Library (ADAL)](active-directory-authentication-libraries.md) segítségével szolgáltatás hívásait kiszolgálói egymás folyamat egy MVC4 webes API többi szolgáltatás biztonságos.
| C# / .NET | [Bejelentkezés a webes alkalmazás segítségével az Azure Active Directory hozzáadása](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Állítsa be a .NET-alkalmazások webes egyszeri bejelentkezés az Azure Active Directory vállalati címtár elvégzéséhez.
| C# / .NET | [Több elem bérlői webalkalmazások Azure Active Directory létrehozása](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Azure Active Directory egyszeri bejelentkezés a és a címtár-elérés több szervezet belül tevékenységekhez egy .NET alkalmazás felvétele segítségével.
JAVA | [Java minta alkalmazás Azure Active Directory-grafikonhoz API](http://go.microsoft.com/fwlink/?LinkId=263969) | Access-címtár adatok az Azure Active Directory a Graph API segítségével.
PHP | [Azure Active Directory-grafikonhoz API PHP minta alkalmazás](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Access-címtár adatok az Azure Active Directory a Graph API segítségével.
| C# / .NET | [Azure Active Directory-grafikonhoz API minta alkalmazás](http://go.microsoft.com/fwlink/?LinkID=262648) | Access-címtár adatok az Azure Active Directory a Graph API segítségével.
| C# / .NET | [Azure Active Directory-grafikon megkülönböztető lekérdezés minta alkalmazás](http://go.microsoft.com/fwlink/?LinkId=275398) | Segítségével a megkülönböztető lekérdezést a diagram API felhasználói objektumok periodikus módosításainak Azure AD-ös bérlői.
| C# / .NET | [Minta alkalmazás integrálása az Azure Active Directory több bérlői felhő alkalmazás](http://go.microsoft.com/fwlink/?LinkId=275397) | A több elem bérlői alkalmazások integrálása az Azure Active Directory.
| C# / .NET | [A Windows áruház-alkalmazás és a REST Web Service segítségével az Azure Active Directory biztonságossá tétele](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Hozzon létre egy egyszerű webes API erőforrás és Azure Active Directory használata a Windows áruházból ügyfélalkalmazás és az [Azure Active Directory Authentication Library (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [A lekérdezés Azure AD a diagram API segítségével](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Állítsa be a Microsoft .NET-alkalmazások elérni az adatokat az Azure AD-bérlő címtárhoz az Azure Active Directory Graph API segítségével.

## <a name="see-also"></a>Lásd még:

##### <a name="other-resources"></a>Egyéb források

[Azure Active Directory fejlesztői útmutató](active-directory-developers-guide.md)

[Azure Active Directory Graph elvi API és hivatkozás](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure Active Directory-grafikon API-segítő tárban](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

