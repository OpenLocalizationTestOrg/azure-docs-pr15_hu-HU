<properties     
    pageTitle="Azure adatok Factory - minta" 
    description="Részletes információ szállított minták az Azure Data Factory szolgáltatással." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure adatok Factory - minta

## <a name="samples-on-github"></a>A GitHub minták
A [GitHub Azure-DataFactory tárházba](https://github.com/azure/azure-datafactory) tartalmaz több mintát, amelyek segítségével gyorsan Azure Data Factory szolgáltatással felfelé emelkedő (vagy) módosíthatja a parancsfájlok, és saját alkalmazásban használni. A Samples\JSON mappát a gyakori alkalmazási területek JSON kódrészletek tartalmazza.

| Minta | Leírás |
| :----- | :---------- | 
| [ADF útmutató](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | Ez a példa egy végpont – útmutató nyújt a naplófájlok naplófájlból az adatok mélyebb kapcsolja be az Azure Data Factory használatával feldolgozása. <br/><br/>Az útmutató az adatok gyári folyamat gyűjti össze a minta naplók, folyamatok dúsítja hivatkozás adatokkal naplók adatait, és az adatok, amelyek a legutóbb elindult marketingkampányok hatékonyságát ki szeretné számítani átalakítások. |
| [JSON minták](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | Ez a példa a gyakori alkalmazási területek JSON példákat. | 
| [HTTP-adatok Downloader minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | A minta showcases letöltésének HTTP zárólap Azure Blob-tárolóhoz használatával egyéni .NET tevékenység adatait. |
| [Idegen AppDomain pont nettó tevékenység minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | Ez a példa egy egyéni .NET tevékenységeket, amelyek nem korlátozza a ADF megnyitó ikonra (például WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x stb.) által használt összeállítás-verziók Szerző teszi lehetővé. |
| [R parancsfájl futtatása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  Ez a példa tartalmazza a Data Factory egyéni tevékenység RScript.exe meghívásához használható. Ez a példa csak a saját (nem igény szerinti) HDInsight fürt, amely már rendelkezik R telepítve rajta működik. |
| [A HDInsight Hadoop fürthöz külső feladatok meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | Ez a példa megtudhatja, hogy hogyan MapReduce tevékenység meghívni külső programot. A külső program csak adatokat másolja egy Azure Blob-tárolóból között. |
| [Az Azure gépi tanulási köteg pontozási tevékenység Twitter elemző](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | Ez a példa bemutatja, hogyan AzureMLBatchScoringActivity segítségével az Azure gépi tanulási modell, amely hajt végre pontozás, a szövegbevitel stb twitter sentiment-elemzés indítása. |
| [Az egyéni tevékenység Twitter-elemzés](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  Ez a példa bemutatja egy egyéni .NET tevékenység használatáról az Azure gépi tanulási modell pontozás, a szövegbevitel stb twitter sentiment-elemzés végző meghívásához. |
| [Az Azure gépi tanulási paraméteres folyamatok](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | A minta-N folyamatok pontozási és ahonnan parameters.txt fájl, amely szerepel ebben a példában a területek listájának érkezik más területére paraméter mindegyiket átképzés üzembe végpont C# kód biztosít. | 
| [Hivatkozás az Azure Értékáram-elemzés feladatok adatok frissítése](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  Ez a példa bemutatja, hogyan Azure Data Factory és Azure Értékáram-elemzés együttes használata a lekérdezések futtatása a hivatkozási adatok és a hivatkozási adatok ütemezett frissítést beállítani. |
| [A helyszíni Hortonworks Hadoop hibrid folyamat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | A minta egy helyszíni Hadoop fürthöz számítási cél futó feladatok Data Factory csak hasonlóan, mint amikor más számítási célok, mint egy HDInsight-alapú Hadoop fürt felhőben használja. |
| [JSON átalakító eszközt](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | Ez az eszköz JSONs konvertálása 2015-07-01-előnézet legújabb vagy a Skype 2015-07-01 – előzetes verzió (alapértelmezett) előtt verziójából teszi lehetővé. |  
| [Az SQL-U beviteli mintafájl](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  A fájl U-SQL nyelvben tevékenység által használt mintafájl. | 

## <a name="azure-resource-manager-templates"></a>Azure erőforrás-kezelő sablonok
Az alábbi erőforrás-kezelő Azure-sablonok a Github Data Factory talál. 

| Sablon | Leírás |
| -------- | ----------- | 
| [Azure Blob-tárolóhoz másolja Azure SQL-adatbázishoz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Ezzel a sablonnal üzembe helyezése az Azure adatok gyári létrehoz egy folyamat, amely másolja az adatokat a megadott Azure blob-tárolóhoz az Azure SQL-adatbázis |    
| [Másolja a Salesforce Azure Blob-tárolóhoz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Az Azure adatok gyári üzembe helyezése a sablon létrehoz egy folyamat, amely a megadott Salesforce-fiók adatait másolja az Azure blob-tárolóhoz. |    
| [Adatok átalakítása egy Azure hdinsight szolgáltatáshoz fürt struktúra parancsfájl futtatásával](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Az Azure adatok gyári üzembe helyezése a sablon létrehoz egy folyamat, amely adatok alakítja a struktúra mintaparancsfájl egy Azure hdinsight szolgáltatáshoz Hadoop fürt futtatásával. | 


## <a name="samples-in-azure-portal"></a>A minták az Azure-portálon
Használhatja a **minta folyamatok** csempére az adatok factory kezdőlapján kattintson az adatok gyári minta folyamatok és a kapcsolódó szervezetek (adatkészleteket és csatolt szolgáltatások) a telepítéshez használni. 

1. Adatok gyár létrehozása vagy egy meglévő adatok gyári megnyitása. Című [Azure Data Factory – első lépések](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) a lépések adatok gyár létrehozásához.
2. Az adatok gyári **DATA FACTORY** lap kattintson a **minta folyamatok** csempére.

    ![Minta folyamatok csempe](./media/data-factory-samples/SamplePipelinesTile.png)

2. Kattintson a **minta folyamatok** lap **minta** telepítéshez használni kívánt. 
    
    ![Minta folyamatok lap](./media/data-factory-samples/SampleTile.png)

3. Adja meg a minta beállításait. Például az Azure tároló nevét és a fiók fiókkulcs, Azure SQL-kiszolgáló neve, adatbázis, felhasználói azonosító, és a jelszó stb. 

    ![A lap minta](./media/data-factory-samples/SampleBlade.png)

4. Miután végzett a beállítások megadásával, kattintson a **Létrehozás** létrehozása és telepítése a minta folyamatok és csatolt szolgáltatások/táblázatokat a folyamatok által használt gombra.
5. A minta csempére kattintott korábbi kattintson a **minta folyamatok** lap telepítési állapotának látható.

    ![Telepítési állapota](./media/data-factory-samples/DeploymentStatus.png)

6. Ha a minta csempéje a **sikeres telepítés** üzenetet látja, zárja be a **minta folyamatok** lap.  
5. A **DATA FACTORY** lap látható, hogy az adatok gyári csatolt szolgáltatások, a adatkészletek és a folyamatok vannak felvéve.  

    ![Adatok gyári lap](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>A Visual Studióban minták

### <a name="prerequisites"></a>Előfeltételek

A következő, a számítógépen telepítve kell rendelkeznie: 

- Visual Studio 2013 vagy a Visual Studio 2015
- Töltse le a Visual Studio 2013 vagy a Visual Studio 2015 Azure SDK csomagjában talál. Nyissa meg [Azure töltse le a lapot](https://azure.microsoft.com/downloads/) , és kattintson **és 2013-ban** vagy a **VIEWBEN 2015** **.NET** szakaszában.
- Töltse le a legújabb Azure Data Factory beépülő modul for Visual Studio: [és 2013-ban](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) vagy [a Skype 2015 VIEWBEN](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Ha a Visual Studio 2013 használata esetén is módosíthatja a bővítményt az alábbi lépésekkel: menüben kattintson az **eszközök** -> **Extensions és frissítések** -> **Online** -> **Visual Studio gyűjtemény** -> **A Microsoft Azure adatok gyári Tools for Visual Studio** -> **frissítése**.

### <a name="use-data-factory-templates"></a>Adatok gyári sablonok használata

1. Kattintson a **fájl** menü, mutasson az **Új**, és kattintson a **Projekt**. 
2. **Új projekt** párbeszédpanelen hajtsa végre az alábbi lépéseket: 
    1. Jelölje ki a **DataFactory** **sablonok**csoportban. 
    2. Jelölje ki a jobb oldali **Adatok gyári sablonok** . 
    3. Adja meg a projekt **nevére** . 
    4. Jelölje ki a projekt **helyét** . 
    5. Kattintson az **OK gombra**. 

    ![Új projekt párbeszédpanel](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. **Adatok gyári sablonok** párbeszédpanelen jelölje be a minta sablon **Használatieset sablonok** szakaszából, és kattintson a **Tovább**gombra. Az alábbi lépésekkel végigvezetik Önt a **Felhasználói adatgyűjtés** sablonnal. Lépések hasonlóak a mintáknál. 

    ![Adatok gyári sablonok párbeszédpanel](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. Az **Adatok gyári konfigurációs** párbeszédpanelen kattintson a **Tovább** gombra az **Adatok gyári alapjai** lapon.
8. A **Configure adatok gyári** lapon végezze el az alábbi lépéseket: 
    1. **Hozzon létre új Data Factory**kijelölése Emellett bejelölheti a **használata a meglévő adatok gyári**.
    2. Írja be egy **nevet** a data factory.
    3. Jelölje ki az **Azure előfizetés** , amelyben a data factory létrehozni. 
    4. Jelölje ki az adatok gyári **erőforráscsoport** .
    5. Jelölje ki a **Nyugati Amerikai Egyesült Államok**, a **Kelet-Amerikai Egyesült Államok**, vagy a **Észak-Európa** a **régió**.
    6. Kattintson a **Tovább**gombra. 
9. **Konfigurálása adatokat tárolja** lapon adjon meg egy meglévő **Azure SQL-adatbázishoz** , és **Azure tároló fiók** (vagy) létrehozása az adatbázis/tárhely, és kattintson a Tovább gombra. 
10. Az **kiszámítania beállítása** lapon jelölje be az alapértelmezett értékek, és kattintson a **Tovább**gombra. 
11. Az **Összegzés** lapon tekintse át az összes beállításokat, és kattintson a **Tovább**gombra. 
12. A **Telepítési állapota** lapon, a várja meg, amíg befejeződik a telepítés, és kattintson a **Befejezés gombra**.
13. Kattintson a jobb gombbal a project a megoldást Intézőben, és kattintson a **Közzététel**gombra. 
19. **Jelentkezzen be Microsoft-fiókja** párbeszédpanel jelenik meg, ha az Azure előfizetéssel rendelkező adja meg a hitelesítő adatait, és kattintson a **Bejelentkezés**gombra.
20. Meg kell jelennie a következő párbeszédpanelen:

    ![Közzététele párbeszédpanel](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. A **Configure adatok gyári** lapon hajtsa végre az alábbi lépéseket: 
    1. Győződjön meg arról, **használja a meglévő adatok gyári** beállítást.
    2. Jelölje ki az **adatok gyári** is voltak, jelölje be a sablon használata esetén. 
    6. Kattintson a **Tovább gombra** kattintva lépjen az **Elemek közzététele** lapra. (Nyomja meg a **lap** áthelyezése a név mezőben, hogy ki, ha a **következő** gomb nem érhető el.) 
23. Az **Elemek közzététel** lapon győződjön meg róla, hogy az adatok gyárak szervezetek legyen kijelölve, majd kattintson a **Tovább gombra** kattintva lépjen az **összefoglaló** lapra.     
24. Tekintse át az összefoglaló, és kattintson a **Tovább gombra** kattintva indítsa el a telepítési folyamatot, és a **Telepítés állapotának**megtekintése gombra.
25. A **Telepítési állapota** lapon a telepítés állapotának láthatók. Miután végzett a telepítést, kattintson a Befejezés gombra. 

Kapcsolatos további tudnivalók: [az első adatok gyári (Visual Studio) összeállítása](data-factory-build-your-first-pipeline-using-vs.md) a Visual Studio segítségével szerzői adatok gyári személyek és Azure közzététellel.          