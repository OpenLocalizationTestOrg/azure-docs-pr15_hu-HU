<properties 
    pageTitle="Az SQL Server tárolt eljárás tevékenység" 
    description="Megtudhatja, hogyan használhatja az SQL Server tárolt eljárás tevékenység egy SQL Azure-adatbázis vagy az Azure SQL-adatraktár egy Data Factory folyamat a tárolt eljárás meghívásához." 
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
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>Az SQL Server tárolt eljárás tevékenység
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[egyéni .NET](data-factory-use-custom-activities.md)

Az SQL Server-a tárolt eljárás tevékenység az adatok gyári [folyamat](data-factory-create-pipelines.md) meghívása a tárolt eljárás között az alábbi adatokat tárolja az használhatók: 


- Azure SQL-adatbázis 
- Az SQL Azure-adatraktár  
- SQL Server-adatbázisból a enterprise vagy az Azure virtuális. Meg kell az adatkezelési átjáró telepítése, ugyanazon a gépen az adatbázist tároló vagy egy másik számítógépre, az adatbázis-kezelő erőforrások gyorskorcsolyázásban elkerülése érdekében. Az adatkezelési átjáró a szoftveres kapcsolódó a helyszíni adatok források/adatforrások az Azure VMs hosed felhőszolgáltatásokhoz biztonságos és felügyelt módon. Lásd: [az adatok helyszíni és felhőbeli közötti áthelyezése a](data-factory-move-data-between-onprem-and-cloud.md) cikk az adatkezelési átjáró kapcsolatban további tájékoztatást. 

Ez a cikk a [tevékenységekre vonatkozó adatok átalakítása](data-factory-data-transformation-activities.md) cikk bemutatja, átalakítási és a támogatott átalakítása tevékenységek általános áttekintése épül.

## <a name="walkthrough"></a>Útmutató

### <a name="sample-table-and-stored-procedure"></a>Mintatáblázat és a tárolt eljárás
1. Az alábbi **táblázat** az SQL Server Management Studio segítségével Azure SQL-adatbázis létrehozása vagy más eszköz gondot. A datetimestamp oszlop, a dátum és idő a megfelelő azonosító létrehozásakor. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Azonosító egyedi azonosította pedig a datetimestamp oszlop dátuma és időpontja, a megfelelő azonosító létrehozásakor.
    ![Mintaadatok](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] Ez a példa Azure SQL-adatbázis használ, de ugyanúgy működik, az Azure SQL-adatraktár és az SQL Server-adatbázishoz. 
2. Hozzon létre a következő **tárolt eljárás** , szúrja be a **sampletable**az adatokat.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Nevet** és **géphez** (ebben a példában a DateTime) paraméter egyeznie kell a folyamat/tevékenység JSON megadott paramétert. A tárolt eljárás definíció győződjön meg róla, hogy **@** előtaggal a paraméterhez használt.
    
