<properties 
    pageTitle="SQL-adatbázis XEvent csengetés pufferelési kódját |} Microsoft Azure" 
    description="A gyűrű pufferelési céljának Azure SQL-adatbázisban használatával gyors és egyszerű végzett Transact-SQL nyelvben kód minta biztosít." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Csengetés, bővített események SQL-adatbázisban pufferelési cél kódját

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

A bővített esemény egy próba során rögzítése és a jelentés adatait rövid legegyszerűbben kívánt teljes kód minta. A bővített eseményadatok legegyszerűbb célja [csengetés pufferelési cél](http://msdn.microsoft.com/library/ff878182.aspx).


Ez a témakör bemutatja a Transact-SQL-kódot minta, amely:


1. Létrehoz egy táblázatot az adatokkal, amelyek bemutatják a.

2. Létrehoz egy meglévő bővített esemény, vagyis **sqlserver.sql_statement_starting**-munkamenetet.
    - Az esemény korlátozódik, egy adott frissítés karakterláncot tartalmazó SQL-utasítások: **utasítás, PÉLDÁUL a "frissítés tabEmployee %"**.
    - Cél típusú csengetés pufferelési, azaz **package0.ring_buffer**az esemény kimenetként választja.

3. Az esemény munkamenet indítása.

4. Néhány egyszerű frissítés az SQL-utasítások hibák.

5. Hibák egy SQL kiválasztása esemény kimeneti beolvasni a csengetés puffer.
    - **sys.dm_xe_database_session_targets** és más dinamikus kezelése nézetek (DMVs) csatlakozott.

6. Leállítja az esemény munkamenetet.

7. A gyűrű pufferelési cél, engedje fel az erőforrások esik.

8. A rendezvény munkamenetet, és a bemutató táblázat esik.


## <a name="prerequisites"></a>Előfeltételek


- Az Azure-fiók és az előfizetés. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)jelentkezhetnek.


- Bármely adatbázis táblájához is létrehozhat.
 - Másik lehetőségként hozhat [létre egy **AdventureWorksLT** bemutató adatbázis](sql-database-get-started.md) perc alatt.


- SQL Server Management Studio (ssms.exe), ideális esetben a legújabb havi frissítés verzióját. A legújabb ssms.exe a töltheti le:
 - [Töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx)című témakört.
 - [Közvetlen hivatkozás letöltéséhez.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Példa a kódot.


Nagyon kis módosítással az alábbi példa a csengetés pufferelési kód futtatható Azure SQL-adatbázis vagy a Microsoft SQL Server. A különbség a "ír elé _adatbázis" neve bizonyos dinamikus kezelése nézetek (DMVs), az 5 a FROM záradékban használt csomópont jelenlétét. Példa:

- sys.dm_xe**ír elé _adatbázis**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Csengetés pufferelési tartalma


A kód minta futtatásához ssms.exe azt használni.


Az eredmények megtekintéséhez, azt az oszlop élőfej **target_data_XML**csoportban a cella gombra kattintva.

Az eredmények ablaktáblájában azt rákattintva a cella csoportban az oszlop élőfej **target_data_XML**. A létrehozott egy másik fájl fülre, amelyben az eredmény cella tartalmának megjelent, XML formátumban ssms.exe gombra.


A kimenet megjelenik a következő blokk. Hosszú néz ki, de csak a két **<event>** elemeket.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>A gyűrű pufferelési birtokában megjelenés erőforrások


Amikor befejezte a csengetés pufferelési, távolítsa el azt, és engedje fel az erőforrásait kiállító egy **módosíthatja** például a következőket:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Az esemény munkamenet definícióját frissíteni, de nem. Később egy másik példányát a csengetés puffer adhat az esemény munkamenetet:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>További információ


Az elsődleges témakör Azure SQL-adatbázis a bővített eseményekre vonatkozóan van:


- [Kiterjesztett esemény előtt megfontolandó kérdések SQL-adatbázisban](sql-database-xevent-db-diff-from-svr.md), ellentétben néhány bővített események és a Microsoft SQL Server Azure SQL-adatbázis alkalmazás közötti eltérő szempontjait.


Az alábbi hivatkozásokat a bővített események minta kód témakörök érhetők el. Azonban rendszeresen ellenőrizni kell minden olyan minta kattintva megtekintheti, hogy a minta célként Microsoft SQL Server Azure SQL-adatbázis és. Ezután eldöntheti, hogy kisebb változások a minta futtatásához szükséges.


- Azure SQL-adatbázis kód minta: [nézeteihez cél kód SQL-adatbázisban bővített eseményekre vonatkozóan:](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
