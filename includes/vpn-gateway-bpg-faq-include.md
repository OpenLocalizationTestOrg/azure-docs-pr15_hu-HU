### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Támogatott BGP Azure virtuális Magánhálózati átjáró minden termékváltozatban?

Nem, BGP Azure **normál** és **HighPerformance** VPN átjárók támogatott. **Egyszerű** Termékváltozat nem támogatott.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Használhatom-e BGP Azure Policy-Based VPN átjárók?

Nem, csak az útvonal alapú virtuális Magánhálózati átjárók BGP esetén támogatott.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Van-e lehetőség a magánjellegű ASN-ek (önálló rendszer számok)?

Igen, használhatja a saját ASN-ek nyilvános vagy magánjellegű ASN-ek a helyszíni hálózatok és az Azure virtuális hálózatok.

#### <a name="are-there-asns-reserved-by-azure"></a>Vannak fenntartva Azure ASN-EK?

Igen, a következő ASN-ek vannak fenntartva Azure-külső és belső peerings:

- Nyilvános ASN-ek: 8075, 8076, 12076
- Magánjellegű ASN-ek: 65515, 65517, 65518, 65519, 65520

Ezek ASN-ek nem adja meg az alkalmazás telepítésű virtuális Magánhálózati eszközök, ha átjárók Azure virtuális Magánhálózati kapcsolatot.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Használhatom-e az azonos ASN helyszíni VPN hálózatok és Azure VNets?

Nem, hozzá kell rendelnie különböző ASN-ek a helyszíni hálózatok és az Azure VNets között ha csatlakozik őket BGP együtt. Azure virtuális Magánhálózati átjárók ASN 65515 rendelve, az alapértelmezett van, hogy BGP engedélyezve van, nem a az idegen helyszíni kapcsolatot. Bírálja felül az alapértelmezett értéket egy másik ASN hozzárendelésével a virtuális Magánhálózati átjáró létrehozása vagy módosítása a ASN, az átjáró létrehozása után. Meg kell a helyszíni ASN-ek hozzárendelése a megfelelő helyi Azure-hálózati átjárók.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Milyen cím prefixumokban fog Azure virtuális Magánhálózati átjárók meghirdetése számomra?

Azure virtuális Magánhálózati átjáró a helyszíni BGP eszközök a következő útvonalak fog meghirdetése:

- A VNet cím prefixumokban
- Minden egyes helyi hálózati átjárók cím prefixumokban csatlakozik az Azure virtuális Magánhálózati átjáró
- A leveleket más csatlakozik az Azure virtuális Magánhálózati átjáró, **kivéve az alapértelmezett útvonal vagy bármely VNet előtaggal átfedésben útvonalak**BGP peering munkamenetek tanultakhoz.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Alapértelmezett útvonalat (0.0.0.0/0) Azure virtuális Magánhálózati átjárók is meghirdetése?

Nem adott időben.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Is a pontos előtagot is meghirdetése, a virtuális hálózati prefixumokban?

Nem, a azonos előtagot, mint a virtuális hálózat közül bármelyik hirdetési cím prefixumokban fog letiltott vagy az Azure platform szerint szűrt. Azonban Ön is egy előtagot, amelyet a virtuális hálózat belül van felülbírálja helyüket. A virtuális hálózat használhatja a hely címe 10.10.0.0/16 és a sikerült meghirdetése a 10.0.0.0/8.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Használhatom-e BGP a VNet-VNet kapcsolatokat?

Igen, határokon helyszíni kapcsolatok és a VNet-VNet kapcsolatok BGP is használhatja.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Is lehet keverje BGP nem BGP kapcsolatokat az Azure virtuális Magánhálózati átjárók?

Igen, az Azure virtuális Magánhálózati átjárón BGP és a nem BGP kapcsolatainak is keverjen ki.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Azure virtuális Magánhálózati átjáró támogatási BGP hálózaton átvitt útválasztás elve

Igen, BGP hálózaton átvitt útválasztás támogatott, a kivétel, hogy az Azure virtuális Magánhálózati átjárók **nem** fog meghirdetése alapértelmezett útvonalak más BGP partnerek számára. Hálózaton átvitt útválasztás keresztül több Azure virtuális Magánhálózati átjáró engedélyezéséhez engedélyeznie kell BGP összes köztes VNet-VNet-kapcsolatot.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Felvehetek-e több alagutak Azure virtuális Magánhálózati átjáró és a helyszíni hálózaton között?

Igen, akkor alagutakat hozhatnak létre egynél több S2S VPN-Azure virtuális Magánhálózati átjáró és a helyszíni hálózaton között. Felhívjuk a figyelmét arra, hogy ezek a alagutak lesznek megszámlálva az Azure virtuális Magánhálózati átjárók alagutak száma alapján. Az Azure virtuális Magánhálózati átjáró és a helyszíni hálózaton egyik között két felesleges alagutak Ha, például fog ki a teljes kvóta az Azure virtuális Magánhálózati átjáró (10 standard) és a 30 HighPerformance 2 alagutak igényelnek.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Felvehetek-e több alagutak BGP a két Azure VNets között?

Nem, nem támogatottak a felesleges alagutak két virtuális hálózatok között.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Van lehetőség BGP S2S VPN készült ExpressRoute/S2S VPN együttes jelenléte konfigurációban?

Nem adott időben.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Milyen cím használata a Azure virtuális Magánhálózati átjáró BGP Peer IP?

Az Azure virtuális Magánhálózati átjáró tartományból GatewaySubnet virtuális hálózatok osztja fel a egyetlen IP-címet. Alapértelmezés szerint a tartomány második utolsó címe. Például ha a GatewaySubnet 10.12.255.0/27, kezdve 10.12.255.0 10.12.255.31, majd Azure virtuális Magánhálózati átjáró BGP Peer IP-címe lesz 10.12.255.30. Megtalálhatja a ezt az információt, ha a lista az Azure virtuális Magánhálózati átjáró adatait.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Mik azok a követelmények BGP Peer IP-címek a virtuális Magánhálózati készülékemen?

A helyszíni BGP peer címet **Kell nem** ugyanazok, mint a virtuális Magánhálózati eszköz nyilvános IP-címét. A BGP Peer IP-VPN-eszköz használata egy másik IP-címet. Ez lehet az eszközre a visszacsatolási felület rendelt címre. Adja meg a megfelelő helyi hálózati átjáró a helyet, amely a cím.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Mit kell megadni, a cím prefixumokban a helyi hálózati átjáró BGP használatakor?

Azure helyi hálózati átjáró adja meg a kezdeti címe prefixumokban a helyszíni hálózaton. A BGP, hozzá kell rendelnie az állomás előtag (/ 32 előtag) a BGP Peer IP-címének, mint a címterületet az, hogy a helyszíni hálózaton. Ha a BGP Peer IP 10.52.255.254, a "10.52.255.254/32", a helyi hálózati átjáró a helyszíni hálózaton, amely a localNetworkAddressSpace kell megadnia. Ez akkor győződjön meg arról, hogy az Azure virtuális Magánhálózati átjáró létrehozza-e a BGP-munkamenetet, a S2S VPN-csatorna keresztül.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Mit kell felvenni a helyszíni VPN eszközömön a BGP peering munkamenet?

Mutasson a IPsec S2S VPN-csatorna VPN eszközén kell adja meg az Azure BGP Peer IP-címek host utat. Az Azure virtuális Magánhálózati Peer IP "10.12.255.30", kell VPN eszközén például hozzáadása "10.12.255.30" nexthop kezelőfelületen jelennek meg a megfelelő IPsec alagutas felület host útvonal.
