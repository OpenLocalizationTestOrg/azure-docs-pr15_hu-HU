<properties
   pageTitle="Olyan adatokat másol és onnan WASB Distcp használatával tó adattár |} Microsoft Azure"
   description="Adatainak másolása és az Azure BLOB-tároló tó adattár Distcp eszközzel"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Azure BLOB-tároló és tó adattár közötti adatok másolása Distcp használatával

> [AZURE.SELECTOR]
- [DistCp használata](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy használata](data-lake-store-copy-data-azure-storage-blob.md)


Miután létrehozott egy HDInsight fürthöz tó adattár fiókhoz hozzáféréssel rendelkező, Hadoop ökológiai eszközök, például a Distcp adatok másolása **és onnan** egy HDInsight fürt tároló (WASB) tó adattár-fiókba is használhatja. Ez a cikk ismertető cél.

##<a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- **Az Azure előfizetés engedélyezése** tó adattár nyilvános előzetes. [Útmutatást](data-lake-store-get-started-portal.md#signup)találhat.
- **Azure hdinsight szolgáltatáshoz fürt** az Access-szel tó adattár-fiókjába. Lásd: [létrehozása egy HDInsight fürthöz tó áruházzal](data-lake-store-hdinsight-hadoop-use-portal.md). Győződjön meg arról, hogy a fürt engedélyezése a távoli asztal.

## <a name="do-you-learn-fast-with-videos"></a>Megismerheti egy videók tegye?

[Ebből a videóból](https://mix.office.com/watch/1liuojvdx6sie) adatok Azure BLOB-tároló és tó adattár közötti másolásáról DistCp használatával.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Távoli asztal (Windows fürthöz) vagy (Linux fürthöz) SSH Distcp használata

Egy HDInsight fürthöz megtalálható a Distcp segédprogramot, amely a különböző forrásokból származó adatokat másol egy HDInsight fürthöz használható. A HDInsight fürt adatok tó tároló használatára, egy további tárterületet állította be, ha a Distcp segédprogram lehet használt kimenő az-kész másolhatja az adatokat, és a tó adattár-fiókból is. Ez a szakasz azt megnézheti a Distcp segédprogram használata.

1. Ha a Windows fürtre, távoli be egy HDInsight fürtre, amely hozzáféréssel rendelkezik tó adattár-fiókjába. Útmutatásért lásd: [Csatlakozás fürt RDP segítségével](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). A fürt asztali nyissa meg a Hadoop parancsot.

    Ha a Linux fürtre, SSH segítségével csatlakozzon a fürthöz. Olvassa el a [Csatlakozás a Linux-alapú HDInsight fürtre](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). A parancsokat SSH parancssorából.

3. Győződjön meg arról, hogy az Azure tároló BLOB (WASB) is elérheti. A következő parancsot:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Ez biztosítania kell a tároló blob tartalma listáját.

4. Hasonlóképpen ellenőrizze, hogy érheti el a tó adattár fiókot a fürtre. A következő parancsot:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    A fájlok és mappák tó adattár fiók listájának biztosítania kell.

5. Distcp segítségével adatok másolása WASB tó adattár-fiókjába.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Ez **** másolja/Példa/adatok/gutenberg/mappa tartalmát az WASB **/myfolder** tó adattár fiók.

6. Hasonlóképpen Distcp segítségével tó adattár fiók adatainak másolása WASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Ez átvezeti **/myfolder** tartalmának tó adattár **** fiók/Példa/adatok/gutenberg/WASB mappájában.

## <a name="see-also"></a>Lásd még:

- [Adatok másolása Azure BLOB-tároló tó adattárhoz](data-lake-store-copy-data-azure-storage-blob.md)
- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
