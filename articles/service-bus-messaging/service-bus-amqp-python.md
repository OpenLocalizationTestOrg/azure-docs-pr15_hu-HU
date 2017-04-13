<properties 
    pageTitle="Service Bus és a AMQP 1.0 Python |} Microsoft Azure"
    description="A Python szolgáltatás Bus használata AMQP."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-python-with-amqp-10"></a>A Python szolgáltatás Bus használata AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-Python egy Python nyelvi kötést Proton-C; Ez azt jelenti mint egy csomagolópapír körül c szerepelni fog motor Proton-Python alkalmazása

## <a name="download-the-proton-client-library"></a>Töltse le a Proton ügyfél-tárban

[Http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)letöltheti Proton-C és a hozzá társított kötések (beleértve a Python). A letöltés van kód-formájában. A kód készítéséhez kövesse a képernyőn megjelenő utasításokat a letöltött csomag lévő.

Ne feledje, hogy az írás időben Proton-C az SSL-támogatás csak Linux operációs rendszereken. Azure Service Bus SSL használatához, mert Proton-C (és a nyelvi kötések) csak használható szolgáltatás Bus hozzáférhet Linux adott időben. Ahhoz, hogy Proton-C az SSL használatával a Windows munka is meghívhat így jelölőnégyzet gyakran frissítéseit.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Szolgáltatás Bus sorban várakozó, téma és az előfizetések Python használata

A következő kódot mutatja üzenetek küldése és fogadása a szolgáltatás Bus a szervezet üzenetküldés.

### <a name="send-messages-using-proton-python"></a>Az üzenetek Proton-Python küldése

A következő kódot mutatja üzenetet küldeni egy szolgáltatás Bus entitás üzenetküldés.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Az üzenetek Proton-Python fogadása

A következő kódot mutatja hibaüzenetet küld egy szolgáltatás Bus entitás üzenetküldés.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>.NET és Proton-Python közötti Csevegés

### <a name="application-properties"></a>Szolgáltatásalkalmazás tulajdonságainak

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python karbantartására Bus .NET API-hoz

Proton-Python üzenetek támogatja a következő típusú alkalmazás tulajdonságai: **int**, **hosszú**, **lebegtetés**, **uuid**, **logikai**és **karakterlánc**. A következő Python kódot tulajdonságainak beállítása egy üzeneten mindegyik tulajdonság segítségével mutatja be.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

A szolgáltatás Bus .NET API üzenet szolgáltatásalkalmazás tulajdonságainak végzik a [BrokeredMessage][] **Properties** gyűjteményéhez. A következő kódot olvassa el az alkalmazás tulajdonságai Python ügyfél kapott üzenet mutatja.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

Az alábbi táblázatban felsorolt Python tulajdonság a .NET tulajdonság típusok rendeli hozzá.

| Python tulajdonság típusa | .NET-tulajdonság típusa |
|----------------------|--------------------|
| int                  | int                |
| Lebegtetés                | dupla             |
| hosszú                 | Int64              |
| UUID                 | globálisan egyedi azonosítója               |
| logikai                 | logikai               |
| karakterlánc               | karakterlánc             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Szolgáltatás Bus .NET API-hoz való Proton-Python

A [BrokeredMessage][] típusú alkalmazás tulajdonságait a következő típusú támogatja: **bájt**, **sbyte**, **karakter**, **rövid**, **ushort**, **int**, **uint**, **hosszú**, **ulong**, **lebegtetés**, **dupla**, **decimális**, **logikai**, **globálisan egyedi azonosítója**, **karakterlánc**, **Uri**, **DateTime**, **DateTimeOffset**, és **időszak**. A következő .NET-kód használatával mindegyik tulajdonság [BrokeredMessage][] objektum tulajdonságainak beállítása mutatja.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

A következő Python kódot olvassa el a szolgáltatás Bus .NET ügyféltől kapott üzenet alkalmazás tulajdonságainak mutatja.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

Az alábbi táblázat a .NET tulajdonság típusok Python tulajdonság fájltípusok rendeli hozzá.

