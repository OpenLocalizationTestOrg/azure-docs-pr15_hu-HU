<properties
    pageTitle="Alkalmazás az összefüggéseket a Windows-szolgáltatások és dolgozó szerepkörök |} Microsoft Azure"
    description="Az alkalmazás az összefüggéseket SDK manuális felvétele a ASP.NET-alkalmazások használatát, az elérhetőség és a teljesítmény elemzése."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="awills"/>


# <a name="manually-configure-application-insights-for-aspnet-4-applications"></a>Kézi megadása az alkalmazás az összefüggéseket ASP.NET-4-alkalmazásokhoz

*Alkalmazás háttérismeretek az előzetes verzióban.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Manuálisan beállíthatja a [Visual Studio alkalmazás háttérismeretek](app-insights-overview.md) Windows szolgáltatások, dolgozó szerepkörök és más ASP.NET-alkalmazások figyelése. Web Apps alkalmazások a manuális konfiguráció ahelyett, hogy a Visual Studio által kínált [automatikus beállítási](app-insights-asp-net.md) .

Alkalmazás háttérismeretek segít diagnosztizálása problémák és teljesítményének figyelése és az élő alkalmazás használatát.

![Példa teljesítmény figyelése a diagramok](./media/app-insights-windows-services/10-perf.png)


#### <a name="before-you-start"></a>Előzetes teendők

szükséged van:

