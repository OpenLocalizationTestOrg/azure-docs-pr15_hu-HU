<properties
    pageTitle="Teljesítményét figyelve Fejlesztőeszközök Analytics a mobil web Apps-alkalmazások |} Microsoft Azure"
    description="Alkalmazás teljesítményének és a használat figyelése mobilalkalmazás fejlesztők számára. , asztali, webes szolgáltatás és kódmentes alkalmazások HockeyApp és az alkalmazás az összefüggéseket."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobil Analytics HockeyApp és az alkalmazás Hírcsatornájában

Figyelje meg a teljesítmény és a béta-próba és a [HockeyApp](https://hockeyapp.net/)telepített mobil és az asztali alkalmazások használatát. A támogató webes szolgáltatás, és kódmentes használt alkalmazások figyelése a [Visual Studio alkalmazásban az összefüggéseket](app-insights-overview.md). Az ügyfél és a kiszolgáló alkalmazások Analytics adatainak elemzése és megjelenítése egymás mellett a diagram egy Azure irányítópultján.

A Microsoft Developer Analytics két megoldások alkalmazás teljesítményét figyelve kínál:

* **HockeyApp mobilkészülékekre** és az asztali alkalmazások.
 * A kijelölt felhasználóknak próbaverziók terjesztése.
 * Elemző összeomlik.
 * Egyéni telemetriai használatát elemzéshez.
* **A webhelyek alkalmazás Hírcsatornájában** , és szolgáltatások kódmentes alkalmazásokat.
 * Teljesítményének és látogatottságának mértékek és értesítések.
 * Kivétel jelentése és diagnosztikai nyomkövetési.
 * Diagnosztikai keresése a szűrés és a kapcsolódó telemetriai hivatkozásokkal.

Két megoldás kínálják:

 * Hatékony **[analitikus lekérdezésnyelv](app-insights-analytics.md)** diagnosztikai és elemzés.
 * **[Adatok exportálása](app-insights-export-telemetry.md)** a saját tároló.
 * Elemző diagramok és táblázatok **integrált irányítópult** megjelenítését.

## <a name="monitor-your-app-components"></a>Az alkalmazás-összetevők figyelése

Kövesse ezeket a lapokat a SDK telepítse a kód és nyomon követheti az alkalmazás a.

### <a name="web-apps---application-insights"></a>Web Apps alkalmazások – alkalmazás Hírcsatornájában

* [ASP.NET web App alkalmazásban](app-insights-asp-net.md) 
* [Java web App alkalmazásban](app-insights-java-get-started.md)
* [NODE.js web App alkalmazásban](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure Cloud Services](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobilalkalmazások - HockeyApp

* [iOS-alkalmazás](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X alkalmazásban](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android-alkalmazás](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Univerzális Windows-alkalmazás](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Windows Phone 8 vagy 8.1 alkalmazás](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [A Windows bemutató Foundation alkalmazás](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Az asztali Windows-alkalmazások azt javasoljuk HockeyApp. De is [küldése a Windows-alkalmazás mélyebb alkalmazásból telemetriai](app-insights-windows-desktop.md). Érdemes lehet, hogy kísérletezés alkalmazás az összefüggéseket a.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analitikai és HockeyApp telemetriai az Exportálás

Vizsgálja meg a HockeyApp egyéni, és jelentkezzen be az analitikai és a folyamatos exportálása a funkcióival alkalmazás mélyebb [hidat](app-insights-hockeyapp-bridge-app.md)beállításával telemetriai.




