<properties 
    pageTitle="Hadoop Streaming tevékenység" 
    description="Megtudhatja, hogyan használhatja a Hadoop Streaming tevékenység-Azure adatok gyári a programok a folyamatos átvitelű Hadoop-a-igény szerinti /: a saját HDInsight fürt futtatásához." 
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
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop Streaming tevékenység
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[.NET, egyéni](data-factory-use-custom-activities.md)

A HDInsightStreamingActivity tevékenység is használhatja az Azure Data Factory folyamat egy Hadoop Streaming feladat meghívásához. A következő JSON kódtöredékének a folyamat JSON fájl a HDInsightStreamingActivity használatához az alábbi jeleníti meg. 

A [folyamat](data-factory-create-pipelines.md) Data Factory HDInsight Streaming tevékenység Hadoop Streaming programok [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy Linux rendszerhez, a Windows-alapú HDInsight fürt [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) hajt végre. Ez a cikk a [tevékenységekre vonatkozó adatok átalakítása](data-factory-data-transformation-activities.md) cikk bemutatja, átalakítási és a támogatott átalakítása tevékenységek általános áttekintése épül.

## <a name="json-sample"></a>JSON minta
A HDInsight fürt automatikusan kitölti (wc.exe és cat.exe) Példa programok és az adatok (davinci.txt). Alapértelmezés szerint a tároló a HDInsight fürt által használt neve a fürthöz nevét. Ha például a csoport neve myhdicluster, neve a társított blob-tárolóhoz myhdicluster lenne. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Vegye figyelembe az alábbiakat:

1. A **linkedServiceName** állítsuk be a csatolt szolgáltatás, amely a HDInsight fürt, amelyen a továbbított mapreduce feladat futtatása mutat nevére.
2. Állítsa a tevékenység típusa **HDInsightStreaming**.
3. A **hozzárendelést** tulajdonság adja meg a hozzárendelést végrehajtható fájl nevét. A példában szereplő cat.exe a végrehajtható hozzárendelést.
4. A **nyomáscsökkentő** tulajdonság adja meg a végrehajtható nyomáscsökkentő nevét. A példában szereplő wc.exe a végrehajtható nyomáscsökkentő.
5. **Beviteli** típus tulajdonság adja meg a hozzárendelést a bemeneti fájlt (a helyet is beleértve). A példában: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample a blob-tárolóhoz, például/adatok/Gutenberg a mappát, és davinci.txt a blob.
6. A **Kimenet** típusa tulajdonság adja meg a nyomáscsökkentő a kimeneti fájl (a helyet is beleértve). A kimenet a Hadoop Streaming feladat írja be a tulajdonság értékét a megadott helyre.
7. **FilePaths** szakaszban adja meg a hozzárendelést és nyomáscsökkentő végrehajtható fájl elérési útját. A példában: "adfsample/example/apps/wc.exe", adfsample a blob-tárolóhoz, például/alkalmazások a mappát, és wc.exe a végrehajtható fájl.
8. A **fileLinkedService** tulajdonság adja meg az Azure csatolt tárhelyszolgáltatáshoz, amely az Azure tárhely, amely tartalmazza a megadott filePaths szakaszában fájlok.
9. Az **argumentumok** tulajdonság adja meg az argumentumokat a továbbított projektre vonatkozóan.
10. A **getDebugInfo** tulajdonsága valamely választható elemhez. Hiba van beállítva, ha a hiba csak a naplók töltődnek le. Beállítás az összes, amikor naplók mindig töltődnek le a végrehajtási állapota függetlenül.

> [AZURE.NOTE] Ahogy a példában látható, adja meg a kimeneti adatkészlet a **Kimenet** tulajdonság a Hadoop Streaming tevékenység. Ez az adatkészlet csak egy üres adatkészlet a folyamat ütemezésre szükséges. Nem kell bármely beviteli adatkészlet a tevékenységhez a **bemenetben** tulajdonság adja meg.  

    
## <a name="example"></a>Példa
A folyamat az útmutató a szavak száma adatfolyam leképezés/kicsinyítés program az Azure hdinsight szolgáltatáshoz fürtre fut. 

### <a name="linked-services"></a>Kapcsolt szolgáltatások

#### <a name="azure-storage-linked-service"></a>Azure csatolt tárhelyszolgáltatáshoz
Első lépésként hozzon létre egy csatolt szolgáltatás a Azure hdinsight szolgáltatáshoz fürt az Azure adatok gyári által használt Azure tárolására, amelyre a hivatkozás. Ha Ön másolás és beillesztés a következő kódot, ne felejtse el fiók nevét, és a fiókkulcs helyettesítése a neve és a kulcs az Azure-tárterületét. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure hdinsight szolgáltatáshoz kapcsolódó szolgáltatás
Ezután hozzon létre egy csatolt szolgáltatás az Azure hdinsight szolgáltatáshoz fürtre csatolása az Azure adatok gyári. Ha, másolás és beillesztés a következő kódot, HDInsight fürtre név helyett a HDInsight fürtre a nevet, és a felhasználó nevét és jelszavát értékek módosítása. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Adatkészletek

#### <a name="output-dataset"></a>Kimeneti adatkészlet
Ebben a példában a folyamat nem veszi bármely bemeneti adatok alapján. Egy kimenet adatkészlet a HDInsight Streaming tevékenység adja meg. Ez az adatkészlet csak egy üres adatkészlet a folyamat ütemezésre szükséges. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Folyamat

Ebben a példában a folyamat típusú csak egy tevékenységet tartalmaz: **HDInsightStreaming**. 

A HDInsight fürt automatikusan kitölti (wc.exe és cat.exe) Példa programok és az adatok (davinci.txt). Alapértelmezés szerint a tároló a HDInsight fürt által használt neve a fürthöz nevét. Ha például a csoport neve myhdicluster, neve a társított blob-tárolóhoz myhdicluster lenne.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Lásd még:
- [Tevékenység struktúra](data-factory-hive-activity.md)
- [Malac tevékenység](data-factory-pig-activity.md)
- [MapReduce tevékenység](data-factory-map-reduce.md)
- [A külső programok meghívása](data-factory-spark.md)
- [Parancsfájlok r billentyűkombinációt meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

