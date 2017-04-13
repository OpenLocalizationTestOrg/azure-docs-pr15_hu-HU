<properties 
   pageTitle="Mik azok a felhasználó által definiált útvonalak és IP-továbbítási?"
   description="Megtudhatja, hogy miként használni kívánt felhasználó által definiált útvonalak (UDR) és IP-továbbítási előre forgalmat a hálózati Azure virtuális készülékek."
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

# <a name="what-are-user-defined-routes-and-ip-forwarding"></a>Mik azok a felhasználó által definiált útvonalak és IP-továbbítási?
Virtuális hálózati (VNet) az Azure virtuális gépeken futó (VMs) ad hozzá, megfigyelheti, hogy a VMs képesek egymással a hálózaton keresztül, automatikusan. Nem szeretné az átjárók, adja meg annak ellenére, hogy a VMs a különböző alhálózathoz. Ugyanez igaz nyilvános internetkapcsolat a VMs történő kommunikációhoz, és még a helyszíni a hálózatról, ha a saját adatközponthoz az Azure hibrid kapcsolat nem tartalmaz adatokat.

Ez a folyamat kommunikációs oka az lehetséges Azure rendszer útvonalak sorozata segítségével határozza meg, hogyan átfolyása az IP-forgalmat. Rendszer útvonalak szabályozását kommunikációs az alábbi esetekben:

- A ugyanahhoz az alhálózathoz belül.
- A másikra egy VNet alhálózat.
- A VMs az internethez.
- A másik VNet VPN átjárón keresztül szeretne egy VNet.
- Az egy VNet, hogy a helyszíni hálózaton keresztül egy virtuális Magánhálózati átjárót.

Az alábbi ábrán egy VNet, két alhálózat és néhány VMs, a rendszer útvonalak, amelyek lehetővé teszik IP forgalmat együtt egy egyszerű beállítás.

![Rendszer útvonalak Azure-ban](./media/virtual-networks-udr-overview/Figure1.png)

Annak ellenére, hogy a rendszer útvonalak használatát automatikusan a telepítéshez forgalom megkönnyíti, vannak, amelyben meg szeretné adni egy virtuális készülék keresztül csomagok útválasztás esetek. Úgy, hogy a felhasználó által definiált útvonalak létrehozásával megadhatja a következő ugrás csomagok inkább a virtuális készülék Ugrás adott alhálózat halad, és a virtuális készülék futtatott a virtuális IP-továbbítás engedélyezése.

Az alábbi ábra szemlélteti a felhasználó által definiált útvonalak, és egy alhálózat IP-továbbítás csomagok küldeni egy másik egy harmadik alhálózat egy virtuális készülék folyamatát.

![Rendszer útvonalak Azure-ban](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] A felhasználó által definiált útvonalak csak alkalmazza a program elhagyása alhálózat forgalmat. megadhatja, hogyan forgalom kijelölésen alhálózat az internetről, például utakat nem hozható létre. A forgalom továbbítása készülék is, nem lehet ugyanahhoz az alhálózathoz, ahol a forgalom származik. A berendezések külön alhálózat mindig létrehozása. 

## <a name="route-resource"></a>Útvonal erőforrás
Csomagok résztvevőkhöz minden csomópont fizikai hálózaton definiálni útvonal táblán alapuló TCP/IP hálózaton keresztül. Útvonal táblázat az egyes útvonalak döntse el, hova továbbítása a cél IP-címe alapján csomagok használt gyűjteménye. Egy útvonalat a következőkből áll:

