<properties 
    pageTitle="Azure adatok gyár külső programok meghívása" 
    description="Megtudhatja, hogy miként meghívni külső programok az Azure adatok gyári a MapReduce tevékenység használatával." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>A külső adatok gyári programok meghívása
## <a name="introduction"></a>– Bevezetés
A Data Factory folyamat a MapReduce tevékenység segítségével a HDInsight külső fürt külső programok futtatása. Lásd: [MapReduce tevékenység](data-factory-map-reduce.md) cikk részletes információt a tevékenység használatáról a cikket elolvasva előtt. 

## <a name="spark-sample-on-github"></a>A GitHub külső minta
A [külső - adatok Factory-minta GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) megtudhatja, hogy hogyan MapReduce tevékenység meghívni külső programot. A külső program csak adatokat másolja egy Azure Blob-tárolóból között. 

## <a name="data-factory-entities"></a>Adatok gyári személyek
A **Külső-ADF/src/ADFJsons** mappa Data Factory entitás (csatolt szolgáltatásokat, adatkészleteket, folyamat) fájlokat tartalmazza.  

Ez a példa két **csatolt szolgáltatások** szerepelnek: Azure-tárhely és Azure hdinsight szolgáltatásból lehetőségre. Adja meg a Azure tárhely és a legfontosabb értékeit **StorageLinkedService.json** clusterUri, felhasználónév és jelszó **HDInsightLinkedService.json**.

Ez a példa két **adatkészleteket** szerepelnek: **input.json** és **output.json**. Ezeket a fájlokat a **adatkészleteket** mappában található.  Ezeket a fájlokat a MapReduce tevékenységhez tartozó bemeneti és kimeneti adatkészleteket képviseli

Minta folyamatok a **ADFJsons/folyamat** mappában találja. Tekintse át a külső program meghívni a MapReduce tevékenység használatával miként folyamat. 

A MapReduce tevékenység **com.adf.sparklauncher.jar** a **adflibs** tárolóban (a StorageLinkedService.json megadva), a Azure tárolóban lévő meghívásához van beállítva. A külső-ADF/src/fő/java/com/adf a program a forráskódot/mappát, és a hívások külső nyújt, és futtassa a külső feladatokat. 

> [AZURE.IMPORTANT] 
> Olvassa el a [– Fontos fájl. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) a legújabb és további információt a minta használata előtt. 
>  
> A saját HDInsight külső fürtre használata ezt a megközelítést meghívni külső programok a MapReduce tevékenység használatával. Az igény szerinti HDInsight fürt használata nem támogatott.   


## <a name="see-also"></a>Lásd még:
- [Tevékenység struktúra](data-factory-hive-activity.md)
- [Malac tevékenység](data-factory-pig-activity.md)
- [MapReduce tevékenység](data-factory-map-reduce.md)
- [Hadoop Streaming tevékenység](data-factory-hadoop-streaming-activity.md)
- [Parancsfájlok r billentyűkombinációt meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
