<properties
    pageTitle="Azure függvények F # Fejlesztői segédlet |} Microsoft Azure"
    description="Megtudhatja, hogyan használatával F # Azure függvények fejlesztését."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure funkciók, függvények, esemény feldolgozása, webhooks, dinamikus számítási, kiszolgáló nélküli architektúrája, F#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Azure függvények F # Fejlesztői segédlet

> [AZURE.SELECTOR]
- [C# parancsfájl](../articles/azure-functions/functions-reference-csharp.md)
- [F # parancsfájl](../articles/azure-functions/functions-reference-fsharp.md)
- [NODE.js](../articles/azure-functions/functions-reference-node.md)

F # Azure függvények megoldás a könnyen futó kis darabokra kódot, vagy a "függvények," a felhőben. Adatok folytatódjon a F # függvény Függvényargumentumok keresztül. Az argumentumok nevét adható `function.json`, és nincsenek előre definiált nevek összetevőjét, például a függvény naplózó és lemondás tokenek eléréséhez.

Ez a cikk tartalma feltételezi, hogy az [Azure függvények Fejlesztői segédlet](functions-reference.md)már olvasott.

## <a name="how-fsx-works"></a>Hogyan működik a .fsx

Egy `.fsx` fájl, F # parancsfájl. Azt is értelmezhető projektként F # egyetlen fájlban lévő. A fájl tartalmaz, mind a program (ebben az esetben az Azure függvény) kódját és kezeléséről a függőségek irányelveket.

Amikor egy `.fsx` az Azure függvény gyakran szükséges szerelvények, amely lehetővé teszi, a "újrafelhasználható", hanem függvény kód kiemelése automatikusan mellékelve.

## <a name="binding-to-arguments"></a>Argumentumok kötése

Minden egyes kötés támogat bizonyos argumentumokat az [Azure függvények eseményindítók és kötések Fejlesztői segédlet](functions-triggers-bindings.md)ismertetett módon. Például az argumentum kötések blob eseményindító támogatja az egyik POCO, amelynek F # rekord lehet megadni. Példa:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Az F # Azure függvény megnyílik egy vagy több argumentum. Azure függvények argumentumai megvitatjuk, amikor azt olvassa el _a bemeneti_ argumentumok és _kimeneti_ argumentumokat. Egy beviteli argumentum pontosan mi úgy tűnik, például: az F # Azure függvény a szövegbeviteli. _Kimenet_ argumentumot akkor változtatható adatok vagy egy `byref<>` szolgál egyik módja a sikeres argumentuma adatok biztonsági _,_ a függvény.

A fenti példában `blob` egy beviteli argumentum és `output` kimenet argumentumot akkor. Figyelje meg, hogy ez `byref<>` az `output` (nem kell hozzáadni a `[<Out>]` széljegyzet). Használja a `byref<>` típus lehetővé teszi, hogy melyik rekordra vagy objektum utal, amelyet a argumentum módosítása a függvény.

Az F # rekord egy beviteli típus használata esetén a rekord definíció kell jelölni `[<CLIMutable>]` ahhoz, hogy az Azure függvények keretrendszer mezőket szeretne megadni a megfelelően előtt a rekord a függvény. A motorháztető alatt `[<CLIMutable>]` hoz létre az a rekord tulajdonságai párbeszédpanelen alkotóival. Példa:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

F # osztály a mindkettő és argumentumok is használható. Az osztály tulajdonságok általában kell Getterek és alkotóival. Példa:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Naplózás

A [folyamatos átvitelű naplók](../app-service-web/web-sites-streaming-logs-and-console.md) F # jelentkeznek kimeneti, a függvény típusú argumentumot kell vennie `TraceWriter`. Egységesebb, az azt javasoljuk ezt az argumentumot nevű `log`. Példa:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Aszinkron

A `async` munkafolyamat is használható, de vissza az eredményt kell adnia egy `Task`. Ez lehet tenni a `Async.StartAsTask`, például:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>A lemondás jogkivonat

Ha a függvény biztonságosan kezelje a Leállítás, adhat neki egy [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentum. Ez is lehet kombinálni `async`, például:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Névtér importálása

Névtér nyithatók meg a szokásos módon:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

A következő névtér automatikusan nyílnak meg:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Külső összeállítások hivatkozó

Hasonlóképpen, keretrendszer összeállítás hivatkozás lehet hozzáadni a `#r "AssemblyName"` irányelv.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

A következő szerelvények automatikusan megjelennek az Azure-függvényekkel üzemeltetési környezet:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Ezeken kívül az alábbi összeállítások speciális cased és simplename is hivatkozhat (pl. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Ha egy privát összeállítás hivatkozni szeretne, az összeállítás fájlt a merevlemezről is feltölthet egy `bin` mappát, a függvény és a hivatkozást, a fájl használatával nevet (például viszonyított  `#r "MyAssembly.dll"`). A fájlok feltöltése a függvény mappába, az alábbi szakasz a tartalmaz csomag kezelése.

## <a name="editor-prelude"></a>Szerkesztő Prelude

A szerkesztő, amely támogatja az F # fordítási szolgáltatások nem lesz tartsa szem előtt a névtér és szerelvények, amely automatikusan tartalmazza az Azure függvények. Ilyen lehet hasznos, amelyet fel szeretne venni egy prelude, amely segít a szerkesztő keresse meg a szerelvények használja, és névtér kifejezetten megnyitásához. Példa:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Azure függvények végrehajtja a kódot, amikor a forrás feldolgozásával `COMPILED` definiálva, így a szerkesztő prelude figyelmen kívül hagyja.

## <a name="package-management"></a>Csomag kezelése

F # függvény NuGet csomagot használ, vegye fel a `project.json` fájl a fájlrendszerben az függvény alkalmazást a függvény mappa. Íme egy példa `project.json` NuGet csomag hivatkozás hozzáadó fájl `Microsoft.ProjectOxford.Face` 1.1.0 verziója:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

A .NET keretrendszer 4.6 támogatott, ezért ügyeljen, amely a `project.json` fájl `net46` itt ismertetett módon.

Amikor feltöltése egy `project.json` fájlt, a futtatókörnyezet a csomagok kap, és a program automatikusan hozzáadja a csomag szerelvények mutató hivatkozásokat. Nem kell hozzáadni `#r "AssemblyName"` irányelvek. Csak adhatja hozzá a kívánt `open` utasítások a `.fsx` fájlt.

Előfordulhat, hogy a szerkesztő prelude javítható a szerkesztő interakció F # fordítási szolgáltatások hivatkozások összeállítások automatikusan elhelyezni kívánt.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Hogyan lehet hozzáadni a `project.json` fájl az Azure függvény

1. Legyen a függvény alkalmazás indítása fut, amelyek úgy, hogy megnyitja a függvény az Azure-portálon végezheti el. Ez is hozzáférést biztosít a továbbított naplók hol csomag a telepítés kimeneti jelenik meg.

2. Töltse fel a `project.json` fájlt, a [függvény app-fájlokkal frissítése](functions-reference.md#fileupdate)ismertetett lehetőségek közül. Ha [Folytonos telepítésének Azure függvények](functions-continuous-deployment.md)használata esetén felvehet egy `project.json` az átmeneti tárolásra szolgáló ág annak érdekében, hogy felveszi a telepítési ág előtt kísérletezni fájlt.

3. Miután a `project.json` fájl kerül, látni fogja a függvény a következő példa hasonló kimeneti adatait a folyamatos átvitelű napló:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>A környezeti változók

Változó vagy az alkalmazás beállítás értékét, használja `System.Environment.GetEnvironmentVariable`, például:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>.Fsx kód újrafelhasználása

Használhatja a többi kódot `.fsx` fájlok használatával egy `#load` irányelv. Példa:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Elérési út jelenít meg a `#load` irányelv is viszonyított helyét a `.fsx` fájlt.

* `#load "logger.fsx"`a függvény mappában található fájl betölti.

* `#load "package\logger.fsx"`egy található fájl betöltése a `package` függvény mappájában.

* `#load "..\shared\mylogger.fsx"`egy található fájl betöltése a `shared` mappát, a függvény mappába, ez azt jelenti, hogy ugyanazon a szinten közvetlenül a `wwwroot`.

A `#load` irányelv csak akkor működik, a `.fsx` (F #) parancsfájlok, és nem a `.fs` fájlokat.

## <a name="next-steps"></a>Következő lépések

További információt az alábbi forrásokban talál:

* [F # útmutató](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure függvények Fejlesztői segédlet](functions-reference.md)
* [Azure függvények C# Fejlesztői segédlet](functions-reference-csharp.md)
* [Azure függvények NodeJS Fejlesztői segédlet](functions-reference-node.md)
* [Azure függvények eseményindítók és kötések](functions-triggers-bindings.md)
* [Tesztelés Azure függvények](functions-test-a-function.md)
* [Méretezés Azure függvények](functions-scale.md)
