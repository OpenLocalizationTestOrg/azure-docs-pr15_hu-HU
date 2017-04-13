<properties 
    pageTitle="Ajánlott eljárások a teljesítmény használ szolgáltatási Bus |} Microsoft Azure"
    description="Azure Service Bus használata a teljesítmény optimalizálása brokered üzenetek adatcsere ismerteti."
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
    ms.date="10/25/2016"
    ms.author="sethm" />

# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Gyakorlati tanácsok a szolgáltatás Bus üzenetküldés teljesítménybeli fejlesztések

Ez a témakör ismerteti az Azure Bus üzenetküldés használata a teljesítmény optimalizálása adatcsere brokered üzeneteket. Ez a témakör első része, amely a teljesítmény növelése érdekében a program felajánlja a különböző mechanizmusok ismerteti. A második rész a szolgáltatás Bus használatát oly módon, hogy a legjobb teljesítmény elérése érdekében adott helyzetben is kínálhatnak nyújt útmutatást.

Ez a témakör egész "ügyfél" kifejezés szolgáltatás Bus hozzáférő bármely személy. Egy ügyfél is igénybe vehet a szerepkör a feladó vagy egy címzett. A "feladó" kifejezést, amely üzeneteket küld a szolgáltatás Bus várólista vagy témát szolgáltatás Bus várólista vagy témakör ügyfél szolgál. A "címzett" kifejezés, amely egy várólista szolgáltatást Bus vagy -előfizetésre kap üzeneteket szolgáltatás Bus várólista vagy -előfizetésre ügyfél.

Ezek a szakaszok mutassa be több fogalmak Bus szolgáltatást használó erősítése teljesítménye érdekében.

## <a name="protocols"></a>Protokollok

Bus szolgáltatás lehetővé teszi, hogy az ügyfelek számára a via három protokollok üzenetek küldése és fogadása

1. Speciális üzenetsor Protocol (AMQP)

2. Protocol (SBMP) üzenetküldési szolgáltatás Bus

3. HTTP

AMQP és a SBMP hatékonyabbak, mert azok a szolgáltatás Bus kapcsolatot tart, mindaddig, amíg az üzenetben gyári létezik. Akkor is végrehajtja, kötegelés és prefetching. Kivéve, ha kifejezetten említett, a minden tartalom a jelen témakör feltételezi AMQP vagy SBMP használata.

## <a name="reusing-factories-and-clients"></a>Újbóli felhasználása diagramsablonok gyárak és ügyfelek

Szolgáltatás Bus ügyfél objektumok [QueueClient][] vagy [MessageSender][], például egy [MessagingFactory][] objektumra, amely is biztosít a belső kapcsolatok kezelése keresztül jönnek létre. Nem zárjon be üzenetben gyárak vagy várólista, témakör és előfizetés-ügyfelek után az üzenetet küldeni, és majd hozza létre őket a következő üzenet elküldésekor. Egy üzenetben gyári bezárása törli a kapcsolatot a szolgáltatás Bus szolgáltatás, és újbóli létrehozása a gyár létrejön az új kapcsolatot. Egy drága műveletet, amely a ugyanazt a gyár és műveletekhez több ügyfél objektumok ismételt segítségével elkerülheti a kapcsolat létrehozása. Nyugodtan használhatja a [QueueClient][] objektum egyidejű aszinkron műveletek és több szálak üzenetek küldéséhez. 

## <a name="concurrent-operations"></a>Egyidejű műveletek

A művelet végrehajtása (küldése, fogadása, törlés, stb) egy kis időt vesz igénybe. Ez esetben a művelet feldolgozása szolgáltatás Bus szolgáltatás mellett a késés a kérelem és a válasz is tartalmaz. Ha növelni szeretné az idő / műveletek számát, műveletek végre kell hajtania egyidejűleg. Számos különböző módon teheti meg:

