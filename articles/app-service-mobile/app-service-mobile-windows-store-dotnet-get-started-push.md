<properties
    pageTitle="Leküldéses értesítések hozzáadása az univerzális Windows platformon (UWP) alkalmazás |} Azure mobilalkalmazások"
    description="Megtudhatja, hogy miként Azure alkalmazás szolgáltatás Mobile-alkalmazások és Azure értesítés hubok leküldéses értesítéseket küldeni az univerzális Windows platformon (UWP) alkalmazás segítségével."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Leküldéses értesítések hozzáadása a Windows-alkalmazás

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban ad hozzá leküldéses értesítéseket a [rövid Windows start](app-service-mobile-windows-store-dotnet-get-started.md) projekthez, hogy a leküldéses értesítést küld az eszköz minden alkalommal, amikor a program beszúrja a rekordot.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, szüksége lesz a leküldéses értesítést bővítmény csomag. [Munka a .NET kódmentes kiszolgálóval Azure Mobile-appokról SDK csomagjában talál](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt talál.

##<a name="configure-hub"></a>Értesítés hubon konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Regisztráljon az alkalmazás a leküldéses értesítések

Az alkalmazást a Windows áruház küldhetik, majd állítsa be a kiszolgáló projekt integrálása a Windows értesítési szolgáltatások (WNS) leküldéses küldeni szeretné.

1. A Visual Studio megoldás Intézőben kattintson a jobb gombbal a UWP alkalmazás projektet, kattintson a **tár** > **... Áruházzal társítása alkalmazást**. 

    ![Windows Áruházbeli alkalmazás társítása](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. A varázslóban kattintson a **Tovább**gombra, jelentkezzen be Microsoft-fiókjával, **Foglalás új alkalmazás nevét**írja be az alkalmazás nevét, majd kattintson a **Foglalás**parancsra.

3. Az alkalmazás regisztrációs sikeres létrehozását követően jelölje be az új alkalmazás nevére, kattintson a **Tovább**gombra, és válassza a **társítani**. Az alkalmazás jegyzék hozzáadása a Windows áruházból regisztráció szükséges információkat.  

7. Nyissa meg a [Windows fejlesztői központ](https://dev.windows.com/en-us/overview), jelentkezzen be Microsoft-fiókja, kattintson a **saját alkalmazások**az új alkalmazás bejegyzésre, majd bontsa ki a **szolgáltatások** > **leküldéses értesítések**. 

8. A **leküldéses értesítések** lapon kattintson a **webhely Live szolgáltatások** a **Microsoft Azure Mobile szolgáltatások**.

9. A regisztráció lapon jegyezze fel az **alkalmazás titkos kulcsok** és a **Csomag biztonsági AZONOSÍTÓK**, amelyek ezután használni kívánt konfigurálása a mobilalkalmazásban kódmentes érték. 

    ![Windows Áruházbeli alkalmazás társítása](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Az ügyfél titkos és biztonsági AZONOSÍTÓK csomagot olyan fontos hitelesítő adatokat. Ne bárkivel megoszthatja a ezeket az értékeket és terjesztése őket az alkalmazást. Az **Alkalmazás azonosítója** Microsoft Account hitelesítés konfigurálása a titkos használatos.

##<a name="configure-the-backend-to-send-push-notifications"></a>A fájlok küldése a leküldéses értesítések beállítása

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Frissítés a kiszolgálót, küldje el a leküldéses értesítések

Használja az alábbi eljárást, amely megfelel a kódmentes projekttípus&mdash; [.NET kódmentes](#dotnet) vagy a [Node.js kódmentes](#nodejs).

### <a name="dotnet"></a>.NET kódmentes projekt

1. A Visual Studióban kattintson a jobb gombbal a kiszolgáló projekt **NuGet csomagok kezelése**gombra, Microsoft.Azure.NotificationHubs kereséséhez, majd kattintson a **telepítés**gombra. Ez az értesítés hubok ügyfél tár a telepítést.

2. Bontsa ki a **vezérlők**, nyissa meg a TodoItemController.cs, és adja hozzá a következő utasítások segítségével:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. A **PostTodoItem** módszer után **InsertAsync**a hívást adja hozzá a következő kódot:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Ez a kód azt jelenti, hogy az után az új elem beszúrási leküldéses értesítést küldhet értesítést-központban.

4. A kiszolgáló projekt közzé.

### <a name="nodejs"></a>NODE.js kódmentes projekt

1. Ha azt még nem tette meg, [Töltse le a quickstart útmutató projekt](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , különben [az Azure-portálon online szerkesztő](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. A meglévő kódot a todoitem.js fájlban lecserélése a következőre:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Ez egy új teendő elem beszúrásakor a item.text tartalmazó WNS értesítőjére küldése

2. Szerkeszti a fájlt a helyi számítógépen, amikor ismét közzéteheti a kiszolgáló-projektet.

##<a id="update-app"></a>Leküldéses értesítések felvétele az alkalmazásba

Ezután az alkalmazás regisztrálnia kell az indításkor a leküldéses értesítéseket. Ha már engedélyezte a hitelesítés, győződjön meg arról, hogy a felhasználó jelek modul mielőtt megpróbálja regisztrálni leküldéses értesítéseket.

1. Nyissa meg a **App.xaml.cs** projekt-fájlját, és adja hozzá a következő `using` kimutatások:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Az ugyanannak a fájlnak az **alkalmazás** osztály a következő **InitNotificationsAsync** módszer definíciót hozzáadása:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Ez a kód a ChannelURI alkalmazás WNS, és az adott ChannelURI majd regisztrálja az alkalmazás Mobile-alkalmazás.

3. A **App.xaml.cs** **OnLaunched** eseménykezelő tetején az **aszinkron** módosító hozzáadása a módszer definícióját, és adja hozzá az új **InitNotificationsAsync** módszer, ahogy a következő példában a következő hívást:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Ez biztosítja, hogy a rövid életű ChannelURI regisztrálva van az alkalmazás minden egyes indításkor.

4. A UWP alkalmazás projekt újraépítéséhez. Az alkalmazás készen áll a bejelentési értesítéseket.

##<a id="test"></a>Teszt leküldéses értesítéseket, az alkalmazásban

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Következő lépések

További információ a leküldéses értesítések:

* [A felügyelt ügyfél használata Azure Mobile-appokról](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Sablonok platformok veremmutatót és honosított veremmutatót küldése rugalmasság ad. Megtudhatja, hogy miként regisztrálhatja a sablonokat.

* [Leküldéses értesítést problémáinak diagnosztizálása](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Okból különböző miért értesítések kerülhetnek, vagy nem a végeredmény eszközökön. Ez a témakör bemutatja, hogyan elemzése és találnia, hogy a leküldéses értesítést hibák okát. 

Fontolja meg a Folytatás megjelenítheti az alábbi oktatóanyagok egyikét:

+ [Hitelesítés felvétele az alkalmazásba](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Megtudhatja, hogy miként tel az alkalmazás felhasználóinak hitelesítést végezni.

+ [Az alkalmazás offline szinkronizálás engedélyezése](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Megtudhatja, hogy miként hozzáadása az offline munka támogatása az alkalmazás-mobilalkalmazás kódmentes használatával. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

