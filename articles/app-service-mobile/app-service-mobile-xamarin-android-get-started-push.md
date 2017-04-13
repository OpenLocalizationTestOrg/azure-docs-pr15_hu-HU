<properties
    pageTitle="Leküldéses értesítések hozzáadása a Xamarin.Android alkalmazás |} Azure alkalmazás szolgáltatás"
    description="Azure alkalmazás szolgáltatás és Azure értesítés hubok használata a leküldéses értesítéseket küldeni az Xamarin.Android alkalmazás"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Leküldéses értesítések felvétele a Xamarin.Android alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>– Áttekintés


Ebben az oktatóanyagban ad hozzá leküldéses értesítéseket a [Xamarin.Android gyors indítása](app-service-mobile-windows-store-dotnet-get-started.md) projekthez, hogy a leküldéses értesítést küld az eszköz minden alkalommal, amikor a program beszúrja a rekordot.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, szüksége lesz a leküldéses értesítést bővítmény csomag. [Munka a .NET kódmentes kiszolgálóval Azure Mobile-appokról SDK csomagjában talál](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt talál.


##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban az alábbiakra van szükség:

+ Aktív a Google-fiókot. Iratkozzon fel a Google-fiók a [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ A [Google Cloud üzenetküldés ügyfél-összetevő](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Értesítés hubon konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Firebase engedélyezése a felhő üzenetküldés

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Azure leküldéses kérelmek küldésére konfigurálása

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Leküldéses értesítések küldésére a kiszolgáló projekt frissítése

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Az ügyfél projekt leküldéses értesítések beállítása

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Leküldéses értesítések kódot az alkalmazás hozzáadása

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Teszt leküldéses értesítéseket, az alkalmazásban

Az alkalmazás tesztelheti a irányító virtuális eszköz használatával. Vannak olyan irányító forgalmi további beállítási lépéseket.

1. Ellenőrizze, hogy bevezetéshez vagy egy virtuális eszközön, amelyen a Google API-hoz a célként beállítása az Android virtuális eszköz (AVD) manager alább látható módon hibakeresési vannak.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Google-fiók hozzáadása az Android-készülék **alkalmazások**kattintva > **Beállítások** > **fiók hozzáadása**, majd kövesse a képernyőn megjelenő utasításokat.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Futtassa az listájába alkalmazást, mielőtt, és szúrja be a Teendők új elemet. Ebben az esetben egy értesítési ikon az értesítési területre jelölőnégyzetet. Az értesítés teljes szövegének megjelenítése az értesítési fiókban is megnyithatja.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
