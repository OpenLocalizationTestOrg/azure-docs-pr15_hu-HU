<properties
   pageTitle="Számítási power az Azure SQL adatok raktári (REST) kezelése |} Microsoft Azure"
   description="Méretezési teljesítmény DWUs eredményhez tartozó tevékenységek Transact-SQL nyelvben (T-SQL). Mentse a költségek méretezéssel vissza nem csúcs időszakokban."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Azure SQL-adatraktár (T-SQL) a számítási power kezelése

> [AZURE.SELECTOR]
- [– Áttekintés](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [A PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [TÖBBI](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Méretezés teljesítmény méretezés kifelé az erőforrások és a terhelést változó igényeit memória számítja ki. Mentés költségek méretezési vissza az erőforrások által nem csúcs időpontok vagy felfüggesztése számítási során teljesen. 

A gyűjtemény a tevékenységek T SQL használja:

- Nézet aktuális DWU beállításai
- Módosítás számítási erőforrások eredményhez tartozó DWUs

Mutasson az egérrel, vagy egy adatbázis folytatása, válassza a tetején látható ez a cikk a többi platformon lehetőségek közül.

Erről című témakörben talál [kezelése power áttekintése számítja ki][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Nézet aktuális DWU beállításai

Az adatbázisok aktuális DWU beállításainak megtekintése:

1. Nyissa meg az SQL Server-objektum Explorer Visual Studio 2015.
2. A logikai SQL-adatbázis kiszolgálója társított a fő adatbázist csatlakoztat.
2. Jelölje ki a sys.database_service_objectives dinamikus kezelése látható. Lássunk egy példát: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Méretezés számítási

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

A DWUs módosítása:


1. A logikai SQL-adatbázis kiszolgálója társított a fő adatbázist csatlakoztat.
2. Az [Adatbázis módosítása][] TSQL utasítást használja. Az alábbi példa a szolgáltatás szintű cél DW1000 az adatbázis MySQLDW állítja. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések

Más teendőkről a [kezelés áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Kezelés – áttekintés]: ./sql-data-warehouse-overview-manage.md
[Kezelni a számítási power – áttekintés]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ADATBÁZIS MÓDOSÍTÁSÁHOZ]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
