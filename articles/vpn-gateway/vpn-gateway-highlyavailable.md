<properties
   pageTitle="Az Azure virtuális Magánhálózati átjárók könnyen hozzáférhető konfigurációk áttekintése |} Microsoft Azure"
   description="Ez a cikk áttekintést nyújt Azure virtuális Magánhálózati átjárók használatával könnyen hozzáférhető beállítási lehetőségeket."
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
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Könnyen hozzáférhető határokon helyszíni és VNet-VNet kapcsolat

Ez a cikk áttekintést könnyen hozzáférhető beállítási lehetőségei az idegen helyszíni és VNet-VNet kapcsolódási Azure virtuális Magánhálózati átjárók használatával.

## <a name = "activestandby"></a>Azure virtuális Magánhálózati átjáró redundancia kapcsolatban

Minden Azure virtuális Magánhálózati átjáró áll, az aktív készenléti konfigurációban két példánya. Tervezett karbantartásokkal kapcsolatos információk vagy tervezett állásidőt azzal, hogy az aktív példány történik az készenléti példány szeretne automatikus átvétele (feladatátvétel), és folytassa a S2S VPN vagy VNet-VNet kapcsolatok. A váltás során egy rövid megszakítás eredményezi. Tervezett karbantartásokkal kapcsolatos információk kapcsolatot az 10 – 15 másodpercek lehet visszaállítani. A váratlan hibák leírását a kapcsolat helyreállítás lesz hosszabb, és a legrosszabb esetben 1 és fél perc körülbelül 1 perc. Az ügyfél kapcsolatot P2S VPN az átjáró a P2S kapcsolatok megszakad a kapcsolata, és a felhasználók az ügyfél gépek az újracsatlakozáshoz kell.

