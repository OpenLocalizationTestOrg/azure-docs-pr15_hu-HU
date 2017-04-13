<properties 
    pageTitle="Adatok áthelyezése és Azure SQL-adatbázis |} Microsoft Azure" 
    description="Megtudhatja, hogy miként adatokat adatbázisból Azure SQL Azure-adatok gyár használatával." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Adatok áthelyezése és az Azure SQL-adatbázis Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a adatbázisból Azure SQL adatok áthelyezése egy másik adattár a/való. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül. 

## <a name="supported-sources-and-sinks"></a>A támogatott adatforrások és mosdók
Lásd: [támogatott adatokat tárolja](data-factory-data-movement-activities.md#supported-data-stores-and-formats) táblázat listáját a támogatott források vagy mosdók a Másolás tevékenység adatokat tárolja. Áthelyezheti adatok bármilyen támogatott adatforrás adattár Azure SQL-adatbázishoz vagy Azure SQL-adatbázis bármilyen támogatott gyűjtő adattár.

## <a name="create-pipeline"></a>Folyamat létrehozása
Egy folyamat egy másolatot a tevékenységhez, amely áthelyezi Azure SQL-adatbázishoz/származó adatokat a különböző eszközök/API-khoz is létrehozhat.  

- Másolja a varázsló
- Azure portál
- Visual Studio
- Azure PowerShell
- .NET API
- REST API-VAL

Lépésenkénti útmutatást adunk a folyamat létrehozása egy másolatot a tevékenységhez különböző módokon [Másolás tevékenység oktatóprogram](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) talál.   

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely adatbázisból Azure SQL adatokat másolja a legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példák adnia minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható. Adatok másolása, Azure SQL-adatbázis és az Azure Blob-tárolóhoz mutatnak. Azonban adatokat másolt **közvetlenül** bármilyen forrásból valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure adatok gyár is lehet.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Minta: Adatok másolása Azure SQL-adatbázis Azure Blob

Azonos határozza meg a következő adatok gyári vállalkozások:

1. Csatolt szolgáltatás típusú [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. A beviteli [adatkészlet](data-factory-create-datasets.md) [AzureSqlTable](#azure-sql-dataset-type-properties)típusú. 
4. Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4. A [folyamat](data-factory-create-pipelines.md) [SqlSource](#azure-sql-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta-idősorok adatokat másolja (óránkénti, napi, stb.) az Azure SQL-adatbázis táblájához blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat.  

**Azure SQL csatolt szolgáltatás**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

A csatolt szolgáltatás tulajdonságbeállítások a listában az [Azure SQL-csatolt szolgáltatás](#azure-sql-linked-service-properties) szakaszban olvashat. 

**Azure Blob-tároló csatolt szolgáltatás**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

A témakör [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) a lista tulajdonságbeállítások csatolt szolgáltatást. 

**Az SQL Azure beviteli adatkészlet**

A minta feltételezi létrehozott táblázat "Táblanév" Azure SQL és az idő adatsor "timestampcolumn" című oszlop tartalmaz. 

"Külső" beállítást: "igaz" tájékoztatja a Azure Data Factory szolgáltatást, hogy az adatkészlet külső adatok gyári és az adatok gyári tevékenységet nem készül.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

Adatkészlet típus tulajdonságbeállítások a listában az [Azure SQL adatkészlet tulajdonságokat](#azure-sql-dataset-type-properties) szakaszban olvashat.  

**Azure Blob-kimeneti adatkészlet**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Adatkészlet típus tulajdonságbeállítások a listában az [Azure Blob-adatkészlet típusú tulajdonságok](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) szakaszban olvashat.  

**Másolás tevékenységeket használó csővezeték**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **SqlSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. Az SQL-lekérdezés a **SqlReaderQuery** tulajdonságban meghatározott kijelöli az adatok másolása az elmúlt óra.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

A példában a SqlSource **sqlReaderQuery** van megadva. A Másolás tevékenység fut az adatok beolvasása SQL Azure-adatbázis forrása ezt a lekérdezést. Másik lehetőségként adhatja meg a tárolt eljárás **sqlReaderStoredProcedureName** és **storedProcedureParameters** megadásával (Ha a tárolt eljárás paraméterek tart).

Ha nem ad meg sqlReaderQuery vagy a sqlReaderStoredProcedureName, az oszlopok az adatkészlet JSON struktúra részében definiált az Azure SQL-adatbázis futtassanak lekérdezés összeállítása szolgálnak. Példa: `select column1, column2 from mytable`. Ha az adatkészlet definition nincs szerkezetét, az összes oszlop kiválasztja az értékeket. 


Című szakasz [Sql forrás](#sqlsource) [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) SqlSource és BlobSink tulajdonságbeállítások listáját. 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Minta: Adatok másolása Azure Blob Azure SQL-adatbázishoz

A mintában a következő adatok gyári vállalkozások határozza meg:  

1.  Csatolt szolgáltatás [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)típusú.
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)típusú.
4.  A [folyamat](data-factory-create-pipelines.md) [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) és [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta másolatok idősorok adatok (óránkénti, napi, stb.) az Azure blob-a táblázatokat a Azure SQL-óránként adatbázis. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 


**Azure SQL csatolt szolgáltatás**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

A csatolt szolgáltatás tulajdonságbeállítások a listában az [Azure SQL-csatolt szolgáltatás](#azure-sql-linked-service-properties) szakaszban olvashat. 

**Azure Blob-tároló csatolt szolgáltatás**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

A témakör [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) a lista tulajdonságbeállítások csatolt szolgáltatást.

**Azure Blob-beviteli adatkészlet**

Adatok van kiválasztott az új blob óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját és nevét a blob-dinamikusan értékeli ki a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használ év, hónap és nap része a kezdési időpontot, és a fájlnevet használja a kezdési időpontot a óra részét. "külső": "igaz" beállítás tájékoztatja a Data Factory szolgáltatás az alábbi táblázat az adatok gyári mutató külső, és nem készül az adatok gyári tevékenységet.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

Adatkészlet típus tulajdonságbeállítások a listában az [Azure Blob-adatkészlet típusú tulajdonságok](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) szakaszban olvashat.

**Azure SQL-kimeneti adatkészlet**

A minta másolja az adatokat egy Azure SQL "táblanév" nevű táblázat. A táblázat, amely tartalmazhat Blob CSV-fájl várt SQL Azure-ugyanannyi oszlopot tartalmazó létrehozása Új sorok kerülnek be a táblába óránként. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Adatkészlet típus tulajdonságbeállítások a listában az [Azure SQL adatkészlet tulajdonságokat](#azure-sql-dataset-type-properties) szakaszban olvashat.

**Másolás tevékenységeket használó csővezeték**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **BlobSource** van állítva, és **SqlSink** **gyűjtő** típusának beállítása.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

Nézze meg az [Sql-gyűjtő](#sqlsink) szakasz és [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) SqlSink és BlobSource tulajdonságbeállítások listáját. 


## <a name="azure-sql-linked-service-properties"></a>Azure SQL csatolt szolgáltatás tulajdonságai
A mintákban használt csatolt szolgáltatás **AzureSqlDatabase** típusú adatok gyár egy Azure SQL-adatbázis csatolása. Az alábbi táblázat ismerteti a JSON elemek Azure SQL-csatolt szolgáltatásra leírását.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| típus | Meg kell a típusa tulajdonság: **AzureSqlDatabase** | igen |
| connectionString | Adja meg az Azure SQL-adatbázis példányához connectionString tulajdonság szükséges információkat. | igen |

> [AZURE.NOTE] Konfigurálja az adatbázis-kiszolgáló engedélyezni [a kiszolgáló elérésére Azure Services](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) [Azure SQL-adatbázis tűzfalon keresztül](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) . Emellett a másolandó adatok Azure SQL-adatbázishoz külső Azure többek között az adatok gyári átjáró a helyszíni adatforrásokból származó, ha konfigurálása a gép adatok Azure SQL-adatbázishoz küldő megfelelő IP-címtartományokat. 

## <a name="azure-sql-dataset-type-properties"></a>Azure SQL-adatkészlet típus tulajdonságai
A mintákban használt **AzureSqlTable** típusú adatkészletet egy Azure SQL-adatbázis táblájához ábrázolásához. 

Szakaszok és a rendelkezésre álló adatkészleteket meghatározásának tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.). 

A typeProperties szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. A következő tulajdonságokat az **typeProperties** szakaszban az adatkészlet **AzureSqlTable** típusú foglalja magában:

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| Táblanév | A tábla a csatolt szolgáltatás hivatkozó Azure SQL-adatbázis-példány nevét. | igen |

## <a name="azure-sql-copy-activity-type-properties"></a>Azure SQL tevékenység típusának Tulajdonságok másolása
Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirend érhetők el.

> [AZURE.NOTE] A Másolás tevékenység csak egy beviteli tart, és csak egy kimenet eredményez.

Minden tevékenység típusa azonban függenek tulajdonságok elérhető **typeProperties** szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók. 

Ha egy Azure SQL-adatbázisból származó adatok áthelyezni, a Másolás tevékenység **SqlSource**az adatforrás típusa meg. Hasonlóképpen ha adatok áthelyezni Azure SQL-adatbázishoz, meg gyűjtő típusa **SqlSink**a Másolás tevékenység. Ez a témakör SqlSource és SqlSink által támogatott tulajdonságok listáját tartalmazza. 

### <a name="sqlsource"></a>SqlSource

Másolás tevékenységet Ha a forrás típusú **SqlSource**, a következő tulajdonságokat érhetők el **typeProperties** szakasz:

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-lekérdezés karakterlánc. Példa: `select * from MyTable`.  | nem |
| sqlReaderStoredProcedureName | A tárolt eljárás, amely beolvassa az adatokat a forrástáblából neve. | A tárolt eljárás nevét. | nem |
| storedProcedureParameters | A tárolt eljárás paramétereinek. | Név/érték párokká. Nevek és a paraméterek géphez egyeznie kell a nevek és a tárolt eljárás paraméterek géphez. | nem |

Ha a **sqlReaderQuery** a SqlSource van megadva, a Másolás tevékenység az adatok beolvasása SQL Azure-adatbázis forrása fut ezt a lekérdezést. Másik lehetőségként adhatja meg a tárolt eljárás **sqlReaderStoredProcedureName** és **storedProcedureParameters** megadásával (Ha a tárolt eljárás paraméterek tart). 

Ha nem ad meg sqlReaderQuery vagy a sqlReaderStoredProcedureName, az adatkészlet JSON struktúra részében meghatározott oszlopokat használják-e a lekérdezések összeállításához (`select column1, column2 from mytable`) futtathatók az Azure SQL-adatbázis együtt. Ha az adatkészlet definition nincs szerkezetét, az összes oszlop kiválasztja az értékeket. 

> [AZURE.NOTE] **SqlReaderStoredProcedureName**használatakor van szüksége az adatkészlet JSON adhat meg a **táblanév** tulajdonság értékét. Vannak nem hajtja végre az alábbi táblázat bár ellenőrzések. 

### <a name="sqlsource-example"></a>Példa SqlSource

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**A tárolt eljárás meghatározása:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** támogatja a következő tulajdonságokat:

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Várja meg, hogy a köteg beillesztési művelet elvégzéséhez előtt időtúllépés történik meg időt. | időszak<br/><br/> Példa: "00: 30:00" (30 perc). | nem | 
| writeBatchSize | Adatok beillesztése az SQL-táblázat writeBatchSize elérésekor pufferelési méretét. | Egész szám (sorok száma)| Nem (alapértelmezett: 10000)
| sqlWriterCleanupScript | Adja meg a lekérdezés másolás tevékenység végrehajtani, hogy az adatok, egy adott szeletet törlődnek. További tudnivalókért lásd: [ismételhetőség szakaszban](#repeatability-during-copy). | Egy lekérdezés utasítást.  | nem |
| sliceIdentifierColumnName | Adja meg, hogy automatikusan létrejön szeletet azonosítójú, amellyel a egy adott szeletet esetén futtassa újra az adatok karbantartása kitöltéséhez másolás tevékenység oszlop nevét. További tudnivalókért lásd: [ismételhetőség szakaszban](#repeatability-during-copy). | Egy oszlop adattípusát binary(32) oszlop neve. | nem |
| sqlWriterStoredProcedureName | A tárolt eljárás nevét (frissítések/beillesztése) upserts adatokat a cél táblába. | A tárolt eljárás nevét. | nem |
| storedProcedureParameters | A tárolt eljárás paramétereket. | Név/érték párokká. Nevek és a paraméterek géphez egyeznie kell a nevek és a tárolt eljárás paraméterek géphez. | nem | 
| sqlWriterTableType | Adja meg a tárolt eljárás használandó táblázatnév típusát. Másolás tevékenység elérhetővé teszi az adatok áthelyezése a táblázat típusú temp táblázatban. Tárolt eljárás kód majd egyesíthet az adatokat másolt a meglévő adatokból. | A táblázatok típus nevét. | nem |

#### <a name="sqlsink-example"></a>Példa SqlSink

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>A céladatbázisban identitás oszlopai
Ez a témakör példa az adatok másolása nélkül azonosító oszlop forrástáblából azonosító oszlopot tartalmazó táblát. 

**Forrás adattáblázathoz:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**A táblázat cél:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Figyelje meg, hogy a cél táblához azonosító oszlop van-e. 

**Forrás adatkészlet JSON meghatározása**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Cél adatkészlet JSON meghatározása**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Figyelje meg, hogy a forrás- és célwebhelyek táblázatként különböző séma van (cél rendelkezik egy újabb oszlopot identitással). Ebben az esetben meg kell adni a **struktúra** tulajdonság nem tartalmazza az azonosító oszlopot cél adatkészlet definícióban. 

Ezután, feleltesse meg a célként megadott adatkészlet oszlopait a forrás adatkészlet oszlopokat. Lásd: [Oszlopok megfeleltetése minták](#column-mapping-samples) szakaszában példát. 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Írja be a hozzárendelés az SQL Server Azure SQL-adatbázis

Az említett a a [tevékenységekre vonatkozó adatok mozgását](data-factory-data-movement-activities.md) cikk másolás tevékenység forrástípus gyűjtése a következő lépés 2 megközelítés az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Amikor adatok áthelyezése és az SQL Azure, SQL server, Sybase következő megfeleltetések használt SQL-típustól .NET típusát, és ez fordítva is igaz.

A hozzárendelés megegyezik az SQL Server adatok típusát hozzárendelését ADO.NET.

| Az SQL Server adatbázismotort típusa | .NET-keretrendszer típusa |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| bináris | Byte] |
| bit | Logikai érték |
| karakter | Karakterlánc, karakter] |
| dátum | Dátum és idő |
| Dátum és idő | Dátum és idő |
| datetime2 | Dátum és idő |
| Datetimeoffset | DateTimeOffset |
| Decimális | Decimális |
| FILESTREAM attribútum (varbinary(max)) | Byte] |
| Lebegtetés | Dupla |
| kép | Byte] | 
| int | Int32 | 
| pénzt | Decimális |
| nchar | Karakterlánc, karakter] |
| ntext | Karakterlánc, karakter] |
| numerikus | Decimális |
| nvarchar | Karakterlánc, karakter] |
| valós | Egyetlen |
| ROWVERSION | Byte] |
| smalldatetime | Dátum és idő |
| smallint | Int16 |
| megjelenítésekor a rendszer | Decimális | 
| sql_variant | Objektum * |
| szöveg | Karakterlánc, karakter] |
| idő | Időszak |
| időbélyeg | Byte] |
| tinyint | Bájt |
| UniqueIdentifier | Globálisan egyedi azonosítója |
| varbinary |  Byte] |
| varchar | Karakterlánc, karakter] |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.