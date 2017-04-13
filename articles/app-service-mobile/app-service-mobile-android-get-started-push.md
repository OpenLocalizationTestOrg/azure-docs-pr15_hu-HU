<properties
    pageTitle="Android alkalmazást az Azure mobilalkalmazások leküldéses értesítéseket hozzáadása"
    description="Megtudhatja, hogy miként Azure Mobile-alkalmazások használata a leküldéses értesítéseket küldeni az Android alkalmazást."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Leküldéses értesítések hozzáadása az Android-alkalmazás

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>– Áttekintés
Ebben az oktatóanyagban vesz fel leküldéses értesítéseket a [rövid útmutató az Android] -projektet, hogy a leküldéses értesítést küld az eszköz minden alkalommal, amikor a program beszúrja a rekordot.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, a leküldéses értesítést bővítmény csomag szüksége. További információ [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

## <a name="prerequisites"></a>Előfeltételek

Az alábbiakra van szükség:

* Attól függően, hogy a projekt kódmentes egy IDE:

    * [Android Studio](https://developer.android.com/sdk/index.html) Ha az alkalmazás egy Node.js kódmentes.

    * [Visual Studio közösségi 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) vagy újabb, ha az alkalmazás tartalmaz egy .net kódmentes.

* Android 2.3-as vagy újabb, a Google tárházba verzió 27 vagy újabb verzió és a Google Play szolgáltatások 9.0.2 vagy újabb Firebase felhő üzenetkezelés.

* Töltse ki az [első lépések az Android].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Hozzon létre egy projekt, amely támogatja a Firebase felhő üzenetküldés

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Értesítés hubon konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure küldése a leküldéses értesítések beállítása

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>A kiszolgáló projekt leküldéses értesítések engedélyezése

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Leküldéses értesítések felvétele az alkalmazásba

Ebben a részben frissítse az ügyfél miként kezelje a leküldéses értesítések Android alkalmazást.

### <a name="verify-android-sdk-version"></a>Android SDK verziójának ellenőrzése

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

A következő lépésként telepítse a Google Play szolgáltatásokat. A Google Cloud üzenetküldés néhány minimális API szintű követelményei vannak fejlesztés és a tesztelés, amely a **minSdkVersion** tulajdonság a jegyzék meg kell felelnie.

Ha egy régebbi eszközzel, olvassa el a [Állítsa be a Google lejátszása szolgáltatások SDK] határozza meg, hogyan alacsony állítsa ezt az értéket, és állítsa be megfelelően.

### <a name="add-google-play-services-to-the-project"></a>A Google Play szolgáltatások hozzáadása a projekthez

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>A forrás szerkesztésével

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Az alkalmazást a közzétett mobilszolgáltatás tesztelése

Az alkalmazás közvetlen csatolása az Android-telefonon egy USB-kábel, vagy egy virtuális eszközt használ, amely a irányító tesztelheti.

## <a name="more"></a>További

<!-- URLs -->
[Első lépések az Android]: app-service-mobile-android-get-started.md

[A Google Play szolgáltatások SDK beállítása]:https://developers.google.com/android/guides/setup
