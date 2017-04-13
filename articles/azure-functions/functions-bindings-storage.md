<properties
    pageTitle="Azure függvények eseményindítók és Azure tárolásra kötések |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure tároló eseményindítók és kötések Azure függvények használata."
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
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure függvények eseményindítók és kötések Azure tárolására

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk ismerteti, hogy beállításáról és kód Azure tároló eseményindítók és Azure-függvényekkel kötések. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure tároló várólista eseményindító

#### <a name="functionjson-for-storage-queue-trigger"></a>tárterület várólista eseményindító Function.JSON

A *function.json* fájl adja meg a következő tulajdonságokat.

- `name`: A változó nevét a kód függvény a sor vagy az várólista üzenetet. 
- `queueName`: A sor lekérdezik a nevét. Várólista elnevezési szabályokat olvassa el a [sorok elnevezéséről és a metaadatokat](https://msdn.microsoft.com/library/dd179349.aspx)című témakört.
- `connection`: Neve, amely tartalmazza a kapcsolati karakterlánc tárterület-app beállítása. Ha hagyja `connection` üres, az eseményindító fog működni, amelyet AzureWebJobsStorage alkalmazás beállítása adott függvény alkalmazáshoz alapértelmezett tároló kapcsolati karakterlánccal.
- `type`: *QueueTrigger*kell beállítani.
- `direction`: Kell beállítani *a*. 

Példa *function.json* tároló várólista eseményindító:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Eseményindító várólista támogatott fájltípusok

A várakozási üzenetet is deszerializálható az alábbiak bármelyikét:

* Objektum (a JSON)
* Karakterlánc
* Tömb bájt 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>A várakozási eseményindító metaadatok

A függvény ezek változó nevük segítségével elérheti várólista metaadatok:

* expirationTime
* insertionTime
* nextVisibleTime
* azonosító
* popReceipt
* dequeueCount
* queueTrigger (úgy is várólista üzenet szövegét karakterláncként adja vissza)

A C# kód példa beolvassa, és a naplók várólista metaadatok:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Poison üzenetek kezelése

Üzenetek, amelynek a tartalmát okoz meghiúsító függvény neve *poison üzeneteket*. Ha a függvény nem sikerül, a várólista üzenet a program nem törli, és végül van felvett ismét meg kell ismételni a ciklus okoz. A SDK automatikusan megszakíthatja a ciklus iterációk száma korlátozott számú után, illetve végezheti el azt manuálisan.

A SDK visszahívja függvény legfeljebb 5 megszorzása várólista üzenet feldolgozása. Ha nem sikerül az ötödik próbálja meg, az üzenet poison várólista kerül.

A poison várólista nevű *{originalqueuename}*-poison. Írhat naplózás őket, vagy elküldheti a értesítést, hogy kézi figyelmet poison sorból folyamat során függvény van szükség. 

Elhalt üzenetek manuálisan kezelni szeretné, ha kattint, jelölje be a hányszor üzenet felvett feldolgozásra `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure tároló várólista kimeneti kötése

#### <a name="functionjson-for-storage-queue-output-binding"></a>tárterület várólista Function.JSON kimeneti kötése

A *function.json* fájl adja meg a következő tulajdonságokat.

- `name`: A változó nevét a kód függvény a sor vagy az várólista üzenetet. 
- `queueName`: A sor a nevét. Várólista elnevezési szabályokat olvassa el a [sorok elnevezéséről és a metaadatokat](https://msdn.microsoft.com/library/dd179349.aspx)című témakört.
- `connection`: Neve, amely tartalmazza a kapcsolati karakterlánc tárterület-app beállítása. Ha hagyja `connection` üres, az eseményindító fog működni, amelyet AzureWebJobsStorage alkalmazás beállítása adott függvény alkalmazáshoz alapértelmezett tároló kapcsolati karakterlánccal.
- `type`: *A várakozási*kell beállítani.
- `direction`: Meg kell *meg*. 

Példa *function.json* tároló várólista kimeneti kötés, használja az eseményindító várólista és várólista üzenet írása:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Várólista kimeneti kötés, támogatott fájltípusok

A `queue` kötés is szerializálni a következő típusú várólista üzenetre:

* Objektum (`out T` C# létrehoz egy üzenetet az null objektumot, ha a paraméter értéke null, a függvény befejeződésekor)
* Karakterlánc (`out string` C# hoz létre az várólista üzenetet, ha a paraméter értéke nem null a függvény befejeződésekor)
* Byte tömb (`out byte[]` C# működés karakterlánc) 
* `out CloudQueueMessage`(C#, ugyanúgy működik, mint a karakterlánc) 

A C# is kell kötni `ICollector<T>` vagy `IAsyncCollector<T>` hol `T` támogatott fájltípusok egyike.

#### <a name="queue-output-binding-code-examples"></a>A várakozási kimeneti kötés kód példák

A C# kód példa minden bemeneti várakozási üzenet egyetlen kimeneti várólista üzenet írása.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

A C# kód példa több üzenetet ír használatával `ICollector<T>` (használata `IAsyncCollector<T>` egy aszinkron függvény):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Tárterület Azure blob-eseményindító

#### <a name="functionjson-for-storage-blob-trigger"></a>tárterület blob eseményindító Function.JSON

A *function.json* fájl adja meg a következő tulajdonságokat.

- `name`: A kód függvény használható az blob változóinak nevéből. 
- `path`: Görbévé, amely meghatározza a Lync-tároló, és tetszés szerint blob neve mintát.
- `connection`: Neve, amely tartalmazza a kapcsolati karakterlánc tárterület-app beállítása. Ha hagyja `connection` üres, az eseményindító fog működni, amelyet AzureWebJobsStorage alkalmazás beállítása adott függvény alkalmazáshoz alapértelmezett tároló kapcsolati karakterlánccal.
- `type`: *BlobTrigger*kell beállítani.
- `direction`: Kell beállítani *a*.

Példa *function.json* , amely figyeli a minták-workitems tároló adott BLOB-tároló blob eseményindító:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Eseményindító BLOB támogatott fájltípusok

A blob is deszerializálható a következő típusú a csomóponton és C# funkciók bármelyikét:

* Objektum (a JSON)
* Karakterlánc

A C# függvények is kell kötni az alábbiak egyikét:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Más típusú által [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

#### <a name="blob-trigger-c-code-example"></a>BLOB eseményindító C# kód példa

A C# kód példa naplózza a megfigyelt tároló hozzáadott minden blob tartalmát.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>BLOB eseményindító neve mintázatok

A minta neve blob adhatja meg a `path` tulajdonság. Példa:

```json
"path": "input/original-{name}",
```

Az elérési út megkereshető az *Eredeti-Blob1.txt* a *bemeneti* tároló és értékének nevű blob a `name` függvény kódban változó lenne `Blob1`.

Lássunk még egy példát:

```json
"path": "input/{blobname}.{blobextension}",
```

Az elérési út volna is megtalálhatók az *Eredeti-Blob1.txt*és értékének nevű blob a `blobname` és `blobextension` függvény kód változók lenne *Eredeti-Blob1* és *txt*.

A függvény a rögzített értékű kiterjesztéssel mintát megadásával kiváltó BLOB típusú korlátozhatja. Ha a `path` */minták / {név} .png*, hogy csak *.png* BLOB-tárolóban *minták* elindítja a függvény.

Kell neve minta megadása blob neveket a kapcsos zárójelek között a nevét, ha duplán a kapcsos zárójeleket. Ha meg szeretné keresni BLOB a *képek* tárolóban ilyen nevű például:

        {20140101}-soundfile.mp3

Ezen a a `path` tulajdonság:

        images/{{20140101}}-{name}

A példában a `name` változó érték *soundfile.mp3*lesz. 

#### <a name="blob-receipts"></a>BLOB visszaigazolások

Az Azure függvényeket futási idejű meggyőződik arról, hogy a funkció nem blob aktiválása többször kap-e hívni egy új vagy frissített blob. Mindezt *blob visszaigazolások* fenntartása érdekében határozza meg, ha egy adott blob-verzió megtörtént feldolgozása.

*Azure-webjobs-hosts* a AzureWebJobsStorage kapcsolati karakterláncban megadott Azure tároló fiók nevű tároló BLOB visszaigazolások tárolja. Blob visszaigazolását rendelkezik az alábbi adatokat:

* A függvény, amely hívták meg az a blob ("*{függvénynevet alkalmazás}*. Funkciók. *{függvénynevet}*", például:"functionsf74b96f7. Functions.CopyBlob")
* A tároló neve
* A blob típusa ("BlockBlob" vagy "PageBlob")
* A blob neve
* A ETag (egy blob verziójának azonosítója, például: "0x8D1DC6E70A277EF")

Ha szeretne blob újrafeldolgozása kötelező, kézzel is törölheti az, hogy a blob-blob-visszaigazolás az *azure-webjobs-hosts* tárolóból.

#### <a name="handling-poison-blobs"></a>Poison BLOB kezelése

Ha nem sikerül blob eseményindító függvény, a SDK csomagjában talál hívások rá újra, abban az esetben, ha a hiba miatt nem tranziens hiba. A blob tartalmát okozza a hibát, ha a parancs nem működik, minden alkalommal, amikor megkísérli a blob feldolgozása. Alapértelmezés szerint a SDK szólít fel egy függvény legfeljebb 5 megszorzása egy adott blob. Ha nem sikerül az ötödik próbálja meg, a SDK üzenet ad *webjobs-blobtrigger-mérgezett*nevű várólista.

A várakozási üzenet poison BLOB-egy JSON-objektum, amely tartalmazza a következő tulajdonságokat:

* FunctionId (a formátum *{függvénynevet alkalmazás}*. Funkciók. *{függvénynevet}*)
* BlobType ("BlockBlob" vagy "PageBlob")
* ContainerName
* BlobName
* ETag (egy blob verziójának azonosítója, például: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>Nagy tárolók lekérdezési BLOB

Ha a blob-tárolóhoz, amely az eseményindító figyeli 10 000-nél több BLOB tartalmaz, a függvényeket futási idejű ellenőrzés a naplófájlok megtekintése az új vagy módosított BLOB. Ez a folyamat nem valós idejű; a függvény előfordulhat, hogy nem kérjen elindítja néhány perccel vagy már a blob létrehozása után. Emellett az alap [tárterület-naplófájlokat hoz létre, a "legjobb erőfeszítéseket"](https://msdn.microsoft.com/library/azure/hh343262.aspx) ; nem sem garantálja, hogy az összes eseményének fog kell irányítania. Bizonyos körülmények között a naplók kihagyott kell. Ha nagy tárolók blob indítók sebességétől és megbízhatóságára korlátozások az alkalmazás nem elfogadhatók, az ajánlott módszer várólista üzenet létrehozása a blob létrehozása, és használjon várólista eseménykódot blob eseményindító helyett a blob feldolgozása.
 
## <a id="storageblobbindings"></a>Azure tároló blob bemeneti és kimeneti kötések

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>tárterület blob-Function.JSON bemeneti vagy kimeneti kötése

A *function.json* fájl adja meg a következő tulajdonságokat.

- `name`: A kód függvény használható az blob változóinak nevéből. 
- `path`: Egy elérési utat, amely meghatározza a blob a olvasása, és írja be a blob, hogy a tároló, és tetszés szerint blob neve mintát.
- `connection`: Neve, amely tartalmazza a kapcsolati karakterlánc tárterület-app beállítása. Ha hagyja `connection` üres, a kötést fog működni, amelyet AzureWebJobsStorage alkalmazás beállítása adott függvény alkalmazáshoz alapértelmezett tároló kapcsolati karakterlánccal.
- `type`: *Blob*kell beállítani.
- `direction`: Állítsa *be* vagy *ki*. 

Példa *function.json* egy tárolására blob-beviteli, vagy a kimeneti kötés, blob várólista az eseményindító használatával:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>BLOB-bemeneti és kimeneti támogatott fájltípusok

A `blob` kötés, szerializálni, vagy a következő típusú Node.js a és C# függvényekben deszerializálni:

* Objektum (`out T` kimeneti blob-C#: létrehoz egy blob null objektumként, ha a paraméter értéke null, amikor befejeződik a függvény)
* Karakterlánc (`out string` kimeneti blob-C#: blob hoz létre, csak akkor, ha a karakterlánc paraméter esetén nem null függvény)

A C# függvények is kell kötni az alábbiak:

* `TextReader`(csak bevitel)
* `TextWriter`(csak a kimenet)
* `Stream`
* `CloudBlobStream`(csak a kimenet)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>BLOB kimeneti C# kód példa

A C# kód példa nevével várólista üzenet érkezik blob másolja.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure tároló tábla bemeneti és kimeneti kötések

#### <a name="functionjson-for-storage-tables"></a>tárterület-táblák Function.JSON

A *function.json* adja meg a következő tulajdonságokat.

- `name`: A függvény kódot használt a táblázat kötése változóinak nevéből. 
- `tableName`: A tábla a neve.
- `partitionKey`és `rowKey` : együtt használva, olvassa el a C# vagy csomópontra függvény egy entitás, vagy írja be egy entitás csomópont függvény.
- `take`: Sorok olvasható csomópont függvény a beviteli tábla a maximális száma.
- `filter`: OData szűrőkifejezés csomópont függvény a beviteli tábla.
- `connection`: Neve, amely tartalmazza a kapcsolati karakterlánc tárterület-app beállítása. Ha hagyja `connection` üres, a kötést fog működni, amelyet AzureWebJobsStorage alkalmazás beállítása adott függvény alkalmazáshoz alapértelmezett tároló kapcsolati karakterlánccal.
- `type`: *Táblázat*kell beállítani.
- `direction`: Állítsa *be* vagy *ki*. 

A következő példa *function.json* várólista az eseményindító használja egy egyetlen sor olvasható. A JSON csomagolásukkor partíciót elsődlegeskulcs-értékének biztosít, és meghatározza, hogy megtalálható-e a sor billentyűt az várólista üzenetből.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Tárterület-táblázatok bemeneti és kimeneti támogatott fájltípusok

A `table` kötés, szerializálni, vagy Node.js a és C# függvények objektumainak deszerializálni. Az objektumok RowKey és PartitionKey tulajdonságok lesz. 

A C# függvények is kell kötni az alábbiak:

* `T`Ha `T` megvalósítása`ITableEntity`
* `IQueryable<T>`(csak bevitel)
* `ICollector<T>`(csak a kimenet)
* `IAsyncCollector<T>`(csak a kimenet)

#### <a name="storage-tables-binding-scenarios"></a>Tárterület táblák kötés felhasználási területei

A táblázat kötése támogatja az alábbi esetekben:

* Olvassa el a C# vagy csomópontra függvény egyetlen sor.

    Beállítása `partitionKey` és `rowKey`. A `filter` és `take` tulajdonságok ebben az esetben nem használják.

* Olvassa el a több sor C# függvény.

    A függvényeket futási idejű tartalmaz egy `IQueryable<T>` objektum kötve a táblázatot. Típus `T` kell származnia `TableEntity` vagy megvalósítása `ITableEntity`. A `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok ebben az esetben nem használják használhatja a `IQueryable` objektum elvégzendő bármely szűrés szükséges. 

* Olvassa el a több sor csomópont függvény.

    Állítsa a `filter` és `take` tulajdonságait. Nem állít `partitionKey` vagy `rowKey`.

* Írja be egy vagy több sor C# függvényt.

    A függvényeket futási idejű tartalmaz egy `ICollector<T>` vagy `IAsyncCollector<T>` kötött a táblázatra, ahol `T` adja meg a hozzáadni kívánt személyek sémájának. Általában, írja be a `T` származik `TableEntity` vagy hajt végre `ITableEntity`, de nem feltétlenül. A `partitionKey`, `rowKey`, `filter`, és `take` tulajdonságok ebben az esetben nem használják.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Tároló tábla példa: egy egyszerű táblázatos entitás C# vagy csomópontra olvasása

Az alábbi C# kód példa működik-e az előző *function.json* fájl korábbi látható egy egyszerű táblázatos entitás olvasható. A várakozási üzenetnek a sor kulcsmező értékét, és a táblázat entitás olvassa be a *run.csx* fájlban definiált típus. A típusban van `PartitionKey` és `RowKey` tulajdonságait, és nem származtatása `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

A következő F # kód példa is működik-e az előző *function.json* fájl egy egyszerű táblázatos entitás olvasható.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Az alábbi példa a csomópont kód is használható, az előző *function.json* fájllal egy egyszerű táblázatos entitás olvasható.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Tároló tábla példa: olvassa el a több tábla entitás c# 

Az alábbi *function.json* és C# kód példa olvasás szervezetek várólista üzenet megadott partíciót kulcs.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

A C# kód hozzáadása az Azure tároló SDK hivatkozást, hogy az a személy típusú származtatása is `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Tároló tábla példa: létrehozása a táblázat személyek a C# 

A következő példa *function.json* és *run.csx* táblázat szervezetek írja be a C# mutatja.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Tároló tábla példa: létrehozása a táblázat szervezetek az F#

A következő példa *function.json* és *run.fsx* táblázat szervezetek írása F # mutatja.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Tároló tábla példa: a csomópont táblázat entitás létrehozása

A következő példa *function.json* és *run.csx* megtudhatja, hogy hogyan írja be a táblázat entitás csomópontot.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
