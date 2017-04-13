<properties 
    pageTitle="Irányítópult, a Monitor, a méretarány konfigurálásához és hibrid kapcsolatok BizTalk szolgáltatások |} Microsoft Azure" 
    description="További tudnivalók: a vezérlők és BizTalk szolgáltatások a klasszikus portál lapokon teljesítmény figyelését: irányítópult, a Monitor, a méretezés, a konfigurálása és a hibrid kapcsolatok. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Tekintse át az irányítópult, a Monitor, a skála, a konfigurálása és a hibrid kapcsolat lapon

A BizTalk szolgáltatás hozzon létre, és az alkalmazások telepítése után módosíthatja a BizTalk szolgáltatás beállításai és az alkalmazás teljesítmény figyelését. 

Az Azure klasszikus portál megnyitásakor automatikusan bekerülnek az **Összes elem** lapját. A BizTalk szolgáltatásban az **Összes elem** lapon jelölje be a BizTalk szolgáltatásban, vagy jelölje ki a **BIZTALK szolgáltatások** lap; és válassza ki a BizTalk szolgáltatás nevét.

Ekkor megnyílik egy új ablakban, a következő lapokon. Ez a témakör ismerteti a következő lapokon.

## <a name="quick-start-quick-startquickstart"></a>Rövid útmutató az első)![Rövid útmutató][QuickStart])
Attól függően, hogy a BizTalk Services Editiont felsorolt összes beállítás nem feltétlenül vehető igénybe. 
<table border="1">
    <tr>
        <td><strong>Eszközökhöz</strong></td>
        <td>Töltse le a BizTalk szolgáltatások SDK csomagjában talál a Visual Studio projektsablonok a helyszíni fejlesztési gépre telepítéséhez. Ezek a sablonok létrehozása a BizTalk szolgáltatásban telepített <strong>BizTalk szolgáltatás eltérések</strong> (átalakítás) a Visual Studio projektek és <strong>BizTalk szolgáltatások</strong> (híd).
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK</a> és <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">telepítése az Azure BizTalk szolgáltatások SDK</a> megjeleníti az első lépéseket.
        </td>
    </tr>
    <tr>
        <td><strong>Partner rendelkezést létrehozása</strong></td>
        <td>Ekkor megnyílik az Azure BizTalk Services portál is Azure, ahol partnerek hozzáadása és X12, AS2, létrehozása és szerkesztése EDIFACT rendelkezést.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Összetevők konfigurálása BizTalk Services portál üzenetküldés szerkesztése</a> az első lépéseket sorolja fel.
        </td>
    </tr>

<tr>
        <td><strong>További tudnivalók a BizTalk szolgáltatások</strong></td>
        <td>Nyissa meg a <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> Ha többet szeretne megtudni az Azure BizTalk szolgáltatások.</td>
</tr>
</table>


Az alsó eszköztáron a következőkre van lehetősége:

<table border="1">

<tr>
<td><strong>Kezelése</strong> a alkalmazás üzembe</td>
<td>Ekkor megnyílik az Azure BizTalk Services portál. A BizTalk Services portál nem a megjelenési szerkesztése beállításait, beleértve a partnerek hozzáadása és X12, AS2, létrehozása és EDIFACT rendelkezést.
<br/><br/>
Az ugyanaz, mint <strong>létrehozása partner rendelkezést</strong> az <strong>Első lépések</strong> lapon.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Összetevők konfigurálása szerkesztése üzenetküldés BizTalk Services portál</a> több információt biztosít a BizTalk szolgáltatások portálon.</td>
</tr>

<tr>
<td><strong>Kapcsolat adatait</strong> , az Access vezérlő Namespace</td>
<td>Válassza a kapcsolat adatait, majd az Access vezérlő Namespace alapértelmezett kibocsátó és alapértelmezett kulcs jelennek meg. Ezek az értékek másolhatja.
<br/><br/>
Az Access vezérlő portálon is megnyithatja. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">A hozzáférés-vezérlés Namespace létrehozása</a> az Access vezérlő portál több információt biztosít.</td>
</tr>

