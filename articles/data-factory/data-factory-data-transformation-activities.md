<properties 
    pageTitle="Átalakítási: Folyamat és átalakítás adatok |} Microsoft Azure" 
    description="Megtudhatja, hogy miként vagy adatok folyamat Azure Data Factory Hadoop, gépi tanulási vagy Azure adatok tó Analytics átalakítása." 
    keywords="átalakítási, a folyamat az adatokat az adatokat, a tevékenység átalakítása"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Adatok az Azure Data Factory átalakítása
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[.NET, egyéni](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>– Áttekintés 
Ez a cikk ismerteti a tevékenységekre vonatkozó adatok átalakítása az Azure Data Factory átalakítása segítségével, és a folyamatok a nyers adatokból az előrejelzések és elemzéseket. Egy átalakítási tevékenységet hajtja végre, például Azure hdinsight szolgáltatáshoz fürt vagy egy Azure köteg számítógépes környezetben. Cikkekre mutató hivatkozások nyújtó minden átalakítási tevékenységet részletes tájékoztatást.
 
Adatok gyári támogatja a következő adatok átalakítása tevékenységek hozzáadott [folyamatok](data-factory-create-pipelines.md) akár egyenként, illetve módszere során egy másik tevékenységeket használó.

> [AZURE.NOTE] Lépésenkénti útmutatásért lásd: [Hozzon létre egy folyamat struktúra átalakítási a](data-factory-build-your-first-pipeline.md) cikk az ismertetését megtalálja.  

## <a name="hdinsight-hive-activity"></a>Tevékenység HDInsight-struktúra
A HDInsight-struktúra tevékenység egy adatok gyár során a struktúra lekérdezései saját vagy Linux rendszerhez, a Windows-alapú HDInsight-fürt igény szerinti hajt végre. A tevékenységgel kapcsolatos részletekért lásd: a [Tevékenység struktúra](data-factory-hive-activity.md) cikk. 

## <a name="hdinsight-pig-activity"></a>HDInsight malac tevékenység
A HDInsight malac tevékenység egy Data Factory során a malac lekérdezései saját vagy Linux rendszerhez, a Windows-alapú HDInsight-fürt igény szerinti hajt végre. A tevékenységgel kapcsolatos részletekért lásd: [Malac tevékenység](data-factory-pig-activity.md) cikk. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce tevékenység
A HDInsight MapReduce tevékenység egy Data Factory során a MapReduce-alkalmazásokat a saját vagy igény szerinti Linux rendszerhez, a Windows-alapú HDInsight fürtre hajt végre. A tevékenységgel kapcsolatos részletekért lásd: a [MapReduce tevékenység](data-factory-map-reduce.md) cikk.

A külső program futtatásához a HDInsight külső fürt MapReduce tevékenység is használhatja. Lásd: [az Azure Data Factory meghívni külső alkalmazások](data-factory-spark.md) további információt.

## <a name="hdinsight-streaming-activity"></a>A folyamatos átvitelű HDInsight tevékenység
A Data Factory során a HDInsight Streaming tevékenység végrehajtja a folyamatos átvitelű Hadoop-alkalmazásokat a saját vagy Linux rendszerhez, a Windows-alapú HDInsight-fürt igény szerinti. A tevékenység részleteit [a folyamatos átvitelű HDInsight tevékenység](data-factory-hadoop-streaming-activity.md) talál.

## <a name="machine-learning-activities"></a>Gépi tanulási tevékenységek
Azure Data Factory lehetővé teszi az eszközzel egyszerűen készíthet, amely a cserélendő elemzéséhez közzétett Azure gépi tanulási webszolgáltatással folyamatok. Használja a [Köteg végrehajtás tevékenység](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) -Azure Data Factory során, az előrejelzések kinagyíthatja a köteg adatainak gépi tanulási webszolgáltatás hívása.

Az idő múlásával a cserélendő modellekben, a kísérletek pontozási gépi tanulási kell kell retrained új beviteli adatkészleteket használatával. Miután végzett a átképzés, frissítse a pontozási webszolgáltatás a retrained gépi tanulási modell szeretne. Az [Erőforrás tevékenység frissítése](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) segítségével frissítse a webszolgáltatás az újonnan képzett modell.  

Lásd: [használata gépi tanulási tevékenységek](data-factory-azure-ml-batch-execution-activity.md) e gépi tanulási tevékenységek kapcsolatban további tájékoztatást. 

## <a name="stored-procedure-activity"></a>Tárolt eljárás tevékenység
Is használhatja az SQL Server-a tárolt eljárás tevékenység egy Data Factory során az egyik az alábbi adatokat tárolja a tárolt eljárás meghívásához: Azure SQL-adatbázissal, Azure SQL-adatraktár, SQL Server-adatbázisból a vállalat vagy egy Azure virtuális. Lásd: a [Tárolt eljárás tevékenység](data-factory-stored-proc-activity.md) cikk további információt.  

## <a name="data-lake-analytics-u-sql-activity"></a>Adatok tó Analytics U-SQL nyelvben tevékenység
Adatok tó Analytics U-SQL nyelvben tevékenység U-SQL nyelvben parancsfájl fut, az Azure adatok tó Analytics fürt. Lásd: [Adatok Analytics U-SQL nyelvben tevékenység](data-factory-usql-activity.md) cikk további információt. 

## <a name="net-custom-activity"></a>.NET egyéni tevékenység
Kell az adatokat, ha nem támogatott a Data Factory, ha saját adatfeldolgozás logikájának hozzon létre egy egyéni tevékenységeket, és a tevékenység használata a során. Úgy is beállíthatja az egyéni .NET tevékenység egy köteg Azure szolgáltatás vagy a egy Azure hdinsight szolgáltatáshoz fürthöz futtatásához. Lásd: [egyéni tevékenységeknek](data-factory-use-custom-activities.md) a cikk további információt. 

A HDInsight fürt R telepítve van az R-parancsprogramokat futtassanak egy egyéni tevékenységet hozhat létre. Lásd: [R parancsfájl futtatása Azure Data Factory használata](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Környezetekben kiszámítása
Hozzon létre egy csatolt szolgáltatást a számítási környezetben, és kattintson a szolgáltatással csatolt egy átalakítási tevékenységet megadásakor. A Data Factory által támogatott számítási környezetek két típusa van. 

1. **Igény szerinti**: Ebben az esetben a számítógépes környezet teljesen által kezelt adatok gyári. Automatikusan létrejön az adatok gyári szolgáltatás által a feladat folyamat adatainak elé, és eltávolítja a feladat befejezése előtt. Állítsa be, és az igény szerinti számítási környezet feladat végrehajtását, a kezelő és a műveletek rendszerindításáért részletes beállításait. 
2. **A saját hozása**: Ebben az esetben Data Factory csatolt szolgáltatásként rögzítheti saját számítógépes környezetben (például HDInsight fürthöz). Ön által felügyelt számítógépes környezet esetén, és az adatok gyári szolgáltatás a tevékenységek végrehajtásához használja. 

Lásd: [Csatolt szolgáltatások számítja ki](data-factory-compute-linked-services.md) a cikk további információt az adatok gyári által támogatott szolgáltatások számítja ki. 


## <a name="summary"></a>Összefoglalás
Azure Data Factory támogatja a következő adatok átalakítása tevékenységek és a számítási környezetben a tevékenységeket. Az átalakítás tevékenységek a folyamatok vagy egyenként, illetve is egy másik tevékenységeket használó módszere során.

Adatok átalakítása tevékenység |  Környezet kiszámítása 
:----------------------- | :--------------------
[A struktúra](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Malac](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[A gép tevékenységek tanulási: köteg végrehajtása és a frissítés erőforrás](data-factory-azure-ml-batch-execution-activity.md) | Azure virtuális 
[Tárolt eljárás](data-factory-stored-proc-activity.md) | Azure SQL Azure SQL-adatraktár és az SQL Server |
[Adatok tó Analytics U-SQL nyelvben](data-factory-usql-activity.md) | Azure adatok tó Analytics 
[DotNet](data-factory-use-custom-activities.md) | [Hadoop] HDInsight vagy Azure köteg
   

