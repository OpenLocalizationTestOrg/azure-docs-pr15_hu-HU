<properties
    pageTitle="Jelentkezzen Analytics Nézettervezőt |} Microsoft Azure"
    description="A napló Analytics Nézettervezőt lehetővé teszi a MOBILE konzolban különböző képi megjelenítésekről a MOBILE adattárban adatokat tartalmazó egyéni nézeteket hozhat létre. Ez a cikk az egyes használható az egyéni nézetek képi megjelenítés részek beállítás hivatkozást tartalmaz."
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
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Log Analytics Nézettervezőt képi megjelenítés rész hivatkozás
A nézet Tervező napló Analytics lehetővé teszi a MOBILE konzolban különböző képi megjelenítésekről adatok az a MOBILE tárat tartalmazó egyéni nézeteket hozhat létre. Ez a cikk az egyes használható az egyéni nézetek képi megjelenítés részek beállítás hivatkozást tartalmaz.

További cikkek a Tervező nézetben érhető el a következők:

- [Nézettervezőt](log-analytics-view-designer.md) – a Tervező nézet és a létrehozásának és szerkesztése az egyéni nézetek áttekintése.
- [Mozaik hivatkozás](log-analytics-view-designer-tiles.md) - hivatkozás az egyes a mozaikokat érhető el az egyéni nézetek használata beállítást. 

Az alábbi táblázat ismerteti a különböző típusú csempék a nézet Tervező érhető el.  Az alábbi szakaszok ismertetik a részletek és tulajdonságaik minden mozaik típusa.