<tr>
<td><strong>Billentyűk szinkronizálása</strong> tároló fiók</td>
<td>Amikor létrehoz egy tárterület-fiókkal, egy elsődleges kulcs és egy másodlagos kulcsa automatikusan létrejönnek. Titkosítási billentyűk a tárterület-fiók elérésének szabályozása A BizTalk szolgáltatás automatikusan használja az elsődleges kulcs. <strong>Szinkronizálási billentyűk</strong> engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges kulcs és a másodlagos kulcs közötti váltáshoz.
<br/><br/>
Ha például azt szeretné, a BizTalk szolgáltatás új elsődleges kulcsot a tárterület-fiókhoz használt. Ennek módja:
<br/><br/>
<ol>
<li>Jelölje ki a BizTalk szolgáltatást, és válassza a <strong>Szinkronizálás billentyűk</strong>. Jelölje ki a másodlagos billentyűt. Ha bejelöli ezt a BizTalk szolgáltatás elindul, a másodlagos kulccsal.</li>
<li>Az Azure klasszikus portálon jelölje ki a tárterület-fiókját, és újbóli létrehozása az elsődleges kulcs. Ne feledje, hogy a BizTalk szolgáltatás használ a másodlagos billentyűt.</li>
<li>Jelölje ki a BizTalk szolgáltatást, és válassza a <strong>Szinkronizálás billentyűk</strong>. Ezután jelölje ki az elsődleges kulcs. Ez a az új elsődleges kulcsot, újbóli létrehozása.</li>
<li>Az Azure klasszikus portálon jelölje ki a tárterület-fiókját, és a másodlagos kulcsa újragenerálása.</li>
</ol>
<br/>
Ez a folyamat "átváltási billentyűzet" nevezik. Az célja, hogy engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges kulcs és a másodlagos kulcs közötti váltáshoz.</td>
</tr>

<tr>
<td><strong>Törlés</strong> az alkalmazás</td>
<td>Kijelölésekor törléséhez a BizTalk szolgáltatásban, és azt rendszerbe minden elem törlődik.</td>
</tr>
</table>


## <a name="dashboard"></a>Irányítópult
Attól függően, hogy a BizTalk Services Editiont felsorolt összes beállítás nem feltétlenül vehető igénybe. 

Ha bejelöli a BizTalk szolgáltatás neve, az irányítópult lap jelenik meg. Az irányítópult lapon a következőkre van lehetősége:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Használatát áttekintése: Használt hibrid kapcsolatok száma látható.
Is megjeleníti az adathasználat GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>A diagram metrikus: Teljesítménymutatók rögzített listájának megjelenítése
Ezek a mértékek adja meg a valós idejű értékeket a BizTalk szolgáltatás állapotát jelző kapcsolatban. A mértékek, a diagramon megjelenített **időtartomány** időtartomány és **relatív** és **abszolút** értéke is választhat. 