-   **Aszinkron műveletek**: az ügyfél ütemez műveletek aszinkron műveletek végrehajtásával. A következő kérés elindul az előző kérelem fejeződött. Az alábbi képen az aszinkron küldés során:

    ```
    BrokeredMessage m1 = new BrokeredMessage(body);
    BrokeredMessage m2 = new BrokeredMessage(body);
    
    Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #1");
      });
    Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #2");
      });
    Task.WaitAll(send1, send2);
    Console.WriteLine("All messages sent");
    ```

    Ez a példa egy aszinkron fogadási műveletet:
    
    ```
    Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    
    Task.WaitAll(receive1, receive2);
    Console.WriteLine("All messages received");
    
    async void ProcessReceivedMessage(Task<BrokeredMessage> t)
    {
      BrokeredMessage m = t.Result;
      Console.WriteLine("{0} received", m.Label);
      await m.CompleteAsync();
      Console.WriteLine("{0} complete", m.Label);
    }
    ```

-   **Több gyárak**: ugyanazt a gyár által létrehozott összes ügyfelek (a címzett kívül feladók) megosztása egy TCP-kapcsolatot. Az üzenetek maximális átviteli szabják meg, hogy a TCP-kapcsolaton keresztül is műveletek száma. TCP oda-vissza időpontok és az üzenet mérete, amely az egyetlen gyár szerezhető be a átviteli jelentősen változhat. Nagyobb teljesítmény sebesség beszerzéséhez több üzenetben gyárak kell használnia.

## <a name="receive-mode"></a>Üzemmódban fogadhat

A várakozási vagy -előfizetésre ügyfél létrehozásakor adhatja meg a fogadás mód: *betekintő-lock* vagy a *vezérlőn és törlése*. Az alapértelmezett fogadása módja [PeekLock][]. Ebben az üzemmódban működő, az ügyfél szolgáltatás Bus kap a kérést küld. Az ügyfél megkapta az üzenetet, miután kérelmének fejezze be az üzenetet küldi.

A fogadás mód [ReceiveAndDelete][]való beállításakor a két lépés egyetlen kérelem egyesíti. Csökkenti a tevékenységek teljes számát, és javíthatja az általános üzenet kapacitása. A teljesítmény nyereség kockázatára üzenetek elvesztése megtalálható.

Szolgáltatás Bus nem támogatja a tranzakciókat fogadása-és-törlési műveletekhez. Ezeken kívül betekintő-zárolása szemantikáját szükségesek, amelyben az ügyfélnek nem felel meg elhalasztásának esetek vagy [kézbesítetlen](service-bus-dead-letter-queues.md) üzenet.

## <a name="client-side-batching"></a>Ügyféloldali kötegelés

Ügyféloldali kötegelés lehetővé teszi az várólista vagy témakör ügyfél késleltetése üzenet küldése egy adott időszakra. Az ügyfél további üzeneteket küld a következő időszakban jelölőnégyzetet, ha az üzenetek egyetlen kötegben továbbítja. Több **kész** kérések köteg az egyetlen összehívásba várólista/előfizetés ügyfél is ügyféloldali kötegelés okoz. Kötegelés csak akkor aszinkron **küldése** és a **teljes** műveletekhez. A szolgáltatás Bus szolgáltatás azonnal elküldi szinkron műveletek. Kötegelés nem fordulhat elő, a betekintő vagy fogadási műveletek, sem bekövetkezik kötegelés ügyfelek keresztül.

Ha a köteget az üzenetek maximális mérete meghaladja, az utolsó üzenetet eltávolítja a köteget, és az ügyfél azonnal elküldi a köteget. Az utolsó üzenetet az első üzenet a következő köteg lesz. Alapértelmezés szerint az ügyfél egy köteg időköz 20 MS használja. A köteg intervallum módosíthatja az üzenetek gyári létrehozása előtt [BatchFlushInterval][] tulajdonság beállításával. Ez a beállítás a gyári által létrehozott összes ügyfelek hatással van.

