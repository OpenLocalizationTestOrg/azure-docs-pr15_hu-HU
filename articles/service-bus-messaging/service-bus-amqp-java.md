<properties 
    pageTitle="Service Bus és a AMQP 1.0 Java |} Microsoft Azure"
    description="Java a szolgáltatás Bus használata AMQP"
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

# <a name="use-service-bus-from-java-with-amqp-10"></a>Java a szolgáltatás Bus használata AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Java üzenet szolgáltatás (JMS) nem szabványos API-val Java-platformon üzenet készült köztes használata. Microsoft Azure Service Bus teszteltük az AMQP 1.0 a JMS ügyfél tár a Apache Qpid project által fejlesztett alapján. A tár támogatja a teljes JMS 1.1 API-t, és bármely AMQP 1.0 kompatibilis üzenetkezelési szolgáltatás kínál. Ebben az esetben is támogatja a [Windows Server szolgáltatás Bus](https://msdn.microsoft.com/library/dn282144.aspx) (helyszíni szolgáltatás Bus). További tudnivalókért lásd: [a Windows Server szolgáltatás Bus AMQP][].

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>Töltse le a Apache Qpid AMQP 1.0 JMS ügyfél-tárban

Letölti-e a Apache Qpid JMS AMQP 1.0 ügyfél tárat a legújabb információkért keresse fel [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html).

Hozzá kell adnia az alábbi négy üveg fájlokat a Apache Qpid JMS AMQP 1.0 terjesztési archívumból a Java CLASSPATH összeállítását, ha a JMS alkalmazások fut a szolgáltatás Bus:

-   geronimo-jms\_1.1\_.jar specifikáció-[verzió]

-   qpid-amqp-1-0-Client-[Version].JAR

-   qpid-amqp-1-0-Client-jms-[Version].JAR

-   qpid-amqp-1-0-Common-[Version].JAR

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Szolgáltatás Bus sorban várakozó, téma és az előfizetések JMS használata

### <a name="java-naming-and-directory-interface-jndi"></a>Java elnevezéséről és a címtár-felület (JNDI)

JMS hozhat létre egy logikai és fizikai nevének elkülönítése Java elnevezéséről és Directoryban felület (JNDI) használja. Kétféle JMS objektumok JNDI szünteti meg: **ConnectionFactory** és a **cél**. JNDI használja a szolgáltató modell, amelybe csatlakoztathatja különböző címtárszolgáltatásaival neve felbontás feladatok kezelése. Egy egyszerű tulajdonságok megtalálható a Apache Qpid JMS AMQP 1.0 tár fájlalapú JNDI szolgáltatót, amely szövegfájlba van beállítva.

A Qpid tulajdonságok fájl JNDI úgy van konfigurálva, a következő formátumban tulajdonságok fájl használatával:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>A kapcsolat gyári konfigurálása

A szöveg egy **ConnectionFactory** Qpid tulajdonságok fájl JNDI szolgáltató használta áll, a következő formátumban:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Ha `[jndi\_name]` és `[ConnectionURL]` jelentése a következő:

| név            | Jelentése                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | A kapcsolat gyári logikai neve. Ez a név Java alkalmazásban már megoldódott a JNDI használatával `IntialContext.lookup()` módot. |   |   |   |   |
| `[ConnectionURL]` | Egy URL-címet, amely a JMS tár AMQP közvetítő szükséges információt biztosít.                                                      |   |   |   |   |

A kapcsolat URL-cím formátuma az alábbi képlettel történik:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Ha `[namespace]`, `[username]`, és `[password]` jelentése a következő:

| név          | Jelentése                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | A szolgáltatás Bus névtér az [Azure portál][]nyert.                      |   |   |   |   |
| `[username]`  | A szolgáltatás Bus Társítások kulcs névből az [Azure-portálon][].                    |   |   |   |   |
| `[password]`  | A szolgáltatás Bus Társítások billentyűt az [Azure portál][]kapott URL-címként kódolt formájában. |   |   |   |   |

> [AZURE.NOTE] URL-kódolás a jelszó manuálisan kell. Egy hasznos URL-cím kódolási segédprogram [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)címen érhető el.

Ha például kapott az adatokat a portálon esetén az alábbi képlettel történik:

| Namespace:   | Test.servicebus.Windows.NET                  |
|--------------|----------------------------------------------|
| Kiállító neve: | RootManageSharedAccessKey                                        |
| Kibocsátó kulcs:  | ABCDEFG |

Kattintson annak érdekében, hogy egy nevű **ConnectionFactory** objektum definiálása `SBCONNECTIONFACTORY`, a konfigurációs karakterlánc lenne, az alábbi képlettel történik:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>Célok konfigurálása

