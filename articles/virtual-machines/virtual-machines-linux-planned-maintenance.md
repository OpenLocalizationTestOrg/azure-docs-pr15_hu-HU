<properties
    pageTitle="Tervezett karbantartás Linux VMs |} Microsoft Azure"
    description="Milyen Azure tervezett karbantartás van, és milyen hatással van a Linux virtuális gépeken futó operációs rendszert futtató Azure-ban ismertetése"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-linux-virtual-machines-in-azure"></a>Tervezett karbantartás Linux virtuális gépeken futó Azure-ban

Ismerje meg, milyen Azure tervezett karbantartás, és milyen hatással vannak a Linux virtuális gépeken futó elérhetőségét. Ez a cikk a [Windows virtuális gépeken futó](virtual-machines-windows-planned-maintenance.md)is érhető el. 

Ebben a cikkben az Azure tervezett karbantartás folyamat mód lehetőség használatával egy hátteret. Ha kívánó kapcsolatos hibák elhárítása az oka annak, hogy a virtuális indítani, [részletező virtuális megtekintése bejegyzés indítsa újra a blogjai naplók](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/)is.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Miért Azure hajtja végre a tervezett karbantartás

Microsoft Azure frissítéseket rendszeresen át a földgömb, a megbízhatóság, a teljesítmény és a biztonsági a host infrastruktúrájára alapjául szolgáló virtuális gépeken futó javítására hajt végre. A frissítések számos bármely hatása a virtuális gépeken futó vagy felhőalapú szolgáltatást, többek között a memória megőrzése frissítések nélkül hajt végre.

Azonban egyes frissítések indítani a számítógépet a virtuális gépeken futó infrastruktúra szükséges frissítéseket alkalmazni szeretné. A virtuális gépeken futó állítsa le a mialatt azt javítás infrastruktúra, majd a virtuális gépeken futó újraindítja.

Ne feledje, hogy vannak-e két típusa, amely hatással lehet a virtuális gépeken futó elérhetősége karbantartás: tervezett és a tervezett. Ez a lap azt ismerteti, hogyan végzi el a Microsoft Azure tervezett karbantartásokkal kapcsolatos információk. Tervezett karbantartás kapcsolatos további tudnivalókért olvassa el a [és a tervezett karbantartás tervezett ismertetése](virtual-machines-linux-manage-availability.md)című témakört.

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
