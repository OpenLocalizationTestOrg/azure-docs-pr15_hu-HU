<properties
    pageTitle="Értesítés hubok - vállalati architektúra leküldéses"
    description="Útmutatást a vállalati környezetben Azure értesítés hubok használata"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Vállalati leküldéses építészeti útmutató

Fokozatosan mozgó felé létrehozása mobilalkalmazások vagy a végfelhasználók (külső) vagy az alkalmazottaknak, (belső) még ma környezetbe. Meglévő kódmentes rendszerek nagyszámítógépeket vagy valamilyen üzleti alkalmazások integrálni a mobilalkalmazás architektúra kell legyen az rendelkeznek. Ez az útmutató az előadás végezze el a megoldást javaslattétel esetei integrációs legjobb módja.

A gyakori követelmény nem leküldéses értesítést küld a felhasználóknak a mobil alkalmazáson keresztül, a fontos események bekövetkezésekor kódmentes rendszerében. Pl. a banki ügyfele, aki rendelkezik a banki banki alkalmazás saját iPhone-on szeretne értesítést Bank történik a saját fiók vagy egy intranetes forgatókönyv, ahol Pénzügy részlegtől egy alkalmazott, aki rendelkezik egy tervezett jóváhagyási alkalmazást a Windows Phone jóváhagyási kérelem kapjon az általa értesítést szeretne egy bizonyos összeg felett.

A banki vagy jóváhagyás feldolgozás valószínű, el kell végezni néhány kódmentes rendszerben, amely a felhasználó leküldéses kell kezdeményezni. Előfordulhat, hogy több ilyen kódmentes rendszerek, amely kell az összes azonos típusú leküldéses végrehajtásához, ha az esemény eseményindítók értesítést logika összeállítása. Itt összetettségétől integrálása több kódmentes rendszerek együtt a egyetlen leküldéses rendszert, ahol a végfelhasználók számára is előfizetett eltérő értesítést, és még akkor is lehet például az intraneten mobilalkalmazások esetében több mobilalkalmazások egy mobilalkalmazás előfordulhat, hogy amelyre több ilyen kódmentes Systems értesítést kapni a helyezkedik el. A kódmentes rendszerek nem ismeri vagy tudnivalók leküldéses szemantikáját/technológia, így a leggyakoribb megoldás bevezetésére, amely lekérdezi a fontos eseményeket a kódmentes rendszerek és a felelős a leküldéses üzenetek küldése az ügyfélnek összetevő hagyományos lett.
Itt az előadás-még jobb megoldás Azure Service Bus - témakör/előfizetés modell, amely fog egyszerűsítheti miközben a megoldást méretezhető használatával.

Íme a megoldást az általános architektúrája, (általános több mobilalkalmazásokkal, de egyaránt alkalmazható, ha csak egy mobilalkalmazás)

## <a name="architecture"></a>Architektúra

![][1]

A fő darab a szerkezeti diagram Azure Service Bus, amely magában foglalja a témakörök/előfizetések programozási modell (rajta a [szolgáltatás Bus Pub/Sub programozási]több meg). A címzett, a mobil kódmentes (általában [Azure mobilszolgáltatási], amelynek szeretne a mobilalkalmazások leküldéses kezdeményez) ebben az esetben, vagyis nem érkeznek meg üzenetek közvetlenül a kódmentes rendszerek, de inkább van még egy köztes hardverabsztrakciós réteg [Azure Service Bus] , amely lehetővé teszi a mobil kódmentes egy vagy több kódmentes rendszerek érkező üzenetek fogadására által biztosított. A szolgáltatás Bus tematikus kell az egyes, például a fiók ó, amelyek alapvetően "témák" kezdeményez küldeni a leküldéses értesítést, amely a vizsgált Pénzügy kódmentes rendszerek hozható létre. A kódmentes rendszerek üzeneteket küldeni az alábbi témakörök. A mobil Kódmentes szolgáltatás Bus előfizetés létrehozásával feliratkozhat egy vagy több olyan témakörökre hivatkozik. Ez lesz tilthatja meg a megfelelő kódmentes rendszerből értesítést kapnak, hogy a mobil kódmentes. Mobil kódmentes továbbra is a előfizetések üzenetek meghallgatása, és, amint az üzenet megérkezik, azzal kikapcsolja a vissza, és elküldi azt értesítés az értesítési hubon keresztül csatlakozott. Értesítés hubok majd végül kézbesíti az üzenetet a mobile-alkalmazás. Így összefoglalva a fő összetevői, felkínálunk:

1. Kódmentes rendszerek (üzleti vagy régebbi rendszerben)
    - Szolgáltatás Bus témakör létrehozása
    - Üzenet küldése
2. Mobil kódmentes
    - Előfizetése létrehozása
    - Üzenetet kap (Kódmentes rendszerből)
    - Értesítést küld az ügyfelek (keresztül Azure értesítés központi)
3. Mobilalkalmazás
    - Kap, és értesítést jelenít meg

###<a name="benefits"></a>Előnyökkel jár:

1. A címzett (alkalmazás/mobilszolgáltatás értesítés központi keresztül) és a feladó (kódmentes rendszerek) között szétválasztás lehetővé teszi, hogy a minimális módosítás alatt álló integrálódik további kódmentes rendszerek.
2. Ezt az alkalmazási példát, az események érkeznek egy vagy több kódmentes rendszerekből több mobilalkalmazások is lehetővé teszi.  

## <a name="sample"></a>Minta:

