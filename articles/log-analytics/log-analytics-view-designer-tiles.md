<properties
    pageTitle="Jelentkezzen be az analitika megtekintése a Lekérdezéstervező csempe hivatkozás |} Microsoft Azure"
    description="A napló Analytics Nézettervezőt lehetővé teszi a MOBILE konzolban különböző képi megjelenítésekről a MOBILE adattárban adatokat tartalmazó egyéni nézeteket hozhat létre. Ez a cikk a csempék az egyéni nézetek használható minden egyes beállítás hivatkozást tartalmaz."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Log Analitikájának megtekintése Tervező csempe hivatkozás
A nézet Tervező napló Analytics lehetővé teszi a MOBILE konzolban különböző képi megjelenítésekről a MOBILE adattárban adatokat tartalmazó egyéni nézeteket hozhat létre. Ez a cikk a csempék az egyéni nézetek használható minden egyes beállítás hivatkozást tartalmaz.

További cikkek a Tervező nézetben érhető el a következők:

- [Nézettervezőt](log-analytics-view-designer.md) – a Tervező nézet és a létrehozásának és szerkesztése az egyéni nézetek áttekintése.
- [Képi megjelenítés rész hivatkozás](log-analytics-view-designer-parts.md) - hivatkozás az egyes a mozaikokat érhető el az egyéni nézetek használata beállítást. 


Az alábbi táblázat a különböző típusú csempék a nézet Tervező érhető el.  Az alábbi szakaszok ismertetik a részletek és tulajdonságaik minden mozaik típusa.

| Csempe | Leírás |
|:--|:--|
| [Szám](#number-tile) | Egy számot a lekérdezés rekordok számának megjelenítése. |
| [Két szám](#two-numbers-tile) | Két, egyetlen szám megjelenítő két különböző lekérdezésből a rekordok számát. |
| [Gyűrű](#donut-tile) | Gyűrű diagram közepén összesített értéket tartalmazó intézett lekérdezés alapján. |
| [Vonaldiagram & Képfelirat](#line-chart-amp-callout-tile) | A vonaldiagram, ábrafelirat összesített értéket tartalmazó és a lekérdezés alapján. |
| [Vonaldiagram](#line-chart-tile) | A vonaldiagram intézett lekérdezés alapján. |
| [Két idősorokkal](#two-timelines-tile) | Halmozott oszlop típusú diagram két sorozat, mindegyik külön lekérdezés alapján. |



## <a name="number-tile"></a>Szám csempe

A **szám** csempét a napló lekérdezés és a címkét a rekordok számát mutató egyetlen számot jeleníti meg.

![Szám csempe](media/log-analytics-view-designer/tile-number.png)

| A beállítás | Leírás |
|:--|:--|
| név        | A mozaik tetején megjelenő szöveg. |
| Leírás | A csempe neve alatt megjelenő szöveg.    |
| **Csempe** |
| A jelmagyarázat | Az érték a megjelenítendő szöveg. |
| Lekérdezés | A lekérdezés futtatásához.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| **Speciális** |  **> Adatfolyam ellenőrzése** |
| Engedélyezve van | Jelölje be, ha adatfolyam ellenőrzési engedélyezni kell a csempére.  Ez egy másik üzenetet biztosít, ha az adatok nem érhető el a csempére.  Ez jellemzően az ideiglenes időszakban mikor a nézet telepítve van, és adatsorainak a rendelkezésre álló üzenet biztosításához. |
| Lekérdezés | A lekérdezés futtatásához ellenőrizze, hogy az adatok nem érhető el, a nézet.  Ha a lekérdezés eredménye nem tartalmaz, akkor üzenet jelenik meg a mezőbe írja be a fő lekérdezés helyett. |
| Üzenet | Üzenet jelenik meg, ha az adatfolyam ellenőrzési lekérdezés nem adatokat ad vissza.  Ha nincs üzenet ad meg, *Elvégzéséhez értékelési* jelenik meg. |
| **Időtartam** |
| Időtartam | Az aktuális dátumtól használni a időintervallum lekérdezési időtartam.  Például ha **7 nap** van megadva, majd a lekérdezés korlátozódik az aktuális dátumra a 7 nappal korábban létrehozott rekordok. |
| Záró adatok eltolása | Az aktuális adatok a időközönként, a fő lekérdezés használata választható eltolással.  Például **-1 nap** a **befejezési dátum eltolás** és a **7 nap** használni **időtartamának**használata esetén kattintson a lekérdezés korlátozódik 8 napja a tegnap létrehozott rekordokat. |

## <a name="two-numbers-tile"></a>Két szám csempe

A **Két szám** csempét a két különböző naplófájl lekérdezések és a címkét a rekordok száma az egyes két szám jeleníti meg.

![Két szám csempe](media/log-analytics-view-designer/tile-two-numbers.png)

| A beállítás | Leírás |
|:--|:--|
| név        | A mozaik tetején megjelenő szöveg. |
| Leírás | A csempe neve alatt megjelenő szöveg.    |
| **Első csempe** |
| A jelmagyarázat | Az érték a megjelenítendő szöveg. |
| Lekérdezés | A lekérdezés futtatásához.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| **Második csempe** |
| A jelmagyarázat | Az érték a megjelenítendő szöveg. |
| Lekérdezés | A lekérdezés futtatásához.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| **Speciális** | **> Adatfolyam ellenőrzése** |
| Engedélyezve van | Jelölje be, ha adatfolyam ellenőrzési engedélyezni kell a csempére.  Ez egy másik üzenetet biztosít, ha az adatok nem érhető el a csempére.  Ez jellemzően az ideiglenes időszakban mikor a nézet telepítve van, és adatsorainak a rendelkezésre álló üzenet biztosításához. |
| Lekérdezés | A lekérdezés futtatásához ellenőrizze, hogy az adatok nem érhető el, a nézet.  Ha a lekérdezés eredménye nem tartalmaz, akkor üzenet jelenik meg a mezőbe írja be a fő lekérdezés helyett. |
| Üzenet | Üzenet jelenik meg, ha az adatfolyam ellenőrzési lekérdezés nem adatokat ad vissza.  Ha nincs üzenet ad meg, *Elvégzéséhez értékelési* jelenik meg. |
| **Időtartam** |
| Időtartam | Az aktuális dátumtól használni a időintervallum lekérdezési időtartam.  Például ha **7 nap** van megadva, majd a lekérdezés korlátozódik az aktuális dátumra a 7 nappal korábban létrehozott rekordok. |
| Záró adatok eltolása | Az aktuális adatok a időközönként, a fő lekérdezés használata választható eltolással.  Például **-1 nap** a **befejezési dátum eltolás** és a **7 nap** használni **időtartamának**használata esetén kattintson a lekérdezés korlátozódik 8 napja a tegnap létrehozott rekordokat. |

## <a name="donut-tile"></a>Gyűrű csempe

A **gyűrű** csempét a napló lekérdezés érték oszlopának összesített egyetlen számot jeleníti meg.  A gyűrű grafikusan felső három rekordot eredményeit jeleníti meg.

![Gyűrű csempe](media/log-analytics-view-designer/tile-donut.png)

| A beállítás | Leírás |
|:--|:--|
| név        | A mozaik tetején megjelenő szöveg. |
| Leírás | A csempe neve alatt megjelenő szöveg.    |
| **Gyűrű** |
| Lekérdezés | A gyűrű futtatandó lekérdezés.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Ez általában a eredmények összefoglalva a **mérték** kulcsszó használó lekérdezés. |
| **Gyűrű** | **> Középre** |
| Szöveg | A gyűrű belül az érték a megjelenítendő szöveg. |
| Művelet | Az összegzendő egyetlen értéket az érték tulajdonságban a végrehajtani kívánt művelet.<br><br>-Összeg: Adja hozzá az értékeket az összes rekordot, a tulajdonság értékét.<br>-Százalékos: Százalékos aránya az összegzett értékek rekordokból a tulajdonság értékét, és összehasonlítása az összegzett értékek az összes rekordot. |
| Középre műveletben eredményértékeinek | Másik lehetőségként kattintson a pluszjelre kattintva adja hozzá egy vagy több értéket.  A lekérdezés eredményét, adja meg a tulajdonság értékeket tartalmazó rekordok korlátozódik.  Egyetlen értéket ad, ha az összes rekordokra szerepelnek a lekérdezést. |
| **Gyűrű** | **> További lehetőségek** |
| Színek | A szín három felső tulajdonságok megjelenítéséhez.  Ha szeretné az egyedi értékű alternatív színek megadása, majd használja szín hozzárendelése speciális. |
| Speciális szín hozzárendelése | Megjeleníti az adott tulajdonságértékeket színét.  Ha a megadott értékkel az felső három, majd a másik színt a szabványos színek helyett jelenik meg.  Ha a tulajdonság nem szerepel a felső három, majd a szín nem jelenik meg. |
| **Speciális** | **> Adatfolyam ellenőrzése** |
| Engedélyezve van | Jelölje be, ha adatfolyam ellenőrzési engedélyezni kell a csempére.  Ez egy másik üzenetet biztosít, ha az adatok nem érhető el a csempére.  Ez jellemzően az ideiglenes időszakban mikor a nézet telepítve van, és adatsorainak a rendelkezésre álló üzenet biztosításához. |
| Lekérdezés | A lekérdezés futtatásához ellenőrizze, hogy az adatok nem érhető el, a nézet.  Ha a lekérdezés eredménye nem tartalmaz, akkor üzenet jelenik meg a mezőbe írja be a fő lekérdezés helyett. |
| Üzenet | Üzenet jelenik meg, ha az adatfolyam ellenőrzési lekérdezés nem adatokat ad vissza.  Ha nincs üzenet ad meg, *Elvégzéséhez értékelési* jelenik meg. |
| **Időtartam** |
| Időtartam | Az aktuális dátumtól használni a időintervallum lekérdezési időtartam.  Például ha **7 nap** van megadva, majd a lekérdezés korlátozódik az aktuális dátumra a 7 nappal korábban létrehozott rekordok. |
| Záró adatok eltolása | Az aktuális adatok a időközönként, a fő lekérdezés használata választható eltolással.  Például **-1 nap** a **befejezési dátum eltolás** és a **7 nap** használni **időtartamának**használata esetén kattintson a lekérdezés korlátozódik 8 napja a tegnap létrehozott rekordokat. |

## <a name="line-chart-tile"></a>Vonal diagram csempe

A **Vonaldiagram** csempét a vonaldiagram több adatsort napló lekérdezés adott idő alatt jeleníti meg.  

![Vonaldiagram & Képfelirat csempe](media/log-analytics-view-designer/tile-line-chart.png)

| A beállítás | Leírás |
|:--|:--|
| név        | A mozaik tetején megjelenő szöveg. |
| Leírás | A csempe neve alatt megjelenő szöveg.    |
| **Vonaldiagram** |  
| Lekérdezés | A vonaldiagram futtatandó lekérdezés.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Ez általában a eredmények összefoglalva a **mérték** kulcsszó használó lekérdezés.  Ha a lekérdezés használja az **intervallum** kulcsszó az x tengely a diagram használata a időszakot.  Ha a lekérdezés nem szerepel az **intervallum** kulcsszó óránként történik használható parancsmagokról a tengelytől. |
| **Vonaldiagram** | **> Y tengellyel** |
| Logaritmikus skála | Jelölje ki az y tengelyt metszi logaritmikus használni. |
| Erőforrás-mennyiség | Adja meg a lekérdezés által visszaadott értékek mértéke.  Ez az információ címkéinek megjelenítése a diagramon, jelezve az értéktípusok és tetszőlegesen a értékeket konvertáló szolgál.  **Mértékegysége** adja meg a kategória egység, és a rendelkezésre álló **Aktuális mértékegysége** értékeket meghatározza.  Ha kijelöl egy érték **konvertálása** majd numerikus értékeket alakulnak át az **Aktuális egység** típusát **átalakítása** típusát. |
| Egyéni címke | Az az Y tengely a mértékegysége címkéjét mellett megjelenő szöveg.  Ha nincs felirat van megadva, csak a mértékegysége megjelenik. |
| **Speciális** | **> Adatfolyam ellenőrzése** |
| Engedélyezve van | Jelölje be, ha adatfolyam ellenőrzési engedélyezni kell a csempére.  Ez egy másik üzenetet biztosít, ha az adatok nem érhető el a csempére.  Ez jellemzően az ideiglenes időszakban mikor a nézet telepítve van, és adatsorainak a rendelkezésre álló üzenet biztosításához. |
| Lekérdezés | A lekérdezés futtatásához ellenőrizze, hogy az adatok nem érhető el, a nézet.  Ha a lekérdezés eredménye nem tartalmaz, akkor üzenet jelenik meg a mezőbe írja be a fő lekérdezés helyett. |
| Üzenet | Üzenet jelenik meg, ha az adatfolyam ellenőrzési lekérdezés nem adatokat ad vissza.  Ha nincs üzenet ad meg, *Elvégzéséhez értékelési* jelenik meg. |
| **Időtartam** |
| Időtartam | Az aktuális dátumtól használni a időintervallum lekérdezési időtartam.  Például ha **7 nap** van megadva, majd a lekérdezés korlátozódik az aktuális dátumra a 7 nappal korábban létrehozott rekordok. |
| Záró adatok eltolása | Az aktuális adatok a időközönként, a fő lekérdezés használata választható eltolással.  Például **-1 nap** a **befejezési dátum eltolás** és a **7 nap** használni **időtartamának**használata esetén kattintson a lekérdezés korlátozódik 8 napja a tegnap létrehozott rekordokat. |


## <a name="line-chart--callout-tile"></a>Vonal diagram & Ábrafelirat csempe

A **Vonaldiagram & Képfelirat** csempét a vonaldiagram napló lekérdezés több adatsort idő- és egy összesített értéket az ábrafelirat fölé jeleníti meg.  

![Vonaldiagram & Képfelirat csempe](media/log-analytics-view-designer/tile-line-chart-callout.png)

| A beállítás | Leírás |
|:--|:--|
| név        | A mozaik tetején megjelenő szöveg. |
| Leírás | A csempe neve alatt megjelenő szöveg.    |
| **Vonaldiagram** |  
| Lekérdezés | A vonaldiagram futtatandó lekérdezés.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Ez általában a eredmények összefoglalva a **mérték** kulcsszó használó lekérdezés.  Ha a lekérdezés használja az **intervallum** kulcsszó az x tengely a diagram használata a időszakot.  Ha a lekérdezés nem szerepel az **intervallum** kulcsszó óránként történik használható parancsmagokról a tengelytől. |
| **Vonaldiagram** | **> Felirat** |
| Ábrafelirat | A címszöveg megjelenítéséhez, ábrafelirat érték fölé. |
| Adatsor neve | Az adatsor használandó, ábrafelirat értéket a tulajdonság értékét.  Amennyiben nem sorozat, a lekérdezés minden rekordját használják. |
| Művelet | A műveletet végre szeretne hajtani összegzéséhez az ábrafelirat az egyetlen értéket az érték tulajdonságban.<br>-Átlag: A mezőbe írja be az összes rekordot átlaga.<br><br>-Száma: A lekérdezés által visszaadott rekordokat számát.<br>-Az utolsó minta: Mezőbe írja be az utolsó időköze, a diagram szerepel.<br>– Max: A diagram szerepel az időszakok közül a maximális érték.<br>– Min: A diagram szerepel az időszakok közül a minimális érték.<br>-Összeg: A mezőbe írja be az összes rekordot összege. |
| **Vonaldiagram** | **> Y tengellyel** |
| Logaritmikus skála | Jelölje ki az y tengelyt metszi logaritmikus használni. |
| Erőforrás-mennyiség | Adja meg a lekérdezés által visszaadott értékek mértéke.  Ez az információ címkéinek megjelenítése a diagramon, jelezve az értéktípusok és tetszőlegesen a értékeket konvertáló szolgál.  **Mértékegysége** adja meg a kategória egység, és a rendelkezésre álló **Aktuális mértékegysége** értékeket meghatározza.  Ha kijelöl egy érték **konvertálása** majd numerikus értékeket alakulnak át az **Aktuális egység** típusát **átalakítása** típusát. |
| Egyéni címke | Az az Y tengely a mértékegysége címkéjét mellett megjelenő szöveg.  Ha nincs felirat van megadva, csak a mértékegysége megjelenik. |
| **Speciális** | **> Adatfolyam ellenőrzése** |
| Engedélyezve van | Jelölje be, ha adatfolyam ellenőrzési engedélyezni kell a csempére.  Ez egy másik üzenetet biztosít, ha az adatok nem érhető el a csempére.  Ez jellemzően az ideiglenes időszakban mikor a nézet telepítve van, és adatsorainak a rendelkezésre álló üzenet biztosításához. |
| Lekérdezés | A lekérdezés futtatásához ellenőrizze, hogy az adatok nem érhető el, a nézet.  Ha a lekérdezés eredménye nem tartalmaz, akkor üzenet jelenik meg a mezőbe írja be a fő lekérdezés helyett. |
| Üzenet | Üzenet jelenik meg, ha az adatfolyam ellenőrzési lekérdezés nem adatokat ad vissza.  Ha nincs üzenet ad meg, *Elvégzéséhez értékelési* jelenik meg. |
| **Időtartam** |
| Időtartam | Az aktuális dátumtól használni a időintervallum lekérdezési időtartam.  Például ha **7 nap** van megadva, majd a lekérdezés korlátozódik az aktuális dátumra a 7 nappal korábban létrehozott rekordok. |
| Záró adatok eltolása | Az aktuális adatok a időközönként, a fő lekérdezés használata választható eltolással.  Például **-1 nap** a **befejezési dátum eltolás** és a **7 nap** használni **időtartamának**használata esetén kattintson a lekérdezés korlátozódik 8 napja a tegnap létrehozott rekordokat. |

## <a name="two-timelines-tile"></a>Két idősorokkal csempe

A **két idősorokkal** csempére két napló lekérdezés eredményének oszlopdiagramok idővel jeleníti meg.  Ábrafelirat az egyes adatsorokban jelenik meg.  

![Két idősorokkal csempe](media/log-analytics-view-designer/tile-two-timelines.png)

| A beállítás | Leírás |
|:--|:--|
| név        | A mozaik tetején megjelenő szöveg. |
| Leírás | A csempe neve alatt megjelenő szöveg.    |
| Első diagram   
| A jelmagyarázat | Az első adatsort dokumentumpanelt a megjelenítendő szöveg.
| Szín | Az első adatsort oszlopai használni színt.
| Diagram lekérdezés | Az első adatsort futtatandó lekérdezés.  Egyes időközönként fölé rekordok számát a diagramoszlopokhoz fog képviseli.
| Művelet | A műveletet végre szeretne hajtani összegzéséhez az ábrafelirat az egyetlen értéket az érték tulajdonságban.<br><br>-Átlag: A mezőbe írja be az összes rekordot átlaga.<br>-Száma: A lekérdezés által visszaadott rekordokat számát.<br>-Az utolsó minta: Mezőbe írja be az utolsó időköze, a diagram szerepel.<br>– Max: A diagram szerepel az időszakok közül a maximális érték.
| **Második diagram** |
| A jelmagyarázat | A második adatsorozat dokumentumpanelt a megjelenítendő szöveg.
| Szín | A második adatsorozat oszlopai használni színt.
| Diagram lekérdezés | A második adatsorozat futtatandó lekérdezés.  Egyes időközönként fölé rekordok számát a diagramoszlopokhoz fog képviseli.
| Művelet | A műveletet végre szeretne hajtani összegzéséhez az ábrafelirat az egyetlen értéket az érték tulajdonságban.<br><br>-Átlag: A mezőbe írja be az összes rekordot átlaga.<br>-Száma: A lekérdezés által visszaadott rekordokat számát.<br>-Az utolsó minta: Mezőbe írja be az utolsó időköze, a diagram szerepel.<br>– Max: A diagram szerepel az időszakok közül a maximális érték. |
| **Speciális** | **> Adatfolyam ellenőrzése** |
| Engedélyezve van | Jelölje be, ha adatfolyam ellenőrzési engedélyezni kell a csempére.  Ez egy másik üzenetet biztosít, ha az adatok nem érhető el a csempére.  Ez jellemzően az ideiglenes időszakban mikor a nézet telepítve van, és adatsorainak a rendelkezésre álló üzenet biztosításához. |
| Lekérdezés | A lekérdezés futtatásához ellenőrizze, hogy az adatok nem érhető el, a nézet.  Ha a lekérdezés eredménye nem tartalmaz, akkor üzenet jelenik meg a mezőbe írja be a fő lekérdezés helyett. |
| Üzenet | Üzenet jelenik meg, ha az adatfolyam ellenőrzési lekérdezés nem adatokat ad vissza.  Ha nincs üzenet ad meg, *Elvégzéséhez értékelési* jelenik meg. |
| **Időtartam** |
| Időtartam | Az aktuális dátumtól használni a időintervallum lekérdezési időtartam.  Például ha **7 nap** van megadva, majd a lekérdezés korlátozódik az aktuális dátumra a 7 nappal korábban létrehozott rekordok. |
| Záró adatok eltolása | Az aktuális adatok a időközönként, a fő lekérdezés használata választható eltolással.  Például **-1 nap** a **befejezési dátum eltolás** és a **7 nap** használni **időtartamának**használata esetén kattintson a lekérdezés korlátozódik 8 napja a tegnap létrehozott rekordokat. |

## <a name="next-steps"></a>Következő lépések

- [Log keresések](log-analytics-log-searches.md) tudnivalók a lekérdezések támogatja a mozaikokon.
- [Képi megjelenítés kijelzők](log-analytics-view-designer-parts.md) hozzáadása az egyéni nézetben.