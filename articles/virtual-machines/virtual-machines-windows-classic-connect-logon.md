<properties
    pageTitle="Jelentkezzen be a klasszikus Azure VM |}] A Microsoft Azure"
    description="Az Azure klasszikus portal segítségével jelentkezzen be a Windows klasszikus telepítési modell létrehozott virtuális géphez."
    services="virtual-machines-windows"
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
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Jelentkezzen be a Windows virtuális gép használata a klasszikus Azure portálon

A klasszikus Azure portálon a **Csatlakozás** gombra a távoli asztali munkamenet elindításához, és jelentkezzen be a Windows virtuális gép használja.

Szeretne csatlakozni a Linux VM? Lásd: [Jelentkezzen be a Linux operációs rendszert futtató virtuális gép](virtual-machines-linux-mac-create-ssh-keys.md).

Tájékoztatás arról, hogyan lehet [a következő lépések segítségével új Azure portál](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Videó forgatókönyv

Itt van egy videó forgatókönyv lépéseit a tankönyv. Végpontok és a Windows VM Azure való csatlakozáshoz használt nyilvános és személyes portok is kiterjed.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Csatlakozás a virtuális géphez

1. Jelentkezzen be a klasszikus Azure portálon.

2. Kattintson a **virtuális gépek**, és válassza ki a virtuális gép.

3. A lap alján a parancssávon kattintson a **Csatlakozás**gombra.

    ![Jelentkezzen be a virtuális gép](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] A **Csatlakozás** gomb nem érhető el, ha a cikk végén a hibaelhárítási tippeket talál

## <a name="log-on-to-the-virtual-machine"></a>Jelentkezzen be a virtuális gép

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>A következő lépések

-   Ha a **Csatlakozás** gomb nem aktív, vagy a távoli asztali kapcsolat más problémák merülnek, próbálja meg a konfiguráció alaphelyzetbe állítása. A virtuális gép irányítópult, a **Gyors áttekintése**, kattintson a **távoli konfiguráció alaphelyzetbe állítása**.
-   Problémák a jelszavát próbáljon beállítása. A virtuális gép irányítópult, a **Gyors áttekintése**, kattintson a **jelszó alaphelyzetbe állítása**.

Ha ilyen ötletek nem működnek, vagy nem, meg kell, lásd: [Hibaelhárítás távoli asztali kapcsolatok a Windows-alapú Azure virtuális gép](virtual-machines-windows-troubleshoot-rdp-connection.md). Ez a cikk ismerteti a keresztül közös problémák kiderítésében és elhárításában.


