<properties
    pageTitle="A Windows virtuális származó adatok lemezen leválasztása |} Microsoft Azure"
    description="Megtudhatja, hogy az erőforrás-kezelő telepítési modellt használja Azure virtuális számítógépről adatok lemezen le."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Hogyan kell a Windows virtuális gép adatok lemezen leválasztása


Ha már nincs szüksége egy virtuális géphez csatlakoztatott adatok lemezen, egyszerűen leválasztás. Ez a lemez eltávolítja a virtuális gép, de azt nem távolítja el a tárhelyről. 

> [AZURE.WARNING] A program nem automatikusan törli lemezen leválasztása esetén. Ha már van prémium tárolóhoz, akkor is tároló díjak a lemez merülnek fel. További információt talál [árak](../storage/storage-premium-storage.md#pricing-and-billing)és a számlázásra prémium tároló használata esetén. 

Ha azt szeretné, a meglévő adatok újra felhasználhatja a lemezen, akkor csatlakoztassa újra virtuális ugyanarra a gépre vagy egy másikat.  


## <a name="detach-a-data-disk-using-the-portal"></a>A portálon adatok lemezen leválasztása

1. A portál-központban válassza a **virtuális gépeken futó**.

2. Jelölje ki a virtuális gép, amelynek a adatok lemez leválasztása, és kattintson az **összes beállítást**szeretné.

3. Válassza a **Beállítások** lap a **lemezt**.

4. Jelölje ki a **lemezen** lap, amely leválasztani kívánt adatok lemez.

5. Az adatok lemezre lap kattintson a **Leválasztás**elemre.


    ![Képernyőkép a Leválasztás gombra.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

A lemez tárolási megmarad, de már nem kapcsolódik egy virtuális számítógépre.


## <a name="detach-a-data-disk-using-powershell"></a>A PowerShell használatá adatok lemez leválasztása

Ebben a példában az első parancs a virtuális gép **MyVM07** nevű fájlt a Get-AzureRmVM parancsmaggal a **RG11** erőforráscsoport kap. A parancs a virtuális gép a **$VirtualMachine** változó tárol. 

A második parancs az adatok lemez DataDisk3 nevű a virtuális számítógépről eltávolítja. 

A véglegesként parancsot a virtuális gép eltávolítása az adatok lemez utolsó lépéseként állapotának frissíti.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


További tudnivalókért lásd: az [Eltávolítás-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Következő lépések

Ha az adatok lemez felhasználni kívánt, akkor csak [azt egy másik virtuális csatolása](virtual-machines-windows-attach-disk-portal.md)
