<properties
    pageTitle="Egy VMs elérhetősége módosítható |} Microsoft Azure"
    description="Megtudhatja, hogy miként módosíthatja a virtuális gépeken futó Azure PowerShell és az erőforrás-kezelő telepítési modell beállítása elérhetőségét."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="drewm"/>



# <a name="change-the-availability-set-for-a-windows-vm"></a>A beállítása a Windows virtuális elérhetőségének módosítása

Az alábbi lépéseket ismertetik, hogyan lehet módosítható az elérhetőség egy virtuális Azure PowerShell használatával. Egy virtuális csak egy elérhetőségének beállítása után is hozzá. Azért, hogy a elérhetőségének módosítása, törlése és újbóli létrehozása a virtuális gépen kell. 

## <a name="change-the-availability-set-using-powershell"></a>A PowerShell használatával elérhetőségének módosítása

1. Rögzítés a virtuális módosítani kell az alábbi fontos adatokat tartalmazzák.

    A virtuális neve
    
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
 
    Virtuális mérete
    
    ```powershell
    $vm.HardwareProfile.VmSize
    ```

    Hálózati elsődleges hálózati felület és a választható hálózati kapcsolatok, ha vannak ilyenek a virtuális a
    
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```

    Operációs rendszer merevlemez-profil

    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```

    Minden adat lemezre lemezre profilok 
    
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```

    Virtuális bővítmények telepítése 
    
    ```powershell
    $vm.Extensions
    ```

2. Törölje a virtuális valamelyik lemezt vagy a hálózati kapcsolatok törlése nélkül.

    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```

3. Az elérhetőség, beállítása, ha még nem létezik létrehozása

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```

4. Hozza létre a virtuális használatával új elérhetősége megadása

    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 

5. Adatok lemezre és bővítmények hozzáadása. További tudnivalókért lásd: [Adatok lemezre csatolni virtuális](virtual-machines-windows-attach-disk-portal.md) és [Bővítmény konfigurációs minták](virtual-machines-windows-extensions-configuration-samples.md). A PowerShell alrendszerrel vagy Azure CLI virtuális adatok lemez és bővítmények bővíthető.

## <a name="example-script"></a>Példa parancsfájl

A következő parancsfájl tartalmaz egy példa a szükséges információk összegyűjtése, az eredeti virtuális törlése, és ezután újra létrehozni egy új elérhetőségének beállítása.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Következő lépések

Tárhely hozzáadása a virtuális egy további [adatok lemez](virtual-machines-windows-attach-disk-portal.md)hozzáadásával.

