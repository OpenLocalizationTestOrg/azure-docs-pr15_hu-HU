<properties
    pageTitle="Leküldéses értesítések az Azure értesítés hubok küldése a Windows Phone |} Microsoft Azure"
    description="Ebből az oktatóanyagból megismerheti, hogyan Azure értesítés hubok használatával a leküldéses értesítések Windows Phone 8 vagy Windows Phone 8.1 Silverlight alkalmazásba."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="leküldéses értesítést, a leküldéses értesítések windows phone leküldéses"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Leküldéses értesítések az Azure értesítés hubok küldése a Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Ebben az oktatóanyagban bemutatja, hogyan leküldéses értesítéseket küldeni a Windows Phone 8 vagy Windows Phone 8.1 Silverlight-alkalmazások Azure értesítés hubok használatával. Ha céloz Windows Phone 8.1 (nem Silverlight), majd olvassa el a [Windows univerzális](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) verziót.
Ebben az oktatóanyagban hozzon létre egy üres Windows Phone 8-alkalmazást, hogy a leküldéses értesítések fogadása a Microsoft leküldéses értesítést szolgáltatás (MPNS) használatával. Ha végzett, is használhatja az értesítési központi szétküldése fut, az alkalmazás az összes eszközök leküldéses értesítéseket.

> [AZURE.NOTE] Az értesítési hubok Windows Phone SDK nem támogatja a Windows leküldéses értesítést szolgáltatás (WNS) használata a Windows Phone 8.1 Silverlight-alkalmazások használatát. A Windows Phone 8.1 Silverlight-alkalmazások használatát WNS (helyett MPNS) használatához kövesse az [Értesítési hubok – Windows Phone Silverlight oktatóprogram]REST API-khoz használó.

Az oktatóprogram értesítés hubok használata egyszerű közvetítési forgatókönyv mutatja be.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban az alábbiakra van szükség:

+ [Visual Studio 2012 Express a Windows Phone], vagy újabb verziója.

Ebben az oktatóanyagban befejezése rendszer minden más értesítés hubok oktatóanyagok-alkalmazások Windows Phone 8.

##<a name="create-your-notification-hub"></a>Az értesítés központi létrehozása

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Kattintson az <b>Értesítési szolgáltatások</b> szakasz (a <i>Beállítások</i>), kattintson a <b>Windows Phone (MPNS)</b> , és válassza a <b>nem hitelesített leküldéses engedélyezése</b> jelölőnégyzetet.</p>
</li>
</ol>

&emsp;&emsp;![Azure portál - engedélyezése unathenticated leküldéses értesítések](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

A központi most már létrehozott és beállítva, hogy nem hitelesített-értesítés a Windows Phone küldjön.

> [AZURE.NOTE] Ebben az oktatóanyagban MPNS nem hitelesített módban használja. Nem hitelesített módban MPNS korlátozásokkal értesítéseket, amelyek az egyes csatornák elküldheti származik. Értesítés hubok támogatja a [MPNS hitelesített mód](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) azáltal, hogy töltse fel a tanúsítvány.

##<a name="connecting-your-app-to-the-notification-hub"></a>Kapcsolódás az alkalmazást az értesítési-központban

1. A Visual Studióban hozzon létre egy új Windows Phone 8-alkalmazást.

    ![Visual Studio - új Project – Windows Phone alkalmazás][13]

    Visual Studio 2013 frissítés 2 vagy újabb ehelyett a Windows Phone-Silverlight-alkalmazások hozzon létre.

    ![Windows Phone Silverlight üres Visual Studio - új projekt - alkalmazás –][11]

2. A Visual Studióban kattintson a jobb gombbal a megoldást, és kattintson a **NuGet csomagok kezelése**gombra.

    Ekkor megjelenik a **NuGet csomagok kezelése** párbeszédpanel.

3. Keressen `WindowsAzure.Messaging.Managed` , és kattintson a **telepítés**gombra, és majd fogadja el a használati feltételei.

    ![Visual Studio - NuGet csomag Manager][20]

    Ez a letöltések, telepíti, és hozzáadja a hivatkozást az Azure üzenetküldés könyvtárra a Windows az <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet csomag</a>használatával.

4. Nyissa meg a fájlt App.xaml.cs, és adja hozzá a következő `using` kimutatások:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Adja hozzá a következő kódot App.xaml.cs **Application_Launching** módszer tetején:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] **MyPushChannel** érték, amely egy meglévő csatornát, a [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) gyűjteményben keresési index. Ha nem egy van, hozzon létre egy új néven.
    
    Ellenőrizze, hogy a központi és a kapcsolati karakterlánc nevű **DefaultListenSharedAccessSignature** szerezte be az előző szakasz beszúrásához.
    Kód a csatorna URI alkalmazás MPNS, és az adott csatornához URI majd regisztrálja az értesítési központi. Azt is biztosítja, hogy a csatorna URI regisztrálva van a értesítés-központban az alkalmazás minden egyes indításkor.

    >[AZURE.NOTE]Ebben az oktatóanyagban bejelentési értesítést küld az eszközt. Amikor csempe értesítést küld, helyette kell hívnia **BindToShellTile** módszer a csatornán. Mindkét bejelentési támogatja, és értesítések csempe, hívja a **BindToShellTile** és **BindToShellToast**is.

