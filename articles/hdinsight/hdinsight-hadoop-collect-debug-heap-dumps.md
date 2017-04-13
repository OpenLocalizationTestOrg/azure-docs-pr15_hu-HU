<properties
    pageTitle="Hibakeresési és elemzése halommemória kiírása Hadoop-szolgáltatások |} Microsoft Azure"
    description="Automatikusan összegyűjtése halommemória kiírása Hadoop-szolgáltatások és hibakeresés és elemzési Azure Blob tároló fiókjában helyezni."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Gyűjt halommemória hibakeresési és elemzése Hadoop-szolgáltatások Blob-tárolóban lévő listázása

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Halommemória kiírása az alkalmazás memóriát, így a változót értékeket a kiírása létrehozásakor pillanatképét tartalmazzák. Így nagyon hasznos, ha diagnosztizálása futásidőben előforduló problémák. Halommemória kiírása automatikusan gyűjtött Hadoop-szolgáltatások és belül HDInsightHeapDumps a felhasználó fiókjának Azure Blob tárolási helyét /. 

A különböző szolgáltatások halommemória kiírása gyűjteménye fürt egyes szolgáltatásaihoz tartozó engedélyezve kell lennie. Ez a szolgáltatás alapértelmezés szerint ki fürt kell. Ezek halommemória kiírása nagy, lehet, ezért tanácsos figyelheti a Blob-tároló fiók azok éppen mentésének helyét a webhelycsoport engedélyezése után.

> [AZURE.NOTE] A cikkben szereplő információk csak a Windows-alapú HDInsight vonatkozik. A HDInsight Linux-alapú további tudnivalókért lásd [halommemória kiírása a Linux-alapú HDInsight Hadoop-szolgáltatások engedélyezése](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Halommemória kiírása jogosult-szolgáltatások

Az alábbi szolgáltatások halommemória kiírása engedélyezheti:

*  **hcatalog** - tempelton
*  **struktúra** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **fonal** - erőforrás-kezelő, nodemanager, timelineserver
*  **fájlrendszerhez** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfigurációs elemek, amelyekben halommemória kiírása

Egy szolgáltatás halommemória kiírása bekapcsolásához kell beállítania a megfelelő konfigurációs elemeket, hogy a szolgáltatás **service_name**által megadott szakaszában.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

**Service_name** értéke a fenti szolgáltatások bármelyike lehet: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, erőforrás-kezelő, nodemanager, timelineserver, datanode, secondarynamenode, vagy namenode.

## <a name="enable-using-azure-powershell"></a>Engedélyezze az Azure PowerShell használatával

Ha például bekapcsolásához halommemória kiírása jobhistoryserver Azure PowerShell használatával, volna tegye a következőket:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Engedélyezze a .NET SDK használatával

Például bekapcsolásához halommemória kiírása jobhistoryserver az Azure hdinsight szolgáltatáshoz .NET SDK használatával meg kellene tegye a következőket:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
