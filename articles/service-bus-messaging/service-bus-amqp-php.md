<properties 
    pageTitle="Service Bus és a AMQP 1.0 PHP |} Microsoft Azure"
    description="A PHP szolgáltatás Bus használata AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>A PHP szolgáltatás Bus használata AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-PHP egy PHP nyelvi kötést Proton-C; Ez azt jelenti mint egy csomagolópapír körül c szerepelni fog motor Proton-PHP alkalmazása

## <a name="downloading-the-proton-client-library"></a>A Proton ügyfél tár letöltését

[Http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)letöltheti Proton-C és a hozzá társított kötések (beleértve a PHP). A letöltés van kód-formájában. A kód készítéséhez kövesse a képernyőn megjelenő utasításokat a letöltött csomagban lévő.

> [AZURE.IMPORTANT] Az írás időben Proton-C az SSL-támogatás érhető el csak Linux operációs rendszerekhez. Azure Service Bus SSL használatához, mert Proton-C (és a nyelvi kötések) csak használható szolgáltatás Bus hozzáférhet Linux adott időben. Ahhoz, hogy Proton-C az SSL használatával a Windows munka is meghívhat, így jelölőnégyzet gyakran frissítéseit.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Szolgáltatás Bus sorban várakozó, téma és az előfizetések PHP használata

A következő kódot mutatja üzenetek küldése és fogadása a szolgáltatás Bus a szervezet üzenetküldés.

### <a name="sending-messages-using-proton-php"></a>Az üzenetek Proton-PHP küldése

A következő kódot mutatja üzenetet küldeni egy szolgáltatás Bus entitás üzenetküldés.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Az üzenetek Proton-PHP fogadása

A következő kódot mutatja hibaüzenetet küld egy szolgáltatás Bus entitás üzenetküldés.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>.NET és Proton-PHP közötti Csevegés

### <a name="application-properties"></a>Szolgáltatásalkalmazás tulajdonságainak

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP karbantartására Bus .NET API-hoz

Proton-PHP üzenetek támogatja a következő típusú alkalmazás tulajdonságai: **egész szám**, **dupla**, **logikai**, **karakterláncot**vagy **objektumot**. A következő PHP-kódot megtudhatja, hogy miként tulajdonságainak beállítása egy üzeneten mindegyik tulajdonság használatával.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

A szolgáltatás Bus .NET API-k, az üzenet szolgáltatásalkalmazás tulajdonságainak végzik a [BrokeredMessage][] **Properties** gyűjteményéhez. A következő kódot olvassa el az alkalmazás tulajdonságai PHP ügyfél kapott üzenet mutatja.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

Az alábbi táblázatban felsorolt PHP tulajdonság a .NET tulajdonság típusok rendeli hozzá.

| PHP tulajdonság típusa | .NET-tulajdonság típusa |
|-------------------|--------------------|
| egész szám           | int                |
| dupla            | dupla             |
| logikai érték           | logikai               |
| karakterlánc            | karakterlánc             |
| objektum            | Objektum             |

#### <a name="service-bus-net-apis-to-php"></a>Szolgáltatás Bus .NET API-hoz való PHP

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
```

A következő PHP-kódot olvassa el a szolgáltatás Bus .NET ügyféltől kapott üzenet alkalmazás tulajdonságainak mutatja.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

Az alábbi táblázat a .NET tulajdonság típusok PHP tulajdonság fájltípusok rendeli hozzá.

| .NET-tulajdonság típusa | PHP tulajdonság típusa | Jegyzetek                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bájt               | egész szám           | -                                                                                                                                                                     |
| sbyte              | egész szám           | -                                                                                                                                                                     |
| karakter               | Karakter              | Proton-PHP osztály                                                                                                                                                    |
| rövid              | egész szám           | -                                                                                                                                                                     |
| ushort             | egész szám           | -                                                                                                                                                                     |
| int                | egész szám           | -                                                                                                                                                                     |
| uint               | Egész szám           | -                                                                                                                                                                     |
| hosszú               | egész szám           | -                                                                                                                                                                     |
| ulong              | egész szám           | -                                                                                                                                                                     |
| Lebegtetés              | dupla            | -                                                                                                                                                                     |
| dupla             | dupla            | -                                                                                                                                                                     |
| decimális            | karakterlánc            | Decimális Proton nem támogatott.                                                                                                                     |
| logikai               | logikai érték           | -                                                                                                                                                                     |
| Globálisan egyedi azonosítója               | UUID              | Proton-PHP osztály                                                                                                                                                    |
| karakterlánc             | karakterlánc            | -                                                                                                                                                                     |
| Dátum és idő           | egész szám           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | AMQP típusú megfeleltetve DateTimeOffset.UtcTicks:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Időszak           | DescribedType     | AMQP típusú megfeleltetve Timespan.Ticks:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | AMQP típusú megfeleltetve Uri.AbsoluteUri:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Általános tulajdonságok:

Az alábbi táblázatokban megjelenítése a hozzárendelés a Proton-PHP szabványos üzenet és az [BrokeredMessage][] szabványos üzenetet tulajdonságok között.

| Proton-PHP           | Bus .NET szolgáltatás         | Jegyzetek                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Tartós              | a #hiányzik                      | Szolgáltatás Bus csak tartós üzenetek támogatja.          |
| Prioritás             | a #hiányzik                      | Szolgáltatás Bus csak egy adott üzenet prioritás támogatja. |
| TTL (élettartam)                  | Message.TimeToLive       | Az átalakítás Proton-PHP TTL ezredmásodperc van megadva.   |
| első\_beszerző      | -                          | -                                                          |
| kézbesítési\_száma      | -                          | -                                                          |
| Azonosító                   | Message.Id               | -                                                          |
| felhasználói\_azonosító             | -                          | -                                                          |
| Cím              | Message.To               | -                                                          |
| Tárgy              | Message.Label            | -                                                          |
| válasz\_való            | Message.ReplyTo          | -                                                          |
| korrelációs\_azonosító      | Message.CorrelationId    | -                                                          |
| tartalom\_típusa        | Message.ContentType      | -                                                          |
| tartalom\_kódolást    | a #hiányzik                      | -                                                          |
| lejárat\_idő         | Message.ExpiresAtUTC     | -                                                          |
| létrehozás\_idő       | a #hiányzik                      | -                                                          |
| csoport\_azonosító            | Message.SessionId        | -                                                          |
| csoport\_sorrend      | -                          | -                                                          |
| válasz\_való\_csoport\_azonosító | Message.ReplyToSessionId | -                                                          |
| Formátum               | a #hiányzik                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Szolgáltatás Bus .NET API-hoz való Proton-PHP

| Bus .NET szolgáltatás        | Proton-PHP                                             | Jegyzetek                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Üzenet -\>tartalom\_típusa                                | -                                                        |
| CorrelationId           | Üzenet -\>korrelációs\_azonosító                              | -                                                        |
| EnqueuedTimeUtc         | Üzenet -\>széljegyzetek [x-kiválaszthatják-sorbaállítva-idő]             | -                                                        |
| Címke                   | Üzenet -\>tárgy                                      | -                                                        |
| MessageId               | Üzenet -\>azonosító                                           | -                                                        |
| ReplyTo                 | Üzenet -\>válasz\_való                                    | -                                                        |
| ReplyToSessionId        | Üzenet -\>válasz\_való\_csoport\_azonosító                         | -                                                        |
| ScheduledEnqueueTimeUtc | Üzenet -\>széljegyzetek ["x-kiválaszthatják-ütemezett-várólistára helyezés-time"] | -                                                        |
| Munkamenet-azonosító               | Üzenet -\>csoport\_azonosító                                    | -                                                        |
| Élettartam              | Üzenet -\>TTL (élettartam)                                          | Az átalakítás Proton-PHP TTL ezredmásodperc van megadva. |
| A                      | Üzenet -\>cím                                      | -                                                        |

## <a name="next-steps"></a>Következő lépések

Készen áll a további információt? Látogasson el az alábbi hivatkozások:

- [Szolgáltatás Bus AMQP – áttekintés]
- [A Windows Server szolgáltatás Bus AMQP]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[A Windows Server szolgáltatás Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
[Szolgáltatás Bus AMQP – áttekintés]: service-bus-amqp-overview.md
