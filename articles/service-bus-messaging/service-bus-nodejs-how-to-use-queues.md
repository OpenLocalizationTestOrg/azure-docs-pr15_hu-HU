<properties 
    pageTitle="Szolgáltatás Bus sorban várakozó haszná Node.js |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Azure Service Bus sorban várakozó használata Node.js alkalmazásból." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Szolgáltatás Bus sorban várakozó használata

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez a cikk ismerteti, hogyan Node.js szolgáltatás Bus sorban várakozó használni. A minták JavaScript írt, és használja a Node.js Azure modult. Az érintett esetek **sorban várakozó létrehozása**, **üzenetek küldése és fogadása**, és **Sorok törlése**. Sorban várakozó a további tudnivalókért lásd: a [következő lépésekkel](#next-steps) szakaszban.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-nodejs-application"></a>Node.js-alkalmazás létrehozása

Hozzon létre egy üres Node.js alkalmazást. Node.js-alkalmazás létrehozása című témakörben talál [létrehozása az Azure-webhelyet egy Node.js alkalmazás telepítéséhez és][], vagy a Windows PowerShell használatá [Node.js Felhőszolgáltatásba][] .

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatához szolgáltatás Bus konfigurálása

Azure Service Bus használni, töltse le és Node.js Azure előkészítés. A csomag magában foglalja a egy sor olyan tárat, amellyel kommunikálni a szolgáltatás Bus többi szolgáltatások.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>A csomag beszerzése csomópont csomag Manager (NPM) használatával

1. Nyissa meg azt a **Node.js a Windows PowerShell** -parancsablakban segítségével a **c:\\csomópont\\sbqueues\\WebRole1** mappát, amelyben a minta-alkalmazás létrehozása.

2. Írja be a **npm telepítse az azure** kell eredményhez az alábbihoz hasonló parancsablakban:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3. Manuális futtatását is lehetővé teszi annak ellenőrzésére, hogy az **ls** parancs a **csomópont\_modulok** mappa létrehozásának. Egy adott mappába keresse meg az **azure** csomagra, amely tartalmazza a tárak szolgáltatás Bus sorban várakozó eléréséhez szükséges.

### <a name="import-the-module"></a>A modul importálása

A **server.js** fájlt az alkalmazás tetején Jegyzettömb vagy egy másik szövegszerkesztőben használata esetén adja hozzá a következő:

```
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Az Azure Service Bus kapcsolat beállítása

Az Azure modul beolvassa a környezeti változók AZURE\_SERVICEBUS\_NÉVTÉR és AZURE\_SERVICEBUS\_ACCESS\_szolgáltatás Bus csatlakozhat szükséges információk beszerzése BILLENTYŰT. Ha ezek a környezeti változók nincsenek beállítva, **createServiceBusService**hívásakor meg kell adnia a fiók adatait.

Egy példa a konfigurációs fájl a környezeti változók az Azure felhőalapú szolgáltatás beállítása című témakörben [Node.js Felhőszolgáltatásba adathordozós][].

Példa a környezeti változók beállítása az [Azure klasszikus portál][] az Azure-webhelyhez olvassa el a [Node.js webalkalmazás adathordozós][]című témakört.

## <a name="create-a-queue"></a>Várólista létrehozása

A **ServiceBusService** objektum lehetővé teszi, hogy szolgáltatás Bus sorban várakozó kezelheti. A következő kódot létrehoz egy **ServiceBusService** -objektumot. Hozzá felső részén a **server.js** -fájl importálása az Azure modul-utasítás után:

```
var serviceBusService = azure.createServiceBusService();
```

A **ServiceBusService** objektum **createQueueIfNotExists** meghívásával a megadott várólista ad vissza (ha van ilyen), vagy a megadott nevű új várólista jön létre. A következő kódot használ **createQueueIfNotExists** létrehozása vagy a nevű várólista csatlakoztatása `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** támogatja a további beállítások, amely lehetővé teszi, hogy az alapértelmezett várólista beállítások felülbírálása például üzenet time to live vagy a legnagyobb várólista mérete is. Az alábbi példa a 5 GB, és egyszerre szeretné live (TTL) érték 1 perc állítja be a várólista maximális mérete:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Szűrők

Választható szűrési műveletek végrehajtott műveletek naplóadatai **ServiceBusService**alkalmazhatók. Szűrési műveletek is elhelyezhet, naplózás, automatikusan újra próbálkozik stb. Szűrők olyan objektumok, egy módszer a aláírással végrehajtása:

```
function handle (requestOptions, next)
```

Után hajtsa végre a előtti feldolgozás az kérelem beállításai, a módszerrel kell hívnia `next`, a következő aláírással visszahívás áthaladó:

```
function (returnObject, finalCallback, next)
```

A visszahívás, és utána feldolgozása **returnObject** (válasza a kérelmet a kiszolgálón), vagy kell hívnia a visszahívás `next` Ha létezik továbbra is a további szűrők feldolgozása, vagy egyszerűen meghívása `finalCallback`, amely a művelet lezárja a szolgáltatás hívás.

Két szűrő látható, hogy ismét logikájának megvalósítása is megjelennek az Azure SDK Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. A következő létrehoz egy **ServiceBusService** -objektum, amely a **ExponentialRetryPolicyFilter**használja:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Üzenetek küldése egy sorba

A szolgáltatás Bus várólista üzenet elküldéséhez az alkalmazás hívások **sendQueueMessage** módszer a **ServiceBusService** objektum. Üzenetek küldenek (és a kapott) szolgáltatás Bus sorban várakozó **BrokeredMessage** objektumok, és egy sor olyan általános tulajdonságok (például **címke** és **az élettartam**), tartsa lenyomva az ujját egyéni az alkalmazás-specifikus tulajdonságok használt szótár, és egy tetszőleges alkalmazás adatok törzsében. Az alkalmazások karakterlánc áthaladó az üzenet az üzenet törzsében is beállíthatja. Minden szükséges általános tulajdonságok: a program kitölti az alapértelmezett értékeket.

A következő példa bemutatja, hogyan vizsgálati üzenet küldése a sorba, nevű `myqueue` **sendQueueMessage**használata:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Szolgáltatás Bus sorban várakozó támogatja a [Normál réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md). A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs várólista tartott üzenetek száma korlátozott, de van egy vonalvég az üzenetek várólista birtokában összesített méretét. A várólista mérete létrehozáskor, az 5 GB felső határa van megadva. Kvóták kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus kvóták][]című témakört.

## <a name="receive-messages-from-a-queue"></a>Üzenetek fogadására várólista

A **receiveQueueMessage** módszerrel a **ServiceBusService** objektum várólista érkező üzenetek vannak. Alapértelmezés szerint az üzenetek törlődnek a sorból, a olvasnak; azonban el tudja olvasni (betekintő) és az üzenet zárolása a választható paraméter **isPeekLock** **true**megadásával a sorból törlése nélkül.

Az alapértelmezett működés olvasási és az üzenet törlése a fogadási művelet részeként a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

A **isPeekLock** paraméter értéke **Igaz**, ha a fogadás válik, egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Után az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra), a másodszintű fogadási folyamat befejezése **deleteMessage** metódus meghívása, valamint képzések paraméterként törlődik az üzenetet. A **deleteMessage** módszer a fog megjelölni az üzenetet az éppen, és távolítsa el azt a sorból.

A következő példa bemutatja, hogyan és az üzenetek **receiveQueueMessage**feldolgozása. A példa először kap, és töröl egy üzenetet, majd kap az üzenetek **isPeekLock** értéke **Igaz**, majd törli az üzenetet **deleteMessage**használata:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja **unlockMessage** módszer a **ServiceBusService** objektum. Ennek hatására a szolgáltatás Bus zárolásának feloldása a várólista belül az üzenetet, és ismét kapott igénybe alkalmazásával ugyanazon vagy másik igénybe alkalmazás elérhetővé.

Is van zárolva van, a sor belül üzenet társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenetet a zárolási időkorlát lejár (pl., ha az alkalmazás összeomlik), akkor szolgáltatás Bus zárolásának feloldásához automatikusan az üzenetet, és azt elérhetővé kapott újra.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **deleteMessage** módszer neve előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább után feldolgozása**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el az üzenetet, amely végig a kézbesítési kísérletek változatlan marad **MessageId** tulajdonságával.

## <a name="next-steps"></a>Következő lépések

További tudnivalók sorok, az alábbi forrásokban talál.

-   [Sorok, témák és előfizetések][]
-   [Azure SDK csomópont][] tárházából GitHub
-   [NODE.js Developer Center](/develop/nodejs/)

  [Azure SDK csomóponthoz]: https://github.com/Azure/azure-sdk-for-node
  [Azure klasszikus portál]: http://manage.windowsazure.com
  
  [NODE.js felhőalapú szolgáltatás]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
  [Hozzon létre és telepítsen egy Node.js alkalmazást egy Azure-webhelyre]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [NODE.js Felhőszolgáltatásba tárhely]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Webalkalmazás node.js adathordozós]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Szolgáltatás Bus kvóták]: service-bus-quotas.md
 
