<properties 
    pageTitle="Ismertetése adatfolyam Analytics feladat figyelése |} Microsoft Azure" 
    description="Adatfolyam-elemző feladat figyelése ismertetése" 
    keywords="lekérdezés monitor"
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

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Értékáram-elemzés feladat figyelemmel kísérésére és a lekérdezések figyelése ismertetése

## <a name="introduction-the-monitor-page"></a>Bevezetés: A monitor lap

Az Azure adatkezelési portál és az Azure-portálon is felszíni fő teljesítménymutatók figyelésére és a lekérdezés és a feladat teljesítményével kapcsolatos hibák elhárításának használható. 

Az Azure adatkezelési portálon kattintson a futó Értékáram-elemzés feladatok lásd: ezek a mértékek **Monitor** lapján. Nincs a teljesítménymutatók jelenik meg a Monitor lapon a legfeljebb 1 perc késés.  

  ![Feladat irányítópult figyelése](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Az Azure-portálon nyissa meg a megjelenítő Értékáram-elemzés feladat megnéz mértékek számára kíváncsi, és tekintse meg a **Figyelés** szakaszát.  

  ![Azure portál figyelése feladat irányítópult](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

Értékáram-elemzés feladat jön létre a régió, először szüksége lesz a szolgáltatáshoz való konfigurálása diagnosztika régió. Ehhez kattintson bárhová a **Figyelés** szakasz és a **diagnosztikai** lap jelenik meg. Itt engedélyezheti a diagnosztika, és adja meg a tárterület-fiók-adatok figyelése.  

  ![Azure portál konfigurálása lekérdezés diagnosztika](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Értékáram-elemzés elérhető mérőszámok


| Metrikus | Meghatározása |
|--------|-------------|
| Kihasználtsága SZU (%) | A folyamatos átvitelű egység(ek) hasznosítása a méret lapon a feladat rendelve egy feladatot. Érdemes a kijelzőre elérje a 80 %-os, vagy a fenti nagy valószínűséggel, hogy az esemény feldolgozása késleltethető vagy leállt végez folyamatban van. |
| Beviteli események | Értékáram-elemzés feladat események száma szerint kapott adatok mennyiségét. Ennek ellenőrzéséhez, hogy a bemeneti forrás események küldött használható. |
| Kimeneti események | A kimenet célba események száma a Értékáram-elemzés feladat által küldött adatok mennyiségét. |
| Kimenő sorrendben események | Ki a sorrend akár kihagyott és egy korrigált időbélyeg, az esemény rendezés házirend alapján megadott kapott események száma. Ez a beállítás ki a rendelés hibatűrést ablak beállításai befolyásolhatja. |
| Adatkonverziós hibák | Értékáram-elemzés feladat felmerült adatkonverziós hibák száma. |
| Futási idejű hiba | Hibák, amelyek akkor aktiválódnak Értékáram-elemzés projekt végrehajtásakor száma. |
| Késői beviteli események | Késői érkező a forrást, amely vagy eltávolítja, vagy azok időbélyeg események száma be van állítva, a késői érkezése hibatűrést ablak beállítás esemény rendezés házirend konfigurációja miatt. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Figyelés testreszabása az Azure adatkezelési portálon ##

6 mértékek felfelé megjeleníthetők a diagramon.

Váltás a relatív értékeket (végleges értéke csak egyes mérőszám), és abszolút érték (Y tengely jelenik meg) jeleníti meg, jelölje be a relatív és abszolút tetején látható a diagramon.

  ![A relatív, abszolút lekérdezés monitor](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Mértékek azonban megtekinthetők a Monitor diagramon az összesítésekben az 1 órás, 12 órás éjjel vagy napján.

A időtartomány módosítása a mértékek diagram jeleníti meg, jelölje be 1 óra elteltével a 24 óra vagy napján tetején látható a diagramon.

  ![Lekérdezés monitor időosztás](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

Beállíthatja, hogy felhívhatja a mailben abban az esetben, ha a feladat egy megadott küszöbértékét metszéspontja szabályokat. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Az Azure-portálon figyelése testreszabása ##

Módosítsa a típusú diagramot, látható, a mérőszámok, és a diagram szerkesztése beállításai tartomány idő. A részletekért megtudhatja, [hogy miként figyelése testreszabása](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Azure portál lekérdezés Monitor időosztás](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Feladat állapota

Értékáram-elemzés feladatok állapotát megtekintheti az Azure klasszikus portálon, ahol a feladatok listájának megtekintése. A feladatok listáját megtekintheti az Azure klasszikus portálon Értékáram-elemzés ikonra kattintva.

| Állapot | Meghatározása |
|--------|------------|
| Létrehozva | A feladat létrehozás, azonban nem lett elindítva. |
| Indítása | A felhasználó rákattintott indításkor a feladatot, és a projekt indítása |
| Fut | A feladat kiosztva, beviteli feldolgozása, vagy a beviteli feldolgozása várakozik. Ha a feladat futtatása állapot kimeneti előállító nélkül jeleníti meg, akkor valószínű, hogy az adatkezelési időkeret nagy, vagy a lekérdezés logika bonyolult. Másik oka lehet, hogy jelenleg nem küldi el a feladat adatokat. |
| Leállítása | Egy felhasználó a Leállítás gombra kattintva a feladatot, és a feladat leállítása gombra. |
| Leállítva | A feladat leállt. |
| Csökkent | Ez az állapot jelzi, hogy egy Értékáram-elemzés feladat tranziens hibák léptek (az ex. A bemeneti és kimeneti hibák feldolgozása, hibák átalakítási hiba stb.). A feladat még mindig fut, azonban nincsenek most generált hibák sok. A feladat ügyfél beavatkozást igényel, és az ügyfél láthatja a műveletek naplók a hibákat. |
| Nem sikerült. | Ez azt jelzi, hogy a feladat hibák miatt nem sikerült, a feldolgozás leállt. Az ügyfél kell vizsgálja meg a a műveletek naplók, hogy annak érdekében, hogy a hibák hibakeresési. |
| Törlése | Ez azt jelzi, hogy a feladat törlődik. |

## <a name="diagnosis"></a>Diagnosztikai

Azure kezelőportálja a feladat irányítópult tájékoztatást ad meg, hogy hol kell keresse meg a exportálja a diagnosztikai, azaz a ráfordítások és/vagy a műveletek jelentkezzen. A hivatkozást választva megnyithatja a megfelelő helyre a diagnosztikai vizsgálata, hogy kattint.

  ![Lekérdezés monitor hiba](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Diagnosztikai információ gombra kattintva a bemeneti és kimeneti erőforráson biztosít. Ez frissülnek a legújabb adatokkal diagnosztizálása a feladat futtatása közben.

  ![Lekérdezés diagnosztika](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
