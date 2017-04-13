|    | **Virtuális Magánhálózati átjáró átviteli (1)** | **Virtuális Magánhálózati átjárók maximális IPsec bújtatja (2)** | **Készült ExpressRoute átjáró átviteli** | **Virtuális Magánhálózati átjáró és készült ExpressRoute megtalálhatók.**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Egyszerű Termékváltozat (3)(5)**              |  100 MB | 10                         |  500 MB                           | nem   |
| **Szabványos Termékváltozat (4)(5)**           |  100 MB | 10                         | 1000 MB                           | igen  |
| **Nagy teljesítményű Termékváltozat (4)**   | 200 MB  | 30                         | 2000 MB                           | igen  |

- (1) a virtuális Magánhálózati átviteli durva becslés alapján a méreteket, ugyanabban a Azure régióban VNets között. Még nem egy garantált átviteli az idegen helyszíni kapcsolatot az interneten keresztül. A lehetséges maximális sebesség mérték.
- (2) olvassa el az alagutak száma RouteBased VPN adatai. Csak a PolicyBased VPN támogatják az egy webhely VPN-csatorna.
- (3) az egyszerű Termékváltozat BGP nem támogatott.
- (4) a Termékváltozat PolicyBased VPN adatai nem támogatott. Az egyszerű Termékváltozat általuk támogatott.
- (5) a Termékváltozat aktív-aktív S2S virtuális Magánhálózati átjáró kapcsolatok nem támogatott. Csak a HighPerformance Termékváltozat aktív-aktív támogatott.