<properties
    pageTitle="Kezelése a Hadoop fürt Azure CLI használatával |} Microsoft Azure"
    description="Az Azure CLI használatáról a HDIsight Hadoop fürt kezelése"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Kezelése a Hadoop fürt a HDInsight az Azure CLI használatával

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Megtudhatja, hogy miként [Azure parancssor](../xplat-cli-install.md) segítségével kezelése a Hadoop fürt az Azure hdinsight szolgáltatásból lehetőségre. Az Azure CLI Node.js a megvalósított. Bármely platformon Node.js, beleértve a Windows, Mac és Linux támogató használható.

Ez a cikk bemutatja, hogy csak az Azure CLI használata hdinsight szolgáltatásból lehetőségre. Az általános útmutató Azure CLI használatát, lásd: [Telepítse és állítsa be az Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI** – lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) telepítési és konfigurálási információt.
- A **Csatlakozás az Azure**, az alábbi parancsot:

        azure login

    Hitelesítés, munkahelyi vagy iskolai fiókkal kapcsolatos további tudnivalókért olvassa el a [Csatlakozás az Azure CLI az Azure-előfizetésbe](xplat-cli-connect.md).
    
- **Az erőforrás-kezelő Azure módban váltás**a következő parancsot:

        azure config mode arm

Ha segítségre van szüksége, a **h –** kapcsoló használatával.  Példa:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Fürt létrehozása

Lásd: [létrehozása Linux-alapú fürt a használja az Azure CLI HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Lista és a Megjelenítés csoport részletei
Az alábbi parancsok használatával listára, és fürt részletek megjelenítése:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Fürt törlése

A következő parancsot használja a fürtre törlése:

    azure hdinsight cluster delete <Cluster Name>

Az erőforráscsoport, amely tartalmazza a fürt törlésével fürt is törölheti. Felhívjuk a figyelmét arra, beleértve az alapértelmezett tároló fiók csoportjában található összes erőforrás törlődnek.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Méretezés fürt

A Hadoop fürt méretének módosítása:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>HTTP-hozzáférést fürt engedélyezése vagy letiltása

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Fürt RDP hozzáférés engedélyezése vagy letiltása

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta van más HDInsight fürt felügyeleti feladatok elvégzéséhez. További tudnivalókért lásd: az alábbi cikkekben:

* [HDInsight felügyelheti a Azure portál használatával] [hdinsight-admin-portal]
* [HDInsight felügyelete Azure PowerShell használatával] [hdinsight-admin-powershell]
* [Első lépések az Azure hdinsight szolgáltatáshoz] [hdinsight-get-started]
* [Az Azure CLI használata] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Lista és a Megjelenítés fürt"
