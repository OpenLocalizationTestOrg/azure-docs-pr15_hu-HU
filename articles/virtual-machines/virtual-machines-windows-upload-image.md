<properties
    pageTitle="Az erőforrás-kezelő feltöltése a Windows virtuális |} Microsoft Azure"
    description="Megtudhatja, miként feltöltése a Windows virtuális gépek virtuális a helyszíni Azure, az erőforrás-kezelő telepítési modellt használja. A Válasszon egy általános egy virtuális vagy egy speciális virtuális feltöltheti."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>A Windows Azure szeretné egy helyszíni virtuális a virtuális feltöltése 


Ez a cikk bemutatja, hogyan hozhat létre, és töltse fel a Windows virtuális merevlemez (virtuális) létrehozása az Azure virtuális használható. Egy általános virtuális vagy egy speciális virtuális egy virtuális feltöltheti. 

Ha többet szeretne tudni a lemez és VHD Azure-ban című témakörben [olvashat a lemez és a virtuális gépeken futó VHD](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>A virtuális előkészítése 

Általános és a speciális VHD Azure is feltölthet. Minden egyes igényel, hogy a virtuális megkezdése előtt készítse elő.

- **Általános virtuális** – egy általános virtuális összes Sysprep segítségével távolítja el a személyes fiókadatok volt. Ha azt szeretné, a virtuális képként létrehozásához használandó az új VMs, a következőket kell tennie:
    - [Felkészülés a Windows virtuális Azure feltöltése](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [A virtuális gép Generalize Sysprep használata](virtual-machines-windows-generalize-vhd.md). 

- **Speciális virtuális** – egy speciális virtuális megtartja a felhasználói fiókok, az alkalmazások és az egyéb állam adatok az eredeti virtuális. Ha azt szeretné használni, mint a virtuális-t hozzon létre egy új virtuális, győződjön meg róla, az alábbi lépéseket kell végrehajtani. 
    - [Felkészülés a Windows virtuális Azure feltöltése](virtual-machines-windows-prepare-for-upload-vhd-image.md). A virtuális Sysprep használata **nem** generalize.
    - Távolítsa el a vendégként való bekapcsolódáshoz virtualizációs eszközök és ügynökök is telepítve van a virtuális (azaz VMware eszközök).
    - Gondoskodjon arról, hogy a virtuális be van állítva az IP-cím és a DNS-beállítások DHCP keresztül csoportosítani. Ezzel biztosíthatja, hogy a kiszolgáló IP-címet a VNet belül beolvassa indításakor. 

## <a name="log-in-to-azure"></a>Bejelentkezés az Azure

Ha még nincs PowerShell 1.4, és telepítve van, az olvasási [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md)feletti verziójában.

1. Nyissa meg az Azure PowerShell, és jelentkezzen be az Azure-fiókjába. Előugró ablak megnyílik a adhatja meg a Azure-fiók hitelesítő adatait.

    ```powershell
    Login-AzureRmAccount
    ```


2. Az előfizetés azonosítók beszerzése a rendelkezésre álló előfizetések.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Állítsa a megfelelő előfizetés használata az előfizetéshez azonosítójával. Csere `<subscriptionID>` megfelelő előfizetés azonosítóval.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>A tárterület-fiók beszerzése

A feltöltött virtuális kép tárolásához Azure-ban a tárterület-fiókra van szüksége. Egy meglévő tárterület-fiókot használ, vagy hozzon létre egy újat. 

A rendelkezésre álló tárhely számlák megjelenítése, írja be:

```powershell
Get-AzureRmStorageAccount
```

Ha szeretne egy meglévő tárterület-fiókot használ, ugorjon a [virtuális kép feltöltése](#upload-the-vm-vhd-to-your-storage-account) szakaszban.

Ha meg kell tárterület-fiók létrehozása, kövesse az alábbi lépéseket:

1. Szüksége van az erőforráscsoport, ahol hozható létre a tárterület-fiók nevére. Ha az előfizetés szereplő összes erőforrás csoport, írja be:

    ```powershell
    Get-AzureRmResourceGroup
    ```

**MyResourceGroup** a **Nyugati USA -beli** tartományban lévő nevű erőforráscsoport létrehozásához írja be:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Névvel ellátott **mystorageaccount** erőforrás ebben a csoportban a [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) parancsmaggal tárterület-fiók létrehozása:

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    -SkuName érvényes értékei a következők:

    - **Standard_LRS** - helyileg felesleges tárhelyet. 
    - **Standard_ZRS** - zóna felesleges tároló.
    - **Standard_GRS** - Geo felesleges tárhelyet. 
    - **Standard_RAGRS** - olvasási hozzáférést geo felesleges tárhelyet. 
    - **Premium_LRS** - prémium helyileg felesleges tároló. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Töltse fel a virtuális a tárterület-fiók

A [Hozzáadás-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) parancsmag használatával a kép feltöltése a tároló a tárterület-fiókjában. Ebben a példában a fájl **myVHD.vhd** feltölti `"C:\Users\Public\Documents\Virtual hard disks\"` tárolási fiók neve **mystorageaccount** **myResourceGroup** erőforrás csoportjának. A fájlt tároló **mycontainer** be kerülnek, és az új fájlnevet **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Ha sikeres, kattint választ adott néz ki:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Attól függően, hogy a hálózati kapcsolat és a virtuális fájl méretét Ez a parancs vehet igénybe


## <a name="next-steps"></a>Következő lépések

- [Hozzon létre egy virtuális általános merevlemezről Azure-ban](virtual-machines-windows-create-vm-generalized.md)
- [Speciális merevlemezről Azure-ban egy virtuális létrehozása](virtual-machines-windows-create-vm-specialized.md) csatolva-OS meghajtó legyen-e, amikor létrehoz egy új virtuális.


