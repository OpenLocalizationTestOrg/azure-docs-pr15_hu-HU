<properties
    pageTitle="Készítsen egy virtuális képet az általános Azure virtuális |} Microsoft Azure"
    description="Megtudhatja, hogy miként rögzítheti egy virtuális képet egy általános, az erőforrás-kezelő telepítési modell készült Azure virtuális"
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
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Hogyan rögzítheti egy általános Azure virtuális virtuális képként


Ez a cikk bemutatja, hogyan Azure PowerShell használatával hozzon létre egy általános Azure virtuális képét. A kép használatával hozzon létre egy másik virtuális majd. A kép a OS lemez és a virtuális géphez csatlakoztatott adatok lemez tartalmazza. A kép nem tartalmazza a virtuális hálózati erőforrásokat, így Önnek kell a új virtuális létrehozásakor, ezek az erőforrások beállítása. 


## <a name="prerequisites"></a>Előfeltételek

- [A virtuális általános](virtual-machines-windows-generalize-vhd.md)már van szükség. Egy virtuális generalizing eltávolítja az összes a személyes fiók adatait, többek között, és a gép képként használandó előkészíti.

- Azure PowerShell verziója van szükség 1.0.x vagy újabb telepítve. Ha még nem telepítette a PowerShell, olvassa el a [hogyan telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md) telepítési lépéseket.


## <a name="log-in-to-azure-powershell"></a>Bejelentkezés az Azure PowerShell

1. Nyissa meg az Azure PowerShell, és jelentkezzen be az Azure-fiókjába.

    ```powershell
    Login-AzureRmAccount
    ```

    Előugró ablak megnyílik a adhatja meg a Azure-fiók hitelesítő adatait.

2. Az előfizetés azonosítók beszerzése a rendelkezésre álló előfizetések.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Állítsa a megfelelő előfizetés használata az előfizetéshez azonosítójával.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>A virtuális felszabadítása, és adja meg az általános állam       

1. A virtuális erőforrások felszabadítani.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    Leállítva **(felszabadítása)** **Leállítva** módosítja a *állapota* a virtuális az Azure-portálon.

2. Állítsa a virtuális gép állapotának **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. A virtuális állapotának ellenőrzése. A virtuális **OSState/általános** szakasz **DisplayStatus** **virtuális általános**meg kell rendelkeznie.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>A kép elkészítéséhez 

1. A virtuális gép kép másolása a cél tárolási tároló ezzel a paranccsal. A kép ugyanazt a tárhely fiókot, mint az eredeti virtuális gép jön létre. A `-Path` paraméter a JSON-sablon helyi másolatát menti. A `-DestinationContainerName` paraméter tartsa nyomva a kívánt képet, amelyet a tároló nevét. Ha a tároló nem létezik, akkor létrejön.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    A JSON-fájl sablonból elérheti a kép URL-CÍMÉT. Nyissa meg az **erőforrások** > **storageProfile** > **osDisk** > **kép** > **uri** szakaszában a teljes elérési útját a képet. A kép URL-címe a következőképpen néz: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Is ellenőrizheti az URI a portálon. A **rendszer** a tárterület-fiókjában nevű tároló másolja a képet. 


## <a name="next-steps"></a>Következő lépések

- Most már hozhat [létre egy virtuális a képből](virtual-machines-windows-create-vm-generalized.md).