Kötegelés letiltásához a [BatchFlushInterval][] tulajdonság szeretne **beállítani**. Példa:

```
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Kötegelés nem befolyásolja az számlázható üzenetben műveletek számát, és csak a szolgáltatási Bus ügyfél protokoll érhető el. A HTTP protokoll kötegelés nem támogatja.

## <a name="batching-store-access"></a>Tár access kötegelés

Ha növelni szeretné a kapacitásának várólista/témakör/előfizetés, szolgáltatás Bus több üzenetet tételeket, ha bejegyzi a belső áruházból. Ha engedélyezve van a várakozási vagy témakör, a üzenetek írása a tárba kötegelt lesz. Ha egy várólista vagy -előfizetésre engedélyezve, a üzenetek törlése a áruházból kötegelt lesz. Ha kötegelt áruházból access entitás engedélyezve van, a szolgáltatás Bus késlelteti áruházból írási művelet kapcsolatban, hogy a szervezet által legfeljebb 20 ms. A köteg tároló további műveletek az intervallum során fellépő ad hozzá. Kötegelt áruházból access csak hatással van a **küldése** és a **teljes** műveletek; fogadása műveletek nem érinti. Kötegelt áruház teljes egészében entitás tulajdonságait. Kötegelés történik át, amely kötegelt áruházból hozzáférés engedélyezése minden szervezetek.

Új sor, illetve vagy előfizetést létrehozásakor kötegelt áruházból az access alapértelmezés szerint engedélyezve van. Kötegelt áruházból access letiltásához állítsa a [EnableBatchedOperations][] tulajdonságot **false értékűre** a szervezet létrehozása előtt. Példa:

```
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Kötegelt tároló access nem befolyásolja az számlázható üzenetben műveletek számát, és várólista, témakört, vagy a előfizetés tulajdonsága. Érdemes a fogadás mód és a protokoll ügyfél és a szolgáltatás Bus szolgáltatás közötti használt függetlenül.

## <a name="prefetching"></a>Prefetching

Prefetching lehetővé teszi, hogy a várólista vagy -előfizetésre ügyfél való betöltéséhez további üzeneteket a szolgáltatás fogadási művelet végrehajtásakor. Az ügyfél helyi gyorsítótár az alábbi üzenetek tárol. A gyorsítótár méretét a [QueueClient.PrefetchCount][] vagy [SubscriptionClient.PrefetchCount][] tulajdonságait határozza meg. Minden ügyfél, amely lehetővé teszi, hogy prefetching saját gyorsítótár kezeli. A gyorsítótár nincs megosztva ügyfelek keresztül. Ha az ügyfél kezdeményez egy fogadási műveletet, és a gyorsítótár argumentum üres, akkor a szolgáltatás az üzenetek egy köteg továbbítja. A köteg méretének eredménye a gyorsítótár és 256KB méretét, attól függően, kisebb. Ha az ügyfél kezdeményez egy fogadási műveletet, és a gyorsítótár tartalmaz egy üzenetet, az üzenet kérdezi le a gyorsítótár.

Ha egy üzenet prefetched van, a szolgáltatás zárolja a prefetched üzenet. Ha így tesz, egy másik címzettnek az prefetched üzenetet nem fogadni. A címzett nem sikerül elvégezni az üzenetet, a zárolás lejárta előtt, ha az üzenet többi címzett számára elérhető lesz. Az üzenet prefetched példányát a gyorsítótárban marad. A címzett, a lejárt gyorsítótárban lévő példány fogyasztása kivételt kap, amikor megkísérli fejezze be ezt az üzenetet. Alapértelmezés szerint az üzenetet zárolása lejár, 60 másodperc után. Ez az érték kiterjeszthető 5 percig tart. A lejárt üzenetek felhasználása elkerülése érdekében a gyorsítótár méretét kell mindig kisebb, mint az ügyfél által a zárolás időtúllépés belül felhasználható üzenetek számát.

