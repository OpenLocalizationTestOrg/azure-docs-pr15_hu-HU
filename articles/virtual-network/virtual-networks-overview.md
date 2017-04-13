<properties
   pageTitle="Azure virtuális hálózati (VNet) – áttekintés"
   description="Tudjon meg többet az Azure virtuális hálózatok (VNets)."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="virtual-network-overview"></a>Virtuális hálózati – áttekintés

Az Azure virtuális hálózati (VNet), a megadott a saját hálózaton a felhőben.  -Előfizetéséhez dedikált Azure felhőszolgáltatások egy logikai elkülönítési. Teljesen megadhatja, hogy az IP-címterületet, a DNS-beállítások, a biztonsági házirendek és a útvonal táblázatok a hálózaton belül. További is oszthatja fel a VNet alhálózat be és indítsa el a Azure IaaS virtuális gépeken futó (VMs), illetve [Cloud services (PaaS szerepkör-példány)](../cloud-services/cloud-services-choose-me.md). Emellett a virtuális hálózat csatlakoztathatja közül az Azure-ban elérhető [csatlakozási beállítások](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) a helyszíni hálózaton. Lényegében kibonthatja a hálózat Azure, hogy a teljes vezérlő a IP-címterületet az Azure biztosít vállalati skála előnyeit.

Megértéséhez VNets, nézze meg az ábrán, amely mutatja az egyszerűsített a helyszíni hálózaton.

![A helyszíni hálózaton](./media/virtual-networks-overview/figure01.png)

A fenti ábrán egy helyszíni hálózaton keresztül útválasztó nyilvános internetkapcsolat. A tűzfal az útválasztó és a DMZ-annak a DNS-kiszolgáló és a webes kiszolgálófarm között is láthatja. A webes kiszolgálófarm használata a hardver terheléselosztó van kitéve az internethez, és a belső alhálózat erőforrásokat fogyaszt terheléselosztása. A belső alhálózat van elválasztva a DMZ át egy másik tűzfalat, és a hosts az Active Directory tartományi vezérlő kiszolgálók, az adatbázis-kiszolgáló és a alkalmazáskiszolgálók.

Azure-ban is szerepeltethetők ugyanabba a hálózatba, az alábbi ábrán látható módon.

![Azure virtuális hálózati](./media/virtual-networks-overview/figure02.png)

Figyelje meg, hogyan veszi fel az Azure infrastruktúra a szerepkör az útválasztó, hogy a hozzáférést a VNet nyilvános internetkapcsolat nélkül beállításokat. Tűzfal a hálózati biztonsági csoportok (NSGs) minden egyes alhálózathoz alkalmazott helyettesíthetők. És fizikai terheléselosztókkal vannak helyettesíteni internet szemben és belső terheléselosztókkal Azure-ban.

>[AZURE.NOTE] Azure két telepítési módok szerepelnek: classic (más néven Szolgáltatáskezelés) és Azure erőforrás Manager (ARM). Klasszikus VNets hozzáadott egy affinitás csoportba, vagy a program létrehoz egy területi VNet. Egy VNet affinitás csoportban, ha való [Áttérés a regionális VNet](virtual-networks-migrate-to-regional-vnet.md)ajánlott.

## <a name="virtual-network-benefits"></a>Virtuális hálózati legfontosabb előny

- **Elkülönítési**. VNets teljesen elkülönítik egymástól. Amely lehetővé teszi, hogy az azonos CIDR címterületet használó fejlesztése, tesztelése és munkakörnyezeti különálló hálózatok létrehozása.

- **Nyilvános internetkapcsolat a hozzáférést**. Egy VNet az összes IaaS VMs és PaaS szerepkör-példányok nyilvános internetkapcsolat alapértelmezett érheti el. Megadhatja, hogy az access hálózati biztonsági csoportok (NSGs) használatával.

- **A VNet belül VMs való hozzáférést**. Szerepkör-példányok PaaS és IaaS VMs indulhat virtuális ugyanabba a hálózatba, és csatlakozni tudnak a segítségével saját IP-címek, akkor is, ha az átjáró beállítása és használata a nyilvános IP-címek nélkül különböző alhálózathoz egymást.

- A **névfeloldás**. Azure IaaS VMs és telepítését a VNet az PaaS szerepkör-példányok belső névfeloldás biztosít. Is saját DNS-kiszolgálók telepítése és konfigurálása a VNet használhatók.

- **Biztonsági**. Megadása és kilépés a virtuális gépeken futó és PaaS szerepkör példányokat egy VNet a forgalmat a hálózati biztonsági csoportok használata szabályozható.

- **Kapcsolat**. A hálózati átjárók vagy VNet peering egymással VNets kapcsolhat össze. A helyszíni adatközpontokban hely közötti VPN hálózatok vagy a készült Azure ExpressRoute keresztül VNets kapcsolhat össze. Többet szeretne megtudni a webhely virtuális Magánhálózati kapcsolatot, látogasson el [a virtuális Magánhálózati átjárók](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Szeretne többet megtudni készült ExpressRoute, látogasson el a [készült ExpressRoute technikai áttekintés](../expressroute/expressroute-introduction.md). További információk VNet peering, látogasson el a [VNet peering](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Ellenőrizze, hogy hoz létre egy VNet, mielőtt bármilyen IaaS VMs vagy PaaS szerepkör-példányok a Azure környezetben. ARM-alapú VMs csak egy VNet, és nem adja meg egy meglévő VNet, ha Azure létrehoz egy alapértelmezett VNet, előfordulhat, hogy egy CIDR cím blokk csomópontjánál a helyszíni hálózaton. Így lehetetlenné, hogy csatlakozzon a VNet a helyszíni hálózaton.

## <a name="subnets"></a>Alhálózat

Alhálózat egy a VNet az IP-címek, egy VNet felosztása több alhálózat szervezeti és biztonsági. VMs és PaaS szerepkör példányaiban rendszerbe alhálózathoz (ugyanazon vagy másik) belül egy VNet kommunikálhatnak egymással további konfigurálása nélkül. Beállíthatja úgy is útvonal táblák és NSGs egy alhálózathoz.

## <a name="ip-addresses"></a>IP-címek


Az IP-címek Azure az erőforráshoz rendelt két típusa van: *nyilvános* és *titkos*. Internetes és más Azure nyilvános szolgáltatásokkal, például az [Azure vgx.dll gyorsítótár](https://azure.microsoft.com/services/cache/)és az [Azure esemény hubok](https://azure.microsoft.com/documentation/services/event-hubs/)kommunikáció Azure erőforrások nyilvános IP-címek engedélyezése Magánjellegű IP-címek lehetővé teszi, hogy egy virtuális hálózati együtt azokat, virtuális Magánhálózaton keresztül csatlakozik az erőforrások közötti kommunikációt, nélkül, egy internetes átirányítható IP-cím.

Többet szeretne tudni az Azure IP-címek, látogasson el [a virtuális IP-címek](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Azure terheléselosztókkal

Az Internet segítségével Azure terheléselosztókkal is kitenni virtuális gépeken futó és felhőszolgáltatások virtuális hálózatban. Vonal, amelyek a belső elérhető üzleti alkalmazások csak is osztható belső terheléselosztó használatával.

- **Külső terheléselosztó**. Nyilvános internetkapcsolat elérhető IaaS VMs és PaaS szerepkör előfordulását magas szintű elérhetőség biztosítása egy külső terheléselosztó is használhatja.

- A **belső terheléselosztó**. Magas szintű elérhetőség biztosítása IaaS VMs és PaaS szerepkör-példányok érhető el a VNet a rendszer többi szolgáltatása, amely egy belső terheléselosztó is használhatja.

Többet szeretne tudni az Azure terheléselosztás, keresse fel a [terhelés terheléselosztó áttekintése](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Hálózati biztonsági csoport (NSG)

Bejövő és kimenő hálózati kapcsolatok (NIC), a hozzáférés vezérléséhez NSGs hozhat létre VMs és alhálózat. Egy vagy több minden NSG tartalmaz, vagy megadása vagy sem forgalom jóváhagyott, vagy megtagadja a többi szabály alapján forrás IP-címe, forrásport, IP-címre és célport. Többet szeretne tudni a NSGs, látogasson el a [Mi az hálózati biztonsági csoportokat](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtuális készülékek

A virtuális készülék csak egy másik, a szoftver alapú készülék függvény, például tűzfal, a WAN-optimalizálási vagy behatolási észlelési futó VNet a virtuális. Egy útvonalat a VNet forgalmat keresztül egy virtuális készülék képességeit használandó Azure-ban hozhat létre.

Ha például NSGs használható biztonsági adja meg a VNet. Azonban felkínálja, hogy az NSGs a réteg 4 Access vezérlő lista (vezérlés) bejövő és kimenő csomagok. Szeretné az egy réteg 7 biztonsági modell használata, ha egy tűzfal készülék használni szeretne.

Virtuális készülékek attól függenek, [felhasználó által definiált útvonalak és IP-továbbítás](virtual-networks-udr-overview.md).

## <a name="limits"></a>Korlátai
Virtuális hálózatok előfizetés megengedett számú korlátozás van érvényben, kérjük, olvassa el az [Azure hálózati korlátozások](../azure-subscription-service-limits.md#networking-limits) további információt.

## <a name="pricing"></a>Árak
Nincs nem extra költségének a Azure virtuális hálózatok használatához. A számítási példányok belül a Vnet indított megterheljük a szabványos mértékeket [Azure virtuális árak](https://azure.microsoft.com/pricing/details/virtual-machines/)leírt módon. A [Virtuális Magánhálózati átjárók](https://azure.microsoft.com/pricing/details/vpn-gateway/) és [nyilvános IP-címek] (https://azure.microsoft.com/pricing/details/ip-addresses/) a VNet használt feltöltött szabványos mértékeket is lesz.

## <a name="next-steps"></a>Következő lépések

- [Egy VNet létrehozása](virtual-networks-create-vnet-arm-pportal.md) és alhálózat.
- [Hozzon létre egy virtuális a egy VNet](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Tudjon meg többet [NSGs](virtual-networks-nsg.md).
- Tudjon meg többet a [felhasználó által definiált útvonalak és IP-továbbítás](virtual-networks-udr-overview.md).
