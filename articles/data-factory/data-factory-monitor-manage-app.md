<properties 
    pageTitle="Figyelésére és Azure Data Factory folyamatok kezelése" 
    description="Megtudhatja, hogy miként figyelés és felügyeleti alkalmazás használatával figyelésére és Azure adatok gyárak és folyamatok kezelése." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Figyelheti és kezelheti az Azure Data Factory folyamatok új figyelő és felügyeleti alkalmazás használata
> [AZURE.SELECTOR]
- [Azure portál/Azure PowerShell használatával](data-factory-monitor-manage-pipelines.md)
- [Használatával a figyelés és felügyeleti alkalmazás](data-factory-monitor-manage-app.md)

Ez a cikk leírja, hogy miként figyelheti, kezelése és a folyamatok hibakeresési és hozhat létre értesítést kaphat a **Figyelés és felügyeleti alkalmazás**hibákkal kapcsolatos értesítéseket. Az alábbi videóban, ha többet szeretne tudni a figyelés és felügyeleti alkalmazás segítségével is megtekinthet.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Indítása nyomon követése és kezelése alkalmazás egy
Indítsa el a képernyőt, és a kezelés alkalmazást, kattintson az **Adatok gyári** lap az adatok factory- **Figyelés és kezelés** csempe.

![Adatok Factory kezdőlapján csempe figyelése](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Meg kell jelennie a figyelés és felügyeleti alkalmazás indított külön lapon/ablakban.  

![Figyelő és felügyeleti alkalmazás](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Jelenik meg, hogy a böngésző marad, a "Engedélyező...", ha letiltása vagy törölje belőle a jelet **blokk külső cookie-k és webhelyadatok** beállítást (vagy) tarthatja engedélyezett és hozható létre kivétel **login.microsoftonline.com** a és próbálkozzon újra indítása az alkalmazás.


Ha nem látja a listában, a képernyő alján a tevékenység windows, a **frissítés** gombra az eszköztáron, és frissítse a listát. Emellett meg a megfelelő értékeket, a **Kezdő időpont** és **Záró időpont** szűrők.  


## <a name="understanding-the-monitoring-and-management-app"></a>Ismertetése nyomon követése és kezelése alkalmazás
Három fül (**Erőforrás Explorer**, **A nézetek figyelése**és **értesítések**) a bal oldali és az első lapon (erőforrás-Explorer) a alapértelmezés szerint be van jelölve. 

### <a name="resource-explorer"></a>Erőforrás Explorer
A következő jelenik meg: 

- Erőforrás Explorer **fanézetben** kattintson a bal oldali ablaktáblában.
- **Diagram nézetben** a képernyő tetején.
- **Tevékenység Windows** lista alján, a középső ablaktáblában.
- **Tulajdonságok**/**Tevékenység ablak Explorer** lapok, a jobb oldali ablaktáblában. 

Az erőforrás Explorerben összes erőforrás (folyamatok, adatkészleteket, csatolt szolgáltatások) fanézetben adatok gyári látni. Az erőforrás Explorer kijelölhet egy objektumot, ha azt veszi észre az alábbiakat: 

- kapcsolódó adatok gyári entitás ki van emelve a Diagram nézetben.
- tartozó tevékenység windows (kattintson [ide](data-factory-scheduling-and-execution.md) tevékenység windows kapcsolatos) kiemelve a képernyő alján a tevékenység Windows listában.  
- a Tulajdonságok ablak a jobb oldali ablaktáblában a kijelölt objektum tulajdonságai. 
- A kijelölt objektum, ha van ilyen JSON meghatározása. Példa: egy csatolt szolgáltatás vagy egy adatkészlet vagy a folyamat. 

![Erőforrás Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Lásd: [értekezlet ütemezése, illetve végrehajtás](data-factory-scheduling-and-execution.md) cikk tevékenység ablakban elvi részletes információt. 

### <a name="diagram-view"></a>Diagram nézetben
A Diagram nézet adatok gyár üveg figyelésére és az adatok gyári és fekteti kezelése egyetlen ablaktábla biztosít. Amikor kijelöl egy Data Factory entitás (adatkészlet/folyamat) a diagram nézetben, azt tapasztalja, a következőket:
 
- az adatok gyári entitás be van jelölve a fastruktúrájú nézetben
- a tevékenység Windows listáját a kiemelt társított tevékenység windows.
- a Tulajdonságok ablak a kijelölt objektum tulajdonságai

Ha a folyamat engedélyezve van (nem a szünetel), a zöld vonallal látható. 

![A folyamat futtatása](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Azt veszi észre, hogy nincsenek-e a diagram nézetben a tölcsér választógombok parancsot. A második gomb segítségével mutasson a folyamat. Szüneteltetés az éppen futó tevékenységek megszakítása és nem vele az teljesítése folytassa. A harmadik gomb felfüggeszti a folyamat, és megszakítja a meglévő tevékenységek végrehajtása. Első gombra a folyamat folytatódik. A folyamat fel van függesztve, ha azt veszi észre a következőképpen csempe az folyamat színének módosítása.

![Szünet vagy önéletrajz-csempével](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

Több elem kijelölése (CTRL használatával) két vagy több folyamatok és használható gombok szünet/önéletrajz több folyamatok egyszerre szeretne.

![Szünet vagy önéletrajz a parancssáv](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

A folyamat esetében a tevékenységeket: kattintson a jobb gombbal a folyamat csempét, és **Nyissa meg a folyamat**megjelenik.

![Folyamat menü megnyitása](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

A megnyitott folyamat nézetben látható minden tevékenységet a során. Ebben a példában az csak egy tevékenységhez: másolás tevékenységet. Kattintva visszatérhet az előző nézetre, kattintson az adatok gyári nevét a felső navigációs menüben.

![Megnyitott folyamat](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

A folyamat nézetben kattintson a egy kimenet adatkészlet, vagy ha a kimenet adatkészlet fölé viszi az egérmutatót jelenik tevékenység Windows előugró ablakban adott adatkészlet számára.

![Tevékenység Windows helyi menüje](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

A részletek, a **tulajdonság** ablakban a jobb oldali egy tevékenység-ablak gombra. 

![Tevékenység ablak tulajdonságai](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

A jobb oldali váltson kapcsolatban további részletekre kíváncsi a **Tevékenység ablak Explorer** lapra.

![Tevékenység Intéző-ablak](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

Is látható **változók a rendszer** minden tevékenység futtatásakor megpróbálja **kísérletek** szakaszában. 

![Megszűnt változók](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Kattintson a **parancsprogram** fülre, a kijelölt objektum JSON parancsfájl meghatározása megtekintéséhez.   

![Lap Script](./media/data-factory-monitor-manage-app/ScriptTab.png)

Tevékenység windows három helyen tekintheti meg:

- Tevékenység Windows előugró ablak a diagram nézetben (a középső ablaktáblában).
- Tevékenység ablak Explorer, a jobb oldali ablaktáblában.
- A Windows tevékenységlistája az alsó ablaktáblában.

A tevékenység Windows előugró és a tevékenység ablak Explorer görgetve előző hét és a következő hét bal és jobb nyílbillentyű segítségével.

![Tevékenység ablak Explorer balra/jobbra nyíl](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

A Diagram nézet alján megjelenik a Nagyítás a Nagyítás ki nagyítás igazítása elrendezés zárolása, 100 %-os nagyítás gombra. A zárolás Elrendezés gomb megakadályozza, hogy véletlenül táblák és folyamatok áthelyezése a diagram nézetben, és alapértelmezés szerint be Kapcsolva. Kapcsolja ki, és a személyek mozgás a diagramban. Ha Ön kapcsolja ki, az utolsó gomb segítségével automatikusan elhelyezi a táblázatok és folyamatok. Is nagyíthatja / kicsinyítés görgetőkerék használatával.

![Diagram nézet nagyítása parancsok](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Tevékenység Windows lista
A tevékenység windows listáját, a középső ablaktábla alján található összes tevékenység ablaka az adatkészlet, az erőforrás explorer vagy a diagram nézetben kijelölt jeleníti meg. A lista alapértelmezés szerint ki van a csökkenő sorrendben, ami azt jelenti, hogy a legújabb tevékenység ablakának tetején látható. 

![Tevékenység Windows lista](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

A lista nem frissítése automatikusan, ezért a frissítés gombra az eszköztáron kézi frissítéséhez.  


A tevékenység windows lehet a következő állapotok egyikét:

<table>
<tr>
    <th align="left">Állapot</th><th align="left">Részállapot</th><th align="left">Leírás</th>
</tr>
<tr>
    <td rowspan="8">Várakozás</td><td>ScheduleTime</td><td>A tevékenység ablak futtatásához nem eljött az ideje annak.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>A felsőbb szintű függőségeket nem készen állnak.</td>
</tr>
<tr>
<td>ComputeResources</td><td>A számítási erőforrások nem érhetők el.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>A tevékenység-példányok elfoglaltak másik tevékenységeket windows operációs rendszert futtató.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Tevékenység fel van függesztve, és a tevékenység windows nem futtatható, csak akkor folytatódik.</td>
</tr>
<tr>
<td>Ismételje meg</td><td>Tevékenység végrehajtását megismétlése.</td>
</tr>
<tr>
<td>Adatérvényesítési</td><td>Adatérvényesítési még nem kezdődött.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Várakozás az érvényesítési kell ismételni.</td>
</tr>
<tr>
<tr
<td rowspan="2">Esetbejegyzések</td><td>Érvényesítése</td><td>Adatérvényesítési folyamatban van.</td>
</tr>
<td></td>
<td>A tevékenység ablak feldolgozása.</td>
</tr>
<tr>
<td rowspan="4">Nem sikerült.</td><td>Időtúllépése</td><td>Végrehajtási tartott hosszabb, amely szerint a tevékenység engedélyezve van.</td>
</tr>
<tr>
<td>Lemondva</td><td>Felhasználói művelet megszakítja.</td>
</tr>
<tr>
<td>Adatérvényesítési</td><td>Sikertelen volt.</td>
</tr>
<tr>
<td></td><td>Nem sikerült készítése és/vagy a tevékenység ablak ellenőrzése.</td>
</tr>
<td>Készen áll</td><td></td><td>A tevékenység ablakban készen áll a felhasználás.</td>
</tr>
<tr>
<td>Kihagyott</td><td></td><td>A tevékenység ablak feldolgozása nem.</td>
</tr>
<tr>
<td>Nincs lehetőség</td><td></td><td>Egy tevékenység ablak, amely a különböző állapotú olyan használja, de vissza nem állítja.</td>
</tr>
</table>


A lista egy tevékenység-ablak gombra kattint, jelenik részleteit a **Tevékenység Windows Intéző** vagy **Tulajdonságok** ablakában a jobb oldalon.

![Tevékenység Intéző-ablak](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Tevékenység windows frissítése  
A részletek nem automatikusan frissülnek, hogy használni a **frissítés** gomb (második gomb) a kézi frissítéséhez a windows tevékenységlistája parancssávon.  
 

### <a name="properties-window"></a>Tulajdonságok ablak
A Tulajdonságok ablak a jobb szélső ablak figyelő és felügyeleti alkalmazás szerepel. 

![Tulajdonságok ablak](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Az erőforrás explorer (fastruktúrájú nézetben) (vagy) diagram nézet (vagy) tevékenység windows lista kijelölt elem tulajdonságainak megjeleníti. 

### <a name="activity-window-explorer"></a>Tevékenység Intéző-ablak

A jobb szélső ablaktábla, figyelő és felügyeleti alkalmazás, a **tevékenység ablak** Intézőt. Megjeleníti a a tevékenység Windows előugró vagy a tevékenység Windows lista kijelölt tevékenység ablak részleteket. 

![Tevékenység Intéző-ablak](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

A Naptár nézetben a képernyő tetején kattintva válthat másik tevékenységeket ablakba. **Balra mutató nyíl**is használhatja/**Jobbra nyíl** gomb a tevékenység windows a az előző/következő hét megjelenítéséhez a képernyő tetején.

Használhatja az eszköztárgombok **futtassa újra** a tevékenység ablakban, vagy **frissítése** az alsó ablaktáblában a részletek mezőre. 

### <a name="script"></a>Parancsfájl 
A kijelölt adatok gyári entitás (csatolt szolgáltatás, adatkészlet és folyamat) JSON meghatározása megtekintéséhez a **parancsprogram** lap is használhatja. 

![Lap Script](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Rendszer-nézetek használata
A figyelés és felügyeleti alkalmazás tartalmaz beépített rendszer nézetek (**a legutóbbi tevékenység windows**, a **sikertelen tevékenység windows**, a **Folyamatban lévő tevékenység windows**.), amely lehetővé teszi az adatok gyári legutóbbi/nem sikerült vagy a folyamatban lévő tevékenység windows megtekintését. 

Kattintson a **Nézetek figyelése** fülre a bal oldalon kattintson a hivatkozásra. 

![Nézet lap figyelése](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Vannak jelenleg támogatott három rendszer nézetet. Válasszon egy beállítást szeretné látni a legutóbbi tevékenység windows (vagy) sikertelen tevékenység windows (vagy) a folyamatban lévő tevékenység windows a tevékenység Windows listában (a középső ablaktábla alján). 

**A legutóbbi tevékenység windows** lehetőséget választja, jelenik legutóbbi tevékenység ablakainak **utolsó megpróbálja idő**a csökkenő sorrendben. 

A **sikertelen tevékenység windows** nézet megjelenítéséhez a listában az összes tevékenység sikertelen windows is használhatja. Jelölje ki a listában, szeretne tudni, akkor a a **Tulajdonságok** ablak (vagy a) **Tevékenység ablak Intézőben**sikertelen tevékenység ablak. A sikertelen tevékenység ablak bármely naplók is letöltheti. 


## <a name="sorting-and-filtering-activity-windows"></a>Rendezés és szűrés tevékenység windows
A **Kezdő időpont** és **Záró időpont** beállításainak megváltoztatása a parancssávon szűrése tevékenység Windows. Kezdési idő és befejezési idő módosítása után kattintson a gomb mellett frissítheti a tevékenység Windows listáját a Befejezés időpontja.

![Kezdési és befejezési időpontot](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Minden alkalommal jelenleg a figyelő és felügyeleti alkalmazás UTC formátumban. 

Kattintson a **tevékenység Windows listája**egy oszlopának a nevét (például: állapot). 

![Tevékenység Windows lista oszlop menüje](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Végezheti el a következőket:

- Rendezés növekvő sorrendben.
- Rendezze az adatokat a csökkenő sorrendben.
- Szűrés szerint egy vagy több értéket (kész, Várakozás stb.)

Egy oszlop szűrőt ad meg, amikor a szűrő gombra, jelezve, hogy az oszlopban lévő értékek szűrt értékeinek oszlop engedélyezve látni. 

![Tevékenység Windows lista oszlop szűrése](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Az azonos előugró ablak segítségével szüntetni a szűrést. A tevékenység windows lista az összes szűrő törléséhez kattintson a szűrő törlése gomb a parancssávon. 

![A tevékenység Windows listában az összes szűrő törlése](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Köteg műveletek elvégzésére

### <a name="rerun-selected-activity-windows"></a>Futtassa újra a kijelölt tevékenység windows
Egy tevékenység ablakban jelölje be, az első parancs sáv gomb melletti nyílra, és jelölje ki a **ismétlése** / **futtassa újra a előtt során**. **Futtassa újra a előtt során** lehetőséget választja, ha az összes felsőbb szintű tevékenység windows, valamint ismételt futtatása. 
    ![Futtassa újra a egy tevékenység-ablak](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Több tevékenység ablakban jelölje ki a listában, és futtassa újra a őket egyszerre is. Érdemes szűrése tevékenység windows-alapú állapotát (például: **nem sikerült**) majd futtassa újra a sikertelen tevékenység windows a problémát okozó a tevékenység windows meghiúsító javítása után. A következő szakaszban szűrés a lista tevékenység windows olvashat bővebben.  

### <a name="pauseresume-multiple-pipelines"></a>Több folyamatok szünet vagy folytatása
Több elem kijelölése (CTRL használatával) két vagy több folyamatok és használható gombok (piros téglalap az alábbi képen a kiemelt)-szünet/Folytatás őket.

![Felfüggesztése/önéletrajz a parancssáv](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Értesítések létrehozása 
A riasztások lap segítségével jelzést, a meglévő riasztások megtekintése, szerkesztése és törlése. Akkor is is tiltása vagy engedélyezése értesítés. Ha látni szeretné az értesítések lapra, kattintson az értesítések fülre.

![Értesítések fülre](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Értesítés létrehozása

1. **Értesítés hozzáadása** értesítés hozzáadása lehetőségre. Megjelenik a részleteket tartalmazó lapot. 

    ![Értesítések – Részletek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Adja meg a **nevét** és **leírását** az értesítést, és kattintson a **Tovább**gombra. Meg kell jelennie a **szűrők** lapot.

    ![Értesítések - szűrők lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Jelölje ki az **eseményt**, az **állapot**és **Részállapot** (nem kötelező), amelyre az adatok gyári szolgáltatás jelenít meg értesítést, és kattintson a **Tovább**gombra. Meg kell jelennie a **címzettek** lapot.

    ![Értesítések - címzettek lap létrehozása](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. Jelölje ki az **e-mailek előfizetés rendszergazdák** lehetőséget, illetve adja meg a **rendszergazda további e-mailben**, és kattintson a **Befejezés gombra**. Meg kell jelennie a listában az értesítésre. 
    
    ![Értesítések lista](./media/data-factory-monitor-manage-app/AlertsList.png)

A riasztások listában Szerkesztés és Törlés/tiltása vagy engedélyezése értesítés a riasztást társított gombokkal. 

### <a name="eventstatussubstatus"></a>Esemény/állapot/részállapot
A következő táblázat a rendelkezésre álló események és állapotok (és részállapotok) listáját.

Esemény neve | Állapot | Sub állapota
-------------- | ------ | ----------
A tevékenység lépések futtatása | Lépések | Indítása
Futtassa a befejezett tevékenység | Sikeres | Sikeres 
Futtassa a befejezett tevékenység | Nem sikerült.| Erőforrás-terhelés<br/><br/>Nem sikerült végrehajtása<br/><br/>Időtúllépési<br/><br/>Nem sikerült érvényesítése<br/><br/>Félbehagyott
Igény szerinti HDI fürt lépések létrehozása | Lépések | &nbsp; |
Igény szerinti HDI fürtre sikeresen létrehozása | Sikeres | &nbsp; |
Törölt igény szerinti HDI fürtre | Sikeres | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>A Szerkesztés és törlés/letiltása értesítés


![Értesítések gomb](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


