<properties
    pageTitle="Értékáram-elemzés feladatok növelheti a teljesítményt méretezni |} Microsoft Azure"
    description="Megtudhatja, hogy miként méretezheti Értékáram-elemzés feladatok beviteli partíciók konfigurálása, a lekérdezésdefiníció beállítása és a folyamatos átvitelű egységek feladat beállításával."
    keywords="a folyamatos átvitelű, adatok analytics adatfeldolgozás streaming finomhangolása"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Azure Értékáram-elemzés feladatok adatfeldolgozás-átviteli adatfolyam növeléséhez méretezése

Megtudhatja, hogy miként finomhangolása analytics feladatok és *a folyamatos átvitelű egységek* kiszámításához Értékáram-elemzés, miként méretezheti Értékáram-elemzés feladatok beviteli partíciók konfigurálása, a analytics Lekérdezésdefiníció beállítása és a folyamatos átvitelű egységek feladat. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Mik azok a Értékáram-elemzés feladat részei?
Értékáram-elemzés feladat definícióját bemeneti adatok alapján, a lekérdezés és a kimeneti tartalmazza. Bemenetben származó, ahol az a feladat beolvassa a adatfolyam vannak, a lekérdezés átalakítása beviteli adatfolyam-alapú és a kimeneti, ahol az a feladat küld a feladat között.  

A feladat legalább egy, a bemeneti forrás adatok streaming igényel. Az adatfolyamban beviteli adatforrás tárolhatók az Azure Service Bus esemény központi vagy Azure Blob-tárolóhoz. További tudnivalókért olvassa el a [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md) és [Azure Értékáram-elemzés használatának első lépései](stream-analytics-get-started.md)című témakört.

## <a name="configuring-streaming-units"></a>Adatfolyam egységek konfigurálása
Adatfolyam egységek (SUs) jelenítik meg az erőforrások és a számítási teljesítményt az Azure Értékáram-elemzés feladat végrehajtásához szükséges. SUs írja le a Processzor, a memóriahasználat, a kombinált mértéke alapján kapacitás feldolgozása relatív eseményt ad lehetőséget, és és olvasási díjak. Minden egyes adatfolyam egység nagyjából 1MB/második átviteli felel meg. 

Hány SUs kiválasztása a ráfordítások és a lekérdezés a projektre vonatkozóan definiált partíciót konfigurációjától függ, egy adott munkához szükség. Megadhatja, hogy az erőforrás-mennyiség feladat streaming az Azure klasszikus portál használatával a kvóta felfelé. Egyes Azure előfizetések alapértelmezés szerint legfeljebb 50 adatfolyam egység összes analytics feladatokhoz kvóta adott régióban tartalmaz. Ha növelni szeretné a előfizetésekhez adatfolyam mennyiség, lépjen kapcsolatba [A Microsoft támogatási](http://support.microsoft.com).

A feladat kihasználó adatfolyam egységek számát a ráfordítások és a lekérdezés a projektre vonatkozóan definiált partíciót konfigurációjától függ. Is figyelje meg, hogy adatfolyam Darabszámok érvényes értéket kell használni. Az érvényes értékek indítása 1, 3, 6, majd felfelé az időosztáson 6, az alább látható módon.

![Adatfolyam-mennyiség skála Azure Értékáram-elemzés][img.stream.analytics.streaming.units.scale]

Ez a cikk bemutatja, hogyan kiszámítása, és a lekérdezés növelheti a teljesítményt analytics feladatokhoz beállítását.

## <a name="embarrassingly-parallel-job"></a>A párhuzamos embarrassingly feladat
A párhuzamos embarrassingly feladat a legtöbb méretezhető forgatókönyvet az Azure Értékáram-elemzés van. A kimenet egy partíciót egy partíciót egy példányát a lekérdezés bemeneti csatlakozik. Néhány dolog, amit a párhuzamos eléréséhez szükséges:

1.  Ha a lekérdezés logika ugyanazt a kulcsot a lekérdezés példányt által feldolgozott függ, majd gondoskodnia kell arról, hogy az események lépjen a ugyanarra a bemeneti partíciót. Esemény agyváltók, amíg ez azt jelenti, hogy **PartitionKey** meg kell szerepelnie az esemény adatai particionált feladók is használhatja. A Blob Ez azt jelenti, hogy az események elküldi a partíciót mappába. Ha a lekérdezés logika nem igényel ugyanazt a kulcsot feldolgoztatni a lekérdezés-példányt, majd figyelmen kívül hagyhatja a követelmény. Ez a példa egy egyszerű select vagy project/szűrés lekérdezés lenne.  
2.  Az adatok kártyaformátumban, mint a bemeneti oldalon kell, miután biztosítása, hogy a lekérdezés particionálva van szükség. Ehhez az összes lépését **Partíciót szerint** használni. Több lépést engedélyezettek, de ezek mind ugyanazon a billentyű kell partícionálni. Megjegyzés: a másik megoldás,, hogy jelenleg a particionáló billentyűt van szüksége, hogy a teljes mértékben a párhuzamos feladat **PartitionId** kell állítani.  
3.  Jelenleg csak az esemény hubok és Blob támogatja particionált kimeneti. Esemény hubok kimeneti frissítenie kell a **PartitionKey** mezőre, amely **PartitionId**konfigurálása. A Blob nem kell tennie semmit sem.  
4.  Jegyezze fel a bemeneti partíciók számának egyeznie kell a partíciók kibocsátás száma, a másik megoldás. BLOB kimeneti jelenleg nem támogatja a partíciók, de ez nem probléma, mert a particionálási sémát a felsőbb szintű lekérdezés örökli. Példák, amelyek lehetővé teszik, hogy a teljes mértékben a párhuzamos feladat partíciót értékek:  
    1.  8 esemény hubok bemeneti partíciót és 8 esemény hubok partíciók kibocsátás
    2.  8 esemény hubok bemeneti partíciók és Blob kimeneti  
    3.  8 blob beviteli partíciót és Blob kimeneti  
    4.  8 blob beviteli partíciót és 8 esemény hubok kimeneti partíciót  

Az alábbiakban néhány példák, amelyek embarrassingly párhuzamos.

### <a name="simple-query"></a>Egyszerű lekérdezés
Beviteli – esemény hubok 8 partíciók kibocsátás együtt – esemény központi 8 partíciót a

**Lekérdezés:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

A lekérdezés egy egyszerű szűrő és ilyenként, akkor nem kell aggódnia amiatt, hogy a bemeneti elküldjük Önnek az esemény hubok szétválasztás. Megfigyelheti, hogy a a lekérdezés **Partíciót által** **PartitionId**, a tartoznak, így azt teljesítése követelmény #2 a fentiektől. A kibocsátás az esemény hubok kimeneti beállítása a feladatot a **PartitionKey** mező **PartitionId**meg van szükség. Egy utolsó jelölőnégyzetet, a bemeneti partíciók kibocsátás partíciót ==. Ez a topológia embarrassingly párhuzamos.

### <a name="query-with-grouping-key"></a>A csoportosítás billentyűvel lekérdezés
Beviteli – esemény hubok 8 partíciók kibocsátás együtt – Blob

**Lekérdezés:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ezt a lekérdezést egy csoportosítási billentyűt, és ilyen ugyanazt a kulcsot feldolgoztatni a lekérdezés példányt. Ez azt jelenti, hogy azt el kell küldenie a események események hubok particionált módon. Melyik kulcs tegye azt érdeklő? A valós billentyűt, hogy érdeklő **PartitionId** fogalma feladat logika, **TollBoothId**. Ez azt jelenti, hogy a **PartitionKey** beállíthatja azt az esemény adatok azt küldése esemény hubok kell a **TollBoothId** az esemény. A lekérdezés van **Partíciót által** **PartitionId**, az így jó is van. A kimenet mivel ez éppen Blob, azt nem kell aggódnia **PartitionKey**konfigurálása. #4 követelmény ismét az Blob, így azt nem kell aggódnia. Ez a topológia embarrassingly párhuzamos.

### <a name="multi-step-query-with-grouping-key"></a>A csoportosítás billentyűvel több lépés lekérdezés ###
Beviteli – esemény központi 8 partíciók kibocsátás – 8 partíciót az esemény központi

**Lekérdezés:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ezt a lekérdezést egy csoportosítási billentyűt, és ilyen ugyanazt a kulcsot feldolgoztatni a lekérdezés példányt. Az azonos stratégia az előző lekérdezés használhatja azt. A lekérdezés több lépést foglal magában. Minden egyes lépés **Partíciót által** a **PartitionId**rendelkezik? Igen, így a jó azt. A kimenet szükség beállítása a **PartitionKey** **PartitionId** , például a fent ismertetett, és azt is látható, mint a bemeneti partíciót ugyanannyi rendelkezik. Ez a topológia embarrassingly párhuzamos.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Példák, amelyek nem embarrassingly párhuzamos

### <a name="mismatched-partition-count"></a>Nem egyező partíciót száma ###
Beviteli – esemény hubok 8 partíciók kibocsátás együtt – esemény központi 32 partíciót a

Mindegy, hogy mi a lekérdezés oka az, ebben az esetben a bemeneti partition darab! kimeneti partition count =.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Nem használja az esemény hubok és BLOB ezt az eredményt
Beviteli – esemény hubok 8 partíciók kibocsátás együtt – PowerBI

PowerBI kimeneti jelenleg nem támogatja a szétválasztás.

### <a name="multi-step-query-with-different-partition-by-values"></a>Több lépés lekérdezés más értékekkel partíciót szerint
Beviteli – esemény központi 8 partíciók kibocsátás – 8 partíciót az esemény központi

**Lekérdezés:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Amint látható, a második lépés **TollBoothId** használja a particionáló billentyűt. Ez jelenleg nem ugyanaz, mint az első lépés, és ezért szüksége számunkra, hogy végezze el a sorrendű. 

Néhány példa és counterexamples tudják elérni egy embarrassingly párhuzamos topológia Értékáram-elemzés feladatok és az általa ezek maximális méret lehetőségeit. Amelyek nem illeszkednek a profilok közül feladatokhoz standardként méretezni a kanonikus Értékáram-elemzés forgatókönyvek részét részletező jövőbeli frissítéseit történik.

Most helyezése használja az alábbi általános útmutató:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>A maximális mennyiség feladat streaming kiszámítása
Értékáram-elemzés feladat használt adatfolyam egységek száma attól függ, hogy a számát a feladatot, és a számot az egyes lépések partíciók definiált lekérdezés című témakör lépéseit.

### <a name="steps-in-a-query"></a>A lekérdezés lépések
A lekérdezés beállíthatja, hogy egy vagy több lépéseket. Minden egyes lépés az **WITH** kulcsszó alszint lekérdezését. A csak lekérdezést, amely kívül esik a **WITH** kulcsszó is számítanak lépés, például a **SELECT** utasítás a következő lekérdezést:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Az előző lekérdezés két lépést foglal magában.

> [AZURE.NOTE] Ez a példa lekérdezés részletesen a cikk későbbi részében.

### <a name="partition-a-step"></a>Partition lépés

Az alábbi feltételek szétválasztás lépés van szükség:

- A beviteli forrás kell partícionálni. További információ: az [Esemény hubok programozási útmutató](../event-hubs/event-hubs-programming-guide.md).
- A **SELECT** utasítás a lekérdezés particionált beviteli adatforrásokból kell olvasni.
- A lekérdezés a lépésen belül **Partíciót által** kulcsszó kell rendelkeznie.

Lekérdezés particionálva van, ha a bemeneti események feldolgozása és az összesített külön partíciót csoportok lesz, és a kimeneti értékeket események jönnek létre az egyes csoportok. Ha egy kombinált összesítés célszerű, létre kell hoznia egy másik második lépés összesíteni.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>A maximális mennyiség feladat streaming kiszámítása

Minden másik lépést közös méretezheti legfeljebb hat adatfolyam egységek Értékáram-elemzés feladathoz. Hozzáadása a folyamatos átvitelű egységek, további lépés kell partícionálni. Egyes partíciók hat adatfolyam egységek állhat.

<table border="1">
<tr><th>A feladat lekérdezés</th><th>A folyamatos átvitelű a feladat egységek maximális száma</th></td>

<tr><td>
<ul>
<li>A lekérdezés egy lépésben tartalmaz.</li>
<li>A lépés nem particionálva van.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>A beviteli adatfolyam particionált 3.</li>
<li>A lekérdezés egy lépésben tartalmaz.</li>
<li>A lépés particionálva van.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>A lekérdezés a két lépést tartalmaz.</li>
<li>A lépések egyike sem particionálva van.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Az adatbevitel adatfolyam particionált 3.</li>
<li>A lekérdezés a két lépést tartalmaz. A beviteli lépés particionálva van, és a második lépés nem.</li>
<li>A SELECT utasítás beolvassa a particionált bemeneti.</li>
</ul>
</td>
<td>24 (particionált lépést a 18) + 6 a lépések nem másik</td></tr>
</table>

### <a name="examples-of-scale"></a>Példák a méretezés
Az alábbi lekérdezés autós három tollbooths egy három perces ablakon belül a díjköteles állomáson keresztül számítja ki. A lekérdezés megadhat hat adatfolyam egységek felfelé.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

A lekérdezés további adatfolyam egységek használatához mindkét adatfolyam beviteli, és a lekérdezés kell partícionálni. Tekintve, hogy az adatok adatfolyam partíciót értéke 3, a következő módosítás lekérdezés felfelé a 18 adatfolyam egységek megadhat:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Lekérdezés particionálva van, ha a bemeneti események feldolgozása, és összesíti a csoportok külön partíciót. Kimeneti események is az egyes csoportok jönnek létre. Szétválasztás néhány nem várt eredmények okozhat, amikor a **Group by** mező nem áll a partíciót kulcs az adatbevitel adatfolyam. Például az előző példa lekérdezésre **TollBoothId** mezője nem Input1 partíciót billentyűt. Az adatokat a őrbódét #1-től a több partíciót húzza szét a is.

Minden Input1 partíciók fog feldolgoztatni külön-külön Értékáram-elemzés, és az ugyanabban az tumbling ablakban az azonos őrbódét autós-fázis-től száma több rekordot hoz létre. Ha nem lehet módosítani a bemeneti partíciót billentyűt, a probléma kijavíthatók további nem partíciót lépés, ha például hozzáadásával:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

A folyamatos átvitelű egységek 24 megadhat ezt a lekérdezést.

>[AZURE.NOTE] Két adatfolyamok fognak csatlakozni, győződjön meg arról, hogy a adatfolyamok a partíciót billentyűt az oszlop, megteheti az illesztés, és már partíciót ugyanannyi mindkét adatfolyam megjelenítését a vannak-e particionálva.


## <a name="configure-stream-analytics-job-partition"></a>Értékáram-elemzés feladat partíciót konfigurálása

**Módosítsa a feladat adatfolyam egység**

1. Jelentkezzen be az [adatkezelési portál](https://manage.windowsazure.com).
2. **Értékáram-elemzés** kattintson a bal oldali ablaktáblában.
3. Kattintson a Értékáram-elemzés feladatra, amelyet szeretne méretezni.
4. Kattintson a **MÉRETEZÉS** elemre a lap tetején.

![Adatfolyam-mennyiség skála Azure Értékáram-elemzés][img.stream.analytics.streaming.units.scale]

Az Azure-portálon méretarány-beállításai a beállítások is elérhető:

![Azure portál Értékáram-elemzés feladat konfigurálása][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Feladat teljesítményének figyelése

Az adatkezelési portál használatával, nyomon követheti a feladat a átviteli események/második:

![Azure Értékáram-elemzés monitor feladatok][img.stream.analytics.monitor.job]

A várt kapacitásának a terhelést a események/második számítja ki. Ha a átviteli várható-nél kisebb, finomhangolása a bemeneti partíciót finomhangolása a lekérdezés és további adatfolyam egységek hozzáadása a projekthez.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Analytics-átviteli adatfolyam a méretezés - málna Pi forgatókönyv


Ha meg szeretné érteni, hogy hogyan Értékáram-elemzés feladatok méretezheti értelmez feldolgozás kapacitása tipikus helyzetben keresztül több Streaming mennyiség, az alábbiakban egy kísérlet, amely szenzoradatokat (ügyfelek) küld a esemény elosztóhoz, feldolgozásával azt, és elküldi riasztás vagy statisztika olyan eredménye megegyezik egy másik, esemény-hubon keresztül csatlakozott.

Az ügyfél előállított szenzoradatokat küld esemény hubok Értékáram-elemzés JSON formátumban, és a kimeneti adatokat is JSON formátumban.  Hogyan a mintaadatok lenne a következőhöz hasonló:  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Lekérdezés: "jelzést küldjön amikor egyszerűsített ki kell kapcsolni."  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Átviteli mérési: Ebben a környezetben átviteli az idő (10 perc) rögzített méretű Értékáram-elemzés által feldolgozott bemeneti adatok mennyiségét. Legjobb feldolgozás átviteli a bemeneti adatok eléréséhez mindkét adatfolyam beviteli, és a lekérdezés kell partícionálni. **COUNT()**is szerepel a lekérdezést, hogy hány beviteli események feldolgozott mérjük. Annak érdekében, hogy a feladat nem egyszerűen Várakozás a bemeneti események hamarosan, a bemeneti esemény központi minden partíciót lett előre betöltött használata elegendő bemeneti adatok (300MB).  

Az alábbiakban az eredmények növekvő adatfolyam egységek számát, és a megfelelő partíciót esemény hubok számát.  

<table border="1">
<tr><th>Beviteli partíciót</th><th>A partíciók kibocsátás</th><th>Adatfolyam-mennyiség</th><th>Folyamatos átviteli
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![img.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure Értékáram-elemzés fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
