<properties 
    pageTitle="Több elem shard lekérdezése |} Microsoft Azure" 
    description="Lekérdezések futtatása a rugalmas adatbázis ügyfél-tár használata shards keresztül." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Több elem shard lekérdezése

## <a name="overview"></a>– Áttekintés

A [rugalmas Adatbáziseszközök](sql-database-elastic-scale-introduction.md)sharded adatbázis-megoldások is létrehozhat. **Több elem shard lekérdezése** feladatokat, például adatok gyűjtése és jelentéskészítés igénylő több shards keresztül egy lekérdezést, amely nyújtása futó szolgál. (A ellentétben [útválasztási adatok függő](sql-database-elastic-scale-data-dependent-routing.md), amely munkáját egy egyetlen shard hajt végre.) 

## <a name="overview"></a>– Áttekintés

1. Ismerkedés a egy [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) vagy [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) a [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), a [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)vagy a [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) módszert. Lásd: [**a ShardMapManager megépítése**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) és [**RangeShardMap vagy ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Hozzon létre egy **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** objektumot.
2. Hozzon létre egy **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Kétmintás T-SQL-parancsok a **[CommandText tulajdonság](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** beállítása
3. Hívja fel a **[ExecuteReader módszer](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**a parancs végrehajtása.
4. Az eredmények megtekintése a **[MultiShardDataReader osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**használatával. 

## <a name="example"></a>Példa

A következő kódot a több elem shard lekérdezés használatával egy adott **ShardMap** *myShardMap*nevű használatát mutatja be. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
A fő különbség több shard kapcsolatok szerkezetének. **SqlConnection** működik a egy adatbázist, ahol a **MultiShardConnection** annak a bemeneti adataiként vesz igénybe ***shards gyűjteménye*** . A térkép shard shards gyűjteménye adataival. A lekérdezés majd végrehajtása az **UNION ALL** függvény szemantikáját segítségével egyetlen általános eredményt gyűjtenie shards gyűjteménye. Tetszés szerint a kimenet parancs **ExecutionOptions** tulajdonságával bővíthető a shard, ahol a sor származik nevét. 

Megjegyzés: a **myShardMap.GetShards()**a hívást. Ez a módszer összes shards a shard térkép és az egyszerűvé lekérdezés futtatása a megfelelő adatbázisokra keresztül. Shards gyűjteménye az lehet, hogy több elem shard lekérdezés Iskolás további LINQ lekérdezés végrehajtásával fölé a webhelycsoport – által eredményül adott **myShardMap.GetShards()**a hívást. A részleges eredmények házirend együtt a több elem shard lekérdezése az aktuális videofunkcióinak van kialakítva felfelé shards több száz tízesre jól működik.

A több elem shard lekérdezése korlátozása jelenleg shards és a rendszer megkérdezi, hogy shardlets érvényességi hiánya. Útválasztási adatok függő ellenőrzi, hogy egy adott shard a shard térkép lekérdezése idején, miközben több shard lekérdezések ne hajtsa végre ezt a jelölőnégyzetet. Több elem shard lekérdezések forgalmi adatbázisokhoz shard leképezés eltávolított vezethet.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Több elem shard lekérdezések és felosztása és egyesítése műveletek

Több elem shard lekérdezések ne ellenőrizze e lekérdezett-adatbázisban shardlets vesz részt a folyamatban lévő felosztása és egyesítése műveleteket. (Lásd: [Méretezés a rugalmas adatbázis-kezelő eszközével felosztása és egyesítése](sql-database-elastic-scale-overview-split-and-merge.md).) Ez vezethet következetlenségekre hol az azonos shardlet sorainak megjelenítése ugyanabban a több elem shard lekérdezésben több adatbázishoz. Tartsa szem előtt az alábbi korlátozások vonatkoznak, és vegye tekintetbe kiürítés folyamatban lévő felosztása és egyesítése műveletek és a változások a shard térkép több shard lekérdezések hajt végre.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Lásd még:
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** osztályok és módszerek.


A [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md)használatához shards kezelése. Lehetővé teszi, hogy a lekérdezés egyetlen lekérdezés és eredmény több shards [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) nevű névteret tartalmaz. Egy lekérdezésekor hardverabsztrakciós shards gyűjteménye keresztül biztosít. Alternatív végrehajtás házirendek, különösen részleges eredmények kell foglalkozni az sikertelen, ha sok shards fölé lekérdezése is tartalmaz.  

 