| .NET-tulajdonság típusa | Python tulajdonság típusa | Jegyzetek                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bájt               | int                  | -                                                                                                                                                                     |
| sbyte              | int                  | -                                                                                                                                                                     |
| karakter               | karakter                 | Proton-Python osztály                                                                                                                                                 |
| rövid              | int                  | -                                                                                                                                                                     |
| ushort             | int                  | -                                                                                                                                                                     |
| int                | int                  | -                                                                                                                                                                     |
| uint               | int                  | -                                                                                                                                                                     |
| hosszú               | int                  | -                                                                                                                                                                     |
| ulong              | hosszú                 | Proton-Python osztály                                                                                                                                                 |
| Lebegtetés              | Lebegtetés                | -                                                                                                                                                                     |
| dupla             | Lebegtetés                | -                                                                                                                                                                     |
| decimális            | Karakterlánc               | Decimális Proton nem támogatott.                                                                                                                     |
| logikai               | logikai                 | -                                                                                                                                                                     |
| Globálisan egyedi azonosítója               | UUID                 | Proton-Python osztály                                                                                                                                                 |
| karakterlánc             | karakterlánc               | -                                                                                                                                                                     |
| Dátum és idő           | időbélyeg            | Proton-Python osztály                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | AMQP típusú megfeleltetve DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Időszak           | DescribedType        | AMQP típusú megfeleltetve Timespan.Ticks:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | AMQP típusú megfeleltetve Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Általános tulajdonságok:

Az alábbi táblázatokban megjelenítése a hozzárendelés a Proton-Python szabványos üzenet és az [BrokeredMessage][] szabványos üzenetet tulajdonságok között.

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python karbantartására Bus .NET API-hoz

| Proton-Python        | Bus .NET szolgáltatás         | Jegyzetek                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| tartós              | a #hiányzik                      | Szolgáltatás Bus csak tartós üzenetek támogatja.               |
| prioritás             | a #hiányzik                      | Szolgáltatás Bus csak egy adott üzenet prioritás támogatja.      |
| TTL (élettartam)                  | Message.TimeToLive       | Az átalakítás Proton-Python TTL ezredmásodperc van megadva. |
| első\_beszerző      | a #hiányzik                      | -                                                           |
| kézbesítési\_száma      | a #hiányzik                      | -                                                           |
| Azonosító                   | Message.MessageID        | -                                                           |
| felhasználói\_azonosító             | a #hiányzik                      | -                                                           |
| cím              | Message.To               | -                                                           |
| Tárgy              | Message.Label            | -                                                           |
| válasz\_való            | Message.ReplyTo          | -                                                           |
| korrelációs\_azonosító      | Message.CorrelationID    | -                                                           |
| tartalom\_típusa        | Message.ContentType      | -                                                           |
| tartalom\_kódolást    | a #hiányzik                      | -                                                           |
| lejárat\_idő         | a #hiányzik                      | -                                                           |
| létrehozás\_idő       | a #hiányzik                      | -                                                           |
| csoport\_azonosító            | Message.SessionId        | -                                                           |
| csoport\_sorrend      | a #hiányzik                      | -                                                           |
| válasz\_való\_csoport\_azonosító | Message.ReplyToSessionId | -                                                           |
| Formátum               | a #hiányzik                      | -                                                           |

| Bus .NET szolgáltatás        | Proton                       | Jegyzetek                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.Content\_típusa        | -                                                           |
| CorrelationId           | Message.Correlation\_azonosító      | -                                                           |
| EnqueuedTimeUtc         | a #hiányzik                          | -                                                           |
| Címke                   | Message.subject              | -                                                           |
| MessageId               | Message.ID                   | -                                                           |
| ReplyTo                 | Message.Reply\_való            | -                                                           |
| ReplyToSessionId        | Message.Reply\_való\_csoport\_azonosító | -                                                           |
| ScheduledEnqueueTimeUtc | a #hiányzik                          | -                                                           |
| Munkamenet-azonosító               | Message.group\_azonosító            | -                                                           |
| Élettartam              | Message.TTL                  | Az átalakítás Proton-Python TTL ezredmásodperc van megadva. |
| A                      | Message.address              | -                                                           |

## <a name="next-steps"></a>Következő lépések

Készen áll a további információt? Látogasson el az alábbi hivatkozások:

- [Szolgáltatás Bus AMQP – áttekintés]
- [A Windows Server szolgáltatás Bus AMQP]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[A Windows Server szolgáltatás Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx

[Szolgáltatás Bus AMQP – áttekintés]: service-bus-amqp-overview.md
