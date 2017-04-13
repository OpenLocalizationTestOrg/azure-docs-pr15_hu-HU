<properties
    pageTitle="Ugrás egy Linux virtuális |} Microsoft Azure"
    description="Egy Linux virtuális áthelyezése egy másik Azure előfizetés vagy az erőforrás csoportot az erőforrás-kezelő telepítési modell."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Egy Linux virtuális áthelyezése egy másik, előfizetést, vagy az erőforrás csoport

Ez a cikk végigvezeti a Linux virtuális áthelyezése erőforrás csoportok vagy előfizetések között. Egy virtuális áthelyezése másik előfizetések lehet hasznos, ha létrehozott egy virtuális személyes előfizetést, és most szeretné helyezze át a vállalat előfizetés.

> [AZURE.NOTE] Új erőforrás-azonosítók az áthelyezés részeként jönnek létre. Miután a virtuális át lett helyezve, módosítania kell a eszközök és parancsfájlok az új erőforrás lehetővé tevő dokumentumazonosítók használatához. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Lépés egy virtuális az Azure CLI használatával 

A virtuális sikeresen áthelyezéséhez kell a virtuális és az támogató erőforrásait. Az **azure csoport megjelenítése** paranccsal az erőforráscsoport és azonosítójuk az erőforrások listájának. Ez a parancs pipe fájlba, másolja a vágólapra, és illessze be az azonosítók újabb parancsok könnyebben.

    azure group show <resourceGroupName>

Ugrás egy virtuális és az erőforrások erőforráscsoport egy másik, a paranccsal **azure erőforrás áthelyezése** CLI. A következő példa bemutatja egy virtuális és a leggyakrabban használt erőforrások igényel. A Microsoft **-i** paraméter használatával fázisban és vesszővel tagolt listáját (szóköz nélkül) áthelyezése az erőforrásokhoz tartozó azonosítók.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Ha a virtuális és az erőforrások áthelyezése egy másik előfizetést, vegye fel a **– cél-subscriptionId #60; destinationSubscriptionID & #62;** paraméterrel adja meg a célként megadott előfizetést.

Ha a parancssorból Windows rendszerű számítógépen dolgozik, hogy fel kell vennie egy **$** elé a változóinak nevében, amikor történő deklarálását őket. Ez a Linux nem szükséges.

Győződjön meg arról, hogy az adott erőforrás áthelyezni kívánt kérni. Írja be a **Y** erősítse meg, hogy szeretné-e az erőforrások áthelyezése.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Következő lépések

Áthelyezheti a számos különböző típusú erőforrások erőforráscsoport és előfizetések között. További tudnivalókért olvassa el a [Új erőforráscsoport vagy-előfizetésre erőforrások áthelyezése](../resource-group-move-resources.md)című témakört.    