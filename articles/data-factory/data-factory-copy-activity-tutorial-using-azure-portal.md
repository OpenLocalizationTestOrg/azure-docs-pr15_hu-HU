<properties 
    pageTitle="Oktatóprogram: Hozzon létre egy folyamat Azure portálon másolás tevékenység |} Microsoft Azure" 
    description="Ebben az oktatóanyagban létrehozása az Azure Data Factory folyamat egy másolatot a tevékenységhez a szerkesztőprogrammal az adatok gyári az Azure-portálon." 
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
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység Azure portál használatával
> [AZURE.SELECTOR]
- [Áttekintés és a vonatkozó követelmények](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Másolja a varázsló](data-factory-copy-data-wizard-tutorial.md)
- [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [A PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Erőforrás-kezelő Azure-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-VAL](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)



Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre, és figyelemmel az Azure adatok gyári az Azure portálon. Az adatok gyári folyamat egy példány tevékenység használja az adatok másolása az Azure Blob-tárolóhoz az Azure SQL-adatbázis.

Az alábbiakban a részeként ebben az oktatóanyagban végrehajtandó lépések:

Lépés | Leírás
-----| -----------
[Hozzon létre egy Azure Data Factory](#create-data-factory) | Ebben a lépésben az Azure adatok gyári **ADFTutorialDataFactory**nevű hoz létre.  
[Hozzon létre csatolt szolgáltatások](#create-linked-services) | Ebben a lépésben hoz létre két csatolt szolgáltatások: **AzureStorageLinkedService** és **AzureSqlLinkedService**. <br/><br/>A AzureStorageLinkedService hivatkozások Azure tárolására és AzureSqlLinkedService az Azure SQL-adatbázis csatolása a ADFTutorialDataFactory. A bemeneti adatok az a folyamat az Azure blob-tárhely és a kimeneti adatok blob-tárolóban található kell tárolni az Azure SQL-adatbázis táblájához. Ezért vesz fel két áruház csatolt szolgáltatásként gyári adatok.      
[Létrehozása a bemeneti és kimeneti adatkészleteket](#create-datasets) | Az előző lépésben létrehozott csatolt szolgáltatások hivatkozó bemeneti és kimeneti adatokat tartalmazó adatokat tárolja. Ebben a lépésben két adatkészleteket – **InputDataset** és **OutputDataset** – bemeneti és kimeneti tárolja az adatokat tárolja az adatokat megjelenítő határozza meg. <br/><br/>A InputDataset megadhatja, hogy a blob-tárolóhoz, amely tartalmazza a forrásadatok és az OutputDataset blob, megadhatja, hogy az SQL táblázat, amely a kimeneti adatokat tárolja. Egyéb tulajdonságait, például a struktúra, elérhetőségét és házirend is adja meg. 
[Egy folyamat létrehozása](#create-pipeline) | Ebben a lépésben létrehoz egy **ADFTutorialPipeline** a ADFTutorialDataFactory a nevű folyamat. <br/><br/>**Másolás tevékenység** hozzáadása a másolatok bemeneti adatok az Azure BLOB-e az Azure SQL-eredménytábla során. A Másolás tevékenység az adatok mozgását Azure Data Factory hajt végre. Világszerte elérhető szolgáltatás, amely másolhatja az adatokat a különböző adatokat tárolja, biztonságos, megbízható és méretezhető útján között épül. [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk lásd: a Másolás tevékenység kapcsolatban további tájékoztatást. 
[Monitor folyamat](#monitor-pipeline) | Ebben a lépésben a bemeneti és kimeneti táblák szeleteit Azure portál használatával figyelheti.

## <a name="prerequisites"></a>Előfeltételek 
Teljes Előfeltételek ebben az oktatóanyagban végrehajtása előtt [Oktatóanyag – áttekintés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) című témakörben található.

## <a name="create-data-factory"></a>Adatok gyári létrehozása
Ebben a lépésben az Azure portál létrehozása az Azure adatok gyári **ADFTutorialDataFactory**nevű használja.

1.  Az [Azure portal](https://portal.azure.com/)bejelentkezés után kattintson az **Új**gombra, válassza az **üzletiintelligencia- + Analytics**, és kattintson az **Adatok gyári**parancsra. 

    ![Új-DataFactory >](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)  

6. Kattintson az **új adatok gyári** lap:
    1. Írja be a **név** **ADFTutorialDataFactory** . 
    
        ![Új adatok gyári lap](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)

        Az Azure adatok gyári neve **globálisan egyedinek**kell lennie. Az alábbi hibaüzenet jelenik meg, ha az adatok gyári (például yournameADFTutorialDataFactory) nevének módosítása, és próbáljon meg ismét létrehozni. [Adatok Factory - elnevezési szabályai](data-factory-naming-rules.md) témakört vonatkozó adatok gyári eltérések elnevezési szabályokat.
    
            Data factory name “ADFTutorialDataFactory” is not available  
     
        ![Gyári neve nem áll rendelkezésre](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
    2. Jelölje ki az Azure **előfizetés**.
    3. Az erőforráscsoport tegye a következőket:
        1. Jelölje be a **meglévő használata**, és válassza a meglévő erőforráscsoport a legördülő listából. 
        2. Jelölje be az **Új létrehozása**, és írja be a csoport nevét, egy erőforrás.   
    
            Néhány lépést ebben az oktatóanyagban tegyük fel, hogy a név használható: **ADFTutorialResourceGroup** az erőforráscsoport. Erőforráscsoport kapcsolatos további tudnivalókért lásd: a [használata erőforrás csoportok az Azure erőforrások kezelése](../azure-resource-manager/resource-group-overview.md).  
    4. Válassza ki a data factory **helyét** . Csak az adatok gyári szolgáltatás által támogatott régiók jelennek meg a legördülő listában.
    5. Válassza a **Rögzítés a Startboard**.     
    6. Kattintson a **létrehozása**gombra.

        > [AZURE.IMPORTANT] Adatok Factory-példányok létrehozásához az [Adatok gyári munkatársi](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) szerepkörök, az előfizetés/erőforrás csoport szintjén tagjának kell lennie.
        >  
        >  Az adatok gyári neve bejegyezhető DNS nevével ennélfogva és a jövőben nyilvánosan láthatóvá válnak.              
9.  Az állapot/értesítő üzenetek megtekintéséhez kattintson a csengő ikon az eszköztáron. 

    ![Értesítését](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
10. A létrehozását követően az **Adatok gyári** lap látni a képen látható módon.

    ![Adatok factory kezdőlapján](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Hozzon létre csatolt szolgáltatások
Csatolt szolgáltatások hivatkozás adatokat tárolja, vagy az Azure adatok gyári szolgáltatások számítja ki. Lásd: [támogatott adatokat tárolja](data-factory-data-movement-activities.md##supported-data-stores-and-formats) az adatforrások és mosdók a Másolás tevékenység által támogatott. Lásd: a [csatolt szolgáltatások kiszámítása](data-factory-compute-linked-services.md) az adatok gyári által támogatott számítási szolgáltatások listában. Ebben az oktatóanyagban bármely számítási szolgáltatás nem használja. 

Ebben a lépésben hoz létre két csatolt szolgáltatások: **AzureStorageLinkedService** és **AzureSqlLinkedService**. AzureStorageLinkedService csatolt Azure tároló fiók szolgáltatás hivatkozások és AzureSqlLinkedService Azure SQL-adatbázishoz csatolja a **ADFTutorialDataFactory**. Ebben az oktatóanyagban másolja az adatokat a AzureStorageLinkedService blob-tárolóból AzureSqlLinkedService SQL táblájához belül egy folyamat hoz létre.

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>Csatolt szolgáltatás az Azure tárterület-fiók létrehozása
1.  Kattintson az **Adatok gyári** lap **Szerző és üzembe** csempe az adatok gyári **szerkesztő** indítása.

    ![Tartalom-előállítás és üzembe csempe](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
5. A **szerkesztő**kattintson az eszköztár **új adatok tárolására** gombjára, és a legördülő menüből válassza a **Azure tároló** . Meg kell jelennie a JSON-sablon létrehozása az Azure csatolt tárhelyszolgáltatáshoz a jobb oldali. 

    ![Szerkesztő új adatokat tároló gomb](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
6. Csere `<accountname>` és `<accountkey>` a fiók nevét és a fiók kulcs értékeit az Azure tárterület-fiók. 

    ![Szerkesztő JSON Blob-tárolóhoz](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png) 
6. Kattintson a **központi telepítés** az eszköztáron. Meg kell jelennie a fastruktúrájú nézetben **AzureStorageLinkedService** telepített most. 

    ![Szerkesztő Blob-tárolóhoz terjesztése](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [AZURE.NOTE]
> Kapcsolatos további tudnivalók: [adatok a/Azure Blob áthelyezése](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON tulajdonságait.

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Hozzon létre egy csatolt szolgáltatást az Azure SQL-adatbázis
1. Az **Adatok gyári szerkesztő**kattintson az eszköztár **új adatok tárolására** gombjára, és a legördülő menüből válassza az **Azure SQL-adatbázis** . Meg kell jelennie a JSON sablont hozhat létre a csatolt Azure SQL-szolgáltatás a jobb oldali ablaktáblában.
2. Csere `<servername>`, `<databasename>`, `<username>@<servername>`, és `<password>` neveivel a Azure SQL server, az adatbázis, a felhasználói fiókot, és a jelszavát. 
3. Kattintson a **központi telepítés** készíthetnek és helyezhetnek üzembe a **AzureSqlLinkedService**az eszköztáron.
4. Győződjön meg arról, hogy a fastruktúrájú nézetben látható **AzureSqlLinkedService** . 

> [AZURE.NOTE]
> Kapcsolatos további tudnivalók: [adatok a/Azure SQL-adatbázis áthelyezése](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) JSON tulajdonságait.

## <a name="create-datasets"></a>Hozzon létre adatkészleteket
Az előző lépésben létrehozott csatolt **AzureStorageLinkedService** és szolgáltatások **AzureSqlLinkedService** Azure tároló fiókot és Azure SQL-adatbázis csatolása az adatok gyári: **ADFTutorialDataFactory**. Ebben a lépésben két adatkészleteket – **InputDataset** és **OutputDataset** – az adatok tárolóban AzureStorageLinkedService és AzureSqlLinkedService említett van tárolva bemeneti és kimeneti adatokat megjelenítő határozza meg. InputDataset megadhatja, hogy a blob-tárolóhoz, amely tartalmazza a forrásadatok és az OutputDataset blob, megadhatja, hogy az SQL táblázat, amely a kimeneti adatokat tárolja. 

### <a name="create-input-dataset"></a>Beviteli adatkészlet létrehozása 
Ebben a lépésben létrehoz egy adatkészletet, amely a csatolt **AzureStorageLinkedService** szolgáltatás jelöli Azure-tárolóban lévő blob-tárolóhoz mutat **InputDataset** nevű.

1. Kattintson a **szerkesztő** az adatok gyári **... További**, kattintson az **Új adatkészlet**és **Azure Blob-tárolóhoz** kattintson a legördülő menüből. 

    ![Az új adatkészlet menü](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Cserélje a JSON a jobb oldali ablaktáblában a következő JSON kódtöredékének: 

        {
          "name": "InputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
        
     Vegye figyelembe az alábbiakat: 
    
    - adatkészlet **típusú** **AzureBlob**van beállítva.
    - **linkedServiceName** **AzureStorageLinkedService**értékre van állítva. A 2 létrehozott csatolt szolgáltatást.
    - a **adftutorial** tároló **Mappa_útvonala** van beállítva. Adja meg a nevét a **fájlnév** tulajdonságot használó mappában tárolt blob is. Nem a blob nevét adja meg, mivel minden BLOB-tárolóban adatainak tekinteni egy bemeneti adatok.  
    - formátum **típusa** **TextFormat** beállítása
    - Három elem látható a szövegfájl – **Utónév** és **Vezetéknév** – (**columnDelimiter**) karakterrel vesszővel elválasztott 
    - Az **Elérhetőség** értéke **óránként** (**gyakoriság** értéke **órát** és **intervallum** értéke **1**). Ezért Data Factory megkeresi a bemeneti adatok óránként blob-tárolóhoz (**adftutorial**) megadott a legfelső szintű mappájára. 
    
    Ha Ön nem adja a **bemeneti** adatkészlet egy **fájlnevet** , valamennyi fájlok/BLOB a bemeneti mappából (**Mappa_útvonala**) bemeneti adatok alapján számít. A JSON ad meg egy fájlnevet, ha csak a megadott fájl/blob tekintendő asn beviteli.
 
    Ha nem ad meg egy **fájlnevet** egy **eredménytábla**, a **Mappa_útvonala** a létrehozott fájlok neve a következő formátumban: adatok. &lt;Globálisan egyedi azonosítója\&gt;. a txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **Mappa_útvonala** és **fileName** dinamikusan alapján a **SliceStart** időpontot szeretne beállítani, a **partitionedBy** tulajdonsággal. A következő példában Mappa_útvonala használja évet, hónapot és napot az a SliceStart (a feldolgozása szeletet kezdetének), valamint fileName a SliceStart órára. Ha például az darab készült szelet 2016-09-20T08:00:00, a mappanév wikidatagateway/wikisampledataout/2016/09/20 van állítva, és a fájlnév 08.csv van beállítva. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],
2. Kattintson a **központi telepítés** készíthetnek és helyezhetnek üzembe a **InputDataset** adatkészlet az eszköztáron. Győződjön meg arról, hogy a **InputDataset** belül a fastruktúra nézetben látható.

> [AZURE.NOTE]
> Kapcsolatos további tudnivalók: [adatok a/Azure Blob áthelyezése](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) JSON tulajdonságait.

### <a name="create-output-dataset"></a>Hozzon létre a kimeneti adatkészlet
Ez a lépés részében hozzon létre egy **OutputDataset**nevű kimeneti adatkészlet. Ez az adatkészlet az **AzureSqlLinkedService**jelöli Azure SQL-adatbázis táblájához SQL mutat. 

1. Kattintson a **szerkesztő** az adatok gyári **... További**kattintson az **Új adatkészlet**, és kattintson az **Azure SQL** a legördülő menüből. 
2. Cserélje a JSON a jobb oldali ablaktáblában a következő JSON kódtöredékének:

        {
          "name": "OutputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
        
     Vegye figyelembe az alábbiakat: 
    
    - adatkészlet **típusú** **AzureSQLTable**van beállítva.
    - **linkedServiceName** **AzureSqlLinkedService** (létrehozott csatolt szolgáltatást a 2) értékre van állítva.
    - **táblanév** **a vállalati projektirányítási**értékre van állítva.
    - Három oszlop van – **Azonosítót**, **Vezetéknév**és **Utónév** – a vállalati projektirányítási táblázatban az adatbázisban. Azonosító-azonosító oszlop, így csak a **Vezetéknév** és **Utónév** Itt határozhatja meg kell.
    - **Elérhetőség** a **óránként** (**gyakoriság** **órára** és **intervallum** értéke **1**) értékre van állítva.  Az adatok gyári szolgáltatás készít egy kimenet szeletre óránként a **Vállalati projektirányítási** táblázat az Azure SQL-adatbázisban.

3. Kattintson a **központi telepítés** készíthetnek és helyezhetnek üzembe a **OutputDataset** adatkészlet az eszköztáron. Győződjön meg arról, hogy a **OutputDataset** belül a fastruktúra nézetben látható. 

> [AZURE.NOTE]
> Kapcsolatos további tudnivalók: [adatok a/Azure SQL-adatbázis áthelyezése](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) JSON tulajdonságait.

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz egy folyamat tartalmazó a **Másolás tevékenység** **InputDataset** használja, mint a bemeneti és **OutputDataset** ezt az eredményt.

1. Kattintson a **szerkesztő** az adatok gyári **... További**, és kattintson az **új folyamat**. Azt is megteheti kattintson a jobb gombbal **folyamatok** belül a fastruktúra nézetben, és kattintson az **új folyamat**.
2. Cserélje a JSON a jobb oldali ablaktáblában a következő JSON kódtöredékének: 
        
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
          }
        } 

    Vegye figyelembe az alábbiakat:

    - A tevékenységek csoportban található csak egy tevékenység, amelynek a **típus** értéke **másolása**
    - A tevékenységhez tartozó bemeneti **InputDataset** van állítva, és a tevékenység kimeneti **OutputDataset**van beállítva.
    - **TypeProperties** csoportban a forrás típusa **BlobSource** van megadva, és **SqlSink** van megadva, gyűjtő típust.

    A **Kezdés** tulajdonság értékét cserélje le az aktuális nap és **Záró** érték, a következő napra. Adja meg csak a dátum részt, és ugorja át az időrészre, a dátum-idő. Ha például "2016-02-03", amely egyenértékű "2016 – 02-03T00:00:00Z"
    
    Mindkét indítsa el, és a záró időpontok [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601)kell lennie. Példa: 2016 – 10-14T16:32:41Z. A **befejezési** idő nem kötelező, de ebben az oktatóanyagban használjuk. 
    
    Ha nem adja meg a **befejezési** tulajdonság értékét, mint "**Kezdés + 48 óra**" számítja ki. A folyamat végtelen időre szóló futtatásához adja meg az érték a **befejezési** tulajdonság **9999-09-09** .
    
    Az előző példában vannak 24 adatok szeletek minden szeletre óránként előállított-e.
    
4. Kattintson a **központi telepítés** készíthetnek és helyezhetnek üzembe a **ADFTutorialPipeline**az eszköztáron. Győződjön meg arról, hogy látható-e a fastruktúrájú nézetben a folyamat. 
5. Ezután zárja be a **szerkesztő** lap **X**gombra kattintva. Kattintson az **X** ismét a lásd: az **Adatok Factory** kezdőlapján a **ADFTutorialDataFactory**.

**Gratulálok!** Sikeresen létre az Azure adatok gyári, csatolt szolgáltatások, táblázatok és a folyamat, és a folyamat ütemezett.   
 
### <a name="view-the-data-factory-in-a-diagram-view"></a>Az adatok gyári megtekintése a Diagram nézetben 
1. Kattintson az **Adatok gyári** lap **Diagram**.

    ![Adatok gyári lap - Diagram csempe](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Meg kell jelennie a diagramot, az alábbi képhez hasonló: 

    ![Diagram nézetben](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)

    Nagyítás, Kicsinyítés, a Nagyítás 100 %-ra, a Nagyítás igazítása, automatikus folyamatok és táblázatok elhelyezése és felsorolása megjelenítése (kiemeli a kijelölt elemek felsőbb szintű és lefelé irányuló elemeket).  Objektum (bemeneti és kimeneti tábla vagy folyamat) szeretné megjeleníteni a tulajdonságokat rá duplán. 
3. Kattintson a jobb gombbal a **ADFTutorialPipeline** a Diagram nézetben, és kattintson a **Megnyitás folyamat**. 

    ![Nyissa meg a folyamat](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. Meg kell jelennie a bemeneti és kimeneti, a tevékenységek adatkészleteket együtt során a tevékenységet. Ebben az oktatóanyagban csak egy tevékenységet a folyamat (Másolás tevékenység) InputDataset a bemeneti adatkészlet és OutputDataset kimeneti adatkészlet, akkor.   

    ![Megnyitott folyamat megjelenítése](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Kattintson az **adatok gyári** kattintva térjen vissza a diagram nézet bal felső sarokban a navigációs. A diagram nézet összes folyamatok jeleníti meg. Ebben a példában csak egy folyamat hozott létre.   
 

## <a name="monitor-pipeline"></a>Monitor folyamat
Ebben a lépésben használatával az Azure portal figyelheti az Azure adatok gyári fog. 

### <a name="monitor-pipeline-using-diagram-view"></a>Monitor folyamat diagramnézetében

1. Kattintson az **X** , az adatok gyári Data Factory honlapján találhat a **Diagram** nézet bezárása. Ha a webböngészőben van megnyitva, hajtsa végre az alábbi lépéseket: 
    2. Nyissa meg az [Azure](https://portal.azure.com/)-portálra. 
    2. Kattintson duplán a **ADFTutorialDataFactory** a **Startboard** a (vagy) **adatok gyárak** kattintson a bal oldali menüben, és ADFTutorialDataFactory kereshet. 
3. Meg kell jelennie a darab és a táblák és a folyamat hozta létre a lap nevét.

    ![nevek kezdőlapja](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
4. Ezután kattintson a **adatkészleteket** csempére.
5. Kattintson a **adatkészleteket** lap **InputDataset**. Ez az adatkészlet a beviteli adatkészlet számára **ADFTutorialPipeline**.

    ![A kijelölt InputDataset adatkészleteket](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Kattintson a **... (három pont)** az adatok az összes szelet megtekintéséhez.

    ![Az összes bemeneti adatok szeletek](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  

    Figyelje meg, hogy a pontos idő felfelé adatok szeletek **készen** állnak-e, mert a **emp.txt** fájl mindig megtalálható a blob-tárolóhoz: **adftutorial\input**. Győződjön meg arról, hogy nincs szeletek megjelennek a a **legutóbb sikertelen szeletek** szakaszban, a képernyő alján.

    **A szeletek nemrég frissített** és a **legutóbb nem sikerült a szeletek** is listák a **LEGUTOLSÓ frissítés ideje**szerint vannak rendezve. 
    
    A szeletek szűrése az eszköztáron kattintson a **Szűrés** gombra.  
    
    ![Beviteli szeletek szűrése](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. Zárja be a rögzítéséhez, amíg meg nem jelenik a **adatkészleteket** lap. Kattintson a **OutputDataset**. Ez az adatkészlet a kimeneti adatkészlet számára **ADFTutorialPipeline**.

    ![adatkészletek lap](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
6. Az alábbi képen látható módon a **OutputDataset** lap kell megjelennie:

    ![táblázat lap](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
7. Figyelje meg, hogy az adatok szeletek felfelé az aktuális idő már előállítani, és **készen**áll. Nincs szeletek jelenik meg a **problémát szeletek** szakaszban, a képernyő alján.
8. Kattintson a **... (Három pont)** az összes szelet megtekintéséhez.

    ![adatok szeletek lap](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
9. Kattintson bármelyik szeletre a listából, és meg kell jelennie a **szeletre** lap.

    ![adatok szeletet lap](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
  
    Ha a szeletet nem **kész** állapotú, megtekintheti a felsőbb szintű szeletek, amely nem kész, amelyek gátolják az aktuális szelet végrehajtása a **felsőbb szintű szeletek, amelyek nem áll készen** listában.
11. Az **Adatok SZELETET** lap láthatók az összes tevékenység fut a lista alján. Kattintson egy **tevékenység futtassa** a **tevékenységet, futtassa a részletek** lap megjelenítéséhez. 

    ![Tevékenység futtatása részletei](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. Zárja be az összes rögzítéséhez, amíg el nem vissza a Kezdőlap lap a **ADFTutorialDataFactory**az **X** gombra.
14. (nem kötelező) **Folyamatok** kattintson a Kezdőlap lapon a **ADFTutorialDataFactory** **ADFTutorialPipeline** kattintson a **folyamatok** lap a, és hatoljon beviteli táblázatok (**felhasznált**) vagy kimeneti (**Produced**) keresztül.
15. Indítsa el az **SQL Server Management Studio eszközben**, az Azure SQL-adatbázis csatlakoztatása, és ellenőrizze, hogy a sorok a **Vállalati projektirányítási** táblázatra az adatbázisban vannak beszúrva.

    ![SQL-lekérdezés eredménye](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Lync-folyamat Monitor és kezelheti a alkalmazás használata
Is Monitor használja, és a folyamatok Lync alkalmazás kezeléséhez. Ez az alkalmazás használatával kapcsolatos részletes tudnivalókért lásd [Monitor és kezelheti a figyelés és felügyeleti alkalmazás Azure Data Factory folyamatok](data-factory-monitor-manage-app.md).

1. Kattintson az adatok factory kezdőlapján **Monitor és kezelés** csempére.

    ![Figyelésére és csempe kezelése](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. Meg kell jelennie **Monitor és kezelés alkalmazást**. Módosítsa a **Kezdő időpont** és **Záró időpont** kezdési (2016 – 07 – 12) fejeződjön (2016-07-13) a folyamat, és kattintson az **Alkalmaz**gombra. 

    ![Lync- és az alkalmazás kezelése](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Jelölje ki a **Tevékenységet a Windows** listában, szeretne tudni, hogy egy tevékenység ablakban. 
    ![Tevékenység ablak részletei](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>Összefoglalás 
Ebben az oktatóprogramban egy Azure adatok gyári másolása létrehozott adatainak egy Azure blob-Azure SQL-adatbázishoz. Az Azure portal hozta létre az adatok gyári, csatolt szolgáltatások, adatkészleteket és a folyamat. Ha végzett, ebben az oktatóanyagban a magas szintű lépések a következők:  

1.  Az Azure **adatok gyári**létre.
2.  Hozzon létre **csatolt szolgáltatások**:
    1. Hivatkozás: Azure tárterület-fiókját a bemeneti adatokat tartalmazó az **Azure tároló** csatolt szolgáltatások.    
    2. Egy **Azure SQL** szolgáltatás, amelyre a hivatkozás a kimeneti adatokat tároló Azure SQL-adatbázishoz csatolja. 
3.  A létrehozott **adatkészleteket** , amely jellemzi a bemeneti és kimeneti adatok számára a folyamatok.
4.  Forrás- és **SqlSink** gyűjtő, mint egy **Példány tevékenység** **BlobSource** létre **folyamat** .  


## <a name="see-also"></a>Lásd még:
| A témakör | Leírás |
| :---- | :---- |
| [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) | Ez a cikk a Másolás tevékenység használta az oktatóprogram részletes információt tartalmaz. |
| [Ütemezés- és végrehajtása](data-factory-scheduling-and-execution.md) | Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell ütemezési és a végrehajtás szempontjait. |
| [Folyamatok](data-factory-create-pipelines.md) | Ez a cikk segít megérteni a folyamatok és Azure Data Factory tevékenységeket. |
| [Adatkészletek](data-factory-create-datasets.md) | Ez a cikk segít megérteni az Azure Data Factory adatkészleteket.
| [Figyelésére és figyelése alkalmazással folyamatok kezelése](data-factory-monitor-manage-app.md) | Ez a cikk leírja, hogy miként figyelheti, kezelése és használata a figyelő és felügyeleti alkalmazás folyamatok hibakeresési. 


