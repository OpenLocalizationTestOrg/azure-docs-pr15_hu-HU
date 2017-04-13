<properties
    pageTitle="Hozzon létre egy PowerShell használatával Azure virtuális |} Microsoft Azure"
    description="Azure PowerShell- és erőforrás-kezelő Azure segítségével egyszerűen készíthet egy új virtuális Windows Server operációs rendszert futtató."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Hozzon létre egy Windows virtuális erőforrás-kezelő és a PowerShell használatával

Ez a cikk bemutatja, hogyan, amellyel gyorsan létrehozhatja az Azure virtuális gépen fut a Windows Server és a források szüksége van az [Erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md) és PowerShell. 

Ez a cikk lépéseinek hozhat létre virtuális gép van szükség, és hajtsa végre a lépéseket kell igénybe körülbelül 30 percet. Példa paraméterértékeket a parancsok cserélje le a környezet érvényes nevét.

## <a name="step-1-install-azure-powershell"></a>Lépés: 1: Azure PowerShell telepítése

Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) információt az Azure PowerShell legújabb verzióját, jelölje ki az előfizetés, és bejelentkezés az a fiókjába.
        
## <a name="step-2-create-a-resource-group"></a>Lépés: 2: Az erőforrás csoport létrehozása

Az összes erőforrás tartalmaznia kell egy erőforráscsoport, így lehetővé teszi, hogy először hozzon létre.  

1. Lista beszerzése az elérhető helyek, ahol erőforrások hozhatók létre.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Az erőforrások helyének beállítása. Ez a parancs helyének **centralus**állítja be.

    ```powershell
    $location = "centralus"
    ```
    
3. Hozzon létre egy erőforrás csoportot. Ez a parancs az erőforráscsoport **myResourceGroup** beállított helyen nevű hoz létre.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>3 lépés: A tárterület-fiók létrehozása

A virtuális merevlemez a virtuális gép létrehozott által használt tárolni a [tárterület-fiók](../storage/storage-introduction.md) van szükség. Tárterület fióknevét 3 és 24 karakter hosszúságú között kell lennie, és számokat, és csak a kisbetűket is tartalmazhat.

1. Tesztelje a tárhely egyediségét fiók nevét. Ez a parancs azt vizsgálja, hogy a név **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Ez a parancs **Igaz**értéket ad vissza, ha a javasolt nevet egyediségét Azure belül. 
    
2. Most hozzon létre a tárterület-fiókot.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Lépés: 4: Hozzon létre egy virtuális hálózaton

Az összes virtuális gépeken futó [virtuális hálózati](../virtual-network/virtual-networks-overview.md)részét képezik.

1. A virtuális hálózat alhálózat létrehozása. Ez a parancs **mySubnet** egy címet előtaggal a 10.0.0.0/24 nevű alhálózat hoz létre.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Ezután hozzon létre a virtuális hálózat. Ez a parancs az Ön által létrehozott alhálózat és egy cím előtagját **10.0.0.0/16** **myVnet** nevű virtuális hálózat hoz létre.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>5 lépés: A nyilvános IP-cím és a hálózati kapcsolat létrehozása

Engedélyezi a kommunikációt a virtuális gép a virtuális hálózaton, szüksége van egy [nyilvános IP-cím](../virtual-network/virtual-network-ip-addresses-overview-arm.md) és a hálózati kapcsolat.

1. A nyilvános IP-cím létrehozása. Ez a parancs létrehoz egy nyilvános IP-cím **myPublicIp** nevű egy **dinamikus**terhelés metódusát.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. A hálózati kapcsolat létrehozása. Ez a parancs neve hálózati kapcsolat létrehozása **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Lépés a 6: Virtuális gép létrehozása

Most, hogy az összes csatolt már a helyükön vannak, pedig a virtuális gép létrehozása.

1. Ezzel a paranccsal állítsa be a rendszergazdai fiók nevét és jelszavát a virtuális gép futtatni.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    A jelszó 12-123 karakter hosszú, és legalább egy kisbetű karakter, nagybetű karakter, egy szám vagy egy speciális karakter van szükség. 
        
2. A virtuális gép konfigurációs objektum létrehozása Ez a parancs objektumot hoz létre konfigurációs nevű **myVmConfig** , amely meghatározza a virtuális nevét és a virtuális méretét.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Című témakörben [az Azure virtuális gépeken futó méretét](virtual-machines-windows-sizes.md) a rendelkezésre álló méretű virtuális géphez.
    
3. A virtuális operációs rendszer beállításainak konfigurálása Ez a parancs a virtuális állítja be a számítógép nevét, az operációs rendszer típusa és a fiók hitelesítő adatait.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Adja meg a képet, használja a virtuális hozhatók létre. Ez a parancs a Windows Server kép használni a virtuális határozza meg. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Képek használata kiválasztásával kapcsolatos további tudnivalókért lásd: a [Navigálás, és válassza a Windows PowerShell vagy a CLI Azure-ban virtuális gép képek](virtual-machines-windows-cli-ps-findimage.md).
        
5. Az Ön által létrehozott hálózati kapcsolat hozzáadása a konfiguráción.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Adja meg a virtuális merevlemez helyét és nevét. A virtuális merevlemez-fájlt tároló vannak tárolva. Ez a parancs a lemez **vhds/WindowsVMosDisk.vhd** létrehozott tároló fiók nevű tároló hoz létre.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Az operációs rendszer lemez adatainak hozzáadása a virtuális konfiguráció. **$DiskName** értékének cserélje az operációs rendszer lemezen nevét. A változó létrehozása, és adja meg a lemez adatokat a konfiguráción.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Végül a virtuális gép létrehozása.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Következő lépések

- A telepítési problémák volt, a következő lépésben akkor is megtekintheti a [Hibaelhárítás erőforrás csoport telepítések Azure Portal segítségével](../resource-manager-troubleshoot-deployments-portal.md)
- Megtudhatja, hogy miként kezelheti a virtuális gép megtekintésével [kezelése virtuális gépeken futó Azure erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-ps-manage.md)létrehozott.
- Sablon használatával hozhat létre virtuális gép létrehozása [a Windows virtuális gép az erőforrás-kezelő sablonnal](virtual-machines-windows-ps-template.md) információk segítségével előnyeinek kihasználása
