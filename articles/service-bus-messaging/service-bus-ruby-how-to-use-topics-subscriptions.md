<properties
    pageTitle="Szolgáltatás Bus témakörök (fonetikus) használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként használja a szolgáltatás Bus témakörök és előfizetések Azure-ban. Mintakódok fonetikus alkalmazások kerülnek."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Szolgáltatási Bus témakörök/előfizetések használata

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez a cikk ismerteti, hogyan szolgáltatás Bus témakörök és előfizetések fonetikus alkalmazásból. Az érintett esetek **létrehozása, témák és előfizetés szűrők, üzenetküldés létrehozása az előfizetések** egy témakör, **előfizetésből üzenetek fogadására**és **témák és az előfizetés törlése**. További információt a témakörök és az előfizetéseket lásd: a [Következő lépésekkel](#next-steps) szakaszban.

## <a name="service-bus-topics-and-subscriptions"></a>Szolgáltatás Bus témakörök és előfizetések

Szolgáltatás Bus témakörök és előfizetések támogatja a *Közzététel/előfizetés* üzenetküldés kommunikációs modell. Amikor témák és az előfizetéseket, elosztott alkalmazás részei nem egymással közvetlenül; azok inkább üzenetváltásra közvetítő működik-e témakör keresztül.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Alkalmazással szemben szolgáltatás Bus sorok, ahol minden üzenet egyetlen ügyfél által feldolgozott, témák és előfizetések kommunikációs, mintázattal közzététel/előfizet **egy-a-többhöz** űrlap biztosítanak. Több előfizetés-témakörben regisztrálhatja lehetőség. Egy üzenet elküldésekor témakörben, majd teszi elérhetővé az egyes előfizetések független feldolgozása.

A témakör előfizetés hasonló, akkor egy virtuális várólista kapja a témakör küldött üzenetek másolatát. Másik lehetőségként regisztrálhatja a szűrő, amely lehetővé teszi, a szűrő/korlátozása mely üzenetek témakörben érkeznek témakör előfizetésekről előfizetés alapon témakör szabályok.

Szolgáltatás Bus témakörök és előfizetések engedélyezése, ha át kívánja méretezni nagy mennyiségű üzenetet feldolgozása sok felhasználók és az alkalmazások között.

## <a name="create-a-namespace"></a>Névtér létrehozása

Azure Service Bus sorban várakozó használatának megkezdéséhez, először létre kell hoznia egy névtér. Névtér tartalmazó tároló biztosít megcímezheti szolgáltatás Bus erőforrások az alkalmazáson belül. A névtér a parancssori felületen kell létrehoznia, mert az [Azure portálon][] nem hoz létre a névtér ACS kapcsolattal.

Névtér létrehozása:

1. Nyissa meg az Azure Powershell konzol ablakot.

2. Írja be a következő névtér létrehozása parancsot. Adja meg a saját névtér értéket, és adja meg az ugyanazon régió az alkalmazás.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Namespace létrehozása](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Szerezze be az alapértelmezett a névtér hitelesítő adatok kezelése

Annak érdekében, hogy végezze el az adatkezelési műveletek, például várólista hozza létre az új névtér, be kell szereznie a névtér management hitelesítő adatait.

A szolgáltatás Bus névtér létrehozásához futtatta PowerShell-parancsmag a billentyű használatával kezelheti a névtér jeleníti meg. Másolja a **DefaultKey** értékét. Ezt az értéket kell használni a kód később az oktatóprogram.

![Kulcs másolása](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> A kulcs Ha jelentkezzen be az [Azure-portálra][] , és lépjen a kapcsolat adatait a névtér is megtalálhatók.

## <a name="create-a-ruby-application"></a>Fonetikus-alkalmazás létrehozása

Című cikkben olvashat [a fonetikus alkalmazás a Azure létrehozása](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatát szolgáltatás Bus konfigurálása

Szolgáltatás Bus használni, töltse le és fonetikus Azure előkészítés, kényelmesebbé olyan tárat, amellyel kommunikálni a tároló többi szolgáltatást tartalmaz.

### <a name="use-rubygems-to-obtain-the-package"></a>A csomag beszerzése RubyGems használatával

1. Használja a parancssor, például **PowerShell** (Windows), a **terminált** (Mac) vagy a **Bash** (Unix).

2. Írja be a parancssorablakban telepítheti a gem és függőségek "gem telepítés azure".

### <a name="import-the-package"></a>A csomag importálása

A fonetikus fájlt, amelyben a tárhely használni kívánt tetején a kedvenc szövegszerkesztővel, adja hozzá a következő:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>A szolgáltatás Bus kapcsolat beállítása

Az Azure modul beolvassa a környezeti változók **AZURE\_SERVICEBUS\_NÉVTÉR** és **AZURE\_SERVICEBUS\_ACCESS\_kulcs** való csatlakozáshoz a névtér szükséges információt. Ha ezek a környezeti változók nincsenek beállítva, meg kell adnia a névtér információkat előtt **Azure::ServiceBusService** használata a következő kódot:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Állítsa a névtér értéket az érték, létrehozott, nem pedig annak a teljes URL-CÍMÉT. Ha például az **"yourexamplenamespace"**, nem "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Témakör létrehozása

A **Azure::ServiceBusService** objektum lehetővé teszi, hogy témakörök kezelheti. A következő kódot **Azure::ServiceBusService** objektum hoz létre. A témakör hozhat létre a **létrehozása\_topic()** módot. Az alábbi példa létrehoz egy témakört, vagy nyomtatja ki a hibák, ha vannak.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

A további beállításokat, amelyek lehetővé teszik, hogy az alapértelmezett témakör beállítások felülbírálása üzenet ideje például Live **Azure::ServiceBus::Topic** objektum vagy várólista maximális mérete is átadhatja. A következő példa bemutatja a várólista maximális mérete beállítás 5GB és az idő 1 percre aktív:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Előfizetések létrehozása

A témakör előfizetések is létre az **Azure::ServiceBusService** objektumra. Az előfizetések neve, és beállíthatja, hogy a választható szűrő, amely a virtuális várólista az előfizetés a beérkező üzenetek készlete korlátozza.

Az előfizetések állandó, és továbbra is vagy csak olyan vagy a témakör társítva, törlődnek. Ha az alkalmazás logika előfizetés létrehozása tartalmaz, akkor először győződjön meg ha a már létezik az előfizetés getSubscription módszerrel.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Az alapértelmezett (MatchAll) szűrővel előfizetés létrehozása

A **MatchAll** szűrő az alapértelmezett szűrő, amely akkor használható, ha nincs szűrő van megadva, amikor új előfizetést jön létre. A **MatchAll** szűrő használatakor a témakör közzétett összes üzenet az előfizetés virtuális várólista kerülnek. A következő példa létrehoz egy "az összes-üzenetek" nevű előfizetést, és használja az alapértelmezett **MatchAll** szűrő.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések szűrőkkel létrehozása

Azt is megadhatja, szűrők, amelyek lehetővé teszik, hogy adja meg, amelyek az adott előfizetést belül témakör küldött üzenetek jelenjen meg.

A legtöbb rugalmas típusú előfizetések által támogatott szűrő a **Azure::ServiceBus::SqlFilter**, amely SQL92 egy részét. Az üzenetek, a témakör közzétett tulajdonságainak SQL szűrők vonatkozni fognak. Ha többet szeretne tudni a kifejezések SQL szűrővel használható olvassa el a [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) szintaxis.

Felveheti szűrők előfizetés használatával a **létrehozása\_rule()** **Azure::ServiceBusService** objektum metódusát. Ez a módszer lehetővé teszi az új szűrők hozzáadása meglévő előfizetéshez.

Mivel az alapértelmezett szűrő automatikusan alkalmazza az összes új előfizetést, el kell távolítani a szűrőt, vagy a **MatchAll** felülírják a többi szűrőket is megadhat. Az alapértelmezett szabály használatával eltávolíthatja a **törlése\_rule()** módszer a **Azure::ServiceBusService** objektumon.

Az alábbi példa létrehoz "nagy-üzenetek", amely csak kiválasztja az egyéni üzeneteket **Azure::ServiceBus::SqlFilter** nevű előfizetést **üzenet\_szám** tulajdonság nagyobb, mint 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Hasonlóképpen az alábbi példa létrehoz nevű "alacsony-üzenetek" **Azure::ServiceBus::SqlFilter** , amely csak a **message_number** tulajdonság kisebb vagy egyenlő 3 üzeneteket kiválasztja az előfizetést:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Amikor a "próba-témakör" most már küld üzenetet, mindig kell átadása fizetett elő, a "minden-üzenetek" témakör előfizetést, és a szelektív (az üzenet tartalmának függően) a "nagy-üzenetek" és "alacsony-üzenetek" témakör előfizetéseket előfizetett vevők sikeresen kézbesítve lett a címzett.

## <a name="send-messages-to-a-topic"></a>A témakör üzeneteket küldeni

A szolgáltatás Bus tematikus üzenetet küldeni, az alkalmazás használatához a **küldése\_témakör\_message()** módszer a **Azure::ServiceBusService** objektum. Szolgáltatás Bus témakörök küldött üzeneteket a **Azure::ServiceBus::BrokeredMessage** objektumok példánya is. **Azure::ServiceBus::BrokeredMessage** objektumhoz tartozik-e egy sor olyan általános tulajdonságok ( **például** és **idő\_való\_live**), tartsa lenyomva az ujját egyéni alkalmazás adott tulajdonságai használt szótár és a karakterlánc típusú adatok törzsében. Az alkalmazások állíthat be az üzenet törzsében megkerülhetők, egy karakterlánc értéket, a **küldése\_témakör\_message()** módszer, és minden szükséges általános tulajdonságok: tölti fel a alapértelmezett értékek szerint.

A következő példa bemutatja, hogyan öt vizsgált "próba-témakör" üzenetek küldése. Megjegyzendő, hogy minden üzenet **message_number** egyéni tulajdonság értékét a a példányaiban le változó (Ez határozza meg, hogy mely előfizetés kapja meg):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Szolgáltatás Bus témakörök a [szabványos réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md)használatát támogatja. A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs korlátozott tartani a témakör az üzenetek számát, de van egy vonalvég az üzenetek témakör birtokában összesített méretét. Ez a témakör a méret létrehozáskor, az 5 GB felső határa van megadva.

## <a name="receive-messages-from-a-subscription"></a>Hibaüzenetek előfizetésből

Üzenetek kapott egy előfizetés használata a **fogadása\_előfizetés\_message()** módszer a **Azure::ServiceBusService** objektumon. Alapértelmezés szerint az üzenetek read(peak) és anélkül, hogy az előfizetés törölné zárolva van. El tudja olvasni és az üzenet törlése az előfizetésből megadásával a **betekintő\_zárolása** **hamis**lehetőséget.

Az alapértelmezett működés teszi az olvasási és egy kétszakaszos műveletet, amely is lehetővé teszi a támogatási alkalmazásokat, amelyek nem elviselni hiányzó üzenetek törlése. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően hívja fel a másodszintű fogadási folyamat befejezése **törlése\_előfizetés\_message()** módszer, és az üzenet törlődik paraméterként kezeléséről. A **törlése\_előfizetés\_message()** módszer olvasottként megjelölheti felhasználás alatt álló lesz, és eltávolítása abból az előfizetést.

Ha a **: betekintő\_zárolása** paraméter értéke **hamis**, Olvasás, és törli az üzenetet a legegyszerűbb modell válik, és felhasználási területei, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli legjobb. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

Az alábbi példa bemutatja, hogyan lehet üzeneteket fogadni, és feldolgozott használatával **fogadása\_előfizetés\_message()**. A példa először kap, és az "alacsony-üzenetek" előfizetés használatával üzenet törlése **: betekintő\_zárolása** értéke **hamis**, és a "nagy-üzenetek" a másik üzenetet kap, és a majd törli az üzenet sablonnal **törlése\_előfizetés\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, akkor a felhívhatja, ha azt a **zárolásának feloldásához\_előfizetés\_message()** módszer a **Azure::ServiceBusService** objektumon. Ennek hatására a szolgáltatás Bus zárolásának feloldása az üzenetet az előfizetésben, és tegye elérhetővé az azonos igénybe alkalmazásával vagy egy másik igénybe alkalmazás ismét kapott.

Is van egy üzenetet, az előfizetés belül zárolt társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenetet a zárolás időtúllépési lejár (például ha az alkalmazás összeomlik), akkor szolgáltatás Bus zárolásának feloldásához automatikusan az üzenetet, és azt elérhetővé kapott újra.

Abban az esetben, ha az alkalmazás összeomlik, de mielőtt feldolgozása az üzenet után a **törlése\_előfizetés\_message()** módszer neve, majd az alkalmazás újra az üzenetet, újraindításkor. Gyakran Link **Legalább egyszer feldolgozása**; Ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. A logika gyakran elért használata a **üzenet\_azonosító** tulajdonság az üzenet, amely végig a kézbesítési kísérletek változatlan marad.

## <a name="delete-topics-and-subscriptions"></a>Témák és az előfizetés törlése

Témák és előfizetések állandó, és explicit módon törölni kell az [Azure portál][] vagy keresztül programozás útján. Az alábbi példa bemutatja, hogyan lehet törlés a témakör "próba-témakör" nevű.

```
azure_service_bus_service.delete_topic("test-topic")
```

Témakör törlése a regisztrált a témakör az előfizetése is törli. Az előfizetések egymástól függetlenül is törlődnek. A következő kódot bemutatja, hogyan lehet a "próba-témakör" témakör "nagy-üzenetek" nevű az előfizetés törlése:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus témakörök alapjait, kövesse az alábbi hivatkozásokra kattintva további.

- [Sorok, a témakörök és az előfizetések](service-bus-queues-topics-subscriptions.md)megtekintése
- [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx)API hivatkozását.
- Keresse fel a [Fonetikus Azure SDK](https://github.com/Azure/azure-sdk-for-ruby) tárházba GitHub.
 
[Azure portál]: https://portal.azure.com
