<properties 
    pageTitle="Sorok és témakörök partícionálni |} Microsoft Azure"
    description="Megtudhatja, hogy miként szolgáltatás Bus sorban várakozó és témakörök partition több üzenetet kereskedők használatával."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Particionált sorban várakozó és témakörök

Azure Service Bus alkalmaz több üzenetet kereskedők üzenetek és a több üzenetben készleteinek üzenetek tárolásához. A hagyományos várólista vagy témát egyetlen üzenetre ügynök kezeli, és egy üzenetben tárolja. Szolgáltatás Bus is lehetővé teszi, hogy sorban várakozó vagy témakörök érdekében több üzenetet kereskedők és üzenetben tárolja. Ez azt jelenti, hogy egy particionált várólista vagy a témakör a teljes kapacitásának már nem korlátozza egy adott üzenet közvetítő vagy üzenetben áruházból teljesítményét. Ezeken kívül átmenetileg nem működik egy üzenetben áruház egyáltalán nem jelenik meg egy particionált várólista vagy a témakör nem érhető el. Particionált sorban várakozó és témakörök az összes speciális szolgáltatás Bus funkciókat, például a tranzakciókat, és a munkamenetek támogatása is tartalmazhat.

Ha többet szeretne tudni a szolgáltatás Bus belső a [szolgáltatás Bus architektúra][] témakörben talál.

## <a name="how-it-works"></a>Működése

Több töredékek minden particionált várólista vagy a témakör tartalmaz. Minden részlet egy másik üzenetben tárolja, és egy másik üzenetet ügynök kezeli. Üzenet elküldésekor particionált várólista vagy témakör szolgáltatás Bus rendel az üzenetet a töredékek közül. A kijelölés véletlenszerűen szolgáltatás Bus vagy adhatja meg a feladó partíciót kulcs használatával történik.

Amikor egy ügyfél szeretne megjelenik egy üzenet a particionált sorból vagy particionált témakör, szolgáltatás Bus lekérdezések előfizetésből az üzenetek összes töredékek, adja vissza az első üzenet, amely a címzett kapott bármilyen az üzenetben tárolja. A másik üzenetet, és adja vissza őket amikor kap további szolgáltatás Bus gyorsítótárát kérések kap. Fogadó ügyfél, akkor nem érdemes szem előtt tartania szétválasztás; az ügyfél elérésű működés particionált várólista vagy témakör (például olvasni, fejezze be, addig elhalasztani deadletter, prefetching) megegyezik egy normál entitás viselkedését.

Nem küld üzenetet, vagy üzenetet kap a, a particionált várólista vagy témát külön költség nélkül.

## <a name="enable-partitioning"></a>Engedélyezze a szétválasztás

Azure Service Bus particionált sorban várakozó és témakörök használja, az Azure SDK 2.2 vagy újabb verzióját használja, vagy adjon meg `api-version=2013-10` a HTTP kéri.

Létrehozhat szolgáltatás Bus sorban várakozó és témakörök 1, 2, 3, 4-es vagy 5 GB méretű (az alapértelmezett érték 1 GB). A szétválasztás engedélyezve, szolgáltatás Bus létrehoz 16 partíciót minden megadott GB. Ilyen hoz létre, amely 5 GB méretű várólista, ha a 16 partíciót várólista maximális mérete válik (5 \* 16) = 80 GB. A maximális méretét a particionált várólista vagy a témakör lépése megjeleníti az [Azure portál][]megjelenik.

Többféleképpen particionált várólista vagy témakör létrehozása. Létrehozásakor a várólista vagy a témakör a levelezőprogramból, engedélyezheti vagy a témakör a **true** [QueueDescription.EnablePartitioning][] vagy [TopicDescription.EnablePartitioning][] tulajdonság rendre beállításával szétválasztás. Ezen tulajdonságokat be kell állítani időben a sorban vagy témakör jön létre. Még nem lehet módosítani a tulajdonságok egy meglévő várólista vagy témakört. Példa:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Azt is megteheti akkor hozhat létre egy particionált várólista vagy a témakör a Visual Studio vagy az [Azure portál][]. Amikor a portálon hoz létre egy új sor vagy egy témakört, beállíthatja a **Szétválasztás engedélyezése** lehetőséget az **Általános beállítások megadása** lap várólista vagy témakör **beállításai** ablakban **Igaz**. Az **Új sor** vagy az **Új téma** párbeszédpanel a Visual Studióban, **Szétválasztás engedélyezése** jelölőnégyzetre.

