<properties
    pageTitle="Biztonsági mentése és visszaállítása az alkalmazás szolgáltatás alkalmazások a PowerShell használatával"
    description="Megtudhatja, hogy miként biztonsági mentése és visszaállítása az alkalmazás Azure App szolgáltatásban a PowerShell használatával"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Biztonsági mentése és visszaállítása az alkalmazás szolgáltatás alkalmazások a PowerShell használatával

> [AZURE.SELECTOR]
- [A PowerShell](app-service-powershell-backup.md)
- [REST API-VAL](../app-service-web/websites-csm-backup.md)

Megtudhatja, hogy miként Azure PowerShell-lel való mentése és visszaállítása az [alkalmazás szolgáltatás alkalmazások](https://azure.microsoft.com/services/app-service/web/). További információt a web app biztonsági másolatokat, követelményeket és korlátozásokat, beleértve a [biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást](../app-service-web/web-sites-backup.md)témakörben talál.

## <a name="prerequisites"></a>Előfeltételek
Az alkalmazás biztonsági másolatok kezelése a PowerShell használatával, a következőkre lesz szüksége:

- **A Társítások URL-címet** , amely lehetővé teszi az olvasási és írási hozzáféréssel az Azure tároló tárolóhoz. Társítások URL-címek szakaszok egyenként ismertetik [a Társítások modell ismertetése](../storage/storage-dotnet-shared-access-signature-part-1.md) című cikk nyújt. Lásd: [Azure PowerShell használatá Azure-tároló](../storage/storage-powershell-guide-full.md) kezelése a PowerShell használatá Azure tároló példák.
- **Egy adatbázis-kapcsolati karakterlánc** Ha együtt a web app adatbázis biztonsági mentése szeretne.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Hogyan kell használni a web app biztonsági parancsmagokat Társítások URL létrehozása
Társítások URL-címet a PowerShell hozhatók létre. Íme egy példa bemutatja, hogyan hozhat létre egy, a jelen cikkben tárgyalt parancsmagok használható.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Telepítse az Azure PowerShell 1.3.2-s vagy újabb

[Azure PowerShell használatá Azure erőforrás-kezelővel](../powershell-install-configure.md) telepítésével és használatával Azure Powershellhez útmutatást talál.

## <a name="create-a-backup"></a>Biztonsági másolat létrehozása

A New-AzureRmWebAppBackup parancsmag használatával webalkalmazást biztonsági másolatot készíteni.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Az automatikusan generált nevű Ez létrehozza a biztonsági másolatot. Ha szeretné a biztonsági másolat egy név megadására, BiztonságiMásolatNeve választható paraméter használatával.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

A biztonsági mentés felvenni egy adatbázist, először hozzon létre egy új-AzureRmWebAppDatabaseBackupSetting parancsmaggal a adatbázis biztonsági beállítást, majd adja meg ezt a beállítást, a New-AzureRmWebAppBackup parancsmag az adatbázisok paraméter. Az adatbázisok paraméter egy tömb adatbázis beállításait, amely lehetővé teszi, hogy egynél több adatbázis biztonsági mentése elfogadja.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Biztonsági másolatok beszerzése

A Get-AzureRmWebAppBackupList parancsmagot a webalkalmazás az összes biztonsági másolatok tömbjét adja eredményül. Meg kell adnia a web app és az erőforráscsoport a nevét.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Egy adott biztonsági másolat, használja a Get-AzureRmWebAppBackup parancsmag.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Akkor is pipe egy web app-objektum, sem a biztonságimásolat-kezelési parancsmagok kényelmesebbé.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Az automatikus biztonsági mentés ütemezése

A megadott időközönként automatikusan történjen biztonsági másolatok ütemezheti. Biztonsági mentési ütemezés beállításához használja a Szerkesztés-AzureRmWebAppBackupConfiguration parancsmag. Ezzel a parancsmaggal több paramétert hajtja végre:

- **Név** - a webes alkalmazás nevére.
- **ResourceGroupName** – a csoport nevét, az erőforrás tartalmazó a web App alkalmazásban.
- **Tárolóhely** – nem kötelező. A web app tárolóhely neve.
- **StorageAccountUrl** - Társítások URL-cím a biztonsági másolatok tárolására szolgáló Azure tároló tároló.
- **FrequencyInterval** - milyen gyakran történjen a biztonsági másolatok numerikus értéket. Pozitív egész számnak kell lennie.
- **FrequencyUnit** - időegység esetén milyen gyakran történjen a biztonsági másolatok. A következők órát és napot.
- **RetentionPeriodInDays** – az automatikus biztonsági mentést automatikusan törlésig menteni szeretné, hogy hány nap.
- **Kezdés időpontja** – nem kötelező. Az idő, ha az automatikus biztonsági mentést kell kezdődnie. Biztonsági másolatok azonnal kezdődik, ha ez nem null. Egy DateTime kell lennie.
- **Adatbázisok** – nem kötelező. Tömbje DatabaseBackupSettings biztonsági másolatot készít az adatbázisok.
- **KeepAtLeastOneBackup** – nem kötelező átváltott paraméter. Adja meg a Ha egy biztonsági mindig őrizze a tárterület-fiókot, függetlenül attól, hogy régi.

Következő képen, hogy miként ezzel a parancsmaggal.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Az aktuális ütemezés, használja a Get-AzureRmWebAppBackupConfiguration parancsmag. Ez akkor lehet hasznos, ha módosítja egy ütemtervet, már konfigurált.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Egy webalkalmazás visszaállítása biztonsági másolatból

A visszaállítás-AzureRmWebAppBackup parancsmag használatával webalkalmazást visszaállítása biztonsági másolatból. Ezzel a parancsmaggal legegyszerűbb módja, ha a Get-AzureRmWebAppBackup parancsmag vagy a Get-AzureRmWebAppBackupList parancsmag beolvasni a biztonságimásolat-objektum függőleges vonás.

Ha befejezte a biztonságimásolat-objektum, a visszaállítás-AzureRmWebAppBackup parancsmag be is pipe. Adja meg, hogy a felülírási kapcsoló paraméter jelzi, hogy szeretné-e a webalkalmazás tartalmának felülírása a biztonsági mentés tartalmának. Ha a biztonsági mentés adatbázisok tartalmaz, ezeket az adatbázisok vissza is.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Következő képen, hogyan használhatja a visszaállítás AzureRmWebAppBackup megadásával az összes paramétert.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Biztonsági másolat törlése

Biztonsági másolat törléséhez használja a eltávolítása-AzureRmWebAppBackup parancsmag. Ez a biztonsági mentés eltávolítja a tárterület-fiókját. Adja meg az alkalmazás nevét, az erőforráscsoport és a törölni kívánt biztonsági másolat azonosítója.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

A biztonsági másolat objektum is is pipe, az Eltávolítás-AzureRmWebAppBackup parancsmag törölheti azt be.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
