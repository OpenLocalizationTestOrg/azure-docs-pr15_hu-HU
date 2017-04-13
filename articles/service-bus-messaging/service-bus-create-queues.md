<properties 
    pageTitle="Írás szolgáltatás Bus sorban várakozó használó alkalmazások |} Microsoft Azure"
    description="Hogyan lehet egy egyszerű várólista-alapú alkalmazás, amelyet használ szolgáltatási Bus írni."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Szolgáltatás Bus sorban várakozó használó alkalmazások létrehozása

Ez a témakör ismerteti a szolgáltatás Bus sorban várakozó, és bemutatja, hogyan írhat egy egyszerű várólista-alapú alkalmazás, amelyet használ szolgáltatási Bus.

Fontolja meg a földrajzi kiskereskedelmi, amelyben egyéni Point-of-Sale (POS) részére értékesítési adatok kell irányítani egy készlet rendszer határozza meg, amikor a feltöltendő van árfolyam az adatokat használó példa. A megoldás használ, a terminálok és a készlet rendszer, közötti kommunikációhoz üzenetküldési szolgáltatás Bus az alábbi ábrán látható módon:

![Szolgáltatás Bus sorban várakozó kép 1](./media/service-bus-create-queues/IC657161.gif)

Minden egyes POS Terminálszolgáltatások üzeneteket küld a **DataCollectionQueue**használó jelentések az értékesítési adatok. Az alábbi üzenetek marad a várakozási sorban található adatok lekérése a készlet projektirányítási rendszerek. A minta gyakran nevezik *aszinkron üzenetküldést*, mert a POS terminált nem kell várja meg a készlet-kezelő rendszerből továbbra is a feldolgozás válasz.

## <a name="why-queuing"></a>Miért queuing?

Mielőtt megnézi az Ez az alkalmazás beállítása szükséges a kódot, fontolja meg a előnyei várólista ebben az esetben a POS terminálok bízza beszélhet közvetlenül (szinkron) a készlet management rendszerbe.

### <a name="temporal-decoupling"></a>Időbeli szétválasztás

Az aszinkron üzenetben mintázattal gyártók és a fogyasztói nem kell lenniük online egy időben. Az üzenetkezelési infrastruktúra üzeneteket biztos, hogy tárolja, addig, amíg a igénybe fél kész fogadására őket. Ez azt jelenti, hogy az elosztott alkalmazás összetevői megszakad, vagy önkéntes; például karbantartási, vagy egy összetevő összeomlik miatt nélkül érintő a teljes rendszerben. Ezenkívül a igénybe alkalmazás előfordulhat, hogy csak kell lenniük online a nap bizonyos idején. Ha például kiskereskedelmi ebben az esetben a készlet projektirányítási rendszerek csak lehetnek a munkanap végét követő online állapotba.

### <a name="load-leveling"></a>Simítás betöltése

Számos alkalmazás rendszer betöltés változó időbeli, mivel minden egység munka szükséges időt feldolgozás általában állandó. Intermediating üzenet gyártók és a fogyasztói várólista az azt jelenti, hogy az igénybe alkalmazás (dolgozó) csak a csúcs terhelést, hanem egy átlagos terhelés szolgáltatás kell építenie. A várakozási sorban található mélységét a nagyobb, és a szerződés, a bejövő betöltés változik. Ez közvetlenül kapcsolatban az alkalmazás terhelés szükséges infrastruktúra mennyiségét pénzt menti.

