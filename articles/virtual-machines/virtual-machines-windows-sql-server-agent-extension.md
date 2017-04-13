<properties
    pageTitle="Az SQL Server Agent SQL Server VMs (erőforrás-kezelő) bővítményének |} Microsoft Azure"
    description="Ez a témakör ismerteti az adott SQL Server felügyeleti feladatok automatizálja SQL Server agent bővítmény kezelése. Ezek közé tartozik az automatikus biztonsági mentés, az automatikus javítási és Azure kulcs tárolóra integráció. Ez a témakör az erőforrás-kezelő telepítési mód használja."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>Az SQL Server Agent bővítményének SQL Server VMs (erőforrás-kezelő)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasszikus](virtual-machines-windows-classic-sql-server-agent-extension.md)

Felügyeleti feladatok automatizálásához Azure virtuális gépeken futó SQL Server IaaS ügynök bővítmény (SQLIaaSExtension) fut. Ez a témakör áttekintést nyújt a szolgáltatások, a bővítmény, valamint a telepítést, az állapot és eltávolítási utasításokat által támogatott.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell. Ez a cikk klasszikus verziójának megtekintése, olvassa el az [SQL Server Agent bővítményének SQL Server VMs klasszikus](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Támogatott szolgáltatások

Az SQL Server IaaS ügynök kiterjesztése a következő felügyeleti feladatok támogatja:

| A szolgáltatás felügyelete | Leírás |
|---------------------|-------------------------------|
| **Az automatikus biztonsági mentés SQL** | Automatizálja a biztonsági másolatok adatbázisokra ütemezését a virtuális SQL Server alapértelmezett példányának. További tudnivalókért lásd: [az automatikus biztonsági mentés az SQL Server az Azure virtuális gépeken futó (erőforrás-kezelő)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL automatikus javítása** | Konfigurálja a Karbantartás ablakban alatt, amely a virtuális frissítések is megtörténnek, így elkerülheti a frissítéseket a terhelést a csúcs időszakokban. További tudnivalókért lásd: [az automatikus javítási az SQL Server az Azure virtuális gépeken futó (erőforrás-kezelő)](virtual-machines-windows-sql-automated-patching.md).|
| **Azure kulcs tárolóra-integráció** | Lehetővé teszi, hogy automatikusan telepítése és konfigurálása az SQL Server virtuális Azure kulcs tárolóból elemre. További tudnivalókért lásd: [Konfigurálása Azure kulcs tárolóból elemre integráció az SQL Server Azure VMs (erőforrás-kezelő)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Előfeltételek

Az SQL Server IaaS ügynök bővítmény használatáról a virtuális követelmények:

**Operációs rendszer**:

- A Windows Server 2012
- A Windows Server 2012 R2

**Az SQL Server-verziója**:

- SQL Server 2012
- Az SQL Server 2014-es
- Az SQL Server 2016-ban

**Azure PowerShell**:

- [Töltse le és állítsa be a legújabb Azure PowerShell-parancsait](../powershell-install-configure.md)

## <a name="installation"></a>Telepítés

Az SQL Server IaaS ügynök bővítmény automatikusan települ, amikor Ön kiépítése az SQL Server virtuális gép gyűjtemény képek közül.

Ha-OS csak a Windows Server virtuális gép hoz létre, telepítheti a bővítmény manuálisan a **Set-AzureVMSqlServerExtension** PowerShell-parancsmag használatával. A következő parancs például-OS csak a Windows Server virtuális a bővítmény telepítése és névvel "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Ha az SQL IaaS ügynök bővítmény legújabb verziójára frissít, a bővítmény frissítése után indítsa újra a virtuális gép.

>[AZURE.NOTE] Ha telepíti az SQL Server IaaS ügynök bővítmény manuálisan a Windows Server virtuális, használja, és kezelése a PowerShell-parancsokkal szolgáltatását. A portál felület csak az SQL Server-dokumentumtárban képek érhető el.

## <a name="status"></a>Állapot

Ellenőrizze, hogy a bővítmény telepítve van egy úgy, hogy a ügynök állapotának megtekintése az Azure-portálon. Jelölje ki a virtuális gép lap **minden elérhető beállítás** , és válassza a **bővítmények**. Meg kell jelennie a **SQLIaaSExtension** bővítmény szerepel a listában.

![Az SQL Server IaaS ügynök bővítmény az Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

A **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmag is használhatja.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Az előző parancs a ügynök telepítve van, és általános állapotadatok megerősíti. A következő parancsok végrehajtása az automatikus biztonsági mentés és javítás adott állapotinformációkat is megnyithatja.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Eltávolítása   

Az Azure-portálon távolítsa el a bővítmény a három pontra, kattintson a **bővítmények** lap a virtuális gép tulajdonságai gombra kattintva. Ezután kattintson a **Törlés**parancsra.

![Távolítsa el az SQL Server IaaS ügynök bővítmény az Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Az **Eltávolítás-AzureRmVMSqlServerExtension** Powershell-parancsmag is használhatja.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Következő lépések

A bővítmény által támogatott szolgáltatások közül használatának megkezdéséhez. További információ a Ez a cikk a [támogatott szolgáltatások](#supported-services) részben hivatkozott témakörökben.

A Azure virtuális gépeken futó SQL Server rendszerrel kapcsolatos további tudnivalókért olvassa el a [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.
