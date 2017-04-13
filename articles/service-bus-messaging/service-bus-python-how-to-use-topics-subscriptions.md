<properties 
    pageTitle="Szolgáltatás Bus témakörök használata Python |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Azure Service Bus témakörök és a Python előfizetések." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Szolgáltatás Bus témakörök és előfizetések használatával

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez a cikk ismerteti, hogyan szolgáltatás Bus témakörök és előfizetések használatára. A minták Python írt és [Python Azure csomagot][]. Érintett esetek **létrehozása, témák és az előfizetéseket**, **előfizetés szűrők létrehozása**, **üzenetküldés témakörben**, **előfizetésből üzenetek fogadására**és **témakörök és az előfizetés törlése**. További információt a témakörök és előfizetések című [Következő lépések](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Megjegyzés:** Ha Python vagy a [Python Azure csomag][]telepítéséhez szükséges, című a [Python telepítési útmutatóját](../python-how-to-install.md).

## <a name="create-a-topic"></a>Témakör létrehozása

A **ServiceBusService** objektum lehetővé teszi, hogy témakörök kezelheti. Adja hozzá a következő bármely Python fájlt, amelyben el szeretné érni programozás útján a szolgáltatás Bus tetején:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

A következő kódot létrehoz egy **ServiceBusService** -objektumot. Csere `mynamespace`, `sharedaccesskeyname`, és `sharedaccesskey` a tényleges névtér megosztott Access-aláírás (Társítások) billentyű a nevét, és a fő érték.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Az [Azure portál][]szerezhet be az értékeket a Társítások kulcs neve és értéket.

```
bus_service.create_topic('mytopic')
```

**létrehozása\_témakör** további beállításokat, amelyek lehetővé teszik, hogy az alapértelmezett témakör beállítások felülbírálása például üzenet time to live vagy a témakör maximális mérete is támogat. Az alábbi példa a 5 GB, és egyszerre szeretné live (TTL) érték 1 perc állítja be a témakör maximális mérete:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Előfizetések létrehozása

Előfizetések témakörökre mutató is létre az **ServiceBusService** objektumra. Az előfizetések neve, és beállíthatja, hogy a választható szűrő, amely a virtuális várólista az előfizetés a beérkező üzenetek készlete korlátozza.

> [AZURE.NOTE] Az előfizetések állandó, és továbbra is létezik csak azokat, vagy a témakör, amelyhez iratkozott, törlődnek.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Az alapértelmezett (MatchAll) szűrővel előfizetés létrehozása

A **MatchAll** szűrő az alapértelmezett szűrő, amely akkor használható, ha nincs szűrő van megadva, amikor új előfizetést jön létre. A **MatchAll** szűrő használatakor a témakör közzétett összes üzenet az előfizetés virtuális várólista kerülnek. A következő példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett **MatchAll** szűrő.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések szűrőkkel létrehozása

Azt is megadhatja, szűrők, amelyek lehetővé teszik, hogy adja meg, amelyek egy adott témakör előfizetés belül témakör küldött üzenetek jelenjen meg.

A legtöbb rugalmas típusú előfizetések által támogatott szűrő egy **SqlFilter**, amely SQL92 egy részét. Az üzenetek, a témakör közzétett tulajdonságainak SQL szűrők vonatkozni fognak. A kifejezések SQL szűrővel használható kapcsolatos további tudnivalókért olvassa el a [SqlFilter.SqlExpression][] szintaxisát című témakört.

Felveheti szűrők előfizetés használatával a **létrehozása\_szabály** **ServiceBusService** objektum metódusát. Ez a módszer lehetővé teszi, hogy az új szűrők hozzáadása meglévő előfizetéshez.

> [AZURE.NOTE] Az alapértelmezett szűrő automatikusan alkalmazza az összes új előfizetést, mert el kell távolítani a szűrőt, vagy a **MatchAll** felülírják a többi szűrőket is megadhat. Az alapértelmezett szabály használatával eltávolíthatja a **törlése\_szabály** **ServiceBusService** objektum metódusát.

Az alábbi példa létrehoz egy előfizetés nevű `HighMessages` **SqlFilter** , amely csak a 3-nál nagyobb egyéni **LastMessage elemet** tulajdonság üzeneteket kiválasztja a:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Hasonlóképpen a az alábbi példa létrehoz egy előfizetés nevű `LowMessages` **SqlFilter** , amely csak a **LastMessage elemet** tulajdonság kisebb vagy egyenlő 3 üzeneteket kiválasztja a:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Most, amikor egy üzenettel `mytopic` mindig vevők fizetett elő, az **AllMessages** témakör előfizetést, és a szelektív fizetett elő, a **HighMessages** és **LowMessages** témakör előfizetések (attól függően, hogy az üzenet tartalmának) vevők sikeresen kézbesítve lett kézbesítve.

## <a name="send-messages-to-a-topic"></a>A témakör üzeneteket küldeni

A szolgáltatás Bus tematikus üzenetet küldeni, az alkalmazás használatához a **küldése\_témakör\_üzenet** **ServiceBusService** objektum metódusát.

Az alábbi példa bemutatja, hogyan öt vizsgált üzeneteket küldhet `mytopic`. Megjegyzendő, hogy minden üzenet **LastMessage elemet** a tulajdonság értékét a a példányaiban le változó (Ez határozza meg, hogy melyik előfizetések kapta meg):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Szolgáltatás Bus témakörök a [szabványos réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md)használatát támogatja. A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs korlátozott tartani a témakör az üzenetek számát, de van egy vonalvég az üzenetek témakör birtokában összesített méretét. Ez a témakör a méret létrehozáskor, az 5 GB felső határa van megadva. Kvóták kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus kvóták][]című témakört.