![Aktív készenléti](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Könnyen hozzáférhető határokon helyszíni kapcsolat

Jobb elérhetőség nyújt a helyiségek közötti kapcsolatokat, létezik néhány beállítások érhető el:

- Több helyszíni virtuális Magánhálózati eszközök
- Aktív – aktív Azure virtuális Magánhálózati átjáró
- Mindkét

### <a name = "activeactiveonprem"></a>Több helyszíni virtuális Magánhálózati eszközök

Segítségével több virtuális Magánhálózati eszközök a helyszíni hálózaton az Azure virtuális Magánhálózati átjárón a következő ábrán látható módon:

![Több helyszíni VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Ebben a konfigurációban a azonos Azure virtuális Magánhálózati átjáró a helyszíni eszközén ugyanazon a helyen több aktív bújtatások biztosít. Létezik néhány követelmények és korlátok:

1. Hozzon létre több S2S virtuális Magánhálózati kapcsolatot Azure virtuális Magánhálózati készülékekről szüksége. Több virtuális Magánhálózati eszközök a helyszíni hálózaton való csatlakozáskor Azure, kell minden VPN eszközhöz átjárók helyi hálózaton, és több kapcsolat létrehozása az Azure virtuális Magánhálózati átjáró a helyi hálózati átjáró.

2. A helyi hálózaton átjárók a virtuális Magánhálózati eszközök tartozó egyedi nyilvános IP-címek kell rendelkeznie a "GatewayIpAddress" tulajdonságban.

3. BGP szükség, ebben a konfigurációban. Minden egyes helyi hálózaton átjáró, amely egy virtuális Magánhálózati eszközt egy egyedi BGP peer IP-címet a "BgpPeerIpAddress" tulajdonságban meghatározott kell rendelkeznie.

4. Minden egyes helyi hálózaton átjáró AddressPrefix tulajdonság mezőjére a kell nem áll átfedésben. Akkor kell megadnia a "BgpPeerIpAddress" /32 CIDR formátumú a AddressPrefix mezőben, például 10.200.200.254/32.

5. Az azonos prefixumokban azonos a helyszíni hálózaton prefixumokban a Azure virtuális Magánhálózati átjárónak, és a forgalom továbbításra kerül – ezek alagutak egyidejű meghirdetése BGP kell használni.

6. Minden kapcsolat alagutakat az Azure virtuális Magánhálózati átjáró 10 Basic és a szokásos termékváltozatban, a maximális száma alapján számít és 30 HighPerformance Termékváltozat. 

Ebben a konfigurációban az Azure virtuális Magánhálózati átjáró továbbra is aktív készenléti módban van, hogy az azonos feladatátvételt és rövid megszakítás továbbra is történik ismertetett módon [a fenti](#activestandby). De ez a beállítás hibák vagy a helyszíni hálózaton és a virtuális Magánhálózati eszközök kiküszöbölése ellen védelmet.
 
### <a name="active-active-azure-vpn-gateway"></a>Aktív – aktív Azure virtuális Magánhálózati átjáró

Most már létrehozhat egy Azure virtuális Magánhálózati átjáró aktív-aktív konfigurációban, ahol az átjáró VMs mindkét példányát fog S2S VPN alagutakat létre helyszíni VPN eszközére, a következő ábrán látható módon:

![Aktív-aktív](./media/vpn-gateway-highlyavailable/active-active.png)

Ebben a konfigurációban minden Azure átjárópéldány lesz egy egyedi nyilvános IP-címet, és minden egyes hoznak egy IPsec/IKE S2S VPN-csatorna, a helyi hálózati átjáró és a kapcsolat megadott a helyszíni VPN eszközére. Ne feledje, hogy mindkét VPN bújtatás ténylegesen ugyanazt a kapcsolatot egy részét. Továbbra is kell beállítani a helyszíni VPN eszközt, akik elfogadhatják vagy két S2S VPN alagutakat e két Azure virtuális Magánhálózati átjáró nyilvános IP-címek létre.

Mivel az Azure átjárópéldány aktív-aktív konfigurációja, a Azure virtuális hálózatról a forgalmat a helyszíni hálózaton keresztül történik mindkét alagutak ablakokban, akkor is, ha a helyszíni VPN eszköz a egymás alatti egy alagutas alkalmazást is. Vegye figyelembe, hogy az azonos TCP- és UDP-adatfolyam mindig fog bejárása, alagutas vagy a elérési útját, kivéve, ha a karbantartás esemény példányok egyik történik.

A tervezett karbantartás vagy tervezett esemény több átjárópéldány történik, ha megszakad a IPsec alagutas példányhoz helyszíni VPN eszközére. A virtuális Magánhálózati eszközökön a megfelelő útvonalak kell eltávolítható vagy visszavonni automatikusan, hogy a forgalom az az aktív IPsec alagutas fog átváltott. Azure oldalán a Váltás a fölé automatikusan megtörténik a szóban forgó példányból az aktív példánnyal.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Kettős-redundancia: aktív-aktív VPN átjárók Azure és a helyszíni hálózatok

A legtöbb megbízható, hogy az aktív-aktív átjárók a hálózati és az Azure-egyesítése az alábbi ábrán látható módon.

![Kettős redundancia](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Itt létrehozása és az aktív-aktív konfiguráció az Azure virtuális Magánhálózati átjáró beállítása, és két helyi hálózaton átjáró létrehozása és a két két kapcsolatainak helyszíni virtuális Magánhálózati eszközök fentebb ismertetett. Az eredmény 4 IPsec alagutak Azure virtuális hálózatához, és a helyszíni hálózaton között egy háló internetkapcsolat lesz.

Az összes átjárójához és alagutak is aktív az Azure oldaláról, így a forgalom elosztva között összes 4 alagutak ablakokban, bár minden TCP- és UDP-adatfolyam újra követi a azonos alagutas vagy az elérési utat Azure oldaláról. Annak ellenére, hogy a forgalmat, úgy fölé a IPsec alagutak kissé jobb átviteli jelenhetnek meg, az elsődleges Ez a beállítás célja magas elérhetőség. És miatt a terjesztése statisztikai jellegét, adja meg, hogyan másik alkalmazás forgalom körülmények mérésére hatással van az összesítő átviteli nehezen.

A topológia szükség van a két helyi hálózaton átjáró a helyszíni VPN párban támogatása két kapcsolatot, és BGP szükség engedélyezze a két kapcsolatokat a helyszíni hálózaton. Ezeknek a követelményeknek ugyanazok, mint a [fenti](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Azure virtuális Magánhálózati átjárók – könnyen hozzáférhető VNet-VNet kapcsolat

Az azonos aktív-aktív konfiguráció Azure VNet VNet kapcsolatok is alkalmazhat. Mindkét virtuális hálózatok aktív-aktív VPN átjárók létrehozása, és csatlakozás őket közös megjeleníthető azonos háló kapcsolatot a 4 alagutak a két VNets között az alábbi ábrán látható módon:

![VNet-VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Ez biztosítja, hogy mindig legyenek alagutak minden tervezett karbantartás eseményekhez, még jobb elérhetőség kezeléséről a két virtuális hálózatok között két. Annak ellenére, hogy az azonos topológiájának határokon helyszíni kapcsolat két kapcsolatot igényel, a fent látható VNet-VNet topológia minden egyes átjáró csak egy kapcsolatot lesz szüksége. Ezenkívül BGP nem kötelező, kivéve, ha a hálózaton átvitt útválasztás VNet-VNet-kapcsolaton keresztül szükség.


## <a name="next-steps"></a>Következő lépések

Lásd: az [Aktív-aktív VPN átjárók konfigurálásával Keresztté helyszíni és VNet-VNet kapcsolatok](vpn-gateway-activeactive-rm-powershell.md) , a lépések aktív-aktív határokon helyszíni és VNet-VNet kapcsolatok konfigurálása.
