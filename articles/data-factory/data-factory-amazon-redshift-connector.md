<properties 
    pageTitle="Az adatokat áthelyezheti az Amazon Redshift Data Factory használatával |} Microsoft Azure" 
    description="Tudnivalók az adatokat áthelyezheti az Amazon Redshift Azure Data Factory használatával." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Adatok az Amazon Redshift Azure Data Factory használatával áthelyezése

Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a Ha az adatokat az Amazon Redshift egy másik adatok tárolására. Ez a cikk az [elmozdulásnak a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk bemutatja az adatok mozgás egy általános áttekintése másolás tevékenység és a forrás/gyűjtő adatokat tárolja listája épül.  

Adatok gyári jelenleg csak mozgó adatainak Amazon Redshift egyéb adatokat tárolja, de nem az adatok áthelyezése más adatokat tárolja a Amazon Redshift támogatja.

## <a name="prerequisites"></a>Előfeltételek

- Ha a helyszíni adatok tárolóhoz adatok áthelyezni, hozzáférést adatkezelési átjáró (IP-cím használata a gép) az Amazon Redshift fürt. [Hozzáférés engedélyezése a fürtre](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) talál utasításokat. 
- Ha adatokat egy Azure adatokat tároló helyez át, olvassa el a [Azure adatok központ IP-tartományokat](https://www.microsoft.com/download/details.aspx?id=41653) a kiszámítania IP-címtartományok (beleértve a tartományokra SQL) a Microsoft Azure adatközpontokban által használt. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely az Amazon Redshift lévő adatokat másolja a legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példa a minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható biztosít. Azt mutatja be Amazon Redshift Azure Blob-tárolóhoz adatok másolása. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Minta: Adatok másolása Amazon Redshift Azure Blob
Ez a példa bemutatja, hogyan Amazon Redshift adatbázisból származó adatok másolása Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

- Csatolt szolgáltatás típusú [AmazonRedshift](#linked-service-properties).
- Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- A beviteli [adatkészlet](data-factory-create-datasets.md) [RelationalTable](#dataset-type-properties)típusú.
- Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
- A [folyamat](data-factory-create-pipelines.md) [RelationalSource](#copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta adatokat másolja a lekérdezés eredményét az Amazon Redshift blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

**Csatolt amazon Redshift szolgáltatás**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
            }
        }
    }


**Azure csatolt tárhelyszolgáltatáshoz**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Amazon Redshift beviteli adatkészlet**

Beállítás **"külső": igaz** tájékoztatja a Data Factory szolgáltatás, az adatkészlet adatok gyári mutató külső, és az adatok gyári tevékenységet nem készül. A beviteli adatkészlet nem egy tevékenységet a során által létrehozott igaz meg ezt a tulajdonságot.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


**Azure Blob-kimeneti adatkészlet**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Másolás tevékenységeket használó csővezeték**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **RelationalSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. Az SQL-lekérdezés a **lekérdezés** tulajdonságban meghatározott adatok másolása az elmúlt órában kijelölése
    
    {
        "name": "CopyAmazonRedshiftToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonRedshiftInputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Csatolt szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt Amazon Redshift szolgáltatásra JSON elemek leírását.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| típus | Meg kell a típusa tulajdonság: **AmazonRedshift**. | igen |
| kiszolgáló | IP cím vagy állomásnév az Amazon Redshift kiszolgáló neve. | igen |
| port | Az Amazon Redshift kiszolgáló meghallgatásához az ügyfél-kapcsolatot használó TCP port száma. | Nem, alapértelmezett érték: 5439 |
| adatbázis | Az Amazon Redshift adatbázis nevét. | igen |
| felhasználónév | Felhasználó, aki hozzáfér az adatbázis nevét.| igen |
| jelszó | A felhasználói fiók jelszava.| igen |


## <a name="dataset-type-properties"></a>Adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A **typeProperties** szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az typeProperties szakaszban adatkészlet **RelationalTable** (amely tartalmazza az Amazon Redshift adatkészlet) típusú rendelkezik az alábbi tulajdonságok

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| Táblanév | Az Amazon Redshift adatbázis szolgáltatás csatolt tábla neve hivatkozik. | Nem (Ha nincs megadva a **lekérdezés** **RelationalSource** ) | 

## <a name="copy-activity-type-properties"></a>Tevékenység típusának tulajdonságainak másolása

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirendek érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető **typeProperties** szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

Adatforrás másolása tevékenység típusú **RelationalSource** (amely tartalmazza az Amazon Redshift) esetén a következő tulajdonságokat érhetők el typeProperties szakasz:

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------------- | -------- |
| lekérdezés | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-lekérdezés karakterlánc. Példa: válassza a * from tábla. | Nem (Ha a **táblanév** **adatkészletből** nincs megadva) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Az Amazon Redshift leképezésének

A [tevékenységekre vonatkozó adatok mozgás](data-factory-data-movement-activities.md) a cikkben említett, a Másolás tevékenység forrástípus gyűjtése a következő két lépésből álló módszert az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Adatok áthelyezése a Amazon Redshift, amikor a következő hozzárendelések használandó az Amazon Redshift típusok .NET típusokat.

Amazon Redshift típusa | .NET-alapú típusa
-------------------- | ---------------
SMALLINT | Int16
EGÉSZ SZÁM | Int32
BIGINT | Int64
DECIMÁLIS | Decimális
VALÓS | Egyetlen
DUPLA PONTOSSÁG | Dupla
LOGIKAI ÉRTÉK | Karakterlánc
KARAKTER | Karakterlánc
VARCHAR | Karakterlánc
DÁTUM | Dátum és idő
IDŐBÉLYEG | Dátum és idő
SZÖVEG | Karakterlánc



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.

## <a name="next-steps"></a>Következő lépések
Az alábbi cikkekben talál: 
- [Másolás tevékenység oktatóprogram](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletes útmutatást a folyamat létrehozása egy másolatot a tevékenységhez. 