6. A megoldás Intézőben bontsa ki a **Tulajdonságok**, nyissa meg a `WMAppManifest.xml` fájlt, kattintson a **Funkciók** fülre, és győződjön meg arról, hogy be van-e jelölve a **ID_CAP_PUSH_NOTIFICATION** lehetőséget.

    ![Visual Studio - Windows Phone alkalmazás-funkciók][14]

    Ezzel biztosíthatja, hogy az alkalmazás kaphatnak a leküldéses értesítéseket. Bármely kísérlet leküldéses értesítést küld az alkalmazás nélkül sikertelen lesz.

7. Nyomja le a `F5` billentyű lenyomásával futtassa az alkalmazást.

    Regisztráció üzenet jelenik meg az alkalmazást.

8. Zárja be az alkalmazás.  

   >[AZURE.NOTE] Bejelentési leküldéses értesítést kapnak, hogy az alkalmazás nem futnia kell az előtérben.

##<a name="send-push-notifications-from-your-backend"></a>Leküldéses értesítések küldése a kódmentes

Leküldéses értesítések elküldheti a nyilvános <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">többi felületén</a>keresztül bármely kódmentes értesítés hubok használatával. Ebben az oktatóanyagban .NET konzol alkalmazást használ, a leküldéses értesítéseket küldeni. 

Példa egy ASP.NET WebAPI kódmentes, amely értesítés hubok integrálva a leküldéses értesítéseket küldeni az [Azure értesítés hubok .NET-fájlok értesíti a felhasználók](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md)című cikkben.  

Példa a [REST API-hoz](https://msdn.microsoft.com/library/azure/dn223264.aspx)a leküldéses értesítéseket küldeni tanulmányozza [Java az értesítési hubok használatáról](./notification-hubs-java-push-notification-tutorial.md) és a [PHP az értesítési hubok használatáról](./notification-hubs-php-push-notification-tutorial.md).

1. Kattintson a jobb gombbal a megoldást, **hozzáadása** és az **Új projekt...**, jelölje be a **Visual C#**, kattintson a **Windows** és a **New**, és kattintson az **OK gombra**.

    ![Visual Studio - új projekt - konzol alkalmazás][6]

    Ez a lehetőség egy új vizuális C# konzol alkalmazást a megoldáshoz. Szintén ezt megteheti a külön megoldás.

4. Kattintson az **eszközök**, kattintson a **Tár csomag Manager**, és válassza a **Csomag kezelője konzol**.

    A csomag kezelője konzol jeleníti meg.

5.  **Csomag kezelője konzol** ablakában a **projekt alapértelmezett** beállítása a konzol alkalmazás új projekt, és majd konzol ablakában végre az alábbi parancsot:

        Install-Package Microsoft.Azure.NotificationHubs

    Ez a lehetőség a <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification hubok NuGet csomag</a>használata Azure-értesítés hubok SDK mutató hivatkozás.

6. Nyissa meg a `Program.cs` fájlt, és adja hozzá a következő `using` utasítás:

        using Microsoft.Azure.NotificationHubs;

6. Az a `Program` osztály, és adja meg az alábbi módon:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Győződjön meg arról, hogy le szeretné cserélni a `<hub name>` az értesítési-központban, a portálon megjelenő nevének helyőrzője. Emellett a kapcsolati karakterlánc helyőrző cserélje ki a kapcsolati karakterlánc szerezte be **DefaultFullSharedAccessSignature** című szakaszában "Beállítás a értesítés-központját."

    >[AZURE.NOTE]Győződjön meg arról, hogy használjon **teljes** hozzáférést, az access nem **meghallgatása** a kapcsolati karakterlánc. A figyelés-hozzáférési karakterlánc nincs engedélye a leküldéses értesítéseket küldeni.

4. A következő sor hozzáadása a `Main` módszer:

         SendNotificationAsync();
         Console.ReadLine();

5. A Windows Phone irányító futtatása és az alkalmazás zárva, állítsa a konzol alkalmazás projekt az alapértelmezett indítási projekt, és nyomja le a `F5` billentyű lenyomásával futtassa az alkalmazást.

    Egy bejelentési leküldéses értesítést kap. Koppintson az értesítőben szereplő transzparens betölti az alkalmazást.

A az [értesítőben szereplő katalógus] és a [csempe az alkalmazáskatalógus] témakörök MSDN webhelyen található összes lehetséges tartalmak számára.

##<a name="next-steps"></a>Következő lépések

Egyszerű ebben a példában akkor a Windows Phone 8-eszközök leküldéses értesítéseket rendelkezésére bocsát. 

Annak érdekében, hogy a célként megadott felhasználók, olvassa el a [Használati értesítés hubok a felhasználóknak a leküldéses értesítések] oktatóprogram. 

Oszthatja fel a felhasználók kamat csoportok szerint szeretné, ha erről [Használata értesítés hubok küldése legfrissebb híreket]. 

További tudnivalók az értesítési hubok haszná [Értesítés hubok útmutatást].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Windows Phone Visual Studio 2012 Express]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Értesítés hubok útmutató]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Értesítés hubok segítségével felhasználók a leküldéses értesítések]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Legfrissebb hírek küldése értesítés hubok használatával]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[bejelentési katalógus]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[katalógus csempe]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Értesítés hubok – a Windows Phone Silverlight oktatóprogram]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

