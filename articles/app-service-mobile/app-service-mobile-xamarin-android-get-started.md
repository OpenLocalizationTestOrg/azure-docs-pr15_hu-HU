<properties
    pageTitle="Azure mobilalkalmazások Xamarin.Android alkalmazások – első lépések"
    description="Ezen oktatóprogram lépéseiből elkezdeni használni az Azure Mobile-alkalmazások fejlesztése Xamarin Android"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Xamarin.Android alkalmazás létrehozása

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy hogyan kódmentes felhőalapú szolgáltatás hozzáadása egy Xamarin.Android alkalmazásba. További tudnivalókért olvassa el a [Mik azok a Mobile-alkalmazások](app-service-mobile-value-prop.md)című témakört.

Képernyőkép a bejegyzett alkalmazásból nem éri el:

![][0]

Ebben az oktatóanyagban befejezése rendszer minden más Mobile-alkalmazások oktatóanyagok-Xamarin.Android alkalmazást.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez szükséges az alábbi előfeltételek:

* Azure active fiók. Ha nem rendelkeznek fiókkal, az Azure próbaverzió regisztrálhat és legfeljebb 10 ingyenes Mobile-alkalmazások beszerzéséhez. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Xamarin együtt. [A telepítő és a Visual Studio és Xamarin telepítés](https://msdn.microsoft.com/library/mt613162.aspx) talál utasításokat.

>[AZURE.NOTE]Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](https://tryappservice.azure.com/?appServiceName=mobile)megnyitásához.  Rövid életű alapszintű mobilalkalmazás azonnal App szolgáltatásban hozhat létre. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="create-an-azure-mobile-app-backend"></a>Hozzon létre egy Azure mobilalkalmazás kódmentes

Kövesse ezeket a lépéseket követve hozzon létre egy mobilalkalmazás kódmentes.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Most már van, kiépítéstől a az Azure mobilalkalmazás kódmentes a mobil ügyfélalkalmazások használható. Ezután töltse le a kiszolgáló egy egyszerű "teendőlista" projektté kódmentes és Azure közzéteheti.

## <a name="configure-the-server-project"></a>A kiszolgáló-projekt beállítása

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Töltse le és futtassa a Xamarin.Android alkalmazás

1. A **Töltse le és futtassa a Xamarin.Android projekt**csoportban kattintson a **Letöltés** gombra.

    A tömörített projektfájlok mentésére a helyi számítógépre, és jegyezze fel a mentési helyét.

2. Nyomja le az **F5** billentyűt a projekt létre, és indítsa el az alkalmazást.

3. Abban az alkalmazásban írjon be kifejező szöveg, például _az oktatóprogram kész_ , és kattintson a **Hozzáadás** gombra.

    ![][10]

    A kért adatokat a program beszúrja a TodoItem táblázatba. A táblában tárolt elemek szerepeljenek eredményként a mobilalkalmazásban kódmentes szerint, és az adatok jelennek meg a listában.

    > [AZURE.NOTE] Ellenőrizheti, hogy a lekérdezés és az adatokat, amelyeket a ToDoActivity.cs C# fájlban található beszúrása a mobilalkalmazásban kódmentes hozzáférő kódot.

##<a name="next-steps"></a>Következő lépések

* [Kapcsolat nélküli szinkronizálás az alkalmazás hozzáadása](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Hitelesítés felvétele az alkalmazásba](app-service-mobile-xamarin-android-get-started-users.md)
* [Leküldéses értesítések felvétele a Xamarin.Android alkalmazásba](app-service-mobile-xamarin-android-get-started-push.md)
* [A felügyelt ügyfél használata Azure Mobile-appokról](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
