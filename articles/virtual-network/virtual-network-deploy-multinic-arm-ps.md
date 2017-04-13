<properties
   pageTitle="Az erőforrás-kezelő PowerShell használatával több hálózati kártya VMs telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként telepítése több hálózati kártya VMs az erőforrás-kezelő PowerShell használatával"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-powershell"></a>A PowerShell használatával több hálózati kártya VMs terjesztése

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Jelenleg nem lehet VMs a egy egyetlen hálózati kártya és a több VMs elérhetősége mindazon. Ezért kell a hátsó vége kiszolgálók megvalósítását a különböző erőforráscsoport, minden összetevő-nál. Az alábbi lépésekkel *IaaSStory* nevű fő erőforráscsoport, illetve *IaaSStory-Kódmentes* vissza vége kiszolgálók erőforráscsoport használja.

## <a name="prerequisites"></a>Előfeltételek

A visszahívási vége kiszolgálókat alkalmazhat, mielőtt kell az összes szükséges erőforrások példánkban a fő erőforráscsoport üzembe. Ezek az erőforrások telepítéséhez kövesse az alábbi lépéseket.

1. Nyissa meg [a sablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Kattintson a sablonként lap jobb oldalán a **szülő erőforráscsoport** **az Azure Deploy**.
3. Ha szükséges, módosítsa a paraméterértékeket, majd az Azure előzetes portál erőforráscsoport telepítéséhez kövesse a.

> [AZURE.IMPORTANT] Ellenőrizze, hogy a tárhely fióknevét egyediek. Ismétlődő tárolási fióknevét Azure-ban nem lehet.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>A háttér VMs terjesztése

A kódmentes VMs attól függenek, hogy az alább felsorolt erőforrások létrehozását.

- **Adatok lemezt tárterület-fiók**. A jobb teljesítmény elérése érdekében az adatok lemezt adatbázis kiszolgálókon prémium tárterület-fiók szükséges egyszínű állapota (SSD) meghajtó technológia fogja használni. Ellenőrizze, hogy a támogatási prémium tároló rendszerbe Azure helyét.
- **NIC**. Minden egyes virtuális lesz két kártyát egy adatbázis eléréséhez, és egy kezelésére.
- **Elérhetőség beállítása**. Egy egyetlen elérhetőségének beállítása, győződjön meg róla a VMs legalább egyike be és operációs rendszert futtató karbantartáskor kell használni hozzáadódik az összes adatbázis-kiszolgálók.  

### <a name="step-1---start-your-script"></a>Lépés: 1 – útmutató a parancsprogram

Töltse le a teljes PowerShell-parancsprogramot, használja [az alábbi](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Kövesse az alábbi lépésekkel módosíthatja a parancsfájl a munkát a környezetben.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Módosítsa a meglévő erőforráscsoport üzembe feletti [vonatkozó követelmények](#Prerequisites)alapján változót az alábbi értékeket.

        $existingRGName        = "IaaSStory"
        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"
        $remoteAccessNSGName   = "NSG-RemoteAccess"
        $stdStorageAccountName = "wtestvnetstoragestd"

2. Módosítsa a kódmentes telepítéshez használni kívánt értékek alapján változót az alábbi értékeket.

        $backendRGName         = "IaaSStory-Backend"
        $prmStorageAccountName = "wtestvnetstorageprm"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $publisher             = "MicrosoftSQLServer"
        $offer                 = "SQL2014SP1-WS2012R2"
        $sku                   = "Standard"
        $version               = "latest"
        $vmNamePrefix          = "DB"
        $osDiskPrefix          = "osdiskdb"
        $dataDiskPrefix        = "datadisk"
        $diskSize              = "120"  
        $nicNamePrefix         = "NICDB"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

3. A meglévő erőforrások szükséges a telepítéshez beolvasni.

        $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
        $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
        $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
        $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Lépés: 2 – a szükséges erőforrások létrehozása a VMs

Új erőforráscsoport, az adatok lemez és az összes VMs beállítása elérhetősége tároló fiók létrehozásához szükséges. Alos van szüksége a helyi rendszergazdai fiók hitelesítő adatait minden virtuális. Ezek az erőforrások létrehozásához hajtsa végre az alábbi lépéseket.

1. Hozzon létre egy új erőforráscsoport.

        New-AzureRmResourceGroup -Name $backendRGName -Location $location

2. Az előbb létrehozott erőforráscsoport prémium tároló új fiók létrehozása

        $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
            -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location

3. Hozzon létre egy új elérhetőségét.

        $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location

4. A helyi rendszergazdai fiók hitelesítő adatait, minden egyes virtuális használandó első.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

### <a name="step-3---create-the-nics-and-backend-vms"></a>Lépés a 3 – a hálózati kártyák és kódmentes VMs létrehozása

Akkor használja a ismétlődve hozhat létre, és hozzon létre a szükséges hálózati kártyák és VMs belül a leállításig annyi VMs. A hálózati kártyák és VMs létrehozásához hajtsa végre az alábbi lépéseket.

1. Indítsa el a `for` ismétlése hozhat létre egy virtuális és két kártyát, ahányszor szükséges, a parancsok ismétlődő értékét alapján a `$numberOfVMs` változó.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Az adatbázis hozzáféréshez használt hálózati létrehozása.

            $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
            $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
            $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1

3. Hozzon létre a hálózati kártya távelérési használható. Figyelje meg, hogyan a hálózati kártya van-e a hozzá tartozó NSG.

            $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
            $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
            $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
                -NetworkSecurityGroupId $remoteAccessNSG.Id

4. Hozzon létre `vmConfig` objektumot.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id

5. Hozzon létre egy virtuális két adatok lemez. Figyelje meg, hogy az adatok lemezt láthatók a korábban létrehozott prémium tárterület-fiókot.

            $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"    
            $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
                -VhdUri $data1VhdUri -CreateOption empty -Lun 0

            $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"    
            $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
                -VhdUri $data2VhdUri -CreateOption empty -Lun 1

6. Az operációs rendszer beállítása, valamint a virtuális használt kép.

            $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
            $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version

7. A két kártyát az előbb létrehozott hozzáadása a `vmConfig` objektumot.

            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id

8. Az operációs rendszer lemez létrehozása és a virtuális létrehozása. Értesítés a `}` befejezése a `for` le.

            $osDiskName = $vmName + "-" + $osDiskSuffix
            $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
            $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
            New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
        }

### <a name="step-4---run-the-script"></a>Lépés: 4 - futtatása

Most, hogy letölti-megváltozott a szükségletek parancsfájl, runt ő parancsfájlok létrehozását a háttér adatbázis VMs több.

1. A parancsfájlok mentse, és futtassa a **PowerShell** a parancssor parancsot, vagy a **PowerShell ISE**. A kezdeti kimenet alább látható módon jelenik meg.

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/IaaSStory-Backend

2. Néhány perc múlva töltse ki a hitelesítő adatok kérdésre, és kattintson az **OK gombra**. A kimenet az alábbi egy egyetlen virtuális jelöli. Értesítés a teljes folyamat befejezéséhez 8 perc került.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        EndTime             : 10/30/2015 9:30:03 AM -08:00
        Error               :
        Output              :
        StartTime           : 10/30/2015 9:22:54 AM -08:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