## <a name="use-of-partition-keys"></a>Partition kulcsok használata

Üzenet áthelyezése particionált várólista vagy témakör sorbaállítva esetén szolgáltatás Bus ellenőrzi, hogy az adott partíciót kulcs. Ha megtalálja, kijelöli a részlet alapján billentyűre. Ha nem talál egy partíciót kulcsot, kijelöli a részlet egy belső algoritmus alapján.

### <a name="using-a-partition-key"></a>Partition kulccsal

Bizonyos esetekben munkamenetek vagy tranzakciók, például egy adott fragment tárolni üzenetek szükség. Az összes forgatókönyvekben aposztrófot egy partíciót billentyűt. Az azonos fragment összes üzenetre, amelyet a azonos partíciót billentyűvel van rendelve. A részlet átmenetileg nem érhető el, ha a szolgáltatás Bus hibát ad vissza.

Attól függően, hogy az alkalmazási példát más üzenet-tulajdonságok partíciót kulcsként használt:

**Munkamenet**: Ha egy üzenet, melynek [BrokeredMessage.SessionId][] tulajdonsága, majd a szolgáltatás Bus használja ezt a tulajdonságot a partíciót billentyűt. Ezzel a módszerrel ugyanahhoz a munkamenethez tartozó összes üzenet ugyanazon üzenet közvetítő kezeli. Ez a szolgáltatás Bus rendezést és a munkamenet-állapotok következetességét üzenet biztosításához lehetővé teszi.

**PartitionKey**: Ha egy üzenetnek a [BrokeredMessage.PartitionKey][] tulajdonság, de nem a [BrokeredMessage.SessionId][] tulajdonság beállítása, majd a szolgáltatás Bus a [PartitionKey][] tulajdonságot használja, mint a partíciót billentyűt. Ha az üzenet a [munkamenet-azonosító][] és a [PartitionKey][] tulajdonságainak beállítása is tartalmaz, mind a Tulajdonságok azonosnak kell lennie. A [PartitionKey][] tulajdonság értéke, mint a [munkamenet][] tulajdonság értékét, a szolgáltatás Bus **InvalidOperationException** kivételt adja eredményül. Ha egy feladót a munkamenet-tartsa szem előtt tranzakció alapú üzeneteket küld a [PartitionKey][] tulajdonság kell használni. A partíciók kulcs biztosítja, hogy egy üzenetben közvetítő tranzakción belül küldött összes üzenet kezeli.

**MessageId**: Ha a várólista vagy a témakör van a [QueueDescription.RequiresDuplicateDetection][] tulajdonság értéke **Igaz** és a [BrokeredMessage.SessionId][] vagy [BrokeredMessage.PartitionKey][] tulajdonság nincs beállítva, akkor a [BrokeredMessage.MessageId][] tulajdonság szolgál a partíciót billentyűt. (Tájékoztatjuk, hogy a Microsoft .NET- és AMQP tárak automatikusan hozzárendel egy Üzenetazonosító, ha a küldő alkalmazás nem.) Ugyanezt a hibaüzenetet összes másolatát ebben az esetben, ugyanazon üzenet közvetítő kezeli. Ez a szolgáltatás Bus észleli és ismétlődő üzenetek kiküszöbölheti lehetővé teszi. Ha a [QueueDescription.RequiresDuplicateDetection][] tulajdonság nincs beállítva, **Igaz**, szolgáltatás Bus nem veszi figyelembe a [MessageId][] tulajdonság partíciót kulcsként.

### <a name="not-using-a-partition-key"></a>Nem partíciót kulccsal

Partition kulcs hiányában szolgáltatás Bus elosztja particionált várólista vagy a témakör az összes töredékek ciklikus módon üzeneteket. A választott részlet nem érhető el, ha a szolgáltatás Bus az üzenet rendel egy másik kódrészletet. Ezzel a módszerrel a Küldés művelet egy üzenetben áruház ideiglenes elérhetetlenné ellentétben sikeres lesz.

