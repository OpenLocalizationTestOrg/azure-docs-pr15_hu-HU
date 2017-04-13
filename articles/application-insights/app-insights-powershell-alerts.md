<properties
    pageTitle="Értesítések beállítása az alkalmazás az összefüggéseket a Powershell használatával"
    description="E-mailek metrikus változásokról megszerezni alkalmazás háttérismeretek konfigurálása automatizálása."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Értesítések beállítása az alkalmazás az összefüggéseket a PowerShell használatával

Automatizálható a [Visual Studio alkalmazás mélyebb](app-insights-overview.md) [értesítések](app-insights-alerts.md) konfigurálása.

Ezeken kívül állíthat be [a választ, hogy jelzést automatizálása webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Egyszeri beállítása

Ha még nem használta PowerShell az Azure-előfizetéséhez előtt:

Telepítse az Azure Powershell-modult a számítógépen, amelyre a parancsfájlok futtatásának.

 * Telepítse [Microsoft webes Platform Installer (v5 vagy újabb)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Telepítse a Microsoft Azure Powershell használatával


## <a name="connect-to-azure"></a>Azure csatlakoztatása

Indítsa el az Azure PowerShell és [az előfizetés csatlakozni](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Értesítéseket kapjon

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Értesítés hozzáadása


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Példa 1

E-mailben, ha a HTTP-kérések átlagolni 5 percig a kiszolgáló választ 1 másodperc lassabb. Az alkalmazás háttérismeretek az erőforrás neve IceCreamWebApp, és a Fabrikam erőforráscsoport. Az Azure előfizetés tulajdonosa vagyok.

A globálisan egyedi azonosítója az előfizetés azonosítója (nem a műszerezettségi billentyűt az alkalmazás).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Példa 2

Van egy alkalmazást, amelyben segítségével [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) egy mérőszám "salesPerHour." nevű jelentés Küldés e-mailben a munkatársaimnál, ha a "salesPerHour" 100, alá esik átlagolni 24 óra feletti.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Ugyanezt a szabályt a jelentett a [Mértékegység paraméter](app-insights-api-custom-events-metrics.md#properties) egy másik nyomon követés hívás például TrackEvent vagy trackPageView mérőszám használható.

## <a name="metric-names"></a>Metrikus nevek

Metrikus neve | Képernyő neve | Leírás
---|---|---
`basicExceptionBrowser.count`|Kivételek a böngészőben|A böngészőben nem kivételek számát.
`basicExceptionServer.count`|Kiszolgáló kivételek|Az alkalmazás esetén nem kezelt kivételek száma
`clientPerformance.clientProcess.value`|Ügyfél feldolgozási ideje|A dokumentum utolsó bájt fogadása, mindaddig, amíg a DOM betöltött között eltelt idő. Aszinkron kérések feldolgozás továbbra is lehet.
`clientPerformance.networkConnection.value`|Lap betöltési hálózati csatlakozás idejének| A böngésző csatlakozzon a hálózathoz szükséges időt. 0 akkor lehet, ha a gyorsítótárazott.
`clientPerformance.receiveRequest.value`|Válaszidő fogadása| A böngésző kérést küld el kell érkezett válasz között eltelt idő.
`clientPerformance.sendRequest.value`|Kérés ideje küldése| Idő böngésző-összehívás küldése.
`clientPerformance.total.value`|Böngésző lap betöltési idejének|A felhasználói kérelem mindaddig, amíg a DOM, a stíluslapok, a parancsprogramokat és a képek betöltött ideje.
`performanceCounter.available_bytes.value`|Rendelkezésre álló memória|Fizikai memória közvetlenül egy folyamat vagy rendszer használatra.
`performanceCounter.io_data_bytes_per_sec.value`|Folyamat IO ráta|Az összes bájt / szekundum és a fájlokat, hálózati és eszközök olvashatók.
`performanceCounter.number_of_exceps_thrown_per_sec`|Kivétel ráta|Kivételek másodpercenként.
`performanceCounter.percentage_processor_time.value`|A folyamat Processzor|Eltelt idő az alkalmazások folyamatot a processzor végrehajtás című témakörben által használt összes szál százaléka.
`performanceCounter.percentage_processor_total.value`|Processzor|A százalékos aránya a processzor nem üresjárati szálak tölt.
`performanceCounter.process_private_bytes.value`|A magánjellegű bájt folyamatábra|A memória kizárólag a megfigyelt alkalmazás folyamatok rendelt.
`performanceCounter.request_execution_time.value`|ASP.NET kérés végrehajtása ideje|A legutóbbi kérés végrehajtása ideje.
`performanceCounter.requests_in_application_queue.value`|ASP.NET kérések végrehajtás várakozási sorában|Az alkalmazás kérés várólista hosszát.
`performanceCounter.requests_per_sec`|ASP.NET kérelem ráta|Az alkalmazás másodpercenként érkező kérések ASP.NET mértéke.
`remoteDependencyFailed.durationMetric.count`|Függőség hibák|A kiszolgáló alkalmazás külső erőforrások sikertelen hívások számát.
`request.duration`|Kiszolgáló válaszidő|HTTP-kérelem fogadására és befejezése, a válasz küldése között eltelt idő.
`request.rate`|Ráta kérése|Az alkalmazás másodpercenként összes kérelmet mértéke.
`requestFailed.count`|Sikertelen kérések|Válasz kódot eredményező száma a HTTP kérések > = 400
`view.count`|Lap nézetek|Ügyfél felhasználói kérések az weblapon számát. Szintetikus forgalom ki van szűrve a nézetből.
{az egyéni metrikus neve}|{Metrikus neve}|A metrikus érték jelenteni [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) vagy a [nyomon követés hívás mérések paramétert](app-insights-api-custom-events-metrics.md#properties).

A mérési módja miatt különböző telemetriai modulok által küldött:

Metrikus csoport | Adatgyűjtő modul
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>nézet | [A JavaScript böngészőben](app-insights-javascript.md)
performanceCounter | [Teljesítmény](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Függőség](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
kérés<br/>requestFailed|[Kiszolgáló kérés](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Azt is megteheti, hogy [jelzést visszajelzését automatizálása](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure fogja hívni egy webcímet, lehetőség akkor következik be, egy figyelmeztetés.

## <a name="see-also"></a>Lásd még:


* [Parancsfájl alkalmazás háttérismeretek konfigurálása](app-insights-powershell-script-create-resource.md)
* [Alkalmazás mélyebb és a webes vizsgálat erőforrások létrehozása sablonból](app-insights-powershell.md)
* [Microsoft Azure diagnosztika csatolási alkalmazás mélyebb automatizálása](app-insights-powershell-azure-diagnostics.md)
* [A választ, hogy jelzést automatizálása](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