Az alapértelmezett zárolása lejárati 60 másodperc használatakor, egy jó [SubscriptionClient.PrefetchCount][] értéke 20 alkalommal maximális feldolgozás mértékének gyár minden címzett. Például gyár 3 vevők hoz létre, és minden címzett dolgozhat másodpercenként legfeljebb 10 üzeneteket. Az előzetes lehívása száma nem haladhatja meg a 20\*3\*10 = 600. Alapértelmezés szerint [QueueClient.PrefetchCount][] értéke 0, ami azt jelenti, hogy, hogy a szolgáltatás nem további üzenetek megszámolása.

Üzenetek prefetching növeli a teljes kapacitásának várólista vagy előfizetést, mivel a csökkentik az üzenet műveletek vagy ciklikus utakat teljes számát. Az első üzenet beolvasása, azonban több időt vesz igénybe (miatt a nagyobb üzenetméret). Prefetched üzenetek fogadására gyorsabb lesz, mert ezeket az üzeneteket az ügyfél által már letöltött.

A time-to-live (TTL) tulajdonság, egy üzenet szerint be van jelölve a kiszolgáló a kiszolgáló az üzenetet küld az ügyfél időben. Az ügyfél nem ellenőrzi az üzenet TTL (élettartam) tulajdonságot, amikor az üzenet érkezik. Az üzenet ehelyett fogadható, még akkor is, ha az üzenet TTL (élettartam) eltelte, miközben az üzenetet az ügyfél által lett gyorsítótárazott.

Prefetching nem befolyásolja az számlázható üzenetben műveletek számát, és csak a szolgáltatási Bus ügyfél protokoll érhető el. A HTTP protokoll prefetching nem támogatja. Prefetching érhető el a szinkronizált és a aszinkron fogadási műveletek.

## <a name="express-queues-and-topics"></a>Sorok és témakörök Express

Express szervezetek magas átviteli engedélyezése, és a késés esetek csökkentett. Express személyekkel üzenet elküldése várólista vagy témakört, ha az üzenet nem azonnal tárolja az üzenetek tárolása. Ehelyett, akkor a gyorsítótárban tárolt. Ha több, mint egy pár másodpercre a sorban üzenet marad, automatikusan írt tárolására, így védelem elvesztése miatt egy üzemszünetek állandó. Az üzenet írása egy gyorsítótárban való növeli a átviteli, és csökkenti a késés, mivel az nem lehet hozzáférni az üzenet elküldése időben állandó a tárhely. Néhány másodpercet belül felhasznált üzenetek nem kerülnek a üzenetben áruházból. Az alábbi példa létrehoz egy express témakört.

```
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Express entitás nem lehet nem fontos információkat tartalmazó üzenetet küld, ha a feladó kényszerítheti szolgáltatás Bus közvetlenül a az üzenet állandó tárolóra **true** [ForcePersistence][] tulajdonságának beállításával is.

## <a name="use-of-partitioned-queues-or-topics"></a>Particionált sorban várakozó vagy témakörök használata

Belső szolgáltatás Bus használja a ugyanarra a csomópontra, és a folyamat, és egy üzenetben entitás (várólista vagy témakör) minden üzenetek tárolásához üzenetek tárolása. A particionált várólista vagy a témát, azonban, van meghatározva több csomópontok és üzenetben tárolja. Particionált sorban várakozó és témakörök nem csak egy magasabb átviteli, mint normál sorban várakozó és témakörök hozam, is mutathat kiváló elérhetőségét. Egy particionált entitás létrehozásához a tulajdonság [EnablePartitioning][] **Igaz**, az alábbi példában látható módon. Particionált szervezetek kapcsolatos további tudnivalókért olvassa el a [üzenetben szervezetek particionálva][]című témakört.

```
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Több sorban várakozó felhasználása

