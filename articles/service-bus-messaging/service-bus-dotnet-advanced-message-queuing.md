<properties 
    pageTitle="AMQP 1.0 használatáról a .NET-szolgáltatás Bus API-val |} Microsoft Azure" 
    description="Megtudhatja, hogy miként speciális üzenet Queuing Protodol (AMQP) 1.0 használja a Azure .NET szolgáltatás Bus API-val." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>AMQP 1.0 használatáról a szolgáltatás Bus .NET API-val

A speciális üzenet Queuing Protocol (AMQP) 1.0 nem használható össze robusztus, platformok, üzenetküldés alkalmazások hatékony, megbízható, vezetékes szintű üzenetben protokoll.

Támogatás a szolgáltatás Bus AMQP 1,0 azt jelenti, hogy, hogy a queuing használhatja és közzététel/előfizetés brokered üzenetküldési szolgáltatások egy cellatartományból platformokon egy hatékony bináris protokoll használatával. Ezenkívül készíthet alkalmazások verzióban lévő összetevők beépített kombinálja a keretek, operációs rendszeren és nyelvek használata.

Ez a cikk ismerteti, hogyan az brokered szolgáltatás Bus üzenetküldési szolgáltatások használatához (sorban várakozó és közzététel/előfizetés témakörök) a szolgáltatás Bus .NET API .NET-alkalmazásokból. Megtudhatja, hogy miként azonos használja a szabványos Java üzenet szolgáltatás (JMS) API elvégzendő [companion cikk](service-bus-java-how-to-use-jms-api-amqp.md) van. Két útmutatókban együtt használható platformok közötti AMQP 1.0 használatával kapcsolatos.

## <a name="get-started-with-service-bus"></a>Első lépések a szolgáltatás Bus

Ez a cikk tartalma feltételezi, hogy már rendelkezik tartalmazó "1." nevű várólista szolgáltatást Bus névtér Ha nem, majd létrehozhat a névtér és az [Azure portál][]várólista. Szolgáltatás Bus névtér és a sorok létrehozásával kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus sorban várakozó – első lépések](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal)című témakört.

## <a name="download-the-service-bus-sdk"></a>A szolgáltatás Bus SDK letöltése

AMQP 1.0 támogatási szolgáltatás Bus SDK 2.1-es vagy újabb verziója érhető el. A legújabb bittel letölthető NuGet [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/)elemre.

## <a name="code-net-applications"></a>.NET-alkalmazások kódot.

Alapértelmezés szerint a szolgáltatás Bus .NET ügyfél tárat a szolgáltatás Bus szolgáltatás egy SOAP-alapú dedikált protokoll használatával kommunikál. AMQP 1.0 használata helyett az alapértelmezett protokollt explicit konfigurációjának szolgáltatás Bus kapcsolati karakterláncot, a következő szakaszban ismertetett módon szükséges. Ez a módosítás nem alkalmazás kódja alapvetően változatlan marad AMQP 1.0 használata esetén.

