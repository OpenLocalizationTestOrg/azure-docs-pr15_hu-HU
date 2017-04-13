<properties
    pageTitle="Hozzon létre egy Cordova alkalmazás Azure alkalmazás szolgáltatás mobilalkalmazások |} Microsoft Azure"
    description="Ezen oktatóprogram lépéseiből – első lépések Apache Cordova fejlesztési Azure mobilalkalmazás háttérkiszolgálókon verzióval"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, mobil, a javascript-ügyfél" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Hozzon létre egy Apache Cordova alkalmazás

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy hogyan kódmentes felhőalapú szolgáltatás hozzáadása egy Apache Cordova mobilalkalmazás az Azure mobilalkalmazás kódmentes használatával.  Létrehoz egy új mobilalkalmazás kódmentes és egy egyszerű _Teendőlista_ Azure-ban alkalmazás adatokat tároló Apache Cordova alkalmazás is.

Ebben az oktatóanyagban befejezése rendszer minden más Apache Cordova oktatóanyagok Azure App szolgáltatásban a Mobile-alkalmazások funkció használatáról.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban a következőkre lesz szüksége:

* PC-n [Visual Studio közösségi 2015] vagy újabb verziót.
* [Visual Studio Tools for Apache Cordova].
* [Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).

Is Visual Studio átlépése, és közvetlenül a a Apache Cordova parancsot használja.  Ez akkor hasznos, ha a Mac gépen az oktatóprogram befejezése.  Ebben az oktatóanyagban nem vonatkozik a parancssorból Apache Cordova ügyfélalkalmazásokban összeállítása.

## <a name="create-a-new-azure-mobile-app-backend"></a>Hozzon létre egy új Azure mobilalkalmazás kódmentes

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Hasonló lépésekkel megjelenítő videó megtekintése](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>A kiszolgáló-projekt beállítása

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Töltse le és futtassa a Apache Cordova alkalmazás

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Következő lépések

Most, hogy elvégezte a rövid útmutató az oktatóprogram, folytassa az alábbi oktatóanyagok közül:

* [Hitelesítés hozzáadása] a Apache Cordova alkalmazást.
* [Leküldéses értesítések hozzáadása] a Apache Cordova alkalmazást.

További tudnivalók a alapfogalmak Azure alkalmazás szolgáltatással.

* [Hitelesítés]
* [Leküldéses értesítések]

Megtudhatja, hogy miként használhatja a SDK.

* [Apache Cordova SDK]
* [ASP.NET-kiszolgáló SDK]
* [NODE.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio közösségi 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Hitelesítés hozzáadása]: app-service-mobile-cordova-get-started-users.md
[Adja hozzá a leküldéses értesítések]: app-service-mobile-cordova-get-started-push.md
[Hitelesítés]: app-service-mobile-auth.md
[Leküldéses értesítések]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET-kiszolgáló SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[NODE.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
