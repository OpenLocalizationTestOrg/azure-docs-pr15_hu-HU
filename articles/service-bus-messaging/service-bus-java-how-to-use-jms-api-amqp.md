<properties 
    pageTitle="AMQP 1.0 használatáról a Java szolgáltatás Bus API-val |} Microsoft Azure" 
    description="Hogyan lehet Azure Service Bus és speciális üzenet Queuing Protodol (AMQP) 1.0 Java üzenet szolgáltatás (JMS) használata." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>A Java üzenet szolgáltatás (JMS) API használata szolgáltatás Bus és AMQP 1.0

A speciális üzenet Queuing Protocol (AMQP) 1.0 nem hatékony, megbízható, vezetékes szintű üzenetben protokoll robusztus, platformok üzenetküldési alkalmazások készítéséhez használható.

Támogatás a szolgáltatás Bus AMQP 1,0 azt jelenti, hogy, hogy a queuing használhatja és közzététel/előfizetés brokered üzenetküldési szolgáltatások egy cellatartományból platformokon egy hatékony bináris protokoll használatával. Ezenkívül készíthet alkalmazások verzióban lévő összetevők beépített kombinálja a keretek, operációs rendszeren és nyelvek használata.

Ez a cikk ismerteti, hogyan szolgáltatás Bus üzenetküldési szolgáltatások (sorban várakozó és közzététel/előfizetés témakörök) használata a népszerű Java üzenet szolgáltatás (JMS) szabványos API segítségével Java-alkalmazásokból. A [kereső a cikk](service-bus-dotnet-advanced-message-queuing.md) bemutatja, hogyan tegye ugyanezt a szolgáltatás Bus .NET API van. Két útmutatókban együtt használható platformok közötti AMQP 1.0 használatával kapcsolatos.

## <a name="get-started-with-service-bus"></a>Első lépések a szolgáltatás Bus

