<properties 
    pageTitle="Analytics – az alkalmazás az összefüggéseket a hatékony keresés eszköz |} Microsoft Azure" 
    description="Analytics, az alkalmazás az összefüggéseket a hatékony diagnosztikai keresési eszköz áttekintése. " 
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
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Az alkalmazás mélyebb Analytics


[Analytics](app-insights-analytics.md) a hatékony keresés funkció az [Alkalmazás az összefüggéseket](app-insights-overview.md). Ezeket a lapokat a lekérdezés Analitikájának lanquage ismertetik. 

* **[A bevezető videóból](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Tesztelése Analytics, hogy szimulált adatait a](https://analytics.applicationinsights.io/demo)** Ha az alkalmazás nem küld adatokat alkalmazás mélyebb még.

## <a name="queries-in-analytics"></a>Lekérdezések Analytics
 
Egy tipikus lekérdezés elválasztva *operátorok* sorozata követ *forrás* adattáblázathoz `|`. 

Ha például most megtudhatja, Hyderabad polgárainak próbálja meg a saját webalkalmazás nap mely időszakában. És, hogy nincs, miközben lássuk, milyen eredményt kódok visszatér a HTTP-kérelmeket. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Azt számolja meg külön ügyfél IP-címek, az elmúlt 7 nap alatt az órát napjának által csoportokba rendezheti. 

Vegyük megjeleníti az eredményt, a sávdiagram bemutató, az eredményeket a különböző válasz rendelkező halmozása kiválasztása:

![Válassza a sávdiagram, x és y tengely, majd a szegmens](./media/app-insights-analytics/020.png)

Tűnik lunchtime és beágyazása idejű Hyderabad a leggyakrabban használt az alkalmazás. (És azt kell vizsgálja meg a 500 kódok.)


Is hatékony statisztikai műveletek:

![](./media/app-insights-analytics/025.png)


A nyelv sok vonzó funkciókkal rendelkezik:

* [Szűrés](app-insights-analytics-reference.md#where-operator) a nyers alkalmazás telemetriai minden olyan mezők, beleértve az egyéni tulajdonságok és mértékek alapján.
* [Csatlakozás](app-insights-analytics-reference.md#join-operator) több tábla – lap nézetek, függőség hívásokat, kivétel és napló correlate kérések követi nyomon.
* Hatékony statisztikai [összesítések](app-insights-analytics-reference.md#aggregations).
* Csak a hatékony, mint az SQL, de jelentősen megkönnyíti a komplex lekérdezések: beágyazási kimutatások helyett egy általános iskolai művelet adatait a következőre pipe.
* Azonnali és hatékony megjelenítések.







## <a name="connect-to-your-application-insights-data"></a>Az alkalmazás az összefüggéseket adatok csatlakoztatása


Nyissa meg az alkalmazás [áttekintése lap](app-insights-dashboards.md) az alkalmazás az összefüggéseket Analytics: 

![Nyissa meg a portal.azure.com, nyissa meg az alkalmazást az összefüggéseket erőforrást, és válassza az analitika.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Korlátai

Jelenleg a lekérdezés eredményét fognak korlátozódni csak a múlt adatokat hetente fölé.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Következő lépések


* Azt javasoljuk, hogy a [nyelv bemutatója](app-insights-analytics-tour.md)kezdődik.