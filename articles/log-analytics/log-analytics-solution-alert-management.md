<properties
   pageTitle="A felhasználó műveleteket Management csomagja (MOBILE) projektmenedzsment megoldás |} Microsoft Azure"
   description="A riasztási projektmenedzsment megoldás napló Analytics segítségével elemezheti az összes figyelmeztetések a környezetben.  Figyelmeztetéseket MOBILE alkalmazásból történő, azt importál riasztások csatlakoztatott System Center műveletek Manager (SCOM) management csoportokból napló Analytics."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Riasztási projektmenedzsment megoldás műveletek Management csomagja (MOBILE)

![Figyelmeztető Management ikon](media/log-analytics-solution-alert-management/icon.png) A riasztási projektmenedzsment megoldás segítségével elemezheti az összes figyelmeztetések a környezetben.  Figyelmeztetéseket MOBILE alkalmazásból történő, azt importál riasztások csatlakoztatott System Center műveletek Manager (SCOM) management csoportokból napló Analytics.  Több felügyeleti csoportokkal környezetekben a riasztási projektmenedzsment megoldás fog révén összevont nézetben riasztásokat összes felügyeleti csoportot.

## <a name="prerequisites"></a>Előfeltételek

- Értesítések importálása SCOM, a megoldás a MOBILE munkaterület és [Operations Manager kapcsolatot a naplóban Analytics](log-analytics-om-agents.md)leírt eljárással SCOM kezelése csoport közötti kapcsolatot igényel.  


## <a name="configuration"></a>Konfiguráció

