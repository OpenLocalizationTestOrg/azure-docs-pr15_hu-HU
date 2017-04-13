|                              | **Pont-webhely**                                                                            | **Webhely-webhely**                                                                                        | **Készült ExpressRoute**                                                                                                                     |
|------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Azure támogatott szolgáltatások** | Felhőszolgáltatások és a virtuális gépeken futó                                                          | Felhőszolgáltatások és a virtuális gépeken futó                                                                     | [Szolgáltatások lista](../expressroute/expressroute-faqs.md#supported-services)                                                       |
| **Tipikus sávszélessége**       | A szokásos < 100 MB összesítés                                                               | A szokásos < 100 MB összesítés                                                                          | 50 MB, 100 MB, 200 MB, 500 MB, 1 GB/s, 2 GB/s, 5 GB/s, 10 GB/s                                                               |
| **Támogatott protokollok**      | Secure Sockets bújtatóprotokoll (SSTP)                                                     | IPsec                                                | Közvetlen kapcsolaton keresztül VLAN, NSP a virtuális Magánhálózati technológiák (MPLS, VPLS,...)                                                                                                    |
| **Továbbítás**                  | RouteBased (dinamikus)                                                                        | Támogatjuk (statikus útválasztás) PolicyBased és RouteBased (dinamikus útválasztási VPN)                 | BGP                                                                                                                                  |
| **Kapcsolat tűrőképessége**    | aktív-passzív                                                                               | aktív-passzív                                                                                          | aktív-aktív                                                                                                                        |
| **Tipikus használati eset**         | Prototípusának elkészítéséhez, a fejlesztők / tesztelése / labor forgatókönyvek a felhőszolgáltatások és a virtuális gépeken futó              | A fejlesztői / tesztelése / labor forgatókönyveket és a kis méretezni a felhőszolgáltatások és a virtuális gépeken futó gyártási munkaterhelésekből | Az összes Azure szolgáltatások (érvényesített lista), a vállalati szintű és misszió kritikus munkaterhelésekből, biztonsági mentése, a nagy adatok, az Azure DR helyként való hozzáférés |
| **SLA**                      | [SLA](https://azure.microsoft.com/support/legal/sla/)                                        | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                   | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                                                |
| **Árak**                  | [Árak](https://azure.microsoft.com/pricing/details/vpn-gateway/)                           | [Árak](https://azure.microsoft.com/pricing/details/vpn-gateway/)                                      | [Árak](https://azure.microsoft.com/pricing/details/expressroute/)                                                                   |
| **Műszaki dokumentáció**  | [Virtuális Magánhálózati átjáró dokumentáció](https://azure.microsoft.com/documentation/services/vpn-gateway/) | [Virtuális Magánhálózati átjáró dokumentáció](https://azure.microsoft.com/documentation/services/vpn-gateway/)            | [Dokumentáció készült ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/)                                        |
| **GYAKORI KÉRDÉSEK**                     | [Virtuális Magánhálózati átjáró – gyakori kérdések](vpn-gateway-vpn-faq.md)                                                    | [Virtuális Magánhálózati átjáró – gyakori kérdések](vpn-gateway-vpn-faq.md)                                                               | [Készült ExpressRoute – gyakori kérdések](../expressroute/expressroute-faqs.md)                                                                             |