Ha még nem lehetséges particionált várólista vagy témakör szeretne használni, vagy a várt betöltés nem kezelhetők a egyetlen particionált várólista vagy a témát, több üzenetben entitás kell használnia. Több entitás használatakor, hozzon létre egy dedikált ügyfélprogram minden személy ugyanazon ügyfél használata az összes entitás helyett.

## <a name="development--testing-features"></a>Fejlesztés és tesztelés funkciók

Szolgáltatás Bus rendelkezik, amely több funkció használható kifejezetten fejlesztési mely **munkakörnyezeti a konfigurációk soha nem használhatók**.

[TopicDescription.EnableFilteringMessagesBeforePublishing](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing.aspx)
- Új szabályok vagy szűrők felvétele után a témakörre EnableFilteringMessagesBeforePublishing ellenőrizze, hogy a várt módon működnek-e az új szűrőkifejezés használható.

## <a name="scenarios"></a>Felhasználási területei

Az alábbi szakaszok ismertetik tipikus üzenetküldési forgatókönyvek, és tagolása a használni kívánt szolgáltatás Bus beállítások. Sebessége sorolják be legkisebb (kisebb, mint 1 üzenet/második), mérsékelt (1 üzenet/második vagy nagyobb, de 100-nál kevesebb üzenetek/második), és magas (100 üzenetek/második, vagy nagyobb). Az ügyfelek száma minősülnek kis (5 vagy kevesebb), mérsékelt (legfeljebb 5, de legfeljebb 20), és a nagy (több mint 20).

### <a name="high-throughput-queue"></a>Nagy átviteli várólista

Cél: A egyetlen várólista kapacitásának teljes méretre állítása A küldő és egy kis.

-   Nagyobb teljesítmény és a elérhetősége particionált várólista használható.

-   Az általános küldési sebességének növeléséhez várakozási segítségével több üzenetet gyárak feladók. Mindegyik feladónak használja a műveletek aszinkron vagy több beszélgetésekben.

-   Ha növelni szeretné a sorból az általános fogadási ráta, használja a több üzenetet gyárak vevők létrehozásához.

-   Aszinkron műveletek segítségével ügyféloldali kötegelés előnyeit.

-   Állítsa a eljárásnak intervallum 50ms szolgáltatás Bus ügyfél protokoll átvitelek számának csökkentése érdekében. Ha több feladók használnak, 100 MS eljárásnak intervallum növelése

-   Kilépés a kötegelt áruházból access engedélyezve van. Az általános ráta, amelyen az üzenetek várakozási írhatók esetében növelése.

-   Állítsa az előzetes lehívása száma 20 alkalommal maximális feldolgozás mértékének gyár minden címzett. Ez a szolgáltatás Bus ügyfél protokoll átvitelek számát csökkenti.

### <a name="multiple-high-throughput-queues"></a>Több nagy átviteli sorban várakozó

Cél: A több sorok általános átviteli teljes méretre állítása Az egy adott várólista átviteli közepes vagy nagy.

Különböző több sorok maximális sebesség juthat a kapacitásának egyetlen várólista maximális tagolt beállításokat használni. Ezeken kívül segítségével különböző gyárak küldhet és fogadhat másik sorban várakozó az ügyfelekre.

### <a name="low-latency-queue"></a>Kis késés várólista

Cél: Állítsa kis méretűre várólista vagy a témakör a végpontok közötti késés. A küldő és egy kis. A teljesítmény a várólista kis vagy mérsékelt.

-   Jobb elérhetőség particionált várólista használatával.

-   Tiltsa le az ügyféloldali kötegelés. Az ügyfél azonnal elküldi az üzenetet.

-   Kötegelt áruházból hozzáférés letiltása lehetőséget. A szolgáltatás az üzenet azonnal írja meg az áruház képernyőt.

-   Ha egy ügyfél, állítsa az előzetes lehívása száma 20 alkalommal feldolgozás mennyi díjat kell a címzett. Ha a sor egyszerre több üzenetet érkeznek, a szolgáltatás Bus ügyfél protokoll továbbítja őket az összes egyszerre. Az ügyfél pedig a következő üzenetet kapja, ha a megjelölt üzenetek helyi gyorsítótár már szerepel. A gyorsítótár kis kell lennie.

