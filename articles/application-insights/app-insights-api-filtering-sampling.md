<properties 
    pageTitle="Szűrés és az alkalmazás az összefüggéseket SDK előfeldolgozása |} Microsoft Azure" 
    description="Az írás Telemetriai processzorok és a a SDK Telemetriai inicializálói szűrve, vagy a Tulajdonságok hozzáadása az adatokat, az alkalmazás az összefüggéseket portálra a telemetriai elküldése előtt." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Szűrés és az alkalmazás az összefüggéseket SDK telemetriai előfeldolgozása

*Alkalmazás háttérismeretek az előzetes verzióban.*

Írja be, és állítsa be az alkalmazás az összefüggéseket SDK telemetriai hogyan rögzítse és az alkalmazás az összefüggéseket szolgáltatás elküldés előtt feldolgozott testreszabása bővítmények. 

Ezek a funkciók jelenleg elérhető a ASP.NET SDK csomagjában talál.

* Telemetriai hangerejének [mintavételi](app-insights-sampling.md) csökkenti a statisztikát megtartásával. Közös továbbra is kapcsolatos adatpont is, hogy meg tudja nyitni őket, ha a probléma diagnosztizálása között. A portálon a teljes megszámolja megszorzása az mintavételnél kompenzálja.
* [A Telemetriai processzorok szűrése](#filtering) segítségével válasszon, vagy módosítsa a SDK telemetriai, Küldés a kiszolgálóval előtt. A telemetriai mennyiségű például csökkentheti, hiszen kizárhat kérések közli a. De szűrés csökkentése forgalom mintavételnél-nél több egyszerű megközelítése. További beállítási lehetőségekre mi továbbított lehetővé teszi, de ügyeljen arra, hogy befolyásolja a statisztika – például ki az összes sikeres kérelmeket szűrése esetén kell.
* [Telemetriai inicializálók tulajdonságok hozzáadása](#add-properties) bármely az alkalmazást, például a szokásos modulból telemetriai küldünk telemetriai. Ha például hozzáadhatja a számított értékeket; vagy verziószáma, amellyel szűrheti az adatokat a portálon.
* Egyéni események és mérőszámok küldése [Az SDK API](app-insights-api-custom-events-metrics.md) használják.


Mielőtt elkezdené:

* Az [alkalmazás az összefüggéseket ASP.NET v2 SDK](app-insights-asp-net.md) telepítse az alkalmazást. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Szűrés: ITelemetryProcessor

Ez a módszer lépve mi szerepel vagy kizárja a telemetriai adatfolyam több közvetlen szabályozható. Vele együtt mintavételnél, vagy külön-külön.

Telemetriai szűréséhez írása egy telemetriai processzor, és azt regisztrálása a SDK csomagjában talál. Az összes telemetriai halad át a processzor, és engedje el az adatfolyam, vagy a Tulajdonságok hozzáadása választhatja. Ide tartoznak a szabványos modulból, például a HTTP-kérés adatgyűjtő és a függőség adatgyűjtő telemetriai, valamint telemetriai írt saját magának. Például szűrheti kapcsolatos kéréseit közli, illetve a sikeres függőség hívások telemetriai meg. 

> [AZURE.WARNING] Szűrés a küldünk a SDK telemetriai processzorok segítségével döntés a statisztikai adatokat, láthatja a portálon, és hajtsa végre a kapcsolódó elemek megnehezítik.
> 
> Ehelyett akkor fontolja meg [mintavételnél](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Hozzon létre egy telemetriai processzor

1. Ellenőrizze, hogy az alkalmazás az összefüggéseket SDK a projekt verzió 2.0.0 vagy újabb verziójában. Kattintson a jobb gombbal a projekt a Visual Studio megoldás Intézőben, és válassza a NuGet csomagok kezelése. NuGet csomag manager jelölje be a Microsoft.ApplicationInsights.Web.

1. Szűrő létrehozásának ITelemetryProcessor végrehajtása. Ez a telemetriai modul telemetriai inicializálója és telemetriai csatorna például egy másik bővítési pontra. 

    Figyelje meg, hogy Telemetriai processzorok Egyenletszerkesztővel egy feldolgozás lánc. Akkor hozható létre egy telemetriai processzor, amikor egy hivatkozást a következő processzor láncában, át. Egy telemetriai adatpontra átadott a folyamat módszer, ha munkája tartalmaz, és kattintson a következő Telemetriai processzor felhívja a lánc.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Szúrja be a ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Ez a a azonos szakasz, ahol inicializálni a mintavételnél szűrő.)

A .config fájlból, mert az osztályához nyilvános elnevezett tulajdonságok továbbíthatja karakterláncérték. 

> [AZURE.WARNING] A neve és a kód osztály és tulajdonságok névvel .config fájl bármely tulajdonságok neve megfelelően gondoskodik. Ha a .config fájlt egy nem létező típusa vagy tulajdonság hivatkozik, a SDK csendes meghiúsulhat bármely telemetriai küldhet.

 
**Másik lehetőségként** is inicializálni a kódot a szűrőt. Egy megfelelő inicializálni osztály – például a Global.asax.cs AppStart – a lánc szúrjon be a processzor:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

Ez a pont után létrehozott TelemetryClients a processzorok fogja használni.

### <a name="example-filters"></a>Példa szűrők

#### <a name="synthetic-requests"></a>Szintetikus kérések

Botok és webes vizsgálatok kiszűrése. Mértékek Explorer felajánlja a szintetikus forrásokból. kiszűrése, bár ezt a lehetőséget a SDK csomagjában talál a szűréssel csökkenti a forgalmat.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Nem sikerült hitelesítés

Kiszűrése kérések "401" választ. 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Gyors távoli függőség hívások kiszűrése

Ha csak szeretne lassú hívások diagnosztizálása, szűrheti az adatokat gyors azokat meg. 

> [AZURE.NOTE] Ez lesz döntés a statisztika megjelenik a portálon. A függőséget diagram fog megjelenni, mintha a függőség hívások összes hibák.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Függőség problémáinak diagnosztizálása

[A blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) függőség problémáinak diagnosztizálása automatikusan küld a normál pingelése függőségek projekt ismerteti.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Tulajdonságok hozzáadása: ITelemetryInitializer

Telemetriai inicializálók segítségével küldött összes telemetriai; a globális tulajdonságainak megadása és a szokásos telemetriai modulok kijelölt működési módjának felülbírálására. 

Az alkalmazás az összefüggéseket webes csomag például a HTTP-kérések telemetriai gyűjti össze. Alapértelmezés szerint ezt megjelöli ahogy sikertelen volt egy válasz kóddal kérésének > = 400. De 400 tekinti a siker szeretne, akkor nyújthat egy telemetriai inicializálója, amely a sikeres tulajdonság állítja be.

Ha megad egy telemetriai inicializálója, ennek neve jelenik meg, ha a Track*() módszerek bármelyikével nevezik. Ide tartoznak a szabványos telemetriai modulok nevű módszereket. Által kiállítás ezeket a modulokat bármely inicializálólistában által beállított tulajdonság nem állít be. 

**A inicializálója meghatározása**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**A inicializálója betöltése**

A ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Másik lehetőségként* a inicializáló, a kód, például az Global.aspx.cs létrehozhatnak meg:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Lásd: további a következő példában.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>A JavaScript telemetriai inicializálók

*A JavaScript*

A telemetriai inicializálója beszúrása közvetlenül azután, hogy a a portálon kapott inicializálni kódot: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Elérhető a telemetryItem nem egyéni tulajdonságainak összefoglalását lásd: az [adatmodell](app-insights-export-data-model.md#lttelemetrytypegt).

Annyi inicializálók tetszés szerint hozzáadhat. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor és ITelemetryInitializer

Mi a különbség telemetriai processzorok és telemetriai inicializálók között?

* Vannak bizonyos átfedés az, hogy mit tehet a velük: Tulajdonságok hozzáadása telemetriai egyaránt használható.
* Mindig futtatása előtt TelemetryProcessors TelemetryInitializers.
* TelemetryProcessors teszi teljesen cseréje vagy az Elvetés elemet egy telemetriai elemre.
* TelemetryProcessors teljesítmény számláló telemetriai nem folyamat.



## <a name="persistence-channel"></a>Adatmegőrzési csatorna 

Ha az alkalmazás elindul, ahol az internetkapcsolat kifolyólag sem érhetők el, és a lassú, fontolja meg az adatmegőrzési csatorna az alapértelmezett memóriában csatorna helyett. 

Az alapértelmezett memóriában csatorna, amely még nem küldte bezárja az alkalmazás indításakor minden telemetriai megszakad. Bár használhat `Flush()` megpróbálja küld adatokat a puffer a hátralévő, továbbra is elveszíti adatok, ha nincs internetkapcsolata van, vagy ha előtt leállítja az alkalmazás átviteli kész.

Ezzel ellentétben az adatmegőrzési csatorna pufferel telemetriai fájlban, mielőtt elküldené a portálra. `Flush()`ellenőrzi, hogy az adatok a fájlban vannak tárolva. Ha bármelyik argumentum nem küldte el az idő bezárja az alkalmazást, a fájlban marad. Amikor újraindítja az alkalmazást, az adatokat küld majd, ha van internetkapcsolat. Adatok összegzi sokáig szükség, amíg meg nem érhető el a kapcsolatot a fájlban. 

### <a name="to-use-the-persistence-channel"></a>Az adatmegőrzési csatorna használata

1. A NuGet csomag [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3)importálni.
2. Az alkalmazás megfelelő inicializálni helyen kód tartalmazza:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Használat `telemetryClient.Flush()` előtt a alkalmazás bezárása kattintva ellenőrizze, hogy adatokat küld a portálra vagy menti a fájlt.

    Ne feledje, hogy Flush() az adatmegőrzési csatorna szinkronizált, de más csatornák aszinkron.

 
Az adatmegőrzési csatorna eszközök alkalmazási eseteit, ahol az alkalmazás által létrehozott események száma viszonylag kicsi, és a kapcsolat gyakran nem megbízható van optimalizálva. A csatorna fog írása események lemezre megbízható tárolóba először és próbálja meg küldje el. 

#### <a name="example"></a>Példa

Tegyük fel, hogy kezelt kivételek figyelni kívánt. Előfizetett a `UnhandledException` esemény. A visszahívás akkor írja be a kiürítés hívást kezdeményez, győződjön meg arról, hogy a telemetriai megőrződnek.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Ha az alkalmazás kikapcsolja, látni fogja a fájl `%LocalAppData%\Microsoft\ApplicationInsights\`, amely tartalmazza a tömörített eseményeket. 
 
Következő indításakor: Ha az alkalmazás, a csatorna fog emelje fel a fájlt, és az alkalmazás mélyebb telemetriai olvasása, ha azt is.

#### <a name="test-example"></a>Példa vizsgálat

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Az adatmegőrzési csatorna a kódot be van kapcsolva [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Hivatkozás dokumentumok

* [API – áttekintés](app-insights-api-custom-events-metrics.md)

* [ASP.NET-hivatkozás](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK kódot.

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [A JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Következő lépések


* [Keresési események és naplók][diagnostic]
* [Mintavételnél](app-insights-sampling.md)
* [Hibaelhárítás][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
