<properties
    pageTitle="Hozzon létre virtuális általános merevlemezről |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre virtuális gép Windows Azure a PowerShell használata az erőforrás-kezelő telepítési modell általános virtuális képen lévő."
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
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Hozzon létre egy virtuális egy általános virtuális kép

Egy általános virtuális kép összes [Sysprep](virtual-machines-windows-generalize-vhd.md)segítségével távolítja el a személyes fiókadatok volt. Létrehozhat egy általános virtuális Sysprep egy helyszíni virtuális, majd [az Azure virtuális merevlemez feltöltése](virtual-machines-windows-upload-image.md), a futtatásával vagy egy meglévő Azure virtuális, majd [a virtuális másolása](virtual-machines-windows-vhd-copy.md)a Sysprep futtatásával.

Hozzon létre egy virtuális speciális merevlemezről szeretné, ha lásd: a [létrehozása egy virtuális speciális merevlemezről](virtual-machines-windows-create-vm-specialized.md).

Hozzon létre egy virtuális általános merevlemezről a leggyorsabban az [rövid útmutató az első sablon] használatához (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Előfeltételek

Ha fogja használni egy virtuális egy helyszíni virtuális, például egy a Hyper-V, győződjön meg arról, hogy készült feltöltött követte az [Előkészítés Azure feltöltése a Windows virtuális](virtual-machines-windows-prepare-for-upload-vhd-image.md)útmutatásait. 

Feltöltött VHD és a meglévő Azure virtuális VHD kell egy virtuális létrehozása előtt kell általános ezzel a módszerrel. További tudnivalókért olvassa el a [Windows virtuális gép Generalize Sysprep használata](virtual-machines-windows-generalize-vhd.md)című témakört. 


## <a name="set-the-uri-of-the-vhd"></a>Az URI, valamint a virtuális beállítása

A virtuális használni a URI formátumban van: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Ebben a példában a virtuális **myVHD** nevű szerepel a tároló **mycontainer**a tárterület-fiók **mystorageaccount** .

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Hozzon létre egy virtuális hálózaton

A vNet és a [virtuális hálózati](../virtual-network/virtual-networks-overview.md)alhálózat létrehozása.


1. Az alhálózathoz létrehozása. A következő példa létrehoz egy **mySubnet** az a cím előtaggal **10.0.0.0/24**az erőforrás-csoport **myResourceGroup** nevű alhálózat.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. A virtuális hálózat létrehozása. A következő példa létrehoz egy virtuális hálózati nevű **myVnet** **10.0.0.0/16**cím előtagját a **Nyugati USA -beli** helyen.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat

Engedélyezi a kommunikációt a virtuális gép a virtuális hálózaton, szüksége van egy [nyilvános IP-cím](../virtual-network/virtual-network-ip-addresses-overview-arm.md) és a hálózati kapcsolat.

1. Hozzon létre egy nyilvános IP-címet. Ez a példa létrehoz egy nyilvános IP-cím **myPip**nevű. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. A hálózati létrehozása Ez a példa létrehoz egy hálózati kártya elnevezett **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>A hálózati biztonsági csoport és egy RDP szabály létrehozása

A virtuális RDP segítségével bejelentkezhetne engedélyezni kell, hogy egy biztonsági szabályt, amely lehetővé teszi a port 3389 RDP access. 

Ez a példa egy **myNsg** egy szabályt, amely lehetővé teszi a RDP-forgalmat fölé port 3389 **myRdpRule** nevű tartalmazó nevű NSG hoz létre. NSGs kapcsolatos további tudnivalókért lásd: [a PowerShell használatá Azure-ban egy virtuális portok megnyitása](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>A virtuális hálózat változó létrehozása

Hozzon létre egy befejezett virtuális hálózatok változó. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>A virtuális létrehozása

Az alábbi PowerShell parancsprogramot szemlélteti, hogyan állíthatja be a virtuális gép beállításokat, és a feltöltött virtuális kép forrásaként az új telepítéshez használni.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Ellenőrizze, hogy a virtuális hozták létre 

Amikor elkészült, meg kell jelennie a [Azure portál](https://portal.azure.com) csoportban **Keresse meg**az újonnan létrehozott virtuális > **virtuális gépeken futó**, vagy az alábbi PowerShell-parancsok használatával:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell új virtuális készülék című témakörben [kezelése virtuális gépeken futó Azure erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-ps-manage.md).


