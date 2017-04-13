<properties
    pageTitle="A Cortana intelligencia megoldás sablonba űrtechnikai és a más cégek prediktív karbantartás technikai útmutató |} Microsoft Azure"
    description="A űrtechnikai, segédprogramok és pihenőhelyek prediktív karbantartási technikai útmutató Microsoft Cortana intelligenciával a megoldást sablonba."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>A Cortana intelligencia megoldás sablonba űrtechnikai és a más cégek prediktív karbantartás technikai útmutató

## <a name="acknowledgements"></a>**Nyugták**
Ez a cikk az adatok tudósok Pozsony Zhang, Gauher Shaheen, Fidan Boylu Uz és szoftver adatbázismodellbe Dan Grecoe Microsoft szerzője.

## <a name="overview"></a>**– Áttekintés**

Megoldás sablonok célja, hogy felgyorsítsa a folyamat egy E2E bemutató fölött Cortana üzletiintelligencia-csomagja tudnivalóit. Telepített sablon kiépítése szükséges Cortana üzletiintelligencia-összetevők előfizetés, és a közöttük lévő kapcsolatok összeállítása. A létrehozott egy adatok nyilvántartás-készítő alkalmazás alkalmazás, amely akkor letölti és telepíti a helyi számítógépen, a megoldást sablon telepítését követően a mintaadatokat tartalmazó az adatok folyamat is osztja. Az adatok alapján a nyilvántartás-készítő alkalmazás létre az adatok folyamat és a kezdő létrehozása, amelyek ezután megjeleníthetők a Power BI irányítópulton gépi tanulási előrejelzések fog hydrate. A telepítési folyamatot végigvezeti Önt több lépésben állíthatja be a megoldást hitelesítő adatait. Ellenőrizze, hogy a hitelesítő adatokat, például megoldás neve, felhasználónevét és jelszavát a telepítés során megadott rögzítése.  

A dokumentum a cél, a hivatkozás architektúra és más összetevőket az előfizetése részeként ezzel a sablonnal megoldás kiépítése ismertetik. A dokumentum is folytatott beszélgetést, a mintaadatok lecserélése saját hírcsatornájában, és a saját adatain az előrejelzések láthatja a tényleges adatokat olvashat. Emellett a dokumentum ismerteti, hogy módosítani kell, ha azt a megoldást a saját adatain testre kell megoldást sablon részek. Hogyan lehet a Power BI-irányítópult megoldás sablon utasításokat végén.

>[AZURE.TIP] Letöltheti és nyomtatása [a dokumentum PDF-verzióját](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Áttekintés**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Ha telepíti a megoldást, Cortana Analytics csomagja belül különböző Azure szolgáltatások aktiválódnak (*azaz* esemény-központban Értékáram-elemzés, HDInsight, adatok gyári, gépi tanulási, *stb.*). A fenti architektúra diagram jeleníti meg, a magas szintű hogyan a cserélendő karbantartás űrhajózási megoldás sablon a végpontok közötti összeállítás. Vizsgálja meg ezeket a szolgáltatásokat az azure-portálon parancsával a megoldást sablon diagram a megoldás üzembe helyezése a HDInsight kivételével készült, ez a szolgáltatás igény kiépítéstől, amikor a kapcsolódó folyamat tevékenységek szükségesek futtatása és a törölt ezután tudja.
[A diagram teljes méretű változatát](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png)töltheti le.

Az alábbi szakaszok ismertetik minden darab.

## <a name="data-source-and-ingestion"></a>**Adatforrás és bevitel**

### <a name="synthetic-data-source"></a>Szintetikus adatforrás

Ez a sablon használt adatforrás egy asztali alkalmazás, amely, töltse le és helyben futtatni sikeres telepítés után az jön létre. Az utasításokat követve töltse le és telepítse az alkalmazást a Tulajdonságok sávon, akkor jelölje be az első csomópont prediktív karbantartási adatok nyilvántartás-készítő alkalmazás neve a megoldást sablon diagram találja. Ez az alkalmazás az [Azure esemény központi](#azure-event-hub) szolgáltatás adatpontok vagy események, a többi a megoldást folyamat használt hírcsatornák. Ehhez az adatforráshoz verzióban lévő vagy a [turbóventillátoros motor csökkenés szimulációk adatkészlet](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) [NASA adatok tárházba](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) nyilvánosan elérhető adatainak származik.

Az esemény generációs alkalmazás fog feltöltése az Azure-esemény központban, csak, amíg fut, a számítógépen.

### <a name="azure-event-hub"></a>Azure esemény központi

Az [Azure esemény központi](https://azure.microsoft.com/services/event-hubs/) szolgáltatás érhető el a címzett, a bemeneti a fentebb ismertetett szintetikus adatforrás által biztosított.

## <a name="data-preparation-and-analysis"></a>**Adatok előkészítése és elemzése**


### <a name="azure-stream-analytics"></a>Azure Értékáram-elemzés

Adja meg a bemeneti adatfolyam az [Azure esemény központi](#azure-event-hub) szolgáltatásból valós idejű analytics közelében és -közzététel eredménye egy [Power BI](https://powerbi.microsoft.com) -irányítópult, valamint a archiválására az [Azure](https://azure.microsoft.com/services/storage/) tárhelyszolgáltatáshoz [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás későbbi feldolgozásra összes nyers bejövő esemény alakzatot az [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás szolgál.

### <a name="hd-insights-custom-aggregation"></a>HD-minőségű háttérismeretek egyéni összesítése

Az Azure HD betekintést szolgáltatást biztosít összesítések archiválás az Azure Értékáram-elemzés szolgáltatással történt nyers eseményeket [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok (Azure Data Factory által orchestrated) futtatásához használt.

### <a name="azure-machine-learning"></a>Azure gépi tanulási

Az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) szolgáltatást használják: (Azure Data Factory által orchestrated) az előrejelzések kinagyíthatja a hátralévő leírási idő (Szabályainak) egy adott repülőgép motor megadott érkezett, a bemeneti adatok alapján.

## <a name="data-publishing"></a>**Adatok közzététele**


### <a name="azure-sql-database-service"></a>Azure SQL-adatbázis-szolgáltatás

Az [SQL Azure-adatbázis](https://azure.microsoft.com/services/sql-database/) szolgáltatást használják: tárolásához (Azure Data Factory kezeli) az előrejelzések megkapta az Azure gépi tanulási szolgáltatás, amely a [Power BI](https://powerbi.microsoft.com) irányítópulton fogyni fog.

## <a name="data-consumption"></a>**Adatok fogyasztása**

### <a name="power-bi"></a>A Power BI

A [Power BI](https://powerbi.microsoft.com) szolgáltatás megjelenítése az összesítéseket és értesítések, a [Megjelenítő Értékáram-elemzés Azure](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás által biztosított, valamint Szabályainak az előrejelzések [Azure SQL](https://azure.microsoft.com/services/sql-database/) -adatbázisban tárolt, amelyeket az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) szolgáltatással tartalmazó irányítópult szolgál. Hogyan lehet a Power BI-irányítópult megoldás sablon útmutatásért olvassa el az alábbi szakaszt.

## <a name="how-to-bring-in-your-own-data"></a>**Hogyan kell a saját adatain hozása**

Ez a szakasz ismerteti a saját adatain előtérbe Azure, és mely területeken igényli szeretné a módosításokat, a architektúra összhangba adatokhoz.

Valószínű, hogy minden olyan hozása adatkészlet megfelelő az adatkészlet megoldás sablon használt [turbóventillátoros motor csökkenés szimulációk adatkészlet](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) által használt. Az adatok és a követelmények ismertetése a sablont a saját adatokkal végzett munkához módosíthatja a kulcsfontosságú lesz. Ha ez az első információk megjelenítése a Azure gépi tanulási szolgáltatás, a példa használatával [hogyan hozhat létre az első kísérlet](machine-learning-create-experiment.md)érheti egy bemutatása.

Az alábbi szakaszok bemutatja, hogyan lehet a szakaszok módosítások esetén kell, amikor egy új adatkészletet bevezetett sablon.

### <a name="azure-event-hub"></a>Azure esemény központi

Az Azure esemény központi szolgáltatása nagyon általános, hogy az adatok adhatók fel a központi CSV- vagy JSON formátumban. Speciális feldolgozás nem jelentkezik az Azure-esemény központban, de a fontos, hogy megismeri a mikrofonba dokumentum adatokat.

A dokumentum nem ismertetik, hogyan lehet az adatok ingest, de egyszerűen küldhet események és az adatok Azure esemény hubhoz a esemény központi API.

### <a name="azure-stream-analytics"></a>Azure Értékáram-elemzés

Valós idejű analytics közelében adatfolyamok olvasása és írása az adatokat tetszőleges számú adatforrások nyújtása a Azure Értékáram-elemzés szolgáltatást használják.

A cserélendő karbantartás űrhajózási megoldás sablon az Azure Értékáram-elemzés lekérdezés négy alszint lekérdezések minden egyes igénybe az Azure esemény központi szolgáltatás, és a kimeneti értékeket, hogy négy különböző helyeken eseményeinek áll. Ezek a kimeneti értékeket három Power BI adatkészleteket, és egy Azure tárolási helye állnak.

Az Azure Értékáram-elemzés lekérdezés által találhatók:

-   Bejelentkezés az Azure-portálra

-   Az adatfolyamban analytics feladatok megkeresése ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) , amely jött létre, ha a megoldás telepítették (*pl.*, **maintenancesa02asapbi** és **maintenancesa02asablob** a cserélendő karbantartási megoldás)

-   Kijelölése

    -   Megtekintheti a lekérdezés bemeneti a ***bemeneti adatok alapján***

    -   ***Lekérdezés*** megtekintéséhez magát a lekérdezést

    -   ***EXPORTÁLJA*** a különböző kimeneti értékeket megtekintése

Azure Értékáram-elemzés lekérdezés építés információt a [Adatfolyam Analytics lekérdezés hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx) MSDN webhelyen található.

Ez a megoldás a lekérdezések kimeneti valós idejű analytics információt a Power BI-irányítópult, ezzel a sablonnal megoldás részeként a bejövő adatok adatfolyam mellett a három adatkészleteket. Mivel a bejövő adatformátum implicit ismereteket, ezek a lekérdezések kellene lehet módosítani a adatformátum alapján.

A lekérdezés a második adatfolyam analytics feladat **maintenancesa02asablob** egyszerűen exportálja az összes [Esemény központi](https://azure.microsoft.com/services/event-hubs/) esemény [Azure](https://azure.microsoft.com/services/storage/) tárolóhoz, és így van szükség az adatformátum függetlenül nincs módosítás, a teljes adatai folyamatosan lejátszott tárolóhoz.

### <a name="azure-data-factory"></a>Azure Data Factory

Az [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás orchestrates mozgását és adatok kezelése. Űrhajózási megoldás sablon prediktív fenntartása az adatok gyári tevődnek össze három [folyamatok](../data-factory/data-factory-create-pipelines.md) áthelyezése és különböző technológiákkal adatfeldolgozás.  Az adatok gyári elérheti úgy, hogy megnyitja a az adatok gyári csomópontot a megoldást sablon diagram alján a megoldás üzembe helyezése a készült. Ezzel eljut az adatok gyári az Azure portálon. Hibák a adatkészleteket csoportban jelenik meg, ha figyelmen kívül hagyhatja azokat, mint üzembe helyezéséhez az adatok nyilvántartás-készítő alkalmazás elindítása előtt adatok gyári miatt. Ezek a hibák akadályozza meg, hogy az adatok gyári működik-e.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Ez a szakasz ismerteti, hogy a szükséges [folyamatok](../data-factory/data-factory-create-pipelines.md) és a [tevékenységek](../data-factory/data-factory-create-pipelines.md) az [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)található. Az alábbi van a diagram nézetben a megoldás.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Két a folyamatok, ez a gyár tartalmazzák a [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsprogramok partition és az adatok összesítése szolgálnak. Jegyezni, ha a parancsfájlok található a telepítés során létre [Azure tároló](https://azure.microsoft.com/services/storage/) fiók. Helyükre lesz: maintenancesascript\\\\parancsfájl\\\\struktúra\\ \\ (vagy https://[Your megoldás name].blob.core.windows.net/maintenancesascript).

Hasonló az [Azure Értékáram-elemzés](#azure-stream-analytics-1) lekérdezések, a [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok a bejövő adatformátum implicit ismereteket, ezek a lekérdezések volna kell változtatható meg az adatok formázása és a [szolgáltatás műszaki](machine-learning-feature-selection-and-engineering.md) követelmények alapján.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Ez a [folyamat](../data-factory/data-factory-create-pipelines.md) egy tevékenység - [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl az [Azure-tárolóban](https://azure.microsoft.com/services/storage/) lévő helyezi el a [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/) projekt során adatok partition futó [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) használatával [HDInsightHive](../data-factory/data-factory-hive-activity.md) tevékenységet tartalmaz.

Ehhez a feladathoz particionáló [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Ez a [folyamat](../data-factory/data-factory-create-pipelines.md) több tevékenység is tartozik, és amelynek a végeredménye kísérletezés a scored előrejelzések az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) a kapcsolódó ezzel a sablonnal megoldás.

A tevékenységek található ez a következők:

-   [HDInsightHive](../data-factory/data-factory-hive-activity.md) tevékenység- [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlt összesítések és az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) kísérletet szükséges szolgáltatás műszaki végrehajtásához [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) használatával.
    Ehhez a feladathoz particionáló [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl ***PrepareMLInput.hql***.

-   [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység helyezi át az eredményeket a [HDInsightHive](../data-factory/data-factory-hive-activity.md) tevékenységet, amely a [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység access egyetlen [Tároló Azure](https://azure.microsoft.com/services/storage/) blob.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenységet, amely felhívja az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) kísérletezés, amelynek eredménye az eredmények egyetlen [Tároló Azure](https://azure.microsoft.com/services/storage/) blob alatt helyezi el.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

Ez a [folyamat](../data-factory/data-factory-create-pipelines.md) egy tevékenység - egy [példány](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenységet, amely áthelyezi az [Azure gépi tanulási](#azure-machine-learning) eredményeinek kísérletezés a ***MLScoringPipeline*** az [Azure SQL-adatbázissal](https://azure.microsoft.com/services/sql-database/) , a megoldást sablon telepítés részeként kiépített tartalmazza.

### <a name="azure-machine-learning"></a>Azure gépi tanulási

A megoldás sablon használt [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) kísérlet a hátralévő hasznos élettartama (Szabályainak) repülőgép motor biztosít. A kísérlet az elfogyasztott mennyiség adatkészlet szerinti, és ezért módosítása vagy helyettesítő állapotba kerül az adatok adott szükséges.

Az Azure gépi tanulási kísérletet létrehozási módjától kapcsolatos további tudnivalókért lásd [prediktív karbantartása: lépés 1, 3, adatok előkészítése és szolgáltatás műszaki](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Monitor folyamatban**
 Az adatok nyilvántartás-készítő alkalmazás elindul, amint a folyamat első hidratált kezdődik, és a megoldás különböző összetevői, indítsa el a parancsok az adatok gyári által kibocsátott művelet következő be kicking. Kétféleképpen figyelheti a folyamat.

1. A bejövő nyers adatokból egy, a megjelenítő Értékáram-elemzés feladat ír blob-tárolóhoz. Elemre a Blob-tárolóhoz összetevő a megoldás a képernyőn, sikeresen a megoldást üzembe, és kattintson a jobb oldali panelen a Megnyitás gombra, megnyílik az [adatkezelési portál](https://portal.azure.com/). Egyszer megnyitva, kattintson a BLOB. A következő panelen láthatja a tárolók listáját. Kattintson a **maintenancesadata**. A következő panelen látni fogja a **rawdata** mappát. A rawdata mappában, látni fogja a mappák óra például nevek = 17 óra = a 18 stb. Ezek a mappák jelenik meg, ha azt jelzi, hogy a nyers adatokból sikeresen létrehozott a számítógépén és a blob-tárolóban lévő. Látnia kell rendelkeznie a függvényt méretű MB azokat a mappákat a csv-fájlok.

2. A folyamat utolsó lépésként adatok (például a gépi tanulási előrejelzések) írja be az SQL-adatbázishoz. Előfordulhat, hogy kell várakozniuk legfeljebb három órát, az adatok jelennek meg az SQL-adatbázisban. Egy Lync, hogy mennyi adatot érhető el az SQL-adatbázis módja [azure portálon](https://manage.windowsazure.com/)keresztül. Keresse meg a bal oldali ablaktáblában a SQL-ADATBÁZISAIT ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) , és kattintson rá. Ezután keresse meg az adatbázis **pmaintenancedb** , és kattintson rá. A következő lapon a képernyő alján kattintson a kezelése

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Ebben az esetben kattinthat, új lekérdezést, és a lekérdezés (például a PMResult választó count(*)) sorok számát. Az adatbázis növekedésével a tábla sorainak számát növelni kell.


## <a name="power-bi-dashboard"></a>**A Power BI-irányítópult**

### <a name="overview"></a>– Áttekintés

Ez a szakasz ismerteti, hogyan állíthatja be a Power BI-irányítópult Azure adatfolyam analytics (meleg elérési út), a valós idejű adatok ábrázolásához és köteg előrejelzési származó találatok jelenjenek meg Azure gépi tanulási (hideg elérési út).

### <a name="setup-cold-path-dashboard"></a>A telepítő hideg elérési út irányítópult

A hideg elérési út adatok során az alapvető célja beszerzése minden repülőgép motor prediktív Szabályainak (fennmaradó leírási idő), miután egy repülőút (ciklus) fejeződjön. Az előrejelzés eredménye, amely során az elmúlt 3 óra egy repülőút befejezése után repülőgép motorok előrejelzésére 3 óránként frissül.

A Power BI adatforrásként, az előrejelzési eredmények tároló Azure SQL-adatbázishoz csatlakozik. Megjegyzés: 1), a megoldást üzembe helyezné, valós előrejelzése megjelenik az adatbázis 3 órán belül.
A volt a nyilvántartás-készítő alkalmazás letölthető az pbix fájl tartalmaz néhány kiindulási adatok, így azonnal hozhatja létre, a Power BI-irányítópult. 2) ebben a lépésben a előfeltétel, hogy töltse le és telepítse az ingyenes szoftvert [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Az alábbi lépésekkel végigvezeti a csatlakozás a pbix fájlt a megoldást üzembe (*pl.*adatokat tartalmazó időben fel lett is SQL-adatbázishoz. szövegbevitel eredményei) megjelenítő elem.

1.  Az adatbázis hitelesítő adatok beszerzése.

    **Adatbázis-kiszolgáló nevét, az adatbázis neve, felhasználónév és jelszó** lesz szüksége áthelyezése a következő lépésekkel. Az alábbiakban segítséget nyújtanak keresheti meg azokat a lépéseket.

    -   Miután **"Azure SQL-adatbázis"** a megoldás sablon diagramon zöldre, kattintson rá, és válassza a **"Megnyitás"**.

    -   Megjelenik egy új lapon/böngészőablakban, amely megjeleníti az Azure portál lapot. Kattintson a bal oldali ablaktáblában kattintson a **"Erőforrás csoportok"** hivatkozásra.

    -   Jelölje ki azt az előfizetést, használata esetén a megoldást üzembe helyezné, és válassza a **"YourSolutionName\_ResourceGroup"**.

    -   Kattintson az új előugró panelen, a ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) ikonra a hozzáférést az adatbázishoz. Az adatbázis neve mellett a rendszer ikonra (*például* **"pmaintenancedb"**) és az **adatbázis-kiszolgáló neve** alatt a Server name tulajdonság és **YourSoutionName.database.windows.net**hasonlóan kell kinéznie.

    -   A adatbázis **felhasználónevével** és **jelszavával** ugyanazok, mint a felhasználónév és jelszó korábban rögzített során a megoldás üzembe helyezése.

2.  Jelentés fájl hideg elérési út az adatforrás frissítése a Power BI Desktop.

    -   A mappát, ahol letöltött és a nyilvántartás-készítő alkalmazás fájl kibontott PC-n, kattintson duplán a **PowerBI\\PredictiveMaintenanceAerospace.pbix** fájlt. Ha a fájl megnyitásakor megjelenik a figyelmeztető üzeneteket, hagyja figyelmen kívül őket. A fájl tetején lévő kattintson a **Lekérdezés szerkesztése**gombra.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Két tábla **RemainingUsefulLife** és **PMResult**talál. Jelölje ki az első táblában, és kattintson a ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) **"Forrás"** a **"lekérdezés-beállítások"** jobb oldali ablaktábla **Alkalmazott lépések** csoportban elem mellett. Figyelmen kívül hagyása megjelenő figyelmeztető üzeneteket.

    -   Előugró ablak **"Server"** és **"Adatbázis"** cseréje saját kiszolgáló és az adatbázis nevét, és válassza a **"OK"**. Kiszolgáló nevét győződjön meg arról, adja meg, hogy a port 1433 (**YourSoutionName.database.windows.net, 1433**). Hagyja a Database mező **pmaintenancedb**. Figyelmen kívül hagyja a figyelmeztető üzenetek jelennek meg a képernyőn.

    -   A következő előugró ablak látni fogja a két lehetőség a bal oldali ablaktáblában (a**Windows** és az **adatbázis**). Kattintson az **"Adatbázis"**gombra, írja be a **"Felhasználónév"** és **"Jelszó"** (Ez a felhasználónév és jelszó, amikor először a megoldást üzembe és Azure SQL-adatbázishoz létrehozott megadott). A ***Válassza ki a melyik szintet ezeket a beállításokat szeretne alkalmazni***jelölje be az adatbázis szintű lehetőséget. Kattintson a **"Csatlakozás"**.

    -   Kattintson a második tábla **PMResult** parancsára, majd kattintson az ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) **"Forrás"** a **"lekérdezés-beállítások"** jobb oldali ablaktábla **Alkalmazott lépések** csoportban elem mellett, és frissítse a kiszolgáló és az adatbázis nevét, ahogy a fenti lépéseket, és kattintson az OK gombra.

    -   Miután esetén folyamatát, az előző oldalra, zárja be az ablakot. Üzenet jelenik meg – kattintson az **Alkalmaz**gombra. Végül pedig kattintson a **Mentés** gombra a módosítások mentéséhez. A Power BI-fájl most már létrehozott kapcsolat a kiszolgálóval. Ha a képi üresek, ellenőrizze, törölje a jelet a megjelenítése a jelmagyarázatok jobb felső sarkában lévő radír ikonra kattintva jelenítse meg az adatokat a tartalmat. A frissítés gomb segítségével új adatokat jelenítenek meg a képi megjelenítések. Első lépésként csak látni fogja a kiindulási adatok a képi, az adatok gyári 3 óránként frissítésekor van ütemezve. 3 óra elteltével megjelenjenek a képi az adatok frissítésekor új előrejelzések jelenik meg.

3.  (Nem kötelező) Tegye közzé a hideg elérési út irányítópult [A Power BI online](http://www.powerbi.com/). Megjegyzés: Ez a lépés van szüksége a Power BI fiók (vagy az Office 365-fiókjába).

    -   Kattintson a **"Közzététel"** gombra, és néhány másodperc múlva megjelenik egy ablak megjelenítése a "Közzététel a Power BI sikeres!" a zöld pipára. Kattintson a "Megnyitás PredictiveMaintenanceAerospace.pbix a Power BI"kifejezésre alatti hivatkozásra. Részletes útmutatást, olvassa el [a Power BI Desktop közzététel](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Új irányítópult létrehozása: kattintson a **+** melletti az **irányítópultok** szakasz a bal oldali ablaktáblában. Írja be a nevét az új irányítópult "Prediktív karbantartási bemutató".

    -   Amikor a jelentés megnyitásához kattintson a ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) rögzítése az irányítópultra képi megjelenítések. Részletes útmutatást, olvassa el [a Power BI-irányítópult jelentésekből egy Csempét](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Lépjen az irányítópult lapra, és módosítsa a méret és a képi helyét, és a címben szerkesztése. Részletes útmutatást a csempék szerkesztési módját, olvassa el [Szerkesztés mozaik – átméretezése, áthelyezése, nevezze át, a PIN-kód, törölni szeretne, adja hozzá hivatkozást](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Az alábbiakban néhány hideg elérési út megjelenítésekkel azt a kiemelt egy minta irányítópulton.  Attól függően, hogy meddig futtat a adatok nyilvántartás-készítő alkalmazás a számokat az megjelenítése a eltérő lehet.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Az adatok adatfrissítési ütemezés, hogy mutasson az egérrel a **PredictiveMaintenanceAerospace** adatkészlet, kattintson a ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) , és válassza a **Frissítési ütemezés**.
<br/>
        **Megjegyzés:** Ha megjelenik egy figyelmeztetés masszírozó, kattintson a **Szerkesztés hitelesítő adatok** , és győződjön meg arról, hogy az adatbázis hitelesítő adatai ugyanazok, mint az 1 leírtakhoz.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   A **Frissítési ütemezés** szakasz kibontásához. Kapcsolja be "szeretné tartani az adatokat naprakész".
    <br/>
    -   A frissítés szükségletek ütemezése. További információt, olvassa el [a Power BI Adatfrissítés](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>A telepítő meleg elérési út irányítópult

Az alábbi lépésekkel végigvezeti Önt miként jelenítse meg a megoldást üzembe idején előállított Értékáram-elemzés feladatok valós idejű adatok kimenetét. A [Power BI online](http://www.powerbi.com/) fiók szükséges hajtsa végre az alábbi lépéseket. Ha nem rendelkeznek fiókkal, akkor [Hozzon létre egy újat](https://powerbi.microsoft.com/pricing).

1.  Vegye fel a Power BI kimeneti Azure adatfolyam Analytics (ASA).

    -  Kövesse a kell [Azure Értékáram-elemzés és a Power BI: A valós idejű statisztika irányítópultján a valós idejű adatok streaming láthatóságát](stream-analytics-power-bi-dashboard.md) állíthatja be a Power BI-irányítópult, a megjelenítő Értékáram-elemzés Azure feladatok kimenetét.
    - A ASA lekérdezés három kimeneti értékeket, amelyek **aircraftmonitor** **aircraftalert**és **flightsbyhour**tartalmaz. A lekérdezés lapon kattintson a lekérdezés tekinthet meg. Az alábbi táblázat minden egyes megfelelő, szüksége lesz egy kimenet hozzáadása ASA. Az első kimeneti (*pl.* **aircraftmonitor**) hozzáadásakor ellenőrizze, hogy a **Kimeneti Alias**, az **Adatkészlet nevét** és a **Táblázat neve** az azonos (**aircraftmonitor**). Ismételje meg a kimeneti értékeket **aircraftalert**és **flightsbyhour**hozzáadásának lépéseit. Ha mind a három tábla kimeneti, és a ASA feladat lépések hozzáadott, szerezheti be egy megerősítést kérő üzenet (*pl.*:, "Indítása adatfolyam analytics feladat maintenancesa02asapbi sikeresen befejeződött").

2. Jelentkezzen be [A Power BI online](http://www.powerbi.com)

    -   Kattintson a bal oldali ablaktábla saját munkaterületen adatkészleteket szakaszban az ***ADATKÉSZLET*** neve **aircraftmonitor** **aircraftalert**és **flightsbyhour** jelenjenek meg. Ez a az előző lépésben az Azure Értékáram-elemzés tolni adatfolyam. Az adatkészlet **flightsbyhour** nem lehet jelenik meg a másik két adatkészleteket miatt az SQL-lekérdezés mögötte jellegét egyszerre. Jó helyen jár hogy jelenjen meg után egy órával.
    -   Győződjön meg arról, hogy a ***képi megjelenítések*** ablak meg nyitva, és a képernyő jobb oldalán látható.

3. Ha befejezte a Power BI szét adatokat, indítsa el az adatfolyam adatok megjelenítésére. Alatt egy minta irányítópulton néhány meleg elérési út megjelenítésekkel rögzítve van rá. Más irányítópult-csempék a megfelelő adatkészleteket alapján hozhat létre. Attól függően, hogy meddig futtat a adatok nyilvántartás-készítő alkalmazás a számokat az megjelenítése a eltérő lehet.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Az alábbiakban néhány olyan lépés hozhat létre a fenti csempék közül – a "flotta megtekintése a érzékelő 11 összehasonlítása küszöb 48.26" csempe:

    -   Kattintson az adatkészlet **aircraftmonitor** kattintson a bal oldali ablaktáblában adatkészleteket szakaszban.

    -   Kattintson a **Vonaldiagram** ikonra.

    -   **Feldolgozott** ablaktáblájában kattintson a **mezők** , hogy a **képi megjelenítések** ablakban a "Tengely" mutatja.

    -   Kattintson a "s11" és "s11\_értesítés", hogy mindkettő "Értékek" csoportban jelenik meg. **S11** melletti kis nyílra kattintson, és **s11\_értesítés**, "Összeg" módosítása "Átlag".

    -   A képernyő tetején kattintson a **MENTÉS** gombra, és nevezze el a jelentést "aircraftmonitor". A "aircraftmonitor" nevű jelentés **jelentések** szakaszában a **kezelő** ablaktáblában, a bal oldalon jelenik meg.

    -   Kattintson a vonaldiagram jobb felső sarkában a **PIN-kód vizuális** ikonra. Egy "PIN-kód irányítópult" ablakban a is választhat egy irányítópult jelenik meg. Jelölje be a "Prediktív karbantartási bemutató", majd kattintson a "PIN-kód".

    -   Ez az irányítópulton lévő csempén mutasson az egérrel, és kattintson a jobb felső sarokban a cím "Flotta megtekintése a érzékelő 11 összehasonlítása küszöb 48.26" és "Átlag közötti időbeli flotta" felirat módosítása "Szerkesztés" ikonjára.

## <a name="how-to-delete-your-solution"></a>**A megoldás törlése**
Győződjön meg arról, hogy ha a nem aktív a megoldást, az adatok nyilvántartás-készítő alkalmazás futó merülnek fel, amelyekre magasabb költségek leállítása az adatok nyilvántartás-készítő alkalmazás. Ha már nem használja, törölje a megoldást. A megoldás fog törlésekor minden, a kiépítéstől az előfizetésben, amikor telepítette a megoldást összetevőket. Kattintson a megoldás gombra a saját megoldás nevére a bal oldali ablaktáblában a megoldást sablon törlése, és kattintson a Törlés gombra.

## <a name="cost-estimation-tools"></a>**Költségbecslés eszközök**

A következő két eszközök állnak rendelkezésre segít jobban megértheti az érintett fut a cserélendő karbantartási űrhajózási megoldás sablon az előfizetés a teljes költsége:

-   [Microsoft Azure költség becslés eszköz (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure költség becslés eszköz (asztali)](http://www.microsoft.com/download/details.aspx?id=43376)
