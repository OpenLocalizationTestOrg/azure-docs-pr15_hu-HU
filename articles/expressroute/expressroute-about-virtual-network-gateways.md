<properties 
   pageTitle="Információ készült ExpressRoute virtuális hálózati átjárók |} Microsoft Azure"
   description="Készült ExpressRoute átjáró virtuális hálózati találhat."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Tudnivalók a virtuális hálózati átjárókat készült ExpressRoute


A virtuális hálózati átjáró hálózati forgalmának engedélyezésére Azure virtuális hálózatok között küldi és a helyszíni helyek. Ha készült ExpressRoute kapcsolat beállítása, létrehozása és konfigurálnia kell a virtuális hálózati átjáró és az átjáró virtuális hálózati kapcsolaton.

A virtuális hálózati átjáró létrehozásakor számos beállítást adja meg. A szükséges beállításokat egyik Itt adhatja meg, hogy az átjáró használható készült ExpressRoute vagy a webhely VPN-forgalmat. Az erőforrás-kezelő telepítési modell a értéke "-GatewayType".

Hálózati forgalmának engedélyezésére saját személyes kapcsolat küldésekor az átjáró típus "Készült ExpressRoute" használata. Ez akkor is nevezik egy készült ExpressRoute átjáró. Hálózati forgalmat a nyilvános interneten keresztül küldött titkosított, az átjáró típus "Vpn" használata. Ez egy virtuális Magánhálózati átjárót nevezik. Webhely webhely, webhely pont- és VNet-VNet kapcsolatok összes használja egy virtuális Magánhálózati átjárót. 

Minden egyes virtuális hálózati beállíthatja, hogy csak egy virtuális hálózati átjáró átjáró típusonként. Ha például lehet egy virtuális hálózati átjáró - GatewayType Vpn használó és - GatewayType készült ExpressRoute használó. Ez a cikk a virtuális hálózati készült ExpressRoute átjáró szolgáltatásaival.

## <a name="gwsku"></a>Átjáró SKU

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ha szeretne egy nagyobb teljesítményű átjáró Termékváltozat az átjáró frissítése, a legtöbb esetben is használhatja a "Átméretezés-AzureRmVirtualNetworkGateway" PowerShell-parancsmag. Ez az Standard és HighPerformance termékváltozatok frissítésekhez működni fog. Azonban a UltraPerformance Termékváltozat frissítéséhez szüksége lesz az átjáró újra.

###  <a name="aggthroughput"></a>Becsült összesítő átviteli átjáró Termékváltozat szerint


A következő táblázat mutatja az átjáró-típusok és a becsült összesítő kapacitása. Az alábbi táblázat az erőforrás-kezelő és a klasszikus telepítési modellek vonatkozik.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>REST API-hoz és a PowerShell-parancsmagok

További technikai források és adott szintaxiskövetelmények REST API-hoz és a PowerShell-parancsmagok használata virtuális hálózati átjáró konfigurációk esetén című témakörben talál az alábbi lapokon:

|**Klasszikus** | **Erőforrás-kezelő**|
|-----|----|
|[A PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[A PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API-VAL](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API-VAL](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Következő lépések

Elérhető a kapcsolat konfigurációk kapcsolatos további tudnivalók a [Készült ExpressRoute áttekintése](expressroute-introduction.md) című témakörben találhat. 







 