### <a name="create-a-data-factory"></a>Adatok gyár létrehozása  
4. Jelentkezzen be az [Azure](https://portal.azure.com/)-portálra. 
5. Kattintson az **Új** gombra a bal oldali menüben kattintson a **üzletiintelligencia- + Analytics**, és kattintson az **Adatok gyári**.
    
    ![Új adatok gyári](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  Írja be az **új adatok gyári** lap **SProcDF** nevét. Azure Data Factory nevek **globálisan**egyediek. Kell előtag a nevét, a gyártás a sikeres létrehozásának engedélyezése a adatok gyártás nevét.

    ![Új adatok gyári](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Jelölje ki az **Azure előfizetés**. 
4.  **Erőforráscsoport**hajtsa végre a következő lépések egyikét: 
    1.  Kattintson az **Új létrehozása** , és írjon be egy nevet az erőforráscsoport.
    2.  Kattintson a **meglévő használja** , és válassza a meglévő erőforráscsoport.  
5.  Válassza ki a data factory **helyét** .
6.  Jelölje be a **PIN-kód irányítópult** , hogy megjelenik a data factory az irányítópulton legközelebb bejelentkezik az. 
6.  Kattintson a **Létrehozás** gombra az **új adatok gyári** lap.
6.  Az **Irányítópult** : az Azure portál eredményezne adatok gyári látni. Az adatok gyári létrehozása sikeresen befejeződött, miután az gyári lapon, amely jelzi, hogy az adatok gyári tartalmának látni.
    ![Adatok Factory kezdőlapján](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Hozzon létre egy csatolt Azure SQL-szolgáltatás  
Miután létrehozta a data factory, hozzon létre egy csatolt Azure SQL-szolgáltatás, amely az Azure SQL-adatbázis csatolása az adatok gyári. Az adatbázis tartalmaz a sampletable táblázat és sp_sample tárolt eljárás.

7.  Kattintson a **Szerző és üzembe** a **SProcDF** az adatok gyári szerkesztő indítása a az **Adatok gyári** lap.
2.  Kattintson az **új adatokat tárolja** a parancssávon, és válassza az **Azure SQL-adatbázis**. Meg kell jelennie a JSON parancsfájl hozhat létre egy csatolt Azure SQL-szolgáltatás szerkesztőben. 

    ![Új adatokat tároló](media/data-factory-stored-proc-activity/new-data-store.png)
4. A JSON parancsfájl a következő módosításokat: 
    1. Csere ** &lt;kiszolgálónév&gt; ** az Azure SQL-adatbázis kiszolgálójának nevét.
    2. Csere ** &lt;adatbázisnév&gt; ** a táblázat és a tárolt eljárás létrehozott adatbázis-kezelő.
    3. Csere ** &lt; username@servername ** a felhasználói fiókokkal együtt, amelynek hozzáférést az adatbázishoz.
    4. Csere ** &lt;jelszó&gt; ** és a felhasználói fiók jelszava. 

    ![Új adatokat tároló](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Kattintson a **központi telepítés** a csatolt szolgáltatás üzembe a parancssávon. Győződjön meg arról, hogy a AzureSqlLinkedService a bal oldali fastruktúrájú nézetben látható. 

    ![fanézet csatolt szolgáltatással](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Egy kimenet adatkészlet létrehozása
6. Kattintson a **... További** az eszköztáron kattintson az **Új adatkészlet**, és **Az SQL Azure**gombra. **Új adatkészlet** a parancssáv, és válassza **Az SQL Azure**.

    ![fanézet csatolt szolgáltatással](media/data-factory-stored-proc-activity/new-dataset.png)
7. Másolás és beillesztés a következő JSON parancsfájlt, a és a JSON-szerkesztőben.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Kattintson a **központi telepítés** a telepítéshez használni az adatkészlet parancssávon. Győződjön meg arról, hogy az adatkészlet belül a fastruktúra nézetben látható. 

    ![fanézet csatolt szolgáltatásokkal](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Hozzon létre egy folyamat SqlServerStoredProcedure tevékenység
Most SqlServerStoredProcedure tevékenységgel létrehozása egy folyamat.
 
9. Kattintson a **... További** parancs eszköztár, és kattintson az **új folyamat**. 
9. Másolás és beillesztés a következő JSON kódtöredékének. A **storedProcedureName** **sp_sample**értékűre. Név és a paraméter **DateTime** géphez egyeznie kell a nevét, és a tárolt eljárás definícióban paraméter géphez.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Ha a paraméter null a sikeres van szüksége, használja az alábbi: "Paraméter1": null (kisbetűssé). 
9. Kattintson a **központi telepítés** bevezetését tervezi a folyamat az eszköztáron.  

### <a name="monitor-the-pipeline"></a>A folyamat figyelése

6. Kattintson az **X** adatok gyári szerkesztő pengéit zárja be, és vissza szeretne térni az adatok gyári lap, és kattintson a **Diagram**gombra.

    ![diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. A **Diagram nézetben**című témakörben áttekintést a folyamatok, és ebben az oktatóanyagban használt adatkészleteket. 

    ![diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. A Diagram nézetben kattintson duplán az adatkészlet **sprocsampleout**. A szeletek kész állapotban látni. Nem lehet öt szeletek mivel szelet között a kezdési idő és befejezési idő a JSON az egyes Hour elő.

    ![diagram csempére](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Amikor egy szeletet **kész** állapotban van, futtassa a *sampletable *Jelölje ki* ** lekérdezést futtat, ellenőrizze, hogy az adatok szúrta be a táblázatba a tárolt eljárás Azure SQL-adatbázishoz.

    ![Kimeneti adatai](./media/data-factory-stored-proc-activity/output.png)

    [A folyamat Monitor](data-factory-monitor-manage-pipelines.md) talál információt az Azure Data Factory folyamatok figyelése.  

> [AZURE.NOTE] Ebben a példában a SprocActivitySample rendelkezik nincs bemeneti adatok alapján. Ha meg szeretné ezt a tevékenységet előtt tevékenység (Ez azt jelenti, hogy előzetes feldolgozása) lánc, a kimeneti értékeket a felsőbb szintű tevékenység is alkalmazható a tevékenységnek a bemeneti adatok alapján. Ebben az esetben ez a tevékenység nem hajtható végre mindaddig, amíg a felsőbb szintű tevékenység befejeződött, és a kimeneti értékeket a felsőbb szintű tevékenységek is (a kész állapot). A ráfordítások közvetlenül a tárolt eljárás tevékenységhez paraméterként nem használhatók

## <a name="json-format"></a>JSON formázása
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON tulajdonságai

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
név | A tevékenység neve. | igen
Leírás | Mire használható a tevékenység leíró szöveget | nem
típus | SqlServerStoredProcedure | igen
bemeneti adatok alapján | Nem kötelező. Adja meg a bemeneti adatkészlet, ha kell lennie (a "Kész" állapot) elérhető a tárolt eljárás tevékenység futtatásához. A beviteli adatkészlet paraméterként a tárolt eljárás során nem felhasznált. Csak a tárolt eljárás tevékenység megkezdése előtt ellenőrizze a függőség szolgál. | nem
kimeneti értékeket | Meg kell adnia egy kimenet adatkészlet a tárolt eljárás tevékenységhez. Kimeneti adatkészlet Itt adhatja meg a tárolt eljárás tevékenység **ütemezése** (óránként, hetente, havonta, stb.). <br/><br/>A kimenet adatkészlet Azure SQL-adatbázis vagy az Azure SQL-adatraktár vagy SQL Server-adatbázishoz, amelyben a tárolt eljárás a futtatni kívánt hivatkozó **csatolt szolgáltatás** kell használnia. <br/><br/>A kimenet adatkészlet szolgálhat átadni az eredmény a tárolt eljárás későbbi, másik tevékenységeket (a[tevékenységek láncolás](data-factory-scheduling-and-execution.md#chaining-activities)) a folyamat feldolgozásának lehetőséget. Azonban Data Factory nem automatikusan írni a tárolt eljárás a kimenet a adatkészlet. Érdemes a tárolt eljárás, amely egy SQL-táblázat, amely a kimeneti adatkészlet mutat ír. <br/><br/>Egyes esetekben az kimeneti adatkészlet lehet egy **üres adatkészlet**, csak az adja meg a ütemezést, a tárolt eljárás tevékenység futtatásához használt. | igen
storedProcedureName | Adja meg a tárolt eljárás nevét a Azure SQL-adatbázis vagy az Azure SQL adatraktár, amely a csatolt szolgáltatás eredménytábla használó képviseli. | igen
storedProcedureParameters | Adja meg a tárolt eljárás paraméterek értékeit. Ha üres paraméter átadni, használja az alábbi: "Paraméter1": null (az összes kisbetű). Ha többet szeretne tudni a tulajdonságot a következő példa látható.| nem

## <a name="passing-a-static-value"></a>Statikus értéknek átadása 
Most nézzünk meg az "Forgatókönyv" nevű egy másik oszlop hozzáadása a dokumentum-minta nevű statikus értéket tartalmazó táblázat.

![Mintaadatok 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

A tárolt eljárás tevékenység most át a forgatókönyv paraméter és a értéket. A fenti minta typeProperties szakaszában az alábbi kódtöredékének néz ki:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

