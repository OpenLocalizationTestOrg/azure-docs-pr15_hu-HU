<properties 
    pageTitle="Szolgáltatás Bus sorban várakozó, témák és előfizetések |} Microsoft Azure"
    description="Személyek üzenetküldési szolgáltatás Bus áttekintése."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Szolgáltatás Bus sorban várakozó, témák és előfizetések

Microsoft Azure Service Bus felhőalapú, az üzenet lépések köztes technológiák, például megbízható üzenetsor és tartós közzététel/előfizetés üzenetküldés támogatja. Ezek brokered üzenetkezelő lehetőségeit tekinthető a leválasztott Csevegés támogatása közzététel előfizetés funkciói, időbeli szétválasztás és terheléselosztási esetek a háló üzenetküldési szolgáltatás Bus használatával. Leválasztott kommunikációs előnyökkel jár sok; például ügyfelek és -kiszolgálók is szükség szerint csatlakozás, és hajtsa végre a műveletek aszinkron módon.

Az üzenetben szervezetek, az alapvető az brokered üzenetben funkciók a szolgáltatás Bus alkotó sorban várakozó témakörök/előfizetések és szabályok/műveletek.

## <a name="queues"></a>Sorok

Sorok tud nyújtani első, első ki (FIFO) üzenetkézbesítés egy vagy több versengő felhasználóknak. Ez azt jelenti, hogy üzeneteket általában várhatóan kapott, és a címzett abban a sorrendben, amelyben a sor hozzáadásuk, és minden üzenet kapott, és csak egy üzenet fogyasztói által feldolgozott által feldolgozott. Egy sorban várakozó használatával előnye "időbeli szétválasztás" elérése a alkalmazásösszetevők. Más szóval a gyártók (feladók) és a fogyasztói (címzett) nem kell kell üzenetek küldése és fogadása egyszerre, mert üzenetek tartósan vannak tárolva a várakozási sorban található. Ezenkívül a gyártó nincsenek léptetése az ügyféltől válasz annak érdekében, hogy továbbra is folyamat, és az üzenetek küldése.

A kapcsolódó előnye "terheléselosztás,": Ezzel a funkcióval gyártók és fogyasztói küldhet és fogadhat üzeneteket különböző kamatlába mellett. Számos alkalmazás a rendszer betöltés változó időbeli; minden egyes munka mértékegység szükséges időt feldolgozás azonban általában állandó. Intermediating üzenet gyártók és a fogyasztói várólista az azt jelenti, hogy igénybe alkalmazása csak a csúcs betöltés helyett átlagos terhelés kezelésére engedélyezni kell építenie. A várakozási sorban található mélységét megnő, és szerződést, a bejövő betöltés változik. Ez közvetlenül kapcsolatban az alkalmazás terhelés szükséges infrastruktúra mennyiségét pénzt menti. Növeli a betöltés, mint további dolgozó folyamatok felvehetők a sorból olvasható. Minden üzenet a dolgozó folyamatok közül csak akkor dolgozza fel. Ezenkívül a leküldéses-alapú terheléselosztási lehetővé teszi az optimális használat dolgozó számítógépek akkor is, ha a dolgozó számítógép feldolgozási teljesítmény, különbözőek, azok fog lekérés üzenetek díjazott saját maximális. A minta gyakran adja meg a "gyorskorcsolyázásban fogyasztói" mintázatot.

A sorok közötti üzenet gyártók és a fogyasztói haladó körét egy belső laza összekapcsolási összetevők között. Mivel gyártók és a fogyasztói nem érdemes szem előtt, minden más, a fogyasztói anélkül, hogy semmilyen hatása a gyártó frissíthető.

