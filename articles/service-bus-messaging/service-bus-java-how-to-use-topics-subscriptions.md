<properties
    pageTitle="Java szolgáltatás Bus témakörök használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként használja a szolgáltatás Bus témakörök és előfizetések Azure-ban. Mintakódok Java-alkalmazásokhoz kerülnek."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Szolgáltatás Bus témakörök és előfizetések használatával

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez az útmutató szolgáltatás Bus témakörök és előfizetések használatát ismerteti. A minták Java írt, és a [Azure SDK Java][]használja. Érintett esetek **létrehozása, témák és az előfizetéseket**, **előfizetés szűrők létrehozása**, **üzenetküldés témakörben**, **előfizetésből üzenetek fogadására**és **témakörök és az előfizetés törlése**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Mik azok a szolgáltatás Bus témakörök és előfizetések?

Szolgáltatás Bus témakörök és előfizetések támogatja a *Közzététel/előfizetés* üzenetküldés kommunikációs modell. Amikor témák és az előfizetéseket, elosztott alkalmazás részei nem egymással közvetlenül; Ehelyett azok üzenetváltásra közvetítő működik-e témakör keresztül.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

– A szolgáltatás Bus sorok, amelyben minden üzenet dolgozza fel egy egyetlen fogyasztói témák és előfizetések kommunikációs, mintázattal közzététel/előfizetés "egy-a-többhöz" űrlap biztosítanak. Több előfizetés-témakörben regisztrálhatja lehetőség. Üzenet elküldésekor témakörben, majd az egyes előfizetések számára hozzáférhető fogópont/folyamat egymástól függetlenül.

Előfizetés-témakörben hasonló, akkor egy virtuális várólista kapja a témakör küldött üzenetek másolatát. Másik lehetőségként regisztrálhatja a szűrő, amely lehetővé teszi a szűrő/korlátozása mely üzenetek témakörben érkeznek témakör előfizetésekről előfizetés alapon témakör szabályok.

Szolgáltatás Bus témakörök és előfizetések engedélyezése, ha át kívánja méretezni a nagyon nagy mennyiségű üzenetet feldolgozása nagyon sok felhasználók és az alkalmazások között.

## <a name="create-a-service-namespace"></a>Szolgáltatás névtér létrehozása

Azure szolgáltatás Bus témakörök és előfizetés használatának megkezdéséhez, először létre kell hoznia egy szolgáltatás névtere. Névtér tartalmazó tároló biztosít megcímezheti szolgáltatás Bus erőforrások az alkalmazáson belül.

Névtér létrehozása:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatához szolgáltatás Bus konfigurálása

Győződjön meg arról, hogy telepítve van az [Azure SDK Java][] felépítése a következő példában előtt. Az [Azure eszközkészlete Holdas][] , amely tartalmazza a Java Azure SDK Holdas használata esetén is telepítheti. A **Microsoft Azure-tárak Java** majd felvenni a projektbe:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Adja hozzá a következő importálása kimutatások felső részén a Java-fájl:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Az Azure-tárak Java hozzáadása az összeállítás elérési utat, és adja hozzá a projekt telepítési összeállítás.

## <a name="create-a-topic"></a>Témakör létrehozása

Adatkezelési műveletek szolgáltatás Bus témaköröket végezheti el a **ServiceBusContract** osztály keresztül. Egy **ServiceBusContract** objektumra, amely a Társítások jogkivonat engedélyek kezelése, hogy a megfelelő konfigurációval összeállítás, és a **ServiceBusContract** osztály Azure való kommunikáció egyetlen pontot.

