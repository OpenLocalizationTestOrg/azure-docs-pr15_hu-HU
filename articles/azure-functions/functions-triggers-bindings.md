<properties
    pageTitle="Azure függvények eseményindítók és kötések |} Microsoft Azure"
    description="Megtudhatja, hogyan eseményindítók és kötések Azure függvények használata."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure függvények funkciók, esemény feldolgozása, webhooks, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure függvények eseményindítók és kötések Fejlesztői segédlet


Ez a témakör általános hivatkozás eseményindítók és kötések. Egyes speciális kötés funkciók és diagramtípusokat kötés által támogatott képletszintaxisát tartalmazza.  

Ha egy bizonyos típusú eseményindító vagy kötelező kódolási és konfigurálása körül részletes információt keres, kattintson az eseményindító vagy az alább felsorolt kötések egyik érdemes:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Ezek a cikkek feltételezik, hogy, hogy elolvasta az [Azure függvények Fejlesztői segédlet](functions-reference.md)és [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)vagy [Node.js](functions-reference-node.md) Fejlesztőeszközök útmutatót.



## <a name="overview"></a>– Áttekintés

Eseményindítók olyan esemény válaszok mezőkóddal az egyéni kódot. Lehetővé teszik, hogy az Azure platform vízszintesen vagy a helyszíni környezetbe, események válaszolni. Kötések jelenítik meg a szükséges, a kód kívánt eseményindító vagy társított bemeneti és kimeneti adatok való kapcsolódásra használt metaadatai.

Az Azure függvény alkalmazást integrálhatja a különböző kötések jobb képet kapni, tanulmányozza az alábbi táblázatban.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Eseményindítók megértéséhez, és kötések az általános tegyük fel, hajtsa végre az új elem kihagyott be egy Azure tárolás várólista folyamat kód. Azure függvények támogatásához Azure várólista eseménykód biztosít. Lenne szüksége, a következő információkat a várólista figyelni:
 
- A sor csak tartalmazó tárterület-fiók.
- A várakozási neve.
- A kód szeretne hivatkozni kell az új elemet, amely várakozási eltávolítva használó változóinak nevéből.  
 
Az eseményindító várólista kötés ezt az információt az Azure függvényt tartalmaz. A *function.json* fájl minden függvény az összes kapcsolódó kötések tartalmazza.  Íme egy példa *function.json* tartalmazó várólista kötés elindítani. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

A kód elküldheti a különböző típusú kimeneti attól függően, hogy hogyan az új elem feldolgozása. Például érdemes lehet új bejegyzés írása Azure tároló táblához.  Ehhez beállíthatja a kimeneti kötés Azure tároló táblához. Íme egy példa *function.json* tároló tábla kimeneti kötés, amely az eseményindító várólista felhasználható tartalmazza. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


A következő C# függvény egy új elem alatt kihagyott várakozási válaszol, és Azure tároló táblába új felhasználói bejegyzés írása.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

További példák kódot és Azure tárolási típusok által támogatott kapcsolatos információk relevancia szerinti szűkítése című témakörben talál [Azure függvények eseményindítók és Azure tárolására kötések](functions-bindings-storage.md).


A speciális kötés funkciók az Azure-portálon, kattintson a függvény a **Integrate** lapon a **Speciális szerkesztő** lehetőséget. A speciális szerkesztőben lehetővé teszi, hogy a *function.json* közvetlenül a portálon a szerkesztését.

## <a name="dynamic-parameter-binding"></a>Dinamikus paraméter kötés 

A kimenet kötés tulajdonságainak beállítása a statikus konfiguráció helyett, beállításainak dinamikusan kötni az adatokat, amely az eseményindító bemeneti kötés része. Példa fontolja meg, ahol új rendelések egy Azure tárolás várólista dolgozza fel. Minden új várólista elem a következő tulajdonságok legalább tartalmazó JSON karakterlánc:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Érdemes lehet a Twilio fiókjával, mint a frissítés, hogy érkezett-e a sorrend SMS szöveg üzenet küldése az ügyfélnek.  Beállíthatja a `body` és `to` a Twilio területén kimeneti dinamikusan kötni szeretné kötése a `name` és `mobileNumber` következőképpen voltak, amely a bemeneti részét.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
A függvény kód csak összegyűjtötte az alábbi képlettel történik a kimeneti paraméter inicializálni. Végrehajtás során a kimeneti tulajdonságok fog kötni a kívánt beviteli adatokat.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

NODE.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Véletlen GUID

Azure függvények véletlen GUID létrehozásához a kötések a szintaxist tartalmaz. Kötés szintaxisa a következő kimeneti írni egy új BLOB-tároló Azure tárolóban egy egyedi nevet a: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Egy egyetlen eredményt ad eredményül adása

Az olyan esetekben, ahol a függvény kód értéket ad vissza egy egyetlen eredményt ad, egy névvel ellátott kimeneti kötés használható `$return` kódban több természetes függvény aláírás megtartani. Ez csak akkor használható, amelyek támogatják a visszatérési érték (C#, Node.js, F #) nyelven. A kötelező a következő blob kimeneti kötés, amely az eseményindító várólista hasonló lenne.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


A következő C# kód adja eredményül a kimenet további természetesen nélkül, egy `out` függvény aláírás paraméter.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Ezt a megközelítést a Node.js van igazolni alatt.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # lenti példát.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Ez is kínál több kimeneti paraméter egy egyetlen kimenet kijelölésével `$return`.


## <a name="route-support"></a>Útvonal támogatás

Alapértelmezés szerint HTTP eseményindító, vagy WebHook, függvény létrehozásakor a függvényt ki az űrlap útvonal címmel rendelkező:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Testre szabhatja a választható használatával, ez az útvonal `route` tulajdonság a HTTP hatására a bemeneti kötés. Példaként a következő *function.json* fájl definiálja egy `route` HTTP eseménykód tulajdonságait:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

A fenti konfiguráció használata esetén a függvény ettől kezdve címmel rendelkező, az alábbi útvonalat az eredeti útvonalon helyett.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Ez a két paraméterrel támogatja a címet, a kód függvény lehetővé teszi `category` és `id`. A paraméterrel bármely [Webes API útvonal kényszer](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) is használhatja. A következő C# függvény kódot él mindkét paramétert.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Az alábbiakban Node.js függvény kódot, és használja az útvonal paramétereket.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Alapértelmezés szerint minden függvény útvonal elé *api*-val. Is testre szabhatja, illetve eltávolítása az előtag használata a `http.routePrefix` tulajdonság a *host.json* fájljait. Az alábbi példa az *api* útvonal előtag eltávolítja az üres karakterlánc az előtag az *host.json* fájl használatával.

    {
      "http": {
        "routePrefix": ""
      }
    }

Részletes információkat a *host.json* fájlt, a függvény című témakörben talál [függvény alkalmazás frissítéséhez fájlok](functions-reference.md#fileupdate)frissítése. 

Ebben a konfigurációban ad hozzá, a függvény már címmel rendelkező, az alábbi útvonalat a:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Egyéb beállíthatja a *host.json* fájl tulajdonságainak további tudnivalókért lásd [host.json hivatkozást](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Következő lépések

További információt az alábbi forrásokban talál:

* [Tesztelés függvény](functions-test-a-function.md)
* [Méretezés függvény](functions-scale.md)
