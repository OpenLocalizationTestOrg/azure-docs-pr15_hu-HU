<properties
    pageTitle="Azure függvények esemény központi kötések |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure esemény központi kötések Azure függvények használata."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure működik, függvények, esemény feldolgozása, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure függvények esemény központi kötések

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogy konfigurálása és a kód [Azure esemény központi](../event-hubs/event-hubs-overview.md) kötések Azure függvények. Azure funkciók támogatása Azure esemény hubok eseményindítók és a kimeneti kötések.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Ha még kezdő az Azure esemény hubok, a az [Azure esemény központi – áttekintés](../event-hubs/event-hubs-overview.md)című témakörben találhat.

## <a name="azure-event-hub-trigger-binding"></a>Azure esemény központi kötés elindítása

Azure esemény központi eseménykód egy esemény központi esemény adatfolyam küldött esemény megválaszolása használható. Az esemény-központban állíthatja be az eseményindító kötést olvasási hozzáférést kell rendelkeznie.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>az esemény központi Function.JSON kötés elindítása

Az Azure esemény központi eseménykód *function.json* fájl adja meg a következő tulajdonságokat:

- `type`: *EventHubTrigger*kell beállítani.
- `name`: A változó nevét a kód függvény az esemény központi üzenet. 
- `direction`: Kell beállítani *a*. 
- `path`: Az esemény-központban a neve.
- `consumerGroup`: Ez egy választható tulajdonság beállítja az előfizetés a hubon események használt [fogyasztói csoportot](../event-hubs-overview.md#consumer-groups) . Ha nincs megadva, az `$Default` fogyasztói csoport szolgál. 
- `connection`: A névtér az esemény-központban található a kapcsolati karakterláncot tartalmazó-app beállítása a neve. A névtér, nem magának esemény központi **Kapcsolatadatainak** gombra kattintva másolja a vágólapra a kapcsolati karakterlánc.  A kapcsolati karakterlánc kell legalább elolvasta aktiválása az eseményindító engedélyeket.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure esemény központi eseményindító C# példa
 
A fenti példa function.json használ, az üzenet törzsében rendszer naplózza a C# függvény kód használatával az alábbi:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Azure esemény központi eseményindító F # példa

A fenti példa function.json használ, az üzenet törzsében naplózandó F # függvény kód használatával az alábbi:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure esemény központi eseményindító Node.js példa
 
A fenti példa function.json használ, az esemény üzenettörzsébe rendszer naplózza a Node.js függvény az alábbi kód használatával:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure esemény központi kimeneti kötése

Az Azure esemény központi kimeneti kötés események írni egy esemény központi esemény adatfolyam használják. Események írni egy esemény hubhoz Küldés engedéllyel kell rendelkeznie. 

#### <a name="functionjson-for-event-hub-output-binding"></a>az esemény központi Function.JSON kimeneti kötése

A *function.json* fájlt egy Azure esemény hubhoz kimeneti kötés, adja meg a következő tulajdonságokat:

- `type`: *EventHub*kell beállítani.
- `name`: A változó nevét a kód függvény az esemény központi üzenet. 
- `path`: Az esemény-központban a neve.
- `connection`: A névtér az esemény-központban található a kapcsolati karakterláncot tartalmazó-app beállítása a neve. A névtér, nem magának esemény központi **Kapcsolatadatainak** gombra kattintva másolja a vágólapra a kapcsolati karakterlánc.  A kapcsolati karakterláncot az üzenetet küldeni az esemény központi adatfolyam küldés engedélyekkel kell rendelkezni.
- `direction`: Meg kell *meg*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure esemény központi C# kód példa a kimeneti kötés
 
A következő C# példa függvény kódot bemutatja, hogy egy esemény központi esemény adatfolyam esemény írása. Ez az esemény-központban kimeneti kötés, fent látható példa jelöli C# időzítő eseményindító érvényes.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure esemény központi F # kód példa a kimeneti kötés

A következő F # példa függvény kódot bemutatja, hogy egy esemény központi esemény adatfolyam esemény írása. Ez az esemény-központban kimeneti kötés, fent látható példa jelöli C# időzítő eseményindító érvényes.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Kimeneti kötés Azure esemény központi Node.js kód példa
 
A következő Node.js példa függvény kódot bemutatja, hogy egy esemény központi esemény adatfolyam esemény írása. Ez az esemény-központban kimeneti kötés, fent látható példa jelöli Node.js időzítő eseményindító érvényes.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
