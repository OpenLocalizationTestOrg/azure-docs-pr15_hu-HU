<properties
    pageTitle="Másolat készítése a Windows virtuális |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre egy példányát a speciális, az erőforrás-kezelő telepítési modell fut, a Windows Azure virtuális."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Hozzon létre egy virtuális speciális merevlemezről

Hozzon létre egy új virtuális egy speciális virtuális csatolásával, mint a Powershell használatá OS lemez. A speciális virtuális megtartja a felhasználói fiókok, az alkalmazások és az egyéb állam adatok az eredeti virtuális. 

Hozzon létre egy virtuális általános merevlemezről szeretné, ha lásd: a [létrehozása egy virtuális általános virtuális képen lévő](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Az alhálózathoz és vNet létrehozása

A vNet és a [virtuális hálózati](../virtual-network/virtual-networks-overview.md)alhálózat létrehozása.

1. Az alhálózathoz létrehozása. Ebben a példában **mySubNet**, az erőforrás csoport **myResourceGroup**nevű alhálózat hoz létre, és **10.0.0.0/24**állítja be a cím előtagot.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Hozzon létre a vNet. Ebben a példában a virtuális hálózat neve **myVnetName**, a **Nyugati USA -beli**hely és a cím előtagját a virtuális hálózat **10.0.0.0/16**állítja be. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Hozzon létre egy nyilvános IP-cím és a hálózati kártya

Engedélyezi a kommunikációt a virtuális gép a virtuális hálózaton, szüksége van egy [nyilvános IP-cím](../virtual-network/virtual-network-ip-addresses-overview-arm.md) és a hálózati kapcsolat.

1. A nyilvános IP létrehozása. Ebben a példában a nyilvános IP-cím neve **myIP**értéke.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. A hálózati létrehozása Ebben a példában a hálózati kártya neve **myNicName**értéke.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>A hálózati biztonsági csoport és egy RDP szabály létrehozása

A virtuális RDP segítségével bejelentkezhetne engedélyezni kell, hogy egy biztonsági szabályt, amely lehetővé teszi a port 3389 RDP access. Mivel a virtuális az új virtuális jött létre egy már meglévő speciális virtuális, miután a virtuális hoz létre, használhatja a forrás virtuális gép, amely a korábban engedéllyel RDP használatával jelentkezzen be a meglévő fiók.

Ebben a példában a NSG nevének beállítása **myNsg** és a **myRdpRule**RDP szabály nevét.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Végpontok és NSG szabályok kapcsolatos további tudnivalókért olvassa el a [portok megnyitása egy virtuális gép PowerShell használatával Azure-ban](virtual-machines-windows-nsg-quickstart-powershell.md)című témakört.

## <a name="create-the-vm-configuration"></a>A virtuális konfiguráció létrehozása

A virtuális konfiguráció beállítása a másolt virtuális, mint az operációs rendszer virtuális csatolhat.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Ha adatok lemezt a virtuális csatolva van szükség, akkor is fel kell a következő: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Az adatokra és az operációs rendszer lemez URL-címek valami hasonló látható: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Talál ezt a portálon a cél tároló tároló tallózással, kattintva az operációs rendszer vagy a virtuális másolt adatok és, majd másolja át az URL-címet a tartalmát.


## <a name="create-the-vm"></a>A virtuális létrehozása

Hozzon létre a virtuális az, hogy az imént létrehozott konfigurációk használatával.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Ha ezt a parancsot sikerült, látni fogja a kimeneti jelennek meg:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Ellenőrizze, hogy a virtuális hozták létre 
 
Meg kell jelennie a újonnan létrehozott virtuális vagy az [Azure portálon](https://portal.azure.com) **Nyissa meg**a > **virtuális gépeken futó**, vagy az alábbi PowerShell-parancsok használatával:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Következő lépések

Jelentkezzen be az új virtuális gép, tallózással keresse meg a virtuális a [portált](https://portal.azure.com), kattintson a **Csatlakozás**gombra, és nyissa meg a távoli asztali RDP-fájlt. A fiók hitelesítő adatait az eredeti virtuális gép használatával jelentkezzen be az új virtuális gép. További tudnivalókért olvassa el a [miként csatlakozhat, és jelentkezzen be az Azure virtuális gép Windows operációs rendszert futtató](virtual-machines-windows-connect-logon.md)című témakört.







