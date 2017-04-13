<properties 
   pageTitle="Statikus magánjellegű IP-cím beállíthatja az Azure-portálon klasszikus módban |} Microsoft Azure"
   description="Statikus magánjellegű IP-címei és hogyan kezelheti őket az Azure portálon Klasszikus módú ismertetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Hogyan kell beállítani egy privát statikus IP-cím (klasszikus), az Azure-portálon

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Kezelheti [az erőforrás-kezelő telepítési modell statikus magánjellegű IP-címet](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Az alábbi lépéseket a minta a már létrehozott egy egyszerű környezet várnak. Ha a használni kívánt a lépéseket a jelen dokumentum megjelenített, először összeállítása a [Hozzon létre egy vnet](virtual-networks-create-vnet-classic-pportal.md)ismertetett tesztkörnyezetben.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>A magánjellegű statikus IP-cím megadása, egy virtuális létrehozásakor
A virtuális *DNS01* nevű fájlt, egy névvel ellátott *TestVNet* egy *192.168.1.101*magánjellegű statikus IP-VNet a *FrontEnd* alhálózat létrehozásához kövesse az alábbi lépéseket:

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson az **Új** > **kiszámítania** > **Windows Server 2012 R2 adatközponthoz**, akkor **Válassza ki a telepítési modell** listában már látható a **Klasszikus**, és kattintson a **Létrehozás**gombra.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. A **Virtuális létrehozása** lap adja meg a nevét a virtuális a létrehozandó (*DNS01* a helyzetben), a helyi rendszergazdafiók, és a jelszavát.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Kattintson a **Választható beállítási** > **hálózati** > **Virtuális hálózatot**, és kattintson a **TestVNet**. **TestVNet** nem érhető el, ha győződjön meg arról, hogy a *Központi USA -beli* hely használ, és ez a cikk elején leírt tesztkörnyezetben nem hozott létre.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. A **hálózati** lap a ellenőrizze, hogy az aktuálisan kijelölt alhálózat *FrontEnd*, majd kattintson az **IP-címek**, **IP-cím hozzárendelése** területen kattintson a **statikus**, és írja *192.168.1.101* **IP-** cím, alább látható módon.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. Az **IP-címek** a lap, kattintson az **OK gombra** , majd kattintson az **OK gombra** a **hálózati** lap, és kattintson az **OK gombra** a **választható beállítások** lap.
7. A **Virtuális létrehozása** a lap kattintson a **Létrehozás**gombra. Figyelje meg az irányítópult jelenik meg az alábbi csempére.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan statikus magánjellegű IP-cím adatait egy virtuális beolvasása

Statikus magánjellegű IP-cím információit a virtuális létrehozása a fenti lépések megtekintése, hajtsa végre az alábbi lépéseket.

1. Az Azure Azure portálról, kattintson **Az összes BÖNGÉSZÉSE** > **virtuális gépeken futó (klasszikus)** > **DNS01** > **minden elérhető beállítás** > **IP-címek** és az IP-cím hozzárendelése és IP-cím, ahogy alább látható értesítés.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>A magánjellegű statikus IP-cím eltávolítása egy virtuális
Az előbb létrehozott virtuális a magánjellegű statikus IP-cím eltávolításához kövesse az alábbi lépéseket.
    
1. A fent látható **IP-címek** lap, **dinamikus** jobbra lévő **IP-cím hozzárendelése**, kattintson a, majd kattintson a **Mentés**gombra, és kattintson az **Igen gombra**.

    ![Virtuális létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>A magánjellegű statikus IP-cím felvétele egy meglévő virtuális
A fenti lépések alapján létre virtuális egy privát statikus IP-cím hozzáadásához kövesse az alábbi lépéseket:

1. Az **IP-címek** lap alább látható kattintson a **statikus** **IP-cím hozzárendelése**jobbra.
2. Az **IP-cím**mezőbe írja be a *192.168.1.101* , majd kattintson a **Mentés**gombra, és kattintson az **Igen**gombra.

## <a name="next-steps"></a>Következő lépések

- [Fenntartott nyilvános IP](virtual-networks-reserved-public-ip.md) -címekről bővebb ismertetőt.
- [Példány szintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -címekről bővebb ismertetőt.
- Nézze meg a [fenntartott IP REST API-khoz](https://msdn.microsoft.com/library/azure/dn722420.aspx).