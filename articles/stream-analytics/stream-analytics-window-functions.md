<properties
    pageTitle="Adatfolyam-elemző ablak függvényeit |} Microsoft Azure"
    description="Tudjon meg többet a három ablak funkciókat Értékáram-elemzés (tumbling, hopping, csúszó)."
    keywords="csúszó ablakában ablak hopping ablakában tumbling"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Adatfolyam-elemző ablak függvényei – bevezetés

Sok valós időben esetek streaming hajtsa végre a műveleteket csak az adatok időbeli windows található szükség. Ablakok elrendezése funkciók támogatása natív Azure adatfolyam Analytics, amely áthelyezi a tű Fejlesztőeszközök termelékenység az összetett adatfolyam feldolgozás feladatok létrehozása a fő jellemzője. Értékáram-elemzés lehetővé teszi, hogy a fejlesztők számára, hogy windows [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) és [**csúszó**](https://msdn.microsoft.com/library/dn835051.aspx) segítségével adatfolyam időbeli műveletek hajthatók végre. Érdemes megjegyezni, hogy [minden Ablakműveletek](https://msdn.microsoft.com/library/dn835019.aspx) kimeneti az ablak eredményeit, a **befejezési** . A kimenet valamelyikére lesz az összesítő függvényekkel használt egyszeri esemény. Az esemény lesz az ablak végére időbélyege és az összes ablak függvényeket egy rögzített hosszú. Végül fontos megfigyelheti, hogy az összes ablak függvény kell használni a [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) záradékban.

![Adatfolyam Analytics ablak függvények fogalmak](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Tumbling ablak

Tumbling ablak függvények adatfolyam szakaszokhoz különböző időpont részekre, és hajtsa végre a ellen őket, például az alábbi példában függvény használatával. A fő differentiators Tumbling ablak is, hogy azok ismétlődjön, nem áll átfedésben és esemény egynél több tumbling ablak nem tartozik.

![Adatfolyam Analytics ablak függvények tumbling – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Szórási ablak

Ablak függvények Ugrás előre hopping időben által rögzített pontra. Érdemes őket Tumbling windows, amely átfedésben is, mint események egynél több Hopping ablak eredményhalmaz is tartoznak, egyszerűen lehet. Hogy egy Hopping ablak ugyanaz, mint egy Tumbling egy ablak szeretné elég megadni a azonosítható méretét ugyanaz, mint az ablak méretének. 

![Adatfolyam-elemző ablak függvények hopping – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Csúszó ablak

Csúszó ablak Funkciók eltérően Tumbling vagy Hopping windows konzerv egy kimenet **csak** egy esemény esetén. Ablakok lesz, ha legalább egy eseményt, és az ablak folyamatosan tolja előre-€ (epszilon). A Windows Hopping, például események egynél több csúszó ablak tartozhat.

![Adatfolyam-elemző ablak függvények csúszó – bevezetés](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>A Súgó ablak funkciókkal

További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
