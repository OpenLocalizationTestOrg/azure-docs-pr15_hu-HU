<properties 
    pageTitle="Az adatokat áthelyezheti az Amazon egyszerű Tárhelyszolgáltatáshoz Data Factory használatával |} Microsoft Azure" 
    description="Megismerheti az Azure Data Factory használatával Amazon egyszerű Tárhelyszolgáltatáshoz (S3) adatainak áthelyezése." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Adatok az Amazon egyszerű Tárhelyszolgáltatáshoz Azure Data Factory használatával áthelyezése

Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a Ha az adatokat az Amazon egyszerű Tárhelyszolgáltatáshoz (S3) másik adatok tárolására. Ez a cikk bemutatja az adatok mozgás egy általános áttekintése és a támogatott adatforrások és gyűjtő adatokat tárolja listája másolása a tevékenységhez [a tevékenységekre vonatkozó adatok mozgását](data-factory-data-movement-activities.md) -témakörben hoz létre.  

Adatok gyári jelenleg csak mozgó adatainak Amazon S3 egyéb adatokat tárolja, de nem az adatok áthelyezése más adatokat tárolja a Amazon S3 támogatja.

## <a name="required-permissions"></a>Szükséges engedélyek

Adatok másolása Amazon S3, győződjön meg arról, alatti engedélyek megkapta:

- **S3:GetObject** és **s3:GetObjectVersion** Amazon S3 objektum műveletekhez
- **S3:ListBucket** és **s3:ListAllMyBuckets** (csak a másolás varázsló használt) Amazon S3 gyűjtő műveletekhez 

Amazon S3 permisions mélységű teljes listáját megtalálhatja az [Engedélyek megadása a házirend](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely az Amazon S3 lévő adatokat másolja a legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példa a minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható biztosít. Azt mutatja be Amazon S3 Azure Blob-tárolóhoz adatok másolása. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Minta: Adatok másolása Amazon S3 Azure Blob
Ez a példa bemutatja, hogyan adatainak másolása az Amazon S3 Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

- Csatolt szolgáltatás típusú [AwsAccessKey](#linked-service-properties).
- Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- A beviteli [adatkészlet](data-factory-create-datasets.md) [AmazonS3](#dataset-type-properties)típusú.
- Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
- A [folyamat](data-factory-create-pipelines.md) [FileSystemSource](#copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta adatokat másolja az Amazon S3 az Azure blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

**Csatolt amazon S3 szolgáltatás**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
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

**Amazon S3 beviteli adatkészlet**

Beállítás **"külső": igaz** tájékoztatja a Data Factory szolgáltatás, az adatkészlet adatok gyári mutató külső, és az adatok gyári tevékenységet nem készül. A beviteli adatkészlet nem egy tevékenységet a során által létrehozott igaz meg ezt a tulajdonságot.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
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
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **FileSystemSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Csatolt szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a JSON elemeit adott leírását az Amazon S3 (**AwsAccessKey**) kapcsolódó szolgáltatás.

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | A titkos hívóbetű azonosítója. | karakterlánc | igen |
| secretAccessKey | A titkos hívóbetű magát. | Titkosított titkos karakterlánc | igen | 


## <a name="dataset-type-properties"></a>Adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A **typeProperties** szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az typeProperties szakaszban adatkészlet **AmazonS3** (amely tartalmazza az Amazon S3 adatkészlet) típusú rendelkezik az alábbi tulajdonságok

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------- | ------ | 
| bucketName | S3 gyűjtő neve. | Karakterlánc | igen |
| kulcs | A S3 objektum billentyűt. | Karakterlánc | nem | 
| előtag | Az S3 objektum billentyű előtagot. Objektumok, amelynek billentyűk Kiindulás ezt az előtagot van kijelölve. Hatókör, csak ha kulcsa üres. | Karakterlánc | nem | 
| verzió | Ha S3 Verziószámozás engedélyezve van S3 objektum verziója. | Karakterlánc | nem |  
| Formátum | A következő formátumú már támogatott: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. A tulajdonság **típusa** formátum csoportban az egyik ezeket az értékeket. További információt [Tartalmazó TextFormat](#specifying-textformat), [AvroFormat megadása](#specifying-avroformat), [Megadása JsonFormat](#specifying-jsonformat), [OrcFormat megadása](#specifying-orcformat)és [Megadása ParquetFormat](#specifying-parquetformat) lásd. Ha a fájlokat a másolni kívánt-van közötti fájlalapú tárolók is (bináris) példányát, ugorja át a két bemeneti és kimeneti adatkészlet definíciók a formátuma szakaszban.| nem
| tömörítés | Adja meg a típus és az adatok tömörítés szintjét. Támogatott típusú: **GZip**, a **Deflate**, és a **BZip2** és a támogatott szintek: **Optimal** és **leggyorsabb**. A tömörítési beállításokat jelenleg nem támogatott **AvroFormat** vagy **OrcFormat**adatait. További tudnivalókért lásd: [tömörítés támogatás](#compression-support) szakaszában.  | nem |

> [AZURE.NOTE] bucketName + kulcs elérési útja az S3 objektumra, ahol gyűjtő a legfelső szintű tároló S3 objektumok, kulcs pedig a teljes elérési útját S3 objektum.

### <a name="sample-dataset-with-prefix"></a>Minta adatkészlet előtaggal

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Minta adatkészlet (verziójával)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dinamikus elérési S3

A mintában a rögzített értékeket az Amazon S3 adatkészlet kulcs és bucketName tulajdonságok használjuk. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Adatok gyári billentyűt és dinamikusan futásidőben bucketName számíthatja ki például SliceStart rendszer változók használatával is.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Ugyanezt az Amazon S3 adatkészlet előtag tulajdonsága műveleteket hajthat végre. Lásd: az [adatok gyári függvények és a rendszer változók](data-factory-functions-variables.md) listáját a támogatott funkciói és a változók. 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Tevékenység típusának tulajdonságainak másolása

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirendek érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető **typeProperties** szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

Ha adatforrás másolása tevékenység típusú **FileSystemSource** (amely tartalmazza az Amazon S3), a következő tulajdonságokat szakaszában érhetők typeProperties:

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------------- | -------- | 
| rekurzív | Itt adhatja meg, hogy rekurzív sorolja S3 objektumok csoportban a címtárban. | IGAZ vagy hamis | nem | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.

## <a name="next-steps"></a>Következő lépések
Az alábbi cikkekben talál: 
- [Másolás tevékenység oktatóprogram](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletes útmutatást a folyamat létrehozása egy másolatot a tevékenységhez. 