-   Ha több ügyfél, adja meg az előzetes lehívása száma 0. Ezzel a második ügyfél is megjelenik a második üzenet, az első ügyfél továbbra is az első üzenet feldolgozása közben.

### <a name="queue-with-a-large-number-of-senders"></a>A sok feladók sorba

Cél: Átviteli várólista vagy a témakör a feladók nagyszámú teljes méretre állítása Mindegyik feladónak mérsékelt sebességének üzeneteket küld. A vevők egy kis.

Bus szolgáltatás lehetővé teszi, hogy egy üzenetben személyhez legfeljebb 1000 egyidejű kapcsolatok (vagy 5000 AMQP használata). Ezt a korlátot érvényesül névtere szintre, és a sorok/témakörök/előfizetések fedett per névtér egyidejű kapcsolatok határa. A sorok ennek a számnak megosztott küldő és között. Az összes 1000 kapcsolatok szükség feladók, a sor egyetlen előfizetést és a tematikus kell helyére. A tematikus elfogadja a feladók listában legfeljebb 1000 egyidejű kapcsolatot, mivel az előfizetés elfogadja a egy további 1000 egyidejű kapcsolatot vevők. 1000-nél több egyidejű feladók szükség, ha a feladók kell üzeneteket küldeni a szolgáltatás Bus protokoll HTTP-n keresztül.

A teljes méret átviteli, tegye a következőket:

-   Nagyobb teljesítmény és a elérhetősége particionált várólista használható.

-   Ha mindegyik feladónak található egy másik folyamat, használja a csak egyetlen gyári egy folyamat.

-   Aszinkron műveletek segítségével ügyféloldali kötegelés előnyeit.

-   Használja az alapértelmezett, 20 MS időtartományt kötegelés szolgáltatás Bus ügyfél protokoll átvitelek számának csökkentése érdekében.

-   Kilépés a kötegelt áruházból access engedélyezve van. Ez növeli a teljes ráta, amelynél üzenetek áthelyezése a várólista vagy a témakör írt.

-   Állítsa az előzetes lehívása száma 20 alkalommal maximális feldolgozás mértékének gyár minden címzett. Ez a szolgáltatás Bus ügyfél protokoll átvitelek számát csökkenti.

### <a name="queue-with-a-large-number-of-receivers"></a>A sok címzett sorba

Cél: Fogadási mennyi díjat kell egy várólista vagy -előfizetésre sok címzett tartalmazó teljes méretre állítása Minden címzett fogadja üzeneteket, mérsékelt sebessége. A feladók egy kis.

Bus szolgáltatás lehetővé teszi, hogy legfeljebb 1000 egyidejű egyed-kapcsolatot. Ha várólista 1000-nél több vevők van szüksége, akkor kell cserélni a várólista témakör és több előfizetéssel. Egyes előfizetések legfeljebb 1000 egyidejű kapcsolatok is támogatja. Másik lehetőségként vevők érheti el a sorban a HTTP protokollon keresztül.

A teljes méret átviteli, tegye a következőket:

-   Nagyobb teljesítmény és a elérhetősége particionált várólista használható.

-   Ha minden címzett található egy másik folyamat, használja a csak egyetlen gyári egy folyamat.

-   Címzett szinkron vagy aszinkron műveletekre használható. Megadni egy adott címzett mérsékelt fogadási mértéke, a teljes kérelem kötegelés ügyféloldali nem befolyásolja címzettnek kapacitása.

-   Kilépés a kötegelt áruházból access engedélyezve van. Ez a csökkenti a szervezet általános betöltése. Azt is csökkenti a teljes, amelynél üzenetek áthelyezése a várólista vagy a témakör írt.

