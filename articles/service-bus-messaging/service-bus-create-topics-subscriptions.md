<properties 
    pageTitle="Hozzon létre szolgáltatás Bus témakörök és előfizetések használó alkalmazások |} Microsoft Azure"
    description="Bevezetés a közzététel-előfizetés szolgáltatás Bus témakörök és előfizetések által kínált lehetőségek."
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Szolgáltatás Bus témakörök és előfizetések használó alkalmazások létrehozása

Azure Service Bus többek között a megbízható üzenetsor felhőalapú, üzenet készült köztes technológiák és tartós közzététel/előfizetés üzenetküldés támogatja. Ez a cikk [létrehozása szolgáltatás Bus sorban várakozó használó](service-bus-create-queues.md) alkalmazásokban megadott adatokkal épül, és felajánlja a közzététel/előfizetés funkciók szolgáltatás Bus témakörök által kínált bemutatása.

## <a name="evolving-retail-scenario"></a>Kiskereskedelmi forgatókönyv változó

Ez a cikk a kiskereskedelmi eset [létrehozása szolgáltatás Bus sorban várakozó használó](service-bus-create-queues.md)alkalmazásokban használt továbbra is. Értékesítési adatok az egyes pont az értékesítés (POS) terminálok egy készlet rendszer határozza meg, amikor a feltöltendő van árfolyam adatokat használó kell irányítani visszahívása. Minden egyes POS Terminálszolgáltatások az értékesítési adatok jelentések az üzenetek küldése a **DataCollectionQueue** sorba, ahol maradnak mindaddig, amíg a készlet rendszer, fogadja itt ismertetett módon:

![Szolgáltatás Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Is alapkoncepciójára ebben az esetben, egy új követelmény hozzá lett adva a rendszer: az üzlet tulajdonosa szeretne nyomon követhesse, hogyan az áruház alkalmazásban valós időben hajt végre.

Ez a követelmény címének, a rendszer kell "koppintson" az értékesítési adatok adatfolyam ki. Minden egyes által küldött üzenet a POS terminálok el lehet küldeni a készlet rendszer, mielőtt a továbbra is szeretnénk, de azt szeretné, hogy minden üzenet, amely azt segítségével az üzlet tulajdonosa kell mutatniuk a munkájukat a irányítópult megtekintése egy másik példányát.

Ebben az esetben minden üzenet több felek által igénybe vehető minden például minden olyan helyzet szolgáltatás Bus *témaköröket*is használhatja. Témakörök tartalmaznak, amelyben minden egyes közzétett üzenet elérhetővé válik a témakör regisztrált egy vagy több előfizetésekhez közzététel/előfizetés mintát. Viszont a sorok minden üzenet érkezik egy egyetlen ügyfél.

Üzenetek küldése témakörben ugyanúgy küldött egy sorba. Jó helyen jár amelyek nem érkező üzenetek a témakör közvetlenül; az előfizetések érkeznek. Érdemes elképzelnie, amely az adott témához küldött üzenetek másolatát kapja virtuális várólista témakörben előfizetést. Üzenetek érkeznek előfizetésből ugyanúgy a sorból beérkező.

Ugrás a kiskereskedelmi forgatókönyvet vissza, a sor helyébe a témakör, és előfizetést hozzá van rendelve, amely a készlet management-összetevő használhatja. A rendszer ekkor a következőképpen jelenik meg:

![Szolgáltatás Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

A konfiguráció itt az előző várólista alapú tervezés azonos hajt végre. Ez azt jelenti, hogy a témakör küldött üzenetek továbbítása a **készlet** előfizetéséhez, ahonnan a **Készlet Projektirányítási rendszerek** fogyasztása őket.

Az adatkezelési irányítópult támogatásához hozzunk létre egy második előfizetés című témakör, itt ismertetett módon:

![Szolgáltatás Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Ebben a konfigurációban minden üzenet a POS részére elérhetővé válik az **Irányítópult** és a **készlet** -előfizetésekhez.

## <a name="show-me-the-code"></a>A kód megjelenítése

A cikk [létrehozása alkalmazást használó szolgáltatás Bus sorban várakozó](service-bus-create-queues.md) Regisztrálás az Azure-fiók, és hozzon létre egy szolgáltatás névtere ismerteti. Szolgáltatás Bus névteret használatához az alkalmazások hivatkoznia kell a Service Bus összeállítás, mégpedig Microsoft.ServiceBus.dll. A szolgáltatás Bus függőségek hivatkozás legegyszerűbben a szolgáltatás Bus [Nuget csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus/)telepítéséhez. Az összeállítás az Azure SDK részeként is megtalálhatók. A letöltés érhető el az [Azure SDK letöltési oldalon](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>A témakör és előfizetések létrehozása

Adatkezelési az üzenetkezelés személyek (sorok és a közzététel/előfizetés témakörök) szolgáltatás Bus műveleteket a [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály keresztül. Annak érdekében, hogy egy adott névtér [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) példány létrehozása szükség van a megfelelő hitelesítő adatokkal. Szolgáltatás Bus egy olyan [Megosztott Access-aláírás (Társítások)](service-bus-sas-overview.md) alapú biztonsági modellel használja. A [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) osztály jelöli a biztonsági jogkivonat-szolgáltató és visszatérő néhány közismert jogkivonat szolgáltatók beépített gyári módszerek. Egy [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) módot használjuk tartása a Társítások hitelesítő adatokat. A [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) példány majd épül fel a szolgáltatás Bus névtér és a jogkivonat-szolgáltató alapcímének.

A [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály hozhat létre, számba veheti a webhely és törlése az üzenetek szervezetek bemutat. Az itt ismertetett kódját jeleníti meg, hogyan [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) példány létrejön, és a **DataCollectionTopic** témakör létrehozásához használt.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Figyelje meg, hogy nincsenek-e a [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) módszer túlterhelések, amely lehetővé teszi a témakör tulajdonságainak beállítása. Ha például beállíthatja, hogy az alapértelmezett time-to-live (TTL) értékét a témakör küldött üzeneteket. Ezután adja hozzá a **készlettel** és az **Irányítópult** előfizetéseket.

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>A témakör üzeneteket küldeni

Futási idejű műveletekhez kattintson a személyek szolgáltatás Bus; Ha például üzenetek küldése és fogadása, az alkalmazások először létre kell hoznia egy [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objektum. A [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) osztály hasonlít az [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) példány alapján létre a szolgáltatás névtere és a jogkivonat-szolgáltató alapcímének.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Üzeneteket küldeni és szolgáltatás Bus témakörök érkezett, a [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) osztály példányait. Az osztály áll egy sor olyan általános tulajdonságok (például [címke](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) és [az élettartam](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), a szótárt, amelyet a szolgáltatásalkalmazás tulajdonságainak és a szervezet tetszőleges alkalmazás adatok tárolására szolgál. Az alkalmazás által a (az alábbi példa a POS terminált az értékesítési adatait megjelenítő **SalesData** objektum átadja) szerializálható objektum átadása a szervezetet, amely a [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) kell használni az objektum szerializálható állíthat be. Másik lehetőségként [adatfolyam](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -objektumok is biztosítható.

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Az üzeneteket küldeni a témakör legegyszerűbben [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) -objektum létrehozása közvetlenül az [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) példány [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) használatával.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Hibaüzenetek előfizetésből

Sorok, hasonló üzenetek fogadására használhat egy [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objektumot, amely közvetlenül a [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)használatával létrehozott előfizetésről. Használhatja a másik két módok (**ReceiveAndDelete** és **PeekLock**) kap, [szolgáltatás Bus sorban várakozó használó létrehozása](service-bus-create-queues.md)alkalmazásokban tárgyalt.

Ne feledje, hogy egy [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) előfizetésekhez létrehozásakor a *entityPath* paraméter az űrlap `topicPath/subscriptions/subscriptionName`. Emiatt a [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) **DataCollectionTopic** témakör **készlet** előfizetéshez létrehozásához *entityPath* meg kell `DataCollectionTopic/subscriptions/Inventory`. A kódot a következőképpen jelenik meg:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
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

## <a name="subscription-filters"></a>Előfizetés szűrők

Eddig ebben az esetben a témakör küldött összes üzenet is elérhetők az összes regisztrált-előfizetésekhez. A fő kifejezést "lett érhető el." Szolgáltatás Bus előfizetések a témakör küldött összes üzenet jelenik meg, amíg a virtuális előfizetés sorba másolhatja azokat az üzeneteket csak egy részét. Ez a előfizetés *szűrők*használatával történik. Amikor létrehoz egy előfizetést, adja meg, amely fölé a tulajdonságok az üzenet, a rendszer tulajdonságai (például a [címke](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) és az alkalmazás tulajdonságait, például a **storename mezőt** az előző példában is működik, SQL92 stílus-predikátumok formájában szűrőkifejezés.

Változó a forgatókönyvek ezt mutatják be, a második áruházból van adható hozzá a kiskereskedelmi forgatókönyv. Minden a POS terminálok mindkét tárolja az értékesítési adatok továbbra is a központi készlet projektirányítási rendszerek átirányítását, de az irányítópult eszközzel áruházból kezelője csak érdekli áruház teljesítményét. Szűrés cél előfizetés is használhatja. Figyelje meg, hogy a POS terminálok üzeneteket közzétenni, ha azokat a tulajdonság **storename mezőt** alkalmazás meg az üzenetet. Megadott két tárolja, például **Redmond** és **Seattle**, a redmondig vezető út a POS terminálok tárolni a **storename mezőt** azzal egyenlő **Redmond**, mivel a Seattle-tár POS terminálok használja a **storename mezőt** az értékesítési adatok üzeneteik egyenlő **Seattle**bélyegző. A Redmond store áruházból menedzserének csak szeretné megtekinteni az adatokat a POS részére. A rendszer a következőképpen jelenik meg:

![Szolgáltatás-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

A továbbítás beállításához hoz létre az **Irányítópult** -előfizetés az alábbi képlettel történik:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Csak a **redmondig vezető út** a **storename mezőt** tulajdonsága üzeneteket a program a előfizetés szűrővel az **Irányítópult** -előfizetés a virtuális várólista másolja. Nincs sokkal több előfizetés szűrést, akkor jó helyen jár. Alkalmazások beállíthatja, hogy több szűrő szabályok előfizetésenként kívül üzenet tulajdonságainak módosítását, mint egy előfizetés virtuális várólista továbbítja.

## <a name="summary"></a>Összefoglalás

Queuing [szolgáltatás Bus sorban várakozó használó alkalmazások létrehozása](service-bus-create-queues.md) a használandó okokból is alkalmazása témakörök kifejezetten:

- Időbeli szétválasztás – üzenet gyártók és a fogyasztói nem kell online egy időben.

- Terheléselosztás – által igénybe alkalmazások kell építenie átlagos terhelés helyett csúcs betöltés engedélyezése a témakör az betöltés csúcsok vannak Görbített.

- Terheléselosztás – várólista hasonlóan lehet egyetlen előfizetést, az egyes fogyasztói, ezáltal a betöltés terheléselosztás egyikére beadását üzenetekkel figyel több versengő vonzóbbak lehetnek.

- Laza összekapcsolási – is is alapkoncepciójára az üzenetben hálózati meglévő végpontok; megtartásával például az előfizetések megadása vagy módosítása szűrők-témakörben az új fogyasztói engedélyezni.

## <a name="next-steps"></a>Következő lépések

Lásd: a [szolgáltatás Bus sorban várakozó használó alkalmazások létrehozása](service-bus-create-queues.md) a POS kiskereskedelmi forgatókönyv sorban várakozó használatáról olvashat.