A bejegyzés, amely meghatározza a cél Qpid tulajdonságok fájl JNDI szolgáltató áll, a következő formátumban:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Ha `[jndi\_name]` és `[physical\_name]` jelentése a következő:

| név              | Jelentése                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | A cél logikai neve. Ez a JNDI használatával Java alkalmazásban már megoldódott a nevet `IntialContext.lookup()` módot. |
| `[physical\name]` | A szolgáltatás Bus entitás, amelyhez az alkalmazás küld vagy fogad üzeneteket neve.                                                  |

Vegye figyelembe az alábbiakat:

- A `[physical\name]` értéke lehet a szolgáltatás Bus várólista vagy témát.
- Szolgáltatás Bus témakör előfizetésből fogadásakor a fizikai JNDI megadott kell lennie a témakör a nevét. Az előfizetés nevét kapja a JMS alkalmazás kódot a tartós előfizetés létrehozásakor.
- Ajánlatos is JMS várólista szolgáltatást Bus témakör előfizetés tekinti. Számos előnnyel jár, ezt a megközelítést: ugyanazt kódot címzett sorban várakozó és a tematikus előfizetések használhatók, és az összes a cím mezőt (témakör és az előfizetés nevét) van externalized az a tulajdonságokat fájlt.
- Szolgáltatás Bus témakör előfizetés tekinti JMS várólista, a bejegyzés tulajdonságok fájlban kell lennie az űrlap: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |}

Határozza meg a "Tematikus", "Téma1" nevű szolgáltatás Bus témakörben leképezi nevű logikai JMS cél, az a tulajdonságokat fájlt a bejegyzés lenne az alábbi képlettel történik:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>JMS az üzenetek küldése

A következő kódot üzenetet küldeni a szolgáltatás Bus tematikus mutatja. Feltételezzük, hogy `SBCONNECTIONFACTORY` és `TOPIC` **servicebus.properties** konfigurációs fájl határozzák meg az előző szakaszban leírtak szerint.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Az üzenetek JMS fogadása