|A tulajdonság|Leírás|Korlátozások|Megfontolandó szempontok|
|---|---|---|---|
| Cím előtag | A cél CIDR, amelyre az útvonal vonatkozik, például 10.1.0.0/16.|Kell kell érvényes CIDR tartományt, amely a nyilvános internetkapcsolat, Azure virtuális hálózati vagy a helyszíni adatközponthoz címét.|Győződjön meg arról, hogy a **cím előtag** nem tartalmaz a címet a **következő ugrási cím**, különben a program a csomagok adja meg a forrásból származó Ugrás a következő ugrás nélkül minden eddiginél elérje a cél ismétlődve. |
| Következő azonosítható típusa | Azure azonosítható a típusát a csomagot is szerepel. | A következő értékek egyike lehet: <br/> **Virtuális hálózat**. A helyi virtuális hálózatát jelöli. Például ha két alhálózat, 10.1.0.0/16 és 10.2.0.0/16 virtuális ugyanabba a hálózatba, az útvonal az útvonal táblázat minden egyes alhálózat *Virtuális*hálózat ugrási értékkel fog rendelkezni. <br/> **Virtuális hálózati átjáró**. Az Azure S2S virtuális Magánhálózati átjáró jelöli. <br/> **Internetes**. Az alapértelmezett internetes átjáró az Azure infrastruktúra által biztosított jelöli. <br/> **Virtuális készülék**. Az Azure virtuális hálózatához felvett egy virtuális készülék jelöli. <br/> A **Nincs lehetőséget**. Egy visszajelzést jelöli. Egy visszajelzést továbbított csomagok egyáltalán nem kell továbbítani.| Fontolja meg inkább a **nincs lehetőség** típusú le szeretné állítani a célként megadott halad-csomagokat, azokat. | 
| Következő ugrás címe | A következő ugrás címét csomagok következőnek IP-címét tartalmazza. Következő azonosítható értékek csak a következő azonosítható típusa esetén *Készülék virtuális*útvonalak engedélyezettek.| IP-címet, amely a virtuális hálózaton, a felhasználó által definiált útvonal alkalmaztunk belül érhető el kell lennie. | Ha egy virtuális IP-címét, feltétlenül engedélyezi az [IP-továbbítási](#IP-forwarding) Azure-ban a virtuális. |

Az Azure PowerShell különböző neveket néhány "NextHopType" értéket áll:
- Virtuális Network nem VnetLocal
- Virtuális hálózati átjáró VirtualNetworkGateway
- Virtuális készülék VirtualAppliance
- Internet az interneten
- Nincs egyike sem

### <a name="system-routes"></a>Rendszer útvonalak
Minden virtuális hálózatban létrehozott alhálózat automatikusan kapcsolódik, amely tartalmazza a következő szabályokat a rendszer útvonal útvonal táblázat:

- **Helyi Vnet szabály**: Ez a szabály automatikusan létrejön egy virtuális hálózaton minden alhálózat. Adja meg, hogy közvetlen hivatkozást a VMs a a VNet és között nincs köztes következő ugrás nem.
- **A helyszíni szabály**: Ez a szabály minden forgalmat a helyszíni címtartományokat szánt vonatkozik, és használja a virtuális Magánhálózati átjáró Ugrás a következő helyeként.
- **Internetes szabály**: Ez a szabály fogópontok minden forgalom szánt nyilvános internetkapcsolat (cím előtag 0.0.0.0/0), és használja a következő Ugrás az infrastruktúra internetes átjáró minden forgalom szánt az internethez.

### <a name="user-defined-routes"></a>Felhasználó által definiált útvonalak
A legtöbb környezetben csak szüksége lesz a rendszer útvonalak már Azure határozza meg. Azonban előfordulhat, hozzon létre egy útvonal táblát, és vegyen fel egy vagy több útvonalak bizonyos esetekben, például:

- Lehetséges, hogy a helyszíni hálózaton keresztül az internethez tunneling.
- Az Azure környezetben virtuális készülékek használata.

A fenti esetek útvonal táblázat létrehozása és a felhasználó által definiált útvonalak hozzáadása fog rendelkezni. Több útvonalon tábla is van, és a táblázatból útvonal társítható egy vagy több alhálózat. És minden egyes alhálózat csak egyetlen útvonal táblázat van társítva. Az összes VMs és alhálózat használatban az útvonal táblázat felhőszolgáltatások társított alhálózat.

Alhálózat mindaddig, amíg az útvonal táblázat társítva az alhálózathoz rendszer útvonalak támaszkodhat. Ha már társítás, továbbítás végzett alapján a leghosszabb előtag hol.van (LPM) a felhasználó által definiált útvonalak és a rendszer útvonalak között. Ha egynél több útvonalat az azonos LPM egyező akkor útvonal lesz kijelölve alapján az origin a következő sorrendben:

1. Felhasználói definiált továbbítására
1. BGP útvonal (használatakor készült ExpressRoute)
1. Rendszer továbbítására

A felhasználó által definiált útvonalak létrehozása című témakörből megtudhatja, [hogy miként létrehozása útvonalak és IP-továbbítási engedélyezése Azure-ban](virtual-network-create-udr-arm-template.md).

>[AZURE.IMPORTANT] A felhasználó által definiált útvonalak csak alkalmazza a program Azure VMs és felhőbeli szolgáltatásokhoz. Ha például ha azt szeretné, a tűzfal között a helyszíni hálózaton és Azure virtuális készülék hozzáadása, be kell hoz létre a felhasználó által definiált útvonalat az Azure útvonal táblázatok, amely az összes forgalmat a helyszíni címterületet virtuális készülékre Ugrás továbbítja. (UDR) a felhasználó által definiált útvonalat is hozzáadhatja a GatewaySubnet eljuttatni minden forgalmat a helyszíni Azure virtuális készülék keresztül. A legutóbbi összeadás az.

### <a name="bgp-routes"></a>BGP útvonalak
Ha egy készült ExpressRoute kapcsolat a helyszíni hálózaton és Azure között, engedélyezheti BGP szülőtől útvonalak Azure a helyszíni hálózaton. Ezek az BGP útvonalak a ugyanúgy, mint a rendszer útvonalak és a felhasználó által definiált útvonalak minden Azure alhálózathoz használják. További információ című témakörben [Készült ExpressRoute](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Beállíthatja, hogy a helyszíni hálózaton keresztül, a következő ugrás a virtuális Magánhálózati átjáró használó alhálózat 0.0.0.0/0 felhasználói definiált útvonal létrehozásával tunneling hatályba használni a Azure környezetben. Azonban ez csak akkor működik, ha egy virtuális Magánhálózati nem készült ExpressRoute átjáró használja. A készült ExpressRoute a kényszerített tunneling BGP keresztül van beállítva.

## <a name="ip-forwarding"></a>IP-továbbítás
Ismertetik fölött, mint egy felhasználó definiált útvonal létrehozása fő oka lehet annak egyik továbbítja a forgalmat a virtuális készülék. A virtuális készülék semmi több, mint egy virtuális, az valamilyen módon, például a tűzfalat vagy hálózati Címfordítást eszköz hálózati forgalmának kezeléséhez használható alkalmazást futtató.

Ez a virtuális készülék virtuális érkeznek bejövő forgalom magát nem címzett kell lennie. Egy virtuális más helyekre címezve forgalom fogadására engedélyezéséhez engedélyeznie kell a IP-továbbítási a virtuális. Ez a-beállítás, nem módosítása a vendégként való bekapcsolódáshoz operációs rendszer Azure.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogy miként hozhat [létre az erőforrás-kezelő telepítési modell útvonalak](virtual-network-create-udr-arm-template.md) és alhálózathoz. 
- Megtudhatja, hogy miként hozhat [létre a klasszikus telepítési modell útvonalak](virtual-network-create-udr-classic-ps.md) és alhálózathoz.
