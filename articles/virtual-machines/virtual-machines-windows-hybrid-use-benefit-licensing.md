<properties
   pageTitle="Az ablak Server Azure hibrid használata kedvezménye |} Microsoft Azure"
   description="Útmutató: a Windows Server frissítési garancia előnyöket a helyszíni licencek előtérbe Azure maximalizálása"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>A Windows Server Azure hibrid használata előny

A Windows Server használata frissítési garancia ügyfelek előtérbe a helyszíni licencek Windows Server Azure, és futtassa a Windows Server VMs Azure-ban csökkentett költség mellett. Azure hibrid használata számára nyújtott előnyökre: lehetővé teszi futtassa a Windows Server VMs Azure-ban, és csak az alap számítási ráta az első számlázható. További információt talál az [Azure hibrid használata kedvezménye licencelés lapot](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Ez a cikk ismerteti, hogyan telepítése a Windows Server VMs használni a licencelési kedvezménye Azure-ban.

> [AZURE.NOTE] A Windows Server VMs az Azure hibrid használata előny felhasználásával üzembe Azure piactéren elérhető képek nem használható. A PowerShell vagy az erőforrás-kezelő sablonok használata megfelelően regisztrálhatja a VMs jogosult alap számítási ráta engedmény VMs kell telepítenie.

## <a name="pre-requisites"></a>Előzetes követelmények
Létezik néhány előtti követelmények való csatlakozást Azure hibrid használata ellátás a Windows Server VMs Azure-ban:

- Az Azure PowerShell-modult telepítve van
- A Windows Server virtuális Azure tároló feltöltött

### <a name="install-azure-powershell"></a>Azure PowerShell telepítése
Győződjön meg arról, hogy [telepítette és beállította a legújabb Azure Powershellt](../powershell-install-configure.md). Még ha az erőforrás-kezelő sablonok használata VMs üzembe fogja, van szüksége van telepítve, töltse fel a Windows Server virtuális Azure PowerShell (lásd a következő lépés).

### <a name="upload-a-windows-server-vhd"></a>A Windows Server virtuális feltöltése

A Windows Server virtuális Azure-ban üzembe helyezéséhez először kell egy virtuális, amely tartalmazza az alap Windows Server-összeállítás létrehozása. A virtuális kell megfelelően készíteni keresztül Sysprep Azure feltöltése előtt. [További információ a virtuális követelmények és Sysprep folyamat](./virtual-machines-windows-upload-image.md) és a [Kiszolgálói szerepkörök Sysprep támogatása](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)megtekintheti. Sysprep futtatása előtt készítsen biztonsági másolatot a virtuális. Miután a virtuális készített, töltse fel a virtuális az Azure tárolási fiók használatával a `Add-AzureRmVhd` parancsmag az alábbiak szerint:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server, a SharePoint Server és a Dynamics is használhat a frissítési garancia licencelési. Van szüksége a Windows Server kép előkészítése a alkalmazásösszetevők telepítése és lehetőséget biztosító licencet billentyűk, majd a lemez kép feltöltése Azure. Tekintse át az alkalmazással, például [az SQL Server telepítése előtt megfontolandó kérdések Sysprep használata](https://msdn.microsoft.com/library/ee210754.aspx) vagy [a SharePoint Server 2016 hivatkozás képe (Sysprep) összeállítása](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx)Sysprep futtatása dokumentációját.

Is további információ [a folyamat Azure virtuális feltöltéséről](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Ez a cikk a Windows Server VMs üzembe helyezése koncentrál. Windows-ügyfél VMs azonos módon is telepítheti. Az alábbi példákban cseréje `Server` a `Client` megfelelően.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>A virtuális keresztül PowerShell rövid terjesztése
A Windows Server virtuális PowerShell telepítésekor van-e az újabb paraméterrel `-LicenseType`. Ha befejezte a feltöltött Azure virtuális, amelyet Ön hozott létre egy új virtuális `New-AzureRmVM` , és adja meg a licencelés az alábbi képlettel történik:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

[Olvassa el a PowerShell Azure-ban egy virtuális telepítéséről további részletes ismertetését megtalálja](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) az alábbi is, vagy olvassa el a különböző lépéseket [Hozzon létre egy erőforrás-kezelő és a PowerShell használata Windows virtuális](./virtual-machines-windows-ps-create.md)kifejezőbb útmutató.

## <a name="deploy-a-vm-via-resource-manager"></a>A virtuális keresztül erőforrás-kezelő terjesztése
Az erőforrás-kezelő sablonok, az újabb paraméterrel belül `licenseType` adható meg. Erről további információ az [erőforrás-kezelő Azure-sablonok létrehozása](../resource-group-authoring-templates.md). Ha befejezte a feltöltött Azure virtuális, erőforrás-kezelő sablon szerkesztése a licenctípus szerepeltetni a számítási szolgáltató és a szokásos módon sablon telepítése:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Ellenőrizze, hogy a virtuális van felhasználásával licencelési számára nyújtott előnyökre:
Miután telepítette a virtuális keresztül vagy a PowerShell vagy az erőforrás-kezelő telepítési mód, ellenőrizze a licenccel rendelkező típus `Get-AzureRmVM` az alábbi képlettel történik:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

A kimenet szövege a következőhöz hasonló:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

A következő virtuális nélkül Azure hibrid használata kedvezménye licencelés lapot, például egy virtuális egyenesen az Azure gyűjteményből rendszerbe telepített ezzel ellentétben:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Részletes PowerShell útmutató

A részletes PowerShell lépések megjelenítése egy virtuális álló teljes telepítés. Erről további környezeti tényleges parancsmagok és más összetevőket [Hozzon létre egy erőforrás-kezelő és a PowerShell használata Windows virtuális](./virtual-machines-windows-ps-create.md)hoznak létre. Végezze el az erőforráscsoport, a tárterület-fiók és a virtuális hálózat, ezután a virtuális megadása, és végül létrehozása a virtuális.
 
Első lépésként biztonságosan hitelesítő adatok beszerzése, állítsa be egy helyet és egy erőforrás csoport neve:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Hozzon létre egy nyilvános IP:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Az alhálózathoz, hálózati kártya és VNET meghatározása:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Nevezze el a virtuális, és hozzon létre egy virtuális config:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Adja meg az operációs rendszer:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

A virtuális a hálózati kártya hozzáadásához:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Adja meg a tárterület-fiók használata:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Töltse fel a virtuális megfelelően készíteni, és csatolja a virtuális használatra:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Végül a virtuális létrehozása és Azure hibrid használata kedvezménye használja a licencelési típusának megadásához:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Következő lépések

További információ az [Azure hibrid használata kedvezménye licencelési](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

További tudnivalók az [erőforrás-kezelő sablonok használatával](../azure-resource-manager/resource-group-overview.md).
