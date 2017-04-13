<properties
    pageTitle="Azure alkalmazás szolgáltatás Mobile-alkalmazások Xamarin.iOS alkalmazások használatának első lépései |} Microsoft Azure"
    description="Kövesse az ebben az oktatóanyagban használatba Xamarin.iOS fejlesztési Mobile-alkalmazások használata a."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Xamarin.iOS alkalmazás létrehozása

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy hogyan kódmentes felhőalapú szolgáltatás hozzáadása Xamarin.iOS mobilalkalmazásban az Azure mobilalkalmazás kódmentes használatával.  Létrehozhat egy új mobilalkalmazás kódmentes és egy egyszerű _Teendőlista_ Azure-ban alkalmazás adatokat tároló Xamarin.iOS alkalmazás is.

Ebben az oktatóanyagban befejezése rendszer minden más Xamarin.iOS oktatóanyagok Azure App szolgáltatásban a Mobile-alkalmazások funkció használatáról.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez szükséges az alábbi előfeltételek:

* Azure active fiók. Nem rendelkeznek fiókkal, ha egy Azure próbaverzió regisztrálhat, és legfeljebb 10 ingyenes mobilalkalmazásainak beszerzése, amelyet még a próbaidőszak lejártakor megtartása használ. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Xamarin együtt. [A telepítő és a Visual Studio és Xamarin telepítés](https://msdn.microsoft.com/library/mt613162.aspx) talál utasításokat.

* A Mac vagy újabb verziójával Xcode 7.0 és Xamarin Studio közösségi telepítve. Lásd: [a telepítő for Visual Studio és Xamarin telepítés](https://msdn.microsoft.com/library/mt613162.aspx) és [beállítás, telepítése, és a Mac-felhasználók számára ellenőrzések](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Ha szeretné az első lépések Azure alkalmazás szolgáltatás, mielőtt regisztrál az Azure-fiók, nyissa meg a [Alkalmazás szolgáltatás próbálja meg](https://tryappservice.azure.com/?appServiceName=mobile). Rövid életű starter mobilalkalmazásban azonnal létrehozhat az alkalmazás szolgáltatás – nem kötelező hitelkártya, és nincs nyilatkozatát.

## <a name="create-an-azure-mobile-app-backend"></a>Hozzon létre egy Azure mobilalkalmazás kódmentes

Kövesse ezeket a lépéseket követve hozzon létre egy mobilalkalmazás kódmentes.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>A kiszolgáló-projekt beállítása

Most már van, kiépítéstől a az Azure mobilalkalmazás kódmentes a mobil ügyfélalkalmazások használható. Ezután töltse le a kiszolgáló egy egyszerű "teendőlista" projektté kódmentes és Azure közzéteheti.

Kövesse az alábbi lépésekkel állítsa be a kiszolgáló projekt Node.js vagy .NET kódmentes használni.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Töltse le és futtassa a Xamarin.iOS alkalmazás

1. Nyissa meg az [Azure portált] a böngészőablakban.

2. Kattintson a beállítások lap, a Mobile alkalmazáshoz, kattintson az **Első lépések** > **Xamarin.iOS**. Lépés a 3-as kattintson **egy új alkalmazás létrehozása** , ha nem az az aktív.  Ezután kattintson a **Letöltés** gombra.

    A mobil kódmentes kapcsolódó ügyfélalkalmazás letöltése. A tömörített projektfájlok mentésére a helyi számítógépre, és jegyezze fel a mentési helyét.

3. Bontsa ki a projektet, amely letöltötte, és nyissa meg az Xamarin Studio (vagy Visual Studio).

    ![][9]

    ![][8]

4. Nyomja le az F5 billentyűvel készítése a projekthez, és indítsa el az alkalmazást az iPhone-alapú irányító.

5. Abban az alkalmazásban, írja be a értelmezhető szöveget, például _Xamarin ismerje meg_, és kattintson a **+** gombra.

    ![][10]

    A kért adatokat a program beszúrja a TodoItem táblázatba. A táblában tárolt elemek szerepeljenek eredményként a mobilalkalmazásban kódmentes szerint, és az adatok jelennek meg a listában.

>[AZURE.NOTE]Megtekintheti a lekérdezést, és adatok QSTodoService.cs C# fájl beszúrása a mobilalkalmazásban kódmentes hozzáférő kódot.

##<a name="next-steps"></a>Következő lépések

* [Kapcsolat nélküli szinkronizálás az alkalmazás hozzáadása](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Hitelesítés felvétele az alkalmazásba](app-service-mobile-xamarin-ios-get-started-users.md)
* [Leküldéses értesítések felvétele a Xamarin.Android alkalmazásba](app-service-mobile-xamarin-ios-get-started-push.md)
* [A felügyelt ügyfél használata Azure Mobile-appokról](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portál]: https://portal.azure.com/