-   Az előzetes lehívása száma egy kis értékre állítani (például PrefetchCount = 10). Ez a vevők megakadályozza üresjárati, míg a többi címzett nagyszámú gyorsítótárazott üzeneteket.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Témakör, amelyben kisszámú előfizetések

Cél: A átviteli, amelyben kisszámú előfizetések a témakör teljes méretre állítása Üzenet érkezik, sok előfizetést, ami azt jelenti, hogy minden előfizetés fölé a kombinált fogadási ráta nagyobb, mint a Küldés ráta. A feladók egy kis. A vevők előfizetésenként egy kis.

A teljes méret átviteli, tegye a következőket:

-   Nagyobb teljesítmény és a elérhetősége particionált témakör használható.

-   Az általános küldési sebességének növeléséhez a témakörbe segítségével több üzenetet gyárak feladók. Mindegyik feladónak használja a műveletek aszinkron vagy több beszélgetésekben.

-   Ha növelni szeretné a teljes fogadási ráta előfizetésből, segítségével több üzenetet gyárak vevők. Minden címzett a műveletek aszinkron vagy több témák használja.

-   Aszinkron műveletek segítségével ügyféloldali kötegelés előnyeit.

-   Használja az alapértelmezett, 20 MS időtartományt kötegelés szolgáltatás Bus ügyfél protokoll átvitelek számának csökkentése érdekében.

-   Kilépés a kötegelt áruházból access engedélyezve van. Az általános ráta, amelyre a témakör az üzenetek írhatók esetében növelése.

-   Állítsa az előzetes lehívása száma 20 alkalommal maximális feldolgozás mértékének gyár minden címzett. Ez a szolgáltatás Bus ügyfél protokoll átvitelek számát csökkenti.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Az előfizetések nagyszámú témakör

Cél: A témakör az előfizetések nagyszámú kapacitásának teljes méretre állítása Egy üzenetet megkapta sok előfizetést, ami azt jelenti, hogy minden előfizetés fölé a kombinált fogadási ráta sokkal nagyobb, mint a Küldés kamatláb. A feladók egy kis. A vevők előfizetésenként egy kis.

Az előfizetések nagyszámú témakörök általában egy alacsony általános átviteli fedheti fel, ha az összes üzenetre résztvevőkhöz minden előfizetés. Ennek oka az, hogy minden üzenet érkezik sokszor, és a témakörben szereplő összes üzenetre, és minden előfizetés ugyanabban a tárolóban találhatók. Feltételezzük, hogy a feladók és vevők előfizetésenként számát kis. Szolgáltatás Bus per témakör legfeljebb 2000 előfizetések támogatja.

A teljes méret átviteli, tegye a következőket:

-   Nagyobb teljesítmény és a elérhetősége particionált témakör használható.

-   Aszinkron műveletek segítségével ügyféloldali kötegelés előnyeit.

-   Használja az alapértelmezett, 20 MS időtartományt kötegelés szolgáltatás Bus ügyfél protokoll átvitelek számának csökkentése érdekében.

-   Kilépés a kötegelt áruházból access engedélyezve van. Az általános ráta, amelyre a témakör az üzenetek írhatók esetében növelése.

-   Állítsa az előzetes lehívása száma 20 alkalommal a várt fogadási ráta a másodpercben megadott. Ez a szolgáltatás Bus ügyfél protokoll átvitelek számát csökkenti.

## <a name="next-steps"></a>Következő lépések

További szolgáltatás Bus teljesítmény optimalizálása, olvassa el a [Partitioned üzenetküldés szervezetek][]című témakört.

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [PeekLock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [ReceiveAndDelete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [BatchFlushInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval.aspx
  [EnableBatchedOperations]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations.aspx
  [QueueClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.prefetchcount.aspx
  [SubscriptionClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.prefetchcount.aspx
  [ForcePersistence]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.forcepersistence.aspx
  [EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [Particionált üzenetben személyek]: service-bus-partitioning.md
  