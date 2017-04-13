<properties
   pageTitle="Virtuális hálózat létrehozása az Azure klasszikus portálon webhely átjáró virtuális Magánhálózati kapcsolatot |} Microsoft Azure"
   description="Hozzon létre egy VNet S2S virtuális Magánhálózati átjáró kapcsolatot vonatkozó határokon helyszíni és hibrid konfigurációk klasszikus telepítési modellt használja."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Hozzon létre egy VNet az Azure klasszikus portálon hely közötti kapcsolat

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasszikus – klasszikus portál](vpn-gateway-site-to-site-create.md)

Ez a cikk végigvezeti virtuális hálózat és a webhely virtuális Magánhálózati átjáró kapcsolat a helyszíni hálózaton a klasszikus telepítési modell és a klasszikus portál használatával hozhat létre. Hely közötti kapcsolatok használható határokon helyszíni és hibrid konfigurációk.

![Webhely diagram] (./media/vpn-gateway-site-to-site-create/site2site.png "webhely-webhely")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Telepítési modellek és a hely közötti kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

A következő táblázat mutatja a jelenleg elérhető telepítési modellek és a webhely konfigurációk módszereket. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>További beállítások 

Ha azt szeretné, VNets összekapcsolása, akkor olvassa el [a klasszikus telepítési modell VNet-VNet kapcsolat konfigurálása](virtual-networks-configure-vnet-to-vnet-connection.md). Szeretne hely közötti kapcsolat hozzáadása egy VNet, amely már rendelkezik egy kapcsolatot, lásd: [a meglévő VPN-átjáró kapcsolattal VNet S2S kapcsolat](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Első lépések

Ellenőrizze, hogy rendelkezik-e az alábbi elemek konfigurációs megkezdése előtt.

- Kompatibilis VPN eszköz és olyannak, aki tudja állítani. Című témakörben [olvashat a virtuális Magánhálózati eszközöket](vpn-gateway-about-vpn-devices.md). Ha nem ismeri a virtuális Magánhálózati eszköze konfigurálása, vagy nem ismeri a tartományokat található IP-címét a helyszíni hálózati konfigurációja, a koordinálása másokkal, akik nyújtanak a adatai jelennek meg a szükséges.

- Külső felhasználókkal szemben lévő nyilvános IP-címének a virtuális Magánhálózati eszközt. Az IP-cím nem található meg az mögött egy hálózati címfordítást.

- Egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>A virtuális hálózat létrehozása

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com/).

2. A képernyő bal alsó sarkában kattintson az **Új**gombra. A navigációs ablakban kattintson a **Hálózati szolgáltatásokkal**, és válassza a **Virtuális hálózat**. Kattintson az **Egyéni létrehozása** a konfigurálása varázsló elindításához.

3. A VNet létrehozásához adja meg a megadott beállításokat az alábbi lapokon:

## <a name="Details"></a>Virtuális hálózati Részletek lap

Írja be az alábbi adatokat:

- **Név**: nevezze el a virtuális hálózatot. Ha például *EastUSVNet*. A virtuális hálózat nevét kell használni, amikor rendszerbe állítják a VMs és PaaS-példányok, így előfordulhat, hogy nem szeretné, hogy túl bonyolult nevét.
- **Hely**: A hely közvetlenül kapcsolatban áll a fizikai helyét (régió) az erőforrások (VMs) találhatók. Például ha azt szeretné, hogy a VMs, amely a virtuális hálózati fizikailag *Kelet-*Amerikai Egyesült Államokban található rendszerbe, jelölje ki azt a helyet. A régió virtuális hálózatához kapcsolódó létrehozásuk után nem módosítható.

## <a name="DNS"></a>A DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat lapon

Írja be az alábbi adatokat, és kattintson a jobb alsó sarkában lévő következő nyílra.

- **A DNS-kiszolgálók**: írja be a DNS-kiszolgáló neve és IP-cím vagy a helyi menüből válassza ki a korábban már regisztrált a DNS-kiszolgáló. Ez a beállítás nem hoz létre a DNS-kiszolgáló. Lehetővé teszi, hogy adja meg a DNS-kiszolgálók névfeloldás a virtuális hálózathoz használni kívánt.
- **Állítsa be a webhely VPN**: jelölje be az **beállítása webhely virtuális Magánhálózattal**.
- **Helyi hálózaton**: helyi hálózaton a fizikai helyszíni helyét jelöli. Bejelölheti a korábban létrehozott helyi hálózaton, vagy hozhat létre új helyi hálózaton. Ha bejelöli a korábban létrehozott helyi hálózatot használ, nyissa meg a **Helyi hálózat** konfigurálása lapra, és ellenőrizze, hogy a virtuális Magánhálózati eszköz IP-címe (nyilvános szemben lévő IPv4-cím) a virtuális Magánhálózati eszköz pontos.

## <a name="Connectivity"></a>Webhely-webhely kezelése lap

Ha egy új helyi hálózaton készít, megjelenik a **Webhely-webhely kezelése** lap. Ha szeretne, amely a korábban létrehozott helyi hálózatot használ, ezen az oldalon nem fognak megjelenni a varázsló, és az alábbi szakasszal áthelyezhetők.

Írja be az alábbi adatokat, és kattintson a következő mutató nyílra.

-   **Név**: A név fel szeretne hívni a helyi (helyszíni) hálózati hely.
-   **Virtuális Magánhálózati eszköz IP-cím**: a helyszíni VPN eszköz Azure csatlakozhat használt nyilvános szemben lévő IPv4-címét. A virtuális Magánhálózati eszköz nem található meg az mögött egy hálózati címfordítást.
-   **Címterület**: IP- és a (cím száma) CIDR kezdési tartalmazza. Megadhatja a cím range(s), hogy meg szeretné küldeni a virtuális hálózati átjáró a helyszíni helyi helyre keresztül. Ha a cél IP-címét az itt megadott tartományba eső esik, továbbítás a virtuális hálózati átjáró keresztül.
-   **Hozzáadás címterület**: Ha több címtartományok, amelyet a virtuális hálózati átjáró keresztül küldött, adja meg az egyes további címtartományokat. Felvehet, és később a **Helyi hálózaton** lapon a tartomány eltávolítása.

## <a name="Address"></a>Virtuális hálózati cím szóközöket lapon

Adja meg a virtuális hálózathoz használni kívánt címet tartományt. Ezek a dinamikus IP-címek (IMMERZIÓBAN), amely a VMs és a többi szerepkör példányát, amely a virtuális hálózati rendszerbe fog kiosztani.

Különösen fontos, hogy a helyszíni hálózaton használt tartományok bármelyikével nem áll átfedésben tartomány kijelölése. A hálózati rendszergazda koordinálása szüksége. A hálózati rendszergazda meg egy a helyszíni hálózaton cím adhatja meg a virtuális hálózathoz használni az IP-címeket carve szükséges.

Írja be az alábbi adatokat, és kattintson a jobb alsó sarkában a hálózat beállítása a bejelölést.

- **Címterület**: IP- és a cím darab kezdési tartalmazza. Győződjön meg arról, hogy a megadott cím szóközöket nem áll átfedésben bármelyik, hogy a helyszíni hálózaton van a cím szóközöket.
- **Hozzáadás alhálózat**: olyan kezdő IP- és a cím szám. További alhálózathoz nem szükséges, de érdemes hozzon létre külön alhálózathoz VMs, akihez statikus IMMERZIÓBAN. Vagy van-e a VMs alhálózat különböző nézeten a többi szerepkör futó példányát, érdemes lehet.
- **Hozzáadás átjáró alhálózat**: az átjáró alhálózat beírásához kattintson ide. Az átjáró alhálózat csak a virtuális hálózati átjáró szolgál, és a fenti konfiguráció szükséges.

Kattintson a lap alján kattintson a be van jelölve, és meg is kezdi a virtuális hálózat létrehozása. Ha elkészült, látni fogja **létrehozva** megjelenik **az állapot** csoportban a **hálózatok** lapon az Azure klasszikus portálon. A VNet létrehozását követően kattintson a virtuális hálózati átjáró beállíthatja.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>A virtuális hálózati átjáró beállítása

Állítsa be a virtuális hálózati átjáró biztonságos hely közötti kapcsolat létrehozásához. [A virtuális hálózati átjáró az Azure klasszikus portálon konfigurálása](vpn-gateway-configure-vpn-gateway-mp.md)című témakört.

## <a name="next-steps"></a>Következő lépések

A kapcsolat elkészülte virtuális gépeken futó hozzáadhatja a virtuális hálózatok. Használatáról [virtuális gépeken futó](https://azure.microsoft.com/documentation/services/virtual-machines/) talál további információt.