Az aktuális programcsomag létezik néhány API-szolgáltatások AMQP használata esetén nem támogatott. Nem támogatott funkciókról később jelennek meg a szakasz [nem támogatott funkciókat és korlátozásokat](#unsupported-features-and-restrictions). A speciális konfigurációs beállítások egy részét is kell egy másik AMQP használata esetén. Beállítások közül egyik sem a jelen cikkben használt, de további részleteket érhetők el a [szolgáltatás Bus AMQP áttekintése](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Via App.config konfigurálása

Javasolt használó alkalmazások App.config konfigurációs fájl beállítások tárolására. Szolgáltatás Bus alkalmazásokhoz App.config a szolgáltatás Bus **ConnectionString**tárolására is használhatja. Minta alkalmazás is App.config tárolására használja a szolgáltatás Bus üzenetküldés azt használó személy nevét.

Mintafájl App.config Itt jelennek meg:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>A szolgáltatás Bus kapcsolati karakterlánc konfigurálása

A **Microsoft.ServiceBus.ConnectionString** beállítás értéke a szolgáltatás Bus kapcsolati karakterlánc, amely állítsa be a szolgáltatás Bus kapcsolatot. Formátuma az alábbi képlettel történik:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Ha `[namespace]` és `[SAS key]` az [Azure portál][]kapott. További információt itt tájékozódhat [használja a szolgáltatás Bus sorban várakozó] [].

Amikor AMQP, a kapcsolati karakterlánc hozzáfűzi az `;TransportType=Amqp`, amely azt jelenti, hogy legyen a kapcsolata szolgáltatás Bus az ügyfél tár AMQP 1.0 használatával.

### <a name="configure-the-entity-name"></a>Állítsa be a személy nevét.

A minta alkalmazás használja a `EntityName` App.config fájl **appSettings** szakaszban konfigurálja a nevet, amellyel az alkalmazás üzenetváltásokat üzenetek várólista beállítást.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Egy egyszerű .NET alkalmazást használ szolgáltatási Bus várólista

A következő példa küldi és fogadja üzeneteket, és onnan egy szolgáltatás Bus várólista.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Futtassa az alkalmazást

Az űrlap kimeneti az alkalmazást futtató hoz létre:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Platformok közötti JMS és .NET között

Ez a témakör hogyan .NET használ szolgáltatási Bus üzeneteket küldeni, és szeretne kapni a azokat az üzeneteket .NET mutatott. Azonban egyik AMQP 1.0 legfontosabb előnye, hogy engedélyezi-e alkalmazások létrehozása az összetevők, a megbízható és teljes funkcióvesztést üzenetváltások más nyelven írt.

A fenti minta .NET-alkalmazást, és a kereső útmutató, [a Java üzenet szolgáltatás (JMS) a szolgáltatás Bus és AMQP 1.0 API használatáról](service-bus-java-how-to-use-jms-api-amqp.md), vett hasonló Java-alkalmazások ajánlatos üzenetváltásra .NET és Java között. 

További információt a platformok közötti szolgáltatás Bus és AMQP 1.0 részleteit a a [szolgáltatás Bus AMQP 1.0 – áttekintés](service-bus-amqp-overview.md)című témakörben találhat.

### <a name="jms-to-net"></a>A .NET JMS

.NET üzenetküldés mutatni a JMS:

* Indítsa el a .NET minta alkalmazás parancssori argumentum nélkül.
* Indítsa el a "sendonly" parancssori argumentum Java minta alkalmazása. Ebben az üzemmódban az alkalmazás nem jelennek meg az üzenetet a sorból csak elküldi.
* **Enter** billentyű lenyomásával néhány alkalommal elindítja küldeni Java alkalmazás konzolban.
* Az alábbi üzenetek érkeznek a .NET-alkalmazást.

### <a name="output-from-jms-application"></a>Kimeneti JMS alkalmazásból

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>.NET-alkalmazás kimenete

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET való JMS

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

A .NET Bus API-t, az alábbi funkciók jelenleg nem támogatottak AMQP használata esetén:

 * Tranzakciók
 * Átadás cél küldése

További tudnivalókért lásd: a [nem támogatott funkciókat, korlátozások, és viselkedési különbségek](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Összefoglalás

Ez a cikk AMQP 1.0 és a szolgáltatás Bus .NET API segítségével hogyan érhető el a szolgáltatás Bus brokered üzenetküldési szolgáltatások (sorban várakozó és közzététel/előfizetés témakörök) a .NET mutatott.

Az egyéb nyelvek, többek között a Java, C, Python vagy PHP szolgáltatás Bus AMQP 1.0 is használhatja. Beépített, használja a következő nyelveken összetevők üzenetváltásra képes megbízható és AMQP 1.0 használata szolgáltatás Bus teljes romlását eredményezhetik. További tudnivalókért lásd: a [szolgáltatás Bus AMQP áttekintése](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Következő lépések

Most, hogy az, hogy elolvasta szolgáltatás Bus és a .NET AMQP, lásd: további információt az alábbi hivatkozásokat.

* [Azure Service Bus AMQP 1.0 támogatása](service-bus-amqp-overview.md)
* [A Java üzenet szolgáltatás (JMS) API használatáról a szolgáltatás Bus és AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Szolgáltatás Bus sorban várakozó használata](service-bus-dotnet-get-started-with-queues.md)
 
[Azure portál]: https://portal.azure.com
