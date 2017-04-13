<properties
   pageTitle="A HDInsight PowerShell használata a Hadoop malac |} Microsoft Azure"
   description="Megtudhatja, hogy miként küldhetnek HDInsight Azure PowerShell használatával a Hadoop fürtre malac feladatokat."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>A PowerShell használatával malac feladatok futtatása

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

A dokumentum egy példa egy hadoop HDInsight fürt malac feladatok elküldése Azure PowerShell használatá tartalmaz. Malac lehetővé teszi MapReduce feladatok írni egy nyelvet (malac latin betűs), amely a modellek adatok átalakítások, használatával, hanem megfeleltetése és a függvények csökkentése.

> [AZURE.NOTE] A dokumentum nem biztosít a malac Latin kimutatások példáiban szereplő mire részletes leírását. Az ebben a példában használt malac Latin tudni lásd: [A HDInsight Hadoop malac használata](hdinsight-use-pig.md).

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez az alábbi lesz szüksége.

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>A PowerShell használatával malac feladatok futtatása

Azure PowerShell biztosít, amelyek lehetővé teszik malac feladat távolról futtatható HDInsight *parancsmagok* . Belső ez végezhető el a a HDInsight fürthöz [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (korábbi néven Templeton) hívások többi.

A következő parancsmagok használják a távoli HDInsight fürtre malac feladatok forgalmi:

* **Bejelentkezés-AzureRmAccount**: Azure-előfizetéséhez Azure PowerShell hitelesíti

* **Új-AzureRmHDInsightPigJobDefinition**: hoz létre új *feladat definíciója* a megadott malac Latin kimutatások használatával

* **Kezdés-AzureRmHDInsightJob**: a feladat definíciója küld HDInsight, a projekt indítása és ad vissza egy *feladat* objektum használt a feladat állapotának ellenőrzése

* **Várakozás-AzureRmHDInsightJob**: a projekt-objektum segítségével a feladat állapotának ellenőrzése. Várakozik, hogy amíg a feladat befejezése, vagy a várakozási idő túllépve.

* **Get-AzureRmHDInsightJobOutput**: a kimenet a feladat beolvasásához használt

A parancsmagok használatáról a feladat futtatása a HDInsight fürt bemutatják, az alábbi lépéseket.

1. **Pigjob.ps1**-szerkesztővel mentse a következő kódot. A HDInsight fürt neve ezt kell lecserélnie **CLUSTERNAME** .

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Nyissa meg a Windows PowerShell új parancssort. Váltás a **pigjob.ps1** fájl helyét, majd használja a következő parancs futtatása:

        .\pigjob.ps1
        
    Első bejelentkezési Azure-előfizetéséhez kéri. Ezután, megkéri a HTTPs/rendszergazdai fiók nevét, és a jelszót a HDInsight fürt.

7. A feladat befejezésekor kell visszaadnia információk az alábbihoz hasonló:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Hibaelhárítás

Nincs információ a feladat befejezésekor ad vissza, ha egy meghibásodott feldolgozása közben. Ehhez a feladathoz hibaüzenet adatainak megtekintéséhez adja hozzá a következő parancsot a **pigjob.ps1** fájl végére, és mentse, majd futtassa újra.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Ez ad vissza az adatokat, írt ezzel a kiszolgálón a feladat futtatása során, és próbálkozzon a következőkkel határozza meg, miért hibás a feladat.

##<a id="summary"></a>Összefoglalás

Amint látható, Azure PowerShell egyszerűvé egy HDInsight fürthöz malac feladat futtatható, figyelheti a projekt állapotát, valamint a kimenet.

##<a id="nextsteps"></a>Következő lépések

A HDInsight malac általános információt:

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
