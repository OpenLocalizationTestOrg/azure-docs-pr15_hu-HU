<properties
    pageTitle="Az automatikus javítási az SQL Server VMs (klasszikus) |} Microsoft Azure"
    description="Ismerteti az SQL Server virtuális gépeken futó operációs rendszert futtató a klasszikus telepítési mód Azure-ban, az automatikus javítási funkció."
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

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Az automatikus javítási az SQL Server Azure virtuális gépeken futó (klasszikus)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-automated-patching.md)
- [Klasszikus](virtual-machines-windows-classic-sql-automated-patching.md)

Az automatikus javítás karbantartási ablak az egy Azure virtuális gépen futó SQL Server hoz létre. Az automatikus frissítések karbantartási ablak során csak telepíthető. Az SQL Server ezzel biztosíthatja, hogy a rendszer frissítéseket és bármely társított újraindul fordulhat elő, időben legjobb lehetőség az adatbázis. Az automatikus javítás attól függ, hogy az [SQL Server IaaS ügynök bővítmény](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ez a cikk az erőforrás-kezelő verziójának megtekintéséhez olvassa el [Az SQL Server Azure virtuális gépeken futó-kezelő az automatikus javítási](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Előfeltételek

Az automatikus javítási használatához vegye figyelembe az alábbi előfeltételek:

**Operációs rendszer**:

- A Windows Server 2012
- A Windows Server 2012 R2

**Az SQL Server-verzióra**:

- SQL Server 2012
- Az SQL Server 2014-es
- Az SQL Server 2016-ban

**Azure PowerShell**:

- [Telepítse a legújabb Azure PowerShell-parancsait](../powershell-install-configure.md).

**Az SQL Server IaaS bővítmény**:

- [Az SQL Server IaaS bővítmény telepítése](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások

Az alábbi táblázat ismerteti a állítható be az automatikus javítási beállításokat. A klasszikus VMs PowerShell kell használnia, a beállítások konfigurálása.

|A beállítás|A lehetséges értékek|Leírás|
|---|---|---|
|**Az automatikus javítás**|Engedélyezhetik/letilthatják (korlátozott)|Engedélyezi vagy tiltja az automatikus javítási az Azure virtuális gép.|
|**Karbantartási ütemezése**|Hétköznapi, hétfő, kedd, szerda, csütörtök, péntek, szombat, vasárnap|A virtuális gép Windows, az SQL Server és a Microsoft frissítések letöltése és telepítése az ütemezésben.|
|**Karbantartási kezdő óra**|0-24|A helyi elindítása a virtuális gép frissítéséhez.|
|**Karbantartási ablak időtartam**|30-180|A percek számát engedélyezett a letöltés és frissítések telepítésének befejezéséhez.|
|**Javítás kategória**|Fontos|A frissítések letöltéséhez és telepítéséhez kategóriára.|

## <a name="configuration-with-powershell"></a>A PowerShell konfigurálása

A következő példában a PowerShell automatikus javítási tartományi egy meglévő SQL Server virtuális használják. A **New-AzureVMSqlServerAutoPatchingConfig** parancs beállítja az automatikus frissítések karbantartási új ablakban.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Ez a példa alapján, az alábbi táblázat ismerteti a cél Azure virtuális gyakorlati hatása:

|Paraméter|Hatás|
|---|---|
|**DayOfWeek**|Javítások minden csütörtök telepítve van.|
|**MaintenanceWindowStartingHour**|Kezdőpont frissíti a 11:00 de.|
|**MaintenanceWindowsDuration**|Javítások 120 percen belül telepítve kell lennie. A kezdő időpont alapján, azok el kell végeznie szerint 1:00 pm.|
|**PatchCategory**|A csak lehetséges a paraméter értéke "Fontos".|

Telepítse és állítsa be az SQL Server IaaS Agent néhány percet is eltelhet.

Az automatikus javítási letiltásához futtatása azonos parancsfájl-a - engedélyezése az új AzureVMSqlServerAutoPatchingConfig paraméter nélkül. Telepítés után az eltarthat néhány percig, amíg az automatikus javítási letiltása.

## <a name="next-steps"></a>Következő lépések

Más elérhető automatizálási tevékenységekkel kapcsolatos további tudnivalókért lásd [Az SQL Server IaaS ügynök bővítmény](virtual-machines-windows-classic-sql-server-agent-extension.md).

Futó SQL Server Azure VMs kapcsolatos további tudnivalókért olvassa el a [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.