Adjon szolgáltatás Bus elég ideje várólistára helyezés egy másik kódrészletet be az üzenetet küldi el az üzenetet, az ügyfél által megadott [MessagingFactorySettings.OperationTimeout][] érték nagyobbnak kell lennie 15 másodperc. Javasoljuk, hogy a tulajdonság akkor [így időtúllépés történt][] 60 másodperc alapértelmezett értékének.

Figyelje meg, hogy egy partíciót kulcs "rögzíti" egy adott fragment üzenetet. Az üzenetben áruházból, hogy rendelkezik-e fragment nem érhető el, ha a szolgáltatás Bus hibát ad vissza. Partition kulcs hiányában szolgáltatás Bus választhat egy másik fragment és sikeres művelet. Ajánlott tehát, hogy nem ad meg egy partíciót kulcs kivéve, ha szükséges.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Speciális témakörök: használja a tranzakciók particionált személyekkel

A tranzakciók részeként küldött üzenetek meg kell adnia egy partíciót billentyűt. Ez a következő tulajdonságok egyike lehet: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]vagy [BrokeredMessage.MessageId][]. Ugyanazon a tranzakción részeként küldött összes üzenet ugyanazt a partíciót kulcsot kell megadnia. Ha megpróbálja tranzakción belül partíciót kulcs nélkül üzenet küldése, a szolgáltatás Bus **InvalidOperationException** kivételt adja eredményül. Ha megpróbálja, amely a másik partíciót billentyűparancsok ugyanazon tranzakción belül több üzenetet küld, a szolgáltatás Bus **InvalidOperationException** kivételt adja eredményül. Példa:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

A bármely tulajdonsága, amelyek az partíciót kulcs szolgálják vannak beállítva, ha a szolgáltatás Bus rögzíti egy adott kódrészletet az üzenetet. Ez akkor fordul elő, attól függetlenül, hogy a tranzakciók használják. Partition kulcs Ha még nem szükséges kihagyása ajánlott.

## <a name="using-sessions-with-partitioned-entities"></a>Munkamenetek használatával particionált személyekkel

A témakör munkamenet-et vagy várólista tranzakció alapú üzenet elküldéséhez az üzenet [BrokeredMessage.SessionId][] tulajdonsága kell rendelkeznie. Ha a [BrokeredMessage.PartitionKey][] tulajdonságot, valamint van megadva, meg kell egyeznie a [munkamenet][] tulajdonságot. Ha különböznek, a szolgáltatás Bus **InvalidOperationException** kivételt adja eredményül.

Normál (másik) sorban várakozó és témakörök eltérően még nem lehet egyetlen tranzakció használatával több üzenetet küldeni különböző-munkamenetek. Ha a megpróbálkozik vele a rendszer, a szolgáltatás Bus **InvalidOperationException **kivételt adja eredményül. Példa:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Az üzenetek automatikus átirányítás particionált személyekkel

Azure Service Bus támogatja az üzenetek automatikus továbbítás a, a vagy particionált entitás között. Ahhoz, hogy az üzenetek automatikus továbbítása, a [QueueDescription.ForwardTo][] tulajdonsága az adatforrás várólista vagy előfizetést. Ha az üzenet egy partíciót kulcs ([munkamenet][], [PartitionKey][]vagy [MessageId][]) adja meg, a cél entitás partíciót billentyűre használatos.

## <a name="considerations-and-guidelines"></a>Megfontolandó szempontok és útmutató

