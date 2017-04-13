<properties
    pageTitle="Hadoop, HBase, vihar vagy külső fürt létrehozása az Azure PowerShell használatával HDInsight Linux |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre Hadoop, HBase, vihar vagy külső fürt Linux HDInsight az Azure PowerShell használatával."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Linux-alapú fürt létrehozása HDInsight Azure PowerShell használatával

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell olyan hatékony parancsfájlok környezet ellenőrzése és a telepítési és a Microsoft Azure-ban a feladatok kezelésének automatizálásához használható. A dokumentum Linux-alapú HDInsight fürt létrehozása az Azure PowerShell használatával kapcsolatos információkat tartalmaz. Példa parancsfájl is tartalmaz.

> [AZURE.NOTE] Azure PowerShell Windows-ügyfélalkalmazásokon kizárólag érhető el. Ha egy Linux rendszerhez, a Unix vagy a Mac OS X ügyfél használja, olvassa el a [használatával Azure CLI HDInsight Linux-alapú fürt létrehozása](hdinsight-hadoop-create-linux-clusters-azure-cli.md) fürt létrehozása az Azure CLI használatáról.

## <a name="prerequisites"></a>Előfeltételek
Az eljárás megkezdése előtt a következőket kell rendelkeznie:

- Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Azure PowerShell használata HDInsight kapcsolatos további tudnivalókért olvassa el a [HDInsight kezelése a PowerShell használatával](hdinsight-administer-use-powershell.md)című témakört. HDInsight Windows PowerShell-parancsmagok listáját olvassa el a [HDInsight parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/dn858087.aspx)című témakört.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Fürt létrehozása

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Szeretne létrehozni egy HDInsight fürthöz Azure PowerShell használatával, hajtsa végre az alábbi eljárások valamelyikét:

- Azure erőforrás-ös csoport létrehozása
- Azure tároló fiók létrehozása
- Hozzon létre egy Azure Blob-tárolóhoz
- Hozzon létre egy HDInsight fürthöz

A két legfontosabb paraméterek, meg kell adni a Linux fürt létrehozása lépnek adja meg az operációs rendszer típusa és a felhasználó SSH adatai:

- Győződjön meg arról, hogy a **- OSType** paramétert ad meg **Linux**.
- Kattintson a fürt távoli munkamenetet SSH használatához a SSH felhasználó jelszavát vagy a SSH nyilvános kulcs is megadhat. Ha a SSH felhasználó jelszavát és a nyilvános kulcshoz SSH ad meg, a kulcs figyelmen kívül hagyja. Ha szeretne a SSH billentyűvel a távoli folyamatokhoz, meg kell adnia egy üres SSH jelszót a megjelenő párbeszédpanelen az egyik. HDInsight SSH használatáról további tudnivalókért lásd: az alábbi cikkekben:

    * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

A következő parancsfájl bemutatja, hogyan hozhat létre új fürt:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Az értékek **$clusterCredentials** megadhatja a fürt Hadoop felhasználói fiók létrehozása szolgálnak. Ehhez a fiókhoz segítségével csatlakozzon a fürthöz.

A fürt felhasználó SSH létrehozásához használt **$sshCredentials** megadott értékeket. Ehhez a fiókhoz segítségével indítása távoli SSH a fürt, és futtassa a feladatokat.

> [AZURE.IMPORTANT] Ez a parancsfájl, amely a fürt dolgozó csomópontok számának kell megadnia. Ha több mint 32 dolgozó-csomópont (vagy fürt létrehozását a méretezési a csoport létrehozását követően) használni kívánja, meg kell adnia egy központi csomópont méretének és legalább 8 magmintákat 14 GB RAM.
>
> Csomópont méretét és a kapcsolódó költségek a további tudnivalókért olvassa el a [HDInsight árak](https://azure.microsoft.com/pricing/details/hdinsight/)című témakört.

Legfeljebb 20 perc fürt létrehozása is eltarthat.

A következő példa bemutatja, hogyan lehet további tárterületet fiók hozzáadása:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Fürt testreszabása

- Lásd: [testreszabása HDInsight fürt betöltő használatával](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Lásd: [testre szabhatja a Windows-alapú HDInsight fürt parancsfájl művelettel](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>A csoport törlése

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozott egy HDInsight fürthöz, az alábbi források segítségével megtudhatja, hogy miként dolgozhat a fürt.

### <a name="hadoop-clusters"></a>Hadoop fürt

* [HDInsight struktúra használata](hdinsight-use-hive.md)
* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [HDInsight MapReduce használata](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase fürt

* [A HDInsight HBase – első lépések](hdinsight-hbase-tutorial-get-started-linux.md)
* [A HDInsight HBase az Java-alkalmazások fejlesztői számára](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Vihar fürt

* [Fejleszthet olyan Java topológiák vihar a hdinsight szolgáltatáshoz](hdinsight-storm-develop-java-topology.md)
* [A HDInsight vihar Python összetevők használata](hdinsight-storm-develop-python-topology.md)
* [Üzembe helyezéséhez és a HDInsight a vihar topológiák figyelése](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>A külső fürt

* [Scala használatával standalone-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)
* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)
* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)
* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)
