<properties 
    pageTitle="Malac tevékenység" 
    description="Megtudhatja, hogyan használhatja a malac tevékenység-Azure adatok gyári a malac parancsprogramokat futtassanak-a-igény szerinti /: a saját HDInsight fürt." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Malac tevékenység
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[.NET, egyéni](data-factory-use-custom-activities.md)

A [folyamat](data-factory-create-pipelines.md) Data Factory HDInsight malac tevékenység malac lekérdezések [saját](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy Linux rendszerhez, a Windows-alapú HDInsight fürtre [igény szerinti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) hajt végre. Ez a cikk a [tevékenységekre vonatkozó adatok átalakítása](data-factory-data-transformation-activities.md) cikk bemutatja, átalakítási és a támogatott átalakítása tevékenységek általános áttekintése épül.

## <a name="syntax"></a>Szintaxis

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
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
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Szintaxis részletei

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
név | A tevékenység neve. | igen
Leírás | Mire használható a tevékenység leíró szöveget | nem
típus | HDinsightPig | igen
bemeneti adatok alapján | A malac tevékenységhez felhasznált egy vagy több bemeneti adatok alapján | nem
kimeneti értékeket | Egy vagy több állítanak a malac tevékenység | igen
linkedServiceName | Csatolt adatok gyári szolgáltatásként regisztrált a HDInsight fürt mutató hivatkozás | igen
parancsfájl | Adja meg a malac parancsfájl beágyazott | nem
parancsfájl elérési útja | Azure blob-tárolóhoz a malac parancsfájl tárolni, és adja meg a fájl elérési útját. "Parancsfájl" vagy "Parancsfájl_elérési_útja" tulajdonságot használja. Mindkét nem használhatók együtt. A fájl nevét a program. | nem
határozza meg | Paramétert kulcs/érték párokká, az arra hivatkozó belül a malac parancsfájl | nem

## <a name="example"></a>Példa

Nézzünk meg egy példája analitika, ahová be szeretné azonosítani a vállalat által indított játékok játékosokat által töltött időt játék naplók.
 
A következő példa játék napló fájl vessző (,) elválasztva egymástól. Az alábbi mezők – amelyben a ProfileID, SessionStart, időtartam, SrcIPAddress és GameType tartalmaz.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

A **malac parancsfájl** feldolgozása ezeket az adatokat:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Malac parancsfájl végrehajtani a Data Factory folyamat, tegye a következőket:

1. Hozzon létre egy csatolt szolgáltatás regisztrálhatja a [saját HDInsight fürt kiszámítania](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) vagy konfigurálja a [HDInsight igény szerinti fürt számítja ki](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Vegyük hívja fel a csatolt szolgáltatás **HDInsightLinkedService**.
2.  Hozzon létre egy [csatolt szolgáltatás](data-factory-azure-blob-connector.md) konfigurálása a kapcsolat szolgáltatója, az adatok Azure Blob-tárolóhoz. Vegyük hívja fel a csatolt szolgáltatás **StorageLinkedService**.
3.  Hozzon létre [adatkészleteket](data-factory-create-datasets.md) mutatnak a bemeneti és kimeneti adatai. Vegyük hívja fel a bemeneti adatkészlet **PigSampleIn** és a kimeneti adatkészlet **PigSampleOut**.
4.  Másolja a malac lekérdezést egy fájlt a #2 konfigurált Azure Blob-tárolóhoz. Ha az adatokat tároló Azure tárolására eltér, hogy a lekérdezés fájlt tárolja, hozzon létre egy külön Azure csatolt tárhelyszolgáltatáshoz. Keresse meg a tevékenység konfigurációban a csatolt szolgáltatást. **Parancsfájl_elérési_útja **segítségével malac parancsprogram és **scriptLinkedService**elérési útja. 
    
    > [AZURE.NOTE] A **parancsprogram** -tulajdonság segítségével is megadhat a tevékenység-definícióban az malac parancsfájl beágyazott. Jó helyen jár hogy nem javasolt eljárás szerint meg kell jelölni a parancsprogram igényeinek speciális karaktereket, és hibakeresési-problémákat okozhat. A legjobb módszer, ha követi a lépés: #4.
5. A folyamat létrehozása a HDInsightPig tevékenységet. Ez a tevékenység a bemeneti adatok HDInsight fürtre malac parancsfájl futtatásával dolgozza fel.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. A folyamat üzembe. Lásd: [folyamatok létrehozása](data-factory-create-pipelines.md) a cikk további információt. 
7. Figyelje meg a az adatok gyári figyelő és felügyeleti nézetek használata során. Lásd: [Figyelés és adatok gyári folyamatok kezelése](data-factory-monitor-manage-pipelines.md) a cikk további információt.

## <a name="specifying-parameters-for-a-pig-script"></a>Adja meg a paraméterek malac parancsfájl 

Vegye figyelembe az alábbi példa: játék naplók keresztül a szervezetbe naponta az Azure Blob-tárolóhoz és mappájában particionált alapján dátuma és időpontja. A malac parancsfájl paraméterezni és runtime során dinamikusan adják át a bemeneti mappa helyét, és is készít a dátum és idő particionálni kimenet szeretne.
 
Paraméteres malac parancsfájl használni, tegye a következőket:

- Definiálása paramétereit **határozza meg**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- A malac parancsfájl olvassa el a paraméterek használata "**$parameterName**", az alábbi példában látható módon:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Lásd még:
- [Tevékenység struktúra](data-factory-hive-activity.md)
- [MapReduce tevékenység](data-factory-map-reduce.md)
- [Hadoop Streaming tevékenység](data-factory-hadoop-streaming-activity.md)
- [A külső program meghívása](data-factory-spark.md)
- [Parancsfájlok r billentyűkombinációt meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


