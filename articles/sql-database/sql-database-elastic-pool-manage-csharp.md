<properties
    pageTitle="Figyelheti és kezelheti a és C# egy rugalmas adatbázis készlet |} Microsoft Azure"
    description="Az SQL Azure-adatbázis rugalmas adatbázis készlet kezelése a C# adatbázis fejlesztési technikákat használatával."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Figyelése és a C és 23; #x egy rugalmas adatbázis készlet kezelése 

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-pool-manage-portal.md)
- [A PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [AZ SQL-T](sql-database-elastic-pool-manage-tsql.md)


Megtudhatja, hogy miként kezelheti egy [Rugalmas adatbázis készlet](sql-database-elastic-pool.md) a C és 23; #x segítségével. 

>[AZURE.NOTE] Sok új funkcióval SQL-adatbázis használata esetén megadhatja az [erőforrás-kezelő Azure telepítési modell](../azure-resource-manager/resource-group-overview.md), így mindig a legújabb érdemes használni csak támogatott **Azure SQL adatbázis könyvtár a .NET rendszerhez ([dokumentumok](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. A régebbi [Klasszikus telepítési model-alapú tárak](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) , javasoljuk, hogy az újabb erőforrás-kezelő alapú tárak használata támogatott csak visszamenőleges kompatibilitás érdekében.

A jelen cikkben ismertetett lépések végrehajtásához szükséges az alábbiakat:

- Rugalmas készlet (a kezelni kívánt készlet). Készlet létrehozása, olvassa el a [és C# egy rugalmas adatbázis készlet létrehozása](sql-database-elastic-pool-create-csharp.md)című témakört.
- Visual Studio. Visual Studio ingyenes másolatának talál a [Visual Studio letöltési](https://www.visualstudio.com/downloads/download-visual-studio-vs) lapjáról.


## <a name="move-a-database-into-an-elastic-pool"></a>Adatbázis áthelyezése egy rugalmas készlet

Áthelyezheti egy másik adatbázist, vagy a készlet kicsinyítése.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Lista-adatbázisok az rugalmas készletben

Egy készlet adatbázisokra beolvasásához, hívja fel a [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) módszerrel.

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Egy készlet teljesítmény beállításainak módosítása

Kapható meg a készlet tulajdonságainak meglévő. Módosítsa az értékeket, majd hajtsa végre a CreateOrUpdate módszer.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Rugalmas készlet műveletek késleltetése

- A min eDTUs egy adatbázist, vagy a max eDTUs adatbázisonként általában módosítása egészíti ki az 5 perc alatt vagy annál kisebb.
- Idő (eDTUs) készlet méretének módosítása attól függ, hogy a készletben adatbázisokra együttes méretét. Módosítások átlagos 90 perc vagy annál kevesebb 100 GB. Ha például a teljes pedig az összes adatbázisok-e hely 200 GB, majd a várható időtartam módosítása a készlet eDTU egy készlet 3 óra vagy annál kisebb.




## <a name="additional-resources"></a>További források

- [SQL-hibakódok az SQL-adatbázis-ügyfélalkalmazásokban: adatbázis-kapcsolati hiba és egyéb problémák](sql-database-develop-error-messages.md).
- [SQL-adatbázis](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure erőforrás-kezelés API-k](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Rugalmas adatbázis-készlet létrehozása a és C#](sql-database-elastic-pool-create-csharp.md)
- [Ha egy rugalmas adatbázis készlet használni?](sql-database-elastic-pool-guidance.md)
- Lásd: a [Méretezés ki az Azure SQL-adatbázis](sql-database-elastic-scale-introduction.md): rugalmas adatbázis eszközök segítségével méretezési, az adatok áthelyezése, lekérdezés, vagy hozzon létre a tranzakciók.

