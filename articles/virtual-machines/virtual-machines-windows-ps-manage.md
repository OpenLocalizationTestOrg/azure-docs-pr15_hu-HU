<properties
    pageTitle="Erőforrás-kezelő és a PowerShell használatával VMs kezelése |} Microsoft Azure"
    description="Virtuális gépeken futó Azure erőforrás-kezelő és a PowerShell használatával kezelheti."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Erőforrás-kezelő és a PowerShell használatával Azure virtuális gépeken futó kezelése

## <a name="install-azure-powershell"></a>Azure PowerShell telepítése
 
Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) információt az Azure PowerShell legújabb verzióját, jelölje ki az előfizetés, és bejelentkezés az a fiókjába.

## <a name="set-variables"></a>Változók beállítása

A cikk a minden parancs csak az erőforráscsoport, ahol a virtuális gép található nevét, és kezelheti a virtuális gép nevét. A csoport nevét, az erőforrás, amely tartalmazza a virtuális gép **$rgName** értékének cserélje. **$VmName** értékének cserélje a virtuális nevét. A változók létrehozása.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>A virtuális gép adatainak megjelenítése

A virtuális gép információk.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Ez a példa hasonló adja vissza:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>A virtuális gép leállítása

Állítsa le a virtuális működő géphez.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

A megerősítést kérő megkérdezi:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Adja meg az **Y** le szeretné állítani a virtuális gépen.

Néhány perc múlva adja vissza, ez a példa hasonló:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Indítsa el a virtuális gépen

Indítsa el a virtuális gép, ha le van állítva.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Néhány perc múlva adja vissza, ez a példa hasonló:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Ha meg szeretné indítani a virtuális géphez, amely már fut, a **Restart-AzureRmVM** használata leírt Tovább gombra.

## <a name="restart-a-virtual-machine"></a>Indítsa újra a virtuális gépen

Indítsa újra a virtuális működő géphez.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Ez a példa hasonló adja vissza:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>A virtuális gép törlése

A virtuális gép törlése.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Használhatja a **-hatályba** paramétere ugorja át a megerősítést kérő párbeszédpanelen.

Ha nem használja a - hatályba paraméter megkérdezi megerősítést kér:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Ez a példa hasonló adja vissza:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>A virtuális gép frissítése

Ebben a példában a virtuális gép méretének frissítése.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Ez a példa hasonló adja vissza:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Című témakörben [az Azure virtuális gépeken futó méretét](virtual-machines-windows-sizes.md) a rendelkezésre álló méretű virtuális géphez.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Adatok lemezen hozzáadása egy virtuális számítógépre

Ebben a példában a lemezen adatok hozzáadása egy meglévő virtuális gépen.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

A lemez fel nem inicializált. A lemez inicializálni, bejelentkezés, és a merevlemez-kezelési. Ha telepítette WinRM és a tanúsítvány rajta létrehozásakor, a távoli Powershellt a lemez inicializálni is használhatja. Egy egyéni parancsfájl kiterjesztést is használhatja: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

A parancsprogram lemezt inicializálni kód hasonló tartalmazhatják:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Következő lépések

Ha vannak a telepítéssel kapcsolatos problémák, akkor előfordulhat, hogy tekintse meg [Hibaelhárítás erőforrás csoport telepítések Azure Portal segítségével](../resource-manager-troubleshoot-deployments-portal.md)
