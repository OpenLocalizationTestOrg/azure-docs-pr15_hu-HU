<properties
    pageTitle="A napló Analytics kapacitás projektmenedzsment megoldás |} Microsoft Azure"
    description="Használhatja a kapacitás tervezés megoldást napló Analytics segít megérteni a Hyper-V kiszolgálók System Center virtuális gép Manager által felügyelt kapacitása"
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
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="capacity-management-solution-in-log-analytics"></a>A napló Analytics kapacitás projektmenedzsment megoldás


Log Analytics a kapacitás tervezés megoldást segítségével megismerheti a Hyper-V kiszolgálók System Center virtuális gép Manager által felügyelt kapacitása. Ez a megoldás System Center Operations Manager és a System Center virtuális gép Manager szükséges. Kapacitás tervezés nem érhető el, ha csak közvetlenül csatlakozó ügynökök. A megoldást az Operations Manager ügynök frissítéséhez telepíteni. A megoldás teljesítmény számláló felügyelt kiszolgálói beolvassa, és adatokat küld a használatát a MOBILE szolgáltatás felhőbeli adatkatalógusába feldolgozása. Logika a használatát adatokon alkalmazott, és a felhőbeli szolgáltatástól rekordok az adatokat. Az idő múlásával szokásai megadott és kapacitás tervezett, aktuális felhasználási alapján.

Például a kivetítés előfordulhat, hogy azonosítsa a további Processzormagok vagy további memória szükség-egyes kiszolgálóhoz. Ebben a példában a kivetítés jelezheti, hogy 30 nap a kiszolgáló lesz szüksége további memória. Ez is segítik a memória frissítésre során a kiszolgáló következő karbantartási ablakot, amelyen két hetente egyszer egyike jelenhet meg.

>[AZURE.NOTE] A kapacitás projektmenedzsment megoldás felvenni kívánt munkaterületek nem érhető el. Azon ügyfelek, akik a kapacitás projektmenedzsment megoldás telepítve van továbbra is használhatja a megoldást.  

Megoldás tervezése kapacitása te000129565 frissítést jelenteni kihívásokkal kapcsolatban a következő vevő címe:

- Virtuális gép Manager és a Operations Manager használatának követelménye
- A csoportok alapján meggátoló testreszabása/szűrő
- Óránkénti adatok összesítése gyakori nem elég
- Nincs virtuális szintű Hírcsatornájában
- Adatok megbízhatóságot

Az új kapacitás megoldás előnyei:

- Nagyobb megbízhatóság és a pontosság támogatja a részletes adatok gyűjtése
- A Hyper-V anélkül, hogy VMM támogatása
- A mértékek, a PowerBI képi megjelenítés
- A virtuális szintű kihasználtsági Hírcsatornájában


## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- Operations Manager szükség a kapacitás projektmenedzsment megoldás.
- Virtuális gép Manager szükség a kapacitás projektmenedzsment megoldás.
- Műveletek Manager kapcsolatot a virtuális gép Manager (VMM) szükség. További információt a csatlakozás a rendszerek, megtudhatja, [hogy miként VMM az Operations Manager csatlakozni](http://technet.microsoft.com/library/hh882396.aspx).
- Log Analytics Operations Manager kell csatlakoznia.
- A kapacitás projektmenedzsment megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.


## <a name="capacity-management-data-collection-details"></a>Kapacitás Management webhelycsoport Adatrészletek

Kapacitás Management gyűjti össze a teljesítményadatok, metaadatok és állapot adatok használata az ügynökök, hogy engedélyezve van.

A következő táblázat mutatja az adatgyűjtési módszerek és más adatait, hogy hogyan adatgyűjtés kapacitás kezelésére.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![nem](./media/log-analytics-capacity/oms-bullet-red.png)|![igen](./media/log-analytics-capacity/oms-bullet-green.png)|![nem](./media/log-analytics-capacity/oms-bullet-red.png)|            ![igen](./media/log-analytics-capacity/oms-bullet-green.png)|![igen](./media/log-analytics-capacity/oms-bullet-green.png)| óránkénti|

A következő táblázat mutatja a kapacitás Management által gyűjtött adattípusok példákat:

|**Adattípus**|**Mezők**|
|---|---|
|Metaadatok|BaseManagedEntityId, ObjectStatus, szervezeti_egység, ActiveDirectoryObjectSid, PhysicalProcessors, hálózatnév, IP-cím, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-címet, NetbiosDomainName, LogicalProcessors, Dns_név, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Teljesítmény|Objektumnév, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Állam|StateChangeEventId, és a StateId, NewHealthState, OldHealthState, környezetben, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, módosítás dátuma, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Kapacitás oldala


 A kapacitás tervezés megoldás telepítése után, a **Kapacitás tervezés** csempére az **Áttekintés** lapon, a MOBILE használatával megtekintheti a megfigyelt kiszolgálók kapacitása.

![Kép: kapacitás tervezés csempe](./media/log-analytics-capacity/oms-capacity01.png)

A mozaik megnyílik a **Kapacitás kezelése** irányítópult, ahol megtekintheti a kiszolgáló kapacitás összefoglalását. Az alábbi csempék kattintással láthatók:

- *Virtuális gép száma*: virtuális gépeken futó kapacitása hátralévő napok számának megjelenítése
- *Számítja ki*: Processzormagok és a rendelkezésre álló memória jeleníti meg
- *Tárterület*: jelenít meg a lemezterülettel, és a lemez késés átlagos
- *Keresés*: az adatok explorer használható minden adat keresése a MOBILE rendszerben

![kép kapacitás felügyeleti Irányítópultjára](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>A kapacitás oldal megtekintése

- Az **Áttekintés** oldalon **Kapacitás kezelése**, gombmenü **kiszámítania** vagy **tároló**.

## <a name="compute-page"></a>Lap kiszámítása

Microsoft Azure MOBILE **kiszámítania** Irányítópult segítségével kihasználtsági, beosztását, napjai tervezett és a infrastruktúrára vonatkozó hatékonyság kapacitás adatainak megtekintése. A **kihasználtság** terület használatával Processzor core és a memória-kihasználtság megtekintése a virtuális gép Hosts. A vetítési eszközzel becsléséhez mennyi kapacitás várhatóan egy adott időtartományban érhető el. A **hatékonyság** terület használatával hogyan hatékony a virtuális gép hosts vannak-e. Csatolt elemek részleteinek őket gombra kattintva tekinthet meg.

Excel-munkafüzet, a következő kategóriák hozhat létre:

- A legnagyobb core kihasználtsági felső hosts
- A legnagyobb memória kihasználtsági felső hosts
- A hatékony virtuális gépeken futó felső hosts
- Felső hosts kihasználtsági szerint
- Alsó hosts kihasználtsági szerint

![a kapacitás Management kiszámítania lap képe](./media/log-analytics-capacity/oms-capacity03.png)


A következő területeket a **kiszámítania** irányítópulton jelennek meg:

**Kihasználtsági**: a virtuális gép hosts Processzor nézet alapvető és a memória kihasználtsági.

- *Használt magmintákat*: SZUM, az összes szolgáltató (%-át a kihasználtság Processzor szorozva állomáson fizikai magmintákat számával).
- *Ingyenes magmintákat*: mínusz használt magmintákat fizikai magmintákat teljes.
- *Százalékos magmintákat érhető el*: szabad fizikai magmintákat fizikai magmintákat száma osztva.
- *Egy virtuális virtuális magmintákat*: virtuális magmintákat teljes a rendszer virtuális gépeken futó száma osztva a rendszer.
- *Fizikai magmintákat megtartásával virtuális magmintákat*: a tényleges magmintákat, a rendszer virtuális gépeken futó által használt összes fizikai magmintákat arányát.
- *A virtuális magmintákat elérhető száma*: a rendelkezésre álló fizikai magmintákat szorozva virtuális core fizikai magmintákat megtartásával.
- *Használt memória*: minden állomások van kihasználtság memória összege.
- *Memória*: összes mínusz használt memória fizikai memória.
- *Százalékos memória érhető el*: fizikai memória teljes fizikai memória osztva.
- *A virtuális memória egy virtuális*: a rendszer virtuális gépeken futó száma osztva a rendszer teljes virtuális memória.
- *A virtuális memória fizikai memória megtartásával*: a rendszer a teljes fizikai memória osztva a rendszer teljes virtuális memória.
- *A virtuális memória érhető el*: a rendelkezésre álló fizikai memória szorozva virtuális memória fizikai memória megtartásával.

**Vetítés eszköz**

A kivetítés eszközzel az erőforrás-kihasználtság megtekintése a múltbeli trendek. Ide tartoznak a virtuális gépeken futó, a memóriahasználat, a core és a tárhely használatát trendjeit. A vetítési képesség algoritmust alkalmaz kivetítés segít, hogy ki az erőforrások minden futtatásakor. Ezzel az elemzéssel tisztábban tervezési, hogy meg tudja a kell vásárolnia további kapacitás (például a memóriahasználat, magmintákat vagy tároló) megfelelő kapacitásának kiszámításához.

**Hatékonyság**

- *Virtuális üresjárati*: segítségével a Processzor- és 10 %-os memória 10 %-nál kisebb az adott időszakra.
- *Virtuális overutilized*: segítségével a Processzor- és 90 %-os memória 90 százalékának az adott időszakra.
- *A Host üresjárati*: segítségével a Processzor- és 10 %-os memória 10 %-nál kisebb az adott időszakra.
- *A Host overutilized*: segítségével a Processzor- és 90 %-os memória 90 százalékának az adott időszakra.

### <a name="to-work-with-items-on-the-compute-page"></a>A számítási lapon elemekkel dolgozni

1. **Kiszámítania** irányítópulton a **kihasználtság** területen a Processzormagok és a használatban lévő memória kapacitás adatainak megtekintése.
2. Kattintson a **Keresés** lap megnyitáshoz és részletes információinak megtekintése egy elemet.
3. A **Vetítési** eszköz lévő csúszka dátum a dátumot, kiválaszthatja, hogy a használt kapacitásának leképezésének megjelenítéséhez.
4. A **hatékonyság** területen kapacitás hatékonyság adatainak megtekintése virtuális gépeken futó és a virtuális gép tárolja.

## <a name="direct-attached-storage-page"></a>Közvetlen csatolt tárterület lap

A **Közvetlen csatolt tárolók** irányítópult MOBILE használatával tárterület-felhasználás, a lemez teljesítmény és a tervezett napon kapacitásával kapacitás adatainak megtekintése. A **kihasználtság** terület használatával lemezterület-használat megtekintése a virtuális gép Hosts. A **Lemez** teljesítményszempont lemez átvitel és késés megtekintése a virtuális gép Hosts is használhatja. A vetítési eszközzel is becslése mennyi kapacitás várhatóan egy adott időtartományban érhető el. Csatolt elemek részleteinek őket gombra kattintva tekinthet meg.

A következő kategóriák kapacitás információk az Excel-munkafüzet hozhat létre:

- Felső lemezterület-használat állomás
- Felső átlagos várakozási állomás

A következő területeket a **tárhely** lapon jelennek meg:

- *Kihasználtsági*: a virtuális gép hosts lemezterület-használat megtekintése.
- *Teljes lemezterület*: SZUM (logikai lemezterületet) az összes szolgáltató
- *Lemezterület használt*: SZUM (használt logikai lemezterületet) az összes szolgáltató
- *Rendelkezésre álló lemezterületet*: teljes lemezterület mínusz használt szabad lemezterület
- *Százalékos lemez használt*: használt teljes lemezterület osztva szabad lemezterület
- *Százalékos lemez elérhető*: teljes lemezterület osztva szabad lemezterület

![a kapacitás Management közvetlen csatolt tárolók lap képe](./media/log-analytics-capacity/oms-capacity04.png)

**Merevlemez teljesítménye**

MOBILE használatával megtekintheti a szabad lemezterület korábbi használatát tendenciáját. A vetítési képesség algoritmust projekt jövőbeli kihasználtsága. A lemezterület-használat különösen a kivetítés képesség lehetővé teszi, hogy a projekt futtatásakor esetleg elegendő szabad lemezterület. Ez segít tervezése a megfelelő tárolási és, hogy mikor kell életbe további tárterületet vásárolni.

**Vetítés eszköz**

A kivetítés eszköz használatával megtekintheti a szabad terület kihasználtság múltbeli trendek. A vetítési képesség is lehetővé teszi a projekt nincs elegendő szabad lemezterület futtatásakor. Ez segít tervezése a megfelelő beosztását, és meg, hogy mikor kell életbe nagyobb tárhelyet vásárolni.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Közvetlen csatolt tárolók lapon elemekkel dolgozni

1. A **Közvetlen csatolt tárolók** irányítópulton a **kihasználtság** területen megtekintheti a lemez kihasználtsági információk.
2. Kattintson egy csatolt a **Keresés** lap megnyitáshoz és részletes információinak megtekintése elemre.
3. A **Lemez teljesítménye** területen megtekintheti az lemez átvitel és késés adatait.
4. A **kivetítés eszköz**csúszkát a dátumot a dátumot, kiválaszthatja, hogy a használt kapacitásának leképezésének megjelenítéséhez.


## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes kapacitás adatkezelés megtekintése.
