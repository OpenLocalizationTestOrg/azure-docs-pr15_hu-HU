<properties
   pageTitle="Számítási power az Azure SQL adatok raktári (REST) kezelése |} Microsoft Azure"
   description="A tevékenységek kezelése a PowerShell power számítja ki. Méretezés erőforrások eredményhez tartozó DWUs számítja ki. Vagy mutasson az egérrel, és folytathatja a számítási erőforrások költségeinek mentése."
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

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Számítási power az Azure SQL adatok raktári (REST) kezelése

> [AZURE.SELECTOR]
- [– Áttekintés](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [A PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [TÖBBI](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Méretezés teljesítmény méretezés kifelé az erőforrások és a terhelést változó igényeit memória számítja ki. Mentés költségeket méretezési vissza az erőforrások által nem csúcs időpontok vagy felfüggesztése számítási során teljesen. 

A gyűjtemény a tevékenységek az Azure portal használja:

- Méretezés számítási
- Szünet számítási
- Önéletrajz számítási

Ezzel kapcsolatos további tudnivalókért lásd: a [kezelése – áttekintés számítja ki][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Méretezés számítási power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

A DWUs beállításához használja a [Létrehozás vagy az adatbázis frissítése][] REST API-t. Az alábbi példa a szolgáltatás szintű cél DW1000 az adatbázis-kiszolgálón Sajátkiszolgáló üzemelteti, amely MySQLDW állítja. A kiszolgáló egy ResourceGroup1 nevű Azure erőforráscsoport van.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Szünet számítási

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Mutasson egy adatbázist, [Mutasson az adatbázis][] REST API-t használja. A következő példa szüneteltetheti nevű Database02 kiszolgáló01 nevű kiszolgálón tárolt adatbázis. A kiszolgáló egy ResourceGroup1 nevű Azure erőforráscsoport van.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Önéletrajz számítási

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Ha egy adatbázist, használja az [Önéletrajz adatbázis][] REST API-t. A következő példa nevű Database02 kiszolgáló01 nevű kiszolgálón tárolt adatbázis kezdődik. A kiszolgáló egy ResourceGroup1 nevű Azure erőforráscsoport van. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések

Más teendőkről a [kezelés áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[Kezelés – áttekintés]: ./sql-data-warehouse-overview-manage.md
[Kezelni a számítási – áttekintés]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Szünet adatbázis]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Önéletrajz-adatbázis]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Hozzon létre vagy az adatbázis frissítése]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
