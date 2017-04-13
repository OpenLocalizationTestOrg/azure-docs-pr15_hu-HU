<properties
   pageTitle="Hozzon létre a VNet Peering az Azure portálon |} Microsoft Azure"
   description="Megtudhatja, hogyan hozhat létre az erőforrás-kezelő az Azure portal segítségével virtuális hálózat."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Virtuális hálózati peering az Azure portálon létrehozása

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

VNet peering alapján a forgatókönyv feletti az Azure-portálra létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. VNET peering, meg kell hozzon létre egy minden irányban, két VNets között két hivatkozást. Először VNET peering hivatkozásra kattintva VNET2 VNET1 hozhat létre. A portálon kattintson a **Tallózás** > **Válassza ki a virtuális hálózatok**

    ![Az Azure-portálon peering VNet létrehozása](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. A virtuális hálózatok lap, VNET1, Peerings gombra, majd válassza a Hozzáadás gombra

    ![Válassza a peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. A Peering hozzáadása lap nevezze el peering hivatkozás LinkToVnet2, adja meg az előfizetést és a partner virtuális hálózati VNET2, kattintson az OK gombra.

    ![VNet mutató hivatkozás](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Miután létrejött a VNET peering hivatkozásra. Megjelenik a kapcsolat állapotát, a következőket:

    ![Kapcsolati állapot](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Ezután hozzon létre a VNET peering hivatkozására kattintva VNET1 VNET2. A virtuális hálózatok lap, VNET2, Peerings gombra, majd válassza a Hozzáadás gombra

    ![A többi VNet Peer](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Nevezze el peering hivatkozás LinkToVnet1 Peering hozzáadása lap, adja meg az előfizetést és a partner virtuális hálózati, kattintson az OK gombra.

    ![Virtuális hálózati csempe létrehozása](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Miután létrejött a VNET peering hivatkozásra. Megjelenik a kapcsolat állapotát, a következőket:

    ![Végleges kapcsolati állapot](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Az állapot ellenőrzése LinkToVnet2, és most változik Connected is.  

    ![Végleges kapcsolati állapot 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET peering csak létre, ha mindkét hivatkozások kapcsolódik.

Létezik néhány konfigurálható tulajdonságai az egyes hivatkozások:

|A beállítás|Leírás|Alapértelmezett|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|E a Peer VNet a Virtual_network címke része lesz a hely címe|igen|
|AllowForwardedTraffic|Egy peered VNet nem származó forgalom elfogadását vagy kihagyott e|nem|
|AllowGatewayTransit|Lehetővé teszi, hogy a partner VNet a VNet átjáró használata|nem|
|UseRemoteGateways|A partner VNet átjáró használata. A partner VNet rendelkeznie kell konfigurálni az átjárók és AllowGatewayTransit ki van jelölve. Ez a beállítás nem használható, ha van beállítva az átjárók|nem|

Az egyes hivatkozások VNet peering a fenti tulajdonságok csoportja tartalmaz. Portálról lehet a VNet Peering hivatkozásra, és módosítsa a rendelkezésre álló beállításokat, kattintson a Mentés gombra, hogy az effektus módosítása.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Ebben a példában fogjuk használni A két előfizetések és a B és a két felhasználók ' a ' felhasználó és biztosít az előfizetések jogokkal rendelkező kurzor
3. A portálon kattintson a Tallózás gombra, válassza ki a virtuális hálózatok. Kattintson a VNET, és kattintson a Hozzáadás gombra.

    ![2 forgatókönyv Tallózás](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Kattintson a Hozzáadás a adatelérési lap, jelölje ki azt a szerepkört, és válassza a hálózat közreműködői kattintson felhasználók hozzáadása, írja be a biztosít bejelentkezési nevét, és kattintson az OK gombra.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Ez nem követelmény, peering hozható létre, ha a felhasználóknak egyenként előléptetése peering kérelmek esetében a megfelelő Vnets, mindaddig, amíg a kérések megfelelően. A helyi VNet-felhasználók: a más VNet jogosultságokkal rendelkező felhasználó hozzáadása megkönnyíti a portál beállítása.

5. Majd jelentkezzen be az biztosít, aki a jogosultság felhasználótól SubscriptionB Azure portáljára. Kövesse a fenti lépéseket a hálózati közreműködői ' a ' felhasználó felvétele.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Jelentkezzen ki, és jelentkezzen be a két felhasználói munkamenetek annak érdekében, hogy az engedélyezés sikeresen engedélyezve van-e a böngészőben.

6. Jelentkezzen be a portálra Felhasználó_A, nyissa meg azt a VNET3 lap, kattintson a Peering, jelölje be "állapíthatom meg, hogy az erőforrás-azonosító" jelölőnégyzetet, és írja be az erőforrás azonosító számára a VNET5 formázása.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Erőforrás-azonosító](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Jelentkezzen be a portálon biztosít, és kövesse a fenti lépésben peering hivatkozás VNet3 VNET5 hozzon létre.

    ![Erőforrás-azonosító 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Peering jön létre, és bármelyik virtuális gép VNet3 tud kommunikálni a VNet5 virtuális gépi kell lennie.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Első lépésként, VNET peering hivatkozást, amely HubVnet VNET1. Figyelje meg, hogy továbbított forgalom engedélyezése beállítás nincs kiválasztva a hivatkozás.

    ![Egyszerű Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Következő lépésként peering hivatkozást, amely VNET1 HubVnet hozhat létre. Figyelje meg, hogy továbbítsák engedélyezése forgalom beállítás be van jelölve.

    ![Egyszerű Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Miután peering folyamatban van, olvassa el a [cikk](virtual-network-create-udr-arm-ps.md) , és megadása a felhasználó által definiált Route(UDR) VNet1 forgalom átirányítása egy virtuális készülék képességeit használandó keresztül. Útvonalat a következő ugrási címet ad meg, amikor azt is megadhatja ilyenkor a peer VNet HubVNet készülék virtuális IP-címére


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.

2. Ebben az esetben peering VNET, meg kell csak egy hivatkozást, létrehozása az Azure erőforrás-kezelőben klasszikus egy virtuális hálózatról. Ez azt jelenti, hogy a **VNET1** **VNET2**szeretne. A portálon kattintson a **Tallózás** > Válassza ki a **Virtuális hálózatok**

3. Válassza ki a virtuális hálózatok lap **VNET1**. Kattintson a **Peerings**, majd kattintson a **Hozzáadás**gombra.

4. A Peering hozzáadása lap nevezze el a hivatkozást. Itt **LinkToVNet2**nevezik. Peer a részletek csoportban jelölje be **a klasszikus**.

5. Válassza az előfizetés és a partner virtuális hálózati **VNET2**. Kattintson az OK gombra.

    ![Vnet 2 Vnet1 csatolása](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Amikor létrejött a VNet peering hivatkozásra, a két virtuális hálózat peered vannak, és láthatja a következő lesz:

    ![Peering kapcsolat ellenőrzése](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>VNet Peering eltávolítása

1.  Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2.  Virtuális hálózati lap megnyitásához, kattintson a Peerings, kattintson a hivatkozásra, amelyet el szeretne távolítani, kattintson a Törlés gombra.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Ha eltávolít egy hivatkozás VNET peering, a peer kapcsolati állapot ugrik leválasztott.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. Ezt az állapotot, nem lehet újra létrehozni a hivatkozást mindaddig, amíg a peer kapcsolati állapot kezdeményezett változik. Azt javasoljuk, hogy Ön hozza létre újból a VNET peering előtt távolítsa el a mindkét hivatkozások.
