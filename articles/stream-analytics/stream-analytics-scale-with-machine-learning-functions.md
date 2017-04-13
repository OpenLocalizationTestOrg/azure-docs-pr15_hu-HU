<properties
    pageTitle="Átméretezheti a Értékáram-elemzés feladatok Azure gépi tanulási függvényekkel |} Microsoft Azure"
    description="Megtudhatja, hogy miként megfelelően méretezni Értékáram-elemzés feladatok (szétválasztás, SZU mennyiség és az egyéb) Azure gépi tanulási függvények használata esetén."
    keywords=""
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

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>A Értékáram-elemzés feladatok Azure gépi tanulási függvényekkel méretezése

Felhasználók is gyakran igazán könnyen beállítása egy Értékáram-elemzés feladatot, és futtassa a mintaadatokat is tartalmazó keresztül. Mit kell azt tenni az ugyanazon feladatot még a magasabb szintű adatok mennyiségi szükség? Szükséges nekünk Tanulja meg, hogy azt fogja méretezni, állítsa be a Értékáram-elemzés feladatot. A dokumentumban hogy a méretezés gépi tanulási függvények feladatokat megjelenítő Értékáram-elemzés speciális szempontjait összpontosít. Értékáram-elemzés feladatok általános méretezni olvashat ismertető [Méretezés feladatokat](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>ActiveX-vezérlőket egy Azure gépi tanulási függvény, a megjelenítő Értékáram-elemzés

Értékáram-elemzés a gépi tanulási függvény használható például Értékáram-elemzés lekérdezési nyelvű szabályos függvény hívás. A jelenet, mögött azonban a függvény hívások ténylegesen Azure gépi tanulási webes szolgáltatáskérések is. Gépi tanulási web services támogatja a "kötegelés" több sor, vagyis az minimális köteget, az adott webhely szolgáltatás API hívásban, általános teljesítmény javítása érdekében. Tanulmányozza az alábbi cikkekben további részleteket; [Értékáram-elemzés az Azure gépi tanulási funkciók](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) és [Azure gépi tanulási webszolgáltatásokhoz](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Értékáram-elemzés feladat konfigurálása és gépi tanulási függvények

Értékáram-elemzés feladat gépi tanulási függvény konfigurálásakor vannak is célszerű figyelembe venni, köteg méretét a gépi tanulási függvény hívások és a folyamatos átvitelű egységek (SUs) a Értékáram-elemzés feladathoz kiépítve két paraméterrel. Határozza meg a megfelelő értékeket ezeknél, először döntés kell tenni között késés és az átvitt, ez azt jelenti, hogy Értékáram-elemzés feladatot, és az egyes SZU átviteli késés. SUs mindig adható hozzá átvitt jól particionált Értékáram-elemzés lekérdezések növelése egy feladatot, bár további SUs növeli a feladat futtatása költségét.

Ezért fontos várakozási idő a Értékáram-elemzés feladat futtatása *hibatűrést* határozza meg. Azure gépi tanulási szolgáltatáskérések szakítható további késés természetesen növelik a köteg méretű, amely fog összetett a időtartam, a megjelenítő Értékáram-elemzés feladat. Kézzel, köteg méretének növelése lehetővé teszi, hogy a Értékáram-elemzés feladat feldolgozása *További események a *azonos számú * gépi tanulási webes szolgáltatási kérelmek. Gyakran gépi tanulási web service késés növekedése is rész lineáris növekedéséhez tétel méretét, ezért fontos a költség-leghatékonyabb tétel méretét az adott gépi tanulási webszolgáltatás bármely adott helyzetben figyelembe. Az alapértelmezett köteg méret esetében a webszolgáltatás-kérelmek 1000 és által a [Értékáram-elemzés REST API]-(https://msdn.microsoft.com/library/mt653706.aspx "Értékáram-elemzés REST API") vagy segítségével a [PowerShell-ügyfélprogram Értékáram-elemzés](stream-analytics-monitor-and-manage-jobs-use-powershell.md "Értékáram-elemzés PowerShell-ügyfélprogram")lehet módosítani.

A köteg méretet megállapítása után a folyamatos átvitelű egységek (SUs) mennyiségét meghatározható, a függvény kell, hogy a események száma alapján másodpercenként a folyamat. Találhat további információt a folyamatos átvitelű egységek olvassa el a cikk [adatfolyam Analytics-skála feladatokat](stream-analytics-scale-jobs.md#configuring-streaming-units).

Az általános nincsenek 20 egyidejű kapcsolatok a gépi tanulási webszolgáltatás minden 6 SUs, azzal a különbséggel, hogy 1 SZU feladatok és a 3 SZU feladat kap 20 egyidejű kapcsolatok is.  Ha például a bemeneti adatok ráta másodpercenként 200,000 események és a tétel méretét vannak az alapértelmezett 1000 az eredményül kapott web service várakozási idő 1000 események minimális köteg esetén 200ms. Ez azt jelenti, hogy minden kapcsolat legyen 5 kérelmek a gépi tanulási webszolgáltatás egy második. 20 kapcsolatok a megjelenítő Értékáram-elemzés feladat is feldolgozni 20 000 a 200ms és ezért 100 000 események egy második. Úgy 200,000 másodpercenként dolgozható, a Értékáram-elemzés feladat van szüksége 40 egyidejű kapcsolatok, amely a 12 SUs verzióval. Az alábbi ábrán szemlélteti a gépi tanulási webes szolgáltatás végpontjának Értékáram-elemzés projekthez kérésére – minden 6 SUs kapcsolatban van 20 egyidejű gépi tanulási webszolgáltatás a max.

![Méretezés adatfolyam analitikai és gépi tanulási függvények 2 feladat példa] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Méretezés adatfolyam analitikai és gépi tanulási függvények 2 feladat példa")

Az általános tétel méretét, ***L*** a web service várakozási méretben köteg B ezredmásodpercben, a ***B*** a kapacitásának- ***N*** SUs Értékáram-elemzés feladat van:

![Adatfolyam analitikai és gépi tanulási függvények képlet méretezése] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Adatfolyam analitikai és gépi tanulási függvények képlet méretezése")

Egy további szempontokat lehetséges, hogy a "max egyidejű hívások" gépi tanulási web service oldalán, a legnagyobb értéket (200 jelenleg) beállítani ajánlott.

További információt a ezt a beállítást, kérjük, olvassa el a [Méretezés cikk gépi tanulási webszolgáltatásokhoz](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Példa – Sentiment elemzése

A következő példa az sentiment elemzés gépi tanulási függvény, a megjelenítő Értékáram-elemzés feladat tartalmazza, az [adatfolyam Analytics gépi tanulási integrációs oktatóprogram](stream-analytics-machine-learning-integration-tutorial.md)leírt módon.

A lekérdezés nem teljesen particionált egyszerű lekérdezés a **sentiment** függvényének követ, alább látható módon:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

A következő forgatókönyvvel; egy átviteli a 10 000 twitterre másodpercenként a Értékáram-elemzés feladat elvégzéséhez sentiment elemzése a twitterre (események) hozható létre. 1 SZU használ, a megjelenítő Értékáram-elemzés feladat lehet képes kezelni a forgalom? Az alapértelmezett köteg méretűre 1000 a feladat látnia kell a bemeneti lépést tartani. További a hozzáadott gépi tanulási függvény létrehozzon egy második késés, amely az általános további alapértelmezett várakozási idő a gépi tanulási webszolgáltatás sentiment-elemzés (a köteg hossza 1000). A Értékáram-elemzés feladat **Általános** vagy végpontok közötti késés állna néhány másodpercet. Részletesebb meg figyelembe kell venni a Értékáram-elemzés feladat, *különösen* a gépi tanulási függvény hívások. 1000, amelynek a tétel méretét, egy átviteli 10 000 események lép, hogy körülbelül 10 kérések webszolgáltatásnak. Akár 1 SZU, állnak elég egyidejű kapcsolatainak a bemeneti forgalmának engedélyezésére.

De mi a teendő, ha a bemeneti esemény ráta 100 x nő, és most már van szüksége a Értékáram-elemzés feladat másodpercenként 1,000,000 twitterre feldolgozása? Kétféleképpen lehet:

1.  A köteg méretének növelése vagy
2.  Partition a bemeneti adatfolyam párhuzamosan az események feldolgozása

Az első beállítást választja a feladat- **időtartam** megnöveli.

A második lehetőség további SUs kellene kell építenie, és ezért készítése a több egyidejű gépi tanulási webes szolgáltatási kérelmet. Ez azt jelenti, hogy a projekt **költsége** nő.


Feltételezik sentiment elemzése gépi tanulási webszolgáltatás késleltetése 200ms 1000-esemény kötegekben vagy alatt, az 5000-esemény kötegekben, 10 000-esemény tételek 300ms vagy 500ms 25 000-esemény tételek 250ms.

1. Az első lehetőséget, (**nem** további SUs kiépítési) használ, a tétel méretét is kell emelni **25 000**. Ez a lehetővé teszi a feladatot a gépi tanulási webszolgáltatás 20 egyidejű kapcsolatainak 1,000,000 események feldolgozása (egy hívás 500ms válaszidejű). Így a további időtartam, a megjelenítő Értékáram-elemzés feladat miatt a sentiment függvény kérések szemben a gépi tanulási webes szolgáltatási kérelmet szeretne kell emelni **200ms** a **500ms**. Azonban a látható, hogy a köteg méretét, **nem** lehet kisebb vagy nagyobb végtelen a gépi tanulási webszolgáltatásokhoz megköveteli a tartalom kérelem betűmérete 4 MB-os webszolgáltatás időtúllépés kéri a tevékenység 100 másodperc után.
2. A második lehetőség használata esetén a tétel méretét 1000, a bal 200ms web service válaszidejű, minden 20 egyidejű kapcsolatok a webszolgáltatás tudják 1000 *20* 5 dolgozható = 100 000 másodpercenként. 1,000,000 másodpercenként dolgozható, hogy a feladat kellene 60 SUs. És összehasonlítása az első lehetőséget, Értékáram-elemzés feladat további webes köteg szolgáltatáskérések, viszont létrehozása egy nagyobb költség tenné.

Az alábbi táblázat a átviteli adatfolyam Analytics feladat számára nem különböző SUs és a köteg méretű (a második események száma).

| tétel méretét (Machine Learning időtartama)  | 500 (200ms) | 1000 (200ms) | 5000 (250ms) | 10 000 (300ms) | 25 000 (500ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SZU** | 2500 | 5000 | 20 000 | 30 000 feletti számig | 50 000 |
| **3 SUs** | 2500 | 5000 | 20 000 | 30 000 feletti számig | 50 000 |
| **6 SUs** | 2500 | 5000 | 20 000 | 30 000 feletti számig | 50 000 |
| **12 SUs** | 5000 | 10 000 | 40 000 | 60 000 | 100 000 |
| **18 SUs** | 7,500 | 15 000 | 60 000 | 90,000 | 150 000 |
| **24 SUs** | 10 000 | 20 000 | 80,000 | 120 000 | 200 000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25 000 | 50 000 | 200 000 | 300,000 | 500 000 |

Most már rendelkeznie kell egy jól érthető Értékáram-elemzés gépi tanulási függvényei működéséről. Csak akkor fogják is tisztában Értékáram-elemzés feladatok "ki" adatforrásból származó adatok és az egyes "ki" adja vissza egy köteg események feldolgozása, a megjelenítő Értékáram-elemzés feladathoz. Hogyan a leküldéses modell hatása a gépi tanulási szolgáltatáskérések webes?

A tétel méretét, hogy gépi tanulási funkciók beállítása általában pontosan nem osztható minden Értékáram-elemzés feladat "ki" által visszaadott események száma. Amikor ez történik, hívja a gépi tanulási webszolgáltatás "részleges" kötegekben történik. Ez történik, nem merülnek fel további feladat késés terhelést az coalescing események ki ki.

## <a name="new-function-related-monitoring-metrics"></a>Új függvény kapcsolatos felügyeleti mérőszámok

Értékáram-elemzés feladat Monitor területen három további függvény kapcsolatos mértékek van hozzáadva. Az alábbi ábrán látható módon FÜGGVÉNY KÉRELMEKET, FÜGGVÉNY események és sikertelen FÜGGVÉNY kérelmek.

![Adatfolyam analitikai és gépi tanulási függvények mértékek méretezése] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Adatfolyam analitikai és gépi tanulási függvények mértékek méretezése")

A rendszer a következők:

**FÜGGVÉNY KÉRÉSEK**: függvény kérések száma.

**FÜGGVÉNY-események**: A szám eseményeket, a függvény-összehívásokban.

**Sikertelen FÜGGVÉNY kérelmek**: nem sikerült függvény kérések száma.

## <a name="key-takeaways"></a>Fő Takeaways  

A fő pontok összefoglalva annak érdekében, hogy egy Értékáram-elemzés feladat gépi tanulási függvényekkel méretezni, az alábbiakat kell figyelembe venni:

1.  A bemeneti esemény ráta
2.  A megengedett időtartam a futó Értékáram-elemzés feladat (és így a gépi tanulási webes szolgáltatáskérések köteg méretének)
3.  Adatfolyam-elemző kiépített SUs és gépi tanulási webes szolgáltatáskérések (függvény kapcsolatos többletköltségek) száma

Értékáram-elemzés teljesen particionált lekérdezés példaként használták. Ha összetettebb lekérdezés van szükség az [Azure Értékáram-elemzés fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) nagyszerű forrásul szolgálhat további segítségre van szüksége ahhoz, hogy a Értékáram-elemzés csoport.

## <a name="next-steps"></a>Következő lépések

Értékáram-elemzés kapcsolatos további tudnivalókért lásd:

- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