Ez az útmutató feltételezi, hogy már van egy **1**nevű várólista tartalmazó szolgáltatás Bus névtér. Ha nem, majd is [hozza létre a névtér és várólista](service-bus-create-namespace-portal.md) az [Azure-portálon](https://portal.azure.com). Szolgáltatás Bus névtér és a sorok létrehozásával kapcsolatos további tudnivalókért lásd: a [szolgáltatás Bus sorban várakozó használatáról](service-bus-dotnet-get-started-with-queues.md).

> [AZURE.NOTE] Particionált sorban várakozó és témaköröket is támogatja a AMQP. További információt a [Partitioned üzenetküldés szervezetek](service-bus-partitioning.md) , és [szolgáltatás Bus AMQP 1.0 támogatása a sorok és a témakörök partícionálni](service-bus-partitioned-queues-and-topics-amqp-overview.md)témakörben talál.

## <a name="downloading-the-amqp-10-jms-client-library"></a>A AMQP 1.0 JMS ügyfél tár letöltését

Honnan töltheti le a Apache Qpid JMS AMQP 1.0 ügyfél tárat a legújabb információkért keresse fel a [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Hozzá kell adnia az alábbi négy üveg fájlokat a Apache Qpid JMS AMQP 1.0 terjesztési archívumból a Java CLASSPATH összeállítását, ha a JMS alkalmazások fut a szolgáltatás Bus:

- geronimo-jms\_1.1\_specifikáció-1.0.jar
- qpid-amqp-1-0-Client-[Version].JAR
- qpid-amqp-1-0-Client-jms-[Version].JAR
- qpid-amqp-1-0-Common-[Version].JAR

## <a name="coding-java-applications"></a>Coding Java-alkalmazások

### <a name="java-naming-and-directory-interface-jndi"></a>Java elnevezéséről és a címtár-felület (JNDI)

JMS használja a Java elnevezéséről és a címtár-felület (JNDI) hozzon létre egy logikai és fizikai nevének elkülönítése. Kétféle JMS objektumok JNDI szünteti meg: ConnectionFactory és a cél. JNDI használja a szolgáltató modell, amelybe csatlakoztathatja különböző címtárszolgáltatásaival neve felbontás feladatok kezelése. A Apache Qpid JMS AMQP 1.0 tár megtalálható egy egyszerű tulajdonságok fájlalapú JNDI szolgáltató van konfigurálva, a következő tulajdonságok fájl használatával formázása:

```
# servicebus.properties - sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>A ConnectionFactory konfigurálása

A szöveg egy **ConnectionFactory** Qpid tulajdonságok fájl JNDI szolgáltató használta áll, a következő formátumban:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Hol **[jndi_name]** és **[ConnectionURL]** jelentése a következő:

- **[jndi_name]**: a ConnectionFactory logikai nevét. Ez az a név, a JNDI IntialContext.lookup() módszerrel Java-alkalmazások javított.
- **[ConnectionURL]**: a JMS tár AMQP közvetítő szükséges információt nyújt A URL-CÍMÉT.

A **ConnectionURL** formátuma a következőképpen:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Hol **[névtér]**, **[SASPolicyName]** és **[SASPolicyKey]** jelentése a következő:

- **[névtér]**: A szolgáltatás Bus névtér.
- **[SASPolicyName]**: A várólista megosztott Access aláírás házirend neve.
- **[SASPolicyKey]**: A várólista megosztott Access aláírás házirend billentyűt.

> [AZURE.NOTE] URL-kódolás a jelszó manuálisan kell. Hasznos URL-kódolás segédprogram [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)címen érhető el.

#### <a name="configure-destinations"></a>Célok konfigurálása

A bejegyzés célhelyének meghatározása a Qpid tulajdonságok fájl JNDI szolgáltató használt áll, a következő formátumban:

```
queue.[jndi_name] = [physical_name]
```

vagy

```
topic.[jndi_name] = [physical_name]
```

Ha **[jndi\_neve]** és **[fizikai\_neve]** jelentése a következő:

- **[jndi_name]**: a cél logikai nevét. Ez az a név, a JNDI IntialContext.lookup() módszerrel Java-alkalmazások javított.
- **[physical_name]**: a szolgáltatás Bus entitás, amelyhez az alkalmazás küld vagy fogad üzeneteket nevét.

> [AZURE.NOTE] Szolgáltatás Bus témakör előfizetésből fogadásakor a fizikai JNDI megadott kell lennie a témakör a nevét. Az előfizetés nevét kapja a JMS alkalmazás kódot a tartós előfizetés létrehozásakor. A [szolgáltatás Bus AMQP 1.0 fejlesztői útmutató](service-bus-amqp-dotnet.md) szolgáltatás Bus témakör előfizetések a JMS használata részletesen ismerteti.

### <a name="write-the-jms-application"></a>Írja be a JMS alkalmazás

Nincsenek speciális API-hoz vagy a szolgáltatás Bus JMS használatakor szükséges beállításokat. Vannak azonban olyan néhány korlátozás, később szereplő. Mivel minden JMS alkalmazással az első thing szükséges beállítás a JNDI környezet engedélyezni szeretné a **ConnectionFactory** és célok megoldása.

#### <a name="configure-the-jndi-initialcontext"></a>A JNDI InitialContext konfigurálása

A JNDI környezet konfigurációja, a javax.naming.InitialContext osztály konstruktorának egy hashtable konfigurációs adatok megkerülhetők. A két szükséges a hashtable elemeinek osztály nevét, valamint a kezdeti környezetben gyári szolgáltató URL-CÍMÉT. A következő kód jeleníti meg, hogyan kell használni a Qpid tulajdonságok fájlt a JNDI környezet beállításához JNDI szolgáltató alapú **servicebus.properties**nevű tulajdonságok-fájl segítségével.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Egy egyszerű JMS alkalmazást használ szolgáltatási Bus várólista

A következő példa program JMS TextMessages küld nevű JNDI logikai várólista szolgáltatást Bus várólista, és vissza az üzenetet kap.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Futtassa az alkalmazást

Az űrlap kimeneti az alkalmazást futtató hoz létre:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Platformok közötti JMS és .NET között

Ez az útmutató JMS használatával hogyan küldhet és fogadhat üzeneteket, és a szolgáltatás Bus mutatott. Azonban egyik AMQP 1.0 legfontosabb előnye, hogy engedélyezi-e alkalmazások létrehozása az összetevők, a megbízható és teljes funkcióvesztést üzenetváltások más nyelven írt.

A fenti minta JMS alkalmazás és a kereső útmutató, [a .NET szolgáltatás Bus .NET API-val AMQP 1.0 használatáról](service-bus-dotnet-advanced-message-queuing.md), vett hasonló .NET kérelem közötti .NET és Java cserél. 

Platformok közötti szolgáltatás Bus és AMQP 1.0 részleteit kapcsolatos további tudnivalókért olvassa el a a [szolgáltatás Bus AMQP 1.0 fejlesztői útmutató](service-bus-amqp-dotnet.md)című témakört.

### <a name="jms-to-net"></a>A .NET JMS

.NET üzenetküldés mutatni a JMS:

* Indítsa el a .NET minta alkalmazás parancssori argumentum nélkül.
* Indítsa el a "sendonly" parancssori argumentum Java minta alkalmazása. Ebben az üzemmódban az alkalmazás nem jelennek meg az üzenetet a sorból csak elküldi.
* **Enter** billentyű lenyomásával néhány alkalommal elindítja küldeni Java alkalmazás konzolban.
* Az alábbi üzenetek érkeznek a .NET-alkalmazást.

#### <a name="output-from-jms-application"></a>Kimeneti JMS alkalmazásból

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>.NET-alkalmazás kimenete

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET való JMS

.NET JMS üzenetküldés igazolnia:

* Indítsa el a "sendonly" parancssori argumentum a .NET minta alkalmazást. Ebben az üzemmódban az alkalmazás nem jelennek meg az üzenetet a sorból csak elküldi.
* Indítsa el a Java minta alkalmazás parancssori argumentum nélkül.
* **Enter** billentyű lenyomásával néhány alkalommal elindítja küldeni .NET alkalmazás konzolban.
* Az alábbi üzenetek Java alkalmazásával egyaránt megérkezik.

#### <a name="output-from-net-application"></a>.NET-alkalmazás kimenete

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Kimeneti JMS alkalmazásból

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nem támogatott funkciókat és korlátozások

A következő korlátozások érvényesek használatakor JMS fölé AMQP 1.0 a szolgáltatás Bus, azaz:

* Egyetlen **MessageProducer** vagy **MessageConsumer** minden **munkamenetben**engedélyezett. Ha több **MessageProducers** vagy **MessageConsumers** létrehozása-alkalmazásban kell hozzon létre egy dedikált **munkamenet** mindegyik őket.
* Környezetfüggő témakör előfizetések jelenleg nem támogatott.
* **MessageSelectors** jelenleg nem támogatott.
* Ideiglenes célok, azaz **TemporaryQueue**, **TemporaryTopic** jelenleg nem támogatottak, a **QueueRequestor** és **TopicRequestor** azokat használó API-khoz.
* Feldolgozott munkamenetek és elosztott tranzakciók nem támogatottak.

## <a name="summary"></a>Összefoglalás

Ennek az útmutatóban a népszerű JMS API-val és a AMQP 1.0 brokered szolgáltatás Bus üzenetben lehetőségeinek haszná (sorban várakozó és közzététel/előfizetés témakörök) a Java mutatott.

Az egyéb nyelvek, többek között a .NET, C, Python vagy PHP szolgáltatás Bus AMQP 1.0 is használhatja. Összetevők beépített használja ezeket a különféle nyelvekhez is üzenetváltásra biztos, hogy, és a teljes funkcióvesztést használata a AMQP 1.0 szolgáltatás Bus támogatást. További tudnivalókért lásd: a [szolgáltatás Bus AMQP 1.0 fejlesztői útmutató](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Következő lépések

* [Azure Service Bus AMQP 1.0 támogatása](service-bus-amqp-overview.md)
* [AMQP 1.0 használatáról a szolgáltatás Bus .NET API-val](service-bus-dotnet-advanced-message-queuing.md)
* [Szolgáltatási Bus AMQP 1.0 fejlesztői útmutató](service-bus-amqp-dotnet.md)
* [Szolgáltatás Bus sorban várakozó használata](service-bus-dotnet-get-started-with-queues.md)
* [Java Developer Center](/develop/java/).



 
