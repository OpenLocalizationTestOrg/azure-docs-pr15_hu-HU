<properties
    pageTitle="Java szolgáltatás Bus sorban várakozó használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként használja a szolgáltatás Bus sorban várakozó Azure-ban. Java nyelven írt mintakódok."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Szolgáltatás Bus sorban várakozó használata

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez a cikk ismerteti, hogyan szolgáltatás Bus sorban várakozó használni. A minták Java írt, és a [Azure SDK Java][]használja. Az érintett esetek **sorban várakozó létrehozása**, **üzenetek küldése és fogadása**, és **Sorok törlése**.

## <a name="what-are-service-bus-queues"></a>Mik azok a szolgáltatás Bus sorban várakozó?

Szolgáltatás Bus sorban várakozó **brokered üzenetküldés** kommunikációs modell támogatja. Sorok használatakor elosztott alkalmazás részei nem egymással közvetlenül; Ehelyett azok üzenetváltásra várólista, amely végpontként közvetítő (ügynök) keresztül. Egy üzenet gyártó (Feladó) kezek ki az üzenetet a várakozási sorban található, és majd továbbra is a feldolgozása.
Aszinkron egy üzenet fogyasztói (címzett) gyűjti össze a sorból az üzenetet, és dolgozza fel. A gyártó nem kell a válaszra várakozás az ügyféltől annak érdekében, hogy továbbra is folyamat, és további küldésére. Sorok üzenetkézbesítés **első, első ki (FIFO)** egy vagy több versengő felhasználóknak kínálnak. Ez azt jelenti, hogy az üzenetek általában kapott és az abban a sorrendben, amelyben a sor hozzáadásuk, és minden üzenet kapott, és csak egy üzenet fogyasztói által feldolgozott vevők által feldolgozott.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Szolgáltatás Bus sorban várakozó olyan általános célú technológia esetek széles választéka használható:

- A több szálon Azure alkalmazás webes és dolgozó szerepkörök közötti kommunikáció.
- A helyszíni alkalmazások és az Azure közötti kommunikációt lévő hibrid megoldást is.
- Más szervezetek vagy a részlegek egy szervezet helyszíni futtató elosztott alkalmazás összetevői közötti kommunikációt.

Sorok használatával lehetővé teszi az alkalmazások könnyebben méretezése és a architektúrában tűrőképessége engedélyezése.

## <a name="create-a-service-namespace"></a>Szolgáltatás névtér létrehozása

Azure Service Bus sorban várakozó használatának megkezdéséhez, először létre kell hoznia egy névtér. Névtér tartalmazó tároló biztosít megcímezheti szolgáltatás Bus erőforrások az alkalmazáson belül.

Névtér létrehozása:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatához szolgáltatás Bus konfigurálása

Győződjön meg arról, hogy telepítve van az [Azure SDK Java][] felépítése a következő példában előtt. Az [Azure eszközkészlete Holdas][] , amely tartalmazza a Java Azure SDK Holdas használata esetén is telepítheti. A **Microsoft Azure-tárak Java** majd felvenni a projektbe:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Adja hozzá a következő `import` tetején a Java-fájlt a kimutatások:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Várólista létrehozása

Szolgáltatás Bus sorban várakozó felügyeleti műveleteket végezheti el a **ServiceBusContract** osztály keresztül. Egy **ServiceBusContract** objektumra, amely a Társítások jogkivonat engedélyek kezelése, hogy a megfelelő konfigurációval összeállítás, és a **ServiceBusContract** osztály Azure való kommunikáció egyetlen pontot.

A **ServiceBusService** osztály bemutat hozhat létre, a számba veheti a webhely és a sorok törlése. Az alábbi példában látható, hogyan egy **ServiceBusService** objektum "TestQueue", "HowToSample" nevű névteret nevű várólista létrehozására használható:

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

A **QueueInfo** módszer, amelyek lehetővé teszik a várólista kell állítani a Tulajdonságok (például: az alapértelmezett time-to-live (TTL) értéket a várólista küldött üzenetek alkalmazandó beállítása). A következő példa bemutatja, hogyan hozhat létre egy névvel ellátott várólista `TestQueue` legfeljebb 5GB méretű:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Figyelje meg, hogy felhasználhatja a **listQueues** módszer **ServiceBusContract** objektumok ellenőrzéséhez, ha a megadott nevű már létezik várólista szolgáltatást névteret belül.

## <a name="send-messages-to-a-queue"></a>Üzenetek küldése egy sorba

Szolgáltatás Bus várólista üzenet elküldéséhez az alkalmazás szerez egy **ServiceBusContract** objektumot. A következő kódrészlet szemlélteti, hogyan lehet üzenet küldése az `TestQueue` a korábban létrehozott várólista a `HowToSample` névtér:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Üzeneteket küldeni, és kapott szolgáltatás Bus sorban várakozó [BrokeredMessage][] osztály példányai. Egyéni alkalmazások adott tulajdonságainak tartása használt szótár és tetszőleges alkalmazás adatok törzsében [BrokeredMessage][] objektumhoz tartozik-e egy sor olyan általános tulajdonságok (például [címke](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) és [az élettartam](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)). Az alkalmazás a [BrokeredMessage][]konstruktorának szerializálható objektumokat megkerülhetők állíthat be az üzenet törzsébe, és a megfelelő szerializáló majd használható szerializálni az objektumot. Másik lehetőségként a **java lehet nyújtani. IO. InputStream** objektumot.

Az alábbi példa bemutatja, hogyan öt vizsgált üzeneteket küldhet a `TestQueue` **MessageSender** azt az előző kódtöredék kapott:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Szolgáltatás Bus sorban várakozó támogatja a [Normál réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md). A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs várólista tartott üzenetek száma korlátozott, de van egy vonalvég az üzenetek várólista birtokában összesített méretét. A várólista mérete létrehozáskor, az 5 GB felső határa van megadva.

## <a name="receive-messages-from-a-queue"></a>Üzenetek fogadására várólista

Az elsődleges várólista érkező üzenetek fogadására módja a **ServiceBusContract** objektum használatára. A Beérkezett üzenetek is dolgozhat, két különböző módokban: **ReceiveAndDelete** és **PeekLock**.

A **ReceiveAndDelete** móddal fogadott egy olyan egyetlen-ikon művelet, – ez azt jelenti, hogy szolgáltatás Bus olvasási vonatkozóan kap megkeresést üzenet egy sorban, amikor azt felhasználás alatt álló minősíti az üzenetet, és visszatér azt az alkalmazás. **ReceiveAndDelete** mód (Ez az alapértelmezett mód) a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa.
Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

**PeekLock** üzemmódban fogadhat válik egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a másodszintű fogadási folyamat befejezése hívja fel a **törlése** a kapott üzenet a. Szolgáltatás Bus láthatja a **törlése** hívás, amikor azt fog megjelölni az üzenetet az éppen, és távolítsa el azt a sor.

Az alábbi példa bemutatja, hogyan üzenetek fogadható és feldolgozása **PeekLock** mód (nem az alapértelmezett mód). Az alábbi példában végtelen ciklust tartalmaz, és, hogy a "TestQueue" be a beérkező üzeneteket dolgoz fel:

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja a **unlockMessage** módszer (helyett a **deleteMessage** módszer) kapott üzenet a. Ennek hatására a szolgáltatás Bus zárolásának feloldása a várólista belül az üzenetet, és ismét kapott igénybe alkalmazásával ugyanazon vagy másik igénybe alkalmazás elérhetővé.

Is van zárolva van, a sor belül üzenet társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenetet a zárolási időkorlát lejár (pl., ha az alkalmazás összeomlik), akkor szolgáltatás Bus zárolásának feloldásához automatikusan az üzenetet, és azt elérhetővé kapott újra.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **deleteMessage** kérelem kibocsátása előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább után feldolgozása**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el az üzenetet, amely végig a kézbesítési kísérletek változatlan marad **getMessageId** módszerrel.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus sorban várakozó alapjait, további információt talál [sorban várakozó, témák, és előfizetések][] .

További tudnivalókért lásd: a [Java Developer Center](/develop/java/).


  [Java Azure SDK]: http://azure.microsoft.com/develop/java/
  [Azure eszközkészlete Holdas]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

