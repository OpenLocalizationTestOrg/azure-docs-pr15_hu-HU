<properties
    pageTitle="Egy Azure SQL-adatbázis létrehozása a PowerShell használatával BACPAC-fájl importálása |} Microsoft Azure"
    description="Egy Azure SQL-adatbázis létrehozása a PowerShell használatával BACPAC-fájl importálása"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Egy Azure SQL-adatbázis létrehozása a PowerShell használatával BACPAC-fájl importálása

**Egy adatbázis**

> [AZURE.SELECTOR]
- [Azure portál](sql-database-import.md)
- [A PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Ebben a cikkben utasításokat az Azure SQL-adatbázis létrehozása PowerShell [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájl importálásával.

Az adatbázis-Azure tároló blob-tárolóból importált BACPAC fájlból (.bacpac) jön létre. Ha nincs Azure-tárolóban lévő BACPAC fájl, olvassa el a [Archív egy PowerShell használatával BACPAC fájlba Azure SQL-adatbázishoz](sql-database-export-powershell.md). Ha már van egy BACPAC fájlt, amely nem szerepel az Azure tárolására, [használata AzCopy egyszerűen töltse fel, a tárhely Azure-fiókjába](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Azure SQL-adatbázis automatikusan hozza létre és kezeli biztonsági másolatok minden felhasználó adatbázishoz, amely vissza tudja állítani. A részletekért olvassa [SQL-adatbázis automatikus biztonsági mentést](sql-database-automated-backups.md).


Importálhat egy SQL-adatbázisban, az alábbiakra van szükség:

- Egy Azure-előfizetést. Ha van szüksége az Azure előfizetéssel egyszerűen **Ingyenes próbaverziót** , ez a lap tetején kattintson, és ezután térjen vissza az Ez a cikk Befejezés.
- Az importálni kívánt adatbázis BACPAC fájl. A BACPAC kell lennie az [Azure tárterület-fiók](../storage/storage-create-storage-account.md) blob-tárolóban.



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>A környezet változói beállítása

Van néhány változók, ahol cserélje ki a példa értékeket az adott az adatbázis és a tárterület-fiók szükséges.

A kiszolgáló nevét kell lennie, amely jelenleg csak az előfizetést, az előző lépésben kiválasztott kiszolgáló. A kiszolgáló, létre kell hozni az adatbázis kell tenni. Egy adatbázis importálása rugalmas készletbe közvetlenül nem támogatott. De először importálása egy adatbázist, és helyezze át az adatbázis erőforráskészlethez tartozik.

Az adatbázis nevét a nevet, amelyet az új adatbázis.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


A következő változók vannak a tárterület-fiókból, ahol a BACPAC található. Az [Azure portálon](https://portal.azure.com)nyissa meg megszerezni a következő értékeket tároló fiókját. Az access elsődleges kulcs a tárhely fiókját a lap **összes beállítást** , majd a **billentyűk** kattintva megtalálhatja.

A neve blob-meglévő BACPAC fájlt szeretne létrehozni, az adatbázis nevét. A .bacpac kiterjesztést is használni kell.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Futó az [Get-hitelesítő adatokhoz] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) parancsmag megnyit egy ablakot megkérdezi, hogy a felhasználónevet és jelszót. Írja be a rendszergazda bejelentkezési és a jelszót az SQL-adatbázis kiszolgálója ($ServerName a fentiektől), és nem a felhasználónevet és Azure-fiókjához tartozó jelszót.

    $credential = Get-Credential


## <a name="import-the-database"></a>Az adatbázis importálása

Ez a parancs elküldte a szolgáltatás adatbázis importálása kérelmet. Attól függően, hogy az adatbázis mérete az importálási művelet elvégzéséhez néhány időt vehet igénybe.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>A művelet állapotának nyomon követéséhez

[Új-AzureRmSqlDatabaseImport] futtatása után (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), akkor is a kérelem állapotának futtatásával ellenőrizze a [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>SQL-adatbázis PowerShell-parancsprogramot, importálása


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Következő lépések

- Csatlakozás és a lekérdezés SQL-adatbázisból importált című témakörben talál [Csatlakozás SQL Server Management Studio az SQL-adatbázishoz, és hajtsa végre a minta T-SQL-lekérdezés](sql-database-connect-query-ssms.md)
