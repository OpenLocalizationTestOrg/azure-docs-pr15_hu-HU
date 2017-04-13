<properties 
    pageTitle="Szolgáltatás Bus sorban várakozó használata Python |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Python az Azure Service Bus sorok használja." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Szolgáltatás Bus sorban várakozó használata

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez a cikk ismerteti, hogyan szolgáltatás Bus sorban várakozó használni. A minták Python írt és [Python Azure Service Bus csomagot][]. Az érintett esetek **létrehozása sorban várakozó, üzenetek küldése és fogadása**, és a **Sorok törlése**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Python vagy a [Python Azure Service Bus csomag][]telepítéséhez [Python telepítési útmutatót](../python-how-to-install.md)találhat.

## <a name="create-a-queue"></a>Várólista létrehozása

A **ServiceBusService** objektum lehetővé teszi, hogy sorban várakozó kezelheti. Adja hozzá a bármely Python fájl, amelyben el szeretné érni programozás útján a szolgáltatás Bus felső részén a következő kódot:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

A következő kódot létrehoz egy **ServiceBusService** -objektumot. Csere `mynamespace`, `sharedaccesskeyname`, és `sharedaccesskey` névtér, a megosztott access aláírás (Társítások) kulcs neve és a értéket.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Az értékek a Társítások kulcs neve és a érték található [Azure klasszikus portál][] kapcsolati adatait, illetve a Visual Studio **Tulajdonságok** ablaktáblában kijelölésekor a szolgáltatás Bus névtér Server Explorer (mint az előző szakaszban látható).

```
bus_service.create_queue('taskqueue')
```

**create_queue** támogatja a további beállítások, amely lehetővé teszi, hogy az alapértelmezett várólista beállítások felülbírálása például üzenet time to live (TTL) vagy várólista maximális mérete is. A következő példa 5GB, és a TTL Oszlopbeli értéket, 1 percre állítja be a várólista maximális mérete:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Üzenetek küldése egy sorba

Üzenet küldése egy szolgáltatás Bus sorba, az alkalmazás hívásai a **küldése\_várólista\_üzenet** módszer a **ServiceBusService** objektumon.

A következő példa bemutatja, hogyan vizsgálati üzenet küldése a *taskqueue használatával* nevű sorba **küldése\_várólista\_üzenet**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Szolgáltatás Bus sorban várakozó támogatja a [Normál réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md). A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs várólista tartott üzenetek száma korlátozott, de van egy vonalvég az üzenetek várólista birtokában összesített méretét. A várólista mérete létrehozáskor, az 5 GB felső határa van megadva. Kvóták kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus kvóták][]című témakört.

## <a name="receive-messages-from-a-queue"></a>Üzenetek fogadására várólista

Üzenetek kapott egy várólista használata a **fogadása\_várólista\_üzenet** módszer a **ServiceBusService** objektum:

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Az üzenetek törlődnek a sorból, amikor olvasott a paraméter **betekintő\_zárolása** értéke **hamis**. Olvassa el a (betekintő), és zárolása az üzenet törlése a sorból megadásával a paraméter nélkül **betekintő\_zárolása** **Igaz**.

A olvasási és az üzenet törlése a fogadási művelet részeként a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

Ha a **betekintő\_zárolása** paraméter értéke **Igaz**, a fogadás válik egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a fogadási folyamat a másodszintű befejezése hívja fel a **törlése** módszer az **üzenet** objektumon. A **törlése** módszer fog megjelölni az üzenetet az éppen, és távolítsa el azt a sorból.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. A címzett-alkalmazások esetén nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja a **Zárolás feloldása** módszer az **üzenet** objektumon. Ennek hatására a szolgáltatás Bus zárolásának feloldása a várólista belül az üzenetet, és ismét kapott igénybe alkalmazásával ugyanazon vagy másik igénybe alkalmazás elérhetővé.

Is van zárolva van, a sor belül üzenet társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenetet a zárolási időkorlát lejár (pl., ha az alkalmazás összeomlik), akkor szolgáltatás Bus zárolásának feloldásához automatikusan az üzenetet, és azt elérhetővé kapott újra.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **törlése** módszer neve előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább után feldolgozása**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el az üzenetet, amely végig a kézbesítési kísérletek változatlan marad **MessageId** tulajdonságával.

## <a name="next-steps"></a>Következő lépések

Most, hogy rendelkezik megtanulta szolgáltatás Bus sorban várakozó alapjait, kövesse az alábbi hivatkozásokra kattintva további.

-   [Sorok, a témakörök és az előfizetések][]megtekintése

[Azure klasszikus portál]: https://manage.windowsazure.com
[Python Azure Service Bus csomag]: https://pypi.python.org/pypi/azure-servicebus  
[Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
[Szolgáltatás Bus kvóták]: service-bus-quotas.md
 