A **ServiceBusService** osztály bemutat létrehozása, számba veheti a webhely és témakörök törlése. A következő példa bemutatja, hogyan egy **ServiceBusService** -objektum segítségével nevű-témakör létrehozása `TestTopic`, az úgynevezett névteret `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

A **TopicInfo** módszer, amelyekben kell állítani a témakör tulajdonságai (például: az alapértelmezett time-to-live (TTL) értéket a témakör küldött üzenetek alkalmazandó beállítása). A következő példa bemutatja, hogyan hozhat létre a témakör nevű `TestTopic` legfeljebb 5 GB méretű:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Figyelje meg, hogy felhasználhatja a **listTopics** módszer **ServiceBusContract** objektumok ellenőrzéséhez, ha a témakör a megadott nevű már létezik egy szolgáltatás névtere belül.

## <a name="create-subscriptions"></a>Előfizetések létrehozása

Az előfizetések témakörökre mutató is létre az **ServiceBusService** osztály. Az előfizetések neve, és beállíthatja, hogy a választható szűrő, amely a kapott az előfizetés virtuális várólista üzenetek készlete korlátozza.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Az alapértelmezett (MatchAll) szűrővel előfizetés létrehozása

A **MatchAll** szűrő az alapértelmezett szűrő, amely akkor használható, ha nincs szűrő van megadva, amikor új előfizetést jön létre. A **MatchAll** szűrő használatakor a témakör közzétett összes üzenet az előfizetés virtuális várólista kerülnek. A következő példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett **MatchAll** szűrő.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Előfizetések szűrőkkel létrehozása

Szűrők, amelyek lehetővé teszik fel a témakör küldött üzenetek jelenjen meg egy adott témakör előfizetés belül is létrehozhat.

A legtöbb rugalmas típusú előfizetések által támogatott szűrő a [SqlFilter][], amely SQL92 egy részét. Az üzenetek, a témakör közzétett tulajdonságainak SQL szűrők vonatkozni fognak. Ha többet szeretne tudni a kifejezések SQL szűrővel használható olvassa el a [SqlFilter.SqlExpression][] szintaxis.

Az alábbi példa létrehoz egy előfizetés nevű `HighMessages` csak kiválasztása egy egyéni **LastMessage elemet** tulajdonság 3-nál nagyobb üzeneteket [SqlFilter][] objektummal:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Hasonlóképpen a az alábbi példa létrehoz egy előfizetés nevű `LowMessages` csak kijelöli az üzeneteket, amelyek egy **LastMessage elemet** tulajdonság kisebb vagy egyenlő 3 [SqlFilter][] objektummal:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Ha a most üzenettel `TestTopic`, mindig kerülnek vevők fizetett elő, hogy a `AllMessages` előfizetését, és szelektív kézbesítve vevők fizetett elő, hogy a `HighMessages` és `LowMessages` előfizetések (az üzenet tartalmának függően).

## <a name="send-messages-to-a-topic"></a>A tematikus üzeneteket küldeni

Szolgáltatás Bus témakörben üzenet elküldéséhez az alkalmazás szerez egy **ServiceBusContract** objektumot. A következő kódot bemutatja, hogyan lehet üzenet küldése az `TestTopic` belül a korábban létrehozott témakör a `HowToSample` névtér:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Szolgáltatás Bus témakörök küldött üzeneteket a [BrokeredMessage][] osztály példánya is. [BrokeredMessage][] *objektumhoz tartozik-e egy sor olyan szabványos módszerek (például: * *setLabel* * és *az *élettartam**), tartsa lenyomva az ujját egyéni az alkalmazás-specifikus tulajdonságok használt szótár és egy tetszőleges alkalmazás adatok törzsében. Az alkalmazás a [BrokeredMessage][]és a megfelelő konstruktorának szerializálható objektumokat megkerülhetők állíthat be az üzenet törzsében * *DataContractSerializer* * majd használható szerializálni az objektumot. Másik lehetőségként a * *java.io.InputStream** állnak rendelkezésre.

Az alábbi példa bemutatja, hogyan öt vizsgált üzeneteket küldhet a `TestTopic` **MessageSender** azt a fenti kódrészletet kapott.
Figyelje meg, hogyan változik a minden üzenet **LastMessage elemet** a tulajdonság értékét a Ismétlés a leállításig az a (Ez határozza meg előfizetésekről kapta meg):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Szolgáltatás Bus témakörök a [szabványos réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md)használatát támogatja. A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs korlátozott tartani a témakör az üzenetek számát, de van egy vonalvég az üzenetek, a témakör birtokában összesített méretét. Ez a témakör a méret létrehozáskor, az 5 GB felső határa van megadva.

## <a name="how-to-receive-messages-from-a-subscription"></a>Hogyan üzenetek fogadására előfizetésből

Üzenetek fogadására előfizetésből, használja a **ServiceBusContract** objektum. A Beérkezett üzenetek is dolgozhat, két különböző módokban: **ReceiveAndDelete** és **PeekLock**.

A **ReceiveAndDelete** móddal fogadott egy olyan egyetlen-ikon művelet, – ez azt jelenti, hogy szolgáltatás Bus olvasási vonatkozóan kap megkeresést egy üzenetet, ha azt felhasználás alatt álló minősíti az üzenetet, és visszatér azt az alkalmazás. **ReceiveAndDelete** mód a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

**PeekLock** üzemmódban fogadhat válik egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a másodszintű fogadási folyamat befejezése hívja fel a **törlése** a kapott üzenet a. Szolgáltatás Bus láthatja a **törlése** hívás, amikor azt fog megjelölni az üzenetet az éppen, és távolítsa el azt a témakör.

Az alábbi példa bemutatja, hogyan üzenetek fogadható és feldolgozása **PeekLock** mód (nem az alapértelmezett mód). Az alábbi példában ismétlődve hajt végre, és az "HighMessages" előfizetés üzenetek feldolgozásával, és majd kilép, amikor nincsenek további üzenetek (azt is megteheti, hogy beállítható megvárja, amíg az új üzenetek).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja a **unlockMessage** módszer (helyett a **deleteMessage** módszer) kapott üzenet a. Ennek hatására a szolgáltatás Bus zárolásának feloldásához belül az üzenetet, és ismét kapott igénybe alkalmazásával ugyanazon vagy másik igénybe alkalmazás elérhetővé.

Is van zárolva belül üzenet társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenetet a zárolási időkorlát lejár (például ha az alkalmazás összeomlik), akkor szolgáltatás Bus zárolásának feloldásához automatikusan az üzenetet, és azt elérhetővé kapott újra.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **deleteMessage** kérelem kibocsátása előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább után feldolgozása**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el az üzenetet, amely végig a kézbesítési kísérletek változatlan marad **getMessageId** módszerrel.

## <a name="delete-topics-and-subscriptions"></a>Témák és az előfizetés törlése

Az elsődleges témakörök és az előfizetés törlése módja a **ServiceBusContract** objektum használatára. Témakör törlése is törölheti a regisztrált a témakör az előfizetése. Az előfizetések egymástól függetlenül is törlődnek.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus sorban várakozó alapjait, további információt talál [szolgáltatás Bus sorban várakozó, témák, és előfizetések][] .

  [Java Azure SDK]: http://azure.microsoft.com/develop/java/
  [Azure eszközkészlete Holdas]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Szolgáltatás Bus sorban várakozó, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
