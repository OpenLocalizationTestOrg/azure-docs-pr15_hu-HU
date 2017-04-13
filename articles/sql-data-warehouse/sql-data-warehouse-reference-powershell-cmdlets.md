<properties
   pageTitle="Azure SQL-adatraktár PowerShell-parancsmagok"
   description="Keresse meg a felső PowerShell-parancsmagok az Azure SQL adatraktár, például hogy miként mutasson az egérrel, és folytassa az adatbázis."
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
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-parancsmagok és az SQL adatraktár REST API-khoz

Számos SQL adatraktár felügyeleti feladatok Azure PowerShell-parancsmagok vagy a REST API-khoz is kezelhetők.  Alul néhány példa a PowerShell-parancsok használatával automatizálhatja a gyakori feladatok a SQL adatraktár a láthatók.  Jó többi példák ismertető [kezelése méretezhetőség a többi][].

> [AZURE.NOTE]  Azure PowerShell 1.0.3 verzió szüksége Azure PowerShell használata SQL adatraktár, vagy nagyobb.  Érdemes verziójától futtatásával **Get-modul - ListAvailable-neve Azure**.  A legújabb [Microsoft webes Platform telepítő][]a telepíthető.  A legújabb verziójának telepítésével kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Első lépések az Azure PowerShell-parancsmagok

1. Nyissa meg a Windows PowerShell. 
2. A PowerShell-parancssorában futtassa az ezek a parancsok az Azure erőforrás-kezelő bejelentkezés, és jelölje ki azt az előfizetést.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Szünet SQL adatok raktári példa

Mutasson egy "Kiszolgáló01." nevű kiszolgálón lévő "Database02" nevű adatbázis  A kiszolgáló van egy Azure erőforráscsoport nevű "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Módosítás, az ebben a példában a beolvasott objektum [Felfüggesztett-AzureRmSqlDatabase][]pipák.  Emiatt az adatbázis fel van függesztve. A véglegesként parancsot mutatja az eredményeket.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Indítsa el az SQL-adatok raktári példa

A "Kiszolgáló01." nevű kiszolgálón lévő "Database02" nevű adatbázis folytatása A kiszolgálón lévő "ResourceGroup1." nevű erőforráscsoport

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Módosítás, ez a példa beolvassa "Database02" nevű "Kiszolgáló01", "ResourceGroup1." nevű erőforráscsoport lévő nevű kiszolgálóról adatbázis Az [Önéletrajz-AzureRmSqlDatabase][]beolvasott objektum pipák azt.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Látható, hogy ha a kiszolgáló nem foo.database.windows.net, használja az "élőlá" PowerShell-parancsmagok - kiszolgálónév.

## <a name="frequently-used-powershell-cmdlets"></a>A gyakran használt PowerShell-parancsmagok

A PowerShell-parancsmagok az Azure SQL-adatraktár gyakran használt.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Új AzureRmSqlDatabase][]
- [Eltávolítás-AzureRmSqlDatabase][]
- [Visszaállítás-AzureRmSqlDatabase][] 
- [Önéletrajz-AzureRmSqlDatabase][]
- [Jelölje ki-AzureRmSubscription][]
- [Set-AzureRmSqlDatabase][]
- [Felfüggesztése AzureRmSqlDatabase][]

## <a name="next-steps"></a>Következő lépések
További PowerShell példákat az alábbi témakörökben talál:

- [Hozzon létre egy SQL-adatraktár PowerShell használatával][]
- [Adatbázis visszaállítása][]

A PowerShell automatizálható feladatok listáját olvassa el a [Azure SQL-adatbázis parancsmagok][]című témakört.  A tevékenységek, amely a többi automatizálható, című témakör [Műveletek az Azure SQL-adatbázisait][].

<!--Image references-->

<!--Article references-->
[Telepítse és állítsa be a Azure PowerShell hogyan]: ./powershell-install-configure.md
[Hozzon létre egy SQL-adatraktár PowerShell használatával]: ./sql-data-warehouse-get-started-provision-powershell.md
[Adatbázis visszaállítása]: ./sql-data-warehouse-restore-database-powershell.md
[A többi méretezhetőség kezelése]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL-adatbázis parancsmagok]: https://msdn.microsoft.com/library/mt574084.aspx
[Azure SQL-adatbázisait műveletek]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Új AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Eltávolítás-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Visszaállítás-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Önéletrajz-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Jelölje ki-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Felfüggesztése AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[A Microsoft webes Platform telepítő]: https://aka.ms/webpi-azps
