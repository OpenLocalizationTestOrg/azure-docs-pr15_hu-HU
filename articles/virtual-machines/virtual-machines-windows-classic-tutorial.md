<properties
    pageTitle="Hozzon létre egy virtuális a klasszikus portálon |} Microsoft Azure"
    description="A Windows virtuális gép létrehozása az Azure klasszikus portálon."
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
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Windows operációs rendszert futtató az Azure klasszikus portálon virtuális gép létrehozása

> [AZURE.SELECTOR]
- [Azure klasszikus portál](virtual-machines-windows-classic-tutorial.md)
- [A PowerShell: Klasszikus telepítési](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ismerje meg, [ezeket a lépéseket az erőforrás-kezelő telepítési felhasználva hajtsa végre](virtual-machines-windows-hero-tutorial.md) az **Új Azure portál**. 

Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre az Azure virtuális gép (virtuális) az Azure klasszikus portált a Windows operációs rendszert futtató. A Windows Server kép használjuk szerepel példaként, de ez csak az Azure kínál sok képek közül. Figyelje meg, hogy a kép kívánt szövegeket függenek, az előfizetést. Például a Windows asztali képek feltétlenül vehető igénybe az MSDN webhelyen előfizetők számára.

Ez a szakasz bemutatja, hogyan az Azure klasszikus portált a **Gyűjtemény a** beállítás használatával a virtuális gép létrehozása. Ezt a lehetőséget nyújt további konfigurációs lehetőségek, mint a **Gyors létrehozása** lehetőséget. Például ha egy virtuális gép virtuális hálózathoz csatlakozni szeretne, kell adja meg a **Gyűjtemény a** beállítást.

[A saját](virtual-machines-windows-classic-createupload-vhd.md)képekkel VMs is létrehozhat. Ez, és más módszerekkel kapcsolatos további tudnivalókért olvassa el a [változatos módszerekkel, amelyekkel Windows virtuális gép létrehozása](virtual-machines-windows-creation-choices.md)című témakört.



## <a name="video-walkthrough"></a>Videó forgatókönyv

Az alábbiakban a áttekintése a következő ebben az oktatóanyagban.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>a virtuális gép létrehozása

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [Hozzon létre egy virtuális, az erőforrás-kezelő telepítési modellt használja](virtual-machines-windows-hero-tutorial.md) az új Azure-portálon. 

- Jelentkezzen be a virtuális gépen. Című cikkben olvashat [a Windows Server operációs rendszert futtató virtuális géphez jelentkezzen be](virtual-machines-windows-classic-connect-logon.md).

- Lemezen tárolt adatok csatolása. Üres lemez és a lemezt, adatokat tartalmazó csatolhat. Című cikkben olvashat a [lemezen a klasszikus telepítési modell készült Windows virtuális géphez adatok csatolása](virtual-machines-windows-classic-attach-disk.md).
