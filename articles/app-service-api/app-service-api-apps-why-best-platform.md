<properties 
    pageTitle="API-alkalmazások – bevezetés |} Microsoft Azure" 
    description="Megtudhatja, hogyan Azure alkalmazás szolgáltatás segítségével fejleszt, host, és felhasználja RESTful API-khoz." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>API-alkalmazások – áttekintés

Azure App szolgáltatásban API-alkalmazások könnyebb fejleszt, host, és a felhő és a helyszíni API-khoz felhasználása funkciókat kínáló. Az API-alkalmazások beszerzése a vállalati minőségű biztonsági, egyszerű hozzáférés-vezérlés, hibrid kapcsolódási, automatikus SDK létrehozása és [Logika alkalmazások](../app-service-logic/app-service-logic-what-are-logic-apps.md)zökkenőmentes integráció.

[Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md) pedig egy webes mobil, teljes körű felügyelt platform integrációs forgatókönyvek. API-alkalmazásokat az egyik [Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md)által kínált négyféle alkalmazást.

![Azure App szolgáltatásban alkalmazás típusai](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Miért érdemes használni az API-alkalmazások?

Íme néhány fontosabb funkciói API-alkalmazások:

- **Hozása, mint a már meglévő API-van** -nincs módosítsa a kódot a meglévő API-k élvezhetik az API-alkalmazások – csak az API-alkalmazás telepítése a kódot. A API bármilyen nyelvi vagy a alkalmazás szolgáltatás, beleértve a ASP.NET és C#, Java, PHP, Node.js és Python által támogatott keretrendszer használhat.

- **Könnyen felhasználási** – [Swagger API-metaadatok](http://swagger.io/) integrált támogatása lehetővé teszi az API-khoz egyszerűen felhasználható számos olyan ügyfeleknek.  A különféle nyelvek többek között a C# Java és Javascript API ügyfél-kód automatikus létrehozása. Egyszerűen [CORS](app-service-api-cors-consume-javascript.md) állítsa be, a kód megváltoztatása nélkül. További tudnivalókért lásd: [szolgáltatás API Alkalmazásindítónalkalmazások metaadatait API feltárás és kód létrehozása](app-service-api-metadata.md) és a [felhasználás CORS használatáról JavaScript API alkalmazás](app-service-api-cors-consume-javascript.md). 

- **Egyszerű hozzáférés-vezérlés** - módosítások nem hitelesített hozzáférés az API-alkalmazásokban a kód szembeni védekezésben. Beépített hitelesítési szolgáltatások más szolgáltatások, illetve felhasználókat képviselő ügyfelek biztonságos Access API-khoz. Támogatott Identitásszolgáltatók Azure Active Directory, a Facebook, a Twitteren, a Google és a Microsoft-Account tartalmazzák. Ügyfelek használhatják az Active Directory Authentication tár (ADAL) vagy a Mobile alkalmazások SDK csomagjában talál. További tudnivalókért lásd: a [hitelesítési és az API-alkalmazások Azure alkalmazás szolgáltatás engedélyezési](app-service-api-authentication.md).

- **Integráció a visual Studio** - Visual Studio dedikált eszközeinek racionalizálása: a munka létrehozása, telepítése, használata más, a hibakereséshez és a API-alkalmazások kezelése. További tudnivalókért olvassa el a [kihirdetése az Azure SDK 2.8.1 a .NET rendszerhez](/blog/announcing-azure-sdk-2-8-1-for-net/)című témakört.

- **Integráció a logika alkalmazások** - API-alkalmazások által létrehozott [Alkalmazás szolgáltatás logika alkalmazások](../app-service-logic/app-service-logic-what-are-logic-apps.md)által igénybe vehető.  További információ [az egyéni API is logika alkalmazással alkalmazás szolgáltatás használata](../app-service-logic/app-service-logic-custom-hosted-api.md) és megtekintése [Új séma verzió 2015-08-01 – előzetes verzió](../app-service-logic/app-service-logic-schema-2015-08-01.md)

Ezeken kívül az API-alkalmazásokban kihasználhassa [Web Apps alkalmazások](../app-service-web/app-service-web-overview.md) és a [Mobile-alkalmazások](../app-service-mobile/app-service-mobile-value-prop.md)által kínált szolgáltatások. A Fordított sorrend szintén igaz: webalkalmazás vagy mobilalkalmazás használata egy API tárolni, ha azt kihasználhassa API-alkalmazások szolgáltatások, például az ügyfél Swagger metaadat-alapú kód generálása és CORS az access-tartományok böngészőben. A csak három alkalmazás típusa (API, webes mobil) közötti különbség nevét és az Azure-portálon őket használt ikonra.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Mi a különbség API-alkalmazások és az Azure API-kezelés között?

API-alkalmazások és az [Azure API-szolgáltatások](../api-management/api-management-key-concepts.md) csak a kiegészítő szolgáltatások:

* API-kezelés az API-k kezelése. Az API-kezelés előtér elhelyezése egy API monitor és szabályozási kihasználtsága, kezelheti a bemeneti és kimeneti, több API-khoz egyesíteni egyik végpontját, és így tovább. Felügyelt API-khoz is szerepeltethetők bármely pontjára.
* API-alkalmazásokat az API-khoz szolgáltatója. A szolgáltatás funkciói megkönnyítik a fejlesztéséhez és igénybe API-k tartalmaz, de azt nem figyelés, szabályozásának, kezelésére szolgáló típusú, és a jelent az, hogy API-kezelés összesítésére. Ha már nincs szükség az API-felügyeleti funkciókat, üzemeltetheti az API-khoz az API-alkalmazások API-kezelés használata nélkül.

Az alábbiakban a diagram, amely azt mutatja be, az API-alkalmazásokban és máshol üzemeltetett API-hoz használt API-kezelés.

![Azure API-kezelési és az API-alkalmazások](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Egyes szolgáltatások API-kezelési és az API-alkalmazások van a hasonló feladatokat.  Automatizálható például mindkét CORS támogatás. A két szolgáltatás együttes használata esetén használhatók API-kezelés CORS óta akkor működik, mint az előtér az API-alkalmazásait. 

## <a name="getting-started"></a>Első lépések

Az API-alkalmazások példakódot egy üzembe helyezésével, című cikkben ismerkedhet bármelyik keretrendszer inkább az oktatóprogram:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [NODE.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

API-alkalmazásokkal kapcsolatos kérdéseket, indítsa el a szál [API-alkalmazások fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 
