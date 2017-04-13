<properties
   pageTitle="Figyelheti a terhelést DMVs használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként figyelheti a terhelést DMVs használatával."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Figyelheti a terhelést DMVs használatával

Ez a cikk ismerteti, hogyan használhatja a dinamikus kezelése nézeteket (DMVs) a terhelést figyelésére és vizsgálja meg a lekérdezés-végrehajtási az Azure SQL-adatraktár.

## <a name="permissions"></a>Engedélyek

Ebben a cikkben a DMVs lekérdezéséhez NÉZET adatbázis MEGYE vagy a VEZÉRLŐELEM jogosultsági szüksége van. Nyújtó VIEW adatbázis STATE rendszerint a használni kívánt engedélyt, mert sokkal erősebb korlátozásokat alkalmaz.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Monitor kapcsolatok

Az SQL adatraktár összes bejelentkezések [sys.dm_pdw_exec_sessions][]bejelentkezve.  Ez a DMV az utolsó 10 000 bejelentkezések tartalmazza.  A session_id az elsődleges kulcs és egymás után minden új bejelentkezési van rendelve.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Lekérdezés-végrehajtási monitor

Az összes lekérdezés SQL adatraktár végrehajtása [sys.dm_pdw_exec_requests][]bejelentkezve.  A DMV végrehajtása az utolsó 10 000 lekérdezések tartalmazza.  A request_id egyedileg azonosítja minden lekérdezés és a DMV elsődleges kulcsa.  A request_id van-e hozzárendelve egymás után minden új lekérdezés, és ha van QID jelző azonosítóját a lekérdezést.  Egy adott bejelentkezési az összes lekérdezése ezt az egy adott session_id DMV lekérdezése látható.

>[AZURE.NOTE] Tárolt eljárások több kérése azonosítók használja.  Kérés azonosítók növekvő sorrendben vannak hozzárendelve. 

Az alábbiakban vizsgálja meg a lekérdezés-végrehajtási tervei és időpontok egy adott lekérdezés kövesse a lépéseket.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>LÉPÉS: 1: A lekérdezés vizsgálja meg a kívánt azonosítása

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

A fenti lekérdezés közül, a lekérdezést, amely vizsgálja meg a kívánt **Megjegyzés kérése azonosítója** .

Lekérdezések **felfüggesztett** állapotban vannak éppen aszinkron feldolgozási korlátai miatt. Ezek a lekérdezések is megjelennek a UserConcurrencyResourceType egy adott típusú sys.dm_pdw_waits vár lekérdezésre. Olvassa el a [feldolgozási és terhelést kezelés][] feldolgozási korlátai rendszeren. Lekérdezések is is megvárja, amíg más oka lehet például az objektumok zárolását.  Ha a lekérdezés egy erőforrás várakozik, olvassa el a [Várakozás a források Investigating lekérdezések][] ebben a cikkben további lefelé.

A keresési lekérdezések az sys.dm_pdw_exec_requests táblázat egyszerűsítése Megjegyzés hozzárendelése a lekérdezést, amely a sys.dm_pdw_exec_requests nézetben lehet keresett [címke][] segítségével.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>LÉPÉS: 2: Vizsgálja meg a lekérdezés terv

A lekérdezés elosztott SQL (DSQL) csomag lekérése [sys.dm_pdw_request_steps][]használja kérése Azonosítóját.

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

DSQL terv vártnál tovább tart, ha a probléma okát lehet egy összetett terv több DSQL lépésben vagy csak egyetlen lépés hosszú időt vesz igénybe.  Ha a terv több lépésben több áthelyezése a műveleteket, fontolja meg optimalizálása adatok mozgását csökkentheti a táblában terjesztését. A [táblázat terjesztési][] cikk ismerteti, hogy miért adatok átkerül oldja meg a lekérdezés, és egyes terjesztési stratégiák adatok mozgását minimalizálásához ismerteti.

Vizsgálja meg a további egy lépésben, a hosszú ideig futó lekérdezéslépés *operation_type* oszlopa részleteit, és jegyezze fel a **Lépés Index**:

- 3.a lépés folytassa az **SQL-műveletek**: OnOperation, RemoteOperation, ReturnOperation.
- **Adatok mozgását**műveletekhez 3.b lépés folytatásához: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>3.a lépés: a megosztott adatbázisokat a vizsgálja meg a SQL

Az kérése Azonosítóját, illetve a lépés Index segítségével részleteinek lekérése [sys.dm_pdw_sql_requests][], összes a megosztott adatbázisokat a lekérdezéslépés az adatvégrehajtás-adatokat tartalmazó.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

A lekérdezéslépés futtatásakor [DBCC PDW_SHOWEXECUTIONPLAN][] használható az SQL Server becsült terv beolvasni a egy adott terjesztési futó lépésben az SQL Server terv gyorsítótárból.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>3.b lépés: a megosztott adatbázis mozgást adatok vizsgálata

Használja az kérése Azonosítóját, illetve a lépés Index beolvasni a [sys.dm_pdw_dms_workers][]minden egyes terjesztési futó adatok mozgását lépés adatait.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Jelölje be a *total_elapsed_time* oszlopban tekintheti meg, ha egy adott terjesztési jelentősen hosszabb, mint a többi adatot mozgását tart.
- A hosszabb ideig futó eloszlás jelölje be az *rows_processed* oszlop áthelyezése a terjesztési a sorok száma beállítás van-e lényegesen nagyobb, mint másoknak. Ha igen, ezzel jelezheti a mögöttes adatok ferdeség.

Ha a lekérdezést futtat, [DBCC PDW_SHOWEXECUTIONPLAN][] az SQL Server becsült terv az SQL Server-csomag gyorsítótárból lekérése az éppen futó SQL lépést belül egy adott terjesztési használható.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Várakozás lekérdezések figyelése

Ha, hogy megtalálja, hogy a lekérdezés van nem végez folyamatban, mert az adott erőforrás vár, az alábbiakban egyszerre megjelenítő lekérdezés összes erőforrás lekérdezés várakozik.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Ha a lekérdezés aktívan várakozik, egy másik lekérdezésből erőforrások, akkor az állapot **AcquireResources**lesz.  Ha a lekérdezés rendelkezik a szükséges erőforrások, akkor az állapot **engedélyezve**lesz.

## <a name="next-steps"></a>Következő lépések
Lásd: a [rendszer nézetek][] DMVs további információt.
Lásd: [Gyakorlati tanácsok az SQL-adatraktár][] gyakorlati tanácsokat olvashat

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL-adatraktár ajánlott eljárások]: ./sql-data-warehouse-best-practices.md
[Rendszer-nézetek]: ./sql-data-warehouse-reference-tsql-system-views.md
[Táblázat ki.]: ./sql-data-warehouse-tables-distribute.md
[Feldolgozási és terhelést kezelése]: ./sql-data-warehouse-develop-concurrency.md
[Várakozás az erőforrások lekérdezések vizsgálat alatt]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[CÍMKE]: https://msdn.microsoft.com/library/ms190322.aspx
