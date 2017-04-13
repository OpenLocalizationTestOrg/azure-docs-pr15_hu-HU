<properties
    pageTitle="Azure függvények NodeJS Fejlesztői segédlet |} Microsoft Azure"
    description="Megtudhatja, hogyan NodeJS használatával Azure függvények fejlesztését."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure függvények funkciók, esemény feldolgozása, webhooks, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure függvények NodeJS Fejlesztői segédlet

> [AZURE.SELECTOR]
- [C# parancsfájl](../articles/azure-functions/functions-reference-csharp.md)
- [F # parancsfájl](../articles/azure-functions/functions-reference-fsharp.md)
- [NODE.js](../articles/azure-functions/functions-reference-node.md)

Azure függvények csomópontot/JavaScript tapasztalnak megkönnyíti a függvény, amely átadott exportálása egy `context` objektum a futtatókörnyezet kommunikál, és fogadására és kötések keresztül adatokat küldi.

Ez a cikk tartalma feltételezi, hogy az [Azure függvények Fejlesztői segédlet](functions-reference.md)már olvasott.

## <a name="exporting-a-function"></a>Exportálás függvény

JavaScript-függvények exportálnia kell az egyetlen `function` keresztül `module.exports` keresse meg a függvényt, és indítsa el a futtatási számára. Ez a függvény mindig szerepelnie kell egy `context` objektumot.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

A kötések `direction === "in"` függvény argumentumaként, ami azt jelenti, használható mentén átadott [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) dinamikusan kezelése az új ráfordítások (például használatával `arguments.length` az összes a bemenetben ismétlés). Ez a funkció megegyezik nagyon hasznos, ha csak az eseményindító, nincs további bemeneti adatok alapján, az eseményindító adatok nélkül hivatkozó előre elérheti a `context` objektumot.

Az argumentumok vannak mindig mentén a függvénynek átadott abban a sorrendben következnek be a *function.json*, még akkor is, ha Ön nem adja őket a exportnak utasítás. Tegyük fel például, hogy van- `function(context, a, b)` , és módosíthatja, hogy `function(context, a)`, akkor is elérjék a értékének `b` hivatkozva függvény kódban `arguments[3]`.

Minden kötés, függetlenül az irány is átadott mentén a `context` objektum (lásd alább). 

## <a name="context-object"></a>helyi objektum

A futtatókörnyezet használ egy `context` objektumra, és a függvény az adatok adják át, és lehetővé teszi, hogy a futtatókörnyezet kommunikálni.

A helyi objektum mindig a függvény első paraméter, és mindig szerepeljenek mert módszerek például `context.done` és `context.log` amelyek hibásan használja a futtatókörnyezet szükséges. Tetszés szerinti nevet adhat az objektum bármilyen (azaz `ctx` vagy `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>Context.Bindings

A `context.bindings` objektum gyűjt a bemeneti és kimeneti adatokat. Az adatok bekerülnek az alakzatot a `context.bindings` keresztül az objektum a `name` a kötelező tulajdonsága. Például a következő kötés definíciót *function.json*megkezdésére, érheti el a sor keresztül tartalmának `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

A `context.done` függvény közli a futtatókörnyezet, hogy elkészült a futó. Amikor elkészült, a függvény; a hívni fontos Ha nem, a futtatókörnyezet még soha nem tudja, hogy befejeződött-e a függvény. 

A `context.done` függvény lehetővé teszi átadni vissza egy, a felhasználó által definiált hiba a futtatókörnyezet, valamint a tulajdonság között tulajdonságait, amely felülírja az a tulajdonságokat kapcsolatban a `context.bindings` objektumot.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>Context.log(Message)

A `context.log` metódus lehetővé teszi a kimeneti naplózás célokra közös összefüggésben napló kimutatásokat. Ha `console.log`, az üzenetek csak jeleníti meg folyamat a naplózási szint, amelyek nem hasznosnak.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

A `context.log` módszer támogatja, amely támogatja a csomópont [util.format módszer](https://nodejs.org/api/util.html#util_util_format_format) ugyanazt a paraméter a formátumot. Igen például kód jelennek meg:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

írható jelennek meg:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP indítók: context.req és context.res

HTTP eseményindítók, amíg mivel az ilyen közös mintát használandó `req` és `res` HTTP kérések és válaszok az objektumok azt úgy döntött, hogy könnyen elérheti azokat a helyi objektum helyett kényszerítése szeretne használni, a teljes `context.bindings.name` mintát.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Csomópont verziója és csomag kezelése

A csomópont-verzió zárolta a `5.9.1`. Azt éppen vizsgálja meg a további verziók hozzáadása támogatása, és így konfigurálható.

A függvény által *package.json* fájl feltöltésekor mappába a függvény a függvény alkalmazás fájlrendszerben csomagok is. Fájl feltöltése a megjelenő utasításokat, olvassa el az [Azure függvények developer reference témakörének](functions-reference.md#fileupdate) **függvény app-fájlokkal frissítése** szakaszát. 

Is használhatja `npm install` a függvény alkalmazás SCM (Kudu) parancssor a:

1. Nyissa meg azt: `https://<function_app_name>.scm.azurewebsites.net`.

2. Kattintson a **konzol hibakeresési > CMD**.

3. Nyissa meg azt `D:\home\site\wwwroot\<function_name>`.

4. Futtatása `npm install`.

A szükséges csomagok telepítése után importálja őket a függvény a szokásos módon (azaz keresztül `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>A környezeti változók

Változó vagy az alkalmazás beállítás értékét, használja `process.env`, a következő példa látható módon:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Géppel/CoffeeScript támogatás

Nincs még összeállításához automatikus-géppel/CoffeeScript keresztül a runtime úgy szeretné, hogy az összes olyan közvetlen támogatási kell telepítési időben a runtime kívül kell kezelni. 

## <a name="next-steps"></a>Következő lépések

További információt az alábbi forrásokban talál:

* [Azure függvények Fejlesztői segédlet](functions-reference.md)
* [Azure függvények C# Fejlesztői segédlet](functions-reference-csharp.md)
* [Azure függvények F # Fejlesztői segédlet](functions-reference-fsharp.md)
* [Azure függvények eseményindítók és kötések](functions-triggers-bindings.md)
