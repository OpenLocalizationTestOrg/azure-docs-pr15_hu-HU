<properties
    pageTitle="Az SQL Server virtuális gépeken futó (klasszikus) az automatikus biztonsági mentés |} Microsoft Azure"
    description="Az automatikus biztonsági mentés funkció ismerteti az SQL Server fut az Azure virtuális gépeken futó erőforrás-kezelő használatával. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Az automatikus biztonsági mentés az SQL Server Azure virtuális gépeken futó (klasszikus)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-automated-backup.md)
- [Klasszikus](virtual-machines-windows-classic-sql-automated-backup.md)

Az automatikus biztonsági mentés [Biztonsági másolat felügyelt a Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) automatikusan konfigurálja az SQL Server Standard 2014-es vagy vállalati Azure virtuális a meglévő és új adatbázisokra. Ez lehetővé teszi, hogy tartós Azure blob-tárolóhoz kihasználó adatbázis rendszeres biztonsági mentések beállítása. Az automatikus biztonsági mentés attól függ, hogy az [SQL Server IaaS ügynök bővítmény](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ez a cikk az erőforrás-kezelő verziójának megtekintéséhez olvassa el az [Automatikus biztonsági mentés az SQL Server Azure virtuális gépeken futó-kezelő](virtual-machines-windows-sql-automated-backup.md)című témakört.

## <a name="prerequisites"></a>Előfeltételek

Az automatikus biztonsági mentés használni, vegye figyelembe az alábbi előfeltételek:

**Operációs rendszer**:

- A Windows Server 2012
- A Windows Server 2012 R2

**Az SQL Server-verzió/edition**:

- Az SQL Server 2014-es Standard
- Az SQL Server 2014-es Enterprise

>[AZURE.NOTE] Az SQL Server 2016 még nem támogatott az automatikus biztonsági mentés.

**Adatbázis konfigurálása**:

- Céladatbázis a teljes helyreállítási modell kell használnia.

**Azure PowerShell**:

- [Telepítse a legújabb Azure PowerShell-parancsait](../powershell-install-configure.md).

**Az SQL Server IaaS bővítmény**:

- [Az SQL Server IaaS bővítmény telepítése](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások

Az alábbi táblázat bemutatja, hogy az automatikus biztonsági mentés állítható be. A klasszikus VMs PowerShell kell használnia, a beállítások konfigurálása.

|A beállítás|Tartomány (alapértelmezett)|Leírás|
|---|---|---|
|**Az automatikus biztonsági mentése**|Engedélyezhetik/letilthatják (korlátozott)|Engedélyezi vagy tiltja az automatikus biztonsági mentés egy SQL Server Standard 2014-es vagy vállalati Azure virtuális.|
|**Az adatmegőrzési időszak**|1 – 30 nap (30 nap)|Biztonsági megőrzendő napok számát.|
|**Tárterület-fiók**|Azure tárterület-fiókot (a tárhely hozzon létre fiókot a a megadott virtuális)|Az automatikus biztonsági mentés fájlok tárolására, blob-tárolóban lévő használni kívánt Azure tárolási fiókot. A tároló ezen a helyen tárolja az összes biztonságimásolat-fájlt jön létre. A biztonsági másolatnak elnevezési konvenció tartalmaz, a dátumot, időt és a számítógép nevét.|
|**Titkosítás:**|Engedélyezhetik/letilthatják (korlátozott)|Engedélyezi vagy tiltja a titkosítást. Ha engedélyezve van a titkosítást, a biztonsági mentés visszaállítása használt tanúsítványokat ugyanabban a automaticbackup tárolóban az azonos elnevezési konvenció megadott tárterület-fiókjában található. Ha módosítja a jelszót, hogy a jelszó hoz létre egy újat., de a régi tanúsítványt előzetes biztonsági mentés visszaállítása marad.|
|**Jelszó**|Jelszó-szöveg (nincs)|A titkosítási kulcs jelszavát. Erre csak akkor van szükség, ha engedélyezve van-e titkosítást. Egy titkosított biztonsági másolat visszaállításához kell rendelkeznie a helyes jelszót, és a kapcsolódó időben történt a biztonsági mentés használt tanúsítvány.|

## <a name="configuration-with-powershell"></a>A PowerShell konfigurálása

Az alábbi PowerShell példa az automatikus biztonsági mentés egy meglévő SQL Server 2014-es virtuális van beállítva. A **New-AzureVMSqlServerAutoBackupConfig** parancs konfigurálja az $storageaccount változó által megadott Azure tároló fiók biztonsági másolatok tárolására szolgáló automatikus biztonsági mentés beállításait. Ezek a biztonsági másolatok 10 napig megmarad. A **Set-AzureVMSqlServerExtension** parancs a megadott Azure virtuális frissíti, ezekkel a beállításokkal.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Telepítse és állítsa be az SQL Server IaaS Agent néhány percet is eltelhet.

Ahhoz, hogy a titkosítási, módosítsa az előző parancsfájl adják át a EnableEncryption paraméter együtt a CertificatePassword paraméter jelszó (biztonságos karakterlánc). A következő parancsfájl lehetővé teszi, hogy az automatikus biztonsági mentés beállításait az előző példában, és hozzáadja a titkosítást.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Az automatikus biztonsági mentés letiltása, futtassa a azonos nélkül a **-engedélyezése** a **New-AzureVMSqlServerAutoBackupConfig**paramétert. Telepítés után az eltarthat néhány percig, amíg az automatikus biztonsági mentés letiltása.

>[AZURE.NOTE] Letiltása és eltávolítása az SQL Server IaaS Agent nem távolítja el a korábban beállított felügyelt biztonsági mentés beállításait. Az automatikus biztonsági mentés előtt letiltása és eltávolítása az SQL Server IaaS Agent tiltsa le.

## <a name="next-steps"></a>Következő lépések

Az automatikus biztonsági mentés felügyelt biztonsági másolat Azure VMs állítja be. Ezért fontos, [tekintse át a dokumentáció felügyelt biztonsági másolatának](https://msdn.microsoft.com/library/dn449496.aspx) a működését és a következmények megértéséhez.

Kereshet további biztonsági mentése és visszaállítása az SQL Server Azure VMs útmutatást a következő témakörben: [biztonsági mentése és visszaállítása az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-backup-recovery.md).

Más elérhető automatizálási tevékenységekkel kapcsolatos további tudnivalókért lásd [Az SQL Server IaaS ügynök bővítmény](virtual-machines-windows-classic-sql-server-agent-extension.md).

Futó SQL Server Azure VMs kapcsolatos további tudnivalókért olvassa el a [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.
