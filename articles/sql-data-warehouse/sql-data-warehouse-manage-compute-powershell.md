<properties
   pageTitle="Az Azure SQL-adatraktár (PowerShell) számítási power kezelése |} Microsoft Azure"
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
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Az Azure SQL-adatraktár (PowerShell) számítási power kezelése

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


## <a name="before-you-begin"></a>Első lépések

### <a name="install-the-latest-version-of-azure-powershell"></a>Azure PowerShell legújabb verziójának telepítése

> [AZURE.NOTE]  Adatraktár SQL Azure Powershellhez szoftver használatához szükséges 1.0.3 verzió Azure PowerShell-s vagy újabb.  Ellenőrizze a parancsot a korábbit **Get-modul - ListAvailable-neve Azure**. A [Microsoft webes Platform telepítő][]telepíthetők a legújabb verzióra.  További információért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Első lépések az Azure PowerShell-parancsmagok

Első lépések:

1. Nyissa meg az Azure PowerShell. 
2. A PowerShell-parancssorában futtassa az ezek a parancsok az Azure erőforrás-kezelő bejelentkezés, és jelölje ki azt az előfizetést.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Méretezés számítási power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Ha módosítani szeretné a DWUs, a [Set-AzureRmSqlDatabase][] PowerShell-parancsmag használata. Az alábbi példa a szolgáltatás szintű cél DW1000 az adatbázis-kiszolgálón Sajátkiszolgáló üzemelteti, amely MySQLDW állítja. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Szünet számítási

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Mutasson egy adatbázist, használja a [Felfüggesztett-AzureRmSqlDatabase][] parancsmag. A következő példa szüneteltetheti nevű Database02 kiszolgáló01 nevű kiszolgálón tárolt adatbázis. A kiszolgáló egy ResourceGroup1 nevű Azure erőforráscsoport van. 

> [AZURE.NOTE] Látható, hogy ha a kiszolgáló nem foo.database.windows.net, használja az "élőlá" PowerShell-parancsmagok - kiszolgálónév.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Módosítás, a következő példa olvassa be az adatbázis a $database objektummá. Majd pipák [Felfüggesztett-AzureRmSqlDatabase][]az objektumot. Az eredmények az objektum resultDatabase tárolja. A véglegesként parancsot mutatja az eredményeket.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Önéletrajz számítási

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Ha egy adatbázist, használja az [Önéletrajz-AzureRmSqlDatabase][] parancsmag. A következő példa nevű Database02 kiszolgáló01 nevű kiszolgálón tárolt adatbázis kezdődik. A kiszolgáló egy ResourceGroup1 nevű Azure erőforráscsoport van. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Módosítás, a következő példa olvassa be az adatbázis a $database objektummá. Ezután az [Önéletrajz-AzureRmSqlDatabase][] objektum pipák, és az eredmények $resultDatabase tárolja. A véglegesként parancsot mutatja az eredményeket.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések

Más teendőkről a [kezelés áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Kezelés – áttekintés]: ./sql-data-warehouse-overview-manage.md
[Telepítse és állítsa be a Azure PowerShell hogyan]: ./powershell-install-configure.md
[Kezelni a számítási – áttekintés]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Önéletrajz-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Felfüggesztése AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[A Microsoft webes Platform telepítő]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
