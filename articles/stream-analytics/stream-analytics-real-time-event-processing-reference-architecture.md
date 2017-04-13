<properties 
    pageTitle="Értékáram-elemzés esemény feldolgozása feldolgozás valós idejű esemény |} Microsoft Azure" 
    description="Megtudhatja, hogyan Azure szolgáltatások esemény valós idejű feldolgozás és az elemzést engedélyezésének együttműködhetnek." 
    keywords="valós idejű feldolgozás, az esemény feldolgozása, a hivatkozás architektúra"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Architektúra hivatkozás: Microsoft Azure Értékáram-elemzés feldolgozás valós idejű esemény

Valós idejű esemény Azure Értékáram-elemzés feldolgozás hivatkozás architektúrája üzembe helyezése a valós idejű platform a Microsoft Azure service (PaaS) adatfolyam-feldolgozási megoldásként általános tervrajz számára íródott.

## <a name="summary"></a>Összefoglalás

Régebben analytics-megoldások volna alapozni lehetőségek, például ETL (kivonat, átalakítás, betöltés) és az adatok raktározás, adatokat tároló analysis előtt. Változó követelményeknek, ideértve a további gyorsan érkező adatokat, ebben a modellben meglévő vannak terjesztése, a korlátja. Az azt jelenti, hogy elemezheti adatait tároló előtt adatfolyamok áthelyezése egyik megoldás, és bár nem egy új szolgáltatása, a megközelítés nem körben fogadott összes iparágban területeket keresztül. 

Microsoft Azure-teljes körű katalógust, amelyek képesek másik megoldás esetek és követelmények tömb analytics-technológiák biztosít. Azure-végpont megoldást üzembe szolgáltatások kiválasztása, lehet, hogy egy adott ajánlataiban szélessége kérdés. Ez a papír szolgáltatásait és együttműködési beállításokat, amelyek támogatják az esemény streaming megoldások különböző Azure szolgáltatások szolgál. Ismerteti az jelenik meg, amelyben a vevők előnyeivel megközelítés ilyen típusú részét.

## <a name="contents"></a>Tartalom

- Összefoglalás
- Valós idejű Analytics – bevezetés
- Értékajánlatát valós idejű adatok Azure-ban
- Valós idejű Analytics tipikus esetei
- Architektúra és -összetevők
    - Adatforrások
    - Réteg adatok-integráció
    - Valós idejű Analytics réteg
    - Adatokat tároló rétege
    - Bemutató / felhasználási réteg
- Elfogadásáról

**Szerző:** Háttérismeretek adatközpont Charles Feddersen, megoldás Architect kiválósági, Microsoft Corporation

**Közzétett:** Január 2015

**Verzió:** 1.0

**Letöltése:** [Valós idejű esemény, a Microsoft Azure Értékáram-elemzés feldolgozása](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
