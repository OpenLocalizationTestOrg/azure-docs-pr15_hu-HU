<properties
    pageTitle="Az SQL Server Agent SQL Server VMs (klasszikus) bővítményének |} Microsoft Azure"
    description="Ez a témakör ismerteti az adott SQL Server felügyeleti feladatok automatizálja SQL Server agent bővítmény kezelése. Ezek közé tartozik az automatikus biztonsági mentés, az automatikus javítási és Azure kulcs tárolóra integráció. Ez a témakör a klasszikus telepítési mód használja."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>Az SQL Server Agent bővítményének SQL Server VMs (klasszikus)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasszikus](virtual-machines-windows-classic-sql-server-agent-extension.md)

Felügyeleti feladatok automatizálásához Azure virtuális gépeken futó SQL Server IaaS ügynök bővítmény (SQLIaaSAgent) fut. Ez a témakör áttekintést nyújt a szolgáltatások, a bővítmény, valamint a telepítést, az állapot és eltávolítási utasításokat által támogatott.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ez a cikk az erőforrás-kezelő verziójának megtekintéséhez olvassa el az [SQL Server Agent bővítmény az SQL Server VMs erőforrás-kezelő](virtual-machines-windows-sql-server-agent-extension.md)című témakört.

## <a name="supported-services"></a>Támogatott szolgáltatások

Az SQL Server IaaS ügynök kiterjesztése a következő felügyeleti feladatok támogatja:

| A szolgáltatás felügyelete | Leírás |
|---------------------|-------------------------------|
| **Az automatikus biztonsági mentés SQL** | Automatizálja a biztonsági másolatok adatbázisokra ütemezését a virtuális SQL Server alapértelmezett példányának. További tudnivalókért lásd: [az automatikus biztonsági mentés az SQL Server az Azure virtuális gépeken futó (klasszikus)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL automatikus javítása** | Konfigurálja a Karbantartás ablakban alatt, amely a virtuális frissítések is megtörténnek, így elkerülheti a frissítéseket a terhelést a csúcs időszakokban. További tudnivalókért lásd: [az automatikus javítási az SQL Server az Azure virtuális gépeken futó (klasszikus)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Azure kulcs tárolóra-integráció** | Lehetővé teszi, hogy automatikusan telepítése és konfigurálása az SQL Server virtuális Azure kulcs tárolóból elemre. További tudnivalókért lásd: [Konfigurálása Azure kulcs tárolóból elemre integráció az SQL Server Azure VMs (klasszikus)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Előfeltételek

Az SQL Server IaaS ügynök bővítmény használatáról a virtuális követelmények:

### <a name="operating-system"></a>Operációs rendszer:

- A Windows Server 2012
- A Windows Server 2012 R2

### <a name="sql-server-versions"></a>Az SQL Server-verziója:

- SQL Server 2012
- Az SQL Server 2014-es
- Az SQL Server 2016-ban

### <a name="azure-powershell"></a>Azure PowerShell:

[Töltse le és állítsa be a legújabb Azure PowerShell-parancsait](../powershell-install-configure.md).

Indítsa el a Windows Powershellt, és csatlakoztassa a **Hozzáadás-AzureAccount** paranccsal Azure előfizetését.

    Add-AzureAccount

Ha több előfizetéssel rendelkezik, segítségével a **Select-AzureSubscription** válassza azt az előfizetést, amely tartalmazza a cél klasszikus virtuális.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Ezen a ponton a klasszikus virtuális gépeken futó és a **Get-AzureVM** paranccsal társított szolgáltatás nevük elérheti.

    Get-AzureVM

## <a name="installation"></a>Telepítés

A klasszikus VMs PowerShell kell használnia, az SQL Server IaaS ügynök bővítmény telepítése és beállítása a hozzá társított szolgáltatásokat. A **Set-AzureVMSqlServerExtension** PowerShell-parancsmag használatával a bővítmény telepítése. A következő parancs például a bővítmény telepítése a Windows Server virtuális (klasszikus), és "SQLIaaSExtension" névvel.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Ha az SQL IaaS ügynök bővítmény legújabb verziójára frissít, a bővítmény frissítése után indítsa újra a virtuális gép.

>[AZURE.NOTE] Klasszikus virtuális gépeken futó nincs szolgáló lehetőség telepítse és állítsa be az SQL IaaS ügynök kiterjesztése a portálon keresztül.

## <a name="status"></a>Állapot

Ellenőrizze, hogy a bővítmény telepítve van egy úgy, hogy a ügynök állapotának megtekintése az Azure-portálon. Jelölje ki a virtuális gép lap **minden elérhető beállítás** , és válassza a **bővítmények**. Meg kell jelennie a **SQLIaaSAgent** bővítmény szerepel a listában.

![Az SQL Server IaaS ügynök bővítmény az Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

A **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmag is használhatja.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Eltávolítása   

Az Azure-portálon távolítsa el a bővítmény a három pontra, kattintson a **bővítmények** lap a virtuális gép tulajdonságai gombra kattintva. Ezután kattintson a **Törlés**parancsra.

![Távolítsa el az SQL Server IaaS ügynök bővítmény az Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Az **Eltávolítás-AzureVMSqlServerExtension** Powershell-parancsmag is használhatja.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Következő lépések

A bővítmény által támogatott szolgáltatások közül használatának megkezdéséhez. További információ a Ez a cikk a [támogatott szolgáltatások](#supported-services) részben hivatkozott témakörökben.

A Azure virtuális gépeken futó SQL Server rendszerrel kapcsolatos további tudnivalókért olvassa el a [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.