###<a name="prerequisites"></a>Előfeltételek
Az alábbi oktatóanyagok megismerkednie a fogalmak megértéséhez, valamint közös létrehozás és konfigurálási lépéseket kell végrehajtania:

1. [Szolgáltatás Bus Pub/Sub programozási] – ez megtudhatja, hogy a részletek, a munkát a szolgáltatási Bus témakörök/előfizetések, hogyan hozhat létre, amely tartalmazhat témakörök/előfizetések névteret & küldése e-üzenetek fogadására hogyan.
2. [Értesítés hubok – Windows univerzális oktatóanyagból] – Ez ismerteti a Windows áruház-alkalmazás beállítása és használata a értesítés hubok regisztrálhat, és kattintson az értesítéseket.

###<a name="sample-code"></a>Minta kódot.

A teljes példakódot [Értesítés központi minták]címen érhető el. A oszlik három összetevőket:

1. **EnterprisePushBackendSystem**

    egy. Ehhez a projekthez a *WindowsAzure.ServiceBus* Nuget-csomagot használ, és [Pub/Sub szolgáltatás Bus programozási]alapján.

    b. Ez a egy egyszerű C# konzol alkalmazás hasonlóan egy üzleti rendszerben, mely az üzenetet a mobile-alkalmazás, kézbesítése kezdeményez.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c billentyűkombinációt. `CreateTopic`létrehozására szolgál a szolgáltatás Bus témakör hol azt küld üzeneteket.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`az üzenetek elküldése szolgáltatás Bus témakör szolgál. Itt azt egyszerűen küld egy sor olyan véletlen üzenetek rendszeres alkalmazásában a mintában a témakör. A szokásos módon lesz a kódmentes rendszert, amely üzeneteket küld egy esemény esetén.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    egy. Ehhez a projekthez a *WindowsAzure.ServiceBus* és *Microsoft.Web.WebJobs.Publish* Nuget csomagot használ, és [szolgáltatás Bus Pub/Sub programozási]alapul.

    b. Ez egy másik C# konzol alkalmazás, amely azt fog futni, és az [Azure WebJob] óta kell az üzleti/kódmentes rendszerekből üzenetek meghallgatása folyamatosan futtatásával. Ez a mobil kódmentes része lesz.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c billentyűkombinációt. `CreateSubscription`létrehozására szolgál a témakör szolgáltatás Bus előfizetést hol a kódmentes rendszer üzeneteket küldeni. Az üzleti esete összetevő hoz létre egy vagy több előfizetések (pl. néhány is kell üzenetek fogadására HR rendszerből, néhány, a rendszer Pénzügy és stb.) megfelelő témakörökre mutató

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. Az üzenetek olvasása át a témakör az előfizetést, és ha sikeres az olvasás majd állítson össze (a minta helyzet Windows natív értesítőjére) értesítést kapni a mobilalkalmazás használatával Azure értesítés hubok ReceiveMessageAndSendNotification szolgál.

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Ez egy **WebJob**, közzétételi, a megoldással a Visual Studio alkalmazásban kattintson a jobb gombbal, és válassza a **Közzététel WebJob**

    ![][2]

    f. Jelölje ki a közzétételi profilját, és Azure új webhely létrehozása, ha még nem létezik már amely fog tárolni a WebJob, és ha a webhely, majd a **Közzététel**már.

    ![][3]

    g. Állítsa be a feladat "Folyamatosan futtatása", hogy amikor megkísérel bejelentkezni az [Azure klasszikus portál] meg kell jelennie egy üzenetet, az alábbihoz hasonló:

    ![][4]


3. **EnterprisePushMobileApp**

    egy. Ez a egy Windows áruház alkalmazást, amelyek az operációs rendszert futtató a mobil kódmentes részeként WebJob az értesítőben szereplő értesítéseket, és jelenítse meg. [Értesítés hubok – Windows univerzális oktatóprogram]alapul.  

    b. Győződjön meg arról, hogy az alkalmazás engedélyezve van-e bejelentési értesítést szeretne kapni.

    c billentyűkombinációt. Győződjön meg arról, hogy regisztráció programkódjának hívott az alkalmazás a következő értesítő hubokon indítási (cseréje a *HubName* és *DefaultListenSharedAccessSignature*: után

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Minta fut:

1. Győződjön meg arról, hogy a WebJob sikeresen operációs rendszer fut, és ütemezve, hogy a "Folyamatosan futtatása".
2. Futtassa a Windows áruházból letölthető elkezdenek **EnterprisePushMobileApp** .
3. Futtassa az **EnterprisePushBackendSystem** konzol alkalmazást, amely az üzleti kódmentes fog hasonlóan, és megkezdi az üzenetek küldése és meg kell jelennie bejelentési értesítés jelenik meg, az alábbihoz hasonló:

    ![][5]

4. Az üzenetek eredetileg küldtek szolgáltatás Bus témaköreit, amelyek a webhely feladatok szolgáltatás Bus előfizetéshez felügyeli volt. Üzenet érkezett, miután értesítést létrehozásának és a mobilalkalmazás küldött. Megtekintheti a WebJob naplókat és a webhely feladatok az [Azure klasszikus portálon] naplók hivatkozás lép, győződjön meg arról, hogy a feldolgozás keresztül:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Értesítés központi minták]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobilszolgáltatás]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Szolgáltatás Bus Pub/Sub programozás]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Értesítés hubok – Windows univerzális oktatóprogram]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure klasszikus portál]: https://manage.windowsazure.com/
