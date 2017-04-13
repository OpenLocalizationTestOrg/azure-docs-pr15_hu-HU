<properties
    pageTitle="Az automatikus javítási az SQL Server VMs (erőforrás-kezelő) |} Microsoft Azure"
    description="Az automatikus javítási funkció az SQL Server virtuális gépeken futó operációs rendszert futtató, az erőforrás-kezelővel Azure ismerteti."
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
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Az automatikus javítási az SQL Server Azure virtuális gépeken futó (erőforrás-kezelő)

> [AZURE.SELECTOR]
- [Erőforrás-kezelő](virtual-machines-windows-sql-automated-patching.md)
- [Klasszikus](virtual-machines-windows-classic-sql-automated-patching.md)

Az automatikus javítás karbantartási ablak az egy Azure virtuális gépen futó SQL Server hoz létre. Az automatikus frissítések karbantartási ablak során csak telepíthető. Az SQL Server a rescriction biztosítja, hogy a rendszer frissítéseket és bármely társított újraindul fordulhat elő, időben legjobb lehetőség az adatbázis. Az automatikus javítás attól függ, hogy az [SQL Server IaaS ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell. Ez a cikk klasszikus verziójának megtekintése, olvassa el [Az SQL Server Azure virtuális gépeken futó klasszikus az automatikus javítási](virtual-machines-windows-classic-sql-automated-patching.md).

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

- Ha azt tervezi, hogy állítsa be az automatikus javítási PowerShell, a [telepítse a legújabb Azure PowerShell-parancsait](../powershell-install-configure.md) .

>[AZURE.NOTE] Az automatikus javítás az SQL Server IaaS ügynök bővítmény támaszkodik. Aktuális SQL virtuális gépek galéria képek alapértelmezés szerint adja hozzá a bővítmény. További tudnivalókért lásd: az [SQL Server IaaS ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások

Az alábbi táblázat ismerteti a állítható be az automatikus javítási beállításokat. Tényleges beállítási lépések attól függően, hogy használja-e az Azure portálja vagy az Azure Windows PowerShell-parancsait változnak.

|A beállítás|A lehetséges értékek|Leírás|
|---|---|---|
|**Az automatikus javítás**|Engedélyezhetik/letilthatják (korlátozott)|Engedélyezi vagy tiltja az automatikus javítási az Azure virtuális gép.|
|**Karbantartási ütemezése**|Hétköznapi, hétfő, kedd, szerda, csütörtök, péntek, szombat, vasárnap|A virtuális gép Windows, az SQL Server és a Microsoft frissítések letöltése és telepítése az ütemezésben.|
|**Karbantartási kezdő óra**|0-24|A helyi elindítása a virtuális gép frissítéséhez.|
|**Karbantartási ablak időtartam**|30-180|A percek számát engedélyezett a letöltés és frissítések telepítésének befejezéséhez.|
|**Javítás kategória**|Fontos|A frissítések letöltéséhez és telepítéséhez kategóriára.|

## <a name="configuration-in-the-portal"></a>A portál konfigurálása
Az Azure portal segítségével konfigurálása az automatikus javítás során, hogy kiépítési vagy meglévő VMs.

### <a name="new-vms"></a>Új VMs
Az Azure portal segítségével konfigurálása az automatikus javítási az erőforrás-kezelő telepítési modell egy új SQL Server virtuális gép létrehozásakor.

Az **SQL Server-beállítások** lap válassza **az automatikus javítási**. A következő Azure portál képernyőképen az **SQL automatikus javítási** lap.

![Az SQL Azure-portálon automatikus javítása](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Környezetben témakört teljes a [kiépítési virtuális gép SQL Server Azure-ban](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Meglévő VMs
Meglévő SQL Server virtuális gépeken futó jelölje be az SQL Server virtuális gépet. Válassza a **Beállítások** lap a **SQL Server konfigurálása** című szakaszát.

![SQL az automatikus javítási a meglévő VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Az automatikus javítási szakaszban az **SQL Server konfigurációs** lap a **Szerkesztés** gombra.

![Meglévő VMs SQL automatikus javítási beállítása](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Amikor befejezte, kattintson az **OK** gombra a módosítások mentéséhez az **SQL Server konfigurálása** a lap alján.

Ha engedélyezi az automatikus javítási először, Azure konfigurálja az SQL Server IaaS Agent a háttérben. Ez idő alatt a Azure portál esetleg nem jelenik meg, hogy az automatikus javítási van beállítva. Várjon néhány percet, az ügynök van telepítve, a beállítva. Ezt követően az Azure portál tükrözi, az új beállítások.

>[AZURE.NOTE] Az automatikus javítási sablon használatával is beállítható. További tudnivalókért lásd: [az automatikus javítási Azure quickstart útmutató sablont](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>A PowerShell konfigurálása

Az SQL-virtuális-kiépítése után a PowerShell használatával állítsa be az automatikus javítási.

A következő példában a PowerShell automatikus javítási tartományi egy meglévő SQL Server virtuális használják. A **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** parancs beállítja az automatikus frissítések karbantartási új ablakban.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Ez a példa alapján, az alábbi táblázat ismerteti a cél Azure virtuális gyakorlati hatása:

|Paraméter|Hatás|
|---|---|
|**DayOfWeek**|Javítások minden csütörtök telepítve van.|
|**MaintenanceWindowStartingHour**|Kezdőpont frissíti a 11:00 de.|
|**MaintenanceWindowsDuration**|Javítások 120 percen belül telepítve kell lennie. A kezdő időpont alapján, azok el kell végeznie szerint 1:00 pm.|
|**PatchCategory**|Ez a paraméter csak lehetséges értéke **fontos**.|

Telepítse és állítsa be az SQL Server IaaS Agent néhány percet is eltelhet.

Az automatikus javítási letiltásához futtassa a azonos nélkül a **-engedélyezése** a **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**paramétert. Hiányában a **-engedélyezése** paraméter jelzi a parancsot a szolgáltatás letiltásához.

## <a name="next-steps"></a>Következő lépések

Más elérhető automatizálási tevékenységekkel kapcsolatos további tudnivalókért lásd [Az SQL Server IaaS ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

Futó SQL Server Azure VMs kapcsolatos további tudnivalókért olvassa el a [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.
