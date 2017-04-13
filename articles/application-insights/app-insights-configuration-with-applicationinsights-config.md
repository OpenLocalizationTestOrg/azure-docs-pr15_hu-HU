<properties 
    pageTitle="Alkalmazás háttérismeretek SDK ApplicationInsights.config vagy .xml konfigurálása" 
    description="Engedélyezése vagy letiltása adatok gyűjtése modulok és teljesítmény számláló és további paramétereket hozzáadása" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Az alkalmazás az összefüggéseket SDK ApplicationInsights.config vagy .xml konfigurálása

Az alkalmazás az összefüggéseket .NET SDK NuGet csomagok számos áll. Az [alapvető csomag](http://www.nuget.org/packages/Microsoft.ApplicationInsights) az API biztosít telemetriai küld az alkalmazás az összefüggéseket. [További csomagokat](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) biztosítson telemetriai _modulok_ és _inicializálók_ automatikusan nyomon követés telemetriai az alkalmazás és a hozzá tartozó környezetben. A konfigurációs fájl módosításával engedélyezése vagy letiltása telemetriai modulok és inicializálók, és őket az egyes paraméterek beállítása.

A beállítási fájl neve `ApplicationInsights.config` vagy `ApplicationInsights.xml`, attól függően, milyen típusú az alkalmazás. A project automatikusan felkerül amikor [SDK legtöbb verziójának telepítése][start]. Azt is megjelenik, webalkalmazást [állapot]felügyelő IIS-kiszolgálón[redfield], vagy ha jelölje ki a Appplication háttérismeretek [az Azure webhelyén vagy a virtuális bővítményének](app-insights-azure-web-apps.md).

Nem szabályozhatja az [weblapon SDK]egyenértékű fájlként[client].

Ehhez a dokumentumhoz a szakaszok látni a konfigurációs fájl, hogyan szabályozzák összetevőinek a SDK csomagjában talál, és mely NuGet csomagok betöltése e összetevők ismerteti.

## <a name="telemetry-modules-aspnet"></a>Telemetriai modulok (ASP.NET)

Minden telemetriai modul által gyűjtött adatokat egy bizonyos típusú, és használja az alapvető API elküldhesse az adatokat. A modulokat különböző NuGet csomag, ami azt is hozzáadhat a szükséges sorok a .config fájlt telepítését.

A konfigurációs fájl minden modulhoz van egy csomópontot. A modul letiltása, törli a csomópontot, vagy megjegyzést azt.



### <a name="dependency-tracking"></a>Függőség nyomon követése

[Követés függőség](app-insights-dependencies.md) kapni az alkalmazás és a külső szolgáltatások és adatbázisait módosít hívásokról telemetriai gyűjti össze. Ahhoz, hogy a modul munkára az IIS-kiszolgálót, telepítenie kell [ezeket az állapot Monitor][redfield]. Ahhoz, hogy használhassa az Azure web Apps alkalmazások vagy VMs, [Jelölje be az alkalmazás az összefüggéseket kiterjesztést](app-insights-azure-web-apps.md).

A saját nyomon követése a [TrackDependency API](app-insights-api-custom-events-metrics.md#track-dependency)kód függőség is írhat.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet csomagot.

### <a name="performance-collector"></a>Teljesítmény adatgyűjtő

IIS-telepítések [gyűjti össze a rendszer teljesítményét számláló](app-insights-performance-counters.md) például Processzor, a memória és a hálózati betöltéséhez. Megadhatja, melyik számláló összegyűjtéséhez, többek között a teljesítmény számláló állított be, hogy saját maga.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet csomagot.


### <a name="application-insights-diagnostics-telemetry"></a>Alkalmazás háttérismeretek diagnosztika Telemetriai

A `DiagnosticsTelemetryModule` magát alkalmazás háttérismeretek műszerezettségi kódban hibát jelez. Ha például a kód nem tud hozzáférni a teljesítmény számláló, vagy ha egy `ITelemetryInitializer` kivételt. A [Diagnosztikai keresési]megjelenik ez a modul követi nyomon követése telemetriai[diagnostic]. Diagnosztikai adatok küld dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet csomagot. Ha csak a csomagban, a ApplicationInsights.config fájlt nem automatikusan készül. 

### <a name="developer-mode"></a>Fejlesztői üzemmód

`DeveloperModeWithDebuggerAttachedTelemetryModule`az alkalmazás az összefüggéseket kezd `TelemetryChannel` küldése azonnal adatainak egyszerre, amikor egy csatolva van-e a telepítési folyamatának egy telemetriai elem. Ez a közötti idő mennyiségét csökkenti, amikor az alkalmazás telemetriai nyomon követi, és megjelenik az alkalmazás az összefüggéseket a portálon. A Processzor és a hálózati sávszélesség jelentős terhelést okozza.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Alkalmazás az összefüggéseket a Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet csomag

### <a name="web-request-tracking"></a>Webes kérelem nyomon követése

A [Válasz időt és az eredmény kódot](app-insights-asp-net.md) a HTTP-kérések jelentést. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet csomag

### <a name="exception-tracking"></a>A kivétel nyomon követése

`ExceptionTrackingTelemetryModule`számok esetén nem kezelt kivételeket az webalkalmazásban. Lásd: a [hibák és a kivételek][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet csomag


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`- [elmulasztott tevékenység kivételek](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx)nyomon követi.
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-dolgozó szerepkörök, a windows-szolgáltatások és kezelőkonzol-alkalmazások esetén nem kezelt kivételek nyomon követi.
* [Alkalmazás az összefüggéseket a Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet csomagot.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

A Microsoft.ApplicationInsights csomag SDK [core API-t](https://msdn.microsoft.com/library/mt420197.aspx) tartalmaz. A telemetriai modulok: Ezzel a, és használhatja a is [, hogy a saját telemetriai megadása](app-insights-api-custom-events-metrics.md).

* Nincs ApplicationInsights.config bejegyzést.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet csomagot. Ha csak a NuGet, nincs .config fájl jön létre.

## <a name="telemetry-channel"></a>Telemetriai csatorna

A telemetriai csatorna pufferelési és az alkalmazás az összefüggéseket szolgáltatás telemetriai továbbítása kezeli. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`a szolgáltatások alapértelmezett csatorna van. A memóriában azt pufferel az adatokat.
* `Microsoft.ApplicationInsights.PersistenceChannel`van egy alternatív megoldás console-alkalmazásokhoz. Azt is unflushed adatok mentése állandó tárolóhoz, amikor az alkalmazás bezárul, és elküldi azt, amikor az alkalmazás újraindítása.


## <a name="telemetry-initializers-aspnet"></a>Telemetriai inicializálók (ASP.NET)

Telemetriai inicializálók helyi tulajdonságainak küldjük együtt a telemetriai minden elemet. 

Azt is megteheti, hogy a környezetben tulajdonságainak beállításához [Írja be a saját inicializálók](app-insights-api-filtering-sampling.md#add-properties) .

A szokásos inicializálók összes beállított akár a webhelyen vagy WindowsServer NuGet csomagok:


* `AccountIdTelemetryInitializer`a AccountId tulajdonság beállítása
* `AuthenticatedUserIdTelemetryInitializer`a JavaScript SDK által beállított AuthenticatedUserId tulajdonságát állítja be.
* `AzureRoleEnvironmentTelemetryInitializer`frissítések a `RoleName` és `RoleInstance` tulajdonságait a `Device` információkkal, az Azure futtatókörnyezet kiolvasott naptárelemekhez telemetriai környezetben.
* `BuildInfoConfigComponentVersionTelemetryInitializer`frissítések a `Version` tulajdonsága az `Component` kiolvasott értékkel naptárelemekhez telemetriai környezetben a `BuildInfo.config` MS Build készített fájlt.
* `ClientIpHeaderTelemetryInitializer`frissítések `Ip` tulajdonsága az `Location` telemetriai elemeket környezetében alapján a `X-Forwarded-For` a kérelem HTTP fejlécében.
* `DeviceTelemetryInitializer`az alábbi vezérlőtulajdonságok frissíti a `Device` telemetriai elemeket környezetben.
 - `Type`"PC" beállítása
 - `Id`van beállítva a tartománynév annak a számítógépnek a webalkalmazás futtató.
 - `OemName`a kinyert értékre van állítva a `Win32_ComputerSystem.Manufacturer` WMI használata mezőben.
 - `Model`a kinyert értékre van állítva a `Win32_ComputerSystem.Model` WMI használata mezőben.
 - `NetworkType`a kinyert értékre van állítva a `NetworkInterface`.
 - `Language`van állítva, adja meg a nevét a `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`frissítések a `RoleInstance` tulajdonsága az `Device` telemetriai szereplő összes a tartomány nevét a számítógépen, amelyen a webalkalmazás fut környezetben.
* `OperationNameTelemetryInitializer`frissítések a `Name` tulajdonsága az `RequestTelemetry` és a `Name` tulajdonsága az `Operation` telemetriai elemeket környezetében alapján a HTTP-metódus, valamint a ASP.NET MVC vezérlő és a meghívott kérelem feldolgozásához művelet nevét.
* `OperationIdTelemetryInitializer`vagy `OperationCorrelationTelemetryInitializer` frissítéseket a `Operation.Id` helyi tulajdonság telemetriai cikkek nyomon az automatikusan generált kérelmének kezelése során `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`frissítések a `Id` tulajdonsága az `Session` kiolvasott értékkel rendelkező összes telemetriai elem környezetben a `ai_session` cookie-k generálja a felhasználó böngészőben futó ApplicationInsights JavaScript-műszerezettségi kódot. 
* `SyntheticTelemetryInitializer`vagy `SyntheticUserAgentTelemetryInitializer` frissítéseket a `User`, `Session` és `Operation` kontextusokat tulajdonságok telemetriai cikkek szintetikus adatforrásokból, kérelmének kezelésekor, például az elérhetőség tesztelése, vagy keressen motor bot nyomon. Alapértelmezés szerint [Mértékek Explorer](app-insights-metrics-explorer.md) szintetikus telemetriai nem jelenít meg. 

    A `<Filters>` azonosító kérelmek tulajdonságainak beállítása.
* `UserAgentTelemetryInitializer`frissítések a `UserAgent` tulajdonsága az `User` telemetriai elemeket környezetében alapján a `User-Agent` a kérelem HTTP-fejléc.
* `UserTelemetryInitializer`frissítések a `Id` és `AcquisitionDate` tulajdonságainak `User` kiolvasott értékű naptárelemekhez telemetriai környezetben a `ai_user` cookie-k generálja a felhasználó böngészőben futó alkalmazás háttérismeretek JavaScript-műszerezettségi kódot.
* `WebTestTelemetryInitializer`a felhasználói azonosító, a munkamenet-azonosító és a HTTP-kérelmeket, [rendelkezésre állás-vizsgálat](app-insights-monitor-web-app-availability.md)érkező szintetikus adatforrás tulajdonságai beállítása.
A `<Filters>` azonosító kérelmek tulajdonságainak beállítása.

## <a name="telemetry-processors-aspnet"></a>Telemetriai processzorok (ASP.NET)

Telemetriai processzorok szűrheti és módosítani minden telemetriai elem, csak az a SDK csomagjában talál a portálra elküldése előtt.

Azt is megteheti, [Írja be a saját telemetriai processzorok](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptív mintavételnél telemetriai processzor (az 2.0.0-beta3)

Ez alapértelmezés szerint engedélyezve van. Ha az alkalmazás telemetriai sok küld, a processzor részét eltávolítja.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

A paraméter biztosít a cél, amely a algoritmus megkísérli elérése. A SDK minden példányában egymástól függetlenül működik, ha a kiszolgáló több gépek fürt, a tényleges mennyiségű telemetriai szorozva megfelelően kell-e.

[További tudnivalók a mintavételnél](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Rögzített ráta mintavételnél telemetriai processzor (az 2.0.0-beta1)

Szabványos [mintavételnél telemetriai processzor](app-insights-api-filtering-sampling.md#sampling) (az 2.0.1) is van:

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Csatorna paraméterek (Java)

Ezek a paraméterek hogyan a Java SDK kell tárolására és telemetriai adatokat gyűjt ürítése befolyásolják.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

A SDK memóriában tároló tárolt telemetriai elemek száma. Ennek a számnak elérésekor telemetriai puffer kiürül, – ez azt jelenti, hogy a telemetriai elemek elküldi az alkalmazás az összefüggéseket kiszolgáló.

-   Min: 1
-   Max: 1000
-   Alapértelmezett: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Határozza meg, hogy milyen gyakran az adatokat, amely tárolása a memóriában található ki kell üríteni (elküldött alkalmazás mélyebb).

-   Min: 1
-   Max: 300
-   Alapértelmezett: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Határozza meg, hogy a helyi lemezen állandó tárolására van kiosztott MB maximális mérete. A tárhely nem sikerült az alkalmazás az összefüggéseket végpontot továbbítani megőrzéséhez telemetriai elemekhez használható. A tároló méretét teljesülésekor új telemetriai elemek elvesznek.

-   Min: 1
-   Max: 100
-   Alapértelmezés: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Ez határozza meg az alkalmazást az összefüggéseket erőforrás, az adatok jelenik meg. A szokásos, hozzon létre egy külön erőforrást, külön termékkulccsal, az alkalmazás minden egyes.

Ha meg szeretné adni a kulcs dinamikusan – például ha el szeretné küldeni az eredményeket a különböző erőforrások - alkalmazásból kihagyja az a billentyűt az importált kereséskonfigurációs fájl, és állítsa be azt az kódja ehelyett.

Az kulcs beállítása a TelemetryClient összes előfordulását, szabványos telemetriai modulokat, beleértve a kulcs beállítása a TelemetryConfiguration.Active. Egy inicializálni módszer, például egy ASP.NET szolgáltatásban global.aspx.cs tegye a következőket:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Ha csak szeretne meghatározott események küldése egy másik erőforrást, beállíthatja, hogy a kulcs az egy adott TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[További tudnivalók az API][api].

Egy új kulcs, [Hozzon létre egy új erőforrást, az alkalmazás az összefüggéseket portálon]lépjen[new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