Ezek teljesítménymutatók leírását nyissa meg a [Rendelkezésre álló mértékek](#Metrics) ebben a témakörben.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Kiadványok: Listák a BizTalk szolgáltatás tulajdonságai

<table border="1">

<tr>
<td><strong>Adatbázis követés hitelesítő adatok frissítése</strong></td>
<td>A felhasználónév és a követés adatbázisba történő bejelentkezéshez használt jelszó módosítása</td>
</tr>
<tr>
<td><strong>Az SSL-tanúsítvány frissítése</strong></td>
<td>Frissítheti a BizTalk szolgáltatás különböző SSL-tanúsítványt használni. Önaláírt SSL-tanúsítvány, ha automatikusan megjelenik <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">a BizTalk szolgáltatás hozzon létre</a>.</td>
</tr>
<tr>
<td><strong>Tanúsítvány letöltése</strong></td>
<td>Az SSL-tanúsítvány, a helyi számítógépre a BizTalk szolgáltatás által használt töltheti le.</td>
</tr>
<tr>
<td><strong>Állapot</strong></td>
<td>Megjeleníti a BizTalk szolgáltatás jelenlegi állapotának. Lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk szolgáltatások: szolgáltatás állapota diagram</a>. </td>
</tr>
<tr>
<td><strong>Szolgáltatás URL-címe</strong></td>
<td>A BizTalk szolgáltatás URL-CÍMÉT. Az azonos a <strong>Tartományi URL-cím</strong> megadott a BizTalk szolgáltatásban létrehozásakor.</td>
</tr>
<tr>
<td><strong>Nyilvános virtuális IP-(virtuális) cím</strong></td>
<td>Az IP-cím a BizTalk szolgáltatásban. Használja az összes beviteli végponton és a rendszer a forrás címe kimenő forgalmához. Az IP-cím mindaddig, amíg létrehozása a BizTalk szolgáltatásban tartozik. Ha törli a BizTalk szolgáltatás, az IP-cím egy másik BizTalk szolgáltatás van rendelve.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>Hitelesíti a BizTalk szolgáltatással.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Listák a Edition megadni a BizTalk szolgáltatás létrehozásakor.</td>
</tr>
<tr>
<td><strong>Hely</strong></td>
<td>Megjeleníti a földrajzi területhez tartozik a BizTalk szolgáltatásban tárolja.</td>
</tr>
<tr>
<td><strong>Létrehozva</strong></td>
<td>A dátum és idő a BizTalk szolgáltatás létrehozásának jeleníti meg.</td>
</tr>
<tr>
<td><strong>Adatbázis nyomon követése</strong></td>
<td>A nyomon követés táblákat a BizTalk szolgáltatás által használt tároló Azure SQL-adatbázis neve. 
<br/><br/>A követés adatbázis 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények magyarázata</a> részletesen.</td>
</tr>
<tr>
<td><strong>Figyelés és archiválás tárhely</strong></td>
<td>Azure tároló fiók neve, amely a BizTalk szolgáltatás felügyeleti kimenetét tárolja.
<br/><br/>A tároló fiók 
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények magyarázata</a> részletesen.</td>
</tr>
<tr>
<td><strong>Előfizetés neve</strong></td>
<td>Megjeleníti az előfizetést, a BizTalk szolgáltatásban tárolja. Az előfizetés szabályozza az access az Azure klasszikus portálra.</td>
</tr>
<tr>
<td><strong>Előfizetés azonosítója</strong></td>
<td>Előfizetés létrehozásakor automatikusan létrejön egy előfizetés azonosítója. REST API-k használata esetén szüksége lehet adja meg, az előfizetés-azonosítóval.</td>
</tr>
</table>

[BizTalk szolgáltatások: kiépítési használatával Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280) BizTalk szolgáltatás hozzon létre a lépéseket sorolja fel.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Kezelheti a kapcsolat adatait, szinkronizálási billentyűk résznél, és törölje a tálcájának:

<table border="1">

<tr>
<td><strong>Kezelése</strong> a alkalmazás üzembe</td>
<td>Ekkor megnyílik az Azure BizTalk Services portál. A BizTalk Services portál nem a megjelenési szerkesztése beállításait, beleértve a partnerek hozzáadása és X12, AS2, létrehozása és EDIFACT rendelkezést.
<br/><br/>
Az ugyanaz, mint <strong>létrehozása partner rendelkezést</strong> az <strong>Első lépések</strong> lapon.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Összetevők konfigurálása szerkesztése üzenetküldés BizTalk Services portál</a> több információt biztosít a BizTalk szolgáltatások portálon.</td>
</tr>
<tr>
<td><strong>Kapcsolat adatait</strong> , az Access vezérlő Namespace</td>
<td>Az Access vezérlő Namespace, alapértelmezett kibocsátó és alapértelmezett kulcs értékét; jeleníti meg amely másolható.
<br/><br/>
Az Access vezérlő portálon is megnyithatja. A hozzáférési vezérlő portál megegyezik az Active Directory beállítással a bal oldali navigációs ablakban.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">A ACS Namespace kezelése</a> több információt biztosít az Access vezérlő portálon.</td>
</tr>
<tr>
<td><strong>Billentyűk szinkronizálása</strong> tároló fiók</td>
<td>Amikor létrehoz egy tárterület-fiókkal, egy elsődleges kulcs és egy másodlagos kulcsa automatikusan létrejönnek. Titkosítási billentyűk a tárterület-fiók elérésének szabályozása A BizTalk szolgáltatás automatikusan használja az elsődleges kulcs. <strong>Szinkronizálási billentyűk</strong> engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges kulcs és a másodlagos kulcs közötti váltáshoz.
<br/><br/>
Ha például azt szeretné, a BizTalk szolgáltatás új elsődleges kulcsot a tárterület-fiókhoz használt. Ennek módja:
<br/><br/>
<ol>
<li>Jelölje ki a BizTalk szolgáltatást, és válassza a <strong>Szinkronizálás billentyűk</strong>. Jelölje ki a másodlagos billentyűt. Ha bejelöli ezt a BizTalk szolgáltatás elindul, a másodlagos kulccsal.</li>
<li>Az Azure klasszikus portálon jelölje ki a tárterület-fiókját, és újbóli létrehozása az elsődleges kulcs. Ne feledje, hogy a BizTalk szolgáltatás használ a másodlagos billentyűt.</li>
<li>Jelölje ki a BizTalk szolgáltatást, és válassza a <strong>Szinkronizálás billentyűk</strong>. Ezután jelölje ki az elsődleges kulcs. Ez a az új elsődleges kulcsot, újbóli létrehozása.</li>
<li>Az Azure klasszikus portálon jelölje ki a tárterület-fiókját, és a másodlagos kulcsa újragenerálása.</li>
</ol>
<br/>
Ez a folyamat "átváltási billentyűzet" nevezik. Az célja, hogy engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges kulcs és a másodlagos kulcs közötti váltáshoz.</td>
</tr>

<tr>
<td><strong>Törlés</strong> az alkalmazás</td>
<td>A BizTalk szolgáltatásban és a hozzá rendszerbe elemeinek törlődnek.</td>
</tr>
</table>


## <a name="monitor"></a>Monitor
Nem vonatkozik az ingyenes Edition.

Ha bejelöli a BizTalk szolgáltatás neve, a Monitor lapján érhető el, és a következő üzenetet jeleníti meg:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>A diagram metrikus: A kijelölt teljesítménymutatók jeleníti meg.
Ezek a mértékek adja meg a valós idejű értékeket a BizTalk szolgáltatás állapotát jelző kapcsolatban. Kiválaszthatja, hogy melyik teljesítménymutatók jelennek meg. Egyszerre legfeljebb hat teljesítménymutatók is láthatók. 

A mérési módja miatt megjelenített **időtartomány** időtartomány és **relatív** és **abszolút** értéke is választhat. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Távolítsa el vagy mértékek megjelenítése a diagramon:
1. Jelölje ki a **Monitor** fülre.
2. Válassza a **Mértékek hozzáadása** a tálcán.  
![Válassza a mértékek hozzáadása][AddMetrics]
3. Jelölje be a megjeleníteni kívánt teljesítménymutatók.
4. Jelölje be a négyzetet a **Monitor** lapra való visszatéréshez.
5. Jelölje ki a kör mellett a mérőszám, hogy metrikus érték megjelenítése a diagramon.  

    Ha például a **Processzorhasználata** mérőszám szürke; az eredményt a diagramban nem jelenik meg:  
![Processzorhasználata metrikus szürkén jelenik meg][GrayedMetric]  

    Jelölje ki a a szürke kör ahhoz, hogy a **Processzorhasználata** mérőszám, a diagram jeleníthet meg az eredményt meg:  
![Processzorhasználata metrikus engedélyezve van][EnabledMetric]

6. Ha el szeretne távolítani egy mérőszám a megjelenítési diagram és a lista, jelölje ki a tálcán **Metrikus törlése** . Metrikus hátraküldése felvétele a listára, válassza a **Metrikus hozzáadása** a tálcán, jelölje be a mérőszám, és jelölje be a négyzetet a **Monitor** lapra való visszatéréshez. Jelölje ki a a szürke ahhoz, hogy a mérőszám kör meg.

## <a name="Metrics"></a>Rendelkezésre álló mérőszámok
A következő számláló/teljesítménymutatók állnak rendelkezésre:

<table border="1">

<tr>
<td><strong>RountdTrip időtartama</strong></td>
<td>Jeleníti meg az átlag, egy üzenet az időpontot, az üzenet érkezik, amíg az üzenet teljes mértékben a BizTalk szolgáltatás által feldolgozott feldolgozása ezredmásodperc eltelt idő az összes hidakat keresztül. Csak az üzenetek sikeresen feldolgozott számítanak.<br/><br/>
Az alábbi események végrehajtásakor időbélyeg jön létre:
<ul>
<li>Üzenet belép az átjáró</li>
<li>Üzenet továbbítás a megadott helyre</li>
<li>Cél válasz érkezik</li>
<li>Az átjáró küldött cél nyugtázó válasz</li>
</ul>
<br/>
Ez a mérőszám a következő számítás eredményét jeleníti meg:
<br/><br/>
[Cél nyugtázó küldött válaszok az átjáró] - [üzenet belép az átjáró]</td>
</tr>
<tr>
<td><strong>A forrás hibák</strong></td>
<td>Megjeleníti az üzenetek nem sikerült összesen hány BizTalk szolgáltatás, amikor a forrás végpontok érkező üzenetek adatok.</td>
</tr>
<tr>
<td><strong>Processzorhasználata</strong></td>
<td>Az átlag %-át processzor példányait szerepkört listája.</td>
</tr>
<tr>
<td><strong>Késés feldolgozása</strong></td>
<td>Megjeleníti az átlagos idő ezredmásodperc az üzenet feldolgozása BizTalk szolgáltatás át az összes hidakat, kivéve a célok töltött időt. Csak az üzenetek sikeresen feldolgozott számítanak.<br/><br/>
Az alábbi események minden előfordulásakor időbélyeg jön létre:

<ul>
<li>Üzenet belép az átjáró</li>
<li>Üzenet továbbítás a megadott helyre</li>
<li>Cél válasz érkezik</li>
<li>Az átjáró küldött cél nyugtázó válasz</li>
</ul>
<br/>Ez a mérőszám a következő számítás eredményét jeleníti meg:<br/><br/>
[Cél nyugtázó küldött válaszok az átjáró] - [üzenet belép az átjáró] - [cél választ] + [üzenet továbbítása a cél]</td>
</tr>
<tr>
<td><strong>Folyamatban lévő hibák</strong></td>
<td>Jeleníti meg, amelyeket nem sikerült BizTalk szolgáltatás feldolgozása közben át az összes hidakat belül az egyik időszakra üzenetek száma.</td>
</tr>
<tr>
<td><strong>Elküldött üzenetek</strong></td>
<td>Megjeleníti az egyik időszakra eső összes hidakat végig a BizTalk szolgáltatás által küldött üzenetek száma. Egy folyamat küldött üzenet az útvonal cél eléri a mérőszám nő A mérőszám nem ezzel azt jelzi, hogy a program sikeres dolgozza fel egy üzenetet.<br/><br/>
A kérés / válasz esetben a mérőszám értéke növekszik, ha az útvonal cél küld egy visszaigazolás nyugtázó a folyamat.</td>
</tr>
<tr>
<td><strong>A Beérkezett üzenetek</strong></td>
<td>Megjeleníti az egyik időszakra eső összes hidakat keresztül BizTalk szolgáltatás Beérkezett üzenetek száma. A mérőszám van nő, amikor egy új üzenetet megkapta a folyamat.</td>
</tr>
<tr>
<td><strong>A folyamat üzenetek</strong></td>
<td>Megjeleníti az egyik időszakra eső BizTalk szolgáltatás jelenleg feldolgozása üzenetek száma.</td>
</tr>
<tr>
<td><strong>Üzenetek feldolgozása</strong></td>
<td>Megjeleníti az üzenetek sikeresen a szolgáltatás által feldolgozott BizTalk összes hidakat át az egyik időszakra eső teljes számát. A mérőszám van nő, ha egy üzenet sikeresen megkapta a folyamat, és sikeresen a rendszer nem a cél.</td>
</tr>
</table>


## <a name="scale"></a>Méretarány
A Méret lap hozzáadása vagy kivonása a BizTalk szolgáltatás által használt egységek számát. Alapértelmezés szerint nincs egyik egység beállítva. További egységek bővíthető méretezni a BizTalk szolgáltatásban. Ha a méretezés növeléséhez növelésével nagyobb kapacitása. Erőforrások mennyiségét is nő, például a telepített hidakat, rendelkezést, üzleti kapcsolatok és feldolgozása power. Például növelheti a 2 egység 1 egységből nagyítást. Ebben az esetben a dupla hidakat számát telepíthető, duplán a rendelkezést, duplán a üzleti kapcsolatok és a feldolgozás power duplán.

Egyes BizTalk kiadásai nem ajánlja fel a egy méretarány lehetőséget. Ebben az esetben az egységnyi engedélyezett (permitted). Annak megállapításához, hogy hány egységek a edition megadhat, nézze meg [BizTalk szolgáltatások: kiadásai diagram](biztalk-editions-feature-chart.md).

Erőforrás-mennyiség számának növelése hatással lehet a árak. Ha növeli az erőforrás-mennyiség, a kiválasztása a **Mentés** , amely elárulja, ha számlázási hatással van-e üzenetet jeleníti meg. Akkor válassza a folytatáshoz. Amikor növelheti a mennyiségek, a BizTalk szolgáltatás állapotot aktív frissítése. A frissítése állapotban a BizTalk szolgáltatásban továbbra is fennáll, futtatásához.

[BizTalk szolgáltatások: kiadásai diagram](biztalk-editions-feature-chart.md) "egység".


## <a name="configure"></a>Konfigurálása
Nem vonatkozik a hibrid kapcsolatok.

A biztonsági mentés állapotának állítja be a Nincs értékre állításával vagy automatikus. Ha a beállítás nincs értékre állításával, nincs biztonsági másolatok automatikusan létrejön egy. Ha értéke automatikus, beállíthatja a biztonsági másolat helyét, a gyakoriság a biztonságimásolat-fájlok megtartása a biztonsági mentés és mennyi. 

[BizTalk szolgáltatások: biztonsági mentési és visszaállítási](biztalk-backup-restore.md) a részletesen. 


## <a name="HybridConnections"></a>Hibrid kapcsolatok
Hibrid kapcsolatok csatlakoztatása egy helyszíni erőforrás, amely egy statikus portot használja, például SQL Server, a MySQL, a HTTP webes API-hoz és a legtöbb egyéni webszolgáltatásokhoz egy Azure alkalmazást, például a Web Apps alkalmazások és a Mobile alkalmazások Azure App szolgáltatásban. Hibrid kapcsolatok BizTalk szolgáltatások kezelhetők az Azure klasszikus portálon.

Hibrid kapcsolatok létrehozása az Azure alkalmazás szolgáltatás, olvassa el a [hozzáférést a helyszíni hibrid kapcsolatok Azure alkalmazás szolgáltatás használata erőforrások](../app-service-web/web-sites-hybrid-connection-get-started.md).

Hozzon létre vagy hibrid kezelni az Azure BizTalk szolgáltatásokban, akkor olvassa el a [Hibrid kapcsolatok](integration-hybrid-connection-overview.md).



## <a name="next"></a>Következő
Most, hogy a különböző lapokon ismerős, többet is megtudhat az Azure BizTalk szolgáltatásaival kapcsolatos:

- [BizTalk szolgáltatások: szabályozása](biztalk-throttling-thresholds.md)  
- [BizTalk szolgáltatások: Kibocsátó neve és a kibocsátó billentyűk](biztalk-issuer-name-issuer-key.md)  
- [BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](biztalk-backup-restore.md)

## <a name="see-also"></a>Lásd még:
- [Hibrid kapcsolatok](integration-hybrid-connection-overview.md)  
- [BizTalk szolgáltatások: Fejlesztőeszközök, Basic, normál és Premium kiadásában diagram](biztalk-editions-feature-chart.md)  
- [BizTalk szolgáltatások: Kiépítési használatával Azure klasszikus portál](biztalk-provision-services.md)  
- [BizTalk szolgáltatások: BizTalk szolgáltatás állapota diagram](biztalk-service-state-chart.md)  
- [Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
