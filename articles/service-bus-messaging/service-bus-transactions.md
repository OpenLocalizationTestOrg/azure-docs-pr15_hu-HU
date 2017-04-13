<properties 
    pageTitle="Bus szolgáltatás tranzakciókat |} Microsoft Azure" 
    description="Azure Service Bus atomi tranzakciók és küldés keresztül – áttekintés" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>A szolgáltatás Bus tranzakciók feldolgozását – áttekintés

Ez a cikk azt ismerteti, hogy az Azure Service Bus tranzakció lehetőségeit. Nagy beszélgetés ezt a [Szolgáltatás Bus mintával atomi tranzakciók](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). Ez a cikk korlátozódik tranzakció feldolgozás és szolgáltatás Bus, *küldése* szolgáltatásának áttekintése közben a atomi tranzakcióinak minta szélesebb és összetettebb hatókörét.

## <a name="transactions-in-service-bus"></a>Bus szolgáltatás tranzakciókat

[Transaction](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) csoportok két vagy több művelet egy *Adatvégrehajtás hatókörét*be együtt. Által jellegű például a tranzakciók gondoskodnia kell arról, hogy egy adott csoporthoz műveletek tartozó összes művelet sikeres vagy sikertelen lehet közösen. Ezzel a tranzakciók egy egységként, amely *atomicity*gyakran nevezik működésbe lépnek. 

Szolgáltatás Bus tranzakció alapú üzenet ügynök, és az üzenet tárolja az összes belső műveleteket tranzakció alapú integritását biztosítja. Üzenetek áthelyezése a [kézbesítetlen](service-bus-dead-letter-queues.md) és az üzenetek [automatikus továbbítás](service-bus-auto-forwarding.md) közötti személyek, például belül szolgáltatás Bus, üzenetek áthelyezése tranzakció alapú. Ilyen szolgáltatás Bus fogadja el az üzenetet, ha azt már tárolt és amelyen a sorszámot. Ettől kezdve minden üzenet átvitelek szolgáltatás Bus belül összehangolt műveletek szervezetek keresztül, és nem vezet fokú (forrás tud, és nem sikerül a cél) vagy a párhuzamos (forrás meghiúsul, cél sikeres) az üzenet.

Szolgáltatás Bus tranzakción csoportosítása egy üzenetben entitás (várólista, témakör, előfizetés) hatálya alá tartozó műveleteket támogat. Például több üzenetet küldeni egy várólista egy tranzakció hatálya alá, és az üzenetek csak akkor adja meg a várólista napló a tranzakció sikeresen befejeztével.

## <a name="operations-within-a-transaction-scope"></a>Egy tranzakció hatálya alá műveletek 

A hatókör tranzakción belül elvégezhető műveletek közül:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: elküldéséhez SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: befejezése után, CompleteAsync, eseményértesítéshez, AbandonAsync, Deadletter, DeadletterAsync, halassza, DeferAsync, RenewLock, RenewLockAsync 

Fogadása műveletek nem szerepelnek, mert azt feltételezzük, hogy az alkalmazás szerez üzenetek a [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) üzemmód használata belül néhány fogadása hurok, vagy egy [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) a visszahívás, és csak ezt követően megnyílik az üzenet tranzakció hatókörét.

A Törlés (teljes, eseményértesítéshez, kézbesítetlen, halassza) az üzenet majd hatálya alá tartozó, és a függő a tranzakció általános eredményét a fordul elő.

## <a name="transfers-and-send-via"></a>Átvitelek és a "Küldés"

Ahhoz, hogy az adatok a sorból egy processzor, majd egy másik várólista tranzakció alapú átadási, a szolgáltatás Bus *átvitelek*támogatja. Egy átviteli művelet feladó először üzenetet küld egy "átviteli sorba" és az átviteli sorba közvetlenül az üzenetet helyezi át a kívánt célhelyre várólista használatával, amely a továbbítás automatikus képesség támaszkodik azonos robusztus átadás végrehajtása. Az üzenet soha nem elkötelezett az átviteli sorba napló oly módon, hogy azt láthatóvá válik az átviteli sorban vonzóbbak lehetnek.

A power tranzakció alapú ezt a lehetőséget, kitűnik, ha magát átviteli sorban a forrás beviteli üzeneteknél a feladó. Más szóval szolgáltatás Bus átviheti az üzenetet a célhelyre "e" átviteli sorban egy olyan videókereséskor (vagy addig elhalasztani, vagy kézbesítetlen) a figyelmeztetés az összes műveletben egy atomi műveletet. 

### <a name="see-it-in-code"></a>A funkció kódot.

Az ilyen átvitelek beállításához keresztül átviteli sorban a célhelyre függvénynél üzenet feladójának hoz létre. Egy vevő gyűjti össze, hogy ugyanazt a várakozási érkező üzenetek is. Példa:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Egy egyszerű tranzakció használja ezeket az elemeket, a következő példának megfelelően:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Következő lépések

Szolgáltatás Bus sorban várakozó további információt az alábbi cikkekben talál:

- [Az automatikus továbbítási láncolási szolgáltatás Bus személyek](service-bus-auto-forwarding.md)
- [Automatikus továbbítás minta](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Atomi tranzakciók szolgáltatás Bus mintával](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure sorban várakozó és szolgáltatás Bus sorban várakozó és összehasonlítása](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Szolgáltatás Bus sorban várakozó használata](service-bus-dotnet-get-started-with-queues.md)