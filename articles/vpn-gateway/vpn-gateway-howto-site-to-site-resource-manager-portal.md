<properties
   pageTitle="Virtuális hálózat létrehozása az Azure erőforrás-kezelő és az Azure-portálon webhely virtuális Magánhálózati kapcsolatot |} Microsoft Azure"
   description="Hogyan hozhat létre az erőforrás-kezelő telepítési modell VNet, és csatlakoztassa a helyi a helyszíni hálózaton S2S VPN átjáró kapcsolaton keresztül."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Hozzon létre egy VNet az Azure portálon hely közötti kapcsolat

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasszikus – klasszikus portál](vpn-gateway-site-to-site-create.md)


Ez a cikk végigvezeti virtuális hálózat és webhely VPN átjáró kapcsolat a helyszíni hálózaton, az erőforrás-kezelő Azure telepítési modell és az Azure portál használatával hozhat létre. Hely közötti kapcsolatok használható határokon helyszíni és hibrid konfigurációk.

![Diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Telepítési modellek és a hely közötti kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

A következő táblázat mutatja a jelenleg elérhető telepítési modellek és a webhely konfigurációk módszereket. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>További beállítások 

Ha szeretné VNets összekapcsolása, de ne hozzon létre kapcsolatot a helyszíni helyre, olvassa el a [VNet-VNet kapcsolat konfigurálása](vpn-gateway-vnet-vnet-rm-ps.md)című témakört. Szeretne hely közötti kapcsolat hozzáadása egy VNet, amely már rendelkezik egy kapcsolatot, lásd: [a meglévő VPN-átjáró kapcsolattal VNet S2S kapcsolat](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Első lépések

Ellenőrizze, hogy rendelkezik-e az alábbi elemek a konfigurációban megkezdése előtt:

- Kompatibilis VPN eszköz és olyannak, aki tudja állítani. Című témakörben [olvashat a virtuális Magánhálózati eszközöket](vpn-gateway-about-vpn-devices.md). Ha nem ismeri a virtuális Magánhálózati eszköze konfigurálása, vagy nem ismeri a tartományokat található IP-címét a helyszíni hálózati konfigurációja, a koordinálása másokkal, akik nyújtanak a adatai jelennek meg a szükséges.

- Külső felhasználókkal szemben lévő nyilvános IP-címének a virtuális Magánhálózati eszközt. Az IP-cím nem található meg az mögött egy hálózati címfordítást.
    
- Egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>A gyakorlat konfigurációs mintaértékekre


A gyakorlat használja ezeket a lépéseket, ha a minta konfigurációs értékek is használhatja:

- **VNet neve:** TestVNet1
- **Terület cím:** 10.11.0.0/16 és 10.12.0.0/16
- **Alhálózat:**
    - FrontEnd: 10.11.0.0/24
    - Kódmentes: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Erőforráscsoport:** TestRG1
- **Helye:** Kelet-Amerikai Egyesült Államok
- **DNS-kiszolgáló:** 8.8.8.8
- **Átjáró neve:** VNet1GW
- **Nyilvános IP:** VNet1GWIP
- **VPN típusa:** Útvonal-alapú
- **Kapcsolat típusa:** Webhely (IPsec)
- **Átjáró típusa:** VIRTUÁLIS MAGÁNHÁLÓZATI
- **Helyi hálózaton átjáró neve:** Hely2
- **Kapcsolatnév:** VNet1toSite2


## <a name="CreatVNet"></a>1. a virtuális hálózat létrehozása 

Ha már van egy VNet, győződjön meg róla, hogy a beállításokat a virtuális Magánhálózati átjárók tervezése kompatibilis. Adott figyelmet bármely alhálózathoz, előfordulhat, hogy a más hálózatokon átfedést. Ha egymást átfedő alhálózat, a kapcsolat nem megfelelően működik. Ha a megfelelő beállításokat a VNet van beállítva, külön előkészületek nélkül használhatja a [DNS-kiszolgáló megadása](#dns) című szakasz lépéseit.

### <a name="to-create-a-virtual-network"></a>Virtuális hálózat létrehozása

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. további Címterület és alhálózat hozzáadása

Akkor is adhat további Címterület és alhálózat a VNet létrehozása után.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. a DNS-kiszolgáló meghatározása

### <a name="to-specify-a-dns-server"></a>A DNS-kiszolgáló meghatározása

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. a átjáró alhálózat létrehozása

Mielőtt az átjárók a virtuális hálózathoz csatlakozik, először kell a virtuális hálózat, amelyhez csatlakozni az átjárót alhálózat létrehozása. Ha lehetséges érdemes létrehozni egy átjáró alhálózat alapcím CIDR /28 vagy /27 elég IP-címek további jövőbeli konfigurációs követelmények igazodik biztosítása érdekében.

Ha ebben a konfigurációban, mint egy torna hoz létre, olvassa el az alábbi [értékeket](#values) átjáró alhálózathoz létrehozásakor.

### <a name="to-create-a-gateway-subnet"></a>Átjáró alhálózat létrehozása


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. virtuális hálózati átjáró létrehozása

Ez a beállítás, mint egy torna hoz létre, ha is hivatkozhat a [minta konfigurációs értékeket](#values).

### <a name="to-create-a-virtual-network-gateway"></a>A virtuális hálózati átjáró létrehozása

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. a helyi hálózaton átjáró létrehozása

A helyi hálózaton átjáró a helyszíni hely hivatkozik. Adjon meg egy nevet, amellyel Azure is a hivatkozott a helyi hálózati átjáró. 

Ez a beállítás, mint egy torna hoz létre, ha is hivatkozhat a [minta konfigurációs értékeket](#values).

### <a name="to-create-a-local-network-gateway"></a>A helyi hálózati átjáró létrehozása

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. a virtuális Magánhálózati eszköz beállítása

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. a webhely virtuális Magánhálózati kapcsolat létrehozása

A webhely virtuális Magánhálózati kapcsolat létrehozása a virtuális hálózati átjáró és között VPN eszközére. Ügyeljen arra, hogy az értékek lecserélése saját. A megosztott kulcs egyeznie kell a virtuális Magánhálózati eszköz konfigurációhoz használt értékét. 

Mielőtt elkezdené az ebben a szakaszban, győződjön meg arról, hogy a virtuális hálózati átjáró és a helyi hálózaton átjárók befejezte létrehozását. Ha ebben a konfigurációban, mint egy torna szeretne létrehozni, olvassa el az alábbi [értékeket](#values) a kapcsolat létrehozása.

### <a name="to-create-the-vpn-connection"></a>A virtuális Magánhálózati kapcsolat létrehozása


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Ellenőrizze, hogy a virtuális Magánhálózati kapcsolat

Ellenőrizheti, hogy a virtuális Magánhálózati kapcsolatot a portálon vagy PowerShell használatával.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Következő lépések

- A kapcsolat elkészülte virtuális gépeken futó hozzáadhatja a virtuális hálózatok. Lásd: a virtuális gépeken futó [tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) további információt.

- BGP tudni lásd: a [BGP – áttekintés](vpn-gateway-bgp-overview.md) és [konfigurálásáról BGP](vpn-gateway-bgp-resource-manager-ps.md).
