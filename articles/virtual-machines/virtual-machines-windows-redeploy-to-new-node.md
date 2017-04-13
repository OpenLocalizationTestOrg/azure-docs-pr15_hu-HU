<properties 
    pageTitle="A Windows virtuális gépeken futó telepítsen újra |} Microsoft Azure" 
    description="Telepítsen újra Windows virtuális gépeken futó csökkentésében RDP kapcsolódási problémákat ismerteti." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Új Azure csomópont virtuális gépen újratelepítése

Ha meg van már szemben lévő nehézségeket távoli asztali RDP hibaelhárítási kapcsolat vagy alkalmazás hozzáférést a Windows-alapú Azure virtuális gép (virtuális), a virtuális újbóli nyújthatnak segítséget: Amikor egy virtuális meg újratelepítése, a virtuális áthelyezi egy új csomópontjának az Azure infrastruktúra, és majd hatásköröket az visszakapcsolásához, de megtartja a beállítási lehetőségek és a kapcsolódó erőforrásokat. Ez a cikk bemutatja, hogyan újratelepítenie egy virtuális Azure PowerShell alrendszerrel vagy az Azure-portálra.

> [AZURE.NOTE] Után egy virtuális meg újratelepítése, ideiglenes lemez nem vesznek el, és dinamikus IP-címek virtuális hálózati kapcsolat társított frissülnek. 

## <a name="using-azure-powershell"></a>Azure PowerShell használatával

Győződjön meg arról, hogy a legújabb Azure PowerShell 1.x van telepítve a számítógépre. További információért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

A Powershellhez Azure parancs segítségével a virtuális gép telepítsen újra:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Következő lépések
A virtuális problémák merülnek fel, ha a [Hibaelhárítás RDP-kapcsolatok](virtual-machines-windows-troubleshoot-rdp-connection.md) vagy [részletes RDP hibaelhárításhoz keres útmutatást](virtual-machines-windows-detailed-troubleshoot-rdp.md)segítséget is megkeresheti. Ha egy alkalmazást a virtuális nem fér hozzá, olvassa el [alkalmazás hibaelhárítási problémák](virtual-machines-windows-troubleshoot-app-connection.md).