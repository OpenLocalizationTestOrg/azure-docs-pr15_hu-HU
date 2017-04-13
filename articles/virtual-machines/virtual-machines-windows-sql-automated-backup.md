<properties
    pageTitle="Az SQL Server virtuális gépeken futó (erőforrás-kezelő) az automatikus biztonsági mentés |} Microsoft Azure"
    description="Az automatikus biztonsági mentés funkció ismerteti az SQL Server fut az Azure virtuális gépeken futó erőforrás-kezelő használatával. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Az automatikus biztonsági mentés az SQL Server Azure virtuális gépeken futó (erőforrás-kezelő)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-automated-backup.md)
- [Klasszikus](virtual-machines-windows-classic-sql-automated-backup.md)

Az automatikus biztonsági mentés [Biztonsági másolat felügyelt a Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) automatikusan konfigurálja az SQL Server Standard 2014-es vagy vállalati Azure virtuális a meglévő és új adatbázisokra. Ez lehetővé teszi, hogy tartós Azure blob-tárolóhoz kihasználó adatbázis rendszeres biztonsági mentések beállítása. Az automatikus biztonsági mentés attól függ, hogy az [SQL Server IaaS ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell. Ez a cikk klasszikus verziójának megtekintése, olvassa el az [Automatikus biztonsági mentés az SQL Server Azure virtuális gépeken futó klasszikus](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Előfeltételek

Az automatikus biztonsági mentés használni, vegye figyelembe az alábbi előfeltételek:

**Operációs rendszer**:

- A Windows Server 2012
- A Windows Server 2012 R2

**Az SQL Server-verzió/edition**:

- Az SQL Server 2014-es Standard
- Az SQL Server 2014-es Enterprise

**Adatbázis konfigurálása**:

- Céladatbázis a teljes helyreállítási modell kell használnia.

**Azure PowerShell**:

- Ha azt tervezi, hogy állítsa be az automatikus biztonsági mentés PowerShell, a [telepítse a legújabb Azure PowerShell-parancsait](../powershell-install-configure.md) .

>[AZURE.NOTE] Az automatikus biztonsági mentés az SQL Server IaaS ügynök bővítmény támaszkodik. Aktuális SQL virtuális gépek galéria képek alapértelmezés szerint adja hozzá a bővítmény. További tudnivalókért lásd: az [SQL Server IaaS ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások

Az alábbi táblázat bemutatja, hogy az automatikus biztonsági mentés állítható be. Tényleges beállítási lépések attól függően, hogy használja-e az Azure portálja vagy az Azure Windows PowerShell-parancsait változnak.

|A beállítás|Tartomány (alapértelmezett)|Leírás|
|---|---|---|
|**Az automatikus biztonsági mentése**|Engedélyezhetik/letilthatják (korlátozott)|Engedélyezi vagy tiltja az automatikus biztonsági mentés egy SQL Server Standard 2014-es vagy vállalati Azure virtuális.|
|**Az adatmegőrzési időszak**|1 – 30 nap (30 nap)|Biztonsági megőrzendő napok számát.|
|**Tárterület-fiók**|Azure tárterület-fiókot (a tárhely hozzon létre fiókot a a megadott virtuális)|Az automatikus biztonsági mentés fájlok tárolására, blob-tárolóban lévő használni kívánt Azure tárolási fiókot. A tároló ezen a helyen tárolja az összes biztonságimásolat-fájlt jön létre. A biztonsági másolatnak elnevezési konvenció tartalmaz, a dátumot, időt és a számítógép nevét.|
|**Titkosítás:**|Engedélyezhetik/letilthatják (korlátozott)|Engedélyezi vagy tiltja a titkosítást. Ha engedélyezve van a titkosítást, a biztonsági mentés visszaállítása használt tanúsítványokat ugyanabban a automaticbackup tárolóban az azonos elnevezési konvenció megadott tárterület-fiókjában található. Ha módosítja a jelszót, hogy a jelszó hoz létre egy újat., de a régi tanúsítványt előzetes biztonsági mentés visszaállítása marad.|
|**Jelszó**|Jelszó-szöveg (nincs)|A titkosítási kulcs jelszavát. Erre csak akkor van szükség, ha engedélyezve van-e titkosítást. Egy titkosított biztonsági másolat visszaállításához kell rendelkeznie a helyes jelszót, és a kapcsolódó időben történt a biztonsági mentés használt tanúsítvány.|

## <a name="configuration-in-the-portal"></a>A portál konfigurálása
Az Azure Portal segítségével állítsa be az automatikus biztonsági mentés során, hogy kiépítési vagy meglévő VMs.

### <a name="new-vms"></a>Új VMs
Az Azure Portal segítségével állítsa be az automatikus biztonsági mentés, az erőforrás-kezelő telepítési modell egy új SQL Server 2014-es virtuális gép létrehozásakor.

Az **SQL Server-beállítások** lap válassza **az automatikus biztonsági mentés**. A következő Azure portál képernyőképen az **SQL automatikus biztonsági mentés** lap.

![SQL-biztonsági másolatot az automatikus konfiguráció az Azure-portálon](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Környezetben témakört teljes a [kiépítési virtuális gép SQL Server Azure-ban](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Meglévő VMs
Meglévő SQL Server virtuális gépeken futó jelölje be az SQL Server virtuális gépet. Válassza a **Beállítások** lap a **SQL Server konfigurálása** című szakaszát.

![SQL automatikus biztonsági mentés a meglévő VMs](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Az **SQL Server konfigurációs** lap kattintson a **Szerkesztés** gombra az automatikus biztonsági mentés szakaszában.

![Meglévő VMs SQL automatikus biztonsági mentés konfigurálása](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Amikor befejezte, kattintson az **OK** gombra a módosítások mentéséhez az **SQL Server konfigurálása** a lap alján.

Ha első alkalommal engedélyezi az automatikus biztonsági mentés, Azure konfigurálja az SQL Server IaaS Agent a háttérben. Ez idő alatt a Azure portál esetleg nem jelenik meg, hogy van-e beállítva az automatikus biztonsági mentés. Várjon néhány percet, az ügynök van telepítve, a beállítva. Ezt követően az Azure portál tükrözni fogja az új beállítások.

>[AZURE.NOTE] Az automatikus biztonsági mentés sablon használatával is beállítható. További tudnivalókért lásd: [az automatikus biztonsági mentés Azure quickstart útmutató sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>A PowerShell konfigurálása

Az SQL-virtuális-kiépítése után a PowerShell használatával állítsa be az automatikus biztonsági mentés.

Az alábbi PowerShell példa az automatikus biztonsági mentés egy meglévő SQL Server 2014-es virtuális van beállítva. A **AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig** parancs konfigurálja a virtuális gép társított Azure tárolási fiók biztonsági másolatok tárolására szolgáló automatikus biztonsági mentés beállításait. Ezek a biztonsági másolatok 10 napig megmarad. A **Set-AzureRmVMSqlServerExtension** parancs a megadott Azure virtuális frissíti, ezekkel a beállításokkal.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Telepítse és állítsa be az SQL Server IaaS Agent néhány percet is eltelhet.

Ahhoz, hogy a titkosítási, módosítsa az előző parancsfájl adják át a **EnableEncryption** paraméter együtt a **CertificatePassword** paraméter jelszó (biztonságos karakterlánc). A következő parancsfájl lehetővé teszi, hogy az automatikus biztonsági mentés beállításait az előző példában, és hozzáadja a titkosítást.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Az automatikus biztonsági mentés letiltása, futtassa a azonos nélkül a **-engedélyezése** **AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig** parancs paraméter. Hiányában a **-engedélyezése** paraméter jelzi a parancsot a szolgáltatás letiltásához. Telepítés után az eltarthat néhány percig, amíg az automatikus biztonsági mentés letiltása.

>[AZURE.NOTE] Az SQL Server IaaS Agent eltávolítása törli a korábban beállított automatikus biztonsági mentés beállításait. Az automatikus biztonsági mentés előtt letiltása és eltávolítása az SQL Server IaaS Agent tiltsa le.

## <a name="next-steps"></a>Következő lépések

Az automatikus biztonsági mentés felügyelt biztonsági másolat Azure VMs állítja be. Ezért fontos, [tekintse át a dokumentáció felügyelt biztonsági másolatának](https://msdn.microsoft.com/library/dn449496.aspx) a működését és a következmények megértéséhez.

Kereshet további biztonsági mentése és visszaállítása az SQL Server Azure VMs útmutatást a következő témakörben: [biztonsági mentése és visszaállítása az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-backup-recovery.md).

Más elérhető automatizálási tevékenységekkel kapcsolatos további tudnivalókért lásd [Az SQL Server IaaS ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

Futó SQL Server Azure VMs kapcsolatos további tudnivalókért olvassa el a [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.
