<properties 
    pageTitle="Tevékenység struktúra" 
    description="Megtudhatja, hogyan használhatja a struktúra tevékenység-Azure adatok gyári a struktúra lekérdezések futtatása a-a-igény szerinti /: a saját HDInsight fürthöz." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Tevékenység struktúra
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[.NET, egyéni](data-factory-use-custom-activities.md)

A [folyamat](data-factory-create-pipelines.md) Data Factory HDInsight-struktúra tevékenység struktúra lekérdezések [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy az [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Linux rendszerhez, a Windows-alapú HDInsight fürtre hajt végre. Ez a cikk a [tevékenységekre vonatkozó adatok átalakítása](data-factory-data-transformation-activities.md) cikk bemutatja, átalakítási és a támogatott átalakítása tevékenységek általános áttekintése épül.

## <a name="syntax"></a>Szintaxis

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Szintaxis részletei

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
név | A tevékenység neve. | igen
Leírás | Mire használható a tevékenység leíró szöveget | nem
típus | HDinsightHive | igen
bemeneti adatok alapján | A struktúra tevékenység felhasznált ráfordítások | nem
kimeneti értékeket | Kimeneti értékeket a struktúra tevékenység készített | igen 
linkedServiceName | Csatolt adatok gyári szolgáltatásként regisztrált a HDInsight fürt mutató hivatkozás | igen 
parancsfájl | Adja meg a struktúra parancsfájl beágyazott | nem
parancsfájl elérési útja | Azure blob-tárolóhoz a struktúra parancsfájl tárolni, és adja meg a fájl elérési útját. "Parancsfájl" vagy "Parancsfájl_elérési_útja" tulajdonságot használja. Mindkét nem használhatók együtt. A fájl nevét a program. | nem 
határozza meg | Paramétert kulcs/érték párokká, az arra hivatkozó belül a struktúra parancsfájl használatával "hiveconf"  | nem

## <a name="example"></a>Példa

Nézzünk meg egy példája analitika, ahová be szeretné azonosítani a vállalat által indított játékok felhasználók által töltött időt játék naplók. 

A következő napló minta játék napló, amely vessző (`,`) elválasztott és a következő mezőket – amelyben a ProfileID, SessionStart, időtartam, SrcIPAddress és GameType tartalmazza.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

A **struktúra parancsfájl** feldolgozása ezeket az adatokat:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

A Data Factory során, hajtsa végre a struktúra parancsfájl, tegye a következőket kell

1. Hozzon létre egy csatolt szolgáltatás regisztrálhatja a [saját HDInsight fürt kiszámítania](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálja a [HDInsight igény szerinti fürt számítja ki](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Vegyük hívja fel a csatolt szolgáltatás "HDInsightLinkedService".
2. Hozzon létre egy [csatolt szolgáltatás](data-factory-azure-blob-connector.md) konfigurálása a kapcsolat szolgáltatója, az adatok Azure Blob-tárolóhoz. Vegyük hívja fel a csatolt szolgáltatás "StorageLinkedService"
3. Hozzon létre [adatkészleteket](data-factory-create-datasets.md) mutatnak a bemeneti és kimeneti adatai. Vegyük hívja fel a bemeneti adatkészlet "HiveSampleIn" és a kimeneti adatkészlet "HiveSampleOut"
4. A struktúra lekérdezés Azure Blob-tárolóhoz fájlként másolás konfigurálva a #2. Ha eltér a lekérdezés fájlt tároló-szolgáltatója, az adatok tárolására, hozzon létre egy külön Azure csatolt tárhelyszolgáltatáshoz, és a hivatkozott a tevékenységet. **Parancsfájl_elérési_útja **segítségével struktúra lekérdezés fájl-és **scriptLinkedService** megadhatja az Azure tárhely, amely tartalmazza a parancsprogram elérési útja. 

    > [AZURE.NOTE] A **parancsprogram** -tulajdonság segítségével is megadhat a struktúra parancsfájl beágyazott a tevékenység-definícióban. Hogy nem javasolt eljárás szerint a speciális karaktereket a parancsfájl, meg kell jelölni a JSON dokumentum igényeinek belül, és hibakeresés-problémákat okozhat. A legjobb módszer, ha követi a lépés: #4.
5.  Hozzon létre egy folyamat a HDInsightHive tevékenységet. A tevékenység folyamatok/átalakítók az adatokat.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  A folyamat üzembe. Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) a cikk további információt. 
7.  Figyelje meg a az adatok gyári figyelő és felügyeleti nézetek használata során. Lásd: [Figyelés és adatok gyári folyamatok kezelése](data-factory-monitor-manage-pipelines.md) a cikk további információt. 


## <a name="specifying-parameters-for-a-hive-script"></a>Adja meg a paraméterek struktúra parancsfájl  
Ebben a példában a játék naplókról keresztül a naponta az Azure Blob-tárolóhoz szervezetbe, és a dátum és idő particionálni mappában tárolódnak. A struktúra parancsfájl paraméterezni és runtime során dinamikusan adják át a bemeneti mappa helyét, és is készít a dátum és idő particionálni kimenet szeretne.

Paraméteres struktúra parancsfájl használni, tegye a következőket

- Definiálása paramétereit **határozza meg**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- A struktúra parancsfájl olvassa el a paramétert **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Lásd még:
- [Malac tevékenység](data-factory-pig-activity.md)
- [MapReduce tevékenység](data-factory-map-reduce.md)
- [Hadoop Streaming tevékenység](data-factory-hadoop-streaming-activity.md)
- [A külső program meghívása](data-factory-spark.md)
- [Parancsfájlok r billentyűkombinációt meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









