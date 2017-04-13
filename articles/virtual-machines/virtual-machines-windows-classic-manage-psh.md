<properties
   pageTitle="A virtuális gépeken futó kezelése Azure PowerShell használatával |} Microsoft Azure"
   description="Ismerje meg, hogy a virtuális gépeken futó kezelése a feladatok automatizálásához használható parancsokat."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>A virtuális gépeken futó kezelése Azure PowerShell használatával

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Sok feladatokhoz kezelheti a VMs naponta Azure PowerShell-parancsmagokkal automatizálható. Ez a cikk lehetővé teszi például parancsokat egyszerűbb feladatokhoz és hivatkozások cikkekre mutató megjelenítése a parancsok összetettebb feladatokhoz.

>[AZURE.NOTE] Ha még nem telepítette és beállította az Azure PowerShell, útmutatást [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md)című témakörben kaphat.

## <a name="how-to-use-the-example-commands"></a>A példa a parancsok használata
A parancsok a szöveg egy része cserélje ki a környezet megfelelő szöveg lesz szüksége. A < és > szimbólumok jelzik a szöveg ki kell cserélni. Ha a szöveg cseréje, távolítsa el a szimbólumok, de hagyja a árajánlat a megfelelő helyen.

## <a name="get-a-vm"></a>A virtuális beszerzése
Ez az egyszerű feladat gyakran kell használni. Annak segítségével egy virtuális adatainak, egy virtuális különböző művelet végrehajtását vagy első kimeneti változó tárolásához.

A virtuális kapcsolatos tájékozódáshoz, futtassa a cseréjével kapcsolatban, amit az idézőjelekkel együtt a parancs a < és > karakterek:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

A kimenet $vm változó tárolásához, futtatása:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Jelentkezzen be a Windows-alapú virtuális rendszerbe

A parancsok:

>[AZURE.NOTE] A **Get-AzureVM** parancs megjelenített elérheti a virtuális gép és a felhő szolgáltatás neve.
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>A virtuális leállítása

Ez a parancs futtatása:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]A paraméter használatával nyomon a virtuális IP (virtuális) a felhőalapú szolgáltatás, abban az esetben, ha a legutóbbi virtuális adott felhőszolgáltatásában. <br><br> Ha StayProvisioned paraméter használatával, akkor fognak továbbra is számlát kapni a virtuális.

## <a name="start-a-vm"></a>Indítsa el a virtuális

Ez a parancs futtatása:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Lemezen adatok csatolása
A feladat végrehajtásához szükséges néhány lépést. Először használja az x hozzáadása-AzureDataDisk x parancsmag adja hozzá a lemezt a $vm objektumot. **Frissítés-AzureVM** parancsmag majd, a virtuális beállításainak frissítheti használja.

Is kell eldönteni, hogy az új lemez vagy egy adatokat tartalmazó csatolása e. Egy új lemez a parancs a .vhd fájl hoz létre, és csatolja.

Az új lemez csatolásához Ez a parancs futtatása:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Szeretne csatolni egy meglévő adatok lemezre, ez a parancs futtatása:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Meglévő .vhd fájl blob-tárolóban lévő adatok lemezt csatolásához Ez a parancs futtatása:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>A Windows-alapú virtuális létrehozása

Hozzon létre egy új Windows-alapú virtuális gép Azure-ban, használja az [Azure PowerShell használata hozhat létre, és adja meg a Windows-alapú virtuális gépeken futó előre](virtual-machines-windows-classic-create-powershell.md)a képernyőn megjelenő utasításokat. Ez a témakör lépéseit az Azure PowerShell parancs, amely beállított kibocsátása végig hozza létre a Windows-alapú virtuális, előre meghatározhatja, hogy:

- Az Active Directory tartományi tagság.
- A további lemezt.
- Egy meglévő terheléselosztás tagjaként beállítása.
- A statikus IP-címet.
