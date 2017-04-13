<properties
   pageTitle="MapReduce és a PowerShell használata Hadoop |} Microsoft Azure"
   description="Megtudhatja, hogy miként PowerShell-lel való távolról MapReduce feladat futtatható a Hadoop hdinsight szolgáltatásból lehetőségre."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>A Hadoop MapReduce feladat futtatható HDInsight PowerShell használatával

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

A dokumentum egy példa egy Hadoop MapReduce feladat futtatása a HDInsight fürthöz Azure PowerShell használatá tartalmaz.

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

- **Az Azure hdinsight szolgáltatáshoz (Hadoop a HDInsight) fürt (Windows-alapú vagy Linux-alapú)**

- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Feladat futtatása olyan MapReduce Azure PowerShell használatával

Azure PowerShell *parancsmagok* , amelyek lehetővé teszik MapReduce feladat távolról futtatható HDInsight biztosít. Belső ez végezhető el a a HDInsight fürthöz [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (korábbi néven Templeton) hívások többi.

A következő parancsmagok MapReduce feladatok futtató távoli HDInsight fürt használnak.

* **Bejelentkezés-AzureRmAccount**: Azure PowerShell hitelesíti Azure-előfizetéséhez

* **Új-AzureRmHDInsightMapReduceJobDefinition**: hoz létre új *feladat definíciója* MapReduce megadott adatokkal

* **Kezdés-AzureRmHDInsightJob**: a feladat definíciója küld HDInsight, a projekt indítása és ad vissza egy *feladat* objektum használt a feladat állapotának ellenőrzése

* **Várakozás-AzureRmHDInsightJob**: a projekt-objektum segítségével a feladat állapotának ellenőrzése. Vár, addig, amíg a feladat befejezése után vagy túllépik a várakozási időt.

* **Get-AzureRmHDInsightJobOutput**: a kimenet a feladat beolvasásához használt

A parancsmagok használatáról a feladat futtatása a HDInsight fürt bemutatják, az alábbi lépéseket.

1. A szerkesztő használata esetén a következő kódot mentése **mapreducejob.ps1**. A HDInsight fürt neve ezt kell lecserélnie **CLUSTERNAME** .

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Nyisson meg egy új **Azure PowerShell** parancssort. Váltás a **mapreducejob.ps1** fájl helyét, majd használja a következő parancs futtatása:

        .\mapreducejob.ps1
    
    A parancsfájl futtatásakor Azure-előfizetéséhez hitelesítést végezni kérheti. Akkor is lehet megadását kérni fogja HTTPS/rendszergazdai fiók nevét és jelszavát a HDInsight fürt.

3. A feladat befejezése után, az alábbihoz hasonló kimeneti kell megjelennie:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    A kimenet azt jelzi, hogy a feladat sikeresen befejeződött.

    > [AZURE.NOTE] Ha a **ExitCode** értéke nem 0, akkor a [Hibaelhárítás](#troubleshooting).

    Ebben a példában a parancsfájl futtatásakor a címtárban is tárolja a letöltött fájlok **kimenet.txt** fájlba.

###<a name="view-output"></a>A kimenet megtekintése

Nyissa meg a **kimenet.txt** fájlt egy szövegszerkesztőben, olvassa el a szavak és a feladat készített száma.

> [AZURE.NOTE] A kimenet MapReduce feladat fájlok megváltoztatható. Igen, futtassa a következő példában, ha akkor kell a kimeneti fájl nevének módosítása.

##<a id="troubleshooting"></a>Hibaelhárítás

Nincs információ a feladat befejezésekor ad vissza, ha egy meghibásodott feldolgozása közben. Ehhez a feladathoz hibaüzenet adatainak megtekintéséhez adja hozzá a következő parancsot a **mapreducejob.ps1** fájl végére, és mentse, majd futtassa újra.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Visszaad írt ezzel a kiszolgálón a feladat futtatása során az adatokat, és határozza meg, miért hibás a feladat nyújthatnak segítséget.

##<a id="summary"></a>Összefoglalás

Amint látható, Azure PowerShell egyszerűvé egy HDInsight fürthöz MapReduce feladat futtatható, figyelheti a projekt állapotát, valamint a kimenet.

##<a id="nextsteps"></a>Következő lépések

A HDInsight MapReduce feladatok általános információt:

* [HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)
