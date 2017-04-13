<properties
   pageTitle="Az Azure Data Factory adat betöltése |} Microsoft Azure"
   description="Ismerje meg, hogyan az Azure Data Factory adat betöltése"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Adatok betöltése az Azure Data Factory 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Adatok gyári](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

Ebből az oktatóanyagból megtudhatja, hogy miként, egy folyamat létrehozása az Azure adatok gyár adatok Azure Blob-tároló Azure SQL-adatraktár helyezhetők át. Az alábbi lépésekkel lesz:

+ Állítsa be a mintaadatokat egy tároló Azure Blob.
+ Csatlakozás erőforrások Azure Data Factory.
+ Hozzon létre egy folyamat tároló BLOB adatok áthelyezése SQL adatraktár.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Első lépések

Hogy megismerkedjen Azure Data Factory, című témakörben [Azure Data Factory][].

### <a name="create-or-identify-resources"></a>Hozzon létre vagy az erőforrások azonosítása

Mielőtt hozzákezdene ebben az oktatóanyagban, az alábbi forrásokban van szükség:

   + **Azure Blob-tároló**: az Azure Data Factory folyamat ebben az oktatóanyagban használ az adatforrásként az Azure Blob-tároló, és így egy érhető el, a mintaadatok tárolására van szükség. Ha nem egy már, megtudhatja, hogyan kell a [tároló fiók létrehozása][].

   + **SQL-adatraktár**: Ebben az oktatóanyagban lép az adatokat az Azure Blob-tároló SQL adatraktár, és így szükség van egy adatraktár online AdventureWorksDW mintaadatokkal az betöltött. Ha még nem rendelkezik egy adatraktár, megtudhatja, hogyan hozhatók létre [egyet][Create a SQL Data Warehouse]. Ha egy adatraktár van, de nem kiépítése azt a mintaadatokkal, akkor [Töltse be manuálisan][Load sample data into SQL Data Warehouse].

   + **Azure Data Factory**: Azure Data Factory befejeződik a tényleges betöltés, és így szükség van egy, az adatok mozgását folyamat létrehozásához használható. Ha nem egy már, megtudhatja, hogy miként hozhat létre egyet az 1 az [első lépések az Azure Data Factory (adatok Factory-szerkesztőben)][].

   + **AZCopy**: a mintaadatok másolása a helyi ügyfél az Azure tároló Blob AZCopy van szüksége. Telepítés című cikkben olvashat a [AZCopy dokumentációt][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Lépés: 1: Azure Blob-tároló mintaadatok másolása

Ha már készen áll az összes csatolt, készen áll másolja az Azure tároló Blob mintaadatokat.

1. [Töltse le a mintaadatokat][]. Ezeket az adatokat egy másik három év értékesítési adatok felvétele a AdventureWorksDW mintaadatokat.

2. A AZCopy paranccsal három év adatok másolása az Azure tároló Blob.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Lépés: 2: Azure Data Factory erőforrások csatlakoztatása

Most, hogy az adatok helyen van az Azure adatok gyár prognózist az Azure blob-tárolóhoz az adatok áthelyezése SQL adatraktár létrehozunk.

Első lépésként nyissa meg az [Azure-portálra][] , és a bal oldali menüben válassza az adatok gyár.

### <a name="step-21-create-linked-service"></a>Lépés 2.1: Csatolt szolgáltatás hozzon létre

Hivatkozás a Azure tárhely és a SQL adatraktár az adatok gyári.  

1. Először a regisztrációs folyamat megkezdéséhez kattintson az adatok gyári a "Csatolt szolgáltatások" című szakaszát, és válassza az "új adatokat tárolja." Válasszon egy nevet az azure tároló csoportban válassza az Azure tároló regisztrálhatja a típus, és írja be a fiók nevét, és a Fiókkulcs.

2. Regisztrálni SQL adatraktár nyissa meg a "Szerző és Deploy" csoportban jelölje be az "Új adatokat tároló", majd a "Azure SQL-adatraktár". Másolja és illessze be ezt a sablont, és töltse ki a konkrét adatok.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>2.2 lépés: Az adatkészlet meghatározása

Miután létrehozta a csatolt szolgáltatások, felkínálunk adatkészletek határozza meg.  Itt Ez azt jelenti, definiáló helyezi át a tárhelyről a adatraktár az adatok struktúrájára.  Erről további létrehozásával kapcsolatban

1. Indítsa el a folyamathoz navigálással az adatok gyár "Szerző és Deploy" szakaszához.

2. Kattintson az "Új adatkészlet" és "Azure Blob-tárolóhoz" a tárhely csatolása az adatok gyári.  Használható az Azure Blob-tárolóban lévő adatok megadását parancsfájl alatt:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
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
    ```

3. Most már azt határozza meg az adatkészlet SQL adatraktár. Azt először ugyanúgy, kattintás az "Új adatkészlet" és "Azure SQL-adatraktár".

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Lépés 3: Létrehozása, és a folyamat futtatása

Végezetül azt állíthatja be, és futtassa a folyamat az Azure Data Factory.  Ez a művelet befejezése a tényleges adatok mozgását.  A műveleteket is elvégezheti az SQL adatraktár és Azure Data Factory teljes egészében talál [Itt][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

A "Szerző és Deploy" csoportban kattintson a "További parancsok" és "Új folyamat".  Miután létrehozta a folyamat, használhatja a kódot a adatraktár nem ruházhatja át az adatokat az alábbi:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Következő lépések

Ha többet szeretne, először megtekintése:

- [Azure Data Factory tanulási javaslat][].
- [Azure SQL-adatok raktári összekötő][]. Ez az Azure Data Factory a való Azure SQL-adatraktár core témakör.


Az alábbi témakörök nyújtanak információkat Azure Data Factory. Azokat a különböző Azure SQL-adatbázis vagy HDInsight, de az adatok Azure SQL-adatraktár is érinti.

- [Oktatóprogram: első lépések az Azure Data Factory][] Az adatok az Azure Data Factory feldolgozása az core oktatóprogram: az. Ebben az oktatóprogramban az első folyamat átalakítás és elemzése havonta webes naplók HDInsight használó fog generál. Ne feledje, nincs nincs másolás tevékenység ebben az oktatóanyagban.
- [Oktatóanyag: adatok másolása az Azure Blob-tároló az Azure SQL-adatbázis][]. Ebben az oktatóanyagban hozzon létre egy folyamat az Azure Data Factory adatok másolása Azure Blob-tároló Azure SQL-adatbázishoz.

<!--Image references-->

<!--Article references-->
[AZCopy dokumentáció]: ../storage/storage-use-azcopy.md
[Az SQL Azure-adatok raktári összekötő]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Tárterület-fiók létrehozása]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Első lépések az Azure Data Factory (adatok Factory-szerkesztőben)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Azure Data Factory – bevezetés]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Oktatóanyag: Adatok másolása Azure Blob-tároló Azure SQL-adatbázishoz]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Oktatóprogram: Azure Data Factory – első lépések]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory tanulási javaslat]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portál]: https://portal.azure.com
[Mintaadatok letöltése]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
