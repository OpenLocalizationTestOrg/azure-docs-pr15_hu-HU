<properties
    pageTitle="Egy virtuális gép skála megadása VMs kezelése |} Microsoft Azure"
    description="Az Azure PowerShell használatával virtuális gép skálával virtuális gépeken futó kezelése."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>A virtuális gép skála megadása a virtuális gépeken futó kezelése

Ez a cikk a feladatok használatával kezelheti a virtuális gép skála megadása a virtuális gépeken futó.

A feladatok, amelyeket egy skála megadása a virtuális gép kezelése járnak a legtöbb szükség az, hogy tudja, hogy a gép használni kívánt példány azonosítója. [Azure erőforrás Explorer](https://resources.azure.com) segítségével virtuális gép példány azonosítója keresse meg a skála megadása. Erőforrás Explorer befejezése feladatokat állapotának ellenőrzéséhez is használhatja.

Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) információt az Azure PowerShell legújabb verzióját, jelölje ki az előfizetés, és bejelentkezés az a fiókjába.

## <a name="display-information-about-a-scale-set"></a>Információk jeleníthetők meg róluk egy skála megadása

Méretezés meg, amely a példány nézetben is nevezik általános információkat kaphat. Másik lehetőségként kaphat részletesebb információt, például a skála megadása az erőforrások adatait.

Jegyzett értékeket cserélje ki a nevét vagy a erőforráscsoport és skála megadása és parancsot:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Eredménye alábbihoz hasonló:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Az ajánlat értékek cseréje a erőforrás csoport és a skála megadása a nevet. Csere *#* a virtuális gép kívánt információ jelenik meg, és futtassa a példány azonosítójú:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Ez a példa hasonló adja vissza:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Indítsa el a virtuális gép egy skála megadása

Az ajánlat értékek cseréje a erőforrás csoport és a skála megadása a nevet. Csere *#* a virtuális gép indítása, majd futtatásával kívánt azonosítójával:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Az erőforrás Explorerben azt láthatja, hogy a példány állapota **fut**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

A virtuális gépeken futó is elindíthatók a méretezés - InstanceId paraméter nem használatával.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>A skála megadása a virtuális gép leállítása

Az ajánlat értékek cseréje a erőforrás csoport és a skála megadása a nevet. Csere *#* a virtuális gép, hogy meg szeretné szüntetni, és futtassa az azonosítóval:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Az erőforrás Explorerben azt láthatja, hogy a példány állapota **felszabadítása**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
A virtuális gép leállításához, és nem felszabadítása azt, - StayProvisioned paraméter használatával. Az összes a virtuális gépeken futó megadása leállíthatja a nem a - InstanceId paraméter használatával.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Indítsa újra a virtuális gép egy skála megadása

A jegyzett értékek cseréje a erőforráscsoport és skála megadása a nevet. Csere *#* a virtuális gép, indítsa újra a, majd futtatásával kívánt azonosítójával:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
A virtuális gépeken futó megadása a - InstanceId paraméter nem használatával indítani.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>A virtuális gép eltávolítása egy skála megadása

A jegyzett értékek cseréje a erőforráscsoport és skála megadása a nevet. Csere *#* a virtuális gép eltávolítása, és hajtsa végre a kívánt azonosítójával:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Egyszerre virtuális gép skála megadása a - InstanceId paraméter nem használatával eltávolíthatja.

## <a name="change-the-capacity-of-a-scale-set"></a>A méretezés beállítása kapacitása módosítása

Felvehet, és távolítsa el a virtuális gépeken futó megadása kapacitásával módosításával. Ismerkedés a skála megadása, módosítása, a kapacitás meg a kívánt, hogy legyen, és frissítse a skála megadása a új kapacitással. Ezek a parancsok cserélje ki a nevét az erőforráscsoport és skála megadása jegyzett értékeket.

  $vmss = get-AzureRmVmss - ResourceGroupName "erőforrás csoport neve" - VMScaleSetName "méretarányra beállított neve" $vmss.sku.capacity = 5 frissítés-AzureRmVmss - ResourceGroupName "erőforrás csoport neve"-"méretezni a set name" nevet - VirtualMachineScaleSet $vmss 

Ha el szeretne távolítani virtuális gépeken futó skála megadása a, a legmagasabb azonosítókat össze a virtuális gépeken futó először törlődnek.
