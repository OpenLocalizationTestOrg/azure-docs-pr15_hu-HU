<properties
    pageTitle="Adatok áthelyezése egy helyszíni SQL Server Azure Data Factory az SQL Azure |} Azure"
    description="Állítson be egy ADF folyamat, amely a két közös helyezze át az adatokat naponta adatbázis helyszíni és felhőbeli közötti adatok áttelepítési tevékenységek composes."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Adatok áthelyezése egy helyszíni SQL server Azure Data Factory az SQL Azure

Ez a témakör bemutatja az Azure adatok gyári (ADF használata) áthelyezése adatok egy helyszíni SQL Server-adatbázisból egy SQL Azure-adatbázis Azure Blob-tárolóhoz keresztül.

A következő **menü** tartalmazó témakörökre mutató hivatkozások ismertetik, hogyan lehet adatok ingest cél környezetekben, ahol tárolt és feldolgozása az adatok tudományos során a az adatokat.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Bevezetés: Mi az ADF, és mikor kell azt használni át az adatok?

Azure Data Factory orchestrates és automatizálja a mozgás és az adatok átalakítása teljes körű felügyelt felhőalapú adatok integrációs szolgáltatás. A fő a ADF modell fogalma folyamat. Egy folyamat tevékenységek logikai csoportja, amelyek határozza meg, hogy a műveletek elvégzésére adatkészleteket lévő adatok. Kapcsolt szolgáltatások segítségével határozza meg az adatok gyári csatlakozhat az adatok erőforrások szükséges adatokat.

Lapadagoló az adatok folyamatok, amelyek rendkívül elérhető és felügyelt a felhőben meglévő adatkezelési szolgáltatásokra állhat. Ezek az adatok folyamatok ütemezhető ingest, előkészítése, átalakítás, elemezheti és adatok közzététele és ADF kezeli, és a komplex adatok és a feldolgozás függőségek orchestrates. Megoldások is gyorsan beépített, és csatlakozás helyszíni egyre növekvő számú rendszerbe a felhőben, és felhőbeli adatforrások.

Fontolja meg inkább ADF:

- Ha adatokat kell folyamatosan áttelepített hibrid helyzetben, amely fér hozzá a helyszíni és felhőbeli is erőforrások 
- Ha az adatok feldolgozva van, vagy módosítani kell, és hozzáadja azt, amikor az áttelepítendő üzleti logikával rendelkeznek. 

