<properties 
   pageTitle="Statikus magánjellegű IP-cím beállíthatja az Azure portálon ARM módban |} Microsoft Azure"
   description="Magánjellegű IP-címei (immerzióban), és hogyan kezelhetők az Azure portálon ARM módban ismertetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Hogyan kell beállítani egy privát statikus IP-címet az Azure-portálon

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Kezelheti [a klasszikus telepítési modell statikus magánjellegű IP-címet](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Az alábbi lépéseket a minta a már létrehozott egy egyszerű környezet várnak. Ha a használni kívánt a lépéseket a jelen dokumentum megjelenített, először összeállítása a [Hozzon létre egy vnet](virtual-networks-create-vnet-arm-pportal.md)ismertetett tesztkörnyezetben.

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Hogyan hozhat létre egy virtuális statikus saját IP-címek teszteléshez

Az erőforrás-kezelő telepítési módban egy virtuális kibocsátása során egy privát statikus IP-címet az Azure portál használatával nem lehet beállítani. A virtuális először kell létrehoznia, illeszti be saját személyes, statikus IP.

A virtuális *DNS01* nevű fájlt, egy névvel ellátott *TestVNet*VNet a *FrontEnd* alhálózat létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson az **Új** > **kiszámítania** > **Windows Server 2012 R2 adatközponthoz**, akkor **Válassza ki a telepítési modell** listában már látható az **Erőforrás-kezelő**, és kattintson a **Létrehozás**, az alábbi ábrán látható módon.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. Az **alapvető tudnivalók** lap adja meg a nevét a virtuális a létrehozandó (*DNS01* a helyzetben), a helyi rendszergazdafiók, és a jelszavát, az alábbi ábrán látható módon.

    ![Alapvető tudnivalók lap](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Ellenőrizze, hogy a kiválasztott **hely** *Központi Amerikai Egyesült Államok*, majd a **erőforráscsoport**, kattintson a **Select meglévő** , majd kattintson ismét az **erőforráscsoport** , majd *TestRG*kattintson és kattintson **az OK**gombra.

    ![Alapvető tudnivalók lap](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. A **Válassza ki a méretet** a lap válassza ki az **A1 szabványos**, és válassza a **Jelölje ki**.

    ![Válassza ki a Méret lap](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. A **Beállítások** lap, győződjön meg arról, a következő tulajdonságokat állít be vannak beállítva, az alábbi értékeket, és kattintson **az OK**gombra.

    -**Tárterület-fiók**: *vnetstorage*
    - **Hálózati**: *TestVNet*
    - **Alhálózat**: *FrontEnd*

    ![Válassza ki a Méret lap](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. Az **összefoglaló** lap kattintson **az OK gombra**. Figyelje meg az irányítópult jelenik meg az alábbi csempére.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan statikus magánjellegű IP-cím adatait egy virtuális beolvasása

Statikus magánjellegű IP-cím információit a virtuális létrehozása a fenti lépések megtekintése, hajtsa végre az alábbi lépéseket.

1. Az Azure Azure portálról, kattintson **Az összes BÖNGÉSZÉSE** > **virtuális gépeken futó** > **DNS01** > **minden elérhető beállítás** > **hálózati kapcsolatok** , és válassza a csak hálózati kapcsolaton szerepel a listában.

    ![Virtuális csempe üzembe helyezése](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. A **hálózati kapcsolat** lap, kattintson az **összes** > **IP-címeket** , és figyelje meg a **hozzárendelés** és **IP-cím** értéket.

    ![Virtuális csempe üzembe helyezése](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>A magánjellegű statikus IP-cím felvétele egy meglévő virtuális
A fenti lépések alapján létre virtuális egy privát statikus IP-cím hozzáadásához kövesse az alábbi lépéseket:

1. Az **IP-címek** lap alább látható kattintson **statikus** **hozzárendelés**csoportban.
2. *192.168.1.101* **IP-cím**mezőbe írja be, és kattintson a **Mentés**gombra.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Ha gombra kattint, **Mentse** azt veszi észre, hogy a hozzárendelés továbbra is értéke **dinamikus**, az azt jelenti, hogy a beírt IP-cím már használatban van. Próbáljon meg egy másik IP-címet.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>A magánjellegű statikus IP-cím eltávolítása egy virtuális
Az előbb létrehozott virtuális a magánjellegű statikus IP-cím eltávolításához kövesse az alábbi lépést.
    
1. A fent látható **IP-címek** lap, a **hozzárendelés**csoportban kattintson a **dinamikus** , és kattintson a **Mentés**gombra.

## <a name="next-steps"></a>Következő lépések

- [Fenntartott nyilvános IP](virtual-networks-reserved-public-ip.md) -címekről bővebb ismertetőt.
- [Példány szintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -címekről bővebb ismertetőt.
- Nézze meg a [fenntartott IP REST API-khoz](https://msdn.microsoft.com/library/azure/dn722420.aspx).