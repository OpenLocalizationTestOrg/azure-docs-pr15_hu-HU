<properties
    pageTitle="Azure függvények HTTP és webhook kötések |} Microsoft Azure"
    description="Megtudhatja, hogyan HTTP és webhook eseményindítók és kötések Azure függvények használata."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure függvények HTTP és webhook kötések

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogyan beállítása és a http- és webhook eseményindítók és Azure-függvényekkel kötések kód. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>a http- és webhook kötések Function.JSON

A *function.json* fájl itt tulajdonságaik vannak, amelyek a kérelem és a válasz vonatkoznak.

A HTTP-kérés tulajdonságait:

- `name`: Változó nevét a kód függvény a request objektum (vagy a kérelem szervezet Node.js függvények esetén).
- `type`: *HttpTrigger*kell beállítani.
- `direction`: Kell beállítani *a*. 
- `webHookType`: WebHook eseményindítók, érvényes értékei *github*, *tartalékidő*és *genericJson*. Állítsa a HTTP eseménykód, amely nem egy WebHook, üres karakterláncot ezt a tulajdonságot. További információt a WebHooks a következő részben [WebHook indítók](#webhook-triggers) .
- `authLevel`: Eseményindítók WebHook nem érvényes. Állítsa be a "függvény", "névtelen" legördülő az API kulcs követelmény, vagy az "felügyeleti" a fő API-kulcs megkövetelése API-ja kulcs megkövetelésére. További információt talál [API kulcsok](#apikeys) közül.

A HTTP-válasz tulajdonságait:

- `name`: Változó nevét a kód függvény az adott válaszok objektum.
- `type`: *Http*kell beállítani.
- `direction`: Meg kell *meg*. 
 
Példa *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>Eseményindítók WebHook

Az eseményindító WebHook, amely tartalmazza a következő funkciók WebHooks tervezett HTTP Eseménykód:

* Adott WebHook szolgáltatók (jelenleg GitHub és tartalékidő használható) a függvényeket futási idejű ellenőrzi a szolgáltató aláírást.
* A függvényeket futási idejű Node.js funkciók, itt helyett a request objektum összehívás törzsébe. Nincs különleges kezelési C# függvények, nem mivel szabályozhatja, hogy milyen áll rendelkezésre a paraméter típusa megadásával. Ha megad `HttpRequestMessage` a request objektum kap. Ha egy POCO típusának megadásához a függvényeket futási idejű próbálja elemezni az objektum tulajdonságai feltöltése az értekezlet-összehívást törzsében egy JSON-objektumot.
* Szeretne elindítani egy WebHook függvény a HTTP-kérés tartalmaznia kell egy API billentyűt. Ez a követelmény WebHook HTTP eseményindítók, nem kötelező.

Információ arról, hogy miként állíthat be egy GitHub WebHook olvassa el a [GitHub Fejlesztőeszközök - WebHooks létrehozása](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409)című témakört.

## <a name="url-to-trigger-the-function"></a>URL-CÍMÉT szeretne elindítani a függvény

Szeretne elindítani egy függvényt, a HTTP-kérelem egy URL-címet, a függvény app URL-címe és a függvény neve kombinációi elküldött:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API-kulcsok

Alapértelmezés szerint az API-ja kulcs szerepelnie kell a HTTP-kérelem szeretne elindítani egy HTTP vagy WebHook függvényt. A kulcs beépíthetők elnevezett lekérdezés karakterlánc típusú változóban `code`, vagy a beépíthetők egy `x-functions-key` HTTP-fejléc. Nem WebHook funkciók jelezheti, hogy az API-ja kulcs nincs szükség megadásával a `authLevel` tulajdonság "névtelen" a *function.json* fájlban.

A függvény alkalmazás a fájlrendszer *D:\home\data\Functions\secrets* mappájában található API kulcs értékeit.  A diaminta billentyűt, és a funkcióbillentyű a *host.json* fájlban vannak beállítva ebben a példában látható módon. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

A függvény billentyű *host.json* minden funkció használható, de nem indítja el letiltott függvény. A diaminta billentyűt minden funkció használható, és elindítja a függvényt akkor is, ha le van tiltva. A függvény csak a diaminta kulcs megadásával beállíthatja a `authLevel` "admin" tulajdonságot. 

Ha a *titkos kulcsok* mappában JSON fájl neve megegyezik egy függvény, az a `key` tulajdonság a fájlra mutató is használható a funkció, és a kulcs csak használata a függvény, amire hivatkozik. Például az API-ja kulcs nevű függvény `HttpTrigger` *HttpTrigger.json* a *titkos kulcsok* mappában van megadva. Lássunk egy példát:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Állít be egy WebHook eseményindító, ha nem osztom meg a fő billentyűt a WebHook szolgáltatónál. A függvény a WebHook csak működő termékkulccsal.  A diaminta billentyűt is használható indíthatja el minden függvény, még akkor is tiltható le a függvények.

## <a name="example-c-code-for-an-http-trigger-function"></a>Példa a C# kód egy HTTP eseményindító függvény 

A példa kódot keres egy `name` paramétert a lekérdezési karakterlánc vagy a HTTP-összehívás törzsében.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>Példa F # kód egy HTTP eseményindító függvény

A példa kódot keres egy `name` paramétert a lekérdezési karakterlánc vagy a HTTP-összehívás törzsében.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Szüksége lesz egy `project.json` hivatkozni szeretne NuGet használó fájl a `FSharp.Interop.Dynamic` és `Dynamitey` összeállítások jelennek meg:

```json
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
```

Ez lesz NuGet-ból a függőségek és fog hivatkozni a parancsfájl.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Példa Node.js kód egy HTTP eseményindító függvény 

Ez a példa kód keres egy `name` paramétert a lekérdezési karakterlánc vagy a HTTP-összehívás törzsében.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Példa a C# kód GitHub WebHook függvény 

Ez a példa kód naplózza GitHub probléma megjegyzéseket.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Példa F # kód GitHub WebHook függvény

Ez a példa kód naplózza GitHub probléma megjegyzéseket.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Példa Node.js kód GitHub WebHook függvény 

Ez a példa kód naplózza GitHub probléma megjegyzéseket.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
