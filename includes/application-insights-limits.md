Néhány korlátozás van érvényben a mértékek és az alkalmazás használati események száma (Ez azt jelenti, hogy egy műszerezettségi billentyűt). 

Korlátozások attól függenek, a [réteg árak](https://azure.microsoft.com/pricing/details/application-insights/) választott.

**Erőforrás** | **Alapértelmezett korlát** | **Maximális érték**
-------- | ------------- | -------------
Munkamenet adatpontok<sup>1, 2</sup> / hó | korlátlan | 
Teljes adatpontok havonta kérést, esemény, függőség, nyomkövetési, kivétel és a lap nézet | 5 millió | 50 millió<sup>3</sup>
[Nyomkövetés, és jelentkezzen be](../articles/application-insights/app-insights-search-diagnostic-logs.md) adatsebesség | 200 dp/s | 500 dp/s
[Kivétel](../articles/application-insights/app-insights-asp-net-exceptions.md) adatsebesség | 50 dp/s | 50 dp/s
A kérelmet, esemény, függőség és lap nézet telemetriai teljes adatsebesség | 200 dp/s | 500 dp/s
[A keresés](../articles/application-insights/app-insights-diagnostic-search.md) és [Analytics](../articles/application-insights/app-insights-analytics.md) nyers adatmegőrzés | 7 nap
[Mértékek](../articles/application-insights/app-insights-metrics-explorer.md) Explorer összesített adatmegőrzés | 90 napon
[A tulajdonság](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) neve száma | 100 |
Név hossza: tulajdonság | 150 | 
A tulajdonság értékét hossza | 8192 | 
Nyomon követése és a kivétel üzenet hossza | 10000 |
[Metrikus](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) neve száma | 100 |
Metrikus nevének hossza |  150 | 
[Elérhetőség vizsgálatok](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> az egyik adatpontra az egy adott metrikus érték vagy az esemény csatolt tulajdonságainak és mértékek.

<sup>2</sup> A munkamenet adatpont jelentkezik elején vagy végén a munkamenet, és a felhasználói azonosító jelentkezik.

<sup>3</sup> vásárolhat további kapacitás túl az 50 millió.
 
[Árak és az alkalmazás az összefüggéseket kvóták](../articles/application-insights/app-insights-pricing.md)
