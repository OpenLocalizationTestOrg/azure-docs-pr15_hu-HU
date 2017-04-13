<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet a Windows Phone a Silverlight-alkalmazások – első lépések"
    description="Megtudhatja, hogy miként Azure Mobile tetszés szerint elmélyedhet használata a analitikai és a leküldéses értesítések Windows Phone-Silverlight-alkalmazások."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Azure Mobile tetszés szerint elmélyedhet a Windows Phone a Silverlight-alkalmazások – első lépések

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használni, az alkalmazás használatát és leküldéses értesítések Windows Phone-Silverlight-alkalmazás szegmentált felhasználóknak elküldendő.
Ebben az oktatóprogramban az egyszerű közvetítési forgatókönyv Mobile tetszés szerint elmélyedhet használatával mutatja be. Hozzon létre egy üres Windows Phone-Silverlight-alkalmazás által gyűjtött adatokat, és használja a Microsoft leküldéses értesítést szolgáltatás (MPNS) a leküldéses értesítések fogadása.

> [AZURE.NOTE] Ha Windows Phone 8.1 (nem Silverlight) céloz, keresse meg a [Windows univerzális oktatóprogram](mobile-engagement-windows-store-dotnet-get-started.md).

Ebben az oktatóprogramban az alábbiakra van szükség:

+ Visual Studio 2013-ban
+ [MicrosoftAzure.MobileEngagement] Nuget csomag

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>A telepítő mobil tetszés szerint elmélyedhet a Windows Phone-alkalmazás

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció", amely a minimálisan szükséges adatok összegyűjtése és küldje el a leküldéses értesítést jeleníti meg. A teljes integrációs dokumentációt megtalálható a [Mobile tetszés szerint elmélyedhet Windows Phone SDK-integráció](mobile-engagement-windows-phone-sdk-overview.md)

A Visual Studio keresztül mutatjuk integrációja azt hoz létre egy egyszerű alkalmazást.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Windows Phone Silverlight új projekt létrehozása

A következő lépések feltételezik a Visual Studio 2015 használatát, mintha lépések hasonlóak a Visual Studio korábbi verzióiban. 

1. Indítsa el a Visual Studióban, és a **Kezdőlap** képernyőn jelölje be az **Új projekt**.

2. Az előugró ablakban, jelölje be a **Windows 8** -> **Windows Phone** -> **Üres alkalmazás (Windows Phone Silverlight)**. Írja be az alkalmazás **nevét** és a **megoldás neve**, és kattintson **az OK**gombra.

    ![][1]

3. Megadhatja a **Windows Phone 8.0** vagy a **Windows Phone 8.1**célba.

Most már hozott létre, amelybe azt fogja integrálása az Azure Mobile tetszés szerint elmélyedhet SDK új Windows Phone Silverlight alkalmazás.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

1. Telepítse a [MicrosoftAzure.MobileEngagement] nuget csomag a projektben.

2. Nyissa meg `WMAppManifest.xml` (a mappán Tulajdonságok), és ellenőrizze, hogy a következő vannak deklarálva (hozzá, ha még nem) a a `<Capabilities />` címke:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Most illessze be a kapcsolati karakterlánc, amely a Mobile tetszés szerint elmélyedhet alkalmazás előbb másolt, és illessze be a `Resources\EngagementConfiguration.xml` között a fájl a `<connectionString>` és `</connectionString>` címkék:

    ![][3]

4. Az a `App.xaml.cs` fájl:

    egy. Adja hozzá a `using` utasítás:

            using Microsoft.Azure.Engagement;

    b. A SDK csomagjában talál a inicializálni a `Application_Launching` módszer:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c billentyűkombinációt. A következő beszúrása a `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Valós idejű ellenőrzése engedélyezése

Adatok küldése, és annak biztosítására, hogy a felhasználók aktív indításához legalább egy képernyővel (a tevékenység) kell küldenie a Mobile tetszés szerint elmélyedhet kódmentes.

1. A MainPage.xaml.cs, vegye fel a `using` utasítás:

        using Microsoft.Azure.Engagement;

2. Cserélje ki az alap osztály **MainPage**, amely **PhoneApplicationPage**előtt, a **EngagementPage**.

        class MainPage : EngagementPage 
    
3. Az a `MainPage.xml` fájl:

    egy. A névtér deklarációs hozzáadása:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Csere `phone:PhoneApplicationPage` az XML-címke neve és a `engagement:EngagementPage`.

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Mobil tetszés szerint elmélyedhet lehetővé teszi interaktív módon, és a felhasználók a leküldéses értesítések és az alkalmazás üzenetküldés kampányok környezetében is elérheti. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
Az alábbi szakaszok is kapni az alkalmazás beállítása.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>MPNS leküldéses értesítést szeretne kapni az alkalmazás engedélyezése

Az új funkciók hozzáadása a `WMAppManifest.xml` fájl:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>A GYERMEKEKTŐL SDK inicializálni

1. A `App.xaml.cs`, hívja fel `EngagementReach.Instance.Init();` után a ügynök inicializálni jobbra a **Application_Launching** függvény:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. A `App.xaml.cs`, hívja fel `EngagementReach.Instance.OnActivated(e);` után a ügynök inicializálni jobbra a **Application_Activated** függvény:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Ezzel el is készült. Most már azt ellenőrzi, hogy meg megfelelően meg ez az egyszerű integráció van Mézeskalács.

##<a id="send"></a>Értesítés küldése az alkalmazásba

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Kell most jelenik meg értesítés az eszközön, amely megjelenik, az alkalmazás értesítjük esetén az alkalmazás Megnyitás egyébként egy értesítőjére, az alábbihoz hasonló: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
