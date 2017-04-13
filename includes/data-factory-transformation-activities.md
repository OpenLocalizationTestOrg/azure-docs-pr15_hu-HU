Azure Data Factory a következő átalakítása tevékenységek hozzáadott folyamatok akár egyenként, illetve módszere során egy másik tevékenységeket használó támogatja.

Adatok átalakítása tevékenység |  Környezet kiszámítása 
:----------------------- | :--------------------
[A struktúra](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Malac](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[A folyamatos átvitelű Hadoop](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[A gép tevékenységek tanulási: köteg végrehajtása és a frissítés erőforrás](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure virtuális 
[Tárolt eljárás](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL Azure SQL-adatraktár és az SQL Server |
[Adatok tó Analytics U-SQL nyelvben](../articles/data-factory/data-factory-usql-activity.md) | Azure adatok tó Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | [Hadoop] HDInsight vagy Azure köteg
   
> [AZURE.NOTE] 
> A külső program futtatásához a HDInsight külső fürt MapReduce tevékenység is használhatja. Lásd: [az Azure Data Factory meghívni külső alkalmazások](../articles/data-factory/data-factory-spark.md) további információt.
> A HDInsight fürtre R telepítve van az R-parancsprogramokat futtassanak egy egyéni tevékenységet hozhat létre. Lásd: [R parancsfájl futtatása Azure Data Factory használata](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).