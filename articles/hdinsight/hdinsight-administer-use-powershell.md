<properties
    pageTitle="Kezelése a Hadoop fürt hdinsight szolgáltatáshoz a PowerShell használatával a |} Microsoft Azure"
    description="Megtudhatja, hogy miként felügyeleti feladatok a Hadoop fürtre az Azure PowerShell használatával HDInsight vonatkozóan."
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
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Kezelése a Hadoop fürt HDInsight az Azure PowerShell használatával

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell olyan hatékony parancsfájlok környezet ellenőrzése és a telepítési és Azure-ban a feladatok kezelésének automatizálásához használható. Ebben a cikkben megtanulhatja kezelése a Hadoop fürt Azure hdinsight szolgáltatáshoz a Windows PowerShell használatával helyi Azure PowerShell konzolon használatával. A HDInsight PowerShell-parancsmagok listájáért lásd a [HDInsight parancsmagjai – referencia][hdinsight-powershell-reference].



**Előfeltételek**

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

##<a name="install-azure-powershell"></a>Azure PowerShell telepítése

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Ha telepítette az Azure PowerShell verzió 0,9 x, távolítsa el a újabb verzió telepítése előtt.

A telepített PowerShell verziójának ellenőrzése:

    Get-Module *azure*
    
Távolítsa el a korábbi verziót, futtassa a programok és szolgáltatások a Vezérlőpultról. 


##<a name="create-clusters"></a>Fürt létrehozása

Lásd: [létrehozása Linux-alapú fürt a HDInsight Azure PowerShell használatával](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

##<a name="list-clusters"></a>Lista fürt
A következő parancsot használja az aktuális előfizetés minden fürt listáját:

    Get-AzureRmHDInsightCluster

##<a name="show-cluster"></a>Megjelenítés csoport

A következő parancs használatával egy adott fürt részletek megjelenítése az aktuális előfizetés:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

##<a name="delete-clusters"></a>Fürt törlése

A következő parancsot használja a fürtre törlése:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

Az erőforráscsoport, amely tartalmazza a fürt eltávolításával fürt is törölheti. Felhívjuk a figyelmét arra, beleértve az alapértelmezett tároló fiók csoportjában található összes erőforrás törlődnek.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>
            
##<a name="scale-clusters"></a>Méretezés fürt
A méretezés szolgáltatás fürt lehetővé teszi az Azure hdinsight szolgáltatáshoz a anélkül, hogy hozza létre újból a fürt futtató fürt által használt dolgozó csomópontok számának módosítása.

>[AZURE.NOTE] Csak a HDInsight verzió 3.1.3 fürtök vagy magasabb támogatottak. Ha biztos benne, hogy a fürt verzióját, érdemes tulajdonságok lapon.  Lásd: a [listában, és a Megjelenítés fürt](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

A HDInsight által támogatott fürt minden típusú adat csomópontok számának módosításának hatása:

- Hadoop

    Zökkenőmentes növelésével futtató érintő bármely várakozó vagy futó feladatok nélkül Hadoop fürt dolgozó csomópontok számának. Új feladat is benyújtandó, miközben folyamatban van a műveletet. Méretezési művelet sikertelen biztonságosan kezelése, hogy a fürt mindig marad funkcionális állapotba kerül.

    Amikor Hadoop fürt adatok csomópontok számának csökkentésével átméretezi, egyes szolgáltatások a fürt vannak indítani. Hatására minden futó és a feladatok függőben befejezésekor a méretezési művelet sikertelen lesz. Akkor is, azonban küldje el újra a feladatokat a művelet befejezése után.

- HBase

    Zökkenőmentes felvehet, és távolítsa el a HBase fürt csomópontok futása közben is. A területi kiszolgálók is automatikusan meghatározni a méretezési művelet befejezése néhány percen belül. Jó helyen jár akkor is manuálisan is egyenleg a területi kiszolgálók bejelentkezés a fürt a headnode és futtatása a következő parancsok végrehajtása egy parancssorablakot:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- Vihar

    Zökkenőmentes felvehet, és távolítsa el a vihar fürt adatok csomópontok futása közben is. De a méretezési művelet sikeres befejezését követően a topológia visszaállás kell.

    Szakismeretekre kétféle módon lehet elvégezni:

    * Vihar webes felhasználói felület
    * Parancssori kezelőfelületről eszköz

    Olvassa el a [Apache vihar dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további információt.

    A HDInsight fürt vihar webes felhasználói felület érhető el:

    ![HDInsight vihar skála rebalance](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Íme egy példa a vihar topológia visszaállás a CLI parancs használatával hogyan:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Azure PowerShell használatával a Hadoop fürt méretének módosításához a következő parancsot a egy ügyfélszámítógépre:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
    

##<a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása

HDInsight fürt rendelkezik az alábbi HTTP webszolgáltatásokhoz (az összes az alábbi szolgáltatások vannak RESTful végpontok):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Az access alapértelmezés szerint az alábbi szolgáltatások vannak nyújtani. Akkor is revoke/támogatás az access. Ha vissza szeretné vonni:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

Adni:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"
    
    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

>[AZURE.NOTE] A hozzáférés megadását és visszavonása, visszaállítja, a fürt felhasználó nevét és jelszavát.

Ez a portálon keresztül is elvégezhető. Lásd: [Az Azure portál használatával felügyelheti HDInsight][hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>HTTP-felhasználói hitelesítő adatok frissítése

Ugyanezt az eljárást, mint [támogatás/revoke HTTP access](#grant/revoke-access). Ha a fürt hozzáférést kapott a HTTP, ezt kell először csak azt.  És majd hozzáférést az új HTTP-felhasználói hitelesítő adatokkal.


##<a name="find-the-default-storage-account"></a>Keresse meg az alapértelmezett tárterület-fiók

Az alábbi Powershell parancsprogramot bemutatja, hogyan lehet az alapértelmezett tároló nevét és az alapértelmezett tároló fiókkulcs fürt.

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 

##<a name="find-the-resource-group"></a>Az erőforráscsoport megkeresése

Az erőforrás-kezelő módban minden HDInsight fürt tartozik egy Azure erőforráscsoport.  Az erőforráscsoport megkeresése:

    $clusterName = "<HDInsight Cluster Name>"
    
    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


##<a name="submit-jobs"></a>Feladatok elküldése

**MapReduce feladatok elküldése**

Lásd: a [Windows-alapú HDInsight minták Hadoop MapReduce futtatása](hdinsight-run-samples.md).

**Struktúra feladatok elküldése** 

[A PowerShell használatával futtatása struktúra lekérdezések](hdinsight-hadoop-use-hive-powershell.md)témakörben talál.

**Malac feladatok elküldése**

Lásd: [malac futtatása feladatok PowerShell használatával](hdinsight-hadoop-use-pig-powershell.md).

**Sqoop feladatok elküldése**

Lásd: [használata Sqoop a hdinsight szolgáltatásból lehetőségre](hdinsight-use-sqoop.md).

**Oozie feladatok elküldése**

Lásd: [A Hadoop meghatározása és HDInsight a munkafolyamat futtatása használata Oozie](hdinsight-use-oozie.md).

##<a name="upload-data-to-azure-blob-storage"></a>Töltse fel az adatok Azure Blob-tárolóhoz
Lásd: az [adatok HDInsight feltöltése][hdinsight-upload-data].


## <a name="see-also"></a>Lásd még:
* [HDInsight parancsmag hivatkozási dokumentáció][hdinsight-powershell-reference]
* [HDInsight felügyelete a Azure portál használatával][hdinsight-admin-portal]
* [A parancssor használatával HDInsight felügyelete][hdinsight-admin-cli]
* [HDInsight fürt létrehozása][hdinsight-provision]
* [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]
* [Hadoop feladatok programozás útján elküldése][hdinsight-submit-jobs]
* [Első lépések az Azure hdinsight szolgáltatáshoz][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: powershell-install-configure.md

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
