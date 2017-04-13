<properties
   pageTitle="Hozzon létre egy Windows virtuális több |} Microsoft Azure"
   description="Megtudhatja, hogy miként készíthet egy Windows virtuális több NIC csatolva Azure PowerShell vagy az erőforrás-kezelő sablonok használata."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>A Windows virtuális több létrehozása
Létrehozhat egy virtuális gép (virtuális), amelynek több virtuális hálózati kapcsolat (NIC) kapcsolódik Azure-ban. A leggyakoribb helyzet, hogy az előtér és a kapcsolat vagy megfigyelések vagy biztonsági megoldássá dedikált hálózati különböző alhálózathoz lenne. Ebben a cikkben egy virtuális létrehozását csatolva több NIC gyors parancsok. Részletes információt, például hogy miként hozhat létre saját PowerShell-parancsfájlokat belül több NIC további [többszörös-hálózati VMs telepítésével](../virtual-network/virtual-network-deploy-multinic-arm-ps.md)kapcsolatos. Különböző [méretű virtuális](virtual-machines-windows-sizes.md) NIC eltérő számú támogatják, így méretezés ennek megfelelően a virtuális.

>[AZURE.WARNING] Ha létrehozott egy virtuális – nem adható hozzá NIC egy meglévő virtuális csatlakoztatnia kell több. Létrehozhat [egy virtuális az eredeti virtuális lemez(ek) alapján](virtual-machines-windows-vhd-copy.md) , és hozzon létre több, mint a virtuális rendszerbe.

## <a name="create-core-resources"></a>Alapvető erőforrások létrehozása
Győződjön meg arról, hogy van-e a [legújabb Azure PowerShell telepítette és beállította](../powershell-install-configure.md). Jelentkezzen be az Azure-fiók:

```powershell
Login-AzureRmAccount
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméterek neve szereplő `myResourceGroup`, `mystorageaccount`, és `myVM`.

Első lépésként hozzon létre egy erőforráscsoport. Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `WestUs` helyét:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Ahhoz, hogy a VMs tárterület-fiók létrehozása. Az alábbi példa létrehoz egy tároló nevű fiókot `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Virtuális hálózati és alhálózat létrehozása
Két virtuális hálózati alhálózat - definiálása egyik az előtér-forgalmat, és a háttér-forgalmat. A következő példa két alhálózat nevű definiálja `mySubnetFrontEnd` és `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

A virtuális hálózati és alhálózat hozhat létre. Az alábbi példa létrehoz egy virtuális hálózati nevű `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Több létrehozása
Hozzon létre két kártyát az előtér-alhálózathoz egy hálózati és egy hálózati csatolása a háttéradatbázist alhálózat. Az alábbi példa létrehoz két kártyát nevű `myNic1` és `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

A szokásos is létrehozhat egy [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../load-balancer/load-balancer-overview.md) kezelheti és megoszthatja irányítja át a VMs érdekében. A [részletes többszörös-hálózati virtuális](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) cikk végigvezeti hálózati biztonsági csoport létrehozása és hozzárendelése a NIC.


## <a name="create-the-virtual-machine"></a>A virtuális gép létrehozása
Most már indítsa el a virtuális konfigurációs össze. Minden virtuális méretét rendelkezik, amely egy virtuális vehet fel NIC száma korlátozott. További információ [a Windows virtuális méretét](virtual-machines-windows-sizes.md). 

Első lépésként meg a virtuális hitelesítő adatait a `$cred` változó az alábbiak szerint:

```powershell
$cred = Get-Credential
```

A következő példa megadja egy virtuális nevű `myVM` , és használja a virtuális mérete legfeljebb két kártyát támogató (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

A virtuális config a többi létrehozása. Az alábbi példa létrehoz egy Windows Server 2012 R2 virtuális:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

A korábban létrehozott két kártyát csatolása:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

A tárhely és a virtuális lemez állítsa be az új virtuális:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Végezetül hozzon létre egy virtuális:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Erőforrás-kezelő sablonok használatával több NIC létrehozása
Azure erőforrás-kezelő sablonok deklaráció JSON-fájlok használatával a környezet meghatározása. Erről [az Azure erőforrás-kezelő áttekintése](../azure-resource-manager/resource-group-overview.md). Erőforrás-kezelő sablonok létrehozása több példányának egy erőforrás például több létrehozása a telepítés során ad lehetőséget. *Másolat* használatával hozhat létre példányok számának megadása:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

[Több példányon *másolással*létrehozásával](../resource-group-create-multiple.md)kapcsolatban további. 

Is használhatja a `copyIndex()` hozzáfűzni majd szám erőforrás nevét, amely lehetővé teszi, hogy hozzon létre `myNic1`, `MyNic2`stb. Az alábbi példa az index értékét hozzáfűzése:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

[Erőforrás-kezelő sablonok használatával több NIC létrehozása](../virtual-network/virtual-network-deploy-multinic-arm-template.md)a teljes példa erről.

## <a name="next-steps"></a>Következő lépések
Győződjön meg arról, tekintse át a [Windows virtuális méretű](virtual-machines-windows-sizes.md) hoz létre egy virtuális több meg. Figyelje meg a minden virtuális méretét támogatja NIC maximális száma. 

Ne feledje, hogy egy meglévő virtuális nem vehet fel további NIC, amikor rendszerbe állítják a virtuális létre kell hoznia az összes NIC. Győződjön meg róla, hogy van-e kezdettől fogva szükséges hálózati kapcsolat a telepítés tervezésekor gondoskodik.