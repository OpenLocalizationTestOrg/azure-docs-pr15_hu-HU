<properties
    pageTitle="Log Analytics adathasználat elemzése |} Microsoft Azure"
    description="Log Analytics a használat lap használatával megtekintheti, hogy mennyi adatot küldi el a MOBILE szolgáltatást."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>A napló Analytics adathasználat elemzése

Log Analytics a tevékenységek kezelése csomagja (MOBILE) által gyűjtött adatokat, és elküldi azt a MOBILE szolgáltatás rendszeres időközönként.  A **használat** lap használatával megtekintheti, hogy mennyi adatot küldi el a MOBILE szolgáltatást. A **használat** lap is megtudhatja, hogy mennyi adatot küldött naponta megoldások és milyen gyakran a kiszolgálók adatokat küldi.

>[AZURE.NOTE] Ha a [webhely MOBILE](http://www.microsoft.com/oms)használatával létrehozott ingyenes fiókkal rendelkezik, Ön legfeljebb 500 MB-os adatokat küld a MOBILE szolgáltatás naponta. A napi korlátot elér, a adatelemzés leállítása, és folytathatja a következő napra elején. Még nem fogadta el vagy MOBILE által feldolgozott adatok újbóli kell.

Megtekintheti a használatát, a **használatát** csempére az **Áttekintés** irányítópulton a MOBILE használatával.

![használatát csempe](./media/log-analytics-usage/usage-tile.png)

Ha túllépte a napi használatát engedélyezett, vagy ha megközelíti a korlátot, csökkentheti a MOBILE szolgáltatásnak küldött adatok mennyiségét megoldást tetszés szerint eltávolíthatja. Megoldások eltávolításával kapcsolatos további tudnivalókért lásd: [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md).

![használati irányítópultján](./media/log-analytics-usage/usage-dashboard.png)

A **használat** lap jeleníti meg az alábbi adatokat:

- Napi átlagos használat
- Adatok felhasználása minden megoldást keresztül az elmúlt 30 nap
- Mennyi adatok a kiszolgálókat az környezetben az elmúlt 30 nap keresztül küld a MOBILE szolgáltatás
- A réteg és becsült költség árak mobilinternet-előfizetéssel
- Információ a szolgáltatásiszint-szerződés (SLA), például, hogy mennyi ideig tart a feldolgozása az adatok MOBILE

## <a name="to-work-with-usage-data"></a>Használatát adatokkal végzett munkához

1. Az **Áttekintés** oldalon kattintson a **használat** csempére.
2. A **használat** lapon, hogy Ön érintett területek megjelenítése használatát kategóriák megjelenítése.
3. Ha van használata más túl sok a napi feltöltés kvóta megoldás, akkor fontolja meg, hogy a megoldás eltávolítása.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>A becsült költség és számlázási információk megtekintése
1. Az **Áttekintés** oldalon kattintson a **használat** csempére.
2. A **használati** **használatát** lapján kattintson a sávnyíl (**>**) **becsült költség**mellett.
3. A kibontott a részletek **a mobilinternet-előfizetéssel** , megjelenik a becsült havi költség.  
    ![A mobilinternet-előfizetéssel](./media/log-analytics-usage/usage-data-plan.png)
4. Ha szeretné megtekinteni a számlázási adatait, kattintson a **számlájának megtekintése** az előfizetési adatokat megtekintéséhez.
    - Az előfizetések lapon kattintson az előfizetés részletei és a használatát sortétel listájának megtekintéséhez.  
        ![előfizetés](./media/log-analytics-usage/usage-sub01.png)
    - Előfizetéshez tartozó összesítő lapon végezheti el a tevékenységek kezelése és megtekintése a többet szeretne tudni az előfizetés számos.  
        ![az előfizetés részletei](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>A SZOLGÁLTATÁSISZINT-adatok kötegekben megtekintése
1. Az **Áttekintés** oldalon kattintson a **használat** csempére.
2. **Szolgáltatási szint szerződés** **szolgáltatásiszint-szerződés letöltése Részletek**gombra.
3. Az Excel XLSX-fájl le van véleményezésére.  
    ![SLA részletei](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Következő lépések

- Lásd: a [napló keresések napló Analytics](log-analytics-log-searches.md) megoldások által gyűjtött részletes adatok megjelenítése.
