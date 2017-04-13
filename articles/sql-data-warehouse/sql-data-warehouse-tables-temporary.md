<properties
   pageTitle="Ideiglenes táblákat az SQL adatraktár |} Microsoft Azure"
   description="Első lépések az Azure SQL-adatraktár ideiglenes táblákat."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Ideiglenes táblákat az SQL adatraktár

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Adattípusok][]
- [Terjesztése][]
- [Index][]
- [Partition][]
- [Statisztika][]
- [Ideiglenes][]

Ideiglenes táblák nagyon hasznosak, ha az adatfeldolgozás – különösen hol köztes eredménye tranziens átalakítás során. SQL-adatraktár ideiglenes táblák léteznek a munkamenet szintjén.  Csak láthatók a munkamenethez, amelyben létrehozott, és amikor kijelentkezik a munkamenethez automatikusan törlődnek.  Ideiglenes táblák teljesítmény előny ajánlja fel, mert kiszámolt értékeik megtekintése kerülnek, nem pedig annak a távoli tároló helyi.  Ideiglenes táblák azonosak a némileg eltér az Azure SQL-adatraktár Azure SQL-adatbázis azok webböngészőn keresztül elérhetők tetszőleges belül a munkamenet belüli és kívüli a tárolt eljárás is beleértve.

Ez a cikk ideiglenes tábláinak használatával alapvető útmutatást tartalmaz, és kiemeli a munkamenet szintű ideiglenes táblák alapelvei. Információk felhasználásával Ez a cikk segítséget nyújt a kód újrahasznosításának és a karbantartás, a kód Kezeléstechnikai javítása modularize.

## <a name="create-a-temporary-table"></a>Ideiglenes táblázat létrehozása

Ideiglenes tábla létrehozása a táblázat neve és egyszerűen illesztésével egy `#`.  Példa:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Ideiglenes táblák is hozhatja létre a `CTAS` ugyanazon módszert használja:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`nagyon nagy teljesítményű parancs és az az előnye, hogy nagyon hatékony tranzakció naplózási hely használata során. 


## <a name="dropping-temporary-tables"></a>Ideiglenes táblák

Amikor létrejön az új munkamenet, ideiglenes táblák léteznie kell.  Jó helyen jár Ha a tárolt eljárást, amely hoz létre egy ideiglenes azonos nevű, annak biztosítására, hogy hívja fel a `CREATE TABLE` kimutatások sikeresek egy egyszerű előtti meglétének ellenőrzése a egy `DROP` is használható, mint az az alábbi példában:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Egységesebb kódolási, célszerű használni ezt a minta egyaránt táblázatok és ideiglenes.  Érdemes emellett célszerű használni az `DROP TABLE` ideiglenes táblák eltávolításához, ha a kód velük végzett.  Fejlesztési meglehetősen gyakran ahhoz, hogy ezeknek az objektumoknak eljárás végén közös kapcsolt legördülő parancsok megjelenítéséhez a tárolt eljárás be is törlődnek.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kódot.

Ideiglenes táblák tetszőleges munkamenetben is láthatja, mivel ez is lehet kihasználni segít a alkalmazás kód modularize.  Ha például a tárolt eljárás, az alábbi az egyetlen az ajánlott eljárások a fentiektől DDL, amely a statisztikai neve megváltozik, az adatbázis összes statisztika létrehozásához.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Ebben a szakaszban a csak műveletet, hogy rendelkezik-e a tárolt eljárás, amelyek egyszerűen kibocsátása generált #stats_ddl DDL-utasításokat az ideiglenes tábla.  Ez a tárolt eljárás fog legördülő #stats_ddl, ha már létezik annak érdekében, hogy nem sikerül a Ha többször a munkamenet belül futnak.  Jó helyen jár hiszen nem `DROP TABLE` végén található a tárolt eljárás a tárolt eljárás befejeztével bízza a létrehozott táblázatot, hogy a tárolt eljárás kívüli olvashatja.  Az SQL-adatraktár más SQL Server adatbázisoktól eltérően, ajánlatos az ideiglenes táblázatból, az azt létrehozó eljárás kívül.  Ideiglenes táblákat az SQL-adatraktár használt lehet **tetszőleges** belül a munkamenetet. Vezethet további elemes és kezelhető kód, mint az az alábbi példában:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Az ideiglenes tábla korlátai

SQL adatraktár ír elő, néhány korlátozások, ideiglenes táblák végrehajtása során.  Jelenleg csak a munkamenet hatókörű ideiglenes táblázatok támogatottak.  Globális ideiglenes táblázatok nem támogatottak.  Ezeken kívül nézetek ideiglenes táblázatok nem hozható létre.

## <a name="next-steps"></a>Következő lépések

További információért című témakörben olvashat [Táblázat áttekintése][– Áttekintés], [Táblázat adattípusok][Adattípusok], [táblázat terjesztése][elosztás], [táblázat indexelés][Index], [táblázat szétválasztás][partíciót] és [Fenntartása táblázat statisztika][statisztikai adatokat].  Ajánlott eljárások bővebben: [SQL adatok raktári gyakorlati tanácsokat][].

<!--Image references-->

<!--Article references-->
[– Áttekintés]: ./sql-data-warehouse-tables-overview.md
[Adattípusok]: ./sql-data-warehouse-tables-data-types.md
[Terjesztése]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md
[Ideiglenes]: ./sql-data-warehouse-tables-temporary.md
[Ajánlott eljárások a SQL-adatok raktári]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
