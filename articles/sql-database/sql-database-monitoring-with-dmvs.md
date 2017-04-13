<properties
   pageTitle="Azure SQL-adatbázis kezelése dinamikus nézetek használata figyelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként észleli és közös teljesítménybeli problémáinak diagnosztizálása figyelése a Microsoft Azure SQL-adatbázis kezelése dinamikus nézetek használatával."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Figyelés Azure SQL-adatbázis kezelése dinamikus nézetek használata

Microsoft Azure SQL-adatbázis lehetővé teszi, hogy a letiltott vagy hosszabb ideig futó lekérdezések, az erőforrás szűk, gyenge lekérdezés csomagok és így tovább okozhatja teljesítményproblémákat diagnosztizálása dinamikus kezelése nézetek egy részét. Ez a témakör információkat ismertető általános teljesítményproblémákat feltárása dinamikus kezelése nézetek használatával.

SQL-adatbázis részben támogatja a három kategóriába dinamikus kezelése nézetek:

- Dinamikus kezelése adatbázis kapcsolatos nézeteket hozhat létre.
- Dinamikus kezelése kapcsolatos adatvégrehajtás nézeteket hozhat létre.
- Dinamikus kezelése tranzakció kapcsolatos nézeteket hozhat létre.

Részletes információt nézetek dinamikus kezelése című témakörben talál [dinamikus kezelése nézetek és a függvények (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms188754.aspx) az SQL Server könyvek Online.

## <a name="permissions"></a>Engedélyek

SQL-adatbázisban dinamikus kezelése nézet lekérdezése **VIEW adatbázis STATE** engedéllyel kell rendelkeznie. Az **Adatbázis-állapot megtekintése** engedéllyel minden objektumok belül az aktuális adatbázis adatainak adja eredményül.
A **NÉZET adatbázis állam** engedélyt egy adott adatbázis-felhasználó, futtassa a következő lekérdezés:

```GRANT VIEW DATABASE STATE TO database_user; ```

Egy helyszíni SQL Server-példányban dinamikus kezelése nézetek kiszolgáló állam adatai adja eredményül. SQL-adatbázisban hogy csak az aktuális logikai adatbázis vonatkozó információk adja eredményül.

## <a name="calculating-database-size"></a>Az adatbázis mérete kiszámítása

Az alábbi lekérdezés adja eredményül (megabájtban) az adatbázis mérete:

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Az alábbi lekérdezés (megabájtban) egyes objektumok méretét az adatbázis adja eredményül:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Kapcsolatok figyelése

A [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) nézet segítségével beolvashatja egy adott Azure SQL-adatbázis-kiszolgáló és a részletek, a minden kapcsolat létrejött a kapcsolatok az információkat. Ezeken kívül az [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) nézet akkor hasznos, ha az aktív felhasználót kapcsolatok és a belső feladatok kapcsolatos adatok beolvasása.
A következő lekérdezés által visszaadott az aktuális kapcsolathoz információkat:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Végrehajtásakor az **sys.dm_exec_requests** és **sys.dm_exec_sessions nézetek**, ha **NÉZET adatbázis STATE** engedéllyel rendelkezik az adatbázishoz, látja, az összes végrehajtó munkamenetek az adatbázisban. egyéb esetben csak az aktuális munkamenethez látni.

## <a name="monitoring-query-performance"></a>A lekérdezési teljesítmény figyelése

Lassú, vagy hosszú futó lekérdezések igénybe vehet, jelentős rendszer-erőforrásokat. Ez a szakasz bemutatja, hogyan használhatja a dinamikus kezelése nézeteket néhány gyakori lekérdezési teljesítményproblémákat feltárása. Egy régebbi, de továbbra is hasznos hivatkozást hibaelhárítási, a [Hibaelhárítási teljesítményproblémákat az SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) a cikk a Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Legnépszerűbb vagy legnagyobb N lekérdezések keresése

Az alábbi példa eredménye az átlagos Processzor időpontonként rangsorban legfelső öt lekérdezésekkel kapcsolatos információkat. Ebben a példában a lekérdezések aszerint, hogy a lekérdezés kivonat összesíti az, hogy azok összesített erőforrás-felhasználás szerint csoportosított logikailag egyenértékű lekérdezések.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Figyelés a blokkolt lekérdezések

Lassú, vagy a hosszabb ideig futó lekérdezések hozzájárulhatnak a fölösleges erőforrás-felhasználás és letiltott lekérdezések következményei kell. A blokkolás az oka lehet gyenge alkalmazás tervezés, hibás lekérdezés tervek, hiánya hasznos indexek, és így tovább. Ha az aktuális zárolási tevékenység információkra az Azure SQL-adatbázisban a sys.dm_tran_locks nézetben is használhatja. Ha például kód, lásd: [sys.dm_tran_locks (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms190345.aspx) az SQL Server könyvek Online.

### <a name="monitoring-query-plans"></a>Lekérdezés csomagok ellenőrzése

Egy hatékony lekérdezéstervet is növelheti Processzor felhasználás. Az alábbi példában a [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) nézet megállapíthatja, hogy melyik lekérdezés használja a leggyakrabban göngyölt Processzor.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Lásd még:

[SQL-adatbázis – bevezetés](sql-database-technical-overview.md)
