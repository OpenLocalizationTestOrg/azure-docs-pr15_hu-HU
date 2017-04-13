<properties
   pageTitle="A klasszikus portálon készült ExpressRoute egy virtuális hálózati és az átjáró beállítása |} Microsoft Azure"
   description="Ez a cikk végigvezeti a klasszikus telepítési modell és a klasszikus portál használata készült ExpressRoute virtuális hálózat beállítása."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>A klasszikus portál létrehozása egy virtuális hálózati készült ExpressRoute

Ez a cikk lépéseinek végigvezeti készült ExpressRoute a klasszikus telepítési modell és a klasszikus portal segítségével virtuális hálózatot, és egy virtuális hálózati átjáró konfigurálása.

Az erőforrás-kezelő telepítési modell utasításokat keresi, ha is használhatja az alábbi cikkekben: [PowerShell használatával virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-ps.md) és [hozzáadása egy erőforrás-kezelő VNet számára készült ExpressRoute átjáró VPN](expressroute-howto-add-gateway-resource-manager.md).

**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>A klasszikus VNet és az átjáró létrehozása

Az alábbi lépéseket a klasszikus VNet és a virtuális hálózati átjáró létrehozása Ha már van egy klasszikus VNet, című [beállítása egy meglévő klasszikus VNet](#config) ebben a cikkben.

1. Jelentkezzen be az [Azure klasszikus portálon](http://manage.windowsazure.com).

2. A képernyő bal alsó sarkában kattintson az **Új**gombra. A navigációs ablakban kattintson a **Hálózati szolgáltatásokkal**, és válassza a **Virtuális hálózat**. Kattintson az **Egyéni létrehozása** a konfigurálása varázsló indításához gombra.

3. A **Virtuális hálózati részletek** lapon adja meg az alábbiakat:

    - **Név** – a virtuális hálózat nevét. A virtuális hálózat nevét kell használni, amikor rendszerbe állítják a VMs és PaaS-példányok, így előfordulhat, hogy nem szeretné, hogy túl bonyolult nevét.
    - A **hely** , a hely közvetlenül kapcsolatban áll a fizikai helyét (régió) az erőforrások (VMs) találhatók. Például ha azt szeretné, hogy a VMs, amely a virtuális hálózati fizikailag kelet-Amerikai Egyesült Államokban található rendszerbe, jelölje ki azt a helyet. A régió virtuális hálózatához kapcsolódó létrehozásuk után nem módosítható.

4. A **DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat** lapon írja be az alábbi adatokat, és kattintson a jobb alsó sarkában lévő következő nyílra. 

    - **A DNS-kiszolgálók** - adja meg a DNS-kiszolgáló neve és IP-címet, vagy a helyi menüből válassza ki a korábban már regisztrált a DNS-kiszolgáló. Ez a beállítás nem hoz létre a DNS-kiszolgáló. Lehetővé teszi, hogy adja meg a DNS-kiszolgálók névfeloldás a virtuális hálózathoz használni kívánt.
    - **Webhely kapcsolat** - jelölje ki a jelölőnégyzet a **konfigurálása webhely virtuális Magánhálózattal**.
    - **Készült ExpressRoute** – jelölje ki a jelölőnégyzet **Használata készült ExpressRoute**. Ez a beállítás csak akkor jelenik meg, ha **beállítása webhely VPN**kijelölt.
    - A **Helyi hálózaton** – akkor van szüksége a helyi hálózaton webhely számára készült ExpressRoute. Azonban készült ExpressRoute forrásból származó, amíg a cím prefixumokban a helyi hálózati hely megadott figyelmen kívül hagyja. Ehelyett a cím prefixumokban a Microsoftnak a készült ExpressRoute áramkör keresztül közzététel útválasztási célokra lesz.<BR>Ha már van egy helyi hálózaton a készült ExpressRoute kapcsolatot létrehozni, akkor jelölje ki a legördülő listából. Ha nem, jelölje be az **Új helyi hálózat megadása**.

5. A **webhely-webhely kezelése** lap jelenik meg, ha azt választotta, hogy az előző lépésben adjon meg egy új helyi hálózathoz. Adja meg a helyi hálózaton, írja be az alábbi adatokat, majd a következő mutató nyílra. 

    - A hálózati webhely **neve** - fel szeretne hívni a helyi (helyszíni) nevét.
    - **Címterület** – beleértve a kezdő IP- és CIDR (cím száma). Megadhatja a bármely címtartományokat mindaddig, amíg ki nem az a cím tartomány virtuális hálózatához átfedik egymást. Általában ennek a címtartományokat a helyszíni hálózatok határozza meg, de készült ExpressRoute, amíg nem használják ezeket a beállításokat. Ez a beállítás azonban a helyi hálózaton létrehozásához, a klasszikus portál használata közben van szükség.
    - Nincs készült ExpressRoute vonatkozó **hozzáadása címterület** – ezt a beállítást.


6. A **Virtuális hálózati cím szóközöket** lapon írja be az alábbi adatokat, és kattintson a a jobb alsó sarkában a hálózat beállítása a bejelölést. 

    - **Címterület** – többek között az IP- és a cím darab kezdve. Győződjön meg arról, hogy a megadott cím szóközöket nem áll átfedésben bármelyik, amely a helyi hálózaton van a cím szóközöket.
    - **Hozzáadás alhálózat** – többek között a kezdő IP- és a cím szám. További alhálózathoz nem szükségesek.
    - **Hozzáadás átjáró alhálózat** - az átjáró alhálózat beírásához kattintson ide. Az átjáró alhálózat csak a virtuális hálózati átjáró szolgál, és a fenti konfiguráció szükséges.<BR>CIDR (cím száma) számára készült ExpressRoute átjáró alhálózat /28 kell lennie vagy nagyobb (27, / / 26 stb.). Ez lehetővé teszi az adott alhálózat elég IP-címek engedélyezése a konfiguráció használata. A klasszikus portálon Ha be van jelölve a jelölőnégyzet bejelölésével készült ExpressRoute, használja a portálon megadja átjáró alhálózat /28.  A klasszikus portál címe CIDR számának nem módosítható. Az átjáró alhálózat megjelennek **átjáró** a klasszikus portálon ténylegesen **GatewaySubnet**ugyan az átjáró alhálózat létrehozott valós nevét. Ez a név megtekintheti a PowerShell használatával vagy az Azure-portálon.

7. Kattintson a lap alján kattintson a be van jelölve, és meg is kezdi a virtuális hálózat létrehozása. Ha elkészült, látni fogja **létrehozva** megjelenik **az állapot** csoportban a **hálózatok** lapon a klasszikus portálon.

## <a name="gw"></a>Az átjáró létrehozása

1. A **hálózatok** lapon kattintson az imént létrehozott virtuális hálózati, majd kattintson az **Irányítópult** elemre a lap tetején.

2. Az **Irányítópult** lap alján kattintson az **Átjáró létrehozása** , és válassza a **Dinamikus útválasztás**. **Válassza az Igen lehetőséget** választva erősítse meg, hogy az átjáró létrehozása gombra.

3. Indításakor az átjáró létrehozása, megjelenik egy üzenet bérbeadására tudja, hogy az átjáró el van indítva. Az átjáró létrehozásához 45 perc is eltarthat.

4. A hálózat hivatkozás egy áramkör. Kövesse az [útmutatást követve összekapcsolhat VNets készült ExpressRoute áramkörök a](expressroute-howto-linkvnet-classic.md)cikkben utasításokat.

## <a name="config"></a>Egy meglévő klasszikus VNet beállítása készült ExpressRoute

Ha már van egy klasszikus VNet, beállíthatja, hogy a klasszikus portálon készült ExpressRoute csatlakozni. A beállítások ugyanazok, mint a szakaszok a fenti, olvassa el a szakaszokat hogy megismerje a szükséges beállításokat. Amennyiben szeretne coexisting készült ExpressRoute /-webhelyek-kapcsolat létrehozása, olvassa el [Ez a cikk](expressroute-howto-coexist-classic.md) lépései. Elérik ugyanaz, mint az ebben a cikkben leírt lépéseket.
 
1. A helyi hálózaton létrehozásához, a többi a VNet beállításainak frissítése előtt szükséges. Hozzon létre új helyi hálózaton, amelynek van szükség a klasszikus portálon keresztül készült ExpressRoute konfigurálásakor, kattintson az **Új** **>** **Hálózati szolgáltatások** **>** **Virtuális hálózati** **>** **hozzáadása a helyi hálózaton**. Kövesse a varázsló lépéseit, a helyi hálózaton létrehozásához.

2. Ezen a **konfigurálása** lapon, a többi a VNet beállításainak frissítheti és a helyi hálózathoz a VNet társítani.

3. A beállításainak konfigurálása után ugorjon a jelen cikk az átjáró létrehozása [az átjáró létrehozása](#gw) című.


## <a name="next-steps"></a>Következő lépések

- Ha virtuális gépeken futó hozzáadása a virtuális hálózat, olvassa el a [virtuális gépeken futó tanulási javaslatok](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)című témakört.
- Ha azt szeretné, ha többet szeretne tudni a készült ExpressRoute, a a [Készült ExpressRoute – áttekintés](expressroute-introduction.md)című témakörben találhat.


 
