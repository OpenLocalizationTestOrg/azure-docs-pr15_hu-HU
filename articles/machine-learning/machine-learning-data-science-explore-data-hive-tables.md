<properties
    pageTitle="Struktúra lekérdezések struktúra táblában található adatok feltárása |} Microsoft Azure"
    description="Ismerkedjen meg a struktúra lekérdezésekkel struktúra táblában található adatok."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Struktúra lekérdezések struktúra táblában található adatok feltárása

A dokumentum egy HDInsight Hadoop fürt struktúra táblában található adatok feltárása használt minta struktúra parancsfájlok tartalmaz.

A következő **menü** témakörökre mutató hivatkozásokat, amelyek az adatok vizsgálata különböző tárterület-környezetből eszközök segítségével.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy:

* Az Azure tároló fiókot hozta létre. Ha utasításokat van szüksége, olvassa el az [Azure tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account)
* A HDInsight szolgáltatáshoz a testre szabott Hadoop fürtre kiépítve. Ha a utasításokat van szüksége, olvassa el a [Testreszabása Azure hdinsight szolgáltatáshoz Hadoop fürt speciális ábrázolja](machine-learning-data-science-customize-hadoop-cluster.md).
* Az adatok struktúra táblák Azure hdinsight szolgáltatáshoz Hadoop fürt feltöltötte. Ha nem rendelkezik, kövesse a [struktúra táblázatok létrehozása és betöltés adatokat](machine-learning-data-science-move-hive-tables.md) először struktúra táblák tölthet fel adatokat.
* Engedélyezve van a fürt távoli eléréséhez. Ha a utasításokat van szüksége, olvassa el a [hozzáférést a címsor csomópontot, Hadoop fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Ha struktúra lekérdezések nyújt útmutatást, megtudhatja, [hogy miként struktúra lekérdezések elküldése](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Példa struktúra lekérdezés parancsfájl-adatok feltárása

1. Partition megfigyelések megszámlálása `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Napi észrevételek megszámlálása `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Ismerkedés a kockák oszlop szintek  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Kapcsolatfelvétel a szintek számát két kockák oszlopok kombinációja `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. A numerikus oszlopok eloszlás beszerzése  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Bontsa ki a rekordok két tábla csatlakozását.

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>További lekérdezési parancsfájlok taxi utazás adatok felhasználási területei

Példák a [Következőt: Taxi utazás adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) esetek jellemző lekérdezések [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)adattárban is rendelkezésre állnak. Ezek a lekérdezések már rendelkezik megadott adatszerkezet és futtatásához benyújtandó készen állnak.
