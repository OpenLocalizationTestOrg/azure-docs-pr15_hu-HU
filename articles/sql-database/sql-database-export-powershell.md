<properties
    pageTitle="Az Azure SQL-adatbázis BACPAC fájlba archiválhatja a PowerShell használatával"
    description="Az Azure SQL-adatbázis BACPAC fájlba archiválhatja a PowerShell használatával"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Az Azure SQL-adatbázis BACPAC fájlba archiválhatja a PowerShell használatával

> [AZURE.SELECTOR]
- [Azure portál](sql-database-export.md)
- [A PowerShell](sql-database-export-powershell.md)


Ebben a cikkben utasításokat az Azure SQL-adatbázishoz (Azure Blob-tárolóhoz tárolt) [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlba archiválásra PowerShell használatával.

Kell archiválhatja Azure SQL-adatbázishoz, amikor a adatbázissémát, és az adatokat exportálhatja BACPAC fájlba. Egy fájl BACPAC egyszerűen egy ZIP-fájl .bacpac kiterjesztésű fájlt. BACPAC fájl később Azure Blob-tárolóhoz vagy a helyi tároló helyszíni helyen tárolni. Azt is importálhatók az Azure SQL-adatbázis vagy az SQL Server telepítéséhez a helyszíni.

**Megfontolandó szempontok**

- Egy archívumhoz biztosítani szeretné konzisztens tranzakción keresztül, győződjön meg arról, hogy nincs írási tevékenység Mi az exportálás során, vagy egy [tranzakción keresztül egységes másolása](sql-database-copy.md) az Azure SQL-adatbázis exportál.
- Azure Blob-tárolóhoz archivált BACPAC fájlok maximális mérete 200 GB. Helyi tárolóba nagyobb BACPAC fájl archiválásához segédprogrammal [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssor parancsot. Ez a segédprogram a Visual Studio és az SQL Server rendszerhez. Is megtekintheti, [Töltse le](https://msdn.microsoft.com/library/mt204009.aspx) az SQL Server Data Tools a segédprogram megszerezni legújabb verzióját.
- Azure prémium tárolóhoz BACPAC fájlból archiválás nem támogatott.
- Ha az exportálási művelet túllépi 20 órát, előfordulhat, hogy megszakítható. Exportálás során teljesítmény növelése érdekében a következőkre van lehetősége:
 - A szolgáltatás szintjének ideiglenes növelése
 - Az összes olvasási és írási tevékenység az exportálás során jogosultsága szűnik.
 - Használja a [csoportosított index](https://msdn.microsoft.com/library/ms190457.aspx) létrehozása az összes nagy táblázatok nem null értékű. Csoportosított indexek, nélkül exportálása előfordulhat, hogy sikertelen, ha a 6-12 óránál hosszabb ideig tart. Ennek oka az, próbálja ki a teljes táblázat exportálása egy táblázat keresés befejezéséhez szükséges az Exportálás szolgáltatást. Határozza meg, ha a táblák exportálása szolgáltatáshoz optimalizált jó módszer, hogy futtatni **DBCC SHOW_STATISTICS** , és ügyeljen arra, hogy a *RANGE_HI_KEY* nem null és az érték helyes terjesztési. A részletekért olvassa [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs nem célja, hogy a visszaállítási művelet és biztonsági másolat használható. Azure SQL-adatbázis automatikusan létrehozza a biztonsági másolatok minden felhasználó adatbázishoz. A részletekért olvassa [SQL-adatbázis automatikus biztonsági mentést](sql-database-automated-backups.md).

Ez a cikk a következőkre lesz szüksége:

- Egy Azure-előfizetést.
- Microsoft Azure SQL-adatbázisban.
- Egy [szabványos tároló Azure-fiók](../storage/storage-create-storage-account.md), a BACPAC tárolása a szabványos tároló blob-tároló.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Az adatbázis exportálása

Az [új AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) parancsmag elküldte a szolgáltatás adatbázis exportálás kérelmet. Attól függően, hogy az adatbázis mérete az exportálási művelet elvégzéséhez néhány időt vehet igénybe.

> [AZURE.IMPORTANT] Zökkenőmentes tranzakción keresztül egységes BACPAC fájl, célszerű az első [Hozzon létre egy másolatot az adatbázisról](sql-database-copy-powershell.md), és majd exportálja az adatbázis-példány.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Az exportálási művelet állapotának nyomon követéséhez

[Új-AzureRmSqlDatabaseExport] futtatása után (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), ellenőrizheti a kérelem állapotának futtatásával [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Fut közvetlenül azután, hogy az összehívást általában eredménye **Állapot: esetbejegyzések**. Amikor megjelenik a **Állapot: sikeres** az exportálás befejeződött.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>SQL-adatbázis példa exportálása

A következő példa egy meglévő SQL-adatbázis exportálása egy BACPAC, és ezután szemlélteti, hogyan lehet az exportálási művelet állapotának ellenőrzése.

A példa futtatásához vannak cserélje ki az adatbázis- és a fiók egyedi értékeket kell néhány változók. Az [Azure portálon](https://portal.azure.com)nyissa meg az tárterület-fiókjába a tárhely fióknév, blob-tárolóhoz nevét és kulcs értékét. A kulcs a tárterület a fiók lap a **hívóbetűk** kattintva megtalálhatja.

A következő cseréje `VARIABLE-VALUES` az adott Azure erőforrások értékű. Az adatbázis nevét, amelyet exportálni szeretne, a meglévő adatbázis.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Következő lépések

- Azure SQL-adatbázishoz importálása a Powershell használatával című témakörben talál [a PowerShell használatá BACPAC importálni](sql-database-import-powershell.md).


## <a name="additional-resources"></a>További források

- [Új AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)