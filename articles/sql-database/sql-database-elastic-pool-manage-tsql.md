<properties 
    pageTitle="Hozzon létre vagy egy Azure SQL-adatbázis áthelyezése egy rugalmas készlet segítségével az SQL-T |} Microsoft Azure" 
    description="Az SQL-T használja-Azure SQL-adatbázis létrehozása az rugalmas készletben. Vagy helyezze át a datbase készletek és kijelentkezés az SQL-T használja." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Figyelheti és kezelheti a Transact-SQL-rugalmas adatbázis készlet  

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-pool-manage-portal.md)
- [A PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [AZ SQL-T](sql-database-elastic-pool-manage-tsql.md)

Az [Adatbázis létrehozása (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/dn268335.aspx) és parancsokkal [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) hozhat létre, és helyezze át az adatbázisok be- és kijelentkezés a rugalmas készletek. A rugalmas készlet léteznie kell, mielőtt az alábbi parancsokat használhatja. Ezek a parancsok csak adatbázisok vonatkoznak. Új készletek létrehozása és a készlet tulajdonságai (például a min és max eDTUs) beállítás nem módosítható az SQL-T a parancsok.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Új adatbázis létrehozása az egyik rugalmas készletben
Az adatbázis létrehozása parancs használata a SERVICE_OBJECTIVE lehetőséget.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Az rugalmas készletben adatbázisokra öröklik a szolgáltatási réteg rugalmas készlet (Basic, szabványos, prémium verzióban). 


## <a name="move-a-database-between-elastic-pools"></a>Adatbázis-készletek rugalmas közötti áthelyezése
Az adatbázis módosítása parancs használata a módosítása, és adja meg a szolgáltatás\_ELASZTIKUS OBJEKTÍV lehetőséget\_készlet; Állítsa be a nevet a cél készlet nevére.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Adatbázis áthelyezése egy rugalmas készlet 
Az adatbázis módosítása parancs használata a módosítása, és adja meg a szolgáltatás\_ELASTIC_POOL; OBJEKTÍV lehetőséget Állítsa be a nevet a cél készlet nevére.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Adatbázis áthelyezése egy rugalmas készlet kívül
Az adatbázis módosítása parancsot, és jelölje be a SERVICE_OBJECTIVE, a teljesítmény különböző (S0, S1 stb.).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Lista-adatbázisok az rugalmas készletben
Használja a [sys.database\_szolgáltatás \_célok nézet](https://msdn.microsoft.com/library/mt712619) egy rugalmas készlet adatbázisokra listáját. Jelentkezzen be a fő adatbázist, hogy a lekérdezés a nézetet.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Erőforrás-használati adatok beszerzése a készletben

Használja a [sys.elastic\_készlet \_erőforrás \_stat nézet](https://msdn.microsoft.com/library/mt280062.aspx) szeretné az erőforrás használati statisztika egy rugalmas készlet logikai kiszolgálón vizsgálja meg. Jelentkezzen be a fő adatbázist, hogy a lekérdezés a nézetet.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Erőforrás-kihasználtság első rugalmas adatbázisok

Használja a [sys.dm\_ db\_ erőforrás\_stat nézet](https://msdn.microsoft.com/library/dn800981.aspx) vagy [sys.resource \_stat nézet](https://msdn.microsoft.com/library/dn269979.aspx) szemügyre veszi a erőforrás használati statisztika az rugalmas készletben adatbázis. Ez a folyamat hasonlít lekérdezése az Erőforrás kihasználtsága bármely egyetlen adatbázis.

## <a name="next-steps"></a>Következő lépések

Miután létrehozott egy rugalmas adatbázis készlet, kezelheti a készletben rugalmas adatbázisok rugalmas feladatok létrehozásával. Rugalmas feladatok megkönnyítik a készletben adatbázisok tetszőleges számú parancsprogramok futó az SQL-T. További információ a [Rugalmas adatbázis feladatok áttekintése](sql-database-elastic-jobs-overview.md)című témakörben találhat. 

Lásd: a [Méretezés ki az Azure SQL-adatbázis](sql-database-elastic-scale-introduction.md): rugalmas adatbázis eszközök segítségével méretezési, az adatok áthelyezése, lekérdezés, vagy hozzon létre a tranzakciók.
