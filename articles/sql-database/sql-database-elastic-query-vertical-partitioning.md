<properties
    pageTitle="Lekérdezés különböző séma használata felhő adatbázisok között |} Microsoft Azure"
    description="függőleges partíciót felett több adatbázis-lekérdezések beállítása"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Lekérdezése a felhőben adatbázisok különböző sémával (előzetes verzió)

![Táblázatok eltérő adatbázisokban kiterjedő lekérdezés][1]

Adatbázisok függőlegesen másik táblák különböző csoportjaihoz különböző adatbázisok használja. Ez azt jelenti, hogy a séma eltér a különböző adatbázisok. Például minden olyan tábla készlet találhatók egy adatbázis második adatbázis minden könyvelési kapcsolódó tábla várakozó. 

## <a name="prerequisites"></a>Előfeltételek

* A felhasználó módosíthatja bármely külső ADATFORRÁS engedéllyel kell rendelkeznie. Ezzel az engedéllyel megtalálható az adatbázis módosítása engedéllyel.
* ALTER bármely külső ADATFORRÁS engedélyek szükségesek, ha nézni szeretné a mögöttes adatforrásban.

## <a name="overview"></a>– Áttekintés

**Megjegyzés**: eltérően a vízszintes szétválasztás ezek DDL kimutatások nem függő egy adatok réteg keresztül a rugalmas adatbázis ügyfél tár shard leképezést megadásával.

1. [DIAMINTA KULCS LÉTREHOZÁSA](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MUNKAFÜZETSZINTŰ ADATBÁZIS LÉTREHOZÁSA HITELESÍTŐ ADATOK](https://msdn.microsoft.com/library/mt270260.aspx)
3. [A KÜLSŐ ADATFORRÁS LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [KÜLSŐ TÁBLA LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Adatbázis hatóköre fő billentyűt, és a hitelesítő adatok létrehozása 

A hitelesítő adatok a távoli adatbázisokhoz való csatlakozáshoz a rugalmas lekérdezés használja.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Megjegyzés:**    Győződjön meg arról, hogy a *<username>* nem tartalmaz *"@servername"* utótag. 

## <a name="create-external-data-sources"></a>A külső adatforrás létrehozása

Szintaxis:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Fontos**   A TYPE paraméter **RDBMS**kell beállítani. 

### <a name="example"></a>Példa 

Az alábbi példa bemutatja a kimutatás létrehozása külső adatforrások használata. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
Az aktuális külső adatforrások listája beolvasásához: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Külső tábla 

Szintaxis:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Példa  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

A következő példa bemutatja, hogyan lehet a külső táblalistából lekérése az aktuális adatbázisban: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Megjegyzések:

Rugalmas lekérdezés bővíti a meglévő külső tábla szintaxis meghatározása a külső adatforrás használata választógombot RDBMS típusú külső táblákhoz. Egy külső tábla definícióját függőleges szétválasztás a következőkre terjed ki: 

* **Séma**: A külső tábla DDL határozza meg a lekérdezéseket, amelyekkel séma. Külső tábla definíciójának megadott sémában kell egyeznie a tényleges adatok tárolásának helye a távoli adatbázis sémája. 

* **Távoli adatbázis hivatkozás**: A külső tábla DDL utal, amelyet egy külső adatforrásból. A külső adatforráshoz megadja a logikai kiszolgáló neve és az adatbázis nevét, az a tábla tényleges adatokat tároló távoli adatbázis. 

Külső adatforrás használata, az előző szakaszban ismertetett módon, a külső táblázatok létrehozása szintaxisa a következő: 

A DATA_SOURCE záradék a külső adatforrásban (azaz a távoli adatbázis esetén függőleges szétválasztás), amellyel a külső tábla határozza meg.  

A SCHEMA_NAME és objektumnév záradékok biztosítson feleltesse meg a külső tábla megadása egy másik séma meg a távoli adatbázis táblájához vagy egy táblából egy másik nevet az rendre lehetőséget. Ez akkor hasznos, ha be szeretné állítani a külső tábla katalógus nézet vagy DMV a távoli adatbázis – vagy bármely más olyan esetben, ha a távoli táblanév már foglalt helyileg.  

Az alábbi DDL utasítás egy meglévő külső tábla definícióját esik, a helyi katalógusról. Ez a távoli adatbázis nincs hatással. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Külső tábla létrehozása/LEGÖRDÜLŐ engedélyeinek**: módosíthatja bármely külső ADATFORRÁS engedélyek szükségesek a külső tábla DDL, amelyre hivatkozni kell a mögöttes adatforrásban is szükség van.  

## <a name="security-considerations"></a>Biztonsági megfontolások
A külső tábla hozzáféréssel rendelkező felhasználó automatikusan hozzáférni az alapul szolgáló távoli táblák csoportban a hitelesítő adatok abban a külső adatforrás-definíciót. Gondosan kezelheti a hozzáférést a külső tábla keresztül a hitelesítő adatok a külső adatforrás nemkívánatos kiterjesztését elkerülése érdekében. Normál SQL-engedélyek BIZTOSÍTHAT és vonhat vissza egy külső táblában való hozzáférést, ugyanúgy, mintha egy normál táblázat is használható.  


## <a name="example-querying-vertically-partitioned-databases"></a>Példa: függőlegesen lekérdezése particionálva adatbázisok 

A következő lekérdezés a két helyi tábla rendelések és a sorok és a távoli tábla között a hármas illesztés ügyfeleknek hajt végre. Ez a példa a hivatkozás adatokat rugalmas lekérdezés használatieset: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Normál SQL Server csatlakozási_karakterlánc adatbázisokhoz való csatlakozáshoz a BI és az adatok integrációs eszközök rugalmas lekérdezés engedélyezett és a külső definiált táblák tartalmazó SQL-adatbázis-kiszolgálón is használhatja. Győződjön meg arról, hogy az eszköz adatforrásként az SQL Server támogatott. Ezután hivatkozni a rugalmas lekérdezés és a külső táblákat, mint bármely más SQL Server-adatbázishoz, amely a eszközzel kíván csatlakozni. 

## <a name="best-practices"></a>Ajánlott eljárások 
 
* Győződjön meg arról, hogy a lekérdezés rugalmas végpont adatbázis már megkapta az access a távoli adatbázis eredményez, mivel az access az Azure-szolgáltatások SQL-adatbázis tűzfal konfigurációban. Győződjön meg arról is, hogy a külső adatforrás-definíciót megadott hitelesítő adatok sikeres jelentkezhet be a távoli adatbázis, és rendelkezik engedélyekkel a távoli tábla eléréséhez.  

* Rugalmas lekérdezés működik a legjobban lekérdezések hol a kiszámítása a legtöbb el tud végezni a távoli adatbázist. A szokásos kap, amely a távoli adatbázis vagy a távoli adatbázishoz teljesen elvégezhető illesztések kiértékelhető szelektív szűrő predikátumok a legjobb lekérdezési teljesítmény. Más lekérdezés mintázatok nagy mennyiségű adat betöltése a távoli adatbázis szükséges, és gyengén hajthat végre. 


## <a name="next-steps"></a>Következő lépések

A lekérdezés vízszintesen particionált adatbázisok (más néven sharded adatbázisok), olvassa el a [lekérdezések (vízszintesen particionálva) sharded felhő adatbázisok között](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
