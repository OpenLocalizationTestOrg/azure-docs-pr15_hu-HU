<properties
   pageTitle="A HDInsight PowerShell használata a Hadoop-struktúra |} Microsoft Azure"
   description="A PowerShell használatá struktúra-lekérdezések futtatása a Hadoop a hdinsight szolgáltatásból lehetőségre."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>A PowerShell használatával struktúra lekérdezések futtatása

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

A dokumentum egy példa az Azure erőforráscsoport módban Azure PowerShell használatá struktúra-lekérdezések futtatása a egy Hadoop HDInsight fürt tartalmaz.

> [AZURE.NOTE] A dokumentum nem biztosít a példákban használt HiveQL kimutatások mire részletes leírását. Az ebben a példában használt HiveQL a további tudnivalókért lásd [A Hadoop HDInsight a struktúra használata](hdinsight-use-hive.md).


**Előfeltételek**

A jelen cikkben ismertetett lépések elvégzéséhez az alábbi lesz szüksége.

- **Az Azure hdinsight szolgáltatáshoz (Hadoop a HDInsight) fürt (Windows-alapú vagy Linux-alapú)**
- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Azure PowerShell használatával struktúra lekérdezések futtatása

Azure PowerShell *parancsmaggal* , amelyek lehetővé teszik távolról struktúra-lekérdezések futtatása a HDInsight biztosít. Belső ez végezhető el a a HDInsight fürthöz [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (korábbi néven Templeton) hívások többi.

A következő parancsmagok használják a távoli HDInsight fürtre struktúra lekérdezések futtatásakor:

* **Hozzáadás-AzureRmAccount**: Azure PowerShell hitelesíti Azure-előfizetéséhez

* **Új-AzureRmHDInsightHiveJobDefinition**: hoz létre új *feladat definíciója* a megadott HiveQL kimutatások használatával

* **Kezdés-AzureRmHDInsightJob**: a feladat definíciója küld HDInsight, a projekt indítása és ad vissza egy *feladat* objektum használt a feladat állapotának ellenőrzése

* **Várakozás-AzureRmHDInsightJob**: a projekt-objektum segítségével a feladat állapotának ellenőrzése. Várakozik, hogy amíg a feladat befejezése után, vagy a várakozási idő túllépik.

* **Get-AzureRmHDInsightJobOutput**: a kimenet a feladat beolvasásához használt

* **Invoke-AzureRmHDInsightHiveJob**: HiveQL kimutatások futtatásához használt. Ez a lekérdezés blokkolja befejeződik, majd az eredményt ad

* **Használat-AzureRmHDInsightCluster**: állítja be az aktuális fürtre a **Invoke-AzureRmHDInsightHiveJob** parancs használata

A parancsmagok használatáról a feladat futtatása a HDInsight fürt bemutatják, az alábbi lépéseket:

1. A szerkesztő használata esetén a következő kódot mentése **hivejob.ps1**. A HDInsight fürt neve ezt kell lecserélnie **CLUSTERNAME** .

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Nyisson meg egy új **Azure PowerShell** parancssort. Váltás a **hivejob.ps1** fájl helyét, majd használja a következő parancs futtatása:

        .\hivejob.ps1

    A parancsfájl fut, amikor a rendszer kéri a fürt adja meg a HTTPS/rendszergazdai fiók hitelesítő adatait. Előfordulhat, hogy is kérni bejelentkezési Azure-előfizetéséhez.
    
7. A feladat befejezésekor kell visszaadnia információk az alábbihoz hasonló:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. A korábban említett **Invoke-struktúra** használható futtassa a lekérdezést, és várja meg a választ. Az alábbi parancsokat használhatja, és **CLUSTERNAME** cserélje ki a fürt neve:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    A kimenet jelenik meg, például a következőket:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Hosszabb HiveQL lekérdezések esetén az **Alábbi-karakterláncok** Azure PowerShell-parancsmag vagy HiveQL parancsfájlok is használhatja. A következő kódtöredékének megtudhatja, hogy hogyan **Invoke-struktúra** parancsmag HiveQL script fájl futtatására. A HiveQL parancsprogram fel kell tölteni a wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > **Itt-karakterláncok**kapcsolatos további tudnivalókért olvassa el a <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Használatával a Windows PowerShell itt-karakterláncok</a>című témakört.

##<a name="troubleshooting"></a>Hibaelhárítás

Nincs információ a feladat befejezésekor ad vissza, ha egy meghibásodott feldolgozása közben. Ehhez a feladathoz hibaüzenet adatainak megtekintéséhez adja hozzá a következő **hivejob.ps1** fájl végére, mentse, és majd futtassa újra.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

A írt információ ezzel a kiszolgálón a feladat futtatásakor visszaad, és segíthet határozza meg, miért hibás a feladat.

##<a name="summary"></a>Összefoglalás

Amint látható, Azure PowerShell egyszerűvé struktúra lekérdezések futtatása HDInsight fürt, figyelheti a projekt állapotát, valamint a kimenet.

##<a name="next-steps"></a>Következő lépések

A HDInsight struktúra általános információt:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
