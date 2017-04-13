<properties 
   pageTitle="Adatokat tó adattár és Azure SQL-adatbázisokkal Sqoop között |} Microsoft Azure"
   description="Sqoop segítségével tó adattár között Azure SQL-adatbázis adatainak másolása" 
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

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Tó adattár és Azure SQL-adatbázisokkal Sqoop között adatok másolása

Megtudhatja, hogy miként Azure SQL-adatbázis és tó adattár között az adatok importálása és exportálása Apache Sqoop használatával.
 

## <a name="what-is-sqoop"></a>Mi az Sqoop?

Nagy adatok alkalmazások félig strukturált és strukturálatlan adatokat, például a naplókat és a fájlok feldolgozásra természetes választási lehetőséget. Azonban is lehetnek relációs adatbázisok tárolt strukturált adatok feldolgozása szükség.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) eszköz a adatok átvitele relációs adatbázisok és a nagy adatok összegyűjti, például tó adattár között. Adatok importálása az Azure SQL-adatbázis például relációs adatbázis-kezelő rendszerekhez (RDBMS) tó tárolóba felhasználhatja azt. Ezután átalakítása és elemezheti az adatokat, nagy adatok munkaterhelésekből használ, és majd exportálja az adatokat egy RDBMS állapotfrissítéseket. Ebben az oktatóanyagban használatával Azure SQL-adatbázissal, a relációs adatbázisok importálás/exportálás az.
 

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- **Az Azure előfizetés engedélyezése** tó adattár nyilvános előzetes. [Útmutatást](data-lake-store-get-started-portal.md#signup)találhat. 
- **Azure hdinsight szolgáltatáshoz fürt** az Access-szel tó adattár-fiókjába. Lásd: [létrehozása egy HDInsight fürthöz tó áruházzal](data-lake-store-hdinsight-hadoop-use-portal.md). Ez a cikk feltételezi, hogy van-e egy HDInsight Linux fürtre tó adattár hozzáféréssel rendelkező.
- **Azure SQL-adatbázis**. Hogyan hozhat létre egyet, tanulmányozza [az Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Megismerheti egy videók tegye?

[Ebből a videóból](https://mix.office.com/watch/1butcdjxmu114) adatok Azure BLOB-tároló és tó adattár közötti másolásáról DistCp használatával.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Minta táblák létrehozása az Azure SQL-adatbázisban

1. Kezdje hozzon létre két minta táblázat az Azure SQL-adatbázisban. [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy Visual Studio segítségével az Azure SQL-adatbázis csatlakoztatása, és futtassa a következő lekérdezések.

    **Tábla1 létrehozása**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Hozzon létre tábla2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. **Tábla1**mintaadatokat is tartalmazó hozzáadása. **Tábla2** hagyja üresen. Azt fogja adatainak importálása **tábla1** tó tárolóba. Ezután azt exportálja az adatokat tó áruházból **tábla2**be. Futtassa a következő kódtöredékének.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Az Access-szel egy HDInsight fürthöz Sqoop segítségével tó adattárhoz

Egy HDInsight fürthöz már a Sqoop csomagok érhető el. A HDInsight fürt adatok tó tároló használatára, egy további tárterületet állította be, ha segítségével Sqoop (konfigurációs módosítások) nélkül az importálás/exportálás adatok között egy relációs adatbázisban (az ebben a példában a Azure SQL-adatbázis) és tó adattár fiókkal. 

1. Ebben az oktatóanyagban feltételezzük Linux fürt létrehozott, hogy csatlakozzon a fürthöz SSH kell használnia. Olvassa el a [Csatlakozás a Linux-alapú HDInsight fürtre](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Győződjön meg arról, hogy érheti el a tó adattár fiókot a fürt. A következő parancsot a SSH parancssorból:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    A fájlok és mappák tó adattár fiók listájának biztosítania kell.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Adatok importálása az Azure SQL-adatbázis tó tárba

3. Nyissa meg a könyvtár, ahol Sqoop csomagok érhetők el. A szokásos, ez lesz a `/usr/hdp/<version>/sqoop/bin`. 

4. Az adatok importálása **tábla1** tó adattár-fiókba. A következő szintaxissal:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Figyelje meg, hogy **az sql-adatbázis-kiszolgáló neve** helyőrző az Azure SQL-adatbázis futtató kiszolgáló nevét. **SQL-adatbázis-neve** helyőrző az aktuális adatbázis nevét.

    Ha például

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Győződjön meg arról, hogy az adatok átvitele volt-e a tó adattár fiókot. A következő parancsot:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Meg kell jelennie a következő eredményt.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Minden egyes **rész-m -** * fájl felel meg egy-egy sor a forrástáblához * *tábla1**. A kijelző tartalmának megtekintése-m -* fájlok ellenőrzéséhez.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Adatok exportálása tó áruházból Azure SQL-adatbázis

6. Az adatok exportálása tó adattár fiókból az üres táblát, **tábla2**az Azure SQL-adatbázisban. A következő szintaxissal.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Ha például

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Győződjön meg arról, hogy az adatok feltöltése az SQL-adatbázis táblázatra. [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy Visual Studio segítségével az Azure SQL-adatbázis csatlakoztatása, és futtassa az alábbi lekérdezés.

        
        SELECT * FROM TABLE2

    Ez a következő eredményt kell tartalmaznia.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Lásd még:

- [Adatok másolása Azure BLOB-tároló tó adattárhoz](data-lake-store-copy-data-azure-storage-blob.md)
- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
