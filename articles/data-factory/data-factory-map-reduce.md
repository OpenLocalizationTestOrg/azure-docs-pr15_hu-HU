<properties 
    pageTitle="Az Azure Data Factory MapReduce Program indítása" 
    description="Megtudhatja, hogy miként MapReduce programok futtatásával egy Azure hdinsight szolgáltatáshoz fürthöz az Azure adatok gyári az adatfeldolgozás." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Az adatok gyár MapReduce programok meghívása
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[.NET, egyéni](data-factory-use-custom-activities.md)

A [folyamat](data-factory-create-pipelines.md) Data Factory HDInsight MapReduce tevékenység MapReduce programok [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy Linux rendszerhez, a Windows-alapú HDInsight fürt [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) hajt végre. Ez a cikk a [tevékenységekre vonatkozó adatok átalakítása](data-factory-data-transformation-activities.md) cikk bemutatja, átalakítási és a támogatott átalakítása tevékenységek általános áttekintése épül.

## <a name="introduction"></a>– Bevezetés 
Egy folyamat egy Azure adatok gyári a csatolt tárolása az adatokat a csatolt számítási szolgáltatásokkal dolgozza fel. A tevékenységek, ahol az egyes tevékenységek adott feldolgozás műveletet hajt végre sorozatában tartalmazza. Ez a cikk ismerteti, hogy a HDInsight MapReduce tevékenység használatával.
 
Lásd: [malac](data-factory-pig-activity.md) és [struktúra](data-factory-hive-activity.md) malac/struktúra parancsfájlok futtatása a Linux rendszerhez, a Windows-alapú HDInsight fürthöz egy folyamat a HDInsight malac és struktúra tevékenységek használatával kapcsolatban további tájékoztatást. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>A JSON HDInsight MapReduce figyelése 

A HDInsight tevékenység JSON meghatározása: 
 
1. A **tevékenység** **típusának** beállítása **hdinsight szolgáltatáshoz**.
3. Adja meg a nevét az osztály **osztálynév** tulajdonság.
4. Adja meg a fájl elérési útját üveg **jarFilePath** tulajdonság a fájlnevet is.
5. Adja meg a csatolt szolgáltatás, amely az Azure Blob-tárolóhoz **jarLinkedService** tulajdonság üveg fájlt tartalmazó hivatkozik.   
6. Adja meg a MapReduce programban argumentumai **argumentumokat** szakaszában. Futásidőben, megjelenik néhány további argumentumokat (például: mapreduce.job.tags) a MapReduce keretében. Például megkülönböztetheti egymástól a argumentumokat MapReduce argumentumok, fontolja meg inkább a lehetőséget, és az értéket argumentumként az alábbi példában látható módon (- s, – beviteli, – kimeneti stb lehetőség közvetlenül követi az értékeket).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

A HDInsight MapReduce tevékenység bármely MapReduce üveg fájl egy HDInsight fürt futtatásához is használhatja. A következő példa JSON meghatározása a folyamat a HDInsight tevékenység úgy van beállítva egy Mahout JAR fájl futtatására.

## <a name="sample-on-github"></a>Példa a GitHub
Töltse le a HDInsight MapReduce tevékenységet a minta: [Adatok gyár minták a GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>A szavak száma futtatása
A folyamat ebben a példában a szavak száma térkép/kicsinyítés program fut, az Azure hdinsight szolgáltatáshoz fürt.   

### <a name="linked-services"></a>Kapcsolt szolgáltatások
Első lépésként hozzon létre egy csatolt szolgáltatás a Azure hdinsight szolgáltatáshoz fürt az Azure adatok gyári által használt Azure tárolására, amelyre a hivatkozás. Ha Ön másolás és beillesztés a következő kódot, ne felejtse el **fiók nevét** , és a **fiókkulcs** helyettesítése a neve és a kulcs az Azure-tárterületét. 

#### <a name="azure-storage-linked-service"></a>Azure csatolt tárhelyszolgáltatáshoz

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
Ezután hozzon létre egy csatolt szolgáltatás az Azure hdinsight szolgáltatáshoz fürt csatolása az Azure adatok gyári. Ha, másolás és beillesztés a következő kódot, **HDInsight fürt név** helyett a HDInsight fürt a nevet, és a felhasználó nevét és jelszavát értékek módosítása.   

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
Ebben a példában a folyamat nem veszi bármely bemeneti adatok alapján. Egy kimenet adatkészlet megadása a HDInsight MapReduce tevékenységet. Ez az adatkészlet csak egy üres adatkészlet a folyamat ütemezésre szükséges.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Folyamat
Ebben a példában a folyamat típusú csak egy tevékenységet tartalmaz: HDInsightMapReduce. Néhány fontos a JSON a következők: 

A tulajdonság | Jegyzetek
:-------- | :-----
típus | A típus **HDInsightMapReduce**kell beállítani. 
Osztálynév | Az osztály neve: **wordcount**
jarFilePath | A fájl elérési útját üveg tartalmazó az osztály. Ha Ön másolás és beillesztés a következő kódot, ne felejtse el módosíthatja a csoport nevét. 
jarLinkedService | Azure csatolt tárhelyszolgáltatáshoz üveg fájlt tartalmazó. A csatolt szolgáltatás a tárterület a HDInsight fürt társított hivatkozik. 
argumentumok | A wordcount programban megnyitja a két argumentuma egy beviteli és olyan eredménye. A beviteli fájl a davinci.txt fájlt.
gyakoriság/intervallum | A tulajdonságok értékének felel meg a kimeneti adatkészlet. 
linkedServiceName | a régebbi verzióiban is létrehozott csatolt HDInsight szolgáltatáshoz hivatkozik.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>A külső programok futtatása
A külső program futtatásához a HDInsight külső fürt MapReduce tevékenység is használhatja. Lásd: [az Azure Data Factory meghívni külső alkalmazások](data-factory-spark.md) további információt.  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Lásd még:
- [Tevékenység struktúra](data-factory-hive-activity.md)
- [Malac tevékenység](data-factory-pig-activity.md)
- [Hadoop Streaming tevékenység](data-factory-hadoop-streaming-activity.md)
- [A külső program meghívása](data-factory-spark.md)
- [Parancsfájlok r billentyűkombinációt meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