* [Microsoft Azure](http://azure.com)-előfizetést. A csoportwebhelyen vagy a szervezete rendelkezik egy Azure-előfizetést, ha a tulajdonos is fel kell vennie Önt, a [Microsoft-fiókjával](http://live.com).
* Visual Studio 2013-at vagy újabb verziója.



## <a name="add"></a>1. a alkalmazás háttérismeretek erőforrás létrehozása

Jelentkezzen be az [Azure-portálra](https://portal.azure.com/), és hozzon létre egy új alkalmazás háttérismeretek erőforrást. ASP.NET válassza az alkalmazás típusa.

![Kattintson az új, az alkalmazás Hírcsatornájában](./media/app-insights-windows-services/01-new-asp.png)

Egy [erőforrás](app-insights-resources-roles-access-control.md) az Azure szolgáltatás egy példánya. Ez az erőforrás pedig az alkalmazás telemetriai fog kell elemezni jelenik meg.

A választási lehetőségek alkalmazás típusú állítja be az alapértelmezett tartalom erőforrás rögzítéséhez és a Tulajdonságok [Mértékek Explorer](app-insights-metrics-explorer.md)látható.

#### <a name="copy-the-instrumentation-key"></a>Másolja a műszerezettségi billentyűt

A kulcs az erőforrás azonosítására és az hamarosan fogja a SDK irányítsa át az adatokat az erőforráshoz telepítését.

![Kattintson a Tulajdonságok parancsra, jelölje ki a kulcsot, és nyomja le a ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

A lépéseket, de csak elemeket hozhat létre új erőforrás bármely alkalmazás figyelése jó módszer. Most már küldhet adatokat is.

## <a name="sdk"></a>2. a SDK telepítse az alkalmazásban

Való telepítéséről és konfigurálásáról az alkalmazás az összefüggéseket SDK a platformot az aktív függően változik. Az ASP.NET-alkalmazások használata egyszerű.

1. A Visual Studióban szerkesztheti a web app projekt NuGet csomagokat.

    ![Kattintson a jobb gombbal a projektet, és válassza ki a Nuget csomagok kezelése](./media/app-insights-windows-services/03-nuget.png)

2. Telepítse a Web Apps alkalmazások alkalmazás háttérismeretek SDK csomagjában talál.

    !["Alkalmazás háttérismeretek" keresése](./media/app-insights-windows-services/04-ai-nuget.png)

    *Használhatom-e más csomagok?*

    igen. Ha csak az API segítségével küldje el saját telemetriai, válassza a Core API-val (Microsoft.ApplicationInsights). A Windows Server csomag automatikusan tartalmazza az alapvető API és számos más csomagok, például a teljesítmény számláló webhelycsoport és függőség figyelése. 

#### <a name="to-upgrade-to-future-sdk-versions"></a>A későbbi SDK verzióinak frissítése

Azt engedje fel az SDK új verziója időről-időre.

Egy [új kiadását az SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/)frissítéséhez nyissa meg a NuGet csomag újra manager és a telepített csomagok szűrő. Jelölje ki a **Microsoft.ApplicationInsights.Web** , és válassza a **frissítés**.

Végrehajtott testreszabások ApplicationInsights.config, ha egy példányának mentése, frissítés, és ezt követően a módosítások egyesítése az új verzió előtt.


## <a name="3-send-telemetry"></a>3. telemetriai küldése


**Ha telepítette az core API-csomag:**

* A műszerezettségi billentyűt a kódot, például beállítása a `main()`: 

    `TelemetryConfiguration.Active.InstrumentationKey = "`*a kulcs*`";` 

* [Írja be a saját a API telemetriai](app-insights-api-custom-events-metrics.md#ikey).


**Ha más alkalmazás háttérismeretek csomagok telepítve** van lehetősége, ha azt szeretné, a .config fájl segítségével műszerezettségi kulcs beállítása:

* ApplicationInsights.config szerkesztése (amely hozzá lett adva a NuGet telepített szerint). Szúrja be a közvetlenül a záró címke elé:

    `<InstrumentationKey>`*a másolt műszerezettségi billentyűt*`</InstrumentationKey>`

* Győződjön meg róla, hogy a megoldás Intézőben ApplicationInsights.config tulajdonságainak vannak **összeállítása művelettel = tartalom, a kimeneti könyvtár másolás másolás =**.




## <a name="run"></a>A projekt futtatása

Futtassa az alkalmazást, és próbálja ki az **F5** használatával: Nyissa meg a különböző lapokon néhány telemetriai létrehozásához.

A Visual Studióban látni fogja az elküldött események száma.

![A Visual Studióban események száma](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a>A telemetriai megtekintése

Térjen vissza az [Azure-portálra](https://portal.azure.com/) , és keresse meg az alkalmazást az összefüggéseket erőforrás.


Keresse meg az Áttekintés diagramok az adatokat. Az első egyszerűen csak megjelenik egy vagy két pontját. Példa:

![Kattintson a további adatok](./media/app-insights-windows-services/12-first-perf.png)

Kattintson az egyes diagramokra kattintva tekintse részletesebb mértékek keresztül. [További tudnivalók a mértékek.](app-insights-web-monitor-performance.md)

#### <a name="no-data"></a>Adatok nélkül?

* Használja az alkalmazás, a Megnyitás a különböző lapokon, hogy az egyes telemetriai hoz létre.
* Nyissa meg a [keresési](app-insights-diagnostic-search.md) csempére kattintva megtekintheti az egyes események. Előfordul, hogy tart események kissé közben hosszabb a mértékek folyamat olvas be.
* Várjon néhány másodpercig, és kattintson a **frissítés**parancsra. Diagramok rendszeres időközönként frissítse maguk, de frissítheti manuálisan, ha bizonyos adatok megjelennek az Ön várakozik.
* Lásd: a [Hibaelhárítás](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Az alkalmazás közzététele

Most az alkalmazások telepítése, a kiszolgáló vagy Azure, és az adatok gyűjteniük megtekintés.

![Visual Studio segítségével az alkalmazás közzététele](./media/app-insights-windows-services/15-publish.png)

Hibakeresési módban futtatásakor telemetriai végezhető el a továbbítási folyamatát, hogy meg kell jelennie másodpercek szereplő adatokat. Megjelenés konfigurációban az alkalmazás telepítésekor lassabban összegzi az adatokat.

#### <a name="no-data-after-you-publish-to-your-server"></a>Nincsenek adatok közzététele a kiszolgálón után?

Nyissa meg a kiszolgáló tűzfalában ezeket a kimenő forgalmához portokat:

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### <a name="trouble-on-your-build-server"></a>Problémái vannak az összeállítás kiszolgálóra?

Tanulmányozza [a hibaelhárítás elemre](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [AZURE.NOTE]Ha az alkalmazás telemetriai sok hoz létre (és használ, akkor a ASP.NET SDK verzió 2.0.0-beta3 vagy újabb), a adaptív mintavételnél modul automatikusan csökkenti a kötet, amely csak egy jellemző törtet események küldésével a rendszer elküldi a portálon. Azonban kapcsolódó eseményeket rögzítő ugyanazon kérésére kijelölt lesz, vagy csoportként kijelöletlen, hogy meg tudja nyitni kapcsolódó eseményeket között. 
> [Megtudhatja, hogy mintavételnél](app-insights-sampling.md).




## <a name="next-steps"></a>Következő lépések

* Az alkalmazás 360 fokos áttekintést kaphat a [További telemetriai hozzáadása](app-insights-asp-net-more.md) .



