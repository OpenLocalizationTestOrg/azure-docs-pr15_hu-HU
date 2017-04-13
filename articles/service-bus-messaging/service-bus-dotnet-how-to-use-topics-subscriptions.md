<properties
    pageTitle="Szolgáltatás Bus témakörök használata .NET |} Microsoft Azure"
    description="Megtudhatja, hogy miként szolgáltatás Bus témakörök és előfizetések használata .NET Azure-ban. Mintakódok .NET-alkalmazásokhoz kerülnek."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Szolgáltatás Bus témakörök és előfizetések használatával

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez a cikk ismerteti, hogyan szolgáltatás Bus témakörök és előfizetések használatára. A minták írt C#, és a .NET API-k használata. Az érintett esetek létrehozása, témák és előfizetés szűrők létrehozásával, üzeneteket küld a témakör, előfizetésből üzenetek fogadására és témák és az előfizetés törlése az előfizetések. További információt a témakörök és előfizetések című [következő lépések](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>A szolgáltatás Bus használni kívánt alkalmazást konfigurálása

Az alkalmazások, amelyet használ szolgáltatási Bus létrehozásakor szolgáltatás Bus összeállítás mutató hivatkozás hozzáadása, és a megfelelő névteret tartalmaz. A művelet legegyszerűbben a megfelelő [NuGet](https://www.nuget.org) csomag letöltése.

## <a name="get-the-service-bus-nuget-package"></a>A szolgáltatás Bus NuGet csomag beszerzése

A [szolgáltatás Bus NuGet csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) legkönnyebben szolgáltatás Bus API-val és az alkalmazás állítson be minden szükséges szolgáltatás Bus függőség. A szolgáltatás Bus NuGet szoftvercsomag telepítésekor a projekt, tegye a következőket:

1.  Megoldás Explorerben kattintson a jobb gombbal a **hivatkozást**, majd **NuGet csomagok kezelése**gombra.
2.  Keresse meg a "Szolgáltatás Bus", és jelölje ki a **Microsoft Azure Service Bus** elemet. Kattintson a **telepítés** gombot a telepítés befejezéséhez, majd zárja be a következő párbeszédpanelen:

    ![][7]

Most már készen áll a szolgáltatás Bus kódírás.

## <a name="create-a-service-bus-connection-string"></a>Szolgáltatás Bus kapcsolati karakterlánc létrehozása

Szolgáltatás Bus végpontok és a hitelesítő adatok tárolására szolgáló használja a kapcsolati karakterlánc. Konfigurációs fájl helyett merevlemez-kódolás azt is helyezi el a kapcsolati karakterláncot:

- Azure-szolgáltatások használatakor tárolni a kapcsolati karakterláncot az Azure service Rendszerkonfiguráció (.csdef és .cscfg fájlok) használata ajánlott.
- Azure webhelyekre vagy Azure virtuális gépeken futó használata esetén ajánlott tárolni a kapcsolati karakterláncot, a .NET Rendszerkonfiguráció (például a fájlt) segítségével.

Mindkét esetben meghallgathatja a kapcsolati karakterlánc használata a `CloudConfigurationManager.GetSetting` módszer, a jelen cikk látható módon.

### <a name="configure-your-connection-string"></a>A kapcsolati karakterlánc konfigurálása

A szolgáltatás konfigurálása mechanizmusa dinamikusan konfigurációs beállítások módosítása az [Azure portál][] anélkül, hogy az alkalmazás újbóli teszi lehetővé. Például hozzáadása egy `Setting` címke a szolgáltatás-definíciós (**.csdef**) fájlt, a következő példában látható módon.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

A szolgáltatás beállítása (.cscfg) fájl értékeket adja.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Használja a megosztott Access-aláírás (Társítások) kulcs neve és a fő értékeket, amelyeket a portálról, korábban leírt módon.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Azure webhelyekre és Azure virtuális gépeken futó, állítsa be a kapcsolati karakterlánc

Webhelyek vagy a virtuális gépeken futó használatakor a .NET Rendszerkonfiguráció (például Web.config) használata ajánlott. A kapcsolati karakterlánc használatával tárolhat a `<appSettings>` elemet.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Használja a Társítások neve és a fő érték, amely az [Azure portál][]lekért korábban leírt módon.

## <a name="create-a-topic"></a>Témakör létrehozása

A szolgáltatás Bus témakörök és a [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály használatával előfizetések kezelése műveleteket végezheti el. Az osztály bemutat létrehozása, számba veheti a webhely és témakörök törlése.

A következő példa szerkezetek egy `NamespaceManager` használatával az Azure-objektum `CloudConfigurationManager` szolgáltatás Bus névteret és a megfelelő Társítások alapcímének álló kapcsolati karakterlánc osztály engedélyek kezelése, hogy a hitelesítő adatok. A kapcsolati karakterlánc van, az alábbi képernyő:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

A használja az alábbi példa adott az előző szakasz a beállítások.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Vannak olyan túlterhelések [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) módszer, amely lehetővé teszi a témakör; tulajdonságainak beállítása például az alapértelmezett time-to-live (TTL) érték a témakör küldött üzenetekben való alkalmazásra. Ezeket a beállításokat a [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) osztály alkalmazásával. A következő példa bemutatja, hogyan hozhat létre a témakör **TestTopic** nevű maximális hossza 5 GB és a TTL (élettartam) 1 perces alapértelmezett üzenet.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Ellenőrizze, hogy a témakör a megadott nevű névteret belül már létezik használhatja a [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) objektumok a [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) módszert.

## <a name="create-a-subscription"></a>Előfizetés létrehozása

A témakör előfizetések a [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály használatával is létrehozhat. Az előfizetések neve, és beállíthatja, hogy a választható szűrő, amely a kapott az előfizetés virtuális várólista üzenetek készlete korlátozza.

> [AZURE.IMPORTANT] Ahhoz, hogy előfizetésem fogadja üzeneteket mielőtt bármilyen üzeneteket küld a témakör a előfizetés kell létrehoznia. Ha nincs előfizetések témakörben, a témakör azokat az üzeneteket figyelmen kívül hagyja.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Az alapértelmezett (MatchAll) szűrővel előfizetés létrehozása

Nincs szűrő van megadva, amikor új előfizetést jön létre, a **MatchAll** szűrő esetén az alapértelmezett szűrő, amely szolgál. A **MatchAll** szűrő használatakor a témakör közzétett összes üzenet az előfizetés virtuális várólista kerülnek. A következő példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett **MatchAll** szűrő.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések szűrőkkel létrehozása

Is állíthat be, amelyek lehetővé teszik, hogy adja meg, mely a témakör küldött üzenetek kell szűrők belül egy adott témakör előfizetés jelennek meg.

A legtöbb rugalmas típusú szűrőt előfizetések által támogatott eredetű a [SqlFilter][] , amely SQL92 egy részét. Az üzenetek, a témakör közzétett tulajdonságainak SQL szűrők vonatkozni fognak. A kifejezések SQL szűrővel használható kapcsolatos további tudnivalókért olvassa el a [SqlFilter.SqlExpression][] szintaxisát című témakört.

Az alábbi példa létrehoz **HighMessages** nevű csak kiválasztása egy egyéni **LastMessage elemet** tulajdonság 3-nál nagyobb üzeneteket [SqlFilter][] objektummal előfizetést.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Hasonlóképpen az alábbi példa létrehoz nevű **LowMessages** [SqlFilter][] , amely csak a **LastMessage elemet** tulajdonság kisebb vagy egyenlő 3 üzeneteket kiválasztja az előfizetést.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Most már amikor üzenet érkezik `TestTopic`, mindig vevők fizetett elő, az **AllMessages** témakör előfizetést, és a szelektív fizetett elő, a **HighMessages** és **LowMessages** témakör előfizetések (attól függően, hogy az üzenet tartalmának) vevők sikeresen kézbesítve lett kézbesítve.

## <a name="send-messages-to-a-topic"></a>A témakör üzeneteket küldeni

Üzenet küldése szolgáltatás Bus témakörben, az alkalmazás objektumot hoz létre [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) a kapcsolati karakterláncot.

A következő kódot bemutatja, hogyan lehet a **TestTopic** témakörnek a korábbi verziójában készült [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) -objektum létrehozása a [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API-hívást.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Szolgáltatás Bus témakörök küldött üzeneteket a [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) osztály példánya is. Egyéni alkalmazás-specifikus tulajdonságok tartása használt szótár és tetszőleges alkalmazás adatok törzsében [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objektumhoz tartozik-e egy sor olyan általános tulajdonságok (például [címke](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) és [az élettartam](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)). A megfelelő **DataContractSerializer** van használt szerializálni az objektumot, és az alkalmazás a konstruktor [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objektum szerializálható objektumokat megkerülhetők állíthat be az üzenet törzsébe. Másik lehetőségként a **System.IO.Stream** is biztosítható.

A következő példa bemutatja, hogyan lehet próba küldésére öt a kapott az előző példában kód **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objektum. Megjegyzendő, hogy minden üzenet [LastMessage elemet](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) a tulajdonság értékét attól függően, hogy a példányaiban le változó (Ez határozza meg, hogy melyik előfizetések kapta meg).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Szolgáltatás Bus témakörök a [szabványos réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md)használatát támogatja. A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs korlátozott tartani a témakör az üzenetek számát, de van egy vonalvég az üzenetek témakör birtokában összesített méretét. Ez a témakör a méret létrehozáskor, az 5 GB felső határa van megadva. Ha a szétválasztás engedélyezve van, a felső határ magasabb lesz. További tudnivalókért olvassa el a [Partitioned üzenetküldés szervezetek](service-bus-partitioning.md)című témakört.

## <a name="how-to-receive-messages-from-a-subscription"></a>Hogyan üzenetek fogadására előfizetésből

Az ajánlott módszereket előfizetés érkező üzenetek fogadására egy [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objektum környezetbe. [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objektumok kétféle módon dolgozhat: [ *ReceiveAndDelete* és *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Fogadott **ReceiveAndDelete** módban van egyetlen-kép; a művelet Ez azt jelenti, hogy szolgáltatás Bus olvasási vonatkozóan kap megkeresést egy üzenetet az előfizetés, amikor azt az üzenetet az éppen, és adja vissza azt az alkalmazás. **ReceiveAndDelete** mód a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Felhasználásra, amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása a szolgáltatás Bus van megjelölve az üzenetet, mert azt fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

(Ez az alapértelmezett mód) **PeekLock** módban a fogadási folyamat válik egy két lépésből álló műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a másodszintű fogadási folyamat befejezése hívja fel a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) üzenet a beérkezett. Szolgáltatás Bus láthatja a hívás **befejezése** , amikor az üzenetet, az elfogyasztott alatt, és eltávolítja azt az előfizetést.

Az alábbi példa bemutatja, hogyan üzenetek fogadható és feldolgozása az alapértelmezett **PeekLock** üzemmód használata. Adjon meg egy másik [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) értéket, [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx)egy másik túlterhelés is használhatja. Ebben a példában a [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) visszahívás folyamat során, hogy a beérkező be a **HighMessages** előfizetést.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Ebben a példában a [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) visszahívás [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) objektum használatával állítja be. [Az automatikus kiegészítés](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) értéke **hamis** ahhoz, hogy mikor kell a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) hívja meg a beérkezett üzenetekben manuális ellenőrzése. Az ügyfél [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) értéke 1 perc, amely megjeleníti az ügyfél, ha megvárja, akár egy perc alatt az automatikus megújítás funkció megszakítása előtt és az üzenetek keresése új hívást. Ez a tulajdonság értékét csökkenti az ügyfél lehetővé teszi, amely nem üzeneteihez adóköteles hívások száma.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a fogadó alkalmazás nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja a [elhagyja](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) módszer (helyett a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) módszer) kapott üzenet a. Ennek hatására a szolgáltatás Bus zárolásának feloldása az üzenetet az előfizetésben, és tegye elérhetővé az azonos igénybe alkalmazásával vagy egy másik igénybe alkalmazás ismét kapott.

Is van egy üzenetet, az előfizetés belül zárolt társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenet a zárolás időtúllépési lejár (például ha az alkalmazás összeomlik), majd a szolgáltatás Bus automatikusan feloldja az üzenetet, és újra kapott számára elérhetővé teszi azt.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) kérelem kibocsátása előtt, az üzenet fog kell újra az alkalmazást újraindításkor. Gyakran Link *legalább egyszer feldolgozása*; Ez azt jelenti, hogy minden üzenet legalább egyszer feldolgozása, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el az üzenetet küld, amely a kézbesítési kísérletek keresztül változatlan marad [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) tulajdonságával.

## <a name="delete-topics-and-subscriptions"></a>Témák és az előfizetés törlése

A következő példa bemutatja, hogyan lehet a következő témakört: **TestTopic** törlése a **HowToSample** szolgáltatás névtere.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Témakör törlése a regisztrált a témakör az előfizetése is törli. Az előfizetések egymástól függetlenül is törlődnek. A következő kódot bemutatja, hogyan **HighMessages** nevű **TestTopic** témakör az előfizetés törlése.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus témakörök és előfizetések alapjait, kövesse az alábbi hivatkozásokra kattintva további.

-   [Sorok, témák, és előfizetések][].
-   [A témakör szűrők minta][]
-   [SqlFilter][]API hivatkozását.
-   A munkaidő-alkalmazások, amely küldi és fogadja üzeneteket, és onnan egy szolgáltatás Bus várólista összeállítása: [szolgáltatás Bus brokered üzenetben .NET oktatóprogram][].
-   Szolgáltatás Bus minták: [Azure minták][] töltse le vagy a [– Áttekintés](service-bus-samples.md)című témakörben találhat.

  [Azure portál]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
  [A témakör szűrők minta]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Szolgáltatás Bus brokered üzenetben .NET oktatóprogram]: service-bus-brokered-tutorial-dotnet.md
  [Azure minták]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
