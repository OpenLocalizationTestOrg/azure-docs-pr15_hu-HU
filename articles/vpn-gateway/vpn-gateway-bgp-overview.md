<properties
   pageTitle="Az Azure virtuális Magánhálózati átjárók BGP áttekintése |} Microsoft Azure"
   description="Ez a cikk áttekintést az Azure virtuális Magánhálózati átjárók BGP."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Az Azure virtuális Magánhálózati átjárók BGP áttekintése

Ez a cikk áttekintést nyújt az Azure virtuális Magánhálózati átjárók BGP (szegély átjáró Protocol) támogatás.

## <a name="about-bgp"></a>Tudnivalók a BGP

BGP a szabványos útválasztási protokoll gyakran használt az interneten az exchange Útválasztás és elérhetőség információk két vagy több hálózatok között. Ha használja az Azure virtuális hálózattal környezetben, BGP lehetővé teszi, hogy az Azure virtuális Magánhálózati átjárók, és a helyszíni virtuális Magánhálózati eszközök BGP partnerek vagy szomszédok, az exchange "irányítja", amely tájékoztatja mindkét átjárók elérhetősége és az adott prefixumokban az átjárók és az útválasztó folyamatát elérhetőség című részt vevő. BGP is engedélyezheti a hálózaton átvitt útválasztás több hálózatok között egy BGP peer az összes többi BGP partnerek számára folyamatosan tanul BGP az átjárók útvonalak propagálása szerint.
 
### <a name="why-use-bgp"></a>Miért érdemes használni a BGP?

BGP választható fiókfunkció Azure útvonal-alapú VPN átjárók használhatja. Is győződjön meg arról, hogy a helyszíni virtuális Magánhálózati eszközök támogatják BGP szolgáltatás engedélyezése előtt. Azure virtuális Magánhálózati átjárók és a helyszíni virtuális Magánhálózati eszközök BGP nélkül is. Célszerű az egyenértékű (nélkül BGP) statikus útvonalak *összehasonlítása* használata a dinamikus útválasztás BGP között a hálózat és Azure használatával.

Több előnye és BGP az új funkciók is van:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>A frissítések automatikus és rugalmas előtag támogatása

Az BGP csak kell egy adott BGP peer előtag minimális bejelenteni a IPsec S2S VPN-csatorna fölé. Host előtaggal minél kisebb méretű lehet (/ 32) BGP peer IP-cím a helyszíni VPN eszköz. Beállíthatja, amely a helyszíni hálózaton prefixumokban meghirdetése az Azure Azure virtuális hálózatához eléréséhez engedélyezni szeretné.
    
Akkor is helyüket, hogy egy nagyobb prefixumokban, előfordulhat, hogy néhány VNet címet a prefixumokban, például egy nagy magánjellegű IP-címterület (például 10.0.0.0/8). Felhívjuk a figyelmét arra, hogy a prefixumokban nem azonos a VNet prefixumokban közül bármelyik. Ezeket a VNet prefixumokban azonos útvonalak elutasításra kerül.

>[AZURE.IMPORTANT] Jelenleg az alapértelmezett útvonal (0.0.0.0/0) Azure virtuális Magánhálózati átjárók reklámozása, azzal blokkolhatja. További frissítés nyújtanak, ha ez a képesség engedélyezve van.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Támogatja a több alagutakat egy VNet és egy helyszíni webhely között a BGP alapján automatikusan átveszi

Több kapcsolatot hozhat létre az Azure VNet és a helyszíni virtuális Magánhálózati eszközök ugyanazon a helyen között. Ezt a lehetőséget biztosít a több alagutak (elérési út) az az aktív-aktív konfigurációban hálózatok között. Ha megszakad a alagutak közül, a megfelelő útvonalak fog vonni BGP keresztül, és a forgalmat a hátralévő alagutak fog automatikusan váltás.
    
A következő ábrán egy egyszerű példa: Ez a beállítás nagyon érhető el:
    
![Több aktív út](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Támogatja a hálózaton átvitt továbbítás a helyszíni hálózatok és több Azure VNets között

BGP lehetővé teszi, hogy több átjáró propagálása prefixumokban a különböző hálózati, és megtudhatja, hogy közvetve vagy közvetlenül csatlakoznak. Ez engedélyezheti a hálózaton átvitt útválasztás az Azure virtuális Magánhálózati átjáró a helyszíni webhelyek között, vagy több Azure virtuális hálózaton keresztül.
    
A következő ábrán egy példát a több elem azonosítható topológiájának a forgalmat a helyszíni hálózatok keresztül Azure virtuális Magánhálózati átjárók belül a Microsoft Networks között is továbbítási több elérési út:

![Több elem azonosítható hálózaton átvitt](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>BGP gyakori kérdések


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Következő lépések

Című [Azure virtuális Magánhálózati átjárók a BGP – első lépések](./vpn-gateway-bgp-resource-manager-ps.md) a lépések BGP konfigurálása a határokon helyszíni és VNet-VNet kapcsolatok.

