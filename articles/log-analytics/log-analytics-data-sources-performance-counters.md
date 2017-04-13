<properties 
   pageTitle="A Windows és Linux teljesítményszámlálók napló Analytics |} Microsoft Azure"
   description="Teljesítmény számláló gyűjti össze a Windows és Linux ügynökök-teljesítmény elemzése a napló elemzést.  Ez a cikk ismerteti, hogyan mindkét Windows teljesítmény teljesítménymutatókat webhelycsoport konfigurálása és Linux ügynökök, azok részleteit a MOBILE tárat, és a MOBILE portálon elemzéséhez hogyan tárolja."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>A napló Analytics Linux és a Windows teljesítmény adatforrások 

A Windows és Linux teljesítmény számláló hardver-összetevők, operációs rendszerek és alkalmazások teljesítményének betekintést nyújt.  Log Analytics hosszabb kifejezés elemzéshez teljesítmény adatok összesítése és jelentés túl közel valós idejű (NRT) elemzéshez rendszeres időközönként összegyűjtheti a teljesítmény számláló.

![Teljesítmény számláló](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Teljesítmény számláló konfigurálása

Állítsa be az [Adatok menü, a Analytics beállítások](log-analytics-data-sources.md#configuring-data-sources)származó teljesítménymutatókat.

Amikor először állítja be a Windows vagy Linux teljesítmény számláló új MOBILE munkaterületre vonatkozó, kapnak a a vezérlőt, amellyel gyorsan létrehozhat számos, gyakori számláló.  Ezeket megtalálja az egyes jelölőnégyzetét.  Győződjön meg arról, hogy semmilyen létre szeretne hozni az eredetileg a számlálót be van jelölve, és kattintson a **a kijelölt teljesítmény Számláló hozzáadása**.

![A Windows teljesítmény számláló konfigurálása](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Kövesse az alábbi eljárást hozzáadása egy új Windows teljesítmény számláló gyűjthetők össze.

1. Írja be a számláló nevét a szövegmezőbe, kattintson a formátum *\counter objektum (példány)*.  Amikor elkezdi beírni, és a megfelelő közös számláló is megjelenik.  Kiválaszthat egy számláló vagy a listából, vagy írjon be egy saját.  Egy adott számláló az összes előfordulását *object\counter*megadásával is visszaadhat. 
2. Kattintson a **+** , vagy nyomja le az **ENTER billentyűt** a Számláló hozzáadása a listához.
3. Amikor felvesz egy számláló, a **Minta intervallum**használja az alapértelmezett 10 másodperc.  Módosíthatja a legfeljebb 1800 másodperc (30 perc) a nagyobb értékre, ha a tárhelyre az összegyűjtött teljesítményadatok csökkenteni szeretné.
4. Amikor elkészült a számláló felvételét, kattintson a konfigurációjának mentése a képernyő tetején a **Mentés** gombra.

![Linux teljesítmény számláló konfigurálása](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Kövesse az alábbi eljárást hozzáadása egy új Linux teljesítmény számláló gyűjthetők össze.

1. Összes konfigurációs módosítást alapértelmezés szerint automatikusan tolni összes anyagokkal.  Konfigurációs fájl Linux anyagok, az Fluentd adatgyűjtő küldi.  Ha szeretné módosítani a fájlt kézzel minden Linux ügynök, majd törölje a jelet a *alkalmaz a Linux gépek konfigurációs alatti*mezőbe.
2. Írja be a számláló nevét a szövegmezőbe, kattintson a formátum *\counter objektum (példány)*.  Amikor elkezdi beírni, és a megfelelő közös számláló is megjelenik.  Kiválaszthat egy számláló vagy a listából, vagy írjon be egy saját.  
2. Kattintson a **+** , vagy nyomja le az **ENTER billentyűt** a Számláló hozzáadása a többi számláló az objektum listáját.
3. Az objektum összes számláló azonos **Minta intervallum**használja.  Az alapértelmezett érték 10 másodperc.  Akkor módosítsa az legfeljebb 1800 másodperc (30 perc) nagyobb értéket, ha a tárhelyre az összegyűjtött teljesítményadatok csökkenteni szeretné.
4. Amikor elkészült a számláló felvételét, kattintson a konfigurációjának mentése a képernyő tetején a **Mentés** gombra.

## <a name="data-collection"></a>Adatok gyűjtése

Log Analytics gyűjti össze, a számláló telepített rendelkező összes ügynökök minden megadott teljesítmény számláló a minta megadott időközönként.  Az adatokat a program nem összesíti, és a nyers adatokból érhető el az összes napló keresési nézetben MOBILE előfizetése által megadott időtartama.


## <a name="performance-record-properties"></a>Teljesítmény rekord tulajdonságai párbeszédpanelen

Teljesítmény rekordok **Perf** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| Számítógép         | Számítógép, amely az esemény amelyet gyűjtött. |
| CounterName      | A teljesítmény számláló neve |
| Számláló_elérési_útja      | Teljes elérési útját a képernyőn a számláló \\ \\ \<számítógép >\\objektum(példány)\\számláló. |
| Ellenértéknek     | A számláló numerikus értéket.  |
| Példánynév     | Esemény-példány nevét.  Ha nem üres. |
| Objektumnév       | A teljesítmény objektum neve |
| SourceSystem  | Az összegyűjtött ügynök az adatok típusát. <br> Csatlakozás OpsManager – Windows agent, vagy közvetlen vagy SCOM <br> Linux – az összes Linux ügynökök beállítása  <br> AzureStorage – Azure diagnosztika |
| TimeGenerated       | Dátum és idő, az adatok mintát volt. |


## <a name="sizing-estimates"></a>Méretező ad becslést.

 Az egy adott számláló gyűjteménye, 10 másodperc időközönként durva becslés példányonként napi körülbelül 1 MB.  Egy adott számláló képlete a következő tároló követelményeinek is megbecsüli.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Log keresések teljesítmény rekordok

Az alábbi táblázat, amely a teljesítmény rekordjával napló keresések különböző példákat.

| Lekérdezés | Leírás |
|:--|:--|
| Típus = Perf | Az összes teljesítményadatokat |
| Típus = Perf számítógép = "Sajátgép" | Egy adott számítógépre teljesítményt adatok |
| Típus = Perf CounterName = "Lemez várólista jelenlegi hossza" | Egy adott számláló az összes teljesítményadatokat |
| Típus = Perf (objektumnév = processzor) CounterName = "% processzor idő" példánynév = _Total & #124; Avg(Average), mint a számítógép AVGCPU mérték | Átlagos Processzor kihasználtsági összes számítógépei között |
| Típus = Perf (CounterName = "% processzor idő") & #124;  Mérje le a számítógép max(Max) | Maximális Processzor kihasználtsági összes számítógépei között |
| Típus = Perf objektumnév logikai CounterName = "Aktuális lemezre várólista hossza" számítógép = = "MyComputerName" & #124; Mérje le Avg(Average) példánynév szerint | Egy adott számítógép minden példányára átlagos aktuális lemez várólista hossza |
| Típus = Perf CounterName = "DiskTransfers/sec" & #124; Mérje le a számítógép percentile95(Average) | 95 PERCENTILIS az átvitelek/mp összes számítógépei között |
| Típus = Perf CounterName = "% processzor idő" példánynév = "_Total" & #124; Mérje le a avg(CounterValue) számítógép időköze 1 óra értéket | Az összes számítógépei között processzorhasználata óránkénti átlaga |
| Típus = Perf számítógép = "Sajátgép" CounterName = % * példánynév = _Total & #124; Mérje le percentile70(CounterValue) CounterName időköze 1 óra értéket | Egy adott számítógéphez tartozó minden %-os készültségi számláló óránkénti 70 PERCENTILIS |
| Típus = Perf CounterName = "% processzor idő" példánynév = "_Total" (számítógép = "Sajátgép") & #124; Mérje le a min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) számítógép időköze 1 óra értéket | Óránkénti átlag, minimum, maximális és 75-PERCENTILIS processzorhasználata egy adott számítógépen |

## <a name="viewing-performance-data"></a>A teljesítményadatok megtekintése

Log kereshet az Office.com teljesítményadatokat futtatásakor alapértelmezés szerint a **napló** nézetben jelenik meg.  Az adatok grafikus formában **Mértékek**gombra.  A részletes grafikus nézet, kattintson a **+** számláló mellett.  

![Összecsukott mértékek megtekintése](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Ha a kijelölt időtartomány 6 órával vagy annál kisebb, a diagram minden néhány másodpercet frissül.  Az adatokon világoskék a diagram jobb oldalán megjelenik.

![Élő adatokat tartalmazó kibontott mértékek megtekintése](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

A napló keresés teljesítményadatokat összesítése, akkor olvassa el az [igény szerinti metrikus összesítésére és a Megjelenítés a MOBILE-ban](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Következő lépések

- [Log keresések](log-analytics-log-searches.md) megismerheti az adatforrások és megoldások gyűjtött adatok elemzése céljából.  
- [A Power BI](log-analytics-powerbi.md) további megjelenítések és elemzéshez gyűjtött adatok exportálása.