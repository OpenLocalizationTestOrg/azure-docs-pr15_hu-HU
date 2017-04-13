<properties
    pageTitle="Adatok lemezen csatolása egy Linux virtuális |} Microsoft Azure"
    description="Hogyan lehet új vagy meglévő adatok lemezre csatolása egy Linux virtuális az Azure-portálon használja, az erőforrás-kezelő telepítési modell."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Hogyan csatolhat adatok lemezen egy Linux virtuális az Azure-portálon

Ez a cikk bemutatja, hogyan csatolhat új és a meglévő Linux virtuális gép az Azure portálon keresztül. Is [a Windows virtuális az Azure-portálon adatok lemezre csatolása](virtual-machines-windows-attach-disk-portal.md). Mielőtt ezt megtette, olvassa el ezeket a lépéseket:

- A virtuális gép méretének csatolhat hány adatok lemezt szabályozza. Részletekért olvassa el [a virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).
- Prémium tárterületet használ, szüksége lesz egy Lépésben-sorozatot vagy GS-sorozatot virtuális számítógépre. Az alábbi virtuális gépeken futó lemezt prémium verzió és a normál tárterület-fiókból is használhatja. Prémium tároló bizonyos régióban érhető el. A részletekért olvassa [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md).
- Lemezen csatolni virtuális gépeken futó Azure tároló fiók ténylegesen .vhd fájlok közül. További információ című témakörben [olvashat a lemez és a virtuális gépeken futó VHD](virtual-machines-linux-about-disks-vhds.md).
- Új lemezen nem kell létrehozni a dokumentumot elsőként Azure hoz létre, ha csatolja, mert.
- Egy meglévő lemez a .vhd fájl kell rendelkezésre állnia Azure tárterület-fiókjában. Egy .vhd már használhatja, ha nem csatlakozik egy másik virtuális gép vagy a saját .vhd a tárterület-fiókjába a fájl feltöltése.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Következő lépések

A lemezt a felvétele után szükség előkészítheti őket való használatra. Lásd: a virtuális gép operációs rendszer: "hogyan: Linux az új adatok lemezen inicializálni" Ez a [cikk](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
