<properties
    pageTitle="Az erőforrás-kezelő PowerShell áttelepítése |} Microsoft Azure"
    description="Az alábbiakban ismertetjük IaaS erőforrások platform támogatott áttelepítése a klasszikus az Azure erőforrás-kezelő Azure PowerShell-parancsokkal"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Áttelepítése IaaS erőforrások klasszikus az Azure erőforrás-kezelő Azure PowerShell használatával

Ezeket a lépéseket az Azure PowerShell-parancsok használata erőforrásként egy szolgáltatást (IaaS) a klasszikus telepítési modellből Azure erőforrás-kezelő telepítési adatmodellhez infrastruktúra áttelepítendő megjelenítése. 

Ha azt szeretné, az [Azure parancs sor kezelőfelületről Azure](virtual-machines-linux-cli-migration-classic-resource-manager.md)áttelepítheti erőforrásokat.

- A támogatott áttelepítési forgatókönyvek hátteréről [Klasszikus az Azure erőforrás-kezelő IaaS erőforrásainak áttelepítése Platform támogatott](virtual-machines-windows-migration-classic-resource-manager.md). 
- Részletes útmutatást és áttelepítési útmutató című témakörben talál [a platform támogatott áttérés az Azure erőforrás-kezelő klasszikus technikai mély merülési](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Lépés: 1: Áttelepítési megtervezése

Íme néhány gyakorlati tanácsokat, amely szerint felmérése a áttelepítése IaaS erőforrások klasszikus az erőforrás-kezelő javasoljuk:

- Olvassa el a a [támogatott és a nem támogatott funkciókat és beállításokat](virtual-machines-windows-migration-classic-resource-manager.md). Ha virtuális gépeken futó, amelyek nem támogatott konfigurációk vagy funkcióit, azt javasoljuk, hogy várja meg a beállítások/szolgáltatás támogatása jelenti be. Azt is megteheti Ha azt igényeinek, távolítsa el ezt a szolgáltatást, vagy áttelepítési ahhoz, hogy a konfigurációs kiveszi.
-   Ha a infrastruktúra és az alkalmazások terjesztése ma parancsprogramok rendelkezik automatikus, próbálja meg hozzon létre egy hasonló próba-beállítást a parancsfájlok áttelepítésre használatával. Azt is megteheti akkor beállíthatja minta környezetekben a Azure portál használatával.

>[AZURE.IMPORTANT]Virtuális hálózati átjárók (-webhelyek, Azure készült ExpressRoute, alkalmazás átjárót, a webhely-pont) jelenleg nem támogatottak az erőforrás-kezelő klasszikus az áttelepítésre. Az átjárók klasszikus virtuális hálózat áttelepítésének, távolítsa el az átjáró a hálózati (futtathatja az előkészítés lépésben az átjáró törlése nélkül) áthelyezése végrehajtási művelet futtatása előtt. Miután az áttelepítés, újra az átjáró az Azure erőforrás-kezelő.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Lépés: 2: Azure PowerShell legújabb verziójának telepítéséhez.

Kétféleképpen lehet fő Azure PowerShell telepítésének: [PowerShell-gyűjteménye](https://www.powershellgallery.com/profiles/azure-sdk/) vagy a [Webes Platform telepítő (WebPI)](http://aka.ms/webpi-azps). WebPI kap havi frissítéseket. PowerShell-gyűjtemény a folyamatos frissítések kap. Ez a cikk Azure PowerShell verzió 2.1.0 alapul.

A telepítési utasításokat megtudhatja, [hogy miként telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>3 lépés: Az előfizetés beállítása és a Regisztrálás az áttelepítés

Először egy PowerShell-parancssorában. Áttelepítés esetén, be kell állítania a környezet két klasszikus és erőforrás-kezelő.

Bejelentkezés a fiókjába az erőforrás-kezelő modell.

```powershell
    Login-AzureRmAccount
```

A rendelkezésre álló előfizetéseket kaphat a következő parancs használatával:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Az aktuális munkamenethez az Azure előfizetés beállítása. Ez a példa az alapértelmezett előfizetés neve **Saját Azure előfizetés**állítja be. Cserélje ki az előfizetés példanév saját. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Regisztráció egyszeri lépés, de egyszer az áttelepítés előtt kell tennie. Anélkül, hogy regisztrált, az alábbi hibaüzenet jelenik meg: 

>   *BadRequest: Előfizetési nem regisztrált az áttelepítésre.* 

A következő parancs segítségével regisztrálhatja az áttelepítési erőforrás szolgáltatónál:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Várjon öt percig, amíg a bejegyzés befejezéséhez. A jóváhagyási állapotának ellenőrizheti a következő parancs használatával:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Győződjön meg róla, hogy RegistrationState `Registered` folytatás előtt. 

Jelentkezzen be a fiókjába a Klasszikus modell.

```powershell
    Add-AzureAccount
```

A rendelkezésre álló előfizetéseket kaphat a következő parancs használatával:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Az aktuális munkamenethez az Azure előfizetés beállítása. Ez a példa az alapértelmezett előfizetés **Saját Azure előfizetés**állítja be. Cserélje ki az előfizetés példanév saját. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Lépés: 4: Győződjön meg arról, hogy elég Azure erőforrás-kezelő virtuális gép magmintákat, az aktuális környezetben vagy VNET Azure régióban

Az alábbi PowerShell-parancs használatával jelölje be az Azure erőforrás-kezelő van magmintákat aktuális számát. Core kvóták kapcsolatos további információért olvassa el a [korlátai és az Azure erőforrás-kezelő](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)című témakört. 

Ebben a példában a **Nyugati USA -beli** tartományban lévő elérhető ellenőrzi. Cserélje a régió példanév saját. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Lépés 5: Parancsokat a IaaS erőforrások áttelepítése

>[AZURE.NOTE] Az alábbiakban ismertetjük az összes művelet idempotent. Ha a probléma nem nem támogatott funkciók vagy konfigurációs hiba, azt javasoljuk, hogy meg újra az előkészítés, megszakítása vagy művelet végrehajtása. A platform majd próbálja újra a műveletet.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>(Nem a virtuális hálózat) egy felhőszolgáltatásában virtuális gépeken futó áttelepítése

Ismerkedés a felhőalapú szolgáltatások listáját a következő parancs használatával, és válassza ki az áttelepíteni kívánt felhőszolgáltatásba. Ha a VMs a felhőszolgáltatásában egy virtuális hálózaton, vagy ha webes vagy dolgozó szerepkörök, a parancs hibaüzenetet ad vissza.

```powershell
    Get-AzureService | ft Servicename
```

Ismerkedés a felhőbeli szolgáltatástól telepítési nevét. Ebben a példában a szolgáltatás neve **Saját szolgáltatás**. A szolgáltatás példanév cserélje le a saját szolgáltatás neve. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Felkészülés a virtuális gépeken futó a felhőszolgáltatásában az áttelepítésre. Ha két lehetőség közül választhat.

* **1 lehetőséget. A VMs áttelepítése platform létre virtuális hálózati**

    Első lépésként ellenőrzése, ha telepítheti át a felhőalapú szolgáltatást használ, az alábbi parancsokat:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Az előző parancs bármely figyelmeztetések és a veszi górcső áttelepítési hibák jeleníti meg. Ha érvényességi sikeres, majd az **Előkészítés** lépésre áthelyezhetők:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **2 lehetőséget. Az erőforrás-kezelő telepítési modellben meglévő virtuális hálózat áttelepítése**

    Ez a példa **myResourceGroup**, **myVirtualNetwork** a virtuális hálózat nevét, és a **mySubNet**alhálózat név állítja be az erőforrás-csoport nevét. Cserélje ki a neveket a példában a saját erőforrásnevek.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Első lépésként ellenőrzése, ha telepítheti át a felhőalapú szolgáltatást használ, az alábbi parancsot:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Az előző parancs bármely figyelmeztetések és a veszi górcső áttelepítési hibák jeleníti meg. Ha érvényességi sikeres, majd folytatható a a következő előkészítés lépést:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Miután az előkészítés művelet a fenti lehetőségek valamelyikével létrejött, a lekérdezés a VMs áttelepítés állapota. Győződjön meg arról, hogy vannak-e a a `Prepared` állapota.

Ebben a példában a virtuális neve **myVM**állítja be. A példanév cserélje le a saját virtuális nevére.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Jelölje be az adatokat a előkészített erőforrások PowerShell vagy az Azure portal segítségével. Ha nem az áttelepítésre, és azt szeretné, ha szeretne visszatérni a régi állapot, használja az alábbi parancsot:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Ha az elkészített konfiguráció elégedett az eredménnyel, előrelépésre, és az erőforrások véglegesítse a következő parancs használatával:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Virtuális gépeken futó virtuális hálózat áttelepítése

A hálózat áttelepítése virtuális gépeken futó virtuális hálózatban áttelepítéséhez. A virtuális gépeken futó automatikusan a hálózat áttelepítése. Válassza ki a virtuális hálózat áttelepítése kívánt. 

Ebben a példában a virtuális hálózat neve **myVnet**állítja be. Cserélje ki a virtuális hálózati példanév saját. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Ha a virtuális hálózat nem támogatott beállításokat tartalmazó webhelyen vagy dolgozó szerepkörök, illetve VMs tartalmaz, egy adatérvényesítési hibaüzenetet kapja.

Első lépésként ellenőrzése, ha a következő parancs segítségével áttelepítheti a virtuális hálózat:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Az előző parancs bármely figyelmeztetések és a veszi górcső áttelepítési hibák jeleníti meg. Ha érvényességi sikeres, majd folytatható a a következő előkészítés lépést:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Jelölje be az adatokat az elkészített virtuális gépeken futó Azure PowerShell vagy az Azure portal segítségével. Ha nem az áttelepítésre, és azt szeretné, ha szeretne visszatérni a régi állapot, használja az alábbi parancsot:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Ha az elkészített konfiguráció elégedett az eredménnyel, előrelépésre, és az erőforrások véglegesítse a következő parancs használatával:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>A tároló fiók áttelepítése

Amikor elkészült a virtuális gépeken futó áttelepítette, azt javasoljuk, a tárhely fiókok áttelepítése.

Készítsen minden tárterület-fiók áttelepítése a következő parancs használatával. Ebben a példában a tárhely fióknév **myStorageAccount**. A példanév lecserélése saját tároló fiók nevére. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Jelölje be az adatokat az elkészített tároló fiók Azure PowerShell vagy az Azure portal segítségével. Ha nem az áttelepítésre, és azt szeretné, ha szeretne visszatérni a régi állapot, használja az alábbi parancsot:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Ha az elkészített konfiguráció elégedett az eredménnyel, előrelépésre, és az erőforrások véglegesítse a következő parancs használatával:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Következő lépések

- Az áttelepítési kapcsolatos további tudnivalókért olvassa el a [Platform támogatott áttelepítését a klasszikus az Azure erőforrás-kezelő IaaS erőforrások](virtual-machines-windows-migration-classic-resource-manager.md).
- Az erőforrás-kezelő Azure PowerShell használatával további hálózati erőforrások áttelepítéséhez eljárással hasonló [Áthelyezés-AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Áthelyezés-AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)és [Áthelyezés-AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
- Megnyitás-forrás parancsfájlok áttelepítése Azure erőforrások klasszikus erőforrás-kezelő is használhatja olvassa el a [közösségi eszközök való áttelepítés Azure erőforrás-kezelő áttelepítésre](virtual-machines-windows-migration-scripts.md) című témakört.
