<properties
    pageTitle="Kezelése a Hadoop fürt hdinsight szolgáltatáshoz a .NET SDK használatával a |} Microsoft Azure"
    description="Megtudhatja, hogy miként felügyeleti feladatok a Hadoop fürt a HDInsight HDInsight .NET SDK használata esetében."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    tags="azure-portal"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Kezelése a Hadoop fürt HDInsight a .NET SDK használatával

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Megtudhatja, hogy miként kezelheti a HDInsight fürt [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)használatával.


**Előfeltételek**

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: az [első Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).


##<a name="connect-to-azure-hdinsight"></a>Csatlakozás Azure hdinsight szolgáltatáshoz

A következő Nuget csomagokban lesz szüksége:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

A következő kódot példa bemutatja, hogyan való csatlakozáshoz Azure HDInsight fürt felügyelheti az Azure-előfizetése előtt.

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
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
        }
    }

Kérdés kell látni, amikor a program futtatja.  Ha nem szeretné látni a kérdésre, olvassa el a [létrehozása interaktív hitelesítés .NET HDInsight-alkalmazások](hdinsight-create-non-interactive-authentication-dotnet-applications.md)című témakört.

##<a name="create-clusters"></a>Fürt létrehozása

Lásd: [létrehozása Linux-alapú fürt a HDInsight a .NET SDK használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

##<a name="list-clusters"></a>Lista fürt

A következő kódrészletet sorolja fel, fürt és az egyes tulajdonságai:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

##<a name="delete-clusters"></a>Fürt törlése

A következő kódrészletet segítségével törölheti a fürt aszinkron vagy szinkron: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
            
##<a name="scale-clusters"></a>Méretezés fürt
A méretezés szolgáltatás fürt az Azure hdinsight szolgáltatáshoz a anélkül, hogy hozza létre újból a fürt futtató fürt által használt dolgozó csomópontok számának módosítása teszi lehetővé.

>[AZURE.NOTE] Csak a HDInsight verzió 3.1.3 fürtök vagy magasabb támogatottak. Ha biztos benne, hogy a fürt verzióját, érdemes tulajdonságok lapon.  Lásd: a [listában, és a Megjelenítés fürt](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

A HDInsight által támogatott fürt minden típusú adat csomópontok számának módosításának hatása:

- Hadoop

    Zökkenőmentes növelésével futtató érintő bármely várakozó vagy futó feladatok nélkül Hadoop fürt dolgozó csomópontok számának. Új feladat is benyújtandó, miközben folyamatban van a műveletet. Méretezési művelet sikertelen biztonságosan kezelése, hogy a fürt mindig marad funkcionális állapotba kerül.

    Amikor a Hadoop fürtre adatok csomópontok számának csökkentésével átméretezi, egyes szolgáltatások a fürt újraindítja. Hatására minden futó és a feladatok függőben befejezésekor a méretezési művelet sikertelen lesz. Akkor is, azonban küldje el újra a feladatokat a művelet befejezése után.

- HBase

    Zökkenőmentes felvehet, és távolítsa el a HBase fürt csomópontokat futása közben is. A területi kiszolgálók is automatikusan meghatározni a méretezési művelet befejezése néhány percen belül. Azonban Ön is manuálisan is egyenleg a területi kiszolgálók a headnode fürt bejelentkezés, és futtatása a következő parancsok végrehajtása egy parancssorablakot:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- Vihar

    Zökkenőmentes felvehet, és távolítsa el a vihar fürt adatok csomópontok futása közben is. De a méretezési művelet sikeres befejezését követően a topológia visszaállás kell.

    Szakismeretekre kétféle módon lehet elvégezni:

    * Vihar webes felhasználói felület
    * Parancssori kezelőfelületről eszköz

    További információt a [Apache vihar dokumentációjában](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) találhat.

    A HDInsight fürt vihar webes felhasználói felület érhető el:

    ![hdinsight vihar skála rebalance](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Íme egy példa a vihar topológia visszaállás a CLI parancs használatával hogyan:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

A következő kódrészletet aszinkron vagy szinkron, méretezze át a fürtre mutatja be:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    

##<a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása

HDInsight fürt rendelkezik az alábbi HTTP webszolgáltatásokhoz (az összes az alábbi szolgáltatások vannak RESTful végpontok):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Az access alapértelmezés szerint az alábbi szolgáltatások vannak nyújtani. Akkor is revoke/támogatás az access. Ha vissza szeretné vonni:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

Adni:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


>[AZURE.NOTE] A hozzáférés megadását és visszavonása, visszaállítja, a fürt felhasználó nevét és jelszavát.

Ez a portálon keresztül is elvégezhető. Lásd: [Az Azure portál használatával felügyelheti HDInsight][hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>HTTP-felhasználói hitelesítő adatok frissítése

Ugyanezt az eljárást, mint [támogatás/revoke HTTP access](#grant/revoke-access). Ha a fürt hozzáférést kapott a HTTP, ezt kell először csak azt.  És majd hozzáférést az új HTTP-felhasználói hitelesítő adatokkal.


##<a name="find-the-default-storage-account"></a>Keresse meg az alapértelmezett tárterület-fiók

A következő kódrészletet bemutatja, hogyan lehet az alapértelmezett tároló nevét és az alapértelmezett tároló fiókkulcs fürt.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


##<a name="submit-jobs"></a>Feladatok elküldése

**Küldhetik MapReduce feladatok**

Lásd: [futtatása Hadoop MapReduce minták hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-run-samples-linux.md).

**Struktúra feladatok elküldése** 

Lásd: [futtassa a struktúra lekérdezések .NET SDK csomagjában talál](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**Malac feladatok elküldése**

Lásd: [malac futtatása feladatok .NET SDK használatával](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**Sqoop feladatok elküldése**

Lásd: [használata Sqoop a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).

**Oozie feladatok elküldése**

Lásd: [A Hadoop meghatározása és HDInsight a munkafolyamat futtatása használata Oozie](hdinsight-use-oozie-linux-mac.md).

##<a name="upload-data-to-azure-blob-storage"></a>Töltse fel az adatok Azure Blob-tárolóhoz
Lásd: az [adatok HDInsight feltöltése][hdinsight-upload-data].


## <a name="see-also"></a>Lásd még:
* [HDInsight .NET SDK hivatkozási dokumentáció](https://msdn.microsoft.com/library/mt271028.aspx)
* [HDInsight felügyelete a Azure portál használatával][hdinsight-admin-portal]
* [A parancssor használatával HDInsight felügyelete][hdinsight-admin-cli]
* [HDInsight fürt létrehozása][hdinsight-provision]
* [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]
* [Első lépések az Azure hdinsight szolgáltatáshoz][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