A következő kód megjelenítése `how` kattintva megjelenik egy üzenet szolgáltatás Bus témakör előfizetésről. Feltételezzük, hogy `SBCONNECTIONFACTORY` és a tematikus **servicebus.properties** konfigurációs fájl az előző szakaszban leírtak szerint lesz definiálva. Is feltételezi, hogy van-e az előfizetés neve `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Robusztus alkalmazások létrehozásába szabályai

A JMS specifikációja határozza meg, hogyan az API módszerek és az alkalmazás kód kivétel szerződés kezelheti az ilyen kivételek kell írni. Íme néhány további tudnivaló vonatkozó kezelésének módját:

-   Egy **ExceptionListener** regisztrálhatja a JMS kapcsolat **connection.setExceptionListener**. Ez lehetővé teszi, ha értesítést szeretne kapni a probléma aszinkron ügyfél. Ez az értesítés különösen fontos, hogy az üzenetek csak felhasználása kapcsolatokhoz, akkor nincs mód megtudhatja, hogy azok nem sikerült. Ha a probléma az alapul szolgáló AMQP kapcsolat, munkamenet vagy hivatkozás a **ExceptionListener** neve. Ebben az esetben az alkalmazás kell hozza létre újból a **JMS kapcsolat**, a **munkamenet**, a **MessageProducer** és a nulláról **MessageConsumer** objektumok.

-   Ellenőrizze, hogy egy üzenetet sikeresen küldte a **MessageProducer** egy szolgáltatás Bus személyhez, győződjön meg arról, hogy az alkalmazás van beállítva a **qpid.sync\_közzététele** rendszer tulajdonsága be van jelölve. Ezt megteheti a program a kezdheti is a **-Dqpid.sync\_közzététele = true** Java virtuális beállítás a parancssorban az alkalmazás indításakor. Ennek a beállításnak beállítja a nem a Küldés hívás visszaadására mindaddig, amíg a megerősítést kérő megérkeztek, hogy az üzenet szolgáltatás Bus már elfogadta a tárat. Probléma esetén a Küldés művelet során a **JMSException** merül fel. Ennek két oka lehet: 
    1. Ha a probléma oka az, hogy az adott üzenet elküldése elvetésével szolgáltatás Bus, egy **MessageRejectedException** kivétel kell emelni. Ez a hiba akkor átmeneti, vagy az üzenettel kapcsolatban valamilyen problémát miatt. A javasolt tanfolyam művelet, hogy több megkísérli ismételje meg az egyes vissza kikapcsolása logika a műveletet. Ha a probléma nem szűnik meg, majd az üzenet meg kell szüntetni, helyben bejelentkezett hibát. Nincs szükség az ebben az esetben a **JMS kapcsolaton**, **munkamenet**vagy **MessageProducer** objektumok újra. 
    2. Ha a probléma oka az, hogy a AMQP hivatkozás záró szolgáltatás Bus, **InvalidDestinationException** kivételt kell emelni. Ez egy ideiglenes (tranziens) probléma miatt és miatt az üzenet entitás törléséről is lehet. Bármelyik lehetőséget választja létre kell hozni az **JMS kapcsolat**, **munkamenet**és **MessageProducer** objektumokat. Ha a hiba okát volt ideiglenes (tranziens), majd a művelet ahányat sikeres lesz. Ha a szervezet törli, a hibát végleges lesz.

## <a name="messaging-between-net-and-jms"></a>.NET és JMS közötti Csevegés

### <a name="message-bodies"></a>Üzenettörzs

JMS definiálja ötféle másik üzenetet: **BytesMessage**, **MapMessage**, **ObjectMessage**, **StreamMessage**és **TextMessage**. A szolgáltatás Bus .NET API-t egy adott üzenet típust, [BrokeredMessage][]tartalmaz.

#### <a name="jms-to-service-bus-net-api"></a>A szolgáltatás Bus .NET API JMS

Az alábbi szakaszok bemutatják, hogyan üzeneteket a .NET JMS üzenet különböző használhatnak. Példa **ObjectMessage** nem szerepel, mint egy **ObjectMessage** törzsében lévő Java programnyelv megadása, amely nem egy .NET alkalmazás interpretable szerializálható objektum.

##### <a name="bytesmessage"></a>BytesMessage

A következő kód a szervezet egy **BytesMessage** objektum felhasználni a szolgáltatás Bus .NET API-k használatával mutatja be.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

A következő kód a szervezet egy **MapMessage** objektum felhasználni a szolgáltatás Bus .NET API-k használatával mutatja be. Ez a kód függvény a térkép megjelenítése a neve és a minden elem értékének elemei között.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

A következő kód a szervezet egy **StreamMessage** objektum felhasználni a szolgáltatás Bus .NET API-k használatával mutatja be. Ez a kód felsorolja az egyes az adatfolyam együtt azok típusát elemet.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

A következő kód a szervezet egy **TextMessage** objektum felhasználni a szolgáltatás Bus .NET API-k használatával mutatja be. Ez a kód jeleníti meg, hogy az üzenet törzsében lévő szöveges karakterlánc.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Szolgáltatás Bus .NET API-hoz való JMS

Az alábbi szakaszok bemutatják, hogyan hozhat létre egy .NET alkalmazást egy üzenet, amely a különböző JMS üzenet különböző JMS érkezik. Példa **ObjectMessage** nem szerepel, mint egy **ObjectMessage** törzsében lévő Java programnyelv megadása, amely nem egy .NET alkalmazás interpretable szerializálható objektum.

##### <a name="bytesmessage"></a>BytesMessage

A következő kódot [BrokeredMessage][] -objektum létrehozása a .NET, mint egy **BytesMessage**JMS ügyfél által mentsen mutatja.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

A következő kódot [BrokeredMessage][] -objektum létrehozása a .NET, mint egy **StreamMessage**JMS ügyfél által mentsen mutatja.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

A következő kódot mutatja egy **TextMessage** törzsében felhasználni a szolgáltatás Bus .NET API. Ez a kód jeleníti meg, hogy az üzenet törzsében lévő szöveges karakterlánc.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Szolgáltatásalkalmazás tulajdonságainak

####<a name="jms-to-service-bus-net-apis"></a>JMS karbantartására Bus .NET API-hoz

JMS üzenetek támogatja a következő típusú alkalmazás tulajdonságai: **logikai**, **bájt**, **rövid**, **int**, **hosszú**, **lebegtetés**, **dupla**vagy **karakterláncot**. A következő Java-kód tulajdonságainak beállítása egy üzeneten mindegyik tulajdonság segítségével mutatja be.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

A szolgáltatás Bus .NET API-k, az üzenet szolgáltatásalkalmazás tulajdonságainak végzik a [BrokeredMessage][] **Properties** gyűjteményéhez. A következő kódot olvassa el az alkalmazás tulajdonságai JMS ügyfél kapott üzenet mutatja.

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

A következő táblázat mutatja, hogy hogyan JMS tulajdonság fájltípusok feleltesse meg a tulajdonság .NET típusokat.

| JMS tulajdonság típusa | .NET-tulajdonság típusa |
|-------------------|--------------------|
| Bájt              | sbyte              |
| Egész szám           | int                |
| Lebegtetés             | Lebegtetés              |
| Dupla            | dupla             |
| Logikai érték           | logikai               |
| Karakterlánc            | karakterlánc             |

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

A következő Java-kód olvassa el a szolgáltatás Bus .NET ügyféltől kapott üzenet alkalmazás tulajdonságainak mutatja.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

A következő táblázat mutatja, hogy hogyan feleltesse meg a tulajdonság .NET típusokat JMS tulajdonság fájltípusok.

| .NET-tulajdonság típusa | JMS tulajdonság típusa | Jegyzetek                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bájt               | UnsignedByte      | -                                                                                                                                                                      |
| sbyte              | Bájt              | -                                                                                                                                                                     |
| karakter               | Karakter         | -                                                                                                                                                                     |
| rövid              | Rövid             | -                                                                                                                                                                     |
| ushort             | UnsignedShort     | -                                                                                                                                                                     |
| int                | Egész szám           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| hosszú               | Hosszú              | -                                                                                                                                                                     |
| ulong              | UnsignedLong      | -                                                                                                                                                                     |
| Lebegtetés              | Lebegtetés             | -                                                                                                                                                                     |
| dupla             | Dupla            | -                                                                                                                                                                     |
| decimális            | BigDecimal        | -                                                                                                                                                                     |
| logikai               | Logikai érték           | -                                                                                                                                                                     |
| Globálisan egyedi azonosítója               | UUID              | -                                                                                                                                                                     |
| karakterlánc             | Karakterlánc            | -                                                                                                                                                                     |
| Dátum és idő           | Dátum              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | AMQP típusú megfeleltetve DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Időszak           | DescribedType     | AMQP típusú megfeleltetve Timespan.Ticks:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType     | AMQP típusú megfeleltetve Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>A fejlécek szabványos

A következő táblázatok hogyan a JMS szabványos fejlécek és a [BrokeredMessage][] általános tulajdonságok vannak megfeleltetve AMQP 1.0 használatával.

#### <a name="jms-to-service-bus-net-apis"></a>JMS karbantartására Bus .NET API-hoz

| JMS              | Bus .NET szolgáltatás               | Jegyzetek                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Jelenleg nem érhető el        | Csak a szolgáltatási Bus támogatja tartós üzenetek; Ha például DeliveryMode.PERSISTENT, függetlenül attól, mi van megadva.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Az üzenet. Élettartam            | Átalakítás                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | Alapértelmezés szerint JMSMessageID címként van kódolva, a AMQP üzenetben bináris formában. A bájtok unicode értékein alapuló karakteres bináris üzenetazonosító átvételekor, a .NET-ügyfél tár konvertál. A karakterlánc üzenet lehetővé tevő dokumentumazonosítók JMS tár váltáshoz hozzáfűzése a "bináris-messageid = false" karakterláncot, a lekérdezés paraméterei a JNDI ConnectionURL. Példa: “amqps://[username]:[password]@[namespace].servicebus.windows.net? binárisra-messageid = false ". |
| JMSPriority      | Jelenleg nem érhető el        | Szolgáltatás Bus nem támogatja az üzenetek prioritását.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Jelenleg nem érhető el        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Az üzenet. ReplyTo               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Átalakítás                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms-típus"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Szolgáltatás Bus .NET API-hoz való JMS

| Bus .NET szolgáltatás        | JMS              | Jegyzetek                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | Jelenleg nem érhető el |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Átalakítás              |
| Címke                   | a #hiányzik              | Jelenleg nem érhető el |
| MessageId               | JMSMessageID     | -                         |
| ReplyTo                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | a #hiányzik              | Jelenleg nem érhető el |
| ScheduledEnqueueTimeUtc | a #hiányzik              | Jelenleg nem érhető el |
| Munkamenet-azonosító               | a #hiányzik              | Jelenleg nem érhető el |
| Élettartam              | JMSExpiration    | Átalakítás              |
| A                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Nem támogatott funkciókat és korlátozások

A következő korlátozások érvényesek a szolgáltatás Bus AMQP 1.0 fölé JMS használatakor:

-   Egyetlen **MessageProducer** vagy **MessageConsumer** minden munkamenetben engedélyezett. Ha több **MessageProducer** vagy **MessageConsumer** objektum létrehozása az alkalmazásokban, mindegyiket külön munkamenetek létrehozása

-   Környezetfüggő témakör előfizetések jelenleg nem támogatott.

-   **MessageSelector** objektumok nem támogatottak.

-   Ideiglenes célok; Ha például **TemporaryQueue** vagy **TemporaryTopic**, nem támogatottak, a **QueueRequestor** és **TopicRequestor** API-khoz azokat használó.

-   Feldolgozott munkamenetek nem támogatottak.

-   Az elosztott tranzakciók nem támogatottak.

## <a name="next-steps"></a>Következő lépések

Készen áll a további információt? Látogasson el az alábbi hivatkozások:

- [Szolgáltatás Bus AMQP – áttekintés]
- [A Windows Server szolgáltatás Bus AMQP]

[A Windows Server szolgáltatás Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Szolgáltatás Bus AMQP – áttekintés]: service-bus-amqp-overview.md
[Azure portál]: https://portal.azure.com
