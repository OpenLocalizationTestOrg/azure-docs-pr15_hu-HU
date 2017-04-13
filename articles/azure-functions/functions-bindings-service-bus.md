<properties
    pageTitle="Azure függvények szolgáltatás Bus eseményindítók és kötések |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure Service Bus eseményindítók és kötések Azure függvények használata."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkciók esemény feldolgozása, dinamikus számítási, kiszolgáló nélküli architektúráját, függvények"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure függvények szolgáltatás Bus eseményindítók és a sorok és témakörök kötések

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogy beállításáról és kód Azure Service Bus eseményindítók és Azure-függvényekkel kötések. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Szolgáltatás Bus várólista vagy a témakör a kiváltó ok mező

#### <a name="functionjson"></a>Function.JSON

A *function.json* fájl adja meg a következő tulajdonságokat.

- `name`: A változó nevét a kód függvény a sorban vagy témakör vagy a várólista vagy témakör üzenetet. 
- `queueName`: A várólista elindítása csak, a sor lekérdezik nevét.
- `topicName`: A témakör az elindítani csak a témakör a lekérdezik nevét.
- `subscriptionName`: A témakör az elindítani csak, az előfizetése nevére.
- `connection`: Egy szolgáltatás Bus kapcsolati karakterláncot tartalmazó app beállítása a neve. A kapcsolati karakterlánc nem kizárólag egy adott várólista vagy témakör szolgáltatás Bus névtér kell lennie. Ha a kapcsolati karakterlánc jogok kezelése nem rendelkezik, adja meg a `accessRights` tulajdonság. Ha hagyja `connection` üres, a kiváltó ok mező vagy kötés fog működni, amelyet AzureWebJobsServiceBus alkalmazás beállítása adott függvény alkalmazáshoz alapértelmezett szolgáltatás Bus kapcsolati karakterlánccal.
- `accessRights`: Itt adhatja meg az access a kapcsolati karakterlánc jogosultságokat. Alapértelmezett értéke `manage`. Állítsa `listen` használata a kapcsolati karakterlánc, amely nem nyújt az engedélyek kezelése. A függvények futtatókörnyezet előfordulhat, hogy próbálja meg és nem igénylő műveleteket egyébként jogok kezelése.
- `type`: *ServiceBusTrigger*kell beállítani.
- `direction`: Kell beállítani *a*. 

Példa *function.json* várólista szolgáltatást Bus eseményindító:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# kód példa, hogy a folyamatok várólista szolgáltatást Bus üzenet

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # kód példa, hogy a folyamatok várólista szolgáltatást Bus üzenet

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>NODE.js példa, hogy a folyamatok várólista szolgáltatást Bus üzenet

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Támogatott fájltípusok

A szolgáltatás Bus várólista üzenetet is deszerializálható az alábbiak bármelyikét:

* Objektum (a JSON)
* karakterlánc
* tömb bájt 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock viselkedése

A függvényeket futási idejű az üzenetet kap `PeekLock` mód és a hívások `Complete` esetén a függvény sikeresen befejeződött üzenet vagy hívások `Abandon` , ha a parancs nem működik. Ha hosszabb fut, a függvény a `PeekLock` időtúllépés, a zárolás megújítása automatikusan be van állítva.

#### <a id="sbpoison"></a>Elhalt üzenetek kezelése

A saját elhalt üzenetek kezelésére, amely nem ellenőrzött és Azure függvények konfigurációs vagy kódot konfigurált szolgáltatás Bus hajtja végre. 

#### <a id="sbsinglethread"></a>Egyetlen threading

Alapértelmezés szerint a függvényeket futási idejű dolgozza fel több üzenetek egyidejűleg. Irányítsa át a feldolgozása a egyszerre csak egy adott sorba vagy a témakör üzenet futtatókörnyezet, állítsa `serviceBus.maxConcurrrentCalls` a *host.json* fájlban 1. A *host.json* fájllal kapcsolatos további tudnivalókért lásd: a [Mappastruktúra](functions-reference.md#folder-structure) a Fejlesztői segédlet cikk és [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) a tárházba WebJobs.Script wiki.

## <a id="sboutput"></a>Szolgáltatás Bus várólista vagy a témakör a kimeneti kötése

#### <a name="functionjson"></a>Function.JSON

A *function.json* fájl adja meg a következő tulajdonságokat.

- `name`: A változó nevét a kód függvény a várólista vagy várólista üzenetet. 
- `queueName`: A várólista elindítása csak, a sor lekérdezik nevét.
- `topicName`: A témakör az elindítani csak a témakör a lekérdezik nevét.
- `subscriptionName`: A témakör az elindítani csak, az előfizetése nevére.
- `connection`: Ugyanaz, mint a szolgáltatás Bus indítja el.
- `accessRights`: Itt adhatja meg az access a kapcsolati karakterlánc jogosultságokat. Alapértelmezett értéke `manage`. Állítsa `send` használata a kapcsolati karakterlánc, amely nem nyújt az engedélyek kezelése. Egyéb esetben a függvények futtatókörnyezet próbálja meg és nem igénylő műveleteket kezelése jogok, például a sorok létrehozása.
- `type`: *ServiceBus*kell beállítani.
- `direction`: Meg kell *meg*. 

Példa *function.json* időzítő az eseményindító a szolgáltatás Bus üzenetek írni:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Támogatott fájltípusok

Azure függvények várólista szolgáltatást Bus üzenet hozhat létre az alábbiak közül.

* Objektum (mindig létrehoz egy JSON üzenetet, null objektumot hoz létre az üzenetet, ha az értéke null a függvény befejeződésekor)
* karakterlánc (létrehoz egy üzenetet, ha az érték nem null a függvény befejeződésekor)
* Byte tömb (karakterlánc működése) 
* `BrokeredMessage`(C#, ugyanúgy működik, mint a karakterlánc)

C# függvény több üzenetet készítéséhez használható `ICollector<T>` vagy `IAsyncCollector<T>`. Üzenet hoz létre, amikor felhívja a `Add` módot.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# kód példák, amelyek szolgáltatás Bus üzenetek létrehozása

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # kód példa létrehoz egy szolgáltatás Bus várólista üzenetet

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>NODE.js példa, hogy a szolgáltatás Bus várólista üzenet létrehozása

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
