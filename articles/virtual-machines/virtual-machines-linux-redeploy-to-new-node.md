<properties 
    pageTitle="Telepítsen újra Linux virtuális gépeken futó |} Microsoft Azure" 
    description="Telepítsen újra Linux virtuális gépeken futó csökkentésében SSH kapcsolódási problémákat ismerteti." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Új Azure csomópont virtuális gépen újratelepítése

Ha van már szemben lévő nehézségeket hibaelhárítási SSH vagy alkalmazás hozzáférést az Azure virtuális gép (virtuális), a virtuális újbóli nyújthatnak segítséget. Amikor egy virtuális meg újratelepítése, a virtuális áthelyezi egy új csomópontjának az Azure infrastruktúra, és majd hatásköröket az visszakapcsolásához, de megtartja a beállítási lehetőségek és a kapcsolódó erőforrásokat. Ez a cikk bemutatja, hogyan újratelepítenie egy virtuális Azure CLI vagy az Azure portal segítségével.

> [AZURE.NOTE] Után egy virtuális meg újratelepítése, ideiglenes lemez nem vesznek el, és dinamikus IP-címek virtuális hálózati kapcsolat társított frissülnek. 


## <a name="using-azure-cli"></a>Azure CLI használatával

Ellenőrizze, hogy a [Legújabb Azure CLI telepítve](../xplat-cli-install.md) van a számítógépre, és erőforrás-kezelő mód (`azure config mode arm`).

A következő Azure CLI paranccsal a virtuális gép telepítsen újra:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Megjelenik a virtuális módosítás állapotának újratelepítés folyamatát előrehaladtával. A `PowerState` a virtuális gép Ugrás "futtatásának 'frissítése", "Kezdő", és végül 'fut' azon a folyamaton, egy új állomás újbóli előrehaladtával. Az erőforrás csoporton belül a VMs állapotának ellenőrzése:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Következő lépések
Ha a virtuális problémák merülnek fel, keressen [SSH kapcsolatok hibáinak elhárítása](virtual-machines-linux-troubleshoot-ssh-connection.md) vagy [részletes SSH hibaelhárításhoz keres útmutatást](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md)segítséget. Ha egy alkalmazást a virtuális nem fér hozzá, olvassa el [alkalmazás hibaelhárítási problémák](virtual-machines-linux-troubleshoot-app-connection.md).