A lépéseken várólista létrehozása. Személyek (sorban várakozó és témakörök) üzenetküldés szolgáltatás Bus adatkezelési műveletek az [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály alapcímének a szolgáltatás Bus névtér és a felhasználói hitelesítő adatok megadásával összeállítás keresztül hajtsa végre. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) hozhat létre, számba veheti a webhely és törlése az üzenetek szervezetek bemutat. Egy [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -objektum létrehozása a Társítások nevét és billentyűt, és a szolgáltatási névtér management objektumhoz, után a [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) módszer hozhat létre a várakozási sorban található. Példa:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Ezután létrehozhat várólista-objektumra, és egy üzenetben gyári a szolgáltatás Bus URI az argumentumként. Példa:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Üzenetek majd elküldheti a sorba. Ha például nevű brokered üzenetek van `MessageList`, a kód megjelenik a következőhöz hasonló:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Ezután fogadhat üzenetet a sorból az alábbi képlettel történik:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

A [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) módban a fogadási művelet egyetlen-ikon; Ez azt jelenti, hogy szolgáltatás Bus megkapja a kérelmet, ha azt az üzenetet az éppen, és adja vissza azt az alkalmazás. [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) mód a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Szolgáltatás Bus felhasználás alatt álló, amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása az üzenetet olvasottként jelöli, mert azt fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

[PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) módban a fogadási művelet válik, kétszakaszos, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Szolgáltatás Bus megkapja a kérelmet, ha azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy a többi fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a másodszintű fogadási folyamat befejezése hívja fel a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) üzenet a beérkezett. Szolgáltatás Bus láthatja a hívás [befejezése](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , amikor éppen felhasznált az üzenetet olvasottként jelöli.

Ha az alkalmazás nem lehet feldolgozni az üzenetet valamilyen okból, azt a [elhagyja](https://msdn.microsoft.com/library/azure/hh181837.aspx) módszer (helyett a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)) kapott üzenet a felhívhatja. Ez a szolgáltatás Bus zárolásának feloldása az üzenetet, és tegye elérhetővé a azonos fogyasztói vagy egy másik versengő fogyasztói ismét kapott lehetővé teszi. Másodszor, nincs zárolás társított időtúllépés, és ha az alkalmazás nem sikerül az üzenet feldolgozása előtt a zárolás időtúllépése lejár (például ha az alkalmazás összeomlik), majd a szolgáltatás Bus feloldja az üzenetet, és újra kapott számára elérhetővé teszi azt (lényegében egy [elhagyja](https://msdn.microsoft.com/library/azure/hh181837.aspx) művelet végrehajtása alapértelmezés szerint).

Figyelje meg, hogy abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) kérelem kibocsátása előtt, az üzenet akkor újra az alkalmazás újraindításkor. Ez gyakran nevezik *Legalább egyszer *feldolgozása; Ez azt jelenti, hogy minden üzenet legalább egyszer feldolgozása. Azonban bizonyos esetekben ugyanazt az üzenetet is lehet újra. Ha ismétlődő feldolgozás, az alkalmazási példát nem elviselni, majd a további logika követelmény az alkalmazás feltárása az ismétlődő elemeket, amelyek lehet elérni az üzenet, amely végig a kézbesítési kísérletek változatlan marad a **MessageId** tulajdonság alapján. Ez az úgynevezett *Pontosan egyszer* feldolgozása.

További információt és egy működő példát, hogy miként hozhat létre, és elküldi az üzenetet, és a sorok című témakörben talál a [szolgáltatás Bus brokered .NET oktatóprogram üzenetküldés](service-bus-brokered-tutorial-dotnet.md).

## <a name="topics-and-subscriptions"></a>Témák és előfizetések

Sorok, amelyben minden üzenet dolgozza fel egy egyetlen fogyasztói alkalmazással szemben a *témakörök* és a *előfizetések* kommunikáció a *Közzététel/előfizetés* mintát egy-a-többhöz űrlap biztosít. Hasznos, ha a méretezés a címzettek nagy számok, egyes közzétett üzenetek elérhetővé válik minden előfizetéséhez regisztrált a témakört. Üzenetek küldésének témakörben és egy vagy több társított előfizetések, attól függően, hogy a szűrés szabályok, amelyek előfizetés alapon kézbesítve. Az előfizetések további szűrők használatával korlátozása olyan üzenetre, amelyet azokat meg szeretné kapni. Üzenetek fogadására ugyanúgy témakörben várólista elküldi ezeket, de üzenetet nem érkeznek a témakör közvetlenül. Ehelyett előfizetésekről érkeznek őket. A témakör előfizetés hasonló, akkor egy virtuális várólista kapja a témakör küldött üzenetek másolatát. Üzenetek fogadásának előfizetésből azonos az egy sorból érkező módját.

Útján összehasonlító az üzenet elküldése funkciókat várólista megfelelteti közvetlenül a témakör, és az üzenet-receiving funkciók társít előfizetéshez. Egyebek mellett ez azt jelenti, hogy előfizetések támogatja a korábban sorban várakozó kapcsolatban ebben a szakaszban ismertetett azonos mintázatok: versengő fogyasztói, időbeli szétválasztás, betöltés simítási és terheléselosztás.

A témakör létrehozása hasonlít egy sorban létrehozása az előző szakasz a példában látható módon. A szolgáltatás URI létrehozása, és a [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály segítségével a névtér ügyfél létrehozása. A témakör a [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) módszerrel majd hozhat létre. Példa:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Ezután tetszés szerint adja hozzá az előfizetések:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

A témakör ügyfél majd hozhat létre. Példa:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Az üzenet feladója használ, akkor is üzenetek küldése és fogadása érkező a témakört, és az előző szakaszban látható módon. Példa:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Sorok hasonlóan vannak érkező üzenetek helyett egy [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) objektum egy [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objektum használatával előfizetést. Az előfizetés ügyfél, a témakör a neve, az előfizetést, és (nem kötelező) a fogadás mód átadása paraméterként létrehozása. Ha például a **készlet** előfizetéssel:

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Szabályok és műveletek

Sok esetben az adott jellemzők üzeneteket különböző módokon fel kell dolgozni. Engedélyezéséhez beállíthatja az előfizetések tulajdonságok van szükség, és hajtsa végre az a tulajdonságokat bizonyos módosítások üzenetek kereséséhez. Szolgáltatás Bus előfizetések a témakör küldött összes üzenet jelenik meg, amíg a virtuális előfizetés sorba csak másolhatja azokat az üzeneteket egy részét. A elvégezni, előfizetés szűrők használatával. Ezek a módosítások úgynevezett *szűrési műveletek*. Előfizetés létrehozásakor adhat meg, hogy az üzenet, a rendszer tulajdonságai (például a **címke**) és az alkalmazás egyéni tulajdonságok (már például **storename mezőt**van hátra.) a Tulajdonságok működik szűrőkifejezés Az SQL szűrőkifejezés nem kötelező ebben az esetben; SQL-szűrőkifejezés nélkül minden előfizetés definiált szűrőművelet meg azokat az üzeneteket, hogy az előfizetés fog történni.

Az előző példában **Store1**, érkező üzenetek szűrésére meg kellene hoz létre az irányítópult-előfizetés az alábbi képlettel történik:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Az e előfizetés szűrő helyen csak üzeneteket a `StoreName` tulajdonsága `Store1` virtuális várakozási sorban a program ekkor a `Dashboard` előfizetés.

További információt a lehetséges szűrőértékeket dokumentációjában [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) és [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) osztályok. További tájékoztatásért olvassa el a [Brokered üzenetküldés: speciális szűrők](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) és [Témakör szűrők](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) minták.

## <a name="next-steps"></a>Következő lépések

Lásd: az alábbi témakörök tartalmaznak további információt és brokered szolgáltatás Bus üzenetben szervezetek példák speciális.

- [Szolgáltatás Bus üzenetküldés – áttekintés](service-bus-messaging-overview.md)
- [Szolgáltatás Bus brokered üzenetben .NET oktatóprogram](service-bus-brokered-tutorial-dotnet.md)
- [Szolgáltatás Bus brokered üzenetben többi oktatóprogram](service-bus-brokered-tutorial-rest.md)
- [A témakör szűrők minta](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Brokered üzenetküldés: Speciális szűrők minta](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

