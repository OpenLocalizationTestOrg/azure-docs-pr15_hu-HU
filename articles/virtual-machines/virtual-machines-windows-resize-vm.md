<properties
    pageTitle="Méretezze át a Windows virtuális |} Microsoft Azure"
    description="Méretezze át a Windows Azure Powershell használata az erőforrás-kezelő telepítési modell létrehozott virtuális gép."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>A Windows virtuális átméretezése

Ez a cikk bemutatja, hogyan méretezheti át a Windows virtuális, az erőforrás-kezelő telepítési modell Azure Powershell használatával létrehozott.

Miután létrehozott egy virtuális gép (virtuális), méretezheti a virtuális felfelé vagy lefelé a virtuális méret módosításával. Egyes esetekben kell először felszabadítása a virtuális. Ez akkor fordulhat elő, ha az új méretet a hardver fürt, amelyen a virtuális jelenleg nem érhető el.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Nem található egy elérhetőségének beállítása a Windows virtuális átméretezése

1. A virtuális méretét, a hardver fürthöz a hol helyezkedik el a virtuális a rendelkezésre álló lista. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Ha a kívánt méretet megjelenik, méretezze át a virtuális a következő parancsokat. Ha a kívánt méretet nem szerepel a felsorolásban, ugorjon 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Ha a kívánt méretet nem szerepel, a következő parancsokat felszabadítása a virtuális, átméretezheti, és indítsa újra a virtuális.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] A virtuális rendelt dinamikus IP-címek felszabadítása a virtuális elengedi. Az operációs rendszer és az adatok lemezt nem érinti. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Az elérhetőség beállítása a Windows virtuális átméretezése

Az új méretet a virtuális egy elérhetőségének beállítása a hardver fürt a virtuális jelenleg szolgáltatója nem érhető el, ha minden VMs elérhetősége megadása kell méretezze át a virtuális felszabadítása kell. Is előfordulhat, hogy módosítania kell más VMs méretének megadása után egy virtuális mértékben lett átméretezve elérhetőségének. Az elérhetőség beállítása egy virtuális átméretezéséhez hajtsa végre az alábbi lépéseket.

1. A virtuális oldalméretei érhető el, a hol helyezkedik el a virtuális hardveres fürt sorolja fel.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Ha a kívánt méretet megjelenik, méretezze át a virtuális a következő parancsokat. Ha nem láthatók, ugorjon a 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Ha a kívánt méretet nem szerepel a felsorolásban, folytassa a elérhetőség megadása minden VMs felszabadítása, méretezze át a VMs és újra kell indítania őket az alábbi lépéseket.

4.  Elérhetőség megadása minden VMs leállítása.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Méretezze át, és indítsa újra a VMs elérhetősége megadása.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Következő lépések

- További méretezhetőség több virtuális példánya futtatható és méretezése. További tudnivalókért olvassa el a [virtuális gép skála megadása a Windows gépek automatikus méretezése](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)című témakört.



