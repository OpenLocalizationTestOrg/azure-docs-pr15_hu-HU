<properties
    pageTitle="Frissítse a MOBILE projektmenedzsment megoldás |} Microsoft Azure"
    description="Ebből a cikkből megismerheti, hogyan kezelheti a frissítéseket a Windows és Linux számítógépek a megoldás segítségével."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![A MOBILE projektmenedzsment megoldás frissítése](./media/oms-solution-update-management/update-management-solution-icon.png) A MOBILE projektmenedzsment megoldás frissítése

A frissítés projektmenedzsment megoldás a MOBILE lehetővé teszi a Windows és Linux számítógépek frissítéseinek kezelése.  Mérje fel, hogy minden ügynök számítógépen elérhető frissítések állapotának gyors, és a szükséges kiszolgáló-frissítések telepítése a megkezdéséhez. 

## <a name="prerequisites"></a>Előfeltételek

-   Windows-ügynökök vagy kell beállítania, hogy a Windows Server Update Services (WSUS) kiszolgáló kommunikál, vagy fér hozzá Microsoft Update.  

    >[AZURE.NOTE] A Windows-ügynök nem lehet kezelni egyidejűleg System Center Configuration Manager szerint.  
  
-   Linux ügynökök hozzáféréssel kell rendelkeznie egy frissítés tárházba.  A MOBILE Agent Linux letölthető [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Konfiguráció

A következő lépésekkel vehet fel a frissítés projektmenedzsment megoldás a MOBILE munkaterület, és adja hozzá a Linux ügynökök.  Windows-ügynökök automatikusan hozzáadja a nincs további beállításokkal.

1.  A frissítés projektmenedzsment megoldás hozzáadása a MOBILE munkaterület [hozzáadása MOBILE megoldások](../log-analytics/log-analytics-add-solutions.md) a megoldástárból leírt eljárással.  
2.  A MOBILE portálon válassza a **beállítások ikonra** , majd kattintson a **Csatlakoztatott adatforrásból**.  Megjegyzés: a **munkaterület-Azonosítóját** , illetve az **elsődleges kulcs** vagy **másodlagos kulcsa**.
3.  Hajtsa végre az alábbi lépéseket minden Linux számítógéphez.

    egy.  Telepítse a legújabb a MOBILE Agent Linux a következő parancs futtatásával.  Csere <Workspace ID> a munkaterület-azonosítóval és <Key> az elsődleges és másodlagos használatával.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Ha el szeretné távolítani a agent, futtassa a következő parancsot.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Adatkezelési csomagok

Ha a System Center Operations Manager-kezelés csoport nem csatlakozik a MOBILE munkaterület, majd a következő management csomagok telepíti az Operations Manager a megoldás hozzáadásakor. Nem tartozik konfigurációs vagy az alábbi felügyeleti csomagok szükséges a karbantartás. 

-   Microsoft System Center Advisor frissítés értékelési intelligencia csomag (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Telepítési MP frissítése

Megoldás management csomagok frissítésének kapcsolatos további tudnivalókért olvassa el a [Operations Manager kapcsolatot a naplóban Analytics](../log-analytics/log-analytics-om-agents.md)című témakört.

## <a name="data-collection"></a>Adatok gyűjtése

### <a name="supported-agents"></a>Támogatott ügynökök beállítása

Az alábbi táblázat ismerteti a kapcsolódó források a megoldás által támogatott.

Csatlakoztatott adatforrás | Támogatott | Leírás|
----------|----------|----------|
A Windows ügynökök beállítása | igen | A megoldást gyűjti össze a frissítéseket a rendszer információt a Windows-ügynökök, és a szükséges frissítések telepítésének kezdeményez.|
Linux ügynökök beállítása | igen | A megoldás Linux ügynökök gyűjti össze a frissítéseket a rendszer információt.|
Műveletek Manager kezelése csoport | igen | A megoldás rendszer frissítéseit adatait gyűjti össze a ügynökök egy csatlakoztatott kezelése csoportban.<br>Log Analytics az Operations Manager ügynök közvetlen kapcsolat nincs szükség. Adatok a MOBILE tárházba továbbítja a felügyeleti csoportból.|
Azure tárterület-fiók | nem | Azure tároló nem része a frissítéseket a rendszer információt.|  

### <a name="collection-frequency"></a>A webhelycsoport gyakorisága

Az egyes felügyelt Windows rendszerű számítógépről az ellenőrzés naponta kétszer történik.  Ha telepítve van a frissítés, annak adatait 15 percen belül frissül.  

A keresés végrehajtott minden felügyelt Linux számítógépen 3 óránként.  

## <a name="using-the-solution"></a>A megoldás használata

A frissítés projektmenedzsment megoldás vesz fel a MOBILE munkaterület, a **Frissítés Management** csempére kerüljön a MOBILE irányítópult. Ez a csempe jelzi a darab és a grafikus ábrázolása számítógépek számát a jelenleg a rendszer frissítést igénylő környezetben.<br><br>
![Adatkezelési összefoglaló csempe frissítése](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Megtekintés frissítés felmérések

Kattintson a **Frissítés Management** csempére kattintva nyissa meg az **Adatkezelési Update** irányítópulton. Az irányítópult az oszlopot az alábbi táblázat tartalmazza. Minden egyes oszlopát az oszlop a megadott hatókör és időtartomány feltételnek legfeljebb tíz elem sorolja fel. A napló keresés, amely minden rekordot visszaad, a annak az oszlopnak a képernyő alján **látható a teljes** gombra kattintva vagy az oszlopfejlécre kattintva futtathatja.

Oszlop | Leírás|
----------|----------|
**Hiányzik a frissítések számítógépeken** ||
Kritikus vagy biztonsági frissítések | A felső tíz számítógépek hiányzó frissíti a frissítések száma szerint rendezett listák legyenek hiányzik. Kattintson a log keresés adatszolgáltató összes rekordjainak az adott számítógépre futtatásához a számítógép nevét.|
Kritikus vagy 30 napnál régebbi biztonsági frissítések| Azonosítja, amelyek számítógépek száma hiányzik a kritikus vagy a biztonsági frissítések csoportosítva vannak közzététele a frissítés óta idejének hosszát. Kattintson az egyik adatszolgáltató összes hiányzik, és a fontos frissítések napló keresés futtatásához a bejegyzéseket.|
**Hiányzik a frissítések szükséges**||
Kritikus vagy biztonsági frissítések | Sorolja fel, hogy számítógépek hiányoznak a hiányzó frissítések kategóriájában számítógépek száma szerint rendezett frissítések esettípusokkal. Kattintson egy osztályozás adatszolgáltató összes rekordjainak adott osztályozási napló keresés futtatásához.|
**Frissítés telepítések**||
Frissítés telepítések | Aktuális ütemezett frissítés telepítések és az időtartamot, amíg a következő ütemezett futtatás száma.  Kattintson a ütemezések megtekintése, futó és kész frissítések vagy egy új telepítési ütemezése csempére.|  
<br>  
![Adatkezelési összefoglaló irányítópult frissítése](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Adatkezelési irányítópult számítógép nézet frissítése](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Adatkezelési irányítópult csomag nézet frissítése](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Frissítések telepítése

Frissítések a Windows számítógépek, a környezetben összes volna értékelni, ha meg is szükséges: hozzon létre egy *Frissítési telepítési*telepített frissítéseket.  A frissítés telepítése egy vagy több Windows számítógépre szükséges frissítéseinek ütemezett telepítés.  Megadhatja a dátum és idő a telepítéshez, a számítógép és a számítógépek csoportja, amelyek szerepeljenek mellett.  

Az Azure automatizálás runbooks frissítések telepítését.  Jelenleg nem tekinthetők meg ezeket a runbooks, és nem szükséges beállításokat.  Egy frissítése telepítési létrehozásakor azt hoz létre ütemezés, hogy elindítja a fő frissítési runbook a tájékoztató számítógépeken megadott időpontjában.  A fő runbook a gyermek runbook minden hajtja végre a szükséges frissítések telepítése Windows-ügynök kezdi.  

### <a name="viewing-update-deployments"></a>Megtekintés frissítés telepítések

Kattintson a **Frissítés telepítése** csempére meglévő frissítés telepítések listájának megtekintéséhez.  A csoportosított állapot – **ütemezett** **futtatása**és **a kész**szerint.<br><br> ![Frissítés telepítésének Ütemezés lap](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

A Tulajdonságok jelenik meg az összes frissítés telepítése az alábbi táblázatban ismertetett.

A tulajdonság | Leírás|
----------|----------|
név | A frissítés telepítése neve.|
Ütemterv | Az ütemezési típust.  *OneTime* jelenleg csak lehetséges értékét.|
Elindítása|Dátum és idő, hogy a frissítés telepítése indítására van ütemezve.|
Időtartam | A frissítés telepítése futtatása engedélyezett percek száma.  Ha az összes frissítések telepítése nem belül ez az időtartam, a hátralévő frissítések várnia kell a következő frissítés telepítése.|
Kiszolgálók | A frissítés telepítése által érintett számítógépek száma.|
Állapot | A frissítés telepítése aktuális állapota.<br><br>Lehetséges értékek a következők:<br>-Nem kezdődött el<br>-Fut<br>-Befejezése|  

Kattintson a részletek képernyőn az alábbi táblázat az oszlopok tartalmazó megtekintéséhez frissítés telepítéseken.  Ezek az oszlopok nem töltik, ha a frissítés telepítése még nem indult el.<br>

Oszlop | Leírás|
----------|----------|
**Számítógép eredménye**||
Sikeresen befejeződött | A számítógépek, a frissítés példányban száma állapot sorolja fel.  Kattintson a adatszolgáltató összes rekordok frissítése, hogy állapotú frissítése telepítéshez napló keresés futtatásához állapotot.|
Számítógép telepítési állapotát.| A frissítés telepítése és a frissítések telepítése sikerült résztvevő számítógépek listája. Kattintson az egyik adatszolgáltató összes hiányzik, és a fontos frissítések napló keresés futtatásához a bejegyzéseket.|
**Frissítés példány eredménye**||
Példány telepítési állapotát. | Sorolja fel, hogy számítógépek hiányoznak a hiányzó frissítések kategóriájában számítógépek száma szerint rendezett frissítések esettípusokkal. Kattintson a számítógép adatszolgáltató összes rekordjainak az adott számítógépre napló keresés futtatása gombra.|  
<br><br> ![Frissítés telepítési eredmények áttekintése](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>A frissítés telepítése létrehozása

Hozzon létre egy új frissítés telepítése elemre kattintva nyissa meg az **Új frissítés telepítése** lapon a képernyő tetején a **Hozzáadás** gombra kattintva.  Az a tulajdonságokat az alábbi táblázat értékeit meg kell adnia.

A tulajdonság | Leírás|
----------|----------|
név | Egyedi nevet, amely azonosítja a frissítés telepítése.|
Időzóna | A kezdő időpont használandó időzónát.|
Elindítása | Dátum és idő a frissítés telepítése indításához.|
Időtartam | A frissítés telepítése futtatása engedélyezett percek száma.  Ha az összes frissítések telepítése nem belül ez az időtartam, a hátralévő frissítések várnia kell a következő frissítés telepítése.|
Számítógépek | Azoknak a számítógépen vagy a számítógép csoportok szeretné hozzáadni a frissítés telepítése.  A legördülő listában jelöljön ki egy vagy több tételt.|
<br><br> ![Új frissítés telepítés lap](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Időtartomány

A frissítés projektmenedzsment megoldás elemezheti az adatok körének alapértelmezés szerint ki van az összes csatlakoztatott felügyeleti csoportból hozza létre az utolsó 1 nap belül. 

Az adatok időtartomány, válassza ki az **adatok alapján** az irányítópult tetején. Választhat a rekordok létrehozásakor vagy frissítésekor az utóbbi 7 napból 1 nap és 6 órával belül. Vagy válassza az **egyéni** , és adja meg az egyéni dátumtartományra vonatkozóan.<br><br> ![Egyéni időtartomány beállítás](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Naplóbejegyzések Analytics

A frissítés projektmenedzsment megoldás kétféle típusú rekordok a MOBILE adattárban hoz létre.

### <a name="update-records"></a>Rekordok frissítése

Minden egyes frissítés van telepítve, vagy szükséges az összes olyan számítógépen, egy rekordot a **frissítés** típusa jön létre. Frissítheti a rekordokat az alábbi táblázat az jellemzőkkel rendelkeznek.

A tulajdonság | Leírás|
----------|----------|
Típus | *Frissítés*|
SourceSystem | A forrás, hogy a frissítés telepítése jóváhagyott.<br>Lehetséges értékek a következők:<br>-A Microsoft frissítés<br>– A Windows Update<br>-SCCM<br>-Linux kiszolgálók (beolvasni a menedzserek csomag)|
Jóváhagyott | Itt adhatja meg, hogy elfogadott-e a frissítés telepítéshez.<br> Linux kiszolgálók esetén ez jelenleg nem kötelező, mint javítása nem kezeli MOBILE.|
A Windows besorolás | A frissítés besorolása.<br>Lehetséges értékek a következők:<br>-Alkalmazások<br>-A fontos frissítések<br>-Definíciók frissítései<br>-Szolgáltatás csomagok<br>-Biztonsági frissítések<br>-Szolgáltatás csomagok<br>-Kumulatív<br>-Frissítések|
Osztályozás Linux | A frissítés cassification.<br>Lehetséges értékek a következők:<br>-A fontos frissítések<br>-Biztonsági frissítések<br>-Frissítések|
Számítógép | A számítógép nevét.|
InstallTimeAvailable | Megadja, hogy a telepítést, hogy ugyanazt a frissítés telepítése más ügynökök elérhető.|
InstallTimePredictionSeconds | Becsült telepítési időpont másodperc más ügynökök, hogy ugyanazt a frissítés telepítése alapján.|
KBID | A frissítés leíró Tudásbázis-cikk azonosítója.|
ManagementGroupName | A kezelés csoport SCOM ügynökök nevét.  Más ügynökök, ez az AOI -<workspace ID>.|
MSRCBulletinID | A Microsoft biztonsági közlemény azt mutatja be, a frissítés azonosítója.|
MSRCSeverity | A Microsoft biztonsági közlemény súlyosságát.<br>Lehetséges értékek a következők:<br>-Kritikus<br>– Fontos<br>-Moderálása|
Nem kötelező. | Itt adhatja meg, hogy a frissítés nem kötelező.|
Termék | A frissítés termék neve.  Kattintson a **Nézet** a böngészőben nyissa meg a cikket.|
PackageSeverity | A rögzített, frissítés, a Linux distro gyártók jelentése rés súlyosságát. | 
PublishDate | Dátum és idő, hogy a frissítés telepítése.|
RebootBehavior | Itt adhatja meg, ha a frissítés kezd kell indítani a számítógépet.<br>Lehetséges értékek a következők:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | A frissítés változatszám.|
SourceComputerId | Globálisan egyedi azonosítója azonosítja a számítógépen.|
TimeGenerated | Dátum és idő, amely a rekord utolsó módosításának.|
Cím | A frissítés címére.|
Nevét | Globálisan egyedi azonosítója azonosítja a frissítést.|
UpdateState | Itt adhatja meg, hogy a frissítés ezen a számítógépen telepítve van-e.<br>Lehetséges értékek a következők:<br>-Telepített - frissítés telepítve van a számítógépen.<br>-Szükséges - frissítés nincs telepítve, és ezen a számítógépen van szükség.|  

<br>
Amikor elkészíti a bármely napló keresési, amely egy **frissítés** típusú rekordot ad vissza a **frissítések** nézet, amely jelenít meg a frissítéseket a keresés által visszaadott engedélyeiről csempék közül választhat. A **Hiányzó és alkalmazott frissítések** elemeit, hogy kattint, és a **kötelező és választható frissítések** csempéi szűkítheti a címzettek körét, frissítések, hogy állítsa a nézetet. A **lista** vagy **táblázat** megtekintése az egyes rekordok kijelölése<br> 

![Log keresési frissítés nézet rekordtípus frissítése](./media/oms-solution-update-management/update-la-view-updates.png)  

A böngészőben nyissa meg a tudásbáziscikk bármely rekord **KBID** kattinthat a **táblázat** nézetben. Ez lehetővé teszi, hogy gyorsan további információ: az adott frissítés részleteit.<br> 

![Log keresési tábla nézet Csempék rekordtípus frissítések](./media/oms-solution-update-management/update-la-view-table.png)

A **lista** nézetben a hivatkozásra kattintott **Nézet** kattintva nyissa meg a tudásbáziscikk a KBID mellett.<br>

![Log keresési lista nézet Csempék rekordtípus frissítéseknél](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary rekordok

Egy **UpdateSummary** típusú rekord egyes Windows ügynök számítógép jön létre. Ez a rekord frissül minden alkalommal, amikor a számítógépen van beolvasott frissítések. **UpdateSummary** rekord az alábbi táblázatban az tulajdonságokat tartalmaz.

A tulajdonság | Leírás|
----------|----------|
Típus | UpdateSummary|
SourceSystem | OpsManager |
Számítógép | A számítógép nevét.|
CriticalUpdatesMissing | A számítógépen marad a fontos frissítések száma.|
ManagementGroupName | A kezelés csoport SCOM ügynökök nevét. Más ügynökök, ez az AOI -<workspace ID>.|
NETRuntimeVersion | A .NET futtatókörnyezet a számítógépen telepített verziója.|
OldestMissingSecurityUpdateBucket | Az idő kategorizálásához, mivel a legrégebbi hiányzó biztonsági frissítés ezen a számítógépen gyűjtő közzétették.<br>Lehetséges értékek a következők:<br>-Régebbi<br>-180 napja<br>– 150 nappal korábban<br>-120 napja<br>-90 napja<br>-60 napja<br>– Válassza a 30 nap<br>-Legutóbbi|
OldestMissingSecurityUpdateInDays | Közzétett napokat, mivel a legrégebbi hiányzó biztonsági frissítés ezen a számítógépen.|
OsVersion | A számítógépen telepített operációs rendszer verziója.|
OtherUpdatesMissing | A számítógépen marad frissítések száma.|
SecurityUpdatesMissing | Hiányzik a számítógépen biztonsági frissítések száma.|
SourceComputerId | Globálisan egyedi azonosítója azonosítja a számítógépen.|
TimeGenerated | Dátum és idő, amely a rekord utolsó módosításának.|
TotalUpdatesMissing |A számítógépen marad frissítések száma.|
WindowsUpdateAgentVersion | A számítógépen a Windows Update agent verziószáma.|
WindowsUpdateSetting | Hogyan a számítógépen a fontos frissítések telepítéséhez beállítását.<br>Lehetséges értékek a következők:<br>-Letiltva<br>-Értesítés telepítése előtt<br>-Ütemezett telepítés|
WSUSServer | URL-CÍMÉT a WSUS-kiszolgálón Ha a számítógép egy használatára van beállítva.|  

## <a name="sample-log-searches"></a>Minta napló keresések

Az alábbi táblázat ismerteti a minta napló megkeresi a megoldás által gyűjtött frissítheti a rekordokat. 

Lekérdezés | Leírás|
----------|----------|
Hiányzó frissítések minden számítógépen | Típus = frissítés UpdateState = választható szükséges = false & #124; Válassza a számítógépen, cím, KBID, osztályozási, UpdateSeverity, PublishedDate|
Hiányzó frissítések "COMPUTER01.contoso.com" (a saját számítógép neve a csere) | Típus = frissítés UpdateState = választható szükséges = false számítógép = "COMPUTER01.contoso.com" & #124; Válassza a számítógépen, cím, KBID, termék, UpdateSeverity, PublishedDate|
Minden olyan számítógépet hiányoznak a kritikus vagy biztonsági frissítések | Típus = frissítés UpdateState = választható szükséges = false (besorolás = "Biztonsági frissítések" vagy besorolás = "Fontos frissítések")|
Szükség szerint gépek, ahol frissítéseket manuálisan alkalmazza a kritikus vagy a biztonsági frissítések | Típus = frissítés UpdateState = választható szükséges = false (besorolás = "Biztonsági frissítések" vagy besorolás = "Fontos frissítések") számítógépen lévő {típusa UpdateSummary WindowsUpdateSetting = = kézi & #124; Eltérők számítógép} & #124; Eltérők KBID|
Hiba események hiányzik a kritikus rendelkező vagy a biztonsági frissítések szükséges | Típus = esemény EventLevelName = hiba számítógépen lévő {típusa = frissítés (besorolás = "Biztonsági frissítések" vagy besorolás = "Fontos frissítések") UpdateState = választható szükséges = false & #124; Eltérők számítógép}|
Hiányzó kumulatív minden számítógépen | Típus = frissítés választható = false besorolás = "Kumulatív" UpdateState = szükséges & #124; Válassza a számítógépen, cím, KBID, osztályozási, UpdateSeverity, PublishedDate|
Eltérők hiányzó frissítések összes számítógépei között | Típus = frissítés UpdateState = választható szükséges = false & #124; Eltérők cím|
WSUS számítógép tagság | Típus = UpdateSummary & #124; mérték count() WSUSServer szerint|
Az automatikus frissítés konfigurálása | Típus = UpdateSummary & #124; mérték count() WindowsUpdateSetting szerint|
Az automatikus frissítés letiltva számítógépen | Típus = UpdateSummary WindowsUpdateSetting = manuális|  
A lista összes a Linux gépek, amelyek egy csomag frissítés érhető el | Típus = frissítés és OSType = Linux és UpdateState! = "Nincs szükség" & #124; Mérje le a számítógép count()|
Minden a Linux gépek, amelyek egy csomag frissítés érhető el, amely megszünteti a kritikus vagy a biztonsági rés listája | Típus = frissítés és OSType = Linux és UpdateState! "Nincs szükség" = és (besorolás = "Fontos frissítések" vagy besorolás = "Biztonsági frissítések") & #124; Mérje le a számítógép count()|
Az összes csomagok, amelyekre a rendelkezésre álló frissítés listája | Típus = frissítés és OSType = Linux és UpdateState! = "Nincs szükség"|
Minden csomagok, amelyekre a rendelkezésre álló frissítés listáját, amely megszünteti a kritikus vagy a biztonsági rés | Típus = frissítés és OSType = Linux és UpdateState! "Nincs szükség" = és (besorolás = "Fontos frissítések" vagy besorolás = "Biztonsági frissítések")|
A minden olyan elérhető frissítést "Ubuntu" számítógépek listája | Típus = frissítés és OSType = Linux és OSName = Ubuntu & #124; Mérje le a számítógép count()|

## <a name="next-steps"></a>Következő lépések

- [Log Analytics](../log-analytics/log-analytics-log-searches.md) napló keresések segítségével frissítés részletes adatait.

- [Hozzon létre saját irányítópultjai](../log-analytics/log-analytics-dashboards.md) megjelenítő frissítés megfelelőség az felügyelt számítógépre.

- [Értesítések létrehozása](../log-analytics/log-analytics-alerts.md) fontos frissítéseket észlel hiányoznak a számítógépen vagy a számítógépen, amikor letiltja az automatikus frissítések tartalmaz.  




