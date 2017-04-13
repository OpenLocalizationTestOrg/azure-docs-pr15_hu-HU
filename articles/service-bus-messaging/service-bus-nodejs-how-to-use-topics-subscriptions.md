<properties 
    pageTitle="Szolgáltatás Bus témakörök használata Node.js |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használja szolgáltatás Bus témakörök és a előfizetések Azure Node.js alkalmazásból." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Hogyan használja szolgáltatás Bus témakörök és előfizetések

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez az útmutató ismerteti, hogyan szolgáltatás Bus témakörök és előfizetések Node.js alkalmazásból. Az érintett esetek **létrehozása, témák és az előfizetéseket**, **előfizetés szűrők létrehozásával**, **üzenetküldés** egy témát, **előfizetésből üzenetek fogadására**és **témák és az előfizetés törlése**. További információt a témakörök és előfizetések című [következő lépések](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Node.js-alkalmazás létrehozása

Hozzon létre egy üres Node.js alkalmazást. Egy Node.js-alkalmazás létrehozása, tanulmányozza [létrehozása és helyezhetnek üzembe a Node.js alkalmazás az Azure webhely], a Windows PowerShell vagy webhely használata WebMatrix [Node.js Felhőszolgáltatásba][] .

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatához szolgáltatás Bus konfigurálása

Használ szolgáltatási Bus, töltse le a Node.js Azure csomagot. A csomag magában foglalja a egy sor olyan tárat, amellyel kommunikálni a szolgáltatás Bus többi szolgáltatások.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>A csomag beszerzése csomópont csomag Manager (NPM) használatával

1.  Használja a parancssor **PowerShell** (Windows), például **Terminálszolgáltatások** (Mac), illetve **Bash** (Unix), nyissa meg azt a mappát, amelyen létrehozta a minta alkalmazást.

2.  Írja be a **npm telepítse az azure** a parancs ablakban, amely a következő eredményt kell:

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

3.  Manuális futtatását is lehetővé teszi annak ellenőrzésére, hogy az **ls** parancs a **csomópont\_modulok** mappa létrehozásának. Egy adott mappába keresse meg az **azure** csomagra, amely tartalmazza a tárak szolgáltatás Bus témakörök eléréséhez szükséges.

### <a name="import-the-module"></a>A modul importálása

A **server.js** fájlt az alkalmazás tetején Jegyzettömb vagy egy másik szövegszerkesztőben használata esetén adja hozzá a következő:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>A szolgáltatás Bus kapcsolat beállítása

Az Azure modul beolvassa a környezeti változók AZURE\_SERVICEBUS\_NÉVTÉR és AZURE\_SERVICEBUS\_ACCESS\_kulcs szolgáltatás Bus való kapcsolódáshoz szükséges információt. Ha ezek a környezeti változók nincsenek beállítva, **createServiceBusService**hívásakor meg kell adnia a fiók adatait.

Egy példa a konfigurációs fájl a környezeti változók az Azure felhőalapú szolgáltatás beállítása című témakörben [Node.js Felhőszolgáltatásba adathordozós][].

Példa a környezeti változók beállítása az [Azure klasszikus portál][] az Azure-webhelyhez olvassa el a [Node.js webalkalmazás adathordozós][]című témakört.

## <a name="create-a-topic"></a>Témakör létrehozása

A **ServiceBusService** objektum lehetővé teszi, hogy témakörök kezelheti. A következő kódot létrehoz egy **ServiceBusService** -objektumot. Hozzá felső részén a **server.js** -fájl importálása az azure modul-utasítás után:

```
var serviceBusService = azure.createServiceBusService();
```

**CreateTopicIfNotExists** meghívásával a **ServiceBusService** objektum (ha van ilyen) visszatér a megadott témakört, vagy a megadott nevű új témakör jön létre. A következő kódot **createTopicIfNotExists** létrehozása vagy a témakör "MyTopic" nevű csatlakoztatása használja:

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** támogatja a további beállítások, amely lehetővé teszi, hogy az alapértelmezett témakör beállítások felülbírálása például üzenet time to live vagy a témakör maximális mérete is. A következő példa állítja be a témakör maximális mérete 5GB 1 perc élő idővel:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Szűrők

Választható szűrési műveletek végrehajtott műveletek naplóadatai **ServiceBusService**alkalmazhatók. Szűrési műveletek is elhelyezhet, naplózás, automatikusan újra próbálkozik stb. Szűrők olyan objektumok, egy módszer a aláírással végrehajtása:

```
function handle (requestOptions, next)
```

Végrehajtása az kérelem beállításai előfeldolgozása, után a módszerrel felhívja `next` továbbítása a következő aláírással visszahívás:

```
function (returnObject, finalCallback, next)
```

A visszahívás, és utána feldolgozása **returnObject** (válasza a kérelmet a kiszolgálón) vagy a visszahívás igények meghívása Ezután ha létezik továbbra is a további szűrők feldolgozása, vagy egyszerűen meghívása **finalCallback** egyébként szolgáltatás meghívását kerüljenek.

Két szűrő látható, hogy ismét logikájának megvalósítása is megjelennek az Azure SDK Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. A következő objektumot hoz létre **ServiceBusService** , amely a **ExponentialRetryPolicyFilter**használja:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Előfizetések létrehozása

A témakör előfizetések is létre az **ServiceBusService** objektumra. Az előfizetések neve, és beállíthatja, hogy a választható szűrő, amely a virtuális várólista az előfizetés a beérkező üzenetek készlete korlátozza.

> [AZURE.NOTE] Az előfizetések állandó, és továbbra is vagy csak olyan vagy a témakör társítva, törlődnek. Ha az alkalmazás logika előfizetés létrehozása tartalmaz, akkor először győződjön meg ha a már létezik az előfizetés **getSubscription** módszerrel.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Az alapértelmezett (MatchAll) szűrővel előfizetés létrehozása

A **MatchAll** szűrő az alapértelmezett szűrő, amely akkor használható, ha nincs szűrő van megadva, amikor új előfizetést jön létre. A **MatchAll** szűrő használatakor a témakör közzétett összes üzenet az előfizetés virtuális várólista kerülnek. A következő példa létrehoz egy "AllMessages" nevű előfizetést, és használja az alapértelmezett **MatchAll** szűrő.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések szűrőkkel létrehozása

Szűrők, amelyek lehetővé, hogy a hatókör, amely üzeneteket küldeni a témakör jelenjen meg egy adott témakör előfizetés belül is létrehozhat.

A legtöbb rugalmas típusú előfizetések által támogatott szűrő a **SqlFilter**, amely SQL92 egy részét. Az üzenetek, a témakör közzétett tulajdonságainak SQL szűrők vonatkozni fognak. Ha többet szeretne tudni a kifejezések SQL szűrővel használható, tekintse át a [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxis.

Szűrők előfizetés az **ServiceBusService** objektum **createRule** módszerrel bővíthető. Ez a módszer lehetővé teszi, hogy az új szűrők hozzáadása meglévő előfizetéshez.

> [AZURE.NOTE] Az alapértelmezett szűrő automatikusan alkalmazza az összes új előfizetést, mert el kell távolítani a szűrőt, vagy a **MatchAll** felülírják a többi szűrőket is megadhat. Az alapértelmezett szabály eltávolíthatja a **ServiceBusService** objektum **deleteRule** módszerrel.

Az alábbi példa létrehoz egy előfizetés nevű `HighMessages` **SqlFilter** , amely csak a 3-nál nagyobb egyéni **LastMessage elemet** tulajdonság üzeneteket kiválasztja a:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Hasonlóképpen a az alábbi példa létrehoz egy előfizetés nevű `LowMessages` **SqlFilter** , amely csak a **LastMessage elemet** tulajdonság kisebb vagy egyenlő 3 üzeneteket kiválasztja a:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ha a most üzenettel `MyTopic`, mindig kerülnek vevők fizetett elő, hogy a `AllMessages` témakör előfizetését, és szelektív kézbesítve vevők fizetett elő, hogy a `HighMessages` és `LowMessages` témakör előfizetések (az üzenet tartalmának függően).

## <a name="how-to-send-messages-to-a-topic"></a>A témakör az üzenetek küldése

A szolgáltatás Bus tematikus üzenet elküldéséhez a az alkalmazás a **ServiceBusService** objektum **sendTopicMessage** módszert kell használni.
Szolgáltatás Bus témakörök küldött üzenetek **BrokeredMessage** objektumai.
Egyéni alkalmazások adott tulajdonságainak tartása használt szótár és karakterlánc típusú adatok törzsében **BrokeredMessage** objektumhoz tartozik-e egy sor olyan általános tulajdonságok (például **címke** és **az élettartam**). Az alkalmazások megkerülhetők, ha szöveges értékként a **sendTopicMessage** állíthat be az üzenet törzsébe, és minden szükséges általános tulajdonságok: tölti fel a alapértelmezett értékek szerint.

A következő példa bemutatja, hogyan küldhet öt vizsgált üzeneteket "MyTopic". Megjegyzendő, hogy minden üzenet **LastMessage elemet** a tulajdonság értékét a a példányaiban le változó (Ez határozza meg előfizetésekről kapta meg):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Szolgáltatás Bus témakörök a [szabványos réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md)használatát támogatja. A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs korlátozott tartani a témakör az üzenetek számát, de van egy vonalvég az üzenetek témakör birtokában összesített méretét. Ez a témakör a méret létrehozáskor, az 5 GB felső határa van megadva.

## <a name="receive-messages-from-a-subscription"></a>Hibaüzenetek előfizetésből

A **receiveSubscriptionMessage** módszerrel a **ServiceBusService** objektum előfizetés érkező üzenetek vannak. Alapértelmezés szerint az üzenetek törlődnek az előfizetésből, olvasnak; azonban el tudja olvasni (betekintő) és az üzenet zárolása törlése az előfizetésből **true**a választható paraméter **isPeekLock** megadásával nélkül.

Az alapértelmezett működés olvasási és az üzenet törlése a fogadási művelet részeként a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem feldolgozása abban az esetben egy üzenet elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

A **isPeekLock** paraméter értéke **Igaz**, ha a fogadás válik, egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás.
Után az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra), a másodszintű fogadási folyamat befejezése **deleteMessage** metódus meghívása, valamint képzések paraméterként törlődik az üzenetet. A **deleteMessage** módszer megjelölni az üzenetet az éppen, és a eltávolítása abból az előfizetést.

Az alábbi példa bemutatja, hogyan üzenetek fogadható és **receiveSubscriptionMessage**használatával végzi el. A példa először kap töröl egy üzenetet a "LowMessages" előfizetésből, és kattintson a **isPeekLock** készletével igaz "HighMessages" előfizetésből az üzenetet kap. Kattintson az üzenet **deleteMessage**törlése:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja **unlockMessage** módszer a **ServiceBusService** objektum. Ennek hatására a szolgáltatás Bus zárolásának feloldása az üzenetet az előfizetésben, és tegye elérhetővé az azonos igénybe alkalmazásával vagy egy másik igénybe alkalmazás ismét kapott.

Is van egy üzenetet, az előfizetés belül zárolt társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenet a zárolási időkorlát lejár (például ha az alkalmazás összeomlik), majd a szolgáltatás Bus automatikusan feloldja az üzenetet, és újra kapott számára elérhetővé teszi azt.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **deleteMessage** módszer neve előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább után feldolgozása**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgozza, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása az alkalmazást, hogy kezelni az ismétlődő üzenet kézbesítését. Ez gyakran érhető el az üzenet, amely végig a kézbesítési kísérletek változatlan marad a **MessageId** tulajdonát felhasználni.

## <a name="delete-topics-and-subscriptions"></a>Témák és az előfizetés törlése

Témák és előfizetések állandó, és explicit módon törölni kell az [Azure klasszikus portál][] vagy keresztül programozás útján.
A következő példa bemutatja, hogyan lehet törlés a témakör nevű `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Témakör törlése is törölheti a regisztrált a témakör az előfizetése. Az előfizetések egymástól függetlenül is törlődnek. A következő példa bemutatja, hogyan nevű előfizetés törlése `HighMessages` a a `MyTopic` témakör:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus témakörök alapjait, kövesse az alábbi hivatkozásokra kattintva további.

-   [Sorok, a témakörök és az előfizetések][]megtekintése
-   [SqlFilter][]API hivatkozását.
-   Keresse fel az [Azure SDK csomópont][] tárházba GitHub.

  [Azure SDK csomóponthoz]: https://github.com/Azure/azure-sdk-for-node
  [Azure klasszikus portál]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [NODE.js felhőalapú szolgáltatás]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Hozzon létre és telepítsen egy Node.js alkalmazást az Azure webhely]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [NODE.js Felhőszolgáltatásba tárhely]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Webalkalmazás node.js adathordozós]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