A riasztási projektmenedzsment megoldás hozzáadása a MOBILE munkaterület [hozzáadása megoldások](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.

## <a name="management-packs"></a>Adatkezelési csomagok

Ha a SCOM kezelése csoport csatlakozik a MOBILE munkaterület, majd a következő management csomagok telepíti SCOM a a megoldás hozzáadásakor.  Nem tartozik konfigurációs vagy az alábbi felügyeleti csomagok szükséges a karbantartás.  

- A Microsoft System Center Advisor riasztási Management (Microsoft.IntelligencePacks.AlertManagement)

Megoldás management csomagok frissítésének kapcsolatos további tudnivalókért olvassa el a [Operations Manager kapcsolatot a naplóban Analytics](log-analytics-om-agents.md)című témakört.

## <a name="data-collection"></a>Adatok gyűjtése

### <a name="agents"></a>Ügynökök beállítása

Az alábbi táblázat ismerteti a kapcsolódó források a megoldás által támogatott.

| Csatlakoztatott adatforrás | Támogatás | Leírás |
|:--|:--|:--|
| [A Windows ügynökök beállítása](log-analytics-windows-agents.md) | nem | Közvetlen Windows ügynökök SCOM riasztások nem eredményeznek. |
| [Linux ügynökök beállítása](log-analytics-linux-agents.md) | nem | Közvetlen Linux ügynökök SCOM riasztások nem eredményeznek. |
| [SCOM kezelése csoport](log-analytics-om-agents.md) | igen | SCOM ügynökök létrehozott értesítések kézbesítésének a felügyeleti csoportba és napló Analytics továbbítja.<br><br>Log Analytics közvetlen kapcsolatot a SCOM ügynök nincs szükség. Riasztási adatok a MOBILE tárházba továbbítja a felügyeleti csoportból. |
| [Azure tárterület-fiók](log-analytics-azure-storage.md) | nem | Az Azure tároló fiókok SCOM riasztások nem tárolja. |

### <a name="collection-frequency"></a>A webhelycsoport gyakorisága

Figyelmeztetéseket MOBILE belül közvetlenül a megoldást rendelkezésére állnak.  Riasztási adatokat a rendszer elküldi a SCOM felügyeleti csoportból napló Analytics minden 3 perc.  

## <a name="using-the-solution"></a>A megoldás használata

A riasztási projektmenedzsment megoldás a MOBILE munkaterületre való hozzáadásakor a **Riasztási Management** csempére hozzáadódik az MOBILE irányítópult.  Ez a csempe jelzi a darab és a grafikus ábrázolása az aktuálisan aktív értesítések az utolsó 24 órán belül előállított száma.  Ez az időtartomány nem módosítható.

![Riasztási Management csempe](media/log-analytics-solution-alert-management/tile.png)

Kattintson a **Figyelmeztető Management** csempére kattintva nyissa meg a **Riasztási Management** irányítópult.  Az irányítópult az oszlopot az alábbi táblázat tartalmazza.  Minden egyes oszlopát a felső tíz értesítések száma a megadott hatókör és időtartomány adott oszlop feltételnek megfelelő sorolja fel.  A napló keresés, ahol a teljes listát, a annak az oszlopnak a képernyő alján **látható a teljes** gombra kattintva vagy az oszlop fejlécére kattintva futtathatja.

| Oszlop| Leírás |
|:--|:--|
| Kritikus értesítések | A kritikus riasztás neve szerint csoportosított egy súlyosságát minden riasztást.  Kattintson egy figyelmeztető neve mezőbe, hogy az értesítést az összes rekordot ad vissza napló keresés futtatásához. |
| Figyelmeztetés értesítések | Minden riasztást, egy figyelmeztető riasztás neve szerint csoportosított súlyosságát.  Kattintson egy figyelmeztető neve mezőbe, hogy az értesítést az összes rekordot ad vissza napló keresés futtatásához. |
| Aktív SCOM értesítések | Minden SCOM riasztást bármelyik állam eltérő *Lezárva* az értesítést generáló forrás szerint csoportosítva. |
| Az összes aktív értesítés | Minden riasztást a minden riasztás neve szerint csoportosított súlyosságát. Csak a SCOM riasztások bármely állapota *Lezárva*nem tartalmazza.|

Jobbra görgetés, ha az irányítópulton a felsorolásban, rákattintva a [napló keresés](log-analytics-log-searches.md) végrehajtása riasztási adatokhoz több általános lekérdezések.

![Riasztási Management irányítópult](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Hatókör és időtartományt

A hatókör elemezheti az értesítési projektmenedzsment megoldás riasztások alapértelmezés szerint ki van az összes csatlakoztatott felügyeleti csoportból hozza létre az utóbbi 7 napból belül.  

![Riasztási Management hatóköre](media/log-analytics-solution-alert-management/scope.png)

- A felügyeleti csoportok, az elemzés módosításához kattintson a **hatókör** az irányítópult tetején.  Kiválaszthat vagy **globális** az összes kapcsolódó kezelése csoport, vagy a **Kezelés csoport által** jelöljön ki egy egy felügyeleti csoportot.

- Az értesítések időtartomány, válassza ki az **adatok alapján** az irányítópult tetején.  Az utóbbi 7 napból 1 nap és 6 órával belül figyelmeztetéseket is választhat.  Vagy válassza az **egyéni** , és adja meg az egyéni dátumtartományra vonatkozóan.

## <a name="log-analytics-records"></a>Naplóbejegyzések Analytics

A riasztási projektmenedzsment megoldás elemzi rendelkező **riasztási**típusú rekordot.  Azt fogja is riasztások importálása SCOM, és hozzon létre egy megfelelő rekordot, egy **Figyelmeztetés** , és a **OpsManager**egy SourceSystem típusú.  Ezeket a rekordokat az alábbi táblázat az jellemzőkkel rendelkeznek.  

| A tulajdonság | Leírás |
|:--|:--|
| Típus | *A felhasználó* |
| SourceSystem | *OpsManager* |
| AlertContext | Az XML formátumban jöjjön létre a figyelmeztetési elem részleteit. |
| AlertDescription | Az értesítés részletes leírását. |
| AlertId | Az értesítés globálisan egyedi azonosítója. |
| AlertName | A riasztás nevét. |
| AlertPriority | Az értesítés prioritását. |
| AlertSeverity | A figyelmeztetés szintje súlyosságát. |
| AlertState | Az értesítés legújabb felbontás állapota. |
| LastModifiedBy | A felhasználó, aki legutóbb módosította az értesítésre neve. |
| ManagementGroupName | Ha a figyelmeztető jött létre kezelése csoport nevére. |
| RepeatCount | Az azonos értesítést jött létre ugyanezt az idő száma objektum óta feloldja nyomon követni. |
| ResolvedBy | Az értesítés a rendszer a felhasználó neve. Ha a figyelmeztető még nem megszűnt-e üres. |
| SourceDisplayName | A felügyeleti objektum az értesítést generáló megjelenítendő nevét. |
| SourceFullName | A felügyeleti objektum az értesítést generáló teljes neve. |
| TicketId | Az értesítés, ha a SCOM környezet integrálva van egy folyamat értesítések jegyek hozzárendeléséhez jegy azonosítója.  Üres nincs jegy azonosító van rendelve. |
| TimeGenerated | Dátum és idő, amely az értesítésre. |
| TimeLastModified | Dátum és idő, amelyek a legutóbbi módosításának az értesítésre. |
| TimeRaised | Dátum és idő a figyelmeztető hozott létre. |
| TimeResolved | Dátum és idő, amely a figyelmeztető megoldását. Ha a figyelmeztető még nem megszűnt-e üres. |

## <a name="sample-log-searches"></a>Minta napló keresések

Az alábbi táblázat ismerteti a minta napló megkeresi a riasztási rekordok gyűjti össze a megoldást.  

| Lekérdezés | Leírás |
|:--|:--|
| Típus = riasztási SourceSystem OpsManager AlertSeverity = = hiba TimeRaised > most 24 ÓRÁS | Az elmúlt 24 óra során kritikus értesítések |
| Típus = riasztási AlertSeverity figyelmeztetés TimeRaised = > most 24 ÓRÁS | Figyelmeztetés riasztások során az elmúlt 24 óra  |
| Típus = riasztási SourceSystem OpsManager AlertState =! zárt TimeRaised = > most 24 ÓRÁS és #124; Mérje le a count() száma szerint SourceDisplayName szerint | Adatforrások az elmúlt 24 órán belül hatványát aktív értesítések |
| Típus = riasztási SourceSystem OpsManager AlertSeverity = = hiba TimeRaised > most 24 ÓRÁS AlertState! = lezárva | Az elmúlt 24 óra, amelyek még aktív során kritikus értesítések |
| Típus = riasztási SourceSystem OpsManager TimeRaised = > most 24 ÓRÁS AlertState = bezárása | Az elmúlt 24 óra, most már be van zárva, amely során értesítések |
| Típus = riasztási SourceSystem OpsManager TimeRaised = > NOW - 1 nap és #124; Mérje le a count() száma szerint AlertSeverity szerint | Értesítések során a múltbeli 1 nap, azok súlyosságát szerint csoportosítva, emelt |
| Típus = riasztási SourceSystem OpsManager TimeRaised = > NOW - 1 nap és #124; RepeatCount desc rendezése | Értesítések, emelt közben az ismétlési számot értékük alapján rendezett múltbeli 1 nap |

## <a name="next-steps"></a>Következő lépések

- Értesítések létrehozása a napló Analytics részletes [napló Analytics riasztásai](log-analytics-alerts.md) találhat.