| Nézet típusa | Leírás |
|:--|:--|
| [Lekérdezések listáját](#list-of-queries-part) | Log keresési lekérdezések listáját jeleníti meg.  A felhasználó a minden eredményként a lekérdezés kattinthat.  |
| [Szám és a lista](#number-amp-list-part) | Élőfej egyetlen szám megjelenítő számának napló keresési lekérdezést rekordjaival tartalmaz.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon. |
| [Két lista és számok](#two-numbers-amp-list-part) | Élőfej a rekordokat külön naplót keresési lekérdezések száma két számokat tartalmazza.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon. |
| [Lista- és gyűrű](#donut-amp-list-part) | Élőfej a napló lekérdezés érték oszlopának összesített egyetlen számot jeleníti meg.  A gyűrű grafikusan felső három rekordot eredményeit jeleníti meg. |
| [Két idősorokkal és lista](#two-timelines-amp-list-part) | Élőfej jeleníti meg, a napló lekérdezés érték oszlopának összesített egyetlen szám megjelenítése felirattal két napló lekérdezés eredményének oszlopdiagramok idővel.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon. |   
| [Információk](#information-part) | Élőfej a statikus szöveggé és egy nem kötelező hivatkozás jeleníti meg.  Lista egy vagy több elem statikus szöveggé és címmel jeleníti meg. |
| [Vonaldiagram, ábrafelirat és lista](#line-chart-callout-amp-list-part) | Élőfej a vonaldiagram napló lekérdezés több adatsort idő- és egy összesített értéket az ábrafelirat fölé jeleníti meg.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon. |
| [Lista- és vonaldiagram](#line-chart-amp-list-part) | Élőfej a vonaldiagram több adatsort napló lekérdezés adott idő alatt jeleníti meg.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon. |
| [Egymást fedő sor diagramok rész](#stack-of-line-charts-part) | Jeleníti meg a napló lekérdezés több adatsort három külön vonaldiagramok idővel. |


## <a name="list-of-queries-part"></a>Lekérdezések rész listája

Log keresési lekérdezések listáját jeleníti meg.  A felhasználó a minden eredményként a lekérdezés kattinthat.  A nézet tartalmazza egyetlen lekérdezés alapértelmezés szerint, és a **+ lekérdezés** további lekérdezések hozzáadásához kattintson a.

![Lekérdezések nézet listája](media/log-analytics-view-designer/view-list-queries.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Cím | Kattintson a nézet tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Előre kiválasztott szűrők | Vesszővel elválasztott listája a bal oldali szűrőterület szerepeltetni, amikor egy felhasználó kijelöli a lekérdezés tulajdonságai. |
| Jelenítse meg a mód | Kezdeti nézet jelenik meg, ha a lekérdezés ki van jelölve.  A felhasználó választhat a minden elérhető nézetek után nyissa meg a lekérdezést. |
| **Lekérdezések** |
| Keresési lekérdezés | A lekérdezés futtatásához. |
| Rövid név | A lekérdezés jelenít meg a felhasználó leíró jellegű neve. |


## <a name="number--list-part"></a>Szám és a lista kijelző

Élőfej egyetlen szám megjelenítő számának napló keresési lekérdezést rekordjaival tartalmaz.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon.


![Lekérdezések nézet listája](media/log-analytics-view-designer/view-number-list.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | Kattintson a nézet tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Ikon | Kép az eredményt, a fejléc mellett megjeleníteni kívánt fájlt.
| Ikon használata beállítások megadására | Jelölje ki, hogy az ikon megjelenítését. |
| **Cím** |
| A jelmagyarázat | A fejléc tetején megjelenő szöveg. |
| Lekérdezés | A lekérdezés futtatásához a fejléc.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| **Lista** |
| Lekérdezés | A lista futtatandó lekérdezés.  A találatok között az első tíz rekordok első két tulajdonságainak jelennek meg.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Sávok automatikusan létrejön egy numerikus oszlop relatív értéke alapján.<br><br>A lekérdezés a Rendezés parancs segítségével a rekordok rendezése a listában.  A lekérdezés futtatásához és az összes rekordot ad vissza az összes lásd: a felhasználó elemre. |
| Diagram elrejtése | Jelölje ki a diagram jobb oldalán a számoszlopot letiltása. |
| Értékgörbék engedélyezése | Értékgörbe vízszintes sáv helyett megjelenítéséhez jelölje be.  [Általános beállítások](#sparklines) talál további információt. |
| Szín | A sávok vagy Értékgörbék színét. |
| Nevet és érték elválasztó | Ha szeretne több értéket a szöveg tulajdonság értelmezhető egykarakteres elválasztó.  [Általános beállítások](#name-value-separator) talál további információt. |
| Navigációs lekérdezés | A lekérdezés futtatásához, amikor egy felhasználó kijelöli az elemet a listában.  [Általános beállítások](#navigation-query) talál további információt. |
| **Lista** | **> Oszlopcímek** |
| név | Az első oszlopban, a lista tetején megjelenő szöveg. |
| Érték | A második oszlopban a lista tetején megjelenő szöveg. |
| **Lista** | **> Küszöbérték** |
| Küszöbértékek engedélyezése | Ahhoz, hogy küszöbértékek kijelölése  [Általános beállítások](#thresholds) talál további információt. |


## <a name="two-numbers--list-part"></a>Két szám és a lista kijelző

Élőfej a rekordokat külön naplót keresési lekérdezések száma két számokat tartalmazza.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon.

![Két szám és a lista nézet](media/log-analytics-view-designer/view-two-numbers-list.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | Kattintson a nézet tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Ikon | Kép az eredményt, a fejléc mellett megjeleníteni kívánt fájlt.
| Ikon használata beállítások megadására | Jelölje ki, hogy az ikon megjelenítését. |
| **Cím** |
| A jelmagyarázat | A fejléc tetején megjelenő szöveg. |
| Lekérdezés | A lekérdezés futtatásához a fejléc.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| **Lista** |
| Lekérdezés | A lista futtatandó lekérdezés.  A találatok között az első tíz rekordok első két tulajdonságainak jelennek meg.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Sávok automatikusan létrejön egy numerikus oszlop relatív értéke alapján.<br><br>A lekérdezés a Rendezés parancs segítségével a rekordok rendezése a listában.  A lekérdezés futtatásához és az összes rekordot ad vissza az összes lásd: a felhasználó elemre. |
| Diagram elrejtése | Jelölje ki a diagram jobb oldalán a számoszlopot letiltása. |
| Értékgörbék engedélyezése | Értékgörbe vízszintes sáv helyett megjelenítéséhez jelölje be.  [Általános beállítások](#sparklines) talál további információt. |
| Szín | A sávok vagy Értékgörbék színét. |
| Művelet | A művelet végrehajtása az értékgörbét.  [Általános beállítások](#sparklines) talál további információt. |
| Nevet és érték elválasztó | Ha szeretne több értéket a szöveg tulajdonság értelmezhető egykarakteres elválasztó.  [Általános beállítások](#name-value-separator) talál további információt. |
| Navigációs lekérdezés | A lekérdezés futtatásához, amikor egy felhasználó kijelöli az elemet a listában.  [Általános beállítások](#navigation-query) talál további információt. |
| **Lista** | **> Oszlopcímek** |
| név | Az első oszlopban, a lista tetején megjelenő szöveg. |
| Érték | A második oszlopban a lista tetején megjelenő szöveg. |
| **Lista** | **> Küszöbérték** |
| Küszöbértékek engedélyezése | Ahhoz, hogy küszöbértékek kijelölése  [Általános beállítások](#thresholds) talál további információt. |

## <a name="donut--list-part"></a>Gyűrű és a lista kijelző

Élőfej a napló lekérdezés érték oszlopának összesített egyetlen számot jeleníti meg.  A gyűrű grafikusan felső három rekordot eredményeit jeleníti meg.

![Gyűrű és lista nézet](media/log-analytics-view-designer/view-donut-list.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | A mozaik tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Ikon | Kép az eredményt, a fejléc mellett megjeleníteni kívánt fájlt. |
| Ikon használata beállítások megadására | Jelölje ki, hogy az ikon megjelenítését. |
| **Élőfej** |
| Cím | A fejléc tetején megjelenő szöveg.
| Alcíme | Szöveg megjelenítése a fejléc tetején a cím alá.
| **Gyűrű** |
| Lekérdezés | A gyűrű futtatandó lekérdezés.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie. |
| **Gyűrű** |  **> Középre** |
| Szöveg | A gyűrű belül az érték a megjelenítendő szöveg. |
| Művelet | Az összegzendő egyetlen értéket az érték tulajdonságban a végrehajtani kívánt művelet.<br><br>-Összeg: Adja hozzá az értékeket az összes rekordot.<br>-Százalékos: Százalékos aránya a lekérdezést total rekordjaihoz **központ műveletben eredményértékeinek** szereplő értékek által visszaadott rekordokat. |
| Középre műveletben eredményértékeinek | Másik lehetőségként kattintson a pluszjelre kattintva adja hozzá egy vagy több értéket.  A lekérdezés eredményét, adja meg a tulajdonság értékeket tartalmazó rekordok korlátozódik.  Egyetlen értéket ad, ha az összes rekordok szerepelnek vannak a lekérdezést. |
| **További lehetőségek** | **> Színek** |
| Szín 1<br>Szín 2<br>Szín 3 | Jelölje ki a színt, a minden egyes a gyűrű megjelenített értékeket. |
| **További lehetőségek** | **> Speciális szín hozzárendelése** |
| Mezőérték | Írja be a egy mező nevét, egy másik színt formátumban jeleníti meg, ha a gyűrű szerepel. |
| Szín | Jelölje ki az egyedi mező színét. |
| **Lista** |
| Lekérdezés | A lista futtatandó lekérdezés.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| Diagram elrejtése | Jelölje ki a diagram jobb oldalán a számoszlopot letiltása. |
| Értékgörbék engedélyezése | Értékgörbe vízszintes sáv helyett megjelenítéséhez jelölje be.  [Általános beállítások](#sparklines) talál további információt. |
| Szín | A sávok vagy Értékgörbék színét. |
| Művelet | A művelet végrehajtása az értékgörbét.  [Általános beállítások](#sparklines) talál további információt. |
| Nevet és érték elválasztó | Ha szeretne több értéket a szöveg tulajdonság értelmezhető egykarakteres elválasztó.  [Általános beállítások](#name-value-separator) talál további információt. |
| Navigációs lekérdezés | A lekérdezés futtatásához, amikor egy felhasználó kijelöli az elemet a listában.  [Általános beállítások](#navigation-query) talál további információt. |
| **Lista** | **> Oszlopcímek** |
| név | Az első oszlopban, a lista tetején megjelenő szöveg. |
| Érték | A második oszlopban a lista tetején megjelenő szöveg. |
| **Lista** | **> Küszöbérték** |
| Küszöbértékek engedélyezése | Ahhoz, hogy küszöbértékek kijelölése  [Általános beállítások](#thresholds) talál további információt. |

## <a name="two-timelines--list-part"></a>Két idősorokkal és a lista kijelző

Élőfej jeleníti meg, a napló lekérdezés érték oszlopának összesített egyetlen szám megjelenítése felirattal két napló lekérdezés eredményének oszlopdiagramok idővel.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon.

![Két idősorokkal & listájának megtekintése](media/log-analytics-view-designer/view-two-timelines-list.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | A mozaik tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Ikon | Kép az eredményt, a fejléc mellett megjeleníteni kívánt fájlt. |
| Ikon használata beállítások megadására | Jelölje ki, hogy az ikon megjelenítését. |
| **Először diagram<br>a második diagram** |
| A jelmagyarázat | Az első adatsort dokumentumpanelt a megjelenítendő szöveg. |
| Szín | Az oszlopok az adatsor használni színt. |
| Lekérdezés | Az első adatsort futtatandó lekérdezés.  Egyes időközönként fölé rekordok számát a diagramoszlopokhoz fog képviseli. |
| Művelet | A műveletet végre szeretne hajtani összegzéséhez az ábrafelirat az egyetlen értéket az érték tulajdonságban.<br><br>-Összeg: A mezőbe írja be az összes rekordot összege.<br>-Átlag: A mezőbe írja be az összes rekordot átlaga.<br>-Az utolsó minta: Mezőbe írja be az utolsó időköze, a diagram szerepel.<br>-Első minta: Mezőbe írja be az első intervallum a diagram szerepel.<br>-Száma: A lekérdezés által visszaadott rekordokat számát.|
| **Lista** |
| Lekérdezés | A lista futtatandó lekérdezés.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| Diagram elrejtése | Jelölje ki a diagram jobb oldalán a számoszlopot letiltása. |
| Értékgörbék engedélyezése | Értékgörbe vízszintes sáv helyett megjelenítéséhez jelölje be.  [Általános beállítások](#sparklines) talál további információt. |
| Szín | A sávok vagy Értékgörbék színét. |
| Művelet | A művelet végrehajtása az értékgörbét.  [Általános beállítások](#sparklines) talál további információt. |
| Navigációs lekérdezés | A lekérdezés futtatásához, amikor egy felhasználó kijelöli az elemet a listában.  [Általános beállítások](#navigation-query) talál további információt. |
| **Lista** | **> Oszlopcímek** |
| név | Az első oszlopban, a lista tetején megjelenő szöveg. |
| Érték | A második oszlopban a lista tetején megjelenő szöveg. |
| **Lista** | **> Küszöbérték** |
| Küszöbértékek engedélyezése | Ahhoz, hogy küszöbértékek kijelölése  [Általános beállítások](#thresholds) talál további információt. |

## <a name="information-part"></a>Adatok rész

Élőfej a statikus szöveggé és egy nem kötelező hivatkozás jeleníti meg.  Lista egy vagy több elem statikus szöveggé és címmel jeleníti meg.

![Információk megtekintése](media/log-analytics-view-designer/view-information.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | A mozaik tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Szín | A fejléc háttérszínét. |
| **Élőfej** |
| Kép | Kép a fejlécen megjeleníteni kívánt fájlt. |
| Címke | A fejléc megjelenítendő szöveget. |
| **Élőfej** | **> Hivatkozás** |
| Címke | Hivatkozás a szöveget. |
| URL-címe | Hivatkozás URL-címe. |
| **Információk elemek** |
| Cím | Szöveg jelenjen meg az egyes elemek címére. |
| Tartalom | Mindegyik elemhez megjelenítendő szöveget. |


## <a name="line-chart-callout--list-part"></a>Vonaldiagram, ábrafelirat, és a lista kijelző

Élőfej a vonaldiagram napló lekérdezés több adatsort idő- és egy összesített értéket az ábrafelirat fölé jeleníti meg.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon.

![Vonaldiagram, ábrafelirat és lista nézet](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | A mozaik tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Ikon | Kép az eredményt, a fejléc mellett megjeleníteni kívánt fájlt. |
| Ikon használata beállítások megadására | Jelölje ki, hogy az ikon megjelenítését. |
| **Élőfej** |
| Cím | A fejléc tetején megjelenő szöveg. |
| Alcíme | Szöveg megjelenítése a fejléc tetején a cím alá. |
| **Vonaldiagram** |
| Lekérdezés | A vonaldiagram futtatandó lekérdezés.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Ez általában a eredmények összefoglalva a **mérték** kulcsszó használó lekérdezés.  Ha a lekérdezés használja az **intervallum** kulcsszó az x tengely a diagram használata a időszakot.  Ha a lekérdezés nem szerepel az **intervallum** kulcsszó óránként történik használható parancsmagokról a tengelytől. |
| **Vonaldiagram** | **> Felirat** |
| Ábrafelirat cím | Az ábrafelirat érték felett megjelenő szöveg. |
| Adatsor neve | Az adatsor használandó, ábrafelirat értéket a tulajdonság értékét.  Amennyiben nem sorozat, a lekérdezés minden rekordját használják. |
| Művelet | A műveletet végre szeretne hajtani összegzéséhez az ábrafelirat az egyetlen értéket az érték tulajdonságban.<br><br>-Átlag: A mezőbe írja be az összes rekordot átlaga.<br>-Az összes rekordot, a lekérdezés által visszaadott darabszámát száma.<br>-Az utolsó minta: Mezőbe írja be az utolsó időköze, a diagram szerepel.<br>– Max: A diagram szerepel az időszakok közül a maximális érték.<br>– Min: A diagram szerepel az időszakok közül a minimális érték.<br>-Összeg: A mezőbe írja be az összes rekordot összege. |
| **Vonaldiagram** | **> Y tengellyel** |
| Logaritmikus skála | Jelölje ki az y tengelyt metszi logaritmikus használni. |
| Erőforrás-mennyiség | Adja meg a lekérdezés által visszaadott értékek mértéke.  Ez az információ címkéinek megjelenítése a diagramon, jelezve az értéktípusok és tetszőlegesen a értékeket konvertáló szolgál.  A egység adja meg a kategória egység, és a rendelkezésre álló aktuális mértékegysége értékeket meghatározza.  Ha bejelöli a érték átalakítása, majd a numerikus értékeket alakulnak át az aktuális egység típusát írja be a konvertálás. |
| Egyéni címke | Az az Y tengely a mértékegysége címkéjét mellett megjelenő szöveg.  Ha nincs felirat van megadva, csak a mértékegysége megjelenik. |
| **Lista** |
| Lekérdezés | A lista futtatandó lekérdezés.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| Diagram elrejtése | Jelölje ki a diagram jobb oldalán a számoszlopot letiltása. |
| Értékgörbék engedélyezése | Értékgörbe vízszintes sáv helyett megjelenítéséhez jelölje be.  [Általános beállítások](#sparklines) talál további információt. |
| Szín | A sávok vagy Értékgörbék színét. |
| Művelet | A művelet végrehajtása az értékgörbét.  [Általános beállítások](#sparklines) talál további információt. |
| Nevet és érték elválasztó | Ha szeretne több értéket a szöveg tulajdonság értelmezhető egykarakteres elválasztó.  [Általános beállítások](#name-value-separator) talál további információt. |
| Navigációs lekérdezés | A lekérdezés futtatásához, amikor egy felhasználó kijelöli az elemet a listában.  [Általános beállítások](#navigation-query) talál további információt. |
| **Lista** | **> Oszlopcímek** |
| név | Az első oszlopban, a lista tetején megjelenő szöveg. |
| Érték | A második oszlopban a lista tetején megjelenő szöveg. |
| **Lista** | **> Küszöbérték** |
| Küszöbértékek engedélyezése | Ahhoz, hogy küszöbértékek kijelölése  [Általános beállítások](#thresholds) talál további információt. |

## <a name="line-chart--list-part"></a>Sor lista és a diagram kijelző

Élőfej a vonaldiagram több adatsort napló lekérdezés adott idő alatt jeleníti meg.  Lista megjelenítése a felső tíz eredmények lekérdezés egy számoszlopot vagy annak időbeli változás relatív értékének jelző grafikon.

![Sor lista és a diagram nézetben](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | A mozaik tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Ikon | Kép az eredményt, a fejléc mellett megjeleníteni kívánt fájlt. |
| Ikon használata beállítások megadására | Jelölje ki, hogy az ikon megjelenítését. |
| **Élőfej** |
| Cím | A fejléc tetején megjelenő szöveg. |
| Alcíme | Szöveg megjelenítése a fejléc tetején a cím alá. |
| **Vonaldiagram** |
| Lekérdezés | A vonaldiagram futtatandó lekérdezés.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Ez általában a eredmények összefoglalva a **mérték** kulcsszó használó lekérdezés.  Ha a lekérdezés használja az **intervallum** kulcsszó az x tengely a diagram használata a időszakot.  Ha a lekérdezés nem szerepel az **intervallum** kulcsszó óránként történik használható parancsmagokról a tengelytől. |
| **Vonaldiagram** | **> Y tengellyel** |
| Logaritmikus skála | Jelölje ki az y tengelyt metszi logaritmikus használni. |
| Erőforrás-mennyiség | Adja meg a lekérdezés által visszaadott értékek mértéke.  Ez az információ címkéinek megjelenítése a diagramon, jelezve az értéktípusok és tetszőlegesen a értékeket konvertáló szolgál.  A egység adja meg a kategória egység, és a rendelkezésre álló aktuális mértékegysége értékeket meghatározza.  Ha bejelöli a érték átalakítása, majd a numerikus értékeket alakulnak át az aktuális egység típusát írja be a konvertálás. |
| Egyéni címke | Az az Y tengely a mértékegysége címkéjét mellett megjelenő szöveg.  Ha nincs felirat van megadva, csak a mértékegysége megjelenik. |
| **Lista** |
| Lekérdezés | A lista futtatandó lekérdezés.  A lekérdezés által visszaadott rekordok számát jelennek meg. |
| Diagram elrejtése | Jelölje ki a diagram jobb oldalán a számoszlopot letiltása. |
| Értékgörbék engedélyezése | Értékgörbe vízszintes sáv helyett megjelenítéséhez jelölje be.  [Általános beállítások](#sparklines) talál további információt. |
| Szín | A sávok vagy Értékgörbék színét. |
| Művelet | A művelet végrehajtása az értékgörbét.  [Általános beállítások](#sparklines) talál további információt. |
| Nevet és érték elválasztó | Ha szeretne több értéket a szöveg tulajdonság értelmezhető egykarakteres elválasztó.  [Általános beállítások](#name-value-separator) talál további információt. |
| Navigációs lekérdezés | A lekérdezés futtatásához, amikor egy felhasználó kijelöli az elemet a listában.  [Általános beállítások](#navigation-query) talál további információt. |
| **Lista** | **> Oszlopcímek** |
| név | Az első oszlopban, a lista tetején megjelenő szöveg. |
| Érték | A második oszlopban a lista tetején megjelenő szöveg. |
| **Lista** | **> Küszöbérték** |
| Küszöbértékek engedélyezése | Ahhoz, hogy küszöbértékek kijelölése  [Általános beállítások](#thresholds) talál további információt. |

## <a name="stack-of-line-charts-part"></a>Egymást fedő sor diagramok rész

Jeleníti meg a napló lekérdezés több adatsort három külön vonaldiagramok idővel.

![Jegyzettömb vonaldiagram](media/log-analytics-view-designer/view-stack-line-charts.png)

| A beállítás | Leírás |
|:--|:--|
| **Általános** |
| Csoportnévre | A mozaik tetején megjelenő szöveg. |
| Új csoport | Válassza az új csoport létrehozása a nézetben, az aktuális nézet karaktertől kezdve. |
| Ikon | Kép az eredményt, a fejléc mellett megjeleníteni kívánt fájlt. |
| **Diagram 1<br>2 diagram<br>3 diagram** | **> Élőfej** |
| Cím | A diagram tetején megjelenő szöveg. |
| Alcíme | Szöveg megjelenítése a diagramon tetején a cím alá. |
| **Diagram 1<br>2 diagram<br>3 diagram** | **Vonaldiagram** |
| Lekérdezés | A vonaldiagram futtatandó lekérdezés.  Az első tulajdonság egy szöveges értéket, és a második tulajdonság egy numerikus értéket kell lennie.  Ez általában a eredmények összefoglalva a **mérték** kulcsszó használó lekérdezés.  Ha a lekérdezés használja az **intervallum** kulcsszó az x tengely a diagram használata a időszakot.  Ha a lekérdezés nem szerepel az **intervallum** kulcsszó óránként történik használható parancsmagokról a tengelytől. |
| **Diagram** | **> Y tengellyel** |
| Logaritmikus skála | Jelölje ki az y tengelyt metszi logaritmikus használni. |
| Erőforrás-mennyiség | Adja meg a lekérdezés által visszaadott értékek mértéke.  Ez az információ címkéinek megjelenítése a diagramon, jelezve az értéktípusok és tetszőlegesen a értékeket konvertáló szolgál.  A egység adja meg a kategória egység, és a rendelkezésre álló aktuális mértékegysége értékeket meghatározza.  Ha bejelöli a érték átalakítása, majd a numerikus értékeket alakulnak át az aktuális egység típusát írja be a konvertálás. |
| Egyéni címke | Az az Y tengely a mértékegysége címkéjét mellett megjelenő szöveg.  Ha nincs felirat van megadva, csak a mértékegysége megjelenik. |

## <a name="common-settings"></a>Általános beállítások
Az alábbi szakaszok ismertetik több megjelenítést kijelzők közös beállítások.

### <a name="name-value-separator">Nevet és érték elválasztó</a>
Ha szeretne több értéket a szöveg tulajdonság lista lekérdezés értelmezhető egykarakteres elválasztó.  Elválasztó adja meg, ha minden egyes mezőhöz, a név mezőbe az azonos elválasztó karakterrel tagolt nevek lehet nyújtani.

Például érdemes megvizsgálni a *helyet* , például *Redmond-épület 41* és *Verőcei-Building12*értékeket tartalmaz nevű tulajdonságot.  Megadhatja – tartozó név és érték elválasztó és *Város-építőelem* nevét.  Ez az egyes értékek volna értelmezhető *Város* és *épület*című két tulajdonságok. 

### <a name="navigation-query">Navigációs lekérdezés</a>
A lekérdezés futtatásához, amikor egy felhasználó kijelöli az elemet a listában.  Ha meg szeretné jeleníteni az elemet a felhasználó által kiválasztott szintaxisa *{a kijelölt elem}* használata

Például ha a lekérdezés van a *számítógép* és a navigációs lekérdezés nevű oszlop is *{a kijelölt elem}*, a lekérdezés például *számítógép = "Sajátgép"* szeretné futtatni, a számítógép kiválasztásakor.  Ha a navigációs lekérdezés *típus = {a kijelölt elem} esemény* majd a lekérdezés *típus = esemény számítógép = "Sajátgép"* szeretné futtatni.

### <a name="sparklines">Értékgörbék</a>
Értékgörbe az adott idő alatt egy listaelem értékének illusztráló kis vonaldiagram.  Képi megjelenítés kijelzők listával választhat sáv jelző egy számoszlopot vagy egy értékgörbét, jelezve az érték időbeli relatív értéke vízszintes megjelenjen-e.

Az alábbi táblázat ismerteti a Értékgörbék beállításait.

| A beállítás | Leírás |
|:--|:--|
| Értékgörbék engedélyezése | Értékgörbe vízszintes sáv helyett megjelenítéséhez jelölje be. |
| Művelet | Értékgörbék engedélyezve vannak, ha ez az a minden tulajdonság a listában az értékgörbét értékének kiszámításához a végrehajtandó műveletet.<br><br>-Utolsó minta: Az adatsor keresztül az időszakot utolsó értékét.<br>-Max: Az adatsor keresztül az időszakot a maximális érték.<br>– Min: Az adatsor az időszakot keresztül a minimális érték.<br>-Összeg: A sorozat keresztül az időszakot értékeinek összege.<br>– Összefoglalás: Használja a mérték parancs ugyanaz, mint a fejlécen a lekérdezést. |

### <a name="thresholds">Küszöbérték</a>
Küszöbértékek minden elem mellett egy színes ikon megjelenítése egy listában, az elemek, amelyek haladja meg egy adott érték vagy egy bizonyos tartományba esik rövid vizuális kijelző biztosítva teszi lehetővé.  Például zöld az elemek egy elfogadható értéket, a sárgát, ha az érték, amely a program figyelmeztetésben jelzi tartományon belül, és a piros ikon is látható, ha a munkafüzet meghalad egy hibaérték lesz.

Ha engedélyezi a küszöbértékek egy részére, meg kell adnia egy vagy több küszöbértékek.  Ha az elem értéke nagyobb-e adott küszöbértéknél és kisebb, mint a következő küszöbérték, a szín használják.  Ha az elem nagyobb, mint majd legnagyobb küszöbérték, a Szín legördülő listában.   

Egyes küszöb beállítása **alapértelmezett**érték egy küszöb tartalmaz.  Ez egy, a színt, ha nincs más értékek túllépik beállítása.  Hozzáadhat vagy eltávolíthat küszöbértékek gombra kattintva a **+** vagy az **x** gombra.

Az alábbi táblázat ismerteti a küszöbértékek beállításait.

| A beállítás | Leírás |
|:--|:--|
| Küszöbértékek engedélyezése | Jelölje ki a színes ikont jelenít meg az egyes értékek viszonyított megadott küszöbértékek állapotát jelző balra. |
| név | Nevet, amely azonosítja a határértéknél. |
| Küszöbérték | A küszöbértékét értékét.  A listaelemek állapota színét a legmagasabb küszöbértéket túllépte az elem érték szerinti színének értékre van állítva.  Van egy alapértelmezett küszöbérték, amely a színt, ha nincs küszöbértéket. |
| Szín | A küszöbértéket színét. |


## <a name="next-steps"></a>Következő lépések

- [Log keresések](log-analytics-log-searches.md) tudnivalók a lekérdezések támogatása a kijelzők megjelenítési.