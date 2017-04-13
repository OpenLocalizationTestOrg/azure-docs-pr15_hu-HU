<properties
    pageTitle="Értékesítési előrejelzés Energy technikai útmutató |} Microsoft Azure"
    description="Igény szerint előrejelzés energy a technikai útmutató Microsoft Cortana intelligenciával a megoldást sablonba."
    services="cortana-analytics"
    documentationCenter=""
    authors="yijichen"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="inqiu;yijichen;ilanr9"/>

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>A Cortana üzletiintelligencia megoldás sablont igény szerinti előrejelzés energy a technikai útmutató

## <a name="overview"></a>**– Áttekintés**

Megoldás sablonok célja, hogy felgyorsítsa a folyamat egy E2E bemutató fölött Cortana üzletiintelligencia-csomagja tudnivalóit. A telepített sablon kiépítése, szükséges Cortana üzletiintelligencia-összetevő-előfizetése, és a közötti kapcsolatok létrehozása. Azt is osztja fel a az adatok folyamat első generált szimulációk alkalmazásból mintaadatokkal. Hivatkozást követve töltse le a adatok simulatort használja, és telepítse a helyi számítógép zónában, a további tudnivalók a simulatort használja a readme.txt fájlra. A simulatort használja a létrehozott adatokat fog hydrate, az adatok folyamat és a kezdő gépi tanulási előrejelzési szolgáltatása, amely majd megjeleníthetők a Power BI-irányítópulton létrehozása.

A megoldás sablon megtalálható [Itt](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1) 

A telepítési folyamatot végigvezeti Önt több lépésben állíthatja be a megoldást hitelesítő adatait. Ellenőrizze, hogy a hitelesítő adatokat, például megoldás neve, felhasználónevét és jelszavát a telepítés során megadott rögzítése.

A dokumentum a cél, a hivatkozás architektúra és más összetevőket az előfizetése részeként ezzel a sablonnal megoldás kiépítése ismertetik. A dokumentum is folytatott beszélgetést, a mintaadatok lecserélése saját Hírcsatornájában/előrejelzések adatok nyert Öntől láthatja a tényleges adatokat olvashat. Emellett a dokumentum megbeszélések módosítani kell, ha testre szeretné szabni a megoldást a saját adatain kellene megoldás sablon részek kapcsolatban. Hogyan lehet a Power BI-irányítópult megoldás sablon utasításokat végén.

## <a name="big-picture"></a>**Áttekintés**

![](media\cortana-analytics-technical-guide-demand-forecast\ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architektúra magyarázata
Ha telepíti a megoldást, Cortana Analytics csomagja belül különböző Azure szolgáltatások aktiválódnak (*azaz* esemény-központban Értékáram-elemzés, HDInsight, adatok gyári, gépi tanulási, *stb.*). A architektúra fenti ábrán, magas szintű, hogyan igény szerinti előrejelzés Energy megoldás sablon összeállítás a végpontok közötti. Az alábbi szolgáltatások kivizsgáljuk parancsával a megoldás-sablon a megoldás üzembe helyezése a létrehozott diagram tudja. Az alábbi szakaszok ismertetik minden darab.

## <a name="data-source-and-ingestion"></a>**Adatforrás és bevitel**

### <a name="synthetic-data-source"></a>Szintetikus adatforrás

Ez a sablon használt adatforrás egy asztali alkalmazás, amely, töltse le és helyben futtatni sikeres telepítés után az jön létre. Az utasításokat követve töltse le és telepítse az alkalmazást a Tulajdonságok sávon, akkor jelölje be az első csomópont nevű Energy előrejelzés adatok simulatort használja a megoldást sablon diagram találja. Ez az alkalmazás az [Azure esemény központi](#azure-event-hub) szolgáltatás adatpontok vagy események, a többi a megoldást folyamat használt hírcsatornák.

Az esemény generációs alkalmazás fog feltöltése az Azure-esemény központban, csak, amíg fut, a számítógépen.

### <a name="azure-event-hub"></a>Azure esemény központi

Az [Azure esemény központi](https://azure.microsoft.com/services/event-hubs/) szolgáltatás érhető el a címzett, a bemeneti a fentebb ismertetett szintetikus adatforrás által biztosított.

## <a name="data-preparation-and-analysis"></a>**Adatok előkészítésével és elemzése**


### <a name="azure-stream-analytics"></a>Azure Értékáram-elemzés

Adja meg a bemeneti adatfolyam az [Azure esemény központi](#azure-event-hub) szolgáltatásból a valós idejű elemző közelében és -közzététel eredménye egy [Power BI](https://powerbi.microsoft.com) -irányítópult, valamint a archiválására az [Azure](https://azure.microsoft.com/services/storage/) tárhelyszolgáltatáshoz [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás későbbi feldolgozásra összes nyers bejövő esemény alakzatot az [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás szolgál.

### <a name="hd-insights-custom-aggregation"></a>HD-minőségű háttérismeretek egyéni aggregáció

Az Azure HD betekintést szolgáltatást biztosít összesítések archiválás az Azure Értékáram-elemzés szolgáltatással történt nyers eseményeket [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok (Azure Data Factory által orchestrated) futtatásához használt.

### <a name="azure-machine-learning"></a>Azure gépi tanulási

Az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) szolgáltatást használják: (Azure Data Factory által orchestrated) is létrehozhat egy adott régió, a kapott bemenetben adott jövőbeli energiafogyasztást előrejelzési.

## <a name="data-publishing"></a>**Adatok közzététele**


### <a name="azure-sql-database-service"></a>Azure SQL-adatbázis-szolgáltatás

Az [SQL Azure-adatbázis](https://azure.microsoft.com/services/sql-database/) szolgáltatást használják: tárolásához (Azure Data Factory kezeli) az előrejelzések megkapta az Azure gépi tanulási szolgáltatás, amely a [Power BI](https://powerbi.microsoft.com) irányítópulton fogyni fog.

## <a name="data-consumption"></a>**Adatok fogyasztása**

### <a name="power-bi"></a>A Power BI

A [Power BI](https://powerbi.microsoft.com) szolgáltatás, amely tartalmazza az összesítések, amennyiben az [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/) szolgáltatást, valamint az igény szerinti előrejelzés eredménye [Azure SQL](https://azure.microsoft.com/services/sql-database/) -adatbázisban tárolt, amelyeket az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) szolgáltatással egy irányítópult megjelenítése szolgál. Hogyan lehet a Power BI-irányítópult megoldás sablon útmutatásért olvassa el az alábbi szakaszt.

## <a name="how-to-bring-in-your-own-data"></a>**Hogyan kell a saját adatain hozása**

Ez a szakasz ismerteti a saját adatain előtérbe Azure, és mely területeken igényli szeretné a módosításokat, a architektúra összhangba adatokhoz.

Valószínű bármely hozása adatkészlet volna felel meg az adatkészlet megoldás sablon használható. Az adatok és a követelmények ismertetése a sablont a saját adatokkal végzett munkához módosíthatja a kulcsfontosságú lesz. Ha ez az első információk megjelenítése a Azure gépi tanulási szolgáltatás, a példa használatával [hogyan hozhat létre az első kísérlet](machine-learning\machine-learning-create-experiment.md)érheti egy bemutatása.

Az alábbi szakaszok bemutatja, hogyan lehet a szakaszok módosítások esetén kell, amikor egy új adatkészletet bevezetett sablon.

### <a name="azure-event-hub"></a>Azure esemény központi

Az [Azure esemény központi](https://azure.microsoft.com/services/event-hubs/) szolgáltatása nagyon általános, hogy az adatok adhatók fel a központi CSV- vagy JSON formátumban. Speciális feldolgozás nem jelentkezik az Azure-esemény központban, de a fontos, hogy megismeri a mikrofonba dokumentum adatokat.

A dokumentum nem ismertetik, hogyan lehet az adatok ingest, de az egyik egyszerűen elküldhetik a események és az adatok az Azure esemény hubon keresztül csatlakozott, a [Esemény központi API -val](event-hubs\event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Értékáram-elemzés

Valós idejű analytics közelében adatfolyamok olvasása és írása az adatokat tetszőleges számú adatforrások nyújtása a [Értékáram-elemzés Azure](https://azure.microsoft.com/services/stream-analytics/) szolgáltatás használják.

Az az igény szerinti előrejelzéshez Energy megoldás sablon az Azure Értékáram-elemzés lekérdezés mindegyik használata más ráfordítások az Azure esemény központi szolgáltatásból események és a kimeneti értékeket, hogy két különböző helyeken két alszint lekérdezések áll. Ezek a kimeneti értékeket egy Power BI adatkészlet és egy Azure tárolási helye állnak.

Az [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/) lekérdezés által találhatók:

-   Bejelentkezés az [Azure kezelőportálja segítségével](https://manage.windowsazure.com/)

-   Az adatfolyamban analytics feladatok megkeresése ![](media\cortana-analytics-technical-guide-demand-forecast\icon-stream-analytics.png) telepítették a megoldást, amely jött létre. Egyik az adatok blob-tároló (pl. mytest1streaming432822asablob) küldése, a másikkal az adatok küldése a Power bi (pl. mytest1streaming432822asapbi).


-   Kijelölése

    -   Megtekintheti a lekérdezés bemeneti a ***bemeneti adatok alapján***

    -   ***Lekérdezés*** megtekintéséhez magát a lekérdezést

    -   ***EXPORTÁLJA*** a különböző kimeneti értékeket megtekintése

Azure Értékáram-elemzés lekérdezés építés információt a [Adatfolyam Analytics lekérdezés hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx) MSDN webhelyen található.

Ez a megoldás a az Azure Értékáram-elemzés feldolgozást, amelyet az adatkészlet a valós idejű analytics információt a Power BI-irányítópult a bejövő adatfolyam közelében exportálja a megoldást sablon részeként kapja. Mivel a bejövő adatformátum implicit ismereteket, ezek a lekérdezések kellene lehet módosítani a adatformátum alapján.

Az Azure Értékáram-elemzés feladat exportálja az összes [Esemény központi](https://azure.microsoft.com/services/event-hubs/) események [Azure](https://azure.microsoft.com/services/storage/) -tárolóhoz, és így, a teljes adatai folyamatosan lejátszott tárolóhoz nincs módosítás függetlenül az adatformátum van szükség.

### <a name="azure-data-factory"></a>Azure Data Factory

Az [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) szolgáltatás orchestrates mozgását és adatok kezelése. Az igény szerinti előrejelzésére Energy megoldás sablon adatok gyári tevődnek össze tizenkét [folyamatok](data-factory\data-factory-create-pipelines.md) áthelyezése és különböző technológiákkal adatfeldolgozás.

  Az adatok gyári úgy, hogy megnyitja a Data Factory csomópontot a megoldás-sablon a megoldás üzembe helyezése a létrehozott diagram aljánál érheti el. Ezzel eljut az adatok gyári az Azure adatkezelési portálra. Hibák a adatkészleteket csoportban jelenik meg, ha figyelmen kívül hagyhatja azokat, mint üzembe helyezéséhez az adatok nyilvántartás-készítő alkalmazás elindítása előtt adatok gyári miatt. Ezek a hibák akadályozza meg, hogy az adatok gyári működik-e.

Ez a szakasz ismerteti, hogy a szükséges [folyamatok](data-factory\data-factory-create-pipelines.md) és a [tevékenységek](data-factory\data-factory-create-pipelines.md) az [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)található. Az alábbi van a diagram nézetben a megoldás.

![](media\cortana-analytics-technical-guide-demand-forecast\ADF2.png)

Az öt a folyamatok, ez a gyár tartalmaz [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsprogramok partition és az adatok összesítése szolgálnak. Jegyezni, ha a parancsfájlok található a telepítés során létre [Azure tároló](https://azure.microsoft.com/services/storage/) fiók. Helyükre lesz: demandforecasting\\\\parancsfájl\\\\struktúra\\ \\ (vagy https://[Your megoldás name].blob.core.windows.net/demandforecasting).

Hasonló az [Azure Értékáram-elemzés](#azure-stream-analytics-1) lekérdezések, a [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájlok a bejövő adatformátum implicit ismereteket, ezek a lekérdezések volna kell változtatható meg az adatok formázása és a [szolgáltatás műszaki](machine-learning\machine-learning-feature-selection-and-engineering.md) követelmények alapján.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*

A [folyamat](data-factory\data-factory-create-pipelines.md) folyamat tartalmaz egy egyetlen tevékenységet – használatával, amely a 10 másodpercenként összesítése [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl fut [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) [HDInsightHive](data-factory\data-factory-hive-activity.md) tevékenység az igény szerinti adatok állomás szintbe óránként régió szintre folyamatosan lejátszott és [Azure-tárolóban](https://azure.microsoft.com/services/storage/) lévő veti az Azure Értékáram-elemzés feladat.

Ehhez a feladathoz particionáló [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl ***AggregateDemandRegion1Hr.hql***


#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*

Ez a [folyamat](data-factory\data-factory-create-pipelines.md) a két tevékenység tartalmazza:
- [HDInsightHive](data-factory\data-factory-hive-activity.md) tevékenység [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) futtató állomás szintet úgy, hogy óránkénti terület szint óránkénti előzmények igény szerinti adatainak összesítése és Azure tároló helyezi el a Azure Értékáram-elemzés projekt során struktúra parancsfájl használatával

- [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység helyezi át az összesített adatok tárolására Azure blob az Azure SQL-adatbázissal, a megoldást sablon telepítés részeként kiépített.

Ehhez a feladathoz [struktúra](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) parancsfájl ***AggregateDemandHistoryRegion.hql***.


#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*

Ezek a [folyamatok](data-factory\data-factory-create-pipelines.md) több tevékenységet tartalmaz, és amelynek a végeredménye a scored előrejelzések az Azure gépi tanulási kísérlet megoldás sablonhoz társított. Azonosak lesznek szinte azzal a különbséggel mindegyiket a más területére, amely az egyes régiókra vonatkozó a ADF folyamat és a struktúra parancsfájl átadott különböző RegionID végzett csak kezeli.  
A tevékenységek található ez a következők:
-   [HDInsightHive](data-factory\data-factory-hive-activity.md) tevékenység összesítések és az Azure gépi tanulási kísérletet szükséges szolgáltatás műszaki végrehajtásához struktúra parancsfájl futó [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) használatával. A struktúra parancsfájlok a tevékenységhez tartozó megfelelő ***PrepareMLInputRegionX.hql***.

-   [Másolás](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenység helyezi át az eredményeket a [HDInsightHive](data-factory\data-factory-hive-activity.md) tevékenységet, amely a [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység access egyetlen tároló Azure blob.

-   Az Azure gépi tanulási meghívó [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tevékenység kísérletezhet az eredmények egyetlen Azure-tárolóban lévő üzembe eredményező blob.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Ezek a [folyamatok](data-factory\data-factory-create-pipelines.md) egyetlen tevékenység - egy [másolatot](https://msdn.microsoft.com/library/azure/dn835035.aspx) a tevékenység helyezi át az Azure gépi tanulási kísérlet eredményének a megfelelő ***MLScoringRegionXPipeline*** az Azure SQL-adatbázissal, a megoldást sablon telepítés részeként kiépített tartalmazzák.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
A [folyamatok](data-factory\data-factory-create-pipelines.md) egyetlen tevékenység - egy [másolatot](https://msdn.microsoft.com/library/azure/dn835035.aspx) a tevékenység helyezi át az összesített folyamatban lévő igény szerinti adatok ***LoadHistoryDemandDataPipeline*** az Azure SQL-adatbázissal, a megoldást sablon telepítés részeként kiépített tartalmazzák.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Ezek a [folyamatok](data-factory\data-factory-create-pipelines.md) egyetlen tevékenység - egy [példány](https://msdn.microsoft.com/library/azure/dn835035.aspx) tevékenységet, amely áthelyezi a hivatkozási adatok a régió/állomás/Topologygeo a megoldás sablon telepítése az Azure SQL-adatbázishoz, amely a megoldást sablon telepítés részeként kiépített részeként tároló Azure blob feltöltött tartalmazzák.

### <a name="azure-machine-learning"></a>Azure gépi tanulási
Az [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) kísérletet a megoldást sablonhoz használni az igény szerinti régió előrejelzése tartalmaz. A kísérlet az elfogyasztott mennyiség adatkészlet szerinti, és ezért módosítása vagy helyettesítő állapotba kerül az adatok adott szükséges.

## <a name="monitor-progress"></a>**Monitor folyamatban**
Az adatok nyilvántartás-készítő alkalmazás elindul, amint a folyamat első hidratált kezdődik, és a megoldás különböző összetevői, indítsa el a parancsok az adatok gyári által kibocsátott művelet következő be kicking. Kétféleképpen figyelheti a folyamat.

1. Jelölje be az adatokat az Azure Blob-tárolóhoz.

    A bejövő nyers adatokból egy, a megjelenítő Értékáram-elemzés feladat ír blob-tárolóhoz. Elemre a **Azure Blob-tárolóhoz** összetevő a megoldás a képernyőn, sikeresen a megoldást üzembe és majd kattintson a **Megnyitás** gombra a jobb oldali panelen, megnyílik az [Azure kezelőportálja segítségével](https://portal.azure.com). Egyszer megnyitva, kattintson a **BLOB**. A következő panelen láthatja a tárolók listáját. Kattintson a **"energysadata"**. A következő panelen látni fogja a **"demandongoing"** mappát. A rawdata mappában, látni fogja a mappák, például dátum nevek = 2016-01-28 stb. Ezek a mappák jelenik meg, ha azt jelzi, hogy a nyers adatokból sikeresen létrehozott a számítógépén és a blob-tárolóban lévő. Meg kell jelennie, amelyeknek a függvényt méretű MB azokat a mappákat a fájlokat.

2. Azure SQL-adatbázis adatainak ellenőrzése

    A folyamat utolsó lépésként adatok (például a gépi tanulási előrejelzések) írja be az SQL-adatbázishoz. Előfordulhat, hogy kell várakozniuk legfeljebb 2 órán az adatok jelennek meg az SQL-adatbázisban. Egy Lync, hogy mennyi adatot érhető el az SQL-adatbázis módja [Azure kezelőportálja](https://manage.windowsazure.com/)segítségével. Keresse meg a bal oldali ablaktáblában a SQL-ADATBÁZISAIT![](media\cortana-analytics-technical-guide-demand-forecast\SQLicon2.png) , és kattintson rá. Ezután keresse meg az adatbázis (azaz demo123456db), és kattintson rá. **"Az adatbázis csatlakozás"** szakaszt a következő lapon kattintson a **"Az SQL-adatbázis Transact-SQL nyelvben Futtatás lekérdezéseket"**.

    Itt választhatja ki, új lekérdezést, és a lekérdezés (például "select count(*) a DemandRealHourly) sorok számát a" az adatbázis növekedésével a tábla sorainak számát növelni kell.)

3. Jelölje be a Power BI-irányítópult adatait.

    Beállíthatja a Power BI meleg elérési irányítópult figyelése a bejövő nyers adatokból. Kövesse a "Irányítópult a Power BI" szakasz a utasítást.



## <a name="power-bi-dashboard"></a>**A Power BI-irányítópult**

### <a name="overview"></a>– Áttekintés

Ez a szakasz ismerteti, hogyan állíthatja be a Power BI-irányítópult az Azure Értékáram-elemzés (meleg elérési út) valós idejű adatok ábrázolása, valamint az előrejelzési Azure gépi tanulási (hideg elérési út) származó találatok jelenjenek meg.


### <a name="setup-hot-path-dashboard"></a>A telepítő meleg elérési út irányítópult

Az alábbi lépésekkel végigvezeti Önt miként jelenítse meg a megoldást üzembe idején előállított Értékáram-elemzés feladatok valós idejű adatok kimenetét. A [Power BI online](http://www.powerbi.com/) fiók szükséges hajtsa végre az alábbi lépéseket. Ha nem rendelkeznek fiókkal, akkor [Hozzon létre egy újat](https://powerbi.microsoft.com/pricing).

1.  Vegye fel a Power BI kimeneti Azure adatfolyam Analytics (ASA).

    -  Kövesse a kell [Azure Értékáram-elemzés és a Power BI: A valós idejű statisztika irányítópultján a valós idejű adatok streaming láthatóságát](stream-analytics-power-bi-dashboard.md) állíthatja be a Power BI-irányítópult, a megjelenítő Értékáram-elemzés Azure feladatok kimenetét.

    - Keresse meg az adatfolyam analytics feladat az [Azure kezelőportálja segítségével](https://manage.windowsazure.com). A feladat nevét kell lennie: YourSolutionName + streamingjob"" + véletlen szám + "asapbi" (azaz demostreamingjob123456asapbi).

    - Adja hozzá a PowerBI kimeneti a ASA projektre vonatkozóan. A **Kimenet Alias** értéke lehet **"PBIoutput"**. Beállíthatja az **Adatkészlet nevét** , és a **táblázat neve** **"EnergyStreamData"**. Miután hozzáadta a kimenet, kattintson a **"Start"** a lap alján a Értékáram-elemzés feldolgozás elindításához. Megerősítést kérő üzenet (*pl.*:, "Indítása adatfolyam analytics feladat myteststreamingjob12345asablob sikeresen befejeződött") kell jelenik meg.

2. Jelentkezzen be [A Power BI online](http://www.powerbi.com)

    -   Kattintson a bal oldali ablaktábla saját munkaterületen adatkészleteket szakaszban láthatja a bal oldali ablaktáblában a Power BI egy új adatkészletet kell lennie. Ez a az előző lépésben az Azure Értékáram-elemzés tolni adatfolyam.

    -   Győződjön meg arról, hogy a ***képi megjelenítések*** ablak meg nyitva, és a képernyő jobb oldalán látható.

3. A "Igény szerint időbélyeg" csempe létrehozásához:
    -   Kattintson az adatkészlet **"EnergyStreamData"** , kattintson a bal oldali ablaktáblában adatkészleteket szakaszban.

    -   Kattintson a **"sor"** ikonja ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic8.png).

    -   A **mezők** panelen kattintson a "EnergyStreamData".

    -   Kattintson a **"Időbélyeg"** , és ellenőrizze, hogy a "Tengely" jeleníti meg. Kattintson a **"Betöltés"** gombra, és ellenőrizze, hogy "Értékek" jeleníti meg.

    -   A képernyő tetején kattintson a **MENTÉS** gombra, és a jelentés "EnergyStreamDataReport" nevet. A "EnergyStreamDataReport" nevű jelentés jelentések szakasz bal oldalon a kezelő ablakban fog megjelenni.

    -   Kattintson a **PIN-kód vizuális ""** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) Ez a vonaldiagram jobb felső sarkában lévő ikonra, egy "PIN-kód irányítópult" ablak előfordulhat, hogy jelenik meg az irányítópult választhat. Kérjük, jelölje be a "EnergyStreamDataReport", majd a "rögzítés".

    -   Mutasson az egérrel az irányítópulton lévő csempén alatt kattintson a "Szerkesztés" módosítása a címére, mint "Igény szerint időbélyeg" jobb felső sarokban lévő ikonra

4.  Hozzon létre más irányítópult-csempék a megfelelő adatkészleteket alapján. Az alábbiakban a végleges irányítópult nézetben látható.
        ![](media\cortana-analytics-technical-guide-demand-forecast\PBIFullScreen.png)


### <a name="setup-cold-path-dashboard"></a>A telepítő hideg elérési út irányítópult
Hideg elérési út adatok során az alapvető célja, hogy az egyes régiókra az igény szerinti előrejelzés első. A Power BI adatforrásként, az előrejelzési eredmények tároló Azure SQL-adatbázishoz csatlakozik.

> [AZURE.NOTE] 1) vesz igénybe az irányítópulton elég előrejelzési találatok összegyűjtéséhez néhány órát. Azt javasoljuk, hogy a folyamat 2-3 órát után, az adatok nyilvántartás-készítő alkalmazás delek megkezdése. 2) ebben a lépésben a előfeltétel, hogy töltse le és telepítse az ingyenes szoftvert [Power BI desktop](https://powerbi.microsoft.com/desktop).



1.  Az adatbázis hitelesítő adatok beszerzése.

    **Adatbázis-kiszolgáló nevét, az adatbázis neve, felhasználónév és jelszó** áthelyezése a következő lépések előtt kell. Az alábbiakban segítséget nyújtanak keresheti meg azokat a lépéseket.

    -   Miután a megoldást sablon diagram **"Azure SQL-adatbázis"** zöldre, kattintson rá, és válassza a **"Megnyitás"**. Azure adatkezelési portálra folyamatát, és az adatbázis-adatok lapon nyílik meg, valamint.

    -   A lapon található egy "Adatbázis" című szakaszát. Ki a létrehozott adatbázis sorolja fel. Az adatbázis neve **"A megoldás neve véletlen szám +"db""** (például "mytest12345db") kell lennie.

    -   Az adatbázis, kattintson az új előugró pannel, talál az adatbázis-kiszolgáló nevének a képernyő tetején. Az adatbázis kiszolgáló neve nevet termelőfelhasználásánál legyen **"A megoldás neve véletlen szám +"database.windows.net,1433""** (például "mytest12345.database.windows.net,1433").

    -   A adatbázis **felhasználónevével** és **jelszavával** ugyanazok, mint a felhasználónév és jelszó korábban rögzített során a megoldás üzembe helyezése.

2.  Az adatforrás hideg elérési útját a Power BI-fájl módosítása
    -  Győződjön meg arról, hogy telepítette a [Power BI desktop](https://powerbi.microsoft.com/desktop)legújabb verzióját.

    -   A letöltött **"DemandForecastingDataGeneratorv1.0"** mappát kattintson duplán a **"Power BI Template\DemandForecastPowerBI.pbix"** fájlt. A kezdeti megjelenítések üres adatokon alapuló. **Megjegyzés:** Ha megjelenik egy hiba massage, ellenőrizze, hogy telepítette a Power BI Desktop legújabb verzióját.

        Miután nyit meg, a fájl tetején található, kattintson a **Lekérdezés szerkesztése**lehetőséget. Az előugró ablakban kattintson duplán **"Forrás"** kattintson a jobb oldali ablaktábla.
    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic1.png)

    -   Előugró ablak **"Kiszolgáló"** és az **"Adatbázis"** cseréje saját kiszolgáló és az adatbázis nevét, és válassza a **"OK"**. Kiszolgáló nevét győződjön meg arról, adja meg, hogy a port 1433 (**YourSolutionName.database.windows.net, 1433**). Figyelmen kívül hagyja a figyelmeztető üzenetek jelennek meg a képernyőn.

    -   A következő előugró ablak látni fogja a két lehetőség a bal oldali ablaktáblában (a**Windows** és az **adatbázis**). Kattintson az **"Adatbázis"**gombra, írja be a **"Felhasználónév"** és **"Jelszó"** (Ez a felhasználónév és jelszó, amikor először a megoldást üzembe és Azure SQL-adatbázishoz létrehozott megadott). A ***Válassza ki a melyik szintet ezeket a beállításokat szeretne alkalmazni***jelölje be az adatbázis szintű lehetőséget. Kattintson a **"Csatlakozás"**.

    -   Miután esetén folyamatát, az előző oldalra, zárja be az ablakot. Üzenet jelenik meg – kattintson az **Alkalmaz**gombra. Végül pedig kattintson a **Mentés** gombra a módosítások mentéséhez. A Power BI-fájl most már létrehozott kapcsolat a kiszolgálóval. Ha a képi üresek, ellenőrizze, törölje a jelet a megjelenítése a jelmagyarázatok jobb felső sarkában lévő radír ikonra kattintva jelenítse meg az adatokat a tartalmat. A frissítés gomb segítségével új adatokat jelenítenek meg a képi megjelenítések. Első lépésként csak látni fogja a kiindulási adatok a képi, az adatok gyári 3 óránként frissítésekor van ütemezve. 3 óra elteltével megjelenjenek a képi az adatok frissítésekor új előrejelzések jelenik meg.

3. (Nem kötelező) Tegye közzé a hideg elérési út irányítópult [A Power BI online](http://www.powerbi.com/). Megjegyzés: Ez a lépés van szüksége a Power BI fiók (vagy az Office 365-fiókjába).

    -   Kattintson a **"Közzététel"** gombra, és néhány másodperc múlva megjelenik egy ablak megjelenítése a "Közzététel a Power BI sikeres!" a zöld pipára. "A Power BI megnyitása demoprediction.pbix" alatti hivatkozásra. Részletes útmutatást, olvassa el [a Power BI Desktop közzététel](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Új irányítópult létrehozása: kattintson a **+** melletti az **irányítópultok** szakasz a bal oldali ablaktáblában. Írja be a nevét az új irányítópult "Igény szerint az előrejelzés bemutató".

    -   Amikor a jelentés megnyitásához kattintson a ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) rögzítése az irányítópultra képi megjelenítések. Részletes útmutatást, olvassa el [a Power BI-irányítópult jelentésekből egy Csempét](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
        Lépjen az irányítópult lapra, és módosítsa a méret és a képi helyét, és a címben szerkesztése. Részletes útmutatást a csempék szerkesztési módját, olvassa el [Szerkesztés mozaik – átméretezése, áthelyezése, átnevezés, PIN kód, törölni szeretne, adja hozzá hivatkozást](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Az alábbiakban néhány hideg elérési út megjelenítésekkel azt a kiemelt egy minta irányítópulton.

        ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic7.png)

4. (Nem kötelező) Az adatforrás frissítése ütemezése.
    -     Az adatok adatfrissítési ütemezés, hogy mutasson az egérrel a **Végleges EnergyBPI** adatkészlet, kattintson a ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic3.png) , és válassza a **Frissítési ütemezés**.
    **Megjegyzés:** Ha megjelenik egy figyelmeztetés masszírozó, kattintson a **Szerkesztés hitelesítő adatok** , és győződjön meg arról, hogy az adatbázis hitelesítő adatai ugyanazok, mint az 1 leírtakhoz.

    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic4.png)

    -   A **Frissítési ütemezés** szakasz kibontásához. Kapcsolja be "szeretné tartani az adatokat naprakész".

    -   A frissítés szükségletek ütemezése. További információt, olvassa el [a Power BI Adatfrissítés](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).


## <a name="how-to-delete-your-solution"></a>**A megoldás törlése**
Győződjön meg arról, hogy ha a nem aktív a megoldást, az adatok nyilvántartás-készítő alkalmazás futó merülnek fel, amelyekre magasabb költségek leállítása az adatok nyilvántartás-készítő alkalmazás. Ha már nem használja, törölje a megoldást. A megoldás fog törlésekor minden, a kiépítéstől az előfizetésben, amikor telepítette a megoldást összetevők. Kattintson a megoldás gombra a saját megoldás nevére a bal oldali ablaktáblában a megoldást sablon törlése, és kattintson a Törlés gombra.

## <a name="cost-estimation-tools"></a>**Költség becslés eszközök**

A következő két eszközök állnak rendelkezésre segít jobban megértheti az érintett fut az igény szerinti előrejelzéshez Energy megoldás sablon az előfizetés a teljes költsége:

-   [Microsoft Azure költség becslés eszköz (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure költség becslés eszköz (asztali)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Nyugták**
Ez a cikk adatok tudósok JI Chen és a szoftver adatbázismodellbe Qiu Min, a Microsoft hozta létre.
