<properties
   pageTitle="Méretezési meglévő adatbázis áttelepítése |} Microsoft Azure"
   description="Rugalmas Adatbáziseszközök: hozzon létre egy shard térkép manager használandó sharded adatbázisok konvertálása"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Méretezési meglévő adatbázis áttelepítése

A meglévő sharded méretezett meg adatbázisok Azure SQL-adatbázis Adatbáziseszközök (például a [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md)) használatával egyszerűen kezelése. Először egy meglévő beállítása az adatbázisok [shard megfeleltetése manager](sql-database-elastic-scale-shard-map-management.md)használni kell konvertálnia. 

## <a name="overview"></a>– Áttekintés
Meglévő adatbázis sharded áttelepítendő: 

1. Felkészülés a [shard térkép manager-adatbázis](sql-database-elastic-scale-shard-map-management.md).
2. A shard térkép létrehozása
3. Készítse elő az egyes shards.  
2. Hozzárendelések adja hozzá a shard térképen.

Ezeket a módszereket kell végrehajtani a [.NET-keretrendszer ügyfél tár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), vagy [SQL Azure -](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)DB rugalmas adatbázis eszközök parancsfájlok megtalálható PowerShell-parancsfájlokat. Az alábbi példákban PowerShell-parancsfájlokat.

A ShardMapManager kapcsolatos további tudnivalókért olvassa el a [Shard megfeleltetése kezelése](sql-database-elastic-scale-shard-map-management.md)című témakört. A rugalmas Adatbáziseszközök áttekintést [Rugalmas adatbázis szolgáltatások áttekintése](sql-database-elastic-scale-introduction.md)című témakörben találhat.

## <a name="prepare-the-shard-map-manager-database"></a>Felkészülés a shard térkép manager-adatbázis

A shard térkép felügyelője méretezett kivételének adatbázisok kezelése az adatokat tartalmazó speciális adatbázis. Meglévő adatbázis használata, vagy hozzon létre új adatbázist. Figyelje meg, hogy eljáró shard térkép manager adatbázis nem kell egy shard, ugyanabból az adatbázisból. Tartsa szem előtt, hogy a PowerShell-parancsprogramot, nem az adatbázis hozza létre. 

## <a name="step-1-create-a-shard-map-manager"></a>Lépés: 1: hozzon létre egy shard térkép manager

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>A térkép shard manager beolvasása

A létrehozás után a shard térkép Managert ezzel a parancsmaggal meghallgathatja. Ebben a lépésben minden alkalommal, akkor használja a ShardMapManager objektum van szükség.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Lépés: 2: a shard térkép létrehozása

Milyen típusú shard térkép létrehozása kijelölése A választási lehetőségek adatbázis felépítésétől függ: 

1. Egyetlen bérlői adatbázisonként (kifejezések – lásd: a [Szószedet](sql-database-elastic-scale-glossary.md).) 
2. Több bérlők adatbázisonként (két típusa):
    3. Lista hozzárendelése
    4. Tartomány hozzárendelése
 

Egy egyetlen-bérlői modell **lista megfeleltetés** shard térkép létrehozása. Az egyetlen-bérlői modell bérlőnként egy adatbázis rendelt azonosítószámot. Ez a szoftver fejlesztők számára egy hatékony modellt, szerint egyszerűbbé teszi az adatkezelési.

![Lista hozzárendelése][1]

A több elem bérlői modell rendel egy adatbázis több bérlők (és oszthat bérlők csoportjai több adatbázisok között). Ez a modell használatával minden bérlőjének szükségletei vannak kis adatok várt. Ebben a modellben azt hozzárendelése egy cellatartomány bérlők **tartomány hozzárendelése**egy adatbázist. 
 

![Tartomány hozzárendelése][2]

Vagy a *lista megfeleltetés* használata több bérlők hozzárendelése egy adatbázis több bérlői adatbázismodell is alkalmazhat. Ha például D1 bérlői azonosító 1 és 5 adatainak tárolására szolgál, és DB2 tárolja az adatokat a bérlői 7 és a bérlői 10. 

![A egyetlen DB Muliple bérlők][3] 

**A választási lehetőségek attól függően, válasszon az alábbi lehetőségek egyikét:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>1 a beállítás: a lista hozzárendelés shard térkép létrehozása
ShardMapManager objektumot használva shard térkép létrehozása. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>2 a beállítás: a tartomány hozzárendelés shard térkép létrehozása

Ne feledje, hogy való csatlakozást, a hozzárendelés mintázatot, a bérlői azonosító értékeket kell lennie a folyamatos tartományokat, elfogadható tartományokban lévő távolság biztosítani egyszerűen lejátsszon a tartományt az adatbázisok létrehozása.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>3 lehetőség: Egy adatbázis lista hozzárendelések
A lista térkép létrehozása is állíthat be ezt a minta igényel, lépés: 2, 1 beállítás látható módon.

## <a name="step-3-prepare-individual-shards"></a>3 lépés: Az egyes shards előkészítése

Adja hozzá a shard térkép kezelő minden shard (adatbázis). Ez az egyes adatbázisok előkészítése hozzárendelési adatokat tárolja. Minden egyes shard hajtsa végre ezt a módszert.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Lépés: 4: A hozzárendelések hozzáadása

A hozzárendelések felvett létrehozott shard térkép típusú függ. Ha létrehozott egy lista térképen, hozzáadhat lista hozzárendelések. Ha létrehozott egy tartomány térképen, tartomány megfeleltetések hozzáadása.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>1 beállítást: a lista hozzárendelés térképadatok

A térképadatok minden bérlő lista megfeleltetés hozzáadásával.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>2 lehetőség: a tartomány hozzárendelés térképadatok

Összes bérlői azonosító tartomány – adatbázis társítások tartomány megfeleltetések hozzáadása:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>3 lépés: 4 beállítás: a egy adatbázis több bérlők a térképadatok

Egyes bérlői webhelyen, a Hozzáadás ListMapping futtatása (1, a beállítás a felett). 


## <a name="checking-the-mappings"></a>A hozzárendelések ellenőrzése

A meglévő shards és a velük társított megfeleltetések információt a következő parancsok használatával lekérdezhető:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Összefoglalás

A telepítés befejezése után megkezdheti a rugalmas adatbázis-ügyfél tárral. [Függő útválasztási adatok](sql-database-elastic-scale-data-dependent-routing.md) és a [több elem shard lekérdezés](sql-database-elastic-scale-multishard-querying.md)is használhatja.

## <a name="next-steps"></a>Következő lépések


PowerShell-parancsfájlokat lekérése az [Azure SQL-adatbázis-elasztikus adatbázis eszközök sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Az eszközök is vannak GitHub: [Azure/elasztikus-db-eszközök](https://github.com/Azure/elastic-db-tools).

A felosztása és egyesítése eszközzel áthelyezése egyetlen bérlői modellhez adatok vagy több elem bérlői modellből. Lásd: a [felosztott egyesítés eszközt](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>További források

Közös adatok architektúra mintázatok több bérlői szoftverek,-a-szolgáltatási (szoftver) adatbázis-alkalmazásokat a további tudnivalókért lásd [Tervezés mintázatok az Azure SQL-adatbázis több bérlői szoftver alkalmazásokhoz](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Kérdések és frissítéseiről

A kérdésekre kérjük, keresse meg a szolgáltatás frissítéseiről és az [SQL-adatbázis fórumát](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) , vegyen fel őket a [SQL-adatbázis Visszajelzési fórum](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
