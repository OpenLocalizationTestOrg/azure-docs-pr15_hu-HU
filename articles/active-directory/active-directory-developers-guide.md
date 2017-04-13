<properties
   pageTitle="Azure Active Directory fejlesztői útmutató |} Microsoft Azure"
   description="Ebben a cikkben egy Fejlesztőeszközök készült erőforrások teljes segédvonalat az Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Azure Active Directory fejlesztői útmutató

## <a name="overview"></a>– Áttekintés
Egy szolgáltatás (IDMaaS) platform, Identitáskezelés, mint Azure Active Directory (AD) tartalmaz a fejlesztők az Identitáskezelés integrálhatja a saját alkalmazások hatékony módszer. A következő cikkekben tekintse át a végrehajtás és Azure Active Directory fontosabb funkciói. Javasoljuk, hogy olvassa el őket a sorrendben, vagy Ugrás [az első lépések](#getting-started) Ha készen áll alaposabban.


1. [Azure AD-integráció a előnyei](./develop/active-directory-how-to-integrate.md): megtudhatja, hogy miért integrálása az Azure Active Directory kínál a biztonságos bejelentkezés, és engedélyt a legjobb megoldás.

1. [Azure Active Directory authentication alkalmazási helyzetek](active-directory-authentication-scenarios.md): egyszerűsített bejelentkezés az alkalmazásba megadására Azure AD-hitelesítés előnyeit.

1. [Integrálása az Azure Active Directory-alkalmazások](active-directory-integrating-applications.md): megtudhatja, hogy miként hozzáadása, frissítése és alkalmazások eltávolítása az Azure Active Directory, és az esetleges útmutató integrált alkalmazások kapcsolatos.

1. [Azure Active Directory Graph API](active-directory-graph-api.md): az Azure Active Directory Graph API keresztül REST API-végpontok Azure Active Directory programozás útján eléréséhez használt. Az Azure Active Directory Graph API [Microsoft Graph](https://graph.microsoft.io/)keresztül is érhető el. A Microsoft Graph egy egyesített API-val, amely lehetővé teszi, hogy több Microsoft felhőalapú szolgáltatás API-hoz, keresztül egy REST API-végpontot, és egy egyetlen jogkivonat hozzáférést biztosít.

1. [Azure Active Directory authentication tárak](active-directory-authentication-libraries.md): egyszerűen a hozzáférési jogkivonat beszerzése a .NET rendszerhez, a JavaScript Azure Active Directory authentication tárak használatával a felhasználók hitelesítő cél-C, Android és az egyéb.


## <a name="getting-started"></a>Első lépések

Oktatóanyagokban több platformokon igényeihez vannak szabva, és gyors a az Azure Active Directory címtárral elkészítésének nyújt segítséget. Egy előfeltétel, mint kell [Ismerkedés az Azure Active Directory-bérlői](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Alkalmazás rövid útmutatók Mobile és a PC-re

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android-alapú](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![A Windows univerzális](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0-s verziója](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android-alapú](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[A Windows univerzális](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Integráció közvetlenül az OAuth 2.0-s verziója](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Webes alkalmazás rövid útmutatók

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![A JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![NODE.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![Csatlakozás OpenID](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[A JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[NODE.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Integráció közvetlenül a OpenID csatlakoztatása](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Webes API rövid útmutatók

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![NODE.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[NODE.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>A címtár-quickstart útmutató útmutató lekérdezése

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Diagram API](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>How-TOS

Ezek a cikkek ismertetik, hogyan lehet adott feladatokra Azure Active Directory használatával:

- [Ismerkedés az Azure AD-bérlő](active-directory-howto-tenant.md)
- [Jelentkezzen be az Azure Active Directory bárki a több elem bérlői alkalmazás minta használatával](active-directory-devhowto-multi-tenant-overview.md)
- Idegen-alkalmazás SSO ADAL, használata, [Android](active-directory-sso-android.md) és [iOS](active-directory-sso-ios.md) rendszerű eszközökön engedélyezése
- [Győződjön meg az alkalmazás AppSource tanúsítvánnyal az Azure Active Directory](active-directory-devhowto-appsource-certified.md)
- [Az alkalmazás az Azure Active Directory-alkalmazás gyűjteményben lista](active-directory-app-gallery-listing.md)
- [Web Apps alkalmazások elküldése az Office 365 az eladóhoz irányítópult](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Az Azure Active Directory-alkalmazás jegyzék ismertetése](active-directory-application-manifest.md)
- [A védjegy profilképek a bejelentkezési és az alkalmazás WIA gomb az ügyfélalkalmazás ismertetése](active-directory-branding-guidelines.md)
- [Kép: Hogyan hozhat létre a jelentkezzen be kell jelentkeznie a személyes és munkahelyi vagy iskolai fiók felhasználók alkalmazásai](active-directory-appmodel-v2-overview.md)
- [Kép: Hogyan hozhat létre a regisztráció, és jelentkezzen be a fogyasztói alkalmazásai](../active-directory-b2c/active-directory-b2c-overview.md)
- [Előzetes verzió: jogkivonat élettartama konfigurálása az Azure Active Directory](active-directory-configurable-token-lifetimes.md) PowerShell használatával. Az Azure Active Directory Graph API keresztül konfigurálása [házirend műveletek](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) és a további információt a [házirend entitás](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) látható.

## <a name="reference"></a>Hivatkozás

Ezek a cikkek a többi és authentication library API-hoz, jegyzőkönyvek, hibák, mintakódok és végpontok foundation hivatkozást szükséges.  

###  <a name="support"></a>Támogatás
- [Címkézett kérdések](http://stackoverflow.com/questions/tagged/azure-active-directory): Azure Active Directory keresése megoldások a túlcsordulás Papírhalom keresése a címkék [azure active Directoryval](http://stackoverflow.com/questions/tagged/azure-active-directory) és [adal](http://stackoverflow.com/questions/tagged/adal).
- Lásd: az [Azure Active Directory Fejlesztőeszközök szószedet](active-directory-dev-glossary.md) definíciók része a kapcsolódó alkalmazások fejlesztése és integráció a gyakran használt kifejezések.

### <a name="code"></a>Kód

- [Azure Active Directory Megnyitás-forrás tárak](http://github.com/AzureAD): Ha egy tárban forrást szeretne megkeresni legkönnyebben [tár lista](active-directory-authentication-libraries.md)használatával.

- [Azure Active Directory-minták Megnyitás](https://github.com/azure-samples?query=active-directory): A minták listájának legegyszerűbben az [index mintakódok](active-directory-code-samples.md)használatával.

- [Az active Directory Authentication tár (ADAL) a .NET rendszerhez](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - hivatkozás dokumentáció [a legújabb főverzió](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) és [az előző főverzió](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)érhető el.

### <a name="graph-api"></a>Diagram API

- [Diagram API-hivatkozás](https://msdn.microsoft.com/library/azure/hh974476.aspx): Azure Active Directory Graph API többi hivatkozását. [A Graph API -val interaktív hivatkozás felület megtekintése](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Diagram API jogosultsági hatókörök](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0-s jogosultsági hatókörök, amellyel szabályozhatja az access alkalmazás tartalmazó címtár adatok egy bérlői webhelyen.

### <a name="authentication-and-authorization-protocols"></a>Hitelesítés és az engedélyezés protokollok

- [Kulcs átváltási aláírása az Azure Active Directory](active-directory-signing-key-rollover.md): További tudnivalók: Azure AD az aláíró kulcs átváltási cadence és frissítése a leggyakoribb alkalmazás forgatókönyvet billentyűjét.

- [OAuth 2.0-s Protocol (protokoll): engedélyezési kód megadása használatával](active-directory-protocols-oauth-code.md): webalkalmazások való hozzáférés engedélyezése a OAuth 2.0-s protocol engedélyezési kód megadása is használhatja, és az Azure Active Directory a webes API-khoz bérlői.

- [OAuth 2.0-s Protocol (protokoll): az implicit támogatás ismertetése](active-directory-dev-understanding-oauth2-implicit-grant.md): További információ az implicit engedély megadása és hogy az alkalmazás megfelelő.

- [OAuth 2.0-s Protocol (protokoll): szolgáltatás hívások használatával ügyfél hitelesítő adatai szolgáltatás](active-directory-protocols-oauth-service-to-service.md): támogatás az OAuth 2.0 ügyfél hitelesítő adatait (bizalmas ügyfél) használata a saját hitelesítő adatok hitelesítést végezni, ha egy másik webszolgáltatás hívása webszolgáltatás helyett egy felhasználó lehetővé teszi a. Ebben az esetben az ügyfél nem általában középszintű webszolgáltatás, daemon szolgáltatás vagy webhelyet.

- [OpenID csatlakozás 1.0 protokoll: bejelentkezés és a hitelesítéshez](active-directory-protocols-openid-connect-code.md): A OpenID csatlakozás 1.0 protokoll OAuth 2.0-s bővíti a hitelesítési protokoll használatát. Ügyfélalkalmazás kap egy id_token kezelheti a bejelentkezési folyamatot, vagy a engedélyezési kód folyamat egy id_token és a hitelesítés kód fogadására kiegészítheti.

- [A SAML 2.0 protokoll hivatkozás](active-directory-saml-protocol-reference.md): A SAML 2.0 protokoll lehetővé teszi, hogy az alkalmazások ahhoz, hogy egy egyszeri bejelentkezéses megoldást felhasználóiknak.

- [Webszolgáltatás-összevonás 1.2-es Protocol (protokoll)](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory támogatja a webes szolgáltatások összevonási verzió 1.2-es specifikációja szerinti Webszolgáltatás-összevonás 1.2-es. Az összevonási metaadat-dokumentum kapcsolatos további tudnivalókért lásd [a metaadat-alapú összevonás](active-directory-federation-metadata.md).

- [Támogatott jogkivonat, és a felelős](active-directory-token-and-claims.md): Ez az útmutató használhatja, és a SAML 2.0-s és JSON webes tokenek (JWT) tokenek az értékelni.

## <a name="videos"></a>Videók

### <a name="build"></a>Összeállítása

Alkalmazások fejlesztésével Azure Active Directory használatával ezeket áttekintése a bemutatókat közvetlenül a mérnöki csapatának dolgozó hangszórók funkció. A bemutatók alapvető témaköreit, beleértve a IDMaaS, hitelesítés, identitás összevonási és az egyszeri bejelentkezés terjed ki.

- [Microsoft identitás: Állapotát az Unió és a későbbi irány](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Identitáskezelés alkalmazásokat modern szolgáltatásként](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Modern webalkalmazásokat az Azure Active Directory címtárral](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Az Azure Active Directoryval, modern natív alkalmazások fejlesztői számára](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure péntek
[Azure péntek](https://azure.microsoft.com/documentation/videos/azure-friday/) egy ismétlődő péntekig, amely a kitűzött célja, hogy Ön (10 – 15 perccel) rövid 1:1 videósorozat interviews az Azure témakörök különböző szakértőktől.  A funkcióval szolgáltatások szűrő lapon Azure Active Directory videók megtekintéséhez.

- [Azure identitás 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure identitás 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure identitás 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Közösségi

- [Az Active Directory csapatának blogja](http://blogs.technet.com/b/ad/): Azure Active Directory földrajzi a legfrissebb fejleményekről.

- [Azure Active Directory Graph csapatának blogja](http://blogs.msdn.com/b/aadgraphteam):, amely a diagram API az Azure Active Directory-adatokat.

- [Felhőalapú identitás](http://www.cloudidentity.net): a szolgáltatás, a egy egyszerű Azure Active Directory du Identitáskezelés gondolatait.  

- [Azure Active Directory, a Twitteren](https://twitter.com/azuread): 140 karakternél az Azure Active Directory hirdetmények.

## <a name="windows-server-on-premises-development"></a>A Windows Server helyszínen fejlesztési
A Windows Server és az Active Directory összevonási szolgáltatások (ADFS) fejlesztési használatához című témakörben olvashat:

- [Active Directory összevonási szolgáltatások esetek fejlesztők számára](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): áttekintést nyújt az AD FS-összetevők és működéséről, a részletes tudnivalókat az támogatott hitelesítéstípusok/engedélyezési jelenik meg.
- [Active Directory összevonási szolgáltatások forgatókönyvek](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): segédlet cikkek, amely lépésről lépésre a kapcsolódó hitelesítési/engedélyezési flow megvalósításáról listáját.
