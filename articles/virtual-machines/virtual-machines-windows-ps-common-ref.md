<properties 
   pageTitle="Az VMs közös PowerShell-parancsait |} Microsoft Azure"
   description="Első lépések létrehozása és kezelése a Windows Azure-ban VMs közös PowerShell-parancsait"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Gyakori PowerShell-parancsait létrehozásához és kezeléséhez VMs

Ez a cikk bemutatja, hogy néhány hozhat létre és kezelhet az előfizetése Azure virtuális gépeken futó Azure PowerShell-parancsait.  Az adott parancssori kapcsolók és a beállítások részletes súgója **– Súgó** *parancsot*is használhatja.

Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) információt az Azure PowerShell legújabb verzióját, jelölje ki az előfizetés, és bejelentkezés az a fiókjába.

## <a name="create-a-vm"></a>Hozzon létre egy virtuális

Tevékenység | Parancs
-------------- | -------------------------
Hozzon létre egy virtuális konfiguráció | $vm = [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>A virtuális konfiguráció megadása, és a virtuális beállításainak frissítheti használják. A konfiguráció van inicializálni a virtuális és [méretét](virtual-machines-windows-sizes.md)a nevet.
Konfigurációs beállítások hozzáadása | $vm = a [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - virtuális $vm – a Windows - számítógépnév "számítógépnév" – a hitelesítő adatok $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Operációs rendszer beállítások, beleértve a [hitelesítő adatok](https://technet.microsoft.com/library/hh849815.aspx) bekerülnek a konfigurációs objektumra, amely a korábban létrehozott új-AzureRmVMConfig használatával.
A hálózati kapcsolat hozzáadása | $vm [Hozzáadása-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - virtuális $vm =-azonosító $adaptert Azonosító<BR></BR><BR></BR>A virtuális rendelkeznie kell a [hálózati kapcsolat](virtual-machines-windows-ps-create.md) virtuális hálózatban kapcsolatba lépni. [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) segítségével beolvashatja egy meglévő hálózati kapcsolat objektumra.
Adja meg a platform képe | $vm [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - virtuális $vm - PublisherName "publisher_name" =-ajánlat "publisher_offer" - termékváltozatok "product_sku"-"legújabb" verziója<BR></BR><BR></BR>[Kép információk](virtual-machines-windows-cli-ps-findimage.md) bekerülnek a konfigurációs objektumra, amely a korábban létrehozott új-AzureRmVMConfig használatával. Az objektum által eredményül adott Ez a parancs csak akkor platform kép használata: az operációs rendszer lemez beállításakor használatos.
Beállítása-OS lemezt platform kép használata: | $vm = a [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - virtuális $vm-neve "disk_name" - VhdUri "http://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - CreateOption FromImage<BR></BR><BR></BR>A korábban létrehozott konfigurációs objektum nevét, valamint az operációs rendszer lemezen a [tárolási](../storage/storage-powershell-guide-full.md) helyére kerül.
Operációs rendszer lemezt, hogy egy általános képet beállítása | $vm = a set-AzureRmVMOSDisk - virtuális $vm-neve "disk_name" - SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk. {guid} .vhd"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage – Windows<BR></BR><BR></BR>A konfigurációs objektum nevét, valamint az operációs rendszer lemezen, a forráskép helyét, a lemez [tárolási](../storage/storage-powershell-guide-full.md) helyére kerül.
Egy speciális képet-OS lemezt beállítása | $vm = a set-AzureRmVMOSDisk - virtuális $vm-neve "name_of_disk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/" - CreateOption csatolása – Windows
Hozzon létre egy virtuális | [Új-AzureRmVM]() - ResourceGroupName "resource_group_name"-"location_name" hely – virtuális $vm<BR></BR><BR></BR>Az összes erőforrások [erőforráscsoport](../powershell-azure-resource-manager.md)jönnek létre. A virtuális ugyanazon a [helyen](https://msdn.microsoft.com/library/azure/dn495177.aspx) erőforráscsoport kell létrehozni. Mielőtt a parancsot futtatja, futtassa az új-AzureRmVMConfig, a Set-AzureRmVMOperatingSystem, a Set-AzureRmVMSourceImage, a hozzáadása-AzureRmVMNetworkInterface és a Set-AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>VMs adatainak megtekintése

Tevékenység | Parancs
-------------- | -------------------------
Az előfizetés lista VMs| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Az erőforráscsoport lista VMs | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Erőforrás-csoportok listája az előfizetését, használja [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
A virtuális adatainak megtekintése | Get-AzureRmVM - ResourceGroupName "resource_group_name"-neve "vm_name"

## <a name="manage-vms"></a>VMs kezelése

Tevékenység | Parancs
-------------- | -------------------------
Indítsa el a virtuális | [Kezdés-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-neve "vm_name"
A virtuális leállítása | [Leállítás-AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-neve "vm_name"
Indítsa újra a futó virtuális | [Indítsa újra-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-neve "vm_name"
A virtuális törlése | [Eltávolítás-AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-neve "vm_name"
A virtuális generalize | [Set-AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-neve "vm_name"-általános<BR></BR><BR></BR>Mentés-AzureRmVMImage futtatása előtt a művelet végrehajtása
A virtuális rögzítése | [Mentés-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-elérési út "C:\filepath\filename.json"<BR></BR><BR></BR>A virtuális gép [le, és általános](virtual-machines-windows-generalize-vhd.md) képet létrehozásához használandó kell lennie. Mielőtt a parancsot futtatja, futtassa a Set-AzureRmVm.
A virtuális frissítése | [Frissítés-AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - virtuális $vm<BR></BR><BR></BR>Az aktuális virtuális konfiguráció használata a Get-AzureRmVM első, a virtuális objektum konfigurációs beállítások módosítása, és futtassa a ezt a parancsot.
Adatok lemezen hozzáadása egy virtuális | [Hozzáadás-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - virtuális $vm-neve "disk_name" - VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - logikai #-gyorsítótárazás ReadWrite - DiskSizeinGB # - CreateOption üres<BR></BR><BR></BR>Get-AzureRmVM segítségével a virtuális objektumot. Adja meg a logikai számot és a lemez méretét. Futtassa a frissítés-AzureRmVM a konfigurációs módosítások alkalmazása a virtuális. A lemez fel nem inicializált. Inicializálása lemezre, hozzáadásukkor tudni olvassa el a [kezelése Azure virtuális gépeken futó erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-ps-manage.md)című témakört.
Adatok lemezen eltávolítása egy virtuális | [Eltávolítás-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - virtuális $vm-neve "disk_name"<BR></BR><BR></BR>Get-AzureRmVM segítségével a virtuális objektumot. Futtassa a frissítés-AzureRmVM a konfigurációs módosítások alkalmazása a virtuális.
A virtuális-bővítmény hozzáadása | [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-hely "azure_location" - VMName "vm_name"-neve "extension_name"-"publisher_name" Publisher-típus "extension_type" - TypeHandlerVersion "#. #"-beállítások $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Ez a parancs futtatása a megfelelő [beállításokat](virtual-machines-windows-extensions-configuration-samples.md) a telepíteni kívánt bővítmény.
A virtuális bővítmény eltávolítása | [Eltávolítás-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-neve "extension_name" - VMName "vm_name"

## <a name="next-steps"></a>Következő lépések

- Lásd: az alapvető lépések hozhat létre virtuális gép a [Hozzon létre egy Windows virtuális erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-ps-create.md).

