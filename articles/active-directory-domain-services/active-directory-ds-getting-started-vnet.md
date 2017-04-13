<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Hozzon létre, vagy jelöljön ki egy virtuális hálózatot |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások – első lépések"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Hozzon létre, vagy jelöljön ki egy virtuális hálózatot az Azure Active Directory tartományi szolgáltatások

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Jelölje ki az Azure virtuális hálózati útmutató
> [AZURE.NOTE] **Első lépések**: [szempontjait az Azure Active Directory tartományi szolgáltatások hálózati](active-directory-ds-networking.md)hivatkozik.


## <a name="task-2-create-an-azure-virtual-network"></a>2 feladat: Hozzon létre egy Azure virtuális hálózat
A következő konfigurációs feladat létrehozása az Azure virtuális hálózat és a benne alhálózat. Az alhálózathoz az Azure Active Directory tartományi szolgáltatások engedélyezése a virtuális hálózaton belül. Ha már van egy meglévő virtuális hálózat szeretné használni, kihagyhatja ezt a lépést.

> [AZURE.NOTE] Győződjön meg arról, hogy az Azure virtuális hálózati hoz létre, vagy válassza az Azure Active Directory tartományi szolgáltatások használatára tartozik egy Azure terület Azure Active Directory tartományi szolgáltatások által támogatott. Lásd: a [régió szerint Azure szolgáltatások](https://azure.microsoft.com/regions/#services/) lap tudnivalók a Azure régiók, amelyben Azure Active Directory tartományi szolgáltatások érhető el.

Megjegyzés: lefelé a virtuális hálózat nevét, akkor jelölje be a jobbra virtuális hálózat, ha egy további beállítási lépést az Azure Active Directory tartományi szolgáltatások engedélyezése.

Hajtsa végre az alábbi konfigurálási lépéseket létrehozása az Azure virtuális hálózat, ahol szeretné, hogy engedélyezze az Azure Active Directory tartományi szolgáltatások.

1. Nyissa meg az **Azure klasszikus portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Jelölje ki a bal oldali ablaktáblában a **hálózatok** csomópontot.

    ![Csomópont hálózatok](./media/active-directory-domain-services-getting-started/networks-node.png)

3. A munkaablak, a lap alján kattintson az **Új** gombra.

    ![Virtuális hálózatok csomópontot.](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. A **Hálózati szolgáltatások** csomópontot jelölje ki a **Virtuális hálózat**.

5. Kattintson a **Gyors létrehozása** egy virtuális hálózat létrehozása gombra.

    ![Virtuális hálózat - gyors létrehozása](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Adja meg a virtuális hálózat **nevét** . Dönthet úgy is, a **Címterület** vagy a **virtuális maximális száma** beállítása ehhez a hálózathoz. Most, hagyja a **DNS-kiszolgáló** beállítást, "Nincs" értékre van állítva. Frissítheti a DNS-kiszolgáló szolgáltatások az Azure Active Directory tartományi engedélyezése után beállítást.

7. Győződjön meg arról, hogy egy támogatott Azure régió a **hely** legördülő jelölje ki. Lásd: a [régió szerint Azure szolgáltatások](https://azure.microsoft.com/regions/#services/) lap tudnivalók a Azure régiók, amelyben Azure Active Directory tartományi szolgáltatások érhető el.

8. A virtuális hálózat létrehozásához a **virtuális hálózat létrehozása** gombra.

    ![Hozzon létre egy virtuális hálózati Azure Active Directory tartományi szolgáltatások.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. A virtuális hálózat létrehozását követően jelölje ki a virtuális hálózatot, és kattintson a **beállítás** fülre.

    ![Alhálózat létrehozása](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Nyissa meg a **virtuális hálózati cím szóközöket** szakaszt. Kattintson a **alhálózat hozzáadása** gombra, és adja meg a alhálózat **AaddsSubnet**nevű. Az alhálózathoz létrehozása a **Mentés** gombra.

    ![Alhálózat létrehozása az Azure Active Directory tartományi szolgáltatások.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Tevékenység-3 - engedélyezése Azure Active Directory tartományi szolgáltatások
Ahhoz, hogy [Azure Active Directory tartományi szolgáltatások](active-directory-ds-getting-started-enableaadds.md), akkor a következő konfigurációs feladatot.
