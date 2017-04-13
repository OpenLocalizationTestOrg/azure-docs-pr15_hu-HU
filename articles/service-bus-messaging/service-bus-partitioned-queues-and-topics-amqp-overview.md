<properties 
    pageTitle="Szolgáltatás Bus AMQP 1.0 támogatása a sorok és a témakörök partícionálni |} Microsoft Azure" 
    description="Tudjon meg többet a speciális üzenet Queuing Protocol (AMQP) 1.0 a szolgáltatás Bus particionálva sorban várakozó és témaköröket." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>Szolgáltatás Bus particionálva sorban várakozó és témakörök AMQP 1.0 támogatása 

Azure Service Bus támogatja a speciális üzenet Queuing protokoll (**AMQP**) 1.0-szolgáltatás Bus **particionálva sorban várakozó és témaköröket.**

**AMQP** nem szabványos megnyitott üzenet várólista protokoll, amely lehetővé teszi, hogy másik programnyelv platformok alkalmazások fejlesztése. AMQP kapcsolatos általános tudnivalókat a támogatásáról szolgáltatás Bus című témakörben [AMQP 1.0 Bus szolgáltatás használatát támogatja](service-bus-amqp-overview.md).

**Partitioned sorban várakozó és témaköröket**, más néven *particionált szervezetek*, olyan magasabb rendelkezésre állás, megbízhatóságának és átviteli, mint a hagyományos másik sorban várakozó és témaköröket. Particionált szervezetek kapcsolatos további tudnivalókért olvassa el a [Particionált üzenetküldés szervezetek](service-bus-partitioning.md)című témakört.

Kommunikáció particionált sorban várakozó és témakörök protokolljaként AMQP 1.0 hozzáadásával lehetővé teszi, hogy kihasználhassa megbízhatóság, magasabb rendelkezésre állás és particionálva szolgáltatás Bus személyek által kínált egész együttműködésre képes alkalmazások készítése.

Részletes vezetékes szintű AMQP 1.0 protokoll útmutató, amely bemutatja, hogyan szolgáltatás Bus hajtja végre, és az OASIS AMQP technikai beállítások épül-e, olvassa el a [AMQP 1.0 az Azure Service Bus és esemény protokoll útmutató](service-bus-amqp-protocol-guide.md)című témakört.    

## <a name="use-amqp-with-partitioned-queues"></a>Particionált sorban várakozó AMQP használata

Sorok hasznosak időbeli szétválasztás, betöltés simítási, terheléselosztás és laza összekapcsolási igénylő helyzeteket. Egy sorban közzétevők üzeneteket küldeni a várólista és vonzóbbak lehetnek a várólista esetekben, amikor egy üzenet csak fogadható egyszer érkező üzenetek fogadására. A helyes például a rendszer egy készlet, amelyben pénztári terminálok az adatok közzététele egy várólista helyett közvetlenül a készlet projektirányítási rendszerek. A készlet projektirányítási rendszerek majd fogyaszt az adatok kezelése készlet feltöltési bármikor. Számos előnye, beleértve a készlet projektirányítási rendszerek, ne kelljen mindig online kell azt. Szolgáltatás Bus sorban várakozó kapcsolatos részletekért lásd: a [szolgáltatás Bus sorban várakozó használó létrehozása alkalmazások](service-bus-create-queues.md). 

További particionált várólista növeli az elérhetőség, megbízhatóságának és alkalmazásokhoz, átviteli, a következő sorban várakozó vannak particionálva több üzenetet kereskedők és üzenetben tárolja.     

### <a name="create-partitioned-queues"></a>Particionált sorban várakozó létrehozása

Az [Azure klasszikus portál][] és a szolgáltatás Bus SDK particionált várólista hozhat létre. Particionált várólista létrehozására a [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) tulajdonság **Igaz** az [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) példány. Az alábbi kód használatával a szolgáltatás Bus SDK particionált várólista létrehozása mutatja. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>AMQP használata üzenetek küldése és fogadása

Üzenetek küldése és AMQP használatával, protokolljaként [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) tulajdonság [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) beállításával, ahogy az alábbi kódot particionált várólista érkező üzenetek fogadására.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>AMQP használata particionált témakörök

Témakörök sorban várakozó adatbázistáblákhoz hasonló, de témakörök ugyanezt a hibaüzenetet másolatának átirányítása a több *előfizetések*. A tematikus közzétevők üzeneteket küldeni a témakör és fogyasztói előfizetések érkező üzenetek fogadására. A készlet rendszer pénztári esetben terminálok adatok közzététele a témakörre. A készlet projektirányítási rendszerek majd fogadja előfizetésből az üzeneteket. Ezeken kívül ellenőrző rendszert kaphatnak a ugyanazt az üzenetet másik előfizetésről. Szolgáltatás Bus témakörökhöz, és az előfizetéseket, lásd: [használó szolgáltatás Bus témakörök és előfizetések létrehozása alkalmazásokat](service-bus-create-topics-subscriptions.md). 

Sorok, a particionált témakörök tartalmaznak további növelése az elérhetőség, megbízhatóságának és alkalmazásokhoz, átviteli, az alábbi témakörök és azok előfizetések vannak particionálva több üzenetet kereskedők és üzenetben tárolja. 

### <a name="create-partitioned-topics"></a>Particionált témák létrehozása

Az [Azure klasszikus portál][] és a szolgáltatás Bus SDK particionált témakör hozhat létre. A particionált tematikus létrehozásához a [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) tulajdonság **Igaz** az [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) példány. Az alábbi kód használatával a szolgáltatás Bus SDK particionált témakör létrehozása mutatja.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>AMQP használata üzenetek küldése és fogadása

Üzenetek küldése és AMQP használatával protokollt, ahogy az alábbi kódot [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) tulajdonság beállításával [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), hogy előfizetésem particionált témakör érkező üzenetek fogadására.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Következő lépések

Lásd: további információk a következő particionált üzenetben szervezetek, valamint a AMQP olvashat.

*    [Particionált üzenetben személyek](service-bus-partitioning.md)
*    [Üzenet Queuing (AMQP) protokoll 1.0 verziójú speciális OASIS](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [A szolgáltatás Bus AMQP 1.0 támogatás](service-bus-amqp-overview.md)
*    [Azure Service Bus esemény hubok protokoll útmutató AMQP 1,0](service-bus-amqp-protocol-guide.md)
*    [A Java üzenet szolgáltatás (JMS) API használata szolgáltatás Bus és AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [AMQP 1.0 használatáról a szolgáltatás Bus .NET API-val](service-bus-dotnet-advanced-message-queuing.md)

[Azure klasszikus portál]: http://manage.windowsazure.com