## <a name="receive-messages-from-a-subscription"></a>Hibaüzenetek előfizetésből

Üzenetek kapott egy előfizetés használata a **fogadása\_előfizetés\_üzenet** módszer a **ServiceBusService** objektum:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Az üzenetek törlődnek az előfizetésből, amikor olvasott a paraméter **betekintő\_zárolása** értéke **hamis**. Olvassa el a (betekintő), és zárolása az üzenet törlése a sorból megadásával a paraméter nélkül **betekintő\_zárolása** **Igaz**.

A olvasási és az üzenet törlése a fogadási művelet részeként a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

Ha a **betekintő\_zárolása** paraméter értéke **Igaz**, a fogadás válik egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően az **üzenet** objektum **törlése** módszer meghívásával befejezése a másodszintű fogadási folyamat. A **Törlés** módszer megjelöli az üzenetet az éppen, és eltávolítja azt az előfizetést.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. A címzett-alkalmazások esetén nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja a **Zárolás feloldása** módszer az **üzenet** objektumon. Ennek hatására a szolgáltatás Bus zárolásának feloldása az üzenetet az előfizetésben, és tegye elérhetővé az azonos igénybe alkalmazásával vagy egy másik igénybe alkalmazás ismét kapott.

Is van egy üzenetet, az előfizetés belül zárolt társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenet a zárolási időkorlát lejár (például ha az alkalmazás összeomlik), majd a szolgáltatás Bus automatikusan feloldja az üzenetet, és újra kapott számára elérhetővé teszi azt.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **törlése** módszer neve előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább után feldolgozása**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el az üzenet, amely végig a kézbesítési kísérletek változatlan marad a **MessageId** tulajdonát felhasználni.

## <a name="delete-topics-and-subscriptions"></a>Témák és az előfizetés törlése

Témák és előfizetések állandó, és explicit módon törölni kell az [Azure portál][] vagy keresztül programozás útján. A következő példa bemutatja a nevű témakör törlése `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Témakör törlése a regisztrált a témakör az előfizetése is törli. Az előfizetések egymástól függetlenül is törlődnek. A következő kódrészlet szemlélteti, hogyan lehet nevű előfizetés törlése `HighMessages` a a `mytopic` témakör:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus témakörök alapjait, kövesse az alábbi hivatkozásokra kattintva további.

-   [Sorok, a témakörök és az előfizetések][]megtekintése
-   Összefoglalás: a [SqlFilter.SqlExpression][].

[Azure portál]: https://portal.azure.com
[Python Azure csomag]: https://pypi.python.org/pypi/azure  
[Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Szolgáltatás Bus kvóták]: service-bus-quotas.md 
