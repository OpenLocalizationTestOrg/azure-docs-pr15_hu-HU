<properties
   pageTitle="Windows-alapú Hadoop fürt létrehozása a használatával Azure CLI hdinsight szolgáltatáshoz"
    description="Megtudhatja, hogy miként hozhat létre a fürt Azure hdinsight szolgáltatáshoz az Azure CLI használatával."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Windows-alapú Hadoop fürt létrehozása a használatával Azure CLI hdinsight szolgáltatáshoz

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Megtudhatja, hogyan hozhat létre használatával Azure CLI hdinsight szolgáltatáshoz fürt. Más fürt létrehozása eszközök és szolgáltatások lapon kattintson a lap tetején lévő vagy [fürt létrehozási módszerek](hdinsight-provision-clusters.md#cluster-creation-methods)látható.

##<a name="prerequisites"></a>Előfeltételek:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Ez a cikk utasításait megkezdése előtt a következőket kell rendelkeznie:

- **Azure előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Azure csatlakoztatása

A következő parancsot használja Azure csatlakozhat:

    azure login

Hitelesítés, munkahelyi vagy iskolai fiókkal kapcsolatos további tudnivalókért olvassa el a [Csatlakozás az Azure CLI az Azure-előfizetésbe](../xplat-cli-connect.md).

A következő parancsot használja a ARM módra válthat:

    azure config mode arm

Ha segítségre van szüksége, a **h –** kapcsoló használatával.  Példa:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Fürt létrehozása

Egy Azure erőforrás-kezelés (ARM), és egy Azure Blob-tároló fiókkal kell rendelkeznie, egy HDInsight fürthöz létrehozása előtt. Hozzon létre egy HDInsight fürthöz, adja meg a következőket:

- **Azure erőforráscsoport**: Azure erőforráscsoport belül A adatok tó Analytics-fiókot kell létrehozni. Azure erőforrás-kezelő lehetővé teszi, hogy az erőforrások az alkalmazás csoportosan kezelheti. Telepítése, frissítése, és egyetlen, összehangolt műveletben az alkalmazás törlése az összes erőforrás.

    Az erőforrás-csoportokat, az előfizetése listáját:

        azure group list

    Új erőforráscsoport létrehozása:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **HDInsight fürt neve**

- **Hely**: HDInsight fürt támogató Azure adatközpontokban közül. Támogatott helyeken, című témakör [árak hdinsight szolgáltatásból lehetőségre](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Az alapértelmezett tároló fiók**: HDInsight-Azure Blob-tároló tároló használja az alapértelmezett fájlrendszer. Az Azure tárterület-fiókra szükség, egy HDInsight fürthöz létrehozása előtt.

    Azure tároló új fiók létrehozása:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]A tárterület-fiókot kell kell collocated az az Adatközpont a hdinsight szolgáltatásból lehetőségre.
    > A tároló fióktípus nem lehet ZRS, mert ZRS nem támogatja a táblázatot.

    Azure tároló fiók létrehozása az Azure-portálon a további tudnivalókért lásd [létrehozása, kezelése és tároló fiók törlése] [azure-létrehozása-storageaccount].

    Ha már rendelkezik fiókkal tárhely, de nem ismeri a fiók nevét, és a fiókkulcs, az alábbi parancsokkal az adatok beolvasásához:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    További információt az Azure portál használatával ahhoz, hogy az adatokat olvassa el a [kapcsolatos Azure tárterület-fiókok](../storage/storage-create-storage-account#manage-your-storage-account)"Tárterület-fiókjának kezelését" szakaszát.

- **(Nem kötelező) alapértelmezett Blob-tárolóhoz**: az **azure hdinsight szolgáltatáshoz fürt létrehozása** parancs a tároló hoz létre, ha még nem létezik. Ha úgy dönt, hogy a tárolóra előre létrehozása, a következő parancsot is használhatja:

    Azure tároló tároló létrehozása – fiók neve "<Storage Account Name>" – fiókkulcs <Storage Account Key> [ContainerName]

Miután készített tárterület-fiókkal rendelkezik, készen áll a fürt létrehozása:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Konfigurációs fájl használatával fürt létrehozása
Általában, hozzon létre egy HDInsight fürthöz, feladat futtatható, és törölje a fürt gyorsítják a költség. A parancssor felajánlja a beállításokat egy fájlba menti, hogy újra felhasználhatja azt, minden alkalommal, amikor fürt létrehozása.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Példa: A fürtre létrehozásakor futtatásához parancsfájl műveletet tartalmazó konfigurációs fájl létrehozása.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Parancsfájl műveletet tartalmazó fürt létrehozása

Parancsfájl műveletet szeretné futtatni fürt létrehozása tartalmazó konfigurációs fájl létrehozása.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Az parancsfájl műveletet fürt létrehozása

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Általános parancsfájl művelet információt című témakörben talál [testreszabása HDInsight fürt parancsfájl művelettel (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>ARM-sablonok használata fürt létrehozása

ARM-sablonok meghívásával fürt létrehozásához CLI is használhatja. Lásd: [Azure CLI üzembe helyezéséhez](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Lásd még:

- [Azure hdinsight szolgáltatáshoz – első lépések](hdinsight-hadoop-linux-tutorial-get-started.md) – megtudhatja, hogy miként kezdheti el a munkát a HDInsight fürthöz
- [Elküldése Hadoop feladatok programozás útján](hdinsight-submit-hadoop-jobs-programmatically.md) – megtudhatja, hogy miként programozás útján elküldése HDInsight-feladatok
- [Kezelése a Hadoop fürt a HDInsight az Azure CLI használatával](hdinsight-administer-use-command-line.md)
- [Az Azure CLI Mac Linux és a Windows Azure szolgáltatás kezelése és használata](../virtual-machines-command-line-tools.md)
