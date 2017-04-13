<properties
    pageTitle="Azure értesítés hubok biztonságos leküldéses"
    description="Megtudhatja, hogy miként biztonságos leküldéses értesítéseket küldeni az Azure-ban. A C# a .NET API írt mintakódok."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure értesítés hubok biztonságos leküldéses

> [AZURE.SELECTOR]
- [A Windows univerzális](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-alapú](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>– Áttekintés

Leküldéses értesítést támogatása a Microsoft Azure lehetővé teszi, hogy könnyen használható, többplatformos, amelyekkel méretarányos telepítés leküldéses infrastruktúra nagyban egyszerűsíti a leküldéses értesítések egyszerre fogyasztói és vállalati alkalmazások mobil platformokon történő elérésére.

Szabályozási miatt vagy biztonsági kényszerek, előfordul, hogy az alkalmazás előfordulhat, hogy szeretne foglalni valamit, amit az ablakokat, amelyek nem kell továbbítani a szabványos leküldéses értesítést infrastruktúra keresztül. Ebben az oktatóanyagban az ugyanazon élmény elérése küld a bizalmas információkat az ügyféleszközről és az alkalmazás kódmentes közötti biztonságos, hitelesített kapcsolaton keresztül ismerteti.

Magas szintű a folyamat a következőképpen történik:

1. Az alkalmazás háttéradatbázist:
    - Tárolók biztonságos tartalom háttéradatbázist.
    - Ez az értesítés azonosítója küld az eszköz (nem biztonságos küld adatokat a).
2. Az alkalmazás az eszközön, amikor a értesítést kap:
    - Az eszköz csatlakozik a háttéradatbázist kérése a biztonságos tartalom.
    - Az alkalmazás megjelenítheti a tartalom, az eszközön értesítést.

Fontos tudni, hogy a fenti folyamat (és ebben az oktatóanyagban) feltételezzük, hogy az eszköz tárol egy hitelesítési jogkivonat helyi tároló után a felhasználó bejelentkezik a. Ez a teljesen zökkenőmentes változat, garantálja, mint az eszközre az értesítés biztonságos tartalom a token használatával meghallgathatja. Ha az alkalmazás nem tárolja hitelesítési tokenek az eszközre, vagy ha tokenek lejárt az eszköz alkalmazást, az értesítés fogadásakor megjelenjen-e egy általános értesítést a felhasználótól indítsa el az alkalmazást. Az alkalmazás hitelesíti a felhasználót, majd az értesítési tartalom jeleníti meg.

A biztonságos leküldéses szabhatja biztonságosan leküldéses értesítést küldhet. Az oktatóprogram, célszerű lépések elvégzése, hogy az oktatóanyagban először a [Felhasználók értesítést](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) oktatóprogram épül.

> [AZURE.NOTE] Ebben az oktatóanyagban feltételezi, hogy létrehozása és beállítása a értesítés központi, [Értesítés hubok (Windows áruház) – első lépések](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)leírt módon.
Is ne feledje, hogy Windows Phone 8.1 (nem a Windows Phone) a Windows hitelesítő adatokat kér, hogy a háttérben futó feladatokat nem működnek a Windows Phone 8.0-ban és a Silverlight 8.1 rendszerben. A Windows áruház-alkalmazás, akkor kap értesítést keresztül háttér tevékenység csak akkor, ha az alkalmazás a zárolási képernyőn engedélyezve (a jelölőnégyzetre a Appmanifest az).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>A Windows Phone projekt módosítása

1. App.xaml.cs rögzítése a rajzszög háttér tevékenység a következő kód hozzáadása a **NotifyUserWindowsPhone** projekt. A következő sort a kód hozzáadása a végén a `OnLaunched()` módszer:

        RegisterBackgroundTask();

2. Továbbra is a App.xaml.cs, adja hozzá a következő kód közvetlenül azután, hogy a `OnLaunched()` módszer:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Adja hozzá a következő `using` kimutatások App.xaml.cs fájl tetején:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. A **fájl** menüben, a Visual Studióban az **Összes mentése**gombjára.

## <a name="create-the-push-background-component"></a>A leküldéses háttér összetevő létrehozása

A következő lépésként hozzon létre a leküldéses háttér összetevőt.

1. A megoldás Intézőben kattintson a jobb gombbal a legfelső szintű csomópontot a megoldás (ebben az esetben**megoldás SecurePush** .), majd kattintson a **Hozzáadás**gombra, majd kattintson az **Új projekt**.

2. Bontsa ki a **Áruházból alkalmazásokat**, kattintson a **Windows Phone-alkalmazások**, majd kattintson a **Windows-futási idejű összetevőt (Windows Phone)**. A projekt **PushBackgroundComponent**nevet, és kattintson az **OK** gombra a projekt létrehozásához.

    ![][12]

3. A megoldás Intézőben kattintson a jobb gombbal a **PushBackgroundComponent (Windows Phone 8.1)** -projektet, kattintson a **Hozzáadás**gombra, majd kattintson az **Osztályjegyzetfüzet**. Adjon nevet az új osztály **PushBackgroundTask.cs**. Kattintson a **Hozzáadás** az osztályjegyzetfüzet létrehozásához.

4. A **PushBackgroundComponent** névtér definíció teljes tartalmának cserélje ki az alábbi kódot, helyettesítése a helyőrző `{back-end endpoint}` a háttéradatbázist végponttal kapott üzembe helyezése a háttéradatbázist közben:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. A megoldás Intézőben kattintson a jobb gombbal a **PushBackgroundComponent (Windows Phone 8.1)** projekt, és kattintson a **NuGet csomagok kezelése**gombra.

6. A bal oldalon kattintson az **Online**elemre.

7. A **Keresés** mezőbe írja be a **HTTP-ügyfél**.

8. Eredmények listájában kattintson a **Microsoft HTTP ügyfél tárak**elemet, és kattintson a **telepítés**gombra. A telepítés befejezéséhez.

9. Vissza a NuGet a **Keresés** mezőbe írja be a **Json.net**. Telepítse a **Json.NET** csomagot, majd zárja be a NuGet csomag Manager ablakot.

10. Adja hozzá a következő `using` kimutatások **PushBackgroundTask.cs** fájl tetején:

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Megoldás Explorer, a **NotifyUserWindowsPhone (Windows Phone 8.1)** projektben kattintson a jobb gombbal a **hivatkozást**, majd kattintson a **Hivatkozás hozzáadása**gombra. A hivatkozás-kezelő párbeszédpanel **PushBackgroundComponent**melletti jelölőnégyzetet, és kattintson **az OK**gombra.

12. A megoldás Intézőben duplán **Package.appxmanifest** a **NotifyUserWindowsPhone (Windows Phone 8.1)** a projektben. A **értesítések**területen állítsa **Bejelentési alkalmas** **Igen**.

    ![][3]

13. Továbbra is **Package.appxmanifest**, kattintson a **deklarációs** menü felső részén. A **Rendelkezésre álló deklarációs** tartalmazó legördülő listára kattintson a **Háttérben futó feladatokat**, és kattintson a **Hozzáadás**gombra.

14. **Package.appxmanifest** **tulajdonságai**csoportban jelölje be **a leküldéses értesítést**.

15. **Package.appxmanifest**, az **Alkalmazás beállításai**csoportban írja be a **PushBackgroundComponent.PushBackgroundTask** a **Belépési pontjához** mezőben.

    ![][13]

16. A **fájl** menüben kattintson az **Összes mentése**gombra.

## <a name="run-the-application"></a>Futtassa az alkalmazást

Az alkalmazás futtatásához tegye a következőket:

1. A Visual Studióban a **AppBackend** webes API alkalmazásnak a futtatására. ASP.NET-weblap jelenik meg.

2. A Visual Studióban futtassa a Windows Phone alkalmazás **(Windows Phone 8.1) NotifyUserWindowsPhone** . A Windows Phone irányító fut, és az alkalmazás automatikusan betölti.

3. A alkalmazásban **NotifyUserWindowsPhone** felhasználói felület adja meg a felhasználónevet és jelszót. Ezek a karakterlánc lehet, de ugyanazt az értéket kell lenniük.

4. Kattintson a **NotifyUserWindowsPhone** app felhasználói felület **Jelentkezzen be és regisztrálását**. Kattintson a **Küldés a leküldéses**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
