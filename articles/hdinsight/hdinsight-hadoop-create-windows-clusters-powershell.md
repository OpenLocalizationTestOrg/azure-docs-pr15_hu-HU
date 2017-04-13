<properties
   pageTitle="Windows-alapú Hadoop fürt létrehozása az Azure PowerShell használatával HDInsight |} Microsoft Azure"
    description="Megtudhatja, hogy miként fürt létrehozása az Azure hdinsight szolgáltatáshoz Azure PowerShell használatával."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Windows-alapú Hadoop fürt létrehozása az Azure PowerShell használatával hdinsight szolgáltatáshoz

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Megtudhatja, hogyan hozhat létre HDInsight fürt Azure PowerShell használatával. Azure PowerShell modul, amely tartalmaz kezelése Azure a Windows PowerShell-parancsmagok. Más fürt létrehozása eszközök és szolgáltatások lapon kattintson a lap tetején lévő vagy [fürt létrehozási módszerek](hdinsight-provision-clusters.md#cluster-creation-methods)látható.


##<a name="prerequisites"></a>Előfeltételek:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Ez a cikk utasításait megkezdése előtt a következőket kell rendelkeznie:

- Azure előfizetés. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Fürt létrehozása
Azure PowerShell olyan hatékony parancsfájlok környezet ellenőrzése és a telepítési és Azure-ban a feladatok kezelésének automatizálásához használható. Ez a szakasz ismertető egy HDInsight fürthöz létrehozása az Azure PowerShell használatával. HDInsight Windows PowerShell-parancsmagok futtatásához munkaállomás konfigurálásával kapcsolatos további tudnivalókért lásd [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md). Azure PowerShell használata HDInsight további tájékoztatást talál [HDInsight kezelése a PowerShell használatával](hdinsight-administer-use-powershell.md) A HDInsight Windows PowerShell-parancsmagok listájában a [HDInsight parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/dn858087.aspx)megtekintése


Az alábbi eljárások valamelyikét egy HDInsight fürthöz létrehozása az Azure PowerShell használatával van szükség:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>ARM-sablon segítségével fürt létrehozása

Azure PowerShell telepítése egy ARM sablont, amely hoz létre egy HDInsight fürthöz is használhatja.  Lásd: a [hívás sablonok Azure PowerShell használatával](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Fürt testreszabása

- Lásd: [testreszabása HDInsight fürt betöltő használatával](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Lásd: [testre szabhatja a Windows-alapú HDInsight fürt parancsfájl művelettel](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Következő lépések
Ebben a cikkben többféle módon is létre HDInsight fürt megtanulta azt van. További tudnivalókért lásd: az alábbi cikkekben:

* [Azure hdinsight szolgáltatáshoz – első lépések](hdinsight-hadoop-linux-tutorial-get-started.md) – megtudhatja, hogy miként kezdheti el a munkát a HDInsight fürthöz
* [Elküldése Hadoop feladatok programozás útján](hdinsight-submit-hadoop-jobs-programmatically.md) – megtudhatja, hogy miként programozás útján elküldése HDInsight-feladatok
* [A PowerShell használatá HDInsight kezelése a Hadoop fürt](hdinsight-administer-use-powershell.md) - megtudhatja, hogy miként dolgozhat a HDInsight Azure PowerShell használatával
* [Azure hdinsight szolgáltatáshoz SDK dokumentáció]  [ hdinsight-sdk-documentation] – a HDInsight SDK felfedezése




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
