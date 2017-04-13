<properties
    pageTitle="Azure függvények DocumentDB kötések |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure DocumentDB kötések Azure függvények használata."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
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
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-documentdb-bindings"></a>Azure függvények DocumentDB kötések

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogy konfigurálása és a kód Azure DocumentDB kötések Azure-függvényekkel. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB bemeneti kötése

Beviteli kötések töltse be a dokumentum egy DocumentDB különféle, és közvetlenül a kötelező át is. A dokumentumazonosító is alapján határozza meg, hogy a meghívott a függvény az eseményindító. A C# függvény a rekord módosításait automatikusan küld vissza a webhelycsoport esetén a függvény kijelentkezik a sikeres.

#### <a name="functionjson-for-documentdb-input-binding"></a>beviteli kötés DocumentDB Function.JSON

A *function.json* fájlt tartalmazza a következő tulajdonságokat:

- `name`: Változó nevét a kód függvény a dokumentum.
- `type`: "documentdb" értékre kell állítani.
- `databaseName`: A a dokumentumot tartalmazó adatbázis.
- `collectionName`: Gyűjtemény a dokumentumot tartalmazó.
- `id`: Azonosítója a dokumentum beolvasásához. Ez a tulajdonság támogatja kötések hasonlóan, mint "{queueTrigger}", amely a szöveges értékként a várólista üzenet fog használni a dokumentum azonosítójával.
- `connection`: Ez a karakterlánc-Alkalmazásbeállítással beállítása a végpont DocumentDB fiókjának kell lennie. Ha a Integrate lapon jelölje ki a fiókját, új alkalmazás beállítása létrejön meg, hogy mi a következő formában yourAccount_DOCUMENTDB néven. Ha az alkalmazás beállítása létrehozása manuálisan kell a tényleges csatlakozási karakterlánc kell tennie a következő formában AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- "irányba: *"a"*értékre kell állítani.

Példa *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB beviteli példa C# várólista eseményindítók
 
A fenti példa function.json használ, a bemeneti DocumentDB kötés beolvasni a dokumentumot, amely megfelel a várólista üzenet karakterlánc azonosítójú és adja át a "dokumentum" paramétert. Ha a dokumentum nem található, a "dokumentum" paramétert üres lesz. A dokumentum majd frissül az új szöveges értéket tartalmazó kijelentkezik a függvény.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>F # várólista eseménykód Azure DocumentDB beviteli kódot példa

A fenti példa function.json használ, a bemeneti DocumentDB kötés beolvasni a dokumentumot, amely megfelel a várólista üzenet karakterlánc azonosítójú és adja át a "dokumentum" paramétert. Ha a dokumentum nem található, a "dokumentum" paramétert üres lesz. A dokumentum majd frissül az új szöveges értéket tartalmazó kijelentkezik a függvény.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Szüksége lesz egy `project.json` fájlt, NuGet használatával adja meg a `FSharp.Interop.Dynamic` és `Dynamitey` csomagok csomag függőségek, így:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

Ez lesz NuGet-ból a függőségek és fog hivatkozni a parancsfájl.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB beviteli példa Node.js várólista eseményindítók
 
A fenti példa function.json használ, a DocumentDB beviteli kötés fog beolvasni a dokumentumot, amely megfelel a várólista üzenet karakterlánc azonosítójú és adják át, hogy a `documentIn` kötelező tulajdonsága. Node.js funkciók, a frissített dokumentumok nem kerülnek vissza a webhelycsoport. Azonban lehet átadni a bemeneti kötés közvetlenül egy kötelező DocumentDB kimenet elnevezett `documentOut` frissítések támogatását. Ebben a példában kód frissíti a bemeneti dokumentum tulajdonságlapján a szöveget, és állítja be ezt a kimeneti dokumentum.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB kimeneti kötések

A függvények írhat JSON dokumentumok használata a **Azure DocumentDB dokumentum** Azure DocumentDB adatbázishoz kimeneti kötése. További információt a Azure DocumentDB tekintse át a [DocumentDB – bevezetés](../documentdb/documentdb-introduction.md) és az [első lépések oktatóprogram](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>a DocumentDB Function.JSON kimeneti kötése

A function.json fájlt tartalmazza a következő tulajdonságokat:

- `name`: Változó nevét a kód függvény az új dokumentum.
- `type`: *"documentdb"*értékre kell állítani.
- `databaseName`: A hol hozható létre az új dokumentum a webhelycsoport tartalmazó adatbázis.
- `collectionName`: A webhelycsoport, ahol hozható létre az új dokumentum.
- `createIfNotExists`: Ez a logikai érték jelzi, hogy a webhelycsoport létrejönnek, ha még nem létezik. Az alapértelmezett érték *hamis*. Az OK: az új a webhelycsoportok fenntartott átviteli, amely mellett a következmények árak létre. További információra kíváncsi keresse fel az [ár, oldal](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Ez a karakterlánc kell lennie a **Alkalmazásbeállítással** állítsa az DocumentDB fiókjának végpontot. A **Integrate** lapon jelölje ki a fiókját, ha egy új alkalmazás beállítás létrejön meg, hogy mi a következő formában néven `yourAccount_DOCUMENTDB`. Kell létrehoznia az alkalmazás beállítása, ha a tényleges kapcsolattal kell tennie a következő formában `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: *","*értékre kell állítani. 
 
Példa function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB kimeneti példa Node.js várólista eseményindítók

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

A kimenet dokumentumot:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>F # várólista eseménykód Azure DocumentDB kimeneti kód példa

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB kimeneti példa C# várólista eseményindítók


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB kimeneti kód példa beállítás fájl neve

Ha szeretné beállítani a dokumentum nevét a függvény, csak adja meg a `id` értéket.  Ha például egy alkalmazott tartalom JSON eltávolítva alatt az alábbihoz hasonló várakozási:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Az eseményindító várólista függvényben használható a következő C# kód: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Vagy a egyenértékű F # kód:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Példa kimenet:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
