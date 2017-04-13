<properties
   pageTitle="Töltse fel a nagy mennyiségű adatot offline módszerekkel adatok tó tárolóba |} Microsoft Azure"
   description="Adatok másolása Azure BLOB-tároló tó adattár AdlCopy eszközzel"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Tó adattár adatok offline másolatot Azure importálás – exportálás szolgáltatás használata

Ebben a cikkben ismerkedhet arról, hogy miként nagyon nagy adathalmazok másolása (> 200GB) tárolóba az Azure adatok tó módszerekkel offline másolatot, például [Azure importálás/exportálás szolgáltatás](../storage/storage-import-export-service.md). A fájl szerepel példaként, a jelen cikkben használt többek között a 339,420,860,416 bájt lemezen tehát körülbelül 319GB. Vegyük hívja fel a fájlt 319GB.tsv.

Azure importálás/exportálás szolgáltatáson keresztül biztonságosan átadása szerint a merevlemez-meghajtók az Azure adatközpontja szállítási Azure blob-tárolóhoz nagy mennyiségű adatot.

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure tárterület-fiókot**.

- **Azure adatok tó Analytics-fiókot (nem kötelező)** – lásd: [első lépések az Azure adatok tó Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) módjáról tó adattár fiók létrehozása.


## <a name="preparing-the-data"></a>Az adatok előkészítése

Az importálás/exportálás szolgáltatással előtt azt kell oldaltörés az adatfájl átvitt **be a példányszámot, amelyek 200 GB-nál kisebb** méretű legyen. Ennek az oka az importálási eszköz 200GB-nál nagyobb fájlokkal nem működik. Az ahhoz, hogy az oktatóprogram azt a fájlt felosztása mennyiségű 100GB bájt. Ezt megteheti [Cygwin](https://cygwin.com/install.html)egyszerűen használatával. Cygwin Linux parancs, amely lehetővé teszi, hogy a felhasználók könnyen ehhez támogatja. A lehetőséget választja akkor a következő parancsot használja.

    split -b 100m 319GB.tsv

A felosztott művelet a nevét, az alábbi hozza létre a fájlokat.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Az adatok felkészítése lemez

Kövesse a [Azure importálás/exportálás szolgáltatás használata](../storage/storage-import-export-service.md) (csoportban a **Felkészülés a meghajtókon** ) a merevlemez-meghajtók előkészítése. Felkészülés a meghajtó módjáról az alábbiakban a teljes folyamat.

1. A megkövetelt Auzre importálás/exportálás szolgáltatás használandó merevlemez beszerezni.

2. Azonosítsa az adatok leendő helyének Azure tároló fiók egyszer vágólapra másolt si Azure adatközpontja teljesült.

3. Használja az [Azure importálás/exportálás eszköz](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), egy parancssori segédprogramot. Íme egy példa kódtöredékének az eszköz használatát.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Lásd: [Azure importálás/exportálás szolgáltatás használata](../storage/storage-import-export-service.md) az **Azure importálás/exportálás eszköz**használatáról további minta kódrészletek.

4. A fenti parancs létrehoz egy lapjának fájlt a megadott helyen. A napló fájl segítségével lesz az importálási feladat létrehozása az [Azure klasszikus portálon](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Az importálási feladat létrehozása

Most már létrehozhat egy importálás leírtak [Azure importálás/exportálás szolgáltatás használata](../storage/storage-import-export-service.md) a (szakaszban a **Létrehozás az importálás** ). Projekt importálása egyéb részleteivel is adja meg a napló fájlrendszer lemezmeghajtók előkészítése során létre.

## <a name="physically-ship-the-disks"></a>A lemez fizikailag kiszállítása

Is most fizikailag kiszállítása a lemezt az Azure adatközpont, ahol az adatokat másolja a rendszer az Azure tároló BLOB létrehozása az importálás során a megadott. Is, létrehozásakor a feladatot, ha azt választotta, később a nyomon követés adatokat, is most térjen vissza az importálás és frissíti a nyomon követés számot.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Adatok másolása Azure BLOB-tároló Azure tó adattárhoz

Miután az importálási feladat az Állapot oszlopban a kész, ellenőrizheti, hogy az adatok érhető el az Azure tároló BLOB volna megadott. Többféle módszerrel helyezhetők át az adatokat a tárhely Azure BLOB Azure tó adattár használhatja. Az összes rendelkezésre álló beállítások feltöltéshez olvassa el a [Ingesting adatok tó tárba](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store)című témakört.

Ebben a részben biztosítunk Önnek a JSON-definíciók létrehozása az Azure Data Factory folyamat az adatok másolása használható. A JSON definíciókat [Azure portál](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) , illetve az [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)is használhatja.

### <a name="source-linked-service-azure-storage-blob"></a>Adatforrás kapcsolódó Service (a tárhely Azure Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Megcélozhat csatolt szolgáltatás (Azure tó adattár)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>A bemeneti adatok megadása
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Kimeneti adatok megadása
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>A folyamat (Másolás tevékenység)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
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
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Azure Data Factory használatát az adatok áthelyezése az Azure Blob-tároló és Azure tó adattár további tudnivalókért olvassa el a [Azure tároló Blob Azure adatok tó áruház Azure Factory-adatok használata az adatok áthelyezése](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)című témakört.

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Az adatfájlok az Azure tó adattár állítania

Egy fájl, 319GB méretű és meghibásodott azt kisebb méretű fájlok be, hogy az Azure importálás/exportálás szolgáltatással továbbítható lépések azt. Most, hogy az adatok Azure tó adattár van, azt is reconstrcut a fájl eredeti méretének visszaállításához. Erre az alábbi Azure PowerShell cmldts is használhatja.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Következő lépések

- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
