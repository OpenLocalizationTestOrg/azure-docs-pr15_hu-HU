<properties 
    pageTitle="Az Azure esemény hubok API-k áttekintése |} Microsoft Azure"
    description="A fő esemény hubok .NET ügyfélprogram API-khoz része összefoglalása."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Esemény hubok API – áttekintés

Ez a cikk összefoglalja, hogy az esemény hubok .NET ügyfélprogram API-k billentyűt. Két kategóriába sorolhatók: kezelési és futási idejű API-khoz. Futási idejű API-khoz küldéséhez és fogadásához üzenet szükséges összes művelet állnak. Adatkezelési műveletek lehetővé teszi az esemény hubok entitás állam létrehozása, frissítése és törlése személyek kezelése.

Megfigyeléssel esetek kezelése és a futási idejű időtartamát. A .NET API-k részletes hivatkozás dokumentációja a [Szolgáltatás Bus .NET](https://msdn.microsoft.com/library/azure/mt419900.aspx) és [EventProcessorHost API](https://msdn.microsoft.com/library/azure/mt445521.aspx) hivatkozásokat talál.

## <a name="management-apis"></a>Adatkezelési API-hoz

A következő adatkezelési műveletek elvégzéséhez, kattintson az esemény hubok névtér **kezelése** engedélyekkel kell rendelkeznie:

### <a name="create"></a>Hozzon létre

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Frissítés

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Törlése

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>Futási idejű API-hoz

### <a name="create-publisher"></a>A publisher létrehozása

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Üzenet közzététele

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Fogyasztói létrehozása

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Üzenet felhasználása

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Esemény processzor host API-hoz

Ezeket az API-khoz erősítése dolgozó folyamatok, amely szerint shards terjesztése végig az elérhető dolgozók válhat nem érhető el, adja meg.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

A [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) felületén a következők:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Következő lépések

Többet szeretne tudni az esemény hubok forgatókönyvek, látogasson el ezeket a hivatkozásokat:

- [Mi az Azure esemény hubok?](event-hubs-what-is-event-hubs.md)
- [Esemény hubok – áttekintés](event-hubs-overview.md)
- [Esemény hubok programozási útmutató](event-hubs-programming-guide.md)
- [Esemény hubok mintakódok](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

A .NET API-hivatkozások itt áll:

- [Szolgáltatás Bus és esemény hubok .NET API hivatkozás](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Esemény processzor host API-hivatkozás](https://msdn.microsoft.com/library/azure/mt445521.aspx)