ADF lehetővé teszi, hogy az ütemezés, és a feladatok parancsfájlokkal egyszerű JSON kezelő adatok rendszeres időközönkénti mozgásának nyomon követése. ADF szintén egyéb lehetőségek, például összetett műveletek támogatása. ADF kapcsolatos további tudnivalókért dokumentációjában olvasható az [Azure adatok gyári (ADF)](https://azure.microsoft.com/services/data-factory/)elemre.


## <a name="scenario"></a>Az alkalmazási példát

Egy ADF folyamat, amely a két áttelepítési a tevékenységekre vonatkozó adatok composes beállítása azt. Közös azok helyezze át az adatokat naponta egy helyszíni SQL-adatbázis és a felhőben az Azure SQL-adatbázis között. A két tevékenység a következők:

* Azure Blob-tárolóhoz fiók helyszíni SQL Server-adatbázisból származó adatok másolása
* másolja az adatokat az Azure Blob-tárolóhoz fiókból Azure SQL-adatbázis.

>[AZURE.NOTE] Itt lett igazítani, az a ADF csoport által megadott részletesebb oktatóprogram a bemutatott lépések: [adatok helyszíni adatforrások és az adatkezelési átjáró a felhőbeli közötti áthelyezése a](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) hivatkozásokat, hogy a témakör a vonatkozó szakaszokat is közöljük, szükség esetén.


## <a name="prereqs"></a>Előfeltételek
Ebben az oktatóanyagban feltételezi, hogy van:

* **Azure előfizetés**. Ha nem rendelkezik az előfizetést, jelentkezzen az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
* **Azure tárterület-fiókot**. Az adatok tárolásának ebben az oktatóanyagban Azure tároló fiókot használ. Ha nincs Azure tárterület-fiókkal, ismertető [tároló fiók létrehozása](storage-create-storage-account.md#create-a-storage-account) . Miután létrehozta a tárterület-fiókot, kell szerezze be a fiókkulcs, amellyel elérhető a tár. Lásd: a [nézet, a másolás és a hívóbetűk újragenerálása tárhelyet](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Az **Azure SQL-adatbázis**eléréséhez. Be kell állítania egy Azure SQL-adatbázissal, ha az [Első lépések a Microsoft Azure SQL-adatbázis](../sql-database/sql-database-get-started.md) tpoic ismertetése kiépítését Azure SQL-adatbázis egy új példányát.
* Telepített és konfigurált **Azure PowerShell** helyi meghajtóra. Útmutatásért lásd: [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).

> [AZURE.NOTE] Ez az eljárás a [Azure portálon](https://portal.azure.com/)használja.


##<a name="upload-data"></a>Töltse fel az adatokat a helyszíni SQL Server

A [következőt: Taxi adatkészlet](http://chriswhong.com/open-data/foil_nyc_taxi/) segítségével bemutatják az áttelepítési folyamatot. A következőt: Taxi adatkészlet érhető el, amint azt, hogy a bejegyzést, a Azure blob-tároló [Adatok a következőt: Taxi](http://www.andresmh.com/nyctaxitrips/). Az adatok két fájlokat, a trip_data.csv, utazást részleteit tartalmazó és trip_far.csv fájlt, a jegy ára minden út kifizetett részleteit tartalmazó tartalmaz. A minta és a leírás ezeket a fájlokat a [Következőt: Taxi utakat adatkészlet leírás](machine-learning-data-science-process-sql-walkthrough.md#dataset)találhatók.


Alkalmazkodás az eljárás használatával egy a saját adatain közöljük, vagy kövesse a lépéseket a következőt: Taxi adatkészlet használatával leírt módon. A helyszíni SQL Server-adatbázisba a következőt: Taxi adatkészlet feltöltéséhez hajtsa végre a [Tömeges adatok beolvasása SQL Server-adatbázisba](machine-learning-data-science-process-sql-walkthrough.md#dbload)című témakörben ismertetett lépéseket. Ezeket az utasításokat egy SQL Server-Azure virtuális gép, de az eljárás a helyszíni SQL Server feltöltése nem azonos.


##<a name="create-adf"></a>Hozzon létre egy Azure Data Factory

Az utasításokat egy új Azure Data Factory és erőforráscsoport létrehozása az [Azure portál](https://portal.azure.com/) [létrehozása az Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory)állnak rendelkezésre. Az új ADF példány *adfdsp* és az erőforrás *adfdsprg*létrehozott csoport nevét.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Telepítse és állítsa be az adatkezelési átjáró felfelé

Ahhoz, hogy a folyamatok a dolgozni egy helyszíni SQL Server-Azure adatok gyári, kell csatolt szolgáltatás hozzáadása az adatok gyári. Hozzon létre egy csatolt szolgáltatás egy helyszíni SQL Server, a következőket kell tennie:

- Töltse le és telepítse a Microsoft adatkezelési átjáró a helyszíni számítógépre. 
- a csatolt szolgáltatás használata az átjáró a helyszíni adatforrás konfigurálása. 

Az adatkezelési átjáró serializes, és a forrás- és gyűjtő adatok a számítógépen a jelenlegi deserializes.

Beállítási útmutató és részletes tudnivalókat az adatkezelési átjáró című témakörben talál [az adatok helyszíni adatforrások és az adatkezelési átjáró a felhőbeli közötti áthelyezése](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Hozzon létre csatolt szolgáltatást való csatlakozáshoz a adatforrásai

Csatolt szolgáltatás határozza meg, hogy az Azure Data Factory adatok erőforrás csatlakozhat szükséges adatokat. [Hozzon létre csatolt szolgáltatást](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services)lépésről lépésre hozhat létre csatolt szolgáltatások megadni.

Három erőforrások ebben az esetben, amelynek csatolt szolgáltatások van szükség van.

1. [Csatolt helyszíni SQL Server szolgáltatást](#adf-linked-service-onprem-sql)
2. [Csatolt szolgáltatás Azure Blob-tárolóhoz](#adf-linked-service-blob-store)
3. [Csatolt szolgáltatás Azure SQL-adatbázis](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Csatolt szolgáltatás helyszíni SQL Server-adatbázishoz

A csatolt a helyszíni SQL Server szolgáltatást létrehozása:

- Kattintson az Azure klasszikus portálon ADF céloldal **Adattárhoz** 
- Jelölje ki az **SQL** , és írja be a *felhasználónév* és *jelszó* hitelesítő adatokat a helyszíni SQL Server. Adja meg a kiszolgálónév egy **teljesen minősített kiszolgálónév fordított perjel példány nevét (kiszolgáló_neve\példány_neve)**van szükség. A csatolt szolgáltatás *adfonpremsql*neve.

###<a name="adf-linked-service-blob-store"></a>Csatolt Blob szolgáltatás

A csatolt szolgáltatás az Azure Blob-tárolóhoz fiók létrehozása:

- Kattintson az Azure klasszikus portálon ADF céloldal **Adattárhoz**
- Jelölje ki az **Azure tárterület-fiók** 
- Adja meg az Azure Blob-tárolóhoz kulcs és tároló fióknév. A csatolt szolgáltatás *adfds*neve.

###<a name="adf-linked-service-azure-sql"></a>Csatolt szolgáltatás Azure SQL-adatbázis

A csatolt szolgáltatást az Azure SQL-adatbázis létrehozása:

- Kattintson az Azure klasszikus portálon ADF céloldal **Adattárhoz**
- Jelölje ki az **Azure SQL** , és írja be a *felhasználónév* és *jelszó* hitelesítő adatokat a Azure SQL-adatbázishoz. Kell megadni a *felhasználónevet* *user@servername*.   


##<a name="adf-tables"></a>Határozza meg, és megadhatja, hogyan érhető el a adatkészleteket táblázatok létrehozása

Hozzon létre a struktúra, helyét és elérhetőségét a adatkészletek adja meg az alábbi eljárások parancsprogram-alapú táblákhoz. A táblák definiálása JSON fájlok szolgálnak. Ezek a fájlok felépítésének kapcsolatos további tudnivalókért olvassa el a [adatkészleteket](../data-factory/data-factory-create-datasets.md)című témakört.

> [AZURE.NOTE]  Végre kell hajtani a `Add-AzureAccount` parancsmag végrehajtása a [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) parancsmag győződjön meg arról, hogy be van-e jelölve a jobb oldali Azure előfizetés a parancs végrehajtása előtt. Ezzel a parancsmaggal dokumentációja [Hozzáadás-AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx)című témakör tartalmaz.

A JSON-alapú definíciók a táblázatokban a következő nevek használata:

* a **táblázat neve** a helyszíni SQL server *nyctaxi_data*
* a **tároló neve** Azure Blob-tárolóhoz fiók *containername*  

Három tábla definíciók van szükség az e ADF folyamat:

1. [SQL-e helyszíni tábla](#adf-table-onprem-sql)
2. [BLOB-táblázat](#adf-table-blob-store)
3. [Az SQL Azure-táblából](#adf-table-azure-sql)

> [AZURE.NOTE]  Ezeket a műveleteket Azure PowerShell-lel való definiálása és ADF tevékenységeket hoz létre. De az alábbi műveleteket is végrehajthat az Azure portálon. A részletekért olvassa [létrehozása a bemeneti és kimeneti adatkészleteket](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>SQL-e helyszíni tábla

A tábla definícióját a helyszíni SQL Server van megadva, a következő JSON-fájl:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

Az oszlopnevek nem szerepel itt. Rész kijelölhet oszlopnevei csoportnaptárba itt (a részletek jelölőnégyzetet a [ADF dokumentáció](../data-factory/data-factory-data-movement-activities.md ) témakört.

A táblázat JSON definícióját másolja egy *onpremtabledef.json* fájl nevű fájlt, és mentse egy ismert helyre (Itt-nek tekinti *C:\temp\onpremtabledef.json*). Hozzon létre a táblázatot az ADF a Azure PowerShell-parancsmagot:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>BLOB-táblázat
A következő (képez le Azure blob-e helyszíni j.leny adatainak) szerepel-e a tábla, a kimeneti blob-hely meghatározása:

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

A táblázat JSON definícióját másolja egy *bloboutputtabledef.json* fájl nevű fájlt, és mentse egy ismert helyre (Itt-nek tekinti *C:\temp\bloboutputtabledef.json*). Hozzon létre a táblázatot az ADF a Azure PowerShell-parancsmagot:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>Az SQL Azure-táblából
A táblázat az SQL Azure-definícióját kimeneti az alábbi (Ez a séma rendeli hozzá a blob az adatok):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

A táblázat JSON definícióját másolja egy *AzureSqlTable.json* fájl nevű fájlt, és mentse egy ismert helyre (Itt-nek tekinti *C:\temp\AzureSqlTable.json*). Hozzon létre a táblázatot az ADF a Azure PowerShell-parancsmagot:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Határozza meg, és a folyamat létrehozása

Adja meg, amelyek a folyamat tartoznak, és hozzon létre a folyamat az alábbi eljárások parancsprogram-alapú tevékenységek. A folyamat tulajdonságainak megadása JSON fájl használják.

* A parancsprogram feltételezi, hogy **neve csővezeték** *AMLDSProcessPipeline*.
* Tartsa szem előtt, hogy be van állítva a naponta végre kell hajtani, és a feladat (12 óra UTC szerint) az alapértelmezett végrehajtási idő használata során gyakorisága.

> [AZURE.NOTE]Az alábbi eljárások Azure PowerShell-lel való meghatározása és a ADF során létre. De ha a feladat végrehajtásához az Azure portálon is elvégezhető. A részletekért olvassa [létrehozása és futtatása a folyamat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

A korábban megadott táblázat definíciók használ, a ADF folyamat definíciója van megadva az alábbi képlettel történik:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Ez a folyamat JSON meghatározása másolja egy *pipelinedef.json* fájl nevű fájlt, és mentse egy ismert helyre (Itt-nek tekinti *C:\temp\pipelinedef.json*). A folyamat az alábbi Azure PowerShell-parancsmag a ADF létrehozása:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Győződjön meg arról, hogy megjelenik a ADF az Azure klasszikus portálon a folyamat, jelenik meg, a következő (kattintáskor a diagram)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>A folyamat elindítása
A folyamat futtatható a következő parancsot:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

A *Kezdődátum* és a *Záródátum* paraméterértékeket kell a tényleges dátum, amelyek között, amelyet a folyamat futtatása kell cserélni.

A folyamat végrehajt, amikor láthatja, hogy az adatok jelenjenek meg a tároló kijelölt blob, naponta egy fájlt a.

Figyelje meg, hogy azt van nem kapcsolatos károkozásra által biztosított funkcionalitás ADF függőleges vonás adatok elő. Található ez, és más funkciók ADF által biztosított további tudnivalókért lásd: a [ADF dokumentációt](https://azure.microsoft.com/services/data-factory/).
