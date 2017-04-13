<properties
    pageTitle="Azure mobil részvételét Xamarin.iOS az első lépések"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések Azure Mobile tetszés szerint elmélyedhet használata Xamarin.iOS alkalmazások."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Azure mobil részvételét-Xamarin.iOS alkalmazások – első lépések

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használni, és küldje el a leküldéses értesítéseket szegmentált felhasználók Xamarin.iOS alkalmazásban az alkalmazás használatát.
Ebben az oktatóanyagban hoz létre, amely egyszerű adatait gyűjti össze, és az Apple leküldéses értesítést rendszer (APNS) használata a leküldéses értesítések fogadása üres Xamarin.iOS alkalmazást.

Ebben az oktatóprogramban az alábbiakra van szükség:

+ [Xamarin Studio](http://xamarin.com/studio). Visual Studio az Xamarin is használhatja, de ebben az oktatóanyagban Xamarin Studio használja. Telepítési című cikkben olvashat [a telepítő, és telepítse az Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobil tetszés szerint elmélyedhet Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Mobil tetszés szerint elmélyedhet beállítása az iOS-alkalmazás

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció" van-e a minimális beállítva szükséges adatok összegyűjtése és küldje el a leküldéses értesítést, amely mutatja be.

Egy egyszerű alkalmazás Xamarin keresztül mutatjuk integrálása a hozunk létre:

###<a name="create-a-new-xamarinios-project"></a>Xamarin.iOS új projekt létrehozása

1. Indítsa el a Xamarin Studio. Nyissa meg a **fájl** -> **Új** -> **megoldás** 

    ![][1]

2. Jelölje ki **Egyetlen nézetben alkalmazást**, győződjön meg arról, hogy a kijelölt nyelven **C#** , és kattintson a **Tovább**gombra.

    ![][2]

3. Írja be az **Alkalmazás neve** és a **Szervezet azonosítója** , és kattintson a **Tovább**gombra. 

    ![][3]

    > [AZURE.IMPORTANT] Ügyeljen arra, hogy a közzétételi profilt, végül használatával az iOS-alkalmazás terjesztése van használja az alkalmazás azonosítója, amely megegyezik pontosan itt az első lépésekhez azonosítója. 

4. Szükség esetén frissítse az a **Projekt nevét**, **Megoldás nevét** és **helyét** , és kattintson a **Létrehozás**gombra.

    ![][4]
 
Xamarin Studio hoz létre a bemutató alkalmazást, amelyben Mobile tetszés szerint elmélyedhet integrálja azt. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása Mobile tetszés szerint elmélyedhet kódmentes

1. Kattintson a jobb gombbal a megoldás windows- **csomagok** mappa, és válassza a **Csomagok hozzáadása**

    ![][5]

2. A **Microsoft Azure Mobile tetszés szerint elmélyedhet Xamarin SDK** kereshet, és vegye fel a megoldás.  

    ![][6]
   
3. Nyissa meg a **AppDelegate.cs** , és adja hozzá a következő utasítással:

        using Microsoft.Azure.Engagement.Xamarin;

4. Az a **FinishedLaunching** módot adja meg az alábbi módon inicializálni a kapcsolatot a Mobile tetszés szerint elmélyedhet kódmentes. Ellenőrizze, hogy a **ConnectionString**hozzáadni. Ez a kód is használ, egy üres **NotificationIcon** a Mobile tetszés szerint elmélyedhet SDK csomagjában talál, amelyek esetleg szeretné helyettesíteni által hozzáadott. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Valós idejű ellenőrzése engedélyezése

Annak érdekében, hogy adatokat küldi és annak biztosítására, a felhasználók aktív elindításához legalább egy képernyővel kell küldenie a Mobile tetszés szerint elmélyedhet kódmentes.

1. Nyissa meg a **ViewController.cs** , és adja hozzá a következő utasítással:

        using Microsoft.Azure.Engagement.Xamarin;

2. Az osztály, amelyből cseréje `ViewController` örökli `UIViewController` való `EngagementViewController`. 

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Mobil tetszés szerint elmélyedhet lehetővé teszi, hogy a felhasználók kommunikálni, és a leküldéses értesítések és az-app kampányok környezetében üzenetküldés is ELÉRHETI. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
Az alábbi szakaszok is kapni az alkalmazás beállítása.

### <a name="modify-your-application-delegate"></a>Az alkalmazás meghatalmazott módosítása

1. Nyissa meg a **AppDelegate.cs** , és adja hozzá a következő utasítással:

        using System; 

2. Most már belül a `FinishedLaunching` módot, adja hozzá a következő regisztrálhatja leküldéses üzenetek után`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Végül - frissítése vagy az alábbi módszerek hozzáadása:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. A **Info.plist** fájlra a megoldást győződjön meg arról, hogy **Az első lépésekhez azonosító** megfelel-e az **Alkalmazás azonosítója** az Apple fejlesztői központ a kiépítési profilja is van. 

    ![][7]

5. Ugyanazt a **Info.plist** fájlt győződjön meg arról, hogy be van jelölve a **Háttér mód engedélyezése** és a **Távoli értesítéseket**. 

    ![][8]

6. Futtassa az alkalmazást az eszközön, közzétételi profil társítva van. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
