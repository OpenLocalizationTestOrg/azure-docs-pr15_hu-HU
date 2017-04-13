<properties
    pageTitle="Parancsfájl-műveletek használata HDInsight fürt testreszabása |} Microsoft Azure"
    description="Megtudhatja, hogy miként parancsfájl művelettel HDInsight fürt testreszabása."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Windows-alapú HDInsight fürt parancsfájl művelettel testreszabása

[Egyéni parancsfájlok](hdinsight-hadoop-script-actions.md) meghívása fürtre külön szoftver telepítése fürt létrehozásának folyamata során **Parancsfájl műveletet** használható.

Ez a cikk a Windows-alapú HDInsight fürt jellemző látható. Linux-alapú fürt [parancsfájl művelettel testreszabása Linux-alapú HDInsight fürt](hdinsight-hadoop-customize-cluster-linux.md)talál. 

HDInsight fürt testre szabható számos más módon is, például többek között a további tárterület Azure-fiókok, módosíthatják a Hadoop konfigurációs fájljai (core-site.xml, struktúra-site.xml stb.) vagy a megosztott tárak (például a struktúra, a Oozie) hozzáadása a fürt közös helyek be. Ezekre a beállításokra történik Azure PowerShell, az Azure hdinsight szolgáltatáshoz .NET SDK vagy az Azure-portálon keresztül. További tudnivalókért lásd: [a HDInsight létrehozása Hadoop fürt][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Parancsfájl műveletet a fürt létrehozásának folyamata

Parancsfájl műveletet csak akkor használható, miközben egy fürt létrehozása folyamatban van. A következő diagramon azt mutatja be, amikor parancsfájl művelet végrehajtása a létrehozási folyamat során:

![HDInsight fürt testreszabás és a szakaszok fürt létrehozása során.][img-hdi-cluster-states]

A parancsfájl fut, a a fürt beírja a **ClusterCustomization** szakaszban. Ebben a szakaszban a parancsfájl a rendszerfiókkal rendszergazda, a fürt, minden megadott csomóponton párhuzamosan fut, és ez a témakör teljes rendszergazdai jogosultságait csomópontok.

> [AZURE.NOTE] Rendszergazdai jogosultságait fürt csomópontok tartalmaz **ClusterCustomization** szakaszában, mert a parancsfájl műveleteket, például a szolgáltatást, többek között a Hadoop kapcsolatos szolgáltatások indítása és leállítása is használhatja. Igen, a parancsfájlt győződjön meg arról, hogy a Ambari és egyéb Hadoop kapcsolatos szolgáltatásokat is lépéseket, mielőtt a parancsfájl futásának. Az alábbi szolgáltatások sikeresen megállapítása az állapot és a fürt állapotát, akkor létrehozása közben van szüksége. Ha módosítja a fürt, amely hatással van az alábbi szolgáltatások bármely konfigurációs, a segítő függvényeket kell használnia. Segítő függvényekkel kapcsolatos további tudnivalókért lásd: a [HDInsight kidolgozása parancsfájl művelet parancsfájlok][hdinsight-write-script].

A kimenet és a hibanaplóit, a parancsfájl az alapértelmezett tárterület-fiók a fürt megadott tárolja. A naplók tárolódnak nevű táblázat **u < \cluster-name-fragment >< \time-stamp > setuplog**. Ezek az összes csomóponton (fő csomópont és dolgozó csomópontok) a fürt futtatása forgatókönyv összesítő naplók.

Minden fürt elfogadhatja abban a sorrendben, amelyben megadott meghívott több parancsfájl-műveletek. Egy parancsprogramot is kell adódott a központi csomópontot, a dolgozó csomópontok vagy mindkettőt.

HDInsight az alábbi összetevők telepítése HDInsight fürt több parancsfájlok nyújtja:

név | Parancsfájl
----- | -----
**A külső telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Lásd: a [telepítési és használati dokumentuma a HDInsight fürt][hdinsight-install-spark].
**R telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Lásd: a [telepítés és a HDInsight fürt R használata][hdinsight-install-r].
**Solr telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Lásd: [telepítése és használata a HDInsight Solr fürtök](hdinsight-hadoop-solr-install.md).
- **Giraph telepítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Lásd: [telepítése és használata a HDInsight Giraph fürtök](hdinsight-hadoop-giraph-install.md).
| **Előre a struktúra tárak betöltése** | https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Lásd: [a HDInsight fürt hozzáadása struktúra tárak](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Hívás parancsfájlokat az Azure-portálon

**Az Azure portálról**

1. Indítsa el a fürt létrehozása [a HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md#portal)leírtak.
2. Választható beállítás csoportban a **Parancsfájl-műveletek** lap kattintva **parancsfájl művelet hozzáadása** a parancsfájl műveletet részleteinek megadására alább látható módon:

    ![Parancsfájl műveletek fürt testreszabása] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Parancsfájl műveletek fürt testreszabása")

    <table border='1'>
        <tr><th>A tulajdonság</th><th>Érték</th></tr>
        <tr><td>név</td>
            <td>Adja meg a parancsfájlt beavatkozásra.</td></tr>
        <tr><td>Parancsfájl URI</td>
            <td>Adja meg a parancsfájlt, ha testre szeretné szabni a fürt hív meg a URI. s</td></tr>
        <tr><td>Címsor/dolgozó</td>
            <td>A testreszabási parancsfájl futtatásához adja meg a csomópontok (**fej** vagy **dolgozó**). </b>.
        <tr><td>Paraméterek</td>
            <td>Szükség szerint a parancsfájlt, adja meg a paraméterek.</td></tr>
    </table>

    Nyomja le az ENTER billentyűt a fürt több összetevők telepítése egynél több parancsfájl művelet hozzáadása.

3. Kattintson a **Jelölje ki** a parancsprogram művelet konfiguráció mentéséhez, és fürt létrehozása folytatásához.

## <a name="call-scripts-using-azure-powershell"></a>Hívás parancsfájlok Azure PowerShell használatával

Ez az alábbi PowerShell parancsprogramot bemutatja, hogyan lehet a külső telepíthető Windows-alapú HDInsight fürt.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Egyéb az olyan szoftverek telepítése, szüksége lesz a parancsfájl script fájl cseréje:


Amikor a rendszer kéri, írja be a hitelesítő adatokat a fürt. Eltarthat néhány percig, amíg a fürt létrehozása előtt.

## <a name="call-scripts-using-net-sdk"></a>Hívás parancsfájlok .NET SDK használatával 

A következő példa bemutatja, hogyan lehet a külső telepíthető Windows-alapú HDInsight fürt. Egyéb az olyan szoftverek telepítése, szüksége lesz le szeretné cserélni a parancsprogram be a kódot.

**A külső egy HDInsight fürthöz létrehozásához** 

1. C# console-alkalmazás létrehozása a Visual Studióban.
2. A Nuget csomag Manager konzolról futtassa a következő parancsot.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Használja az alábbi utasítások az Program.cs fájl használatával:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Helyezze a kódot a osztály a következő:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Megnyitás-forrás szoftver HDInsight fürt használt támogatása
A Microsoft Azure HDInsight szolgáltatása, amely lehetővé teszi, hogy a felhőben nagy adatok alkalmazások készíthet Megnyitás-forrás technológiák formázott körül Hadoop-ökológiai használatával rugalmas platformot. Microsoft Azure támogatási általános szintű nyújt a Megnyitás-forrás technológiák, tárgyalt <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure támogatja a gyakori kérdések a webhely</a> **Támogatja a hatóköre** csoportban. A HDInsight szolgáltatás támogatása egyes összetevői, egy további szintet tartalmaz, az alábbiaknak.

A HDInsight szolgáltatásban elérhető összetevők Megnyitás-forrás két típusa van:

- **Beépített összetevők** – ezek az összetevők HDInsight fürt előre telepítve van, és a fürt alapvető funkcionalitást nyújt. Például fonal erőforrás-kezelő, a struktúra lekérdezésnyelv (HiveQL) és a Mahout tárat a kategóriához tartoznak. Egy teljes listát az fürt összetevők érhető el [a HDInsight által biztosított Hadoop fürt verzió újdonságai?](hdinsight-component-versioning.md) </a>.
- **Egyéni összetevő** -, az a fürt felhasználó telepítheti, vagy használja a terhelést a Közösség érhetők el, és Ön által létrehozott minden összetevő.

Beépített összetevők teljesen támogatott, és a Microsoft Support segítséget nyújt azonosíthatók, és az alábbi összetevőket kapcsolatos problémák megoldásához.

> [AZURE.WARNING] A HDInsight fürt összetevői teljesen támogatott, és Microsoft Support segítséget nyújt azonosíthatók, és az alábbi összetevőket kapcsolatos problémák megoldásához.
>
> Egyéni összetevők kap, akár kereskedelmi célra is lehetővé teszi ügyfélszolgálatunkat, további elhárítani a problémát. Ez a probléma megoldása vagy kéri, hogy a hol található az adott technológia mély szakértelmét Megnyitás technológiák elérhető csatornák folytasson vezethet. Például vannak sok közösségi webhelyek használható, például: [HDInsight-fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projekteket is projektwebhelyek a [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/), [külső](http://spark.apache.org/).

A HDInsight szolgáltatás többféleképpen is használhatja az egyéni összetevők. Hogyan összetevő használt vagy a fürt telepítve, függetlenül ugyanakkora terméktámogatási vonatkozik. Az alábbi képen a leggyakoribb módon, hogy használható-e egyéni összetevőinek a HDInsight fürt listája:

1. Feladat elküldése - Hadoop vagy más típusú feladatokat, amelyeket végrehajtása vagy egyéni összetevőket is elhelyez elküldhetők a fürthöz.
2. Fürt testreszabási - fürt létrehozása során megadható további beállításokat, és egyéni összetevőinek a fürt csomópontokat telepített.
3. Minta - népszerű egyéni összetevők, a Microsoft és másokkal, ezek az összetevők hogyan használható a HDInsight fürt a minták rendelkezhetnek. Ezek a minták nem támogató állnak rendelkezésre.

## <a name="develop-script-action-scripts"></a>Parancsfájl műveletet parancsfájlok kidolgozása

Lásd: [HDInsight kidolgozása parancsfájl művelet parancsfájlok][hdinsight-write-script].


## <a name="see-also"></a>Lásd még:

- [Hozzon létre Hadoop fürt HDInsight] [ hdinsight-provision-cluster] nyújt útmutatást, hogy miként hozhat létre egy HDInsight fürthöz más egyéni beállításokkal.
- [Fejleszthet olyan parancsfájl műveletet parancsfájlok hdinsight szolgáltatáshoz][hdinsight-write-script]
- [Telepítése és a külső HDInsight fürt használata][hdinsight-install-spark]
- [Telepítése és R HDInsight fürt használata][hdinsight-install-r]
- [Telepítési és használati Solr a HDInsight fürtök](hdinsight-hadoop-solr-install.md).
- [Telepítési és használati Giraph a HDInsight fürtök](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Szakaszok fürt létrehozása során."
