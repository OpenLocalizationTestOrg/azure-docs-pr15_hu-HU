<properties 
    pageTitle="A WebJobs SDK Azure Service Bus használata" 
    description="Útmutató súgótémaköröket és Azure Service Bus sorban várakozó használata a WebJobs SDK csomagjában talál." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>A WebJobs SDK Azure Service Bus használata

## <a name="overview"></a>– Áttekintés

Ez az útmutató C# kód példák, amelyek bemutatják, hogyan indíthatja el egy folyamat, amikor egy Azure Service Bus üzenet érkezik. A mintakódok [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzióját használja 1.x.

Az útmutató feltételezi, hogy tudja, [hogyan kell a kapcsolati karakterláncot, mutasson a tárterület-fiók a Visual Studióban WebJob projekt létrehozása](websites-dotnet-webjobs-sdk-get-started.md).

A kódtöredék csak a funkciók nem hoz létre, a kód megjelenítése a `JobHost` objektum következő példának megfelelően:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

A [szolgáltatás Bus kész példa](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) az azure-webjobs-sdk-minták tárházából GitHub.com szerepel.

## <a id="prerequisites"></a>Előfeltételek

Dolgozhat a szolgáltatás Bus kell telepíteni a [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet csomag mellett a többi WebJobs SDK csomagot. 

Meg kell beállítani a AzureWebJobsServiceBus kapcsolati karakterláncot, a tárterület a csatlakozási_karakterlánc mellett.  Ehhez a `connectionStrings` szakasz App.config fájl, az alábbi példában látható módon:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Minta projekt, amely tartalmazza a szolgáltatás Bus csatlakozási karakterlánc beállítást a App.config fájlban olvassa el a [szolgáltatás Bus példa](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus)című témakört. 

A kapcsolati karakterláncot is beállítható, hogy a futtatókörnyezet Azure környezetben, majd felülbírálja App.config az Azure; a WebJob futtatásakor További tudnivalókért lásd: [Első lépések a WebJobs SDK csomagjában talál](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Hogyan indíthatja el a függvény a várólista szolgáltatást Bus üzenet érkezésekor

Ha válaszolni szeretne egy függvényt, amely a WebJobs SDK meghívja várólista üzenet érkezésekor, használja a `ServiceBusTrigger` attribútum. Az attribútum konstruktora paramétert a várólista lekérdezik nevét adja meg.

### <a name="how-servicebustrigger-works"></a>Hogyan működik a ServiceBusTrigger

A SDK csomagjában talál az üzenetet kap `PeekLock` mód és a hívások `Complete` esetén a függvény sikeresen befejeződött üzenet vagy hívások `Abandon` , ha a parancs nem működik. Ha hosszabb fut, a függvény a `PeekLock` időtúllépése, a zárolás megújítása automatikusan be van állítva.

Szolgáltatás Bus végzi a saját poison várólista kezelését, amely nem ellenőrzött és nem állítható be a WebJobs SDK csomagjában talál. 

### <a name="string-queue-message"></a>Karakterlánc várólista üzenet

A következő kódot minta felolvassa várólista üzenet, amely tartalmazza a karakterlánc, majd a karakterlánc ír a WebJobs SDK irányítópult.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Megjegyzés:** Ha az üzenetek egy alkalmazásban a WebJobs SDK nem használó hoz létre, feltétlenül [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) meg a "egyszerű szöveg".

### <a name="poco-queue-message"></a>POCO várólista üzenet

A SDK automatikusan deszerializálni a várólista üzenet, amely tartalmazza a POCO JSON [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) típusát. A következő kódot példa beolvassa várólista üzenet, amely tartalmazza a `BlobInformation` objektumra, amely mellett egy `BlobName` tulajdonság:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

A POCO tulajdonságainak használata BLOB és a táblák ugyanazt a funkciót a ábrázoló mintakódok [Ez a cikk a tárhely sorban várakozó verziója](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs)jelenik meg.

Ha a kód, amely hoz létre a várólista üzenetet nem használja a WebJobs SDK csomagjában talál, használja a következőhöz hasonló:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Fájltípusok ServiceBusTrigger működik-e

Emellett `string` és POCO típusú, használhatja a `ServiceBusTrigger` egy bájt tömb attribútummal vagy egy `BrokeredMessage` objektumot.

## <a id="create"></a>Hogyan hozhat létre szolgáltatás Bus üzenetek

Létrehoz egy új várólista üzenet használata függvény írni a `ServiceBus` attribútum és átadni a attribútum konstruktor várólista nevét. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>A függvény nem aszinkron egyetlen várólista üzenet létrehozása

Az alábbi kód példában kimeneti paramétert a várakozási sorban található ugyanazt a tartalmat az üzenet, a "inputqueue" nevű várakozási sorban található érkezett, a "outputqueue" nevű új üzenet létrehozásához.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

A kimeneti paraméter egy adott sorba üzenet írására szolgáló a következő típusú bármelyike lehet:

* `string`
* `byte[]`
* `BrokeredMessage`
* Szerializálható POCO típusa, amely csak. Automatikusan szerializálásának JSON.

POCO típusa paramétereinek várólista üzenet mindig létrejön a függvény befejeződésekor; Ha a paraméter értéke null értékű, a SDK várólista üzenet létrehozása, amelyek null ad vissza, ha az üzenet kapott, és deszerializálni A más fájltípusok Ha a paraméter értéke null nincs várólista üzenet jön létre.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Több üzenetek létrehozása és az aszinkron függvények

Több üzenetet hozhat létre a `ServiceBus` az attribútum `ICollector<T>` vagy `IAsyncCollector<T>`, ahogy a következő kódot minta:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Azonnal létrejön az egyes várólista üzenet esetén a `Add` módszer neve.

## <a id="topics"></a>Szeretné a szolgáltatás Bus témakörök

Ha válaszolni szeretne egy függvényt, amely a SDK meghívja, amikor üzenet érkezik szolgáltatás Bus témához kapcsolódó, használja a `ServiceBusTrigger` , hogy mi az a témakör és előfizetés nevét, ahogy a következő kódot minta konstruktorának attribútummal:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Hozzon létre egy üzenetet témához kapcsolódó, használja a `ServiceBus` témakör nevű ugyanígy használható várólista nevét a attribútum.

## <a name="features-added-in-release-11"></a>Az 1.1-es kiadás szolgáltatásai

Az alábbi szolgáltatások 1.1-es verzióban lettek hozzáadva:

* Lehetővé teszi a mély testreszabását üzenet keresztül feldolgozási `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`a szolgáltatás Bus testreszabása támogatja `MessagingFactory` és `NamespaceManager`.
* A `MessageProcessor` stratégia mintát lehetővé teszi, hogy egy processzor per várólista/témakör Itt adhatja meg.
* Üzenet feldolgozás feldolgozási alapértelmezés szerint támogatott. 
* Egyszerű testreszabás `OnMessageOptions` keresztül `ServiceBusConfiguration.MessageOptions`.
* Lehetővé teszi a [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) meg kell adni a `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (például a hol lehet, hogy nem esetek jogok kezelése). 

## <a id="queues"></a>A tároló sorban várakozó ennek cikkben foglalt Kapcsolódó témakörök

Nem jellemző szolgáltatás Bus WebJobs SDK esetek tudni megtudhatja, [hogy miként Azure várólista tárhely és a WebJobs SDK használja](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Ebben a cikkben szereplő témakörök a következők:

* Aszinkron függvények
* Több példányban
* Biztonságos leállítása
* WebJobs SDK attribútumok törzsében, a függvény használata
* A SDK a csatlakozási_karakterlánc beállítása a kódot.
* Értékek az WebJobs SDK konstruktor paramétereket állíthat a kódot.
* Kézzel indítja el függvény
* Naplók írása

## <a id="nextsteps"></a>Következő lépések

Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezelheti a esetei Azure Service Bus használata. Azure WebJobs és a WebJobs SDK használatával kapcsolatos további tudnivalókért lásd: [Azure WebJobs ajánlott erőforrásokat](http://go.microsoft.com/fwlink/?linkid=390226).
 