- **Magas konzisztencia szolgáltatások**: Ha entitás használja a szolgáltatások, például a munkamenetek, ismétlődő észlelési vagy explicit irányításának szétválasztás billentyűt, majd az üzenetben műveletek mindig vannak-e irányítva adott töredékek. Ha bármelyik a töredékek észlel nagy forgalmat vagy az alapul szolgáló áruházból megsérült, ezek a műveletek nem fognak működni, és a elérhetősége csökken. Általános következetességét még sokkal nagyobb, mint a másik szervezetek; adatforgalom csak egy részhalmazát tapasztal problémákat, nem pedig a minden forgalmat.
- **Adatkezelési**: műveletek, például létrehozása, frissítése és törlése a egység összes töredékek kell elvégezni. Ha minden részlet egy sérült vezethet a hibák ezeket a műveleteket. A Get-művelet információkat, például üzenet megszámolja összesíteni kell az összes töredékek. Ha minden részlet sérült, a szervezet elérhetőségi állapot korlátozva van készként.
- **Alacsony üzenet alkalmazási helyzetek**: Ha ilyen esetben különösen akkor, ha a HTTP protokollon keresztül, végezze el a több is fogadási műveletek annak érdekében, hogy azokat az üzeneteket. Receive kérelmeket az előtér fogadáskor végez az összes töredékek és gyorsítótárát kapott válaszokat. Ugyanazt a kapcsolatot a későbbi fogadási kérelmet, akinek a gyorsítótár összekapcsolhatók és fogadása késések kisebb lesz. Jó helyen jár, ha több kapcsolatot, vagy használja a HTTP, amely új kapcsolatot létesít minden kérelme. Ilyen nem nem biztos, hogy meg kellene land ugyanazon a csomóponton. Ha minden meglévő üzenet zárolva van, és egy másik előtér gyorsítótárban tárolt, a fogadási művelet **null**adja eredményül. Üzenetek ahányat lejár, és újra fogadhat őket. HTTP életben ajánlott.
- **Tallózás/betekintő üzenetek**: PeekBatch mindig nem ad vissza az [MessageCount tulajdonság](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)megadott üzenetek számát. Nincsenek két gyakori oka lehet ennek. Több oka az, hogy üzenetek csoportjára összesített méretét 256KB maximális mérete meghaladja-e. Egy másik oka az, hogy várólista vagy témakör van [EnablePartitioning tulajdonság](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) értéke **Igaz**, ha egy partíciót valószínűleg nincs elég üzenetek befejezéséhez kért az üzenetek számát. Általánosságban elmondható az alkalmazás nem felel meg megadott számú üzeneteket fogadni, ha meg kell hívnia [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) többször addig, amíg kap, hogy az üzenetek számát, vagy nincsenek való további üzenetek. További információért mintakódok, beleértve a [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) és [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx)című cikkben talál.

## <a name="latest-added-features"></a>Legújabb hozzáadott funkciók

- Hozzáadása vagy eltávolítása particionált személyekkel most támogatja a szabályt. Különböző másik személyektől származó, ezek a műveletek nem támogatottak a tranzakciók. 
- Üzenetek küldése és fogadása a és a particionált entitás most támogatott AMQP.
- A következő műveletek most támogatja a AMQP: [Köteg küldése](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Köteg fogadása](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [fogadási sorszám által](https://msdn.microsoft.com/library/azure/hh330765.aspx), [Bepillantás](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Megújítása zárolása](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Ütemezés üzenet](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Ütemezett üzenet visszavonása](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Szabály hozzáadása](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Eltávolítása szabály](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Munkamenet megújítása zárolása](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Munkamenet-állapot beállítása](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Get-munkamenet-állapot](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Munkamenet üzenetek Bepillantás](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)és [Munkamenetek számba veheti a webhely](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Particionált szervezetek korlátai

Szolgáltatás Bus jelenleg az alábbi korlátozások vonatkoznak particionált sorban várakozó és témakörök ír elő:

-   Particionált sorban várakozó és témakörök nem támogatja az üzenetek küldése másik munkamenetek egyetlen tranzakció tartoznak.
-   Szolgáltatás Bus jelenleg lehetővé teszi, hogy legfeljebb 100 particionált sorban várakozó vagy egy névtér témaköröket. Minden particionált várólista vagy a témakör a függvény összeszámolja 10 000 szervezetek névtér (nem vonatkozik prémium réteg) használati kvóta felé.

## <a name="next-steps"></a>Következő lépések

Tanulmányozza a [szolgáltatás Bus AMQP 1.0 támogatása particionálva sorban várakozó és témakörök][] további információk a szétválasztás üzenetben szervezetek. 

  [Szolgáltatás Bus architektúra]: service-bus-architecture.md
  [Azure portál]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [Munkamenet-azonosító]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [Így időtúllépés történt]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [Szolgáltatás Bus particionálva sorban várakozó és témakörök AMQP 1.0 támogatása]: service-bus-partitioned-queues-and-topics-amqp-overview.md
