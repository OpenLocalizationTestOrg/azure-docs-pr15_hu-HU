<properties
    pageTitle="Jelentéskészítés méretezett kivételének felhő adatbázisok között |} Microsoft Azure"
    description="vízszintes partíciót fölé rugalmas lekérdezések beállítása"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Jelentéskészítés keresztül méretezett kivételének felhő adatbázisok (előzetes verzió)

![Shards kiterjedő lekérdezés][1]

Sharded adatbázisok sor elosztása méretezett el adatok réteg. A séma megegyezik az összes résztvevő adatbázisokon vízszintes szétválasztás néven is ismert. Rugalmas lekérdezés használata, sharded adatbázisban adatbázisokra átterjedő jelentéseket hozhat létre.

A rövid útmutató az első a [jelentéskészítő méretezett kivételének felhő adatbázisok közötti](sql-database-elastic-query-getting-started.md)című témakör tartalmaz.

Nem sharded adatbázisok esetén lásd: a [lekérdezés felhő adatbázisok különböző sémával keresztül](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Előfeltételek

* A rugalmas adatbázis ügyfél-tár használata shard térkép létrehozása. Lásd: a [térkép management Shard](sql-database-elastic-scale-shard-map-management.md). Vagy [rugalmas Adatbáziseszközök – első lépések](sql-database-elastic-scale-get-started.md)a minta alkalmazást használja.
* Azt is megteheti lásd: a [létező adatbázisokat áttelepítése méretezett kivételének adatbázisokhoz](sql-database-elastic-convert-to-use-elastic-tools.md).
* A felhasználó módosíthatja bármely külső ADATFORRÁS engedéllyel kell rendelkeznie. Ezzel az engedéllyel megtalálható az adatbázis módosítása engedéllyel.
* ALTER bármely külső ADATFORRÁS engedélyek szükségesek, ha nézni szeretné a mögöttes adatforrásban.

## <a name="overview"></a>– Áttekintés

Ezeket az utasításokat a sharded adatok réteg metaadatok ábrázolása hozza létre a rugalmas lekérdezés adatbázisban. 


1. [DIAMINTA KULCS LÉTREHOZÁSA](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MUNKAFÜZETSZINTŰ ADATBÁZIS LÉTREHOZÁSA HITELESÍTŐ ADATOK](https://msdn.microsoft.com/library/mt270260.aspx)
3. [A KÜLSŐ ADATFORRÁS LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [KÜLSŐ TÁBLA LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>Adatbázis létrehozása 1.1-es hatóköre fő billentyű és a hitelesítő adatok 

A hitelesítő adatok a távoli adatbázisokhoz való csatlakozáshoz a rugalmas lekérdezés használja.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Győződjön meg arról, hogy a *"\<felhasználónév\>"* nem tartalmaz *"@servername"* utótag. 

## <a name="12-create-external-data-sources"></a>Az 1.2 létrehozása külső adatforrások

Szintaxis:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Példa 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Az aktuális külső adatforrások listája beolvasása: 

    select * from sys.external_data_sources; 

A külső adatforrás a shard térképre hivatkozik. Egy rugalmas lekérdezést használja fel a külső adatforrásban és a mögöttes shard térkép számba veheti a webhely az adatbázisokra, amelyek az adatok réteg részt. A hitelesítő adatait használja a shard térkép olvasható, valamint rugalmas lekérdezés feldolgozása közben a shards lévő adatok eléréséhez. 

## <a name="13-create-external-tables"></a>Az 1.3 külső táblázatok létrehozása 
 
Szintaxis:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Példa**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

A külső táblalistából lekérése az aktuális adatbázisban: 

    SELECT * from sys.external_tables; 

A külső táblák eltávolítása:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Megjegyzések:

Az adatok\_forrás záradék a külső adatforrásban (shard térkép), amellyel a külső tábla határozza meg.  

A SÉMA\_nevét és az objektum\_neve záradékok egy táblából egy másik séma feleltesse meg a külső tábla definícióját. Adja meg, ha a távoli objektum sémájának "dbo"-nek tekinti, és a nevére a külső tábla neve alatt álló definiált azonos-nek tekinti. Ez akkor hasznos, ha a távoli táblázatot már nem történik az adatbázisban, amelyre a külső táblázat létrehozásához. Például szeretné meghatározni, egy egyesített katalógus nézetek áttekintést kaphat a külső tábla vagy DMVs méretezett meg az adatokon az első csoportba tartozó. Katalógus nézetek DMVs már létezik és helyi meghajtóra, mert a külső tábla megadása a nevük nem használható. Ehelyett használni egy másik nevet, és a katalógus nézetben vagy a DMV nevét a sémában\_nevét és/vagy objektum\_záradékok nevét. (Lásd az alábbi példában.) 

A TERJESZTÉSI záradék Itt adhatja meg az adatok eloszlás az alábbi táblázat használható. A lekérdezés processzor használja a lehető leghatékonyabb lekérdezési tervek össze a TERJESZTÉSI záradékban megadott adatokkal.  

1. **SHARDED** azt jelenti, hogy az adatok vízszintesen particionált át az adatbázisok. Az adatok eloszlás particionáló kulcsa a **< sharding_column_name >** paramétert.
2. **REPLIKÁLT** azt jelenti, hogy a táblázat azonos másolatait adatbázisonként rajta. A feladata annak érdekében, hogy a replikák azonos-e végig az adatbázisokat.
3. **Kerekítés\_ROBIN** jelenti, hogy a táblázat vízszintes particionálva van az alkalmazás-függő terjesztési módszerrel. 

**Adatok az első csoportba tartozó hivatkozás**: A külső tábla DDL utal, amelyet egy külső adatforrásból. A külső adatforrás megadja egy shard térképen, amely magában foglalja a külső tábla keresse meg a adatbázisokra az adatok réteg szükséges információkat. 


### <a name="security-considerations"></a>Biztonsági megfontolások 

A külső tábla hozzáféréssel rendelkező felhasználó automatikusan hozzáférni az alapul szolgáló távoli táblák csoportban a hitelesítő adatok abban a külső adatforrás-definíciót. Kerülje a hitelesítő adatok a külső adatforrás keresztül nemkívánatos kiterjesztését. Engedélyek biztosítása vagy használata REVOKE külső tábla ugyanúgy, mintha egy normál táblázat.  

Miután meghatározta a külső adatforrásban és a külső táblázatok, most már használhatja teljes T-SQL nyelvben a külső táblák fölé.

## <a name="example-querying-horizontal-partitioned-databases"></a>Példa: a vízszintes particionált adatbázisok lekérdezése 

Az alábbi lekérdezés raktárak, a rendelések és a sorok közötti hármas illesztés hajt végre, és több összegzések és szelektív szűrő használja. Azt feltételezi, hogy (1) vízszintes particionáló (sharding) és (2), hogy raktárak, a rendelések és a rendelési sorok sharded a raktári azonosító oszlop szerint, és, hogy a rugalmas lekérdezés közös keresse meg a shards meg az illesztés, és a lekérdezést a shards párhuzamosan a nagy része feldolgozása. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Tárolt eljárás végrehajtásához távoli az SQL-T: sp\_execute_remote

Rugalmas lekérdezés is található, a tárolt eljárás, amely a shards közvetlen hozzáférést biztosít. A tárolt eljárás nevezik [sp\_végrehajtása \_távoli](https://msdn.microsoft.com/library/mt703714) távoli tárolt eljárásokat vagy T-SQL-kódot végrehajtani a távoli adatbázis a használható. A következő paraméterek vesz igénybe: 

* Adatforrás nevét (nvarchar): a külső adatforrás RDBMS-típus nevére. 
* Lekérdezés (nvarchar): minden shard végre kell hajtani a T-SQL-lekérdezést. 
* Paraméter deklaráció (nvarchar) – nem kötelező: az adatok típusát definíciók a paraméterek (például sp_executesql) a lekérdezési paraméter használt karakterlánc. 
* Paraméter értéklista – nem kötelező: paraméterértékeket (például sp_executesql) vesszővel tagolt listáját.

Az sp\_végrehajtása\_távoli használja a külső adatforrás a Meghívási paraméterek leírt végrehajtani a megadott T-SQL-utasítást a távoli adatbázist. A hitelesítő adatok a külső adatforrás használatával csatlakozhat a shardmap manager-adatbázishoz, és a távoli adatbázis.  

Példa: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Csatlakozási eszközök  

Kapcsolatfelvétel az alkalmazást, a rendszeres SQL Server csatlakozási_karakterlánc használatával a BI és az adatok integrációs eszközök segítségével a külső tábla definíciók az adatbázis. Győződjön meg arról, hogy az eszköz adatforrásként az SQL Server támogatott. Ezután hivatkozást a rugalmas lekérdezés adatbázis más SQL Server-adatbázis például kapcsolatban az eszközt, és a külső táblázatok használata az eszköz vagy a program helyi táblákat, mintha. 

## <a name="best-practices"></a>Ajánlott eljárások 

* Győződjön meg arról, hogy a lekérdezés rugalmas végpont adatbázis már megkapta az access a shardmap adatbázist, és az összes shards az SQL-adatbázis tűzfalon keresztül.  

* Az érvényesítés vagy érvényesítése az adatok eloszlás határozza meg a külső táblában. Ha a tényleges adatok terjesztési eltér a megadott táblázat definíciójának eloszlás, akkor a lekérdezések eredményezhet nem várt eredmények. 

* Rugalmas lekérdezés jelenleg nem hajtja végre shard eltávolítási amikor felett a sharding kulcsa predikátumok tenne lehetővé egyes shards biztonságosan kizárása feldolgozása.

* Rugalmas lekérdezés működik a legjobban lekérdezések hol a shards nagy része a számítási végezhető. A szokásos a legjobb lekérdezési teljesítmény, amely kiértékelhető szelektív szűrő predikátumok shards vagy illesztések keresztül letölthető a particionáló kulcsok shards az összes partíciót igazított módon elvégezhető. Más lekérdezés minták szükség lehet a nagy mennyiségű adat betöltése a shards a központi csomópontra, és gyengén teljesítményt

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
