<properties
    pageTitle="Virtuális gép skála csoportosító létrehozása PowerShell használatával |} Microsoft Azure"
    description="Virtuális gép skála csoportosító létrehozása PowerShell használatával"
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
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Hozzon létre virtuális gép méretaránnyal Windows Azure PowerShell használatával

Ezeket a lépéseket hajtsa végre a fill-a-a-üres megközelítés létrehozása az Azure virtuális gép skála megadása. [Virtuális gép skála állítja be áttekintése](virtual-machine-scale-sets-overview.md) , ha többet szeretne tudni a méretezés beállítása című témakörben találhat.

A jelen cikkben ismertetett lépések végrehajtásához körülbelül 30 perc kell vennie.

## <a name="step-1-install-azure-powershell"></a>Lépés: 1: Azure PowerShell telepítése

Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) információt az Azure PowerShell legújabb verzióját, jelölje ki az előfizetés, és bejelentkezés az a fiókjába.

## <a name="step-2-create-resources"></a>Lépés: 2: Erőforrások létrehozása

Az új skála megadása a szükséges erőforrások létrehozása.

### <a name="resource-group"></a>Erőforráscsoport

Az erőforráscsoport egy virtuális gép méretarányra beállított szerepelnie kell.

1. Lista beszerzése az elérhető helyek, ahol erőforrások is elhelyezhet:

        Get-AzureLocation | Sort Name | Select Name

2. Válasszon egy helyet, amely a legjobban, **$locName** értékének cserélje ki, hogy a hely neve, és hozzon létre a változó:

        $locName = "location name from the list, such as Central US"

3. **$RgName** értékének cserélje ki a nevet, amelyet szeretne használni az új erőforráscsoport, és hozzon létre a változó: 

        $rgName = "resource group name"
        
4. Az erőforráscsoport létrehozása:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Ez a példa hasonló kell megjelennie:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Tárterület-fiók

Egy tárterület-fiókkal használják a virtuális gépen az operációs rendszer lemez és a méretezés használt diagnosztikai adatok tárolására szolgáló. Lehetséges, esetén javasoljuk az létrehozott egy skála megadása a virtuális gépeken a tárterület-fiók. Ha nem a tárterület-fiók egy legfeljebb 20 VMs lehetséges, megtervezése. Ez a cikk a példában a három virtuális gépeken futó eredményezne három tároló fiók.

1. **$SaName** értékének cserélje a tárterület-fiókja nevét. Tesztelje a egyediségét nevét. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Ha a válasz **Igaz**, akkor a javasolt neve egyedi.

3. **$SaType** értékének cserélje ki a tárterület-fiók típusát, és hozzon létre a változó:  

        $saType = "storage account type"
        
    Lehetséges értékek a következők: Standard_LRS, Standard_GRS, Standard_RAGRS vagy Premium_LRS.
        
4. A fiók létrehozása:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Ez a példa hasonló kell megjelennie:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Ismételje meg az 1-4-es három tároló fiókokat, például myst1 myst2 és myst3 hozhat létre.

### <a name="virtual-network"></a>Virtuális hálózati

Virtuális hálózaton szükség a virtuális gépeken futó skála megadása.

1. **$SubnetName** értékének cserélje ki a nevét, majd az a változó létrehozása és használata: a virtuális hálózaton alhálózat kívánt: 

        $subnetName = "subnet name"
        
2. Az alhálózathoz konfiguráció létrehozása:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    A cím előtagját a virtuális hálózat eltérő lehet.

3. **$NetName** értékének cserélje ki a nevet, amelyet a virtuális-hálózathoz használni szeretne készíteni, majd hozza létre az a változó: 

        $netName = "virtual network name"
        
4. A virtuális hálózat létrehozása:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Skála megadása konfigurálása

Ha a skála megadása a konfigurációs, így érdemes létrehozni a dokumentumot kérő összes erőforrás.  

1. **$IpName** értékének cserélje ki a nevet, amelyet az IP-konfiguráció használata szeretne készíteni, majd hozza létre az a változó: 

        $ipName = "IP configuration name"
        
2. Az IP-konfiguráció létrehozása:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. **$VmssConfig** értékének cserélje ki a nevet, amelyet a méretezés beállítás használata szeretne készíteni, majd hozza létre az a változó:   

        $vmssConfig = "Scale set configuration name"
        
3. Hozzon létre skála megadása a beállításokat:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Ebben a példában skálával beállítása a három virtuális gépeken futó eredményezne. További információ a méretezés készletek kapacitása a [Virtuális gép skála állítja be áttekintése](virtual-machine-scale-sets-overview.md) című témakörben találhat. Ezt a lépést is tartalmaz, a virtuális gépeken futó méretének (néven SkuName) beállítás megadása. Ha talál az igényeinek megfelelő méretet, nézze meg a [virtuális gépeken futó méretét](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. A hálózati kapcsolat konfiguráció hozzáadása a méretezés konfiguráció beállítása:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Ez a példa hasonló kell megjelennie:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Operációs rendszer profil

1. A számítógép neve előtagot, amelyet szeretne használni, és hozza létre az a változó cserélheti **$computerName** értékét: 

        $computerName = "computer name prefix"
        
2. A virtuális gépeken a rendszergazdai fiók neve **$adminName** értéket, és hozza létre az a változó:

        $adminName = "administrator account name"
        
3. **$AdminPassword** értékének cserélje ki a fiók jelszavát, és hozzon létre a változó:

        $adminPassword = "password for administrator accounts"
        
4. Az operációs rendszer profil létrehozása:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Tárterület-profil

1. **$StorageProfile** értékének cserélje ki a nevét, majd az a változó létrehozása és használata: a tárterület-profil kívánt:  

        $storageProfile = "storage profile name"
        
2. Hozzon létre a változók, amely a kép használata:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Ha más képek használatára vonatkozó információkat talál, nézze meg a [Navigálás és a Windows PowerShell és az Azure CLI választó Azure virtuális gép képekkel](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Cserélje ki a virtuális merevlemez tároló, például "https://mystorage.blob.core.windows.net/vhds", az út tartalmazó lista **$vhdContainers** értékét, és hozzon létre a változó:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. A tároló profil létrehozása:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Virtuális gép skála megadása

Végezetül skála megadása hozhat létre.

1. **$VmssName** értékének cserélje ki a virtuális gép skála megadása a nevét, és hozzon létre a változó:

        $vmssName = "scale set name"
        
2. A méretezés-készlet létrehozása:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Meg kell jelennie ebben a példában a sikeres telepítés megjelenítő hasonló:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>3 lépés: Az források

Használja az alábbi forrásokat feltárása az Ön által létrehozott virtuális gép skála megadása:

- Azure portál – a csak korlátozott mennyiségű adatot nem érhető el a portálon.
- [Azure erőforrás Explorer](https://resources.azure.com/) – Ez az eszköz nem leginkább megfelelő felfedezése a méretezés beállítása az aktuális állapotát.
- Azure PowerShell – ezzel a paranccsal tájékozódáshoz:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Következő lépések

- Kezelheti az imént létrehozott [virtuális gépeken futó található virtuális gép skála kezelése](virtual-machine-scale-sets-windows-manage.md) információk felhasználásával skála megadása
- Érdemes beállítania a méretezés funkciójá információk [automatikus méretezése és virtuális gép skála állítja be](virtual-machine-scale-sets-autoscale-overview.md) az automatikus méretezés
- További tudnivalók a [Függőleges Automatikus méretezéssel virtuális gép skála értékkészletet](virtual-machine-scale-sets-vertical-scale-reprovision.md) megtekintésével függőleges méretezése
