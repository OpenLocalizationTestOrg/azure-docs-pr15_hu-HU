<properties
    pageTitle="Lemezen csatolása egy virtuális |} Microsoft Azure"
    description="A klasszikus telepítési modell készült Windows virtuális gép lemezen adatok csatolása és inicializálni azt."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>A klasszikus telepítési modell készült Windows virtuális gép lemezen adatok csatolása

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ha azt szeretné, az új portál című témakörben olvashat, [hogyan csatolhat egy Windows virtuális az Azure-portálon adatok lemezre](virtual-machines-windows-attach-disk-portal.md).

Ha szüksége van egy további adatokat lemezre, akkor csatolhat egy üres vagy adatokat tartalmazó egy meglévő lemezről egy virtuális. Mindkét esetben a lemezt a Azure tároló fiók tárolnak .vhd fájlok. Az új lemezen, esetében után a lemezt, csatol is kell inicializálni azt, hogy készen áll a használja-e egy Windows virtuális.

Lemezen kapcsolatos részletekért lásd: [A lemez és a virtuális gépeken futó VHD](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>A lemez inicializálni

1. Csatlakozás a virtuális gépen. Című cikkben olvashat [a Windows Server operációs rendszert futtató virtuális géphez bejelentkezés][logon].

2. A bejelentkezés után a virtuális géphez, nyissa meg a **Kiszolgálókezelő**. A bal oldali ablaktáblában válassza a **fájl- és tárolási szolgáltatásokhoz**.

    ![Nyissa meg a Kiszolgálókezelő](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Bontsa ki a menüt, és válassza ki a **lemezen**.

4. A **lemezt** a szakasz a lemez. A legtöbb esetben 0, 1 lemezre, és lemezzel 2 fog rendelkezni. 0 lemez az operációs rendszer lemez lemez az ideiglenes lemez 1 és 2 lemez az adatok lemez csak csatolva a virtuális. Az új adatok lemezről felsorolásban a partíciók, **Ismeretlen**. Kattintson a jobb gombbal a lemez, és válassza a **inicializálni**.

5.  Értesítést kap, hogy az összes adatok törlődnek, amikor a lemez inicializált van. Kattintson az **Igen** gombra a figyelmeztető üzenet nyugtázza és a lemez inicializálni. Miután elkészült, a Partion **GPT**fog szerepelni. Kattintson ismét a jobb gombbal a lemez, és válassza az **Új mennyiségi**.

6.  A varázsló segítségével az alapértelmezett értékeket. A varázsló befejezése után a **kötet** szakasz az új kötet. A lemez ettől kezdve készen áll a adattárolásra.

    ![Mennyiségi sikerült inicializálni](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] A virtuális méretének határozza meg, hogy hány lemezt, akkor is csatolhat. Részletekért olvassa el [a virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>További források

[Hogyan kell a Windows virtuális gép lemezen leválasztása](virtual-machines-windows-classic-detach-disk.md)

[Tudnivalók a lemez és VHD a virtuális gépeken futó](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
