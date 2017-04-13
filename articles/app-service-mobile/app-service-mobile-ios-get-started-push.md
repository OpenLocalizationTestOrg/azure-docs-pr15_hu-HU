<properties
    pageTitle="Leküldéses értesítések hozzáadása iOS alkalmazásban az Azure Mobile-alkalmazások"
    description="Megtudhatja, hogy miként Azure Mobile-alkalmazások használata a leküldéses értesítéseket küldeni az iOS-alkalmazás."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Leküldéses értesítések hozzáadása az iOS-alkalmazás

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>– Áttekintés
Ebben az oktatóanyagban ad hozzá leküldéses értesítéseket az [iOS gyors indítása] projekthez, hogy a leküldéses értesítést küld az eszköz minden alkalommal, amikor a program beszúrja a rekordot.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, szüksége lesz a leküldéses értesítést bővítmény csomag. [Munka a .NET kódmentes kiszolgálóval Azure Mobile-appokról SDK csomagjában talál](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt talál.

Az [iOS simulatort használja nem támogatja a leküldéses értesítéseket](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). A fizikai iOS-eszközön, és az [Apple Fejlesztőeszközök Program tagság](https://developer.apple.com/programs/ios/)van szüksége.

##<a name="configure-hub"></a>Értesítés központi konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Regisztráció a leküldéses értesítések alkalmazásba

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure küldése a leküldéses értesítések beállítása

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Leküldéses értesítések küldésére kódmentes frissítése

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Leküldéses értesítések felvétele az alkalmazásba

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Teszt leküldéses értesítések

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>További

* Sablonok platformok veremmutatót és honosított veremmutatót küldése rugalmasság ad. [Hogyan használja iOS Mobile-appokról Azure ügyfél tár](app-service-mobile-ios-how-to-use-client-library.md#templates) bemutatja, hogyan regisztrálhatja a sablonok.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[rövid útmutató az iOS]: app-service-mobile-ios-get-started.md
