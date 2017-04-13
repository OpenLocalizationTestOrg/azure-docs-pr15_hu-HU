<properties
   pageTitle="Hogyan méretezheti át egy Linux virtuális |} Microsoft Azure"
   description="Hogyan méretezheti, vagy Linux virtuális gép lefelé a virtuális méret módosításával méretezni."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Hogyan méretezheti át egy Linux virtuális

## <a name="overview"></a>– Áttekintés 

Miután, létrehozhatnak egy virtuális gép (virtuális), méretezheti a virtuális felfelé vagy lefelé a [virtuális méret]módosításával[vm-sizes]. Egyes esetekben kell először felszabadítása a virtuális. Ez akkor fordulhat elő, ha az új méretet a hardver fürt, amelyen a virtuális nem érhető el.

Ebből a cikkből megtudhatja, hogy hogyan méretezheti át a használja az [Azure CLI]Linux virtuális[azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.


## <a name="resize-a-linux-vm"></a>Egy Linux virtuális átméretezése 

A virtuális átméretezéséhez hajtsa végre az alábbi lépéseket.

1. A következő CLI parancsot. Ez a parancs megjeleníti a virtuális érhetők el a hardver fürthöz a hol helyezkedik el a virtuális betűméretet.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Ha a kívánt méretet megjelenik, méretezze át a virtuális a következő parancsot.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    A virtuális újraindul folyamat során. A újraindításkor a meglévő operációs rendszer és az adatok lemezt fog lehet újra társítani. Az ideiglenes lemezen bármi el fog veszni.

    Használja a `--enable-boot-diagnostics` beállítás lehetővé teszi, hogy az [indítási diagnosztika][boot-diagnostics], bármilyen indítási hibáinak bejelentkezni.

3. Ha a kívánt méretet nem szerepel, a következő parancsokat felszabadítása a virtuális, átméretezheti, és indítsa újra a virtuális.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] A virtuális rendelt dinamikus IP-címek is felszabadítása a virtuális elengedi. Az operációs rendszer és az adatok lemezt nem érinti.
   
## <a name="next-steps"></a>Következő lépések

További méretezhetőség több virtuális példánya futtatható és méretezése. További tudnivalókért lásd: a [virtuális gép skála megadása a Linux gépek automatikus méretezése][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md