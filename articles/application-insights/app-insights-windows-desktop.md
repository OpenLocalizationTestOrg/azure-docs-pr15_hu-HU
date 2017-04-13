<properties 
    pageTitle="Használati és a teljesítmény-Windows asztali alkalmazások figyelése" 
    description="Használati és a Windows asztali alkalmazás HockeyApp és az alkalmazás az összefüggéseket a teljesítmény elemzése." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Használati és teljesítményét a Windows asztali alkalmazások figyelése

*Alkalmazás háttérismeretek az előzetes verzióban.*

[Visual Studio alkalmazás háttérismeretek](app-insights-overview.md) és [HockeyApp](https://hockeyapp.net) lehetővé teszik a telepített alkalmazások használatát és a teljesítmény figyelését.

> [AZURE.IMPORTANT] Azt javasoljuk, hogy [HockeyApp](https://hockeyapp.net) terjesztésében és az asztal- és alkalmazások figyelése. A HockeyApp akkor is terjesztési élő tesztelése és felhasználói visszajelzés kezelése, valamint figyelheti a használati és összeomlik jelentéseket. Is [exportálhatja, és a telemetriai Analytics a lekérdezés](app-insights-hockeyapp-bridge-app.md).

> A küldött telemetriai alkalmazás mélyebb egy asztali alkalmazásban, bár ez elsősorban a hasznos hibakeresési és kísérleti céljából.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Alkalmazás mélyebb telemetriai küldése a Windows-alkalmazás

1. Az [Azure portál](https://portal.azure.com) [alkalmazás háttérismeretek erőforrás létrehozása](app-insights-create-new-resource.md). Az alkalmazás típusa válassza a ASP.NET alkalmazást.
2. Eltarthat egy példányát a műszerezettségi billentyűt. Keresse meg az imént létrehozott új erőforrás Essentials legördülő a kulcsot. 
3. A Visual Studióban az alkalmazás projekt NuGet csomagok szerkesztése és Microsoft.ApplicationInsights.WindowsServer hozzáadása. (Vagy válassza ki a Microsoft.ApplicationInsights, ha csak a bA API nélkül a szabványos telemetriai webhelycsoport modulokat.)
4. Állítsa be a műszerezettségi billentyűt vagy a kódot:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*a kulcs*`";` 

    vagy ApplicationInsights.config (Ha a normál telemetriai csomagok egyikéhez telepítve van):
 
    `<InstrumentationKey>`*a kulcs*`</InstrumentationKey>` 

    ApplicationInsights.config használata esetén ellenőrizze, hogy a megoldás Explorer megjeleníti a tulajdonságait vannak beállítva **összeállítása művelettel = tartalom, a kimeneti könyvtár másolás másolás =**.
5. [Használja az API](app-insights-api-custom-events-metrics.md) telemetriai küldhet.
6. Futtassa az alkalmazást, és látható a telemetriai az erőforrás létrehozott az Azure-portálon.

## <a name="telemetry"></a>Példa kódot.

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Következő lépések

* [Irányítópult létrehozása](app-insights-dashboards.md)
* [Diagnosztikai keresés](app-insights-diagnostic-search.md)
* [Mértékek feltárása](app-insights-metrics-explorer.md)
* [Analytics-lekérdezéseket írni](app-insights-analytics.md)
 
