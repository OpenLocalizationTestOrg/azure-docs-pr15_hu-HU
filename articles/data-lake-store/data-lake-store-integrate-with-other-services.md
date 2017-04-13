<properties
   pageTitle="Tó adattár integrálása más Azure szolgáltatások |} Microsoft Azure"
   description="Hogyan integrálja a tó adattár más Azure szolgáltatásokhoz ismertetése"
   documentationCenter=""
   services="data-lake-store"
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

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Tó adattár integrálása más Azure szolgáltatások

Azure tó adattár ahhoz, hogy a esetek szélesebb köre használható más Azure-szolgáltatásokkal együtt. A következő cikk tó adattár is integrálódik szolgáltatásokat sorolja fel.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Tó adattár használata az Azure hdinsight szolgáltatáshoz

Az [Azure hdinsight szolgáltatáshoz](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) fürtre Fájlrendszerhez-kompatibilis tárolására, tó adattár használó is kiépítése. Ebben a kiadásban a Windows és Linux, Hadoop és vihar fürtre vonatkozóan is használhatja tó adattár csak egy további tárterületet. Az ilyen fürt továbbra is használja az alapértelmezett tárolására Azure tároló (WASB). Jó helyen jár a Windows és Linux fürtre HBase, vonatkozóan is használhatja tó adattár alapértelmezett tárolására, vagy további tárterület, vagy mindkettőt.

Kattintson egy HDInsight fürtre tó áruházzal kiépítését című cikkben olvashat:

* [Egy HDInsight fürtre kiépítése az Azure-portálon tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Egy HDInsight fürtre kiépítése adatok tó tárolóval Azure PowerShell használatával](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Inkább a videók?** Hajtsa végre az alábbi hivatkozásokra kattintva a videókat a HDInsight fürt tó adattár használatára vonatkozó útmutatást.

* [Hozzon létre egy HDInsight fürthöz tó adattár hozzáféréssel rendelkező](https://mix.office.com/watch/l93xri2yhtp2)
* Miután a fürt beállította, [Access-adatok a struktúra, és malac parancsfájlokkal tó adattárhoz](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Tó adattár használata az Azure adatok tó Analytics

[Azure adatok tó Analytics](../data-lake-analytics/data-lake-analytics-overview.md) lehetővé teszi a felhőben skála nagy adatokkal végzett munkához. Dinamikusan erőforrások rendelkezések, és végezze el a terabájt vagy a támogatott adatforrások, az egyik tó adattár éppen számos tárolt adatok páros exabájt analytics lehetővé teszi. Adatok tó Analytics kifejezetten készült Azure adatok tó Store - kezeléséről a teljesítményt, átviteli és parallelization meg a legmagasabb szintű nagy adatok munkaterhelésekből van optimalizálva.

Adatok tó Analytics használatát tó áruházzal című cikkben olvashat [Adatok tó Analytics tó adattár használata – első lépések](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Inkább a videók?** Hajtsa végre az alábbi hivatkozásokra kattintva a videókat a HDInsight fürt tó adattár használatára vonatkozó útmutatást.

* [Azure adatok tó Analytics csatlakoztatása Azure tó adattárhoz](https://mix.office.com/watch/qwji0dc9rx9k)
* [Az Access Azure tó adattár adatok tó Analytics keresztül](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Tó adattár használata az Azure Data Factory

[Azure Data Factory](https://azure.microsoft.com/services/data-factory/) segítségével Azure táblázatok, Azure SQL-adatbázissal, Azure SQL-DataWarehouse, Azure BLOB-tároló és a helyszíni adatbázisok adatainak ingest. Az Azure ökológiai egy első osztályú citizen készül, Azure Data Factory használható téve a bevitel Azure tó adattár alábbi forrásból származó adatok.

Azure Data Factory használatát tó áruházzal című cikkben olvashat [adatokat, és az adatok gyári használatával tó adattár](../data-factory/data-factory-azure-datalake-connector.md).

**Videók újra!** Lásd: az [Adatok üzletifolyamat-tervező Azure Data Factory az Azure tó adattár használatával](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Azure BLOB-tároló olyan adatokat másol tó adattárhoz

Azure tó adattár parancssori eszköz AdlCopy, amely lehetővé teszi, hogy az Azure Blob-tárolóhoz olyan adatokat másol egy tó adattár fiók biztosít. További tudnivalókért olvassa el a [Azure tároló BLOB adatok tó áruház adatainak másolása](data-lake-store-copy-data-azure-storage-blob.md)című témakört.

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Adatok másolása Azure SQL-adatbázis és tó adattár között

Azure SQL-adatbázis és tó adattár között az adatok importálása és exportálása Apache Sqoop segítségével. További tudnivalókért lásd: a [tó adattár és Azure SQL-adatbázisokkal Sqoop között adatok másolása](data-lake-store-data-transfer-sql-sqoop.md).

**Ez a videó** a [Relációs adatforrások és Azure tó adattár közötti adatok áthelyezése Apache Sqoop használatával](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Használati adatok tó Store-bA Értékáram-elemzés

A kimeneti értékeket egyikeként tó adattár segítségével adattárolásra folyamatosan lejátszott az Azure Értékáram-elemzés használata. További tudnivalókért olvassa el a [Azure tároló Blob tó tárolóba adatfolyam adatainak Azure Értékáram-elemzés használata](data-lake-store-stream-analytics.md)című témakört.

## <a name="use-data-lake-store-with-power-bi"></a>Tó adattár használata a Power BI

A Power BI segítségével adatok importálása elemzése és az adatok ábrázolása tó adattár fiókból. További tudnivalókért olvassa el a [tó adattár adatainak elemzés használata a Power BI](data-lake-store-power-bi.md)című témakört.

## <a name="use-data-lake-store-with-data-catalog"></a>Tó adattár használata adatkatalógusban

Adatok tó áruházból regisztrálhatja be az Azure adatkatalógus a az adatok tételére a szervezeten belül. További információt a [tó adattár Azure adatkatalógusában adatainak regisztrálni](data-lake-store-with-data-catalog.md)témakörben talál.


## <a name="see-also"></a>Lásd még:

- [Azure tó adattár áttekintése](data-lake-store-overview.md)
- [Első lépések a portálon tó adattárhoz](data-lake-store-get-started-portal.md)
- [Első lépések a PowerShell használatá tó adattárhoz](data-lake-store-get-started-powershell.md)  
