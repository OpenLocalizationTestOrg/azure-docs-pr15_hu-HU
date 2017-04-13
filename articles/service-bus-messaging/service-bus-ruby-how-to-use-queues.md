<properties
    pageTitle="Szolgáltatás Bus sorban várakozó használata fonetikus |} Microsoft Azure"
    description="Megtudhatja, hogy miként használja a szolgáltatás Bus sorban várakozó Azure-ban. A mintakódok fonetikus nyelven íródott."
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

# <a name="how-to-use-service-bus-queues"></a>Szolgáltatás Bus sorban várakozó használata

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez az útmutató szolgáltatás Bus sorban várakozó használatát ismerteti. A minták fonetikus írt, és az Azure gem használja. Az érintett esetek **létrehozása sorban várakozó, üzenetek küldése és fogadása**, és a **Sorok törlése**. Szolgáltatás Bus sorban várakozó kapcsolatos további tudnivalókért olvassa el a a [Következő lépésekkel](#next-steps) szakasz című témakört.

## <a name="what-are-service-bus-queues"></a>Mik azok a sorok szolgáltatás Bus?

Szolgáltatás Bus sorban várakozó *brokered üzenetküldés* kommunikációs modell támogatja. A sorok elosztott alkalmazás részei nem egymással közvetlenül; Ehelyett azok üzenetváltásra közvetítő végpontként várólista keresztül. Egy üzenet gyártó (Feladó) kezek ki az üzenetet a várakozási sorban található, és majd továbbra is a feldolgozása.
Aszinkron egy üzenet fogyasztói (címzett) gyűjti össze a sorból az üzenetet, és dolgozza fel. A gyártó nem kell a válaszra várakozás az ügyféltől annak érdekében, hogy továbbra is folyamat, és további küldésére. Sorok üzenetkézbesítés **első, első ki (FIFO)** egy vagy több versengő felhasználóknak kínálnak. Ez azt jelenti, hogy az üzenetek általában kapott és az abban a sorrendben, amelyben a sor hozzáadásuk, és minden üzenet kapott, és csak egy üzenet fogyasztói által feldolgozott vevők által feldolgozott.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Szolgáltatás Bus sorban várakozó olyan általános célú technológia esetek széles választéka használható:

-   A [több szálon Azure alkalmazás](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)webes és dolgozó szerepkörök közötti kommunikáció.
-   A helyszíni alkalmazások és az Azure közötti kommunikációt alkalmazások [hibrid megoldást](service-bus-dotnet-hybrid-app-using-service-bus-relay.md)is.
-   Más szervezetek vagy a részlegek egy szervezet helyszíni futtató elosztott alkalmazás összetevői közötti kommunikációt.

Sorok használatával lehetővé teszi az alkalmazások jobb méretezése, és engedélyezze a architektúra további erősítése.

## <a name="create-a-namespace"></a>Névtér létrehozása

Azure Service Bus sorban várakozó használatának megkezdéséhez, először létre kell hoznia egy névtér. Névtér tartalmazó tároló biztosít megcímezheti szolgáltatás Bus erőforrások az alkalmazáson belül. A névtér a parancssori felületen létre kell hoznia, mert az Azure portálon nem hoz létre a névtér ACS kapcsolattal.

Névtér létrehozása:

1. Nyissa meg az Azure Powershell konzolt.

2. Írja be a következő szolgáltatás Bus névtér létrehozása parancsot. Adja meg a saját névtér értéket, és adja meg az ugyanazon régió az alkalmazás.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Szerezze be a névtér hitelesítő adatok kezelése

Annak érdekében, hogy végezze el az adatkezelési műveletek, például várólista hozza létre az új névtér, be kell szereznie a névtér management hitelesítő adatait.

A PowerShell-parancsmag létrehozása az Azure szolgáltatás bus névtere futtatta a kulcsot a névtér kezelésére használható jeleníti meg. Másolja a **DefaultKey** értékét. Ezt az értéket kell használni a kód később az oktatóprogram.

![Kulcs másolása](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] A kulcs Ha jelentkezzen be az [Azure-portálra](https://portal.azure.com/) , és nyissa meg azt a kapcsolat adatait, hogy a szolgáltatás Bus névtér is megtalálhatók.

## <a name="create-a-ruby-application"></a>Fonetikus-alkalmazás létrehozása

Fonetikus-alkalmazás létrehozása. Című cikkben olvashat [a fonetikus alkalmazás a Azure létrehozása](/develop/ruby/tutorials/web-app-with-linux-vm/).

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatához szolgáltatás Bus konfigurálása

Azure Service Bus használni, töltse le és fonetikus Azure előkészítés, kényelmesebbé olyan tárat, amellyel kommunikálni a tároló többi szolgáltatást tartalmaz.

### <a name="use-rubygems-to-obtain-the-package"></a>A csomag beszerzése RubyGems használatával

1. Használja a parancssor, például **PowerShell** (Windows), a **terminált** (Mac) vagy a **Bash** (Unix).

2. Írja be a parancssorablakban telepítheti a gem és függőségek "gem telepítés azure".

### <a name="import-the-package"></a>A csomag importálása

A fonetikus fájlt, ahol tároló használni kívánt tetején a kedvenc szövegszerkesztővel, adja hozzá a következő:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Az Azure Service Bus kapcsolat beállítása

Az Azure modul beolvassa a környezeti változók **AZURE\_SERVICEBUS\_NÉVTÉR** és **AZURE\_SERVICEBUS\_ACCESS_KEY** való csatlakozáshoz a szolgáltatás Bus névtér szükséges információt. Ha ezek a környezeti változók nincsenek beállítva, meg kell adnia a névtér információkat előtt **Azure::ServiceBusService** használata a következő kódot:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Állítsa a névtér értéket az érték hozta létre, nem pedig annak a teljes URL-CÍMÉT. Ha például az **"yourexamplenamespace"**, nem "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Hogyan hozhat létre egy várólista

A **Azure::ServiceBusService** objektum lehetővé teszi, hogy sorban várakozó kezelheti. Várólista létrehozására **create_queue()** módszert. Az alábbi példa létrehoz egy várólista vagy nyomtatja ki a hibák.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Egy **Azure::ServiceBus::Queue** objektum további lehetőségeket, amely lehetővé teszi, hogy bírálja felül az alapértelmezett várólista beállításait, például üzenet time to live és várólista maximális mérete is eltelhet. A következő példa bemutatja, hogyan kell beállítani a várólista maximális mérete 5GB és az idő 1 percre aktív:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Üzenetek küldése egy sorba

Üzenet küldése egy szolgáltatás Bus sorba, az alkalmazás hívások a **küldése\_várólista\_message()** módszer a **Azure::ServiceBusService** objektumon. Üzenetek küldenek (és a kapott) szolgáltatás Bus sorban várakozó **Azure::ServiceBus::BrokeredMessage** objektumok, és általános tulajdonságok rendelkeznek ( **például** és **idő\_való\_live**), tartsa lenyomva az ujját egyéni alkalmazás adott tulajdonságai használt szótár és egy tetszőleges alkalmazás adatok törzsében. Az alkalmazás a üzenetként szöveges értékként megkerülhetők állíthat be az üzenet törzsébe, és minden szükséges általános tulajdonságok: tölti fel alapértékeket.

Az alábbi példa bemutatja, hogyan küldhet el a "próba-várólista" használatával nevű várakozási sorban található vizsgálati üzenet **küldése\_várólista\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Szolgáltatás Bus sorban várakozó támogatja a [Normál réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md). A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs várólista tartott üzenetek száma korlátozott, de van egy vonalvég az üzenetek várólista birtokában összesített méretét. A várólista mérete létrehozáskor, az 5 GB felső határa van megadva.

## <a name="how-to-receive-messages-from-a-queue"></a>Hogyan várólista érkező üzenetek fogadására

Üzenetek kapott egy várólista használata a **fogadása\_várólista\_message()** módszer a **Azure::ServiceBusService** objektumon. Alapértelmezés szerint az üzenetek olvasása és zárolt állapotban van, a sor törlése nélkül. Jó helyen jár, törölheti az üzenetek az a sor olvasnak beállításával, a **: peek_lock** **hamis**lehetőséget.

Az alapértelmezett működés teszi az olvasási és egy kétszakaszos műveletet, amely is lehetővé teszi a támogatási alkalmazásokat, amelyek nem elviselni hiányzó üzenetek törlése. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően hívja fel a másodszintű fogadási folyamat befejezése **törlése\_várólista\_message()** módszer, és az üzenet törlődik paraméterként kezeléséről. A **törlése\_várólista\_message()** módszer olvasottként megjelölheti felhasználás alatt álló lesz, és távolítsa el azt a sorból.

Ha a **: betekintő\_zárolása** paraméter értéke **hamis**, Olvasás, és törli az üzenetet a legegyszerűbb modell válik, és felhasználási területei, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli legjobb. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Szolgáltatás Bus fog jelölt felhasználás alatt álló, amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása az üzenetet, mert azt fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

A következő példa bemutatja, hogyan és az üzenetek folyamat **fogadása\_várólista\_message()**. A példa először kap, és törli az üzenet használatával **: betekintő\_zárolása** értéke **hamis**, akkor azt egy másik üzenetet kap, és majd törli az üzenet sablonnal **törlése\_várólista\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, akkor a felhívhatja, ha azt a **zárolásának feloldásához\_várólista\_message()** módszer a **Azure::ServiceBusService** objektumon. Ennek hatására a szolgáltatás Bus zárolásának feloldása a várólista belül az üzenetet, és ismét kapott igénybe alkalmazásával ugyanazon vagy másik igénybe alkalmazás elérhetővé.

Is van zárolva van, a sor belül üzenet társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenet a zárolási időkorlát lejár (például ha az alkalmazás összeomlik), majd a szolgáltatás Bus automatikusan feloldja az üzenetet, és újra kapott számára elérhetővé teszi azt.

Abban az esetben, ha az alkalmazás összeomlik, de mielőtt feldolgozása az üzenet után a **törlése\_várólista\_message()** módszer neve, majd az üzenet fog újra az alkalmazást az újraindításkor. Gyakran Link **Legalább egyszer feldolgozása**; Ez azt jelenti, hogy minden üzenet legalább egyszer feldolgozása, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el használata a **üzenet\_azonosító** tulajdonság az üzenet, amely végig a kézbesítési kísérletek változatlan marad.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus sorban várakozó alapjait, kövesse az alábbi hivatkozásokra kattintva további.

-   [Sorok, a témakörök és a előfizetések](service-bus-queues-topics-subscriptions.md)áttekintése.
-   Keresse fel a [Fonetikus Azure SDK](https://github.com/Azure/azure-sdk-for-ruby) tárházba GitHub.

Ez a cikk és Azure sorban várakozó [fonetikus várólista tárhelyet használata](../storage/storage-ruby-how-to-use-queue-storage.md) című témakörben tárgyalt tárgyalt, az Azure Service Bus sorban várakozó összehasonlítása a [Azure sorban várakozó és Azure Service Bus sorban várakozó - összehasonlítás és Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md) témakörökben talál.
 
