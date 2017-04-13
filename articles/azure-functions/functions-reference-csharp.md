<properties
    pageTitle="Azure függvények Fejlesztői segédlet |} Microsoft Azure"
    description="Megtudhatja, hogyan használatával C# Azure függvények fejlesztését."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure függvények funkciók, esemény feldolgozása, webhooks, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure függvények C# Fejlesztői segédlet

> [AZURE.SELECTOR]
- [C# parancsfájl](../articles/azure-functions/functions-reference-csharp.md)
- [F # parancsfájl](../articles/azure-functions/functions-reference-fsharp.md)
- [NODE.js](../articles/azure-functions/functions-reference-node.md)
 
A C# felület az Azure-funkciók az Azure WebJobs SDK alapul. Adatok folytatódjon a C# függvény módszer argumentumokat keresztül. Az argumentumok nevét adható `function.json`, és nincsenek előre definiált nevek összetevőjét, például a függvény naplózó és lemondás tokenek eléréséhez.

Ez a cikk tartalma feltételezi, hogy az [Azure függvények Fejlesztői segédlet](functions-reference.md)már olvasott.

## <a name="how-csx-works"></a>Hogyan működik a .csx

A `.csx` formátum lehetővé teszi, hogy kevesebb "újrafelhasználható" írása, és csak egy C# függvény írási kiemelése. Azure-függvényekkel, akkor csak olyan összeállítás hivatkozások szerepeltetése és névtér felül, a szokásos módon szükséges felfelé, és tördelése minden névtér és osztály, hanem csak határozhatja meg a `Run` módot. Ha is használni kell bármely osztályok, például POCO objektumok definiálásához belül ugyanannak a fájlnak osztály is.

## <a name="binding-to-arguments"></a>Argumentumok kötése

A különböző kötések kötődnek egy C# függvény keresztül a `name` tulajdonság *function.json* konfigurációjában. Minden egyes kötés van a saját támogatott típus, amely egy kötelező; dokumentált az eseményindító blob támogatniuk például karakterlánc, egy POCO vagy számos más típusú. A típus, amely a leginkább illik a szükséges is használhatja. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Naplózás

Jelentkezzen be a továbbított naplók C# kimeneti, felvehet egy `TraceWriter` argumentumban megadott. Azt javasoljuk, nevezze el `log`. Azt javasoljuk, hogy ne `Console.Write` Azure-függvényekkel.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Aszinkron

Ahhoz, hogy aszinkron függvény, használja a `async` kulcsszó és visszatérés a `Task` objektumot.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>A lemondás jogkivonat

Egyes esetekben előfordulhat műveleteket, amelyek érzékenyek a Leállítás. Minden esetben célszerű képes kezelni az összeomló kódírás, miközben azokban az esetekben, amelyhez kezelje a sikeres-e leállás kérelmeket, definiálása egy [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentumban megadott.  A `CancellationToken` Ha a host leállítása induljanak nyújtanak. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Névtér importálása

Ha névtér importálása kell végezheti el úgy a szokásos, az a `using` záradék.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

A következő névtér importálásának automatikusan, ezért nem kötelező:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Külső összeállítások hivatkozó

Keret összeállítások, felvétele hivatkozások használatával a `#r "AssemblyName"` irányelv.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Ha egy privát összeállítás hivatkozni szeretne, az összeállítás fájlt a merevlemezről is feltölthet egy `bin` mappát, a függvény és a hivatkozást, a fájl használatával nevet (például viszonyított  `#r "MyAssembly.dll"`). A fájlok feltöltése a függvény mappába, az alábbi szakasz a tartalmaz csomag kezelése.

## <a name="package-management"></a>Csomag kezelése

C# függvény NuGet csomagok használni, hogy *project.json* fájl feltöltése a fájlrendszerben az függvény alkalmazást a függvény mappa. Íme egy példa *project.json* fájl, amely egy Microsoft.ProjectOxford.Face verzió 1.1.0 mutató hivatkozás hozzáadása:

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

A .NET keretrendszer 4.6 támogatott, ezért ügyeljen, hogy megadja-e a *project.json* fájl `net46` itt ismertetett módon.

*Project.json* fájl feltöltésekor a futtatókörnyezet kapja a csomagok, és a program automatikusan hozzáadja a csomag szerelvények mutató hivatkozásokat. Nem kell hozzáadni `#r "AssemblyName"` irányelvek. Csak adhatja hozzá a kívánt `using` utasítások a *run.csx* fájlhoz, a NuGet csomagokban definiált típusok használatához.


### <a name="how-to-upload-a-projectjson-file"></a>Project.json fájl feltöltése

1. Legyen a függvény alkalmazás indítása fut, amelyek úgy, hogy megnyitja a függvény az Azure-portálon végezheti el. 

    Ez is hozzáférést biztosít a továbbított naplók hol csomag a telepítés kimeneti jelenik meg. 

2. Project.json fájl feltöltése, használja az [Azure függvények developer reference témakörének](functions-reference.md#fileupdate) **függvény app-fájlokkal frissítése** szakaszban leírt módszerek egyikét. 

3. Után a *project.json* fájlt töltenek fel, például a következő példában a függvény eredménye a folyamatos átvitelű log láthat:

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

Változó vagy az alkalmazás beállítás értékét, használja `System.Environment.GetEnvironmentVariable`, a következő példa látható módon:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>.Csx kód újrafelhasználása

Osztályok és más *.csx* fájlokat a *run.csx* fájl a megadott módszerek is használhatja. Amely használja `#load` irányelvek *run.csx* fájljait, az alábbi példában látható módon.

Példa *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Példa *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

A relatív elérési utat is használhatja a `#load` irányelv:

* `#load "mylogger.csx"`a függvény mappában található fájl betölti.

* `#load "loadedfiles\mylogger.csx"`a függvény mappájában található fájl betölti.

* `#load "..\shared\mylogger.csx"`közvetlenül a *wwwroot*az függvény mappával, ez azt jelenti, hogy a tagolási szintre mappában található fájl betölti.
 
A `#load` irányelv works csak *.csx* (C# parancsfájl) fájlokkal, és nem *.cs* fájlokat. 

## <a name="next-steps"></a>Következő lépések

További információt az alábbi forrásokban talál:

* [Azure függvények Fejlesztői segédlet](functions-reference.md)
* [Azure függvények F # Fejlesztői segédlet](functions-reference-fsharp.md)
* [Azure függvények NodeJS Fejlesztői segédlet](functions-reference-node.md)
* [Azure függvények eseményindítók és kötések](functions-triggers-bindings.md)