![Szolgáltatás Bus sorban várakozó kép 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Terheléselosztás

Növeli a betöltés, mint további dolgozó folyamatok felvehetők a dolgozó sorból olvasható. Minden üzenet a dolgozó folyamatok közül csak akkor dolgozza fel. Ezenkívül a leküldéses-alapú terheléselosztási lehetővé teszi az optimális használat dolgozó számítógépek, ha a dolgozó számítógépek különbözőek feldolgozás power, azok üzenetek fog húzza a saját maximális sebesség. A minta gyakran adja meg a versengő fogyasztói mintázatot.

![Szolgáltatás Bus sorban várakozó kép 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Laza kapcsoló

Üzenet gyártók és a fogyasztói között haladó üzenetsor körét egy belső laza összekapcsolási összetevők között. Mivel gyártók és a fogyasztói nem érdemes szem előtt, minden más, a fogyasztói anélkül, hogy semmilyen hatása a gyártó frissíthető. Ezenkívül a üzenetben topológia anélkül, hogy a meglévő végpontokat is is alapkoncepciójára. Mutatjuk Ez több közzététel/előfizetés megvitatjuk, amikor.

## <a name="show-me-the-code"></a>A kód megjelenítése

A következő szakaszban az alkalmazás létrehozásához használandó szolgáltatás Bus mutatja.

### <a name="sign-up-for-an-azure-account"></a>Regisztrálás az Azure-fiók

Indítsa el a szolgáltatás Bus használata érdekében szüksége lesz az Azure-fiók. Ha nem már rendelkezik egy, jelentkezzen egy ingyenes fiókra [Itt](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Névtér létrehozása

Ha befejezte az előfizetést, akkor [Hozzon létre egy új névtér](service-bus-create-namespace-portal.md). Minden névtér végpontként tartalmazó tároló szolgáltatás Bus szervezetek számára. Nevezze el az új névtér egyedi minden szolgáltatás Bus fiók között. 

### <a name="install-the-nuget-package"></a>A NuGet csomag telepítéséhez

A szolgáltatás Bus névtér használatához az alkalmazások hivatkoznia kell a Service Bus összeállítás, mégpedig Microsoft.ServiceBus.dll. A Microsoft Azure SDK részeként ebben összeállítás talál, és a letöltés érhető el az [Azure SDK letöltési oldalon](https://azure.microsoft.com/downloads/). A [szolgáltatás Bus NuGet csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) viszont a legegyszerűbben a szolgáltatás Bus API és állítsa be az alkalmazás az összes szolgáltatás Bus függőségek.

### <a name="create-the-queue"></a>A sor létrehozása

Adatkezelési az üzenetkezelés személyek (sorok és a közzététel/előfizetés témakörök) szolgáltatás Bus műveleteket a [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály keresztül. Szolgáltatás Bus egy olyan [Megosztott Access-aláírás (Társítások)](service-bus-sas-overview.md) alapú biztonsági modellel használja. A [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) osztály jelöli a biztonsági jogkivonat-szolgáltató és visszatérő néhány közismert jogkivonat szolgáltatók beépített gyári módszerek. Egy [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) módot használjuk tartása a Társítások hitelesítő adatokat. A [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) példány majd épül fel a szolgáltatás Bus névtér és a jogkivonat-szolgáltató alapcímének.

A [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály hozhat létre, számba veheti a webhely és törlése az üzenetek szervezetek bemutat. Az itt ismertetett kódját jeleníti meg, hogyan [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) példány létrejön, és a **DataCollectionQueue** várólista létrehozásához használt.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Figyelje meg, hogy nincsenek-e a [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) módszer túlterhelések, amelyek lehetővé teszik a várólista kell állítani a tulajdonságok. Ha például beállíthatja, hogy az alapértelmezett time-to-live (TTL) értékét a várólista küldött üzeneteket.

### <a name="send-messages-to-the-queue"></a>A várakozási sorban található üzenetek küldése

Futási idejű műveletekhez kattintson a személyek szolgáltatás Bus; Ha például üzenetek küldése és fogadása, az alkalmazások először létre kell hoznia egy [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objektum. A [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály hasonlít az [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) példány alapján létre a szolgáltatás névtere és a jogkivonat-szolgáltató alapcímének.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Üzeneteket küldeni, és kapott szolgáltatás Bus sorban várakozó [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) osztály példányai. Az osztály áll egy sor olyan általános tulajdonságok (például [címke](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) és [az élettartam](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), a szótárt, amelyet a szolgáltatásalkalmazás tulajdonságainak és a szervezet tetszőleges alkalmazás adatok tárolására szolgál. Az alkalmazás által a (az alábbi példa a POS terminált az értékesítési adatait megjelenítő **SalesData** objektum átadja) szerializálható objektum átadása a szervezetet, amely a [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) kell használni az objektum szerializálható állíthat be. Másik lehetőségként [adatfolyam](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -objektumok is biztosítható.

Az üzeneteket küldeni egy adott várólista az esetben a **DataCollectionQueue**legegyszerűbben [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) -objektum létrehozása közvetlenül az [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) példány [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) használatával.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>A sorból üzenetek fogadása

A várakozási sorban található érkező üzenetek fogadására, a [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objektumot, amely közvetlenül a [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)használatával létrehozott is használhatja. Üzenet vevők is dolgozhat, két különböző módokban: **ReceiveAndDelete** és **PeekLock**. A [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) be a program, amikor az üzenetet fogadó jön létre, a [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) hívásba paraméter.


Amikor a **ReceiveAndDelete** mód, a fogadás-e az egyetlen-kép; a művelet Ez azt jelenti, hogy szolgáltatás Bus megkapja a kérelmet, ha azt az üzenetet az éppen, és adja vissza azt az alkalmazás. **ReceiveAndDelete** mód legegyszerűbb modellje, és a legjobban alkalmazási helyzetek, amelyben az alkalmazás elviseli, egy üzenet nem feldolgozása, ha a hiba fordul elő. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus megjelölt felhasználás alatt álló, amikor az alkalmazás újraindítása és használata más újra üzenetek elindul, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált az üzenetet.

**PeekLock** módban a fogadás lesz egy két lépésből álló, a művelet, amely támogatja az alkalmazásokat, amelyek nem elviselni hiányzó üzenetek lehetővé teszi. Szolgáltatás Bus megkapja a kérelmet, ha azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a másodszintű fogadási folyamat befejezése hívja fel a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) üzenet a beérkezett. Szolgáltatás Bus láthatja a hívás [befejezése](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , amikor éppen felhasznált az üzenetet olvasottként jelöli.

Két többi eredményhez lehetségesek. Először is, ha az alkalmazás nem lehet feldolgozni az üzenetet valamilyen okból azt felhívhatja [elhagyja](https://msdn.microsoft.com/library/azure/hh181837.aspx) (helyett a [teljes](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)) kapott üzenet a. Ennek hatására a szolgáltatás Bus zárolásának feloldása az üzenetet, és tegye elérhetővé a azonos fogyasztói vagy egy másik befejezése fogyasztói ismét kapott. Második, a zárolás társított időtúllépés és, ha az alkalmazás nem lehet feldolgozni azokat az üzenet előtt a zárolás időtúllépési lejár (például ha az alkalmazás összeomlik), majd a szolgáltatás Bus zárolásának feloldása az üzenetet, és azt elérhetővé újra kapott (lényegében egy [elhagyja](https://msdn.microsoft.com/library/azure/hh181837.aspx) művelet végrehajtása alapértelmezés szerint).

Figyelje meg, hogy az alkalmazás összeomlik, az üzenetek feldolgozásával, de a [Befejezés](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) előtt kérelem adta ki, ha az üzenet fog kell újra az alkalmazás újraindításkor. Ez gyakran nevezik *Legalább egyszer* feldolgozása. Ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az alkalmazási példát ismétlődő feldolgozása nem elviselni, majd további logika ismétlődések feltárása az alkalmazásban van szükség. Ez lehet elérni az üzenet a [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) tulajdonság alapján. Ez a tulajdonság értékét végig a kézbesítési kísérletek változatlan marad. Ezt *Pontosan egyszer* feldolgozás nevezik.

A kód, amely az itt ismertetett fogadása, és a **PeekLock** mód, amelyek az alapértelmezett érték, ha nincs [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) érték explicit módon megadva az üzenetek feldolgozásával.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

### <a name="use-the-queue-client"></a>A várakozási ügyfél használata

A Példák korábbi részében létrehozott [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) és [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objektumok közvetlenül az üzenetek küldése és fogadása a sorból, illetve a [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) . Alternatív megközelítés környezetbe a [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) osztály, mely támogatja egyaránt működőképessé tehető a műveletek speciális funkciókat, például a munkamenetek mellett.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta sorban várakozó alapjait, olvassa el a [szolgáltatás Bus témakörök és előfizetések használó létrehozása alkalmazások](service-bus-create-topics-subscriptions.md) továbbra is a vitafórum-témakörök szolgáltatás Bus és előfizetések közzététel/előfizetés lehetőségeivel című témakört.