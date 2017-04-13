<properties
    pageTitle="Példák a közös használat mintázatok Értékáram-elemzés a lekérdezés |} Microsoft Azure"
    description="Gyakori Azure adatfolyam Analytics lekérdezés mintázatok "
    keywords="lekérdezés példák"
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
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>A lekérdezés közös Értékáram-elemzés szokásai példákat is tartalmaz

## <a name="introduction"></a>– Bevezetés

Értékáram-elemzés Azure lekérdezések SQL-szerű lekérdezés nyelven, amelyek [adatfolyam Analytics lekérdezési nyelv](https://msdn.microsoft.com/library/azure/dn834998.aspx) megfelelőire ismertetett kell megadni.  Ez a cikk ismerteti, több közös lekérdezés mintázatok valós esetek alapuló megoldásokat.  Folyamatban lévő, és továbbra is szerepelni fognak frissülni új mintázatok folyamatos kombinálásával.

## <a name="query-example-data-type-conversions"></a>Példa: adatok adattípus-konverzió korlátai
**Leírás**: az a tulajdonságokat típusú meg a bemeneti adatfolyam.
Például elérhető lesz az autós súly a bemeneti adatfolyam a karakterláncok és végrehajtásához INT alakíthatók át kell ÖSSZEGEZNIE, mint.

**Beviteli**:

| Helyezése | Idő | Súly |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Kimenet**:

| Helyezése | Súly |
| --- | --- |
| Honda | 3000 |

**Megoldás**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Magyarázat**: a súly mezőt egy CAST utasítást használatával adja meg a (lásd: támogatott adattípusok listáját [Itt](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Példa: segítségével hasonló/nem szeretne mintázat egyező
**Leírás**: Ellenőrizze, hogy az esemény egy mező értéke megfelel-e egy bizonyos minta, például a feladó, a kezdő és záró a 9-es licenc-táblák

**Beviteli**:

| Helyezése | LicensePlate | Idő |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA ÉS 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Kimenet**:

| Helyezése | LicensePlate | Idő |
| --- | --- | --- |
| Toyota | AAA ÉS 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Megoldás**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Magyarázat**: Ellenőrizze, hogy a LicensePlate mező értékét A kezdődik, majd a tetszőleges karakterlánc helyett nulla vagy több karaktert tartalmaz, és 9 végződése a LIKE kimutatás segítségével. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Példa: különböző esetek/értékek (HASZNÁLATIESET-utasítások) logika megadása
**Leírás**: Adja meg a különböző kiszámítása az egy bizonyos feltételeknek megfelelő mezőt.
Ha például leírást is karakterlánc-1, a speciális eset azonos tétele átadott hány autókat.

**Beviteli**:

| Helyezése | Idő |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Kimenet**:

| CarsPassed | Idő |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01-01T00:00:10.0000000Z |

**Megoldás**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Magyarázat**: az esetet záradék lehetővé teszi, hogy egy másik számítási bizonyos feltételeknek megfelelő (a eset az összesítő ablakban autós számának) adja meg, hogy mi.

## <a name="query-example-send-data-to-multiple-outputs"></a>Példa: adatok küldése a több kimeneti értékeket
**Leírás**: adatok küldése több kimeneti célok egyetlen feladat.
Például egy küszöb-alapú értesítés az adatok elemzése és archiválása összes eseményének blob-tárolóhoz

**Beviteli**:

| Helyezése | Idő |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Helyezése | Idő |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output2**:

| Helyezése | Idő | Darabszám |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Megoldás**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Magyarázat**: Ez az a INTO záradék Értékáram-elemzés a kimeneti értékeket Ez az utasítás írják az adatok körét.
Az első lekérdezés egy olyan eredménye, hogy ArchiveOutput nevű azt kapott adatok átadó.
A második lekérdezés tartalmaz néhány egyszerű összesítési és szűrés és az eredményt elküldi a későbbi figyelmeztető rendszert.
*Megjegyzés*: CTEs (azaz a kimutatások) eredményét újbóli használata több kimeneti kimutatásokban – további előnye, a bemeneti forrást kevesebb olvasót megnyitása azt.
Ha például 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Példa: egyedi értékek számolva
**Leírás**: jelennek meg az adatfolyam belül egy időkeret egyedi mezőértékek számának.
Például autós hány egyedi helyezése haladt 2 második ablakban a díjköteles stand keresztül?

**Beviteli**:

| Helyezése | Idő |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Kimenet:**

| Darabszám | Idő |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Megoldás:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Magyarázat:** Hajtanak végre, egy kezdeti összesítést egyedi megkönnyíti a darab és az ablak felett eléréséhez.
Azt végezze el azt minden egyedi értékek ablakban elérhető az azonos időbélyeg, majd a második összesítési ablakában kell lennie nem összesítéséhez 2 windows az első lépés a minimális használ – megadott hány gyártmányú egy összesítést.

## <a name="query-example-determine-if-a-value-has-changed"></a>Példa: határozza meg, ha az érték megváltozott#
**Leírás**: tekintse meg egy korábbi érték, amelyre állapítsa meg, hogy az aktuális például értéke az előző autó ugyanaz, mint az aktuális autós győződjön díjköteles utazás eltérő?

**Beviteli**:

| Helyezése | Idő |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Kimenet**:

| Helyezése | Idő |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Megoldás**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Magyarázat**: használata eltérés való be a bemeneti adatfolyam-vissza egy eseményt, és a helyezése érték. Hasonlítsa össze a győződjön meg az aktuális esemény, majd az esemény kimeneti, ha nem egyeznek.

## <a name="query-example-find-first-event-in-a-window"></a>Példa: keresés első esemény ablakban
**Leírás**: keresés első autós minden 10 perc intervallum?

**Beviteli**:

| LicensePlate | Helyezése | Idő |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Kimenet**:

| LicensePlate | Helyezése | Idő |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Megoldás**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Most vegyük végezze el az adott problémát, és a keresés első autó módosítása minden 10 perc időköz.

| LicensePlate | Helyezése | Idő |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Megoldás**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Példa: keresés utolsó esemény ablakban
**Leírás**: keresés utolsó autós minden 10 perc intervallum.

**Beviteli**:

| LicensePlate | Helyezése | Idő |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Kimenet**:

| LicensePlate | Helyezése | Idő |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Megoldás**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Magyarázat**: a lekérdezés a két lépésből áll: a legelső legújabb időbélyeg megtalálja a windows 10 perc. A második lépés az eredeti adatfolyam eseményeket egyező utolsó időbélyegeket minden ablakban keresse meg az első lekérdezés eredményei fűz össze. 

## <a name="query-example-detect-the-absence-of-events"></a>Példa: a távolléti események észleli
**Leírás**: Ellenőrizze, hogy adatfolyam nem tartalmaz értéket, amely megfelel egy bizonyos feltételeknek.
Például az azonos tétele 2 egymást követő autók írt díjköteles utazik 90 másodpercek?

**Beviteli**:

| Helyezése | LicensePlate | Idő |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA ÉS 999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | DEFINÍCIÓJA-987 | 2015-01-01T00:00:03.0000000Z |
| Honda | A 345 GHI | 2015-01-01T00:00:04.0000000Z |

**Kimenet**:

| Helyezése | Idő | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA ÉS 999 | ABC-123 | 2015-01-01T00:00:01.0000000Z |

**Megoldás**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Magyarázat**: használata eltérés való be a bemeneti adatfolyam-vissza egy eseményt, és a helyezése érték. Hasonlítsa össze a győződjön meg az aktuális esemény, majd az esemény kimeneti, ha megegyeznek, és eltérés használata az előző autós adatait.

## <a name="query-example-detect-duration-between-events"></a>Példa: különbségét események észleli
**Leírás**: keresse meg az adott esemény időtartamát. Például a webes clickstream az adott határozza meg valamelyik funkciójával töltött időt.

**Beviteli**:  
  
| Felhasználói | A szolgáltatás | Esemény | Idő |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Indítása | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Vége | 2015-01-01T00:00:08.0000000Z |
  
**Kimenet**:  
  
| Felhasználói | A szolgáltatás | Időtartam |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Megoldás**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Magyarázat**: utolsó függvénnyel utolsó időértéket beolvasásának esemény típusa lett indítása". Figyelje meg, hogy utolsó függvény használja PARTÍCIÓT felhasználónként [], jelezve, hogy eredmény egyedi felhasználónként kell számítani.  A lekérdezés "Start" és "leállítása események közötti időkülönbség 1 óra maximális küszöbértékét rendelkezik, de állítható be, mint a szükséges (korlát DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Lekérdezés példa: egy feltétel időtartama feltárása
**Leírás**: Keresse ki mennyi feltétel fordult elő.
Tegyük fel, hogy egy hiba, az összes autók egy helytelen súly problémákat okozó hibát időtartamának kiszámításához szeretnénk (fölött 20 000 font) –.

**Beviteli**:

| Helyezése | Idő | Súly |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Kimenet**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Megoldás**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Magyarázat**: használja a bemeneti adatfolyam megtekintése 24 órát, és keresse meg a eltérés hol vannak a StartFault és StopFault átnyúló által példányok weight < 20000.

## <a name="query-example-fill-missing-values"></a>Példa: Töltse ki a hiányzó értékek
**Leírás**: az eseményeket, amelyek a hiányzó értékeket adatfolyam, a konzerv rendszeres időközönként események adatfolyam.
Esemény például készítése a legutóbb látható adatpont jelentést 5 másodpercenként.

**Beviteli**:

| Kétmintás t | érték |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

A **kimenet (az első 10 sorok)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Megoldás**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Magyarázat**: ezt a lekérdezést hoz létre események 5 másodpercenként, és az utolsó esemény előtt mentsen fog kimeneti. [Hopping ablak] (https://msdn.microsoft.com/library/dn835041.aspx "Hopping ablak - Azure Értékáram-elemzés") időtartama határozza meg, hogy a lekérdezés keresése a legújabb esemény (ebben a példában a 300 másodperc) jelenik meg, milyen távol vissza.


## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
