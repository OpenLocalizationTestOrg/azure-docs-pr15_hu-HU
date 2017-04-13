<properties
    pageTitle="Mik azok a virtuális skála állítja be? | Microsoft Azure"
    description="Tudjon meg többet a virtuális skála beállítása."
    keywords="Linux virtuális gép, virtuális gép skála állítja be." 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Mik azok a virtuális gép skála állítja be?

Virtuális gép skála halmazok kezelése több VMs tárolt teszi lehetővé. Magas szintű skála halmazhoz a következő előnyei és hátrányai:

Szakemberek számára:

1. Magas elérhető. Minden skála megadása a VMs helyezi az 5 hibafa tartományok (FDs) és az 5 frissítés tartományok (UDs) állásának egy elérhetőségének beállítása (a további információt a FDs vagy UDs, olvassa el a [virtuális elérhetőség](./virtual-machines-linux-manage-availability.md)). 
2. Egyszerű integráció a Azure terheléselosztó és a alkalmazás átjáró.
3. Egyszerű integráció a Azure Automatikus méretezéssel.
4. Egyszerűsített példányban-kezelés, és a VMs jelenjenek meg.
5. Közös Windows és Linux változatok, valamint az egyéni képek támogatja.

Negatívumokat:

1. Adatok lemezre nem tud csatlakozni a skála megadása a virtuális példányok. Ehelyett kell használnia Blob-tárolóhoz, Azure fájlokat, Azure táblák vagy más tárolási megoldás.

## <a name="quick-create-using-azure-cli"></a>Gyors létrehozása Azure CLI használatával

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Következő lépések

Általános információt jelölje ki a [Méretezés eredménye a fő céloldal](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

További dokumentációt tanulmányozza a [fő dokumentáció lap méretezés állítja be](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Az erőforrás-kezelő sablonok használatával skála készleteket keres az [Azure quickstart útmutató sablonok github repó](https://github.com/Azure/azure-quickstart-templates)"vmss".

