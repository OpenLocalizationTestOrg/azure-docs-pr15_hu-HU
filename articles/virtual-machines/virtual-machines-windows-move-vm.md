<properties
    pageTitle="Helyezze át a Windows virtuális |} Microsoft Azure"
    description="Egy Windows virtuális áthelyezése egy másik Azure előfizetés vagy az erőforrás csoportot az erőforrás-kezelő telepítési modell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>A Windows virtuális áthelyezése másik Azure előfizetés vagy az erőforrás csoportba 

Ez a cikk végigvezeti a Windows virtuális áthelyezése erőforrás csoportok vagy előfizetések között. Előfizetések közötti áthelyezése lehet hasznos, ha eredetileg létrehozott egy virtuális személyes előfizetést, és most szeretné helyezze át a vállalat előfizetés továbbra is a munkát.

> [AZURE.NOTE] Új erőforrás-azonosítók létrejön az áthelyezés részeként. Miután a virtuális át lett helyezve, szüksége lesz az eszközök és az új erőforrás lehetővé tevő dokumentumazonosítók használatához parancsfájlok frissítése. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Ugrás egy virtuális a Powershell használatával

Virtuális gép áthelyezése egy másik erőforráscsoport, győződjön meg arról, hogy is helyezi át az összes függő erőforrás szükséges. Az áthelyezés-AzureRMResource parancsmag használata esetén szüksége van az erőforrás neve és az erőforrás típusát. Mind a keresés-AzureRMResource parancsmag elérheti.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Ugrás egy virtuális több erőforrások áthelyezése szükség. Hogy csak hozzon létre külön változók az egyes erőforrásokhoz és majd listában. Ez a példa tartalmazza a legtöbb egyszerű erőforrások egy virtuális, de több szükség szerint hozzáadhat.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Az erőforrások áthelyezése másik előfizetést, helyezze el a **- DestinationSubscriptionId** paramétert. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



A rendszer kéri, hogy erősítse meg, hogy szeretné-e a megadott erőforrások áthelyezése. Írja be a **Y** erősítse meg, hogy szeretné-e az erőforrások áthelyezése.

  
## <a name="next-steps"></a>Következő lépések

Áthelyezheti a számos különböző típusú erőforrások erőforráscsoport és előfizetések között. További tudnivalókért olvassa el a [Új erőforráscsoport vagy-előfizetésre erőforrások áthelyezése](../resource-group-move-resources.md)című témakört.    