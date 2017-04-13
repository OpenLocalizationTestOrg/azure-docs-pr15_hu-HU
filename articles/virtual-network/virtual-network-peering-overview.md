
<properties
   pageTitle="Azure virtuális hálózati peering |} Microsoft Azure"
   description="Tudjon meg többet az Azure peering VNet."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>VNet peering

A két virtuális hálózatok (VNets) kapcsolódó ugyanabban a régióban az Azure Gerinchez hálózaton keresztül mechanizmusa VNet peering. Miután peered, a két virtuális hálózat egyik minden kapcsolat célra jelennek meg. Továbbra is külön erőforrásként kezelése, de saját IP-címek segítségével közvetlenül az alábbi virtuális hálózatok virtuális gépeken futó kommunikálhat egymással.

A forgalom virtuális gépeken futó a peered virtuális hálózatok között az Azure infrastruktúra továbbít munkaterületekhez hasonló a forgalom továbbítás virtuális ugyanabba a hálózatba VMs között. A előnyei VNet peering a következők:

- A kis-késés, a nagy sávszélességű kapcsolat erőforrások különböző virtuális hálózatok között.
- A gyorselemzés erőforrások, például hálózati készülékek és a virtuális Magánhálózati átjárók egy peered VNet a hálózaton átvitt pontként.
- Az azt jelenti, hogy virtuális hálózatot, amely egy virtuális hálózatot, amely a klasszikus telepítési modellt, és ezek a virtuális hálózatok az erőforrások teljes összekapcsolását engedélyezése a Azure erőforrás-kezelő modellt.

Követelmények és VNet peering főbb részei:

- A két virtuális hálózatot, hogy vannak-e peered ugyanabban a Azure régióban kell lennie.
- A virtuális hálózatok, hogy vannak-e peered IP-cím szóközöket mozaikszerűen kell rendelkeznie.
- VNet peering virtuális hálózatok között, és nincs származtatott tranzitív kapcsolat. Például ha virtuális a hálózathoz van peered virtuális hálózattal B, és ha virtuális hálózati B C virtuális hálózattal van peered, azt nem fordítása való virtuális hálózati egy éppen peered virtuális hálózattal C.
- Peering hozható létre a két különböző előfizetések virtuális hálózatok között ideig a jogosultságokkal rendelkező felhasználó mindkét előfizetések engedélyezi a peering és az előfizetések az Active Directory ugyanahhoz a bérlőhöz van társítva. 
- Erőforrás-kezelő modell virtuális hálózati és klasszikus telepítési modell között peering igényel, hogy a VNets ugyanabban az előfizetésben kell lennie.
- A virtuális hálózat, amelyhez az erőforrás-kezelő telepítési modell is peered, egy másik virtuális hálózat, amely használja ezt a modellt, vagy egy virtuális hálózat, amely használja a klasszikus telepítési modell. A klasszikus telepítési modell használó virtuális hálózatok peered nem egymással.
- Abban az esetben, ha a közlemény virtuális gépeken futó peered virtuális hálózatok között van további sávszélesség korlátozás nélkül, sávszélesség vonalvég virtuális mérete alapján is érinti.


![Egyszerű VNet peering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Kapcsolat
Két virtuális hálózatok peered vannak, miután egy virtuális gép (webes/dolgozó szerepkör) a virtuális hálózaton közvetlenül csatlakoztathatja más virtuális gépeken futó a peered virtuális hálózaton. Ezek a két hálózatok teljes IP-szinttel rendelkezik kapcsolatot.

A hálózati késés a két virtuális gépeken futó peered virtuális hálózatok között egy üzenetváltási ugyanúgy történik, mint egy üzenetváltási virtuális helyi hálózaton belül. A hálózati teljesítmény alapján a sávszélesség a virtuális gép arányos méretének tartalmaz-e. A sávszélesség további korlátozás nem.

Közvetlenül az Azure háttéradatbázist infrastruktúra és az átjáró révén nem továbbítás a forgalmat a virtuális gépeken futó peered virtuális hálózatok között.

A belső terheléselosztás (ILB) végpontok a peered virtuális hálózaton virtuális gépeken futó virtuális hálózatban érheti el. Hálózati biztonsági csoportok (NSGs) vagy más virtuális hálózatok vagy alhálózat hozzáférésének letiltása, ha azt szeretné, hogy virtuális hálózati is alkalmazható.

Amikor a felhasználók beállítása peering, azt nyissa meg, vagy zárja be a NSG szabályokat a virtuális hálózatok között. Ha a felhasználó úgy dönt, hogy nyissa meg a teljes összekapcsolását peered virtuális hálózatok (Ez az alapértelmezett beállítás), majd segítségével NSGs adott alhálózat vagy virtuális gépeken futó blokkolni vagy adott elutasítsa.

Azure által biztosított belső DNS-névfeloldás a virtuális gépeken futó peered virtuális hálózaton keresztül nem működik. Virtuális gépeken futó van a belső DNS-nevek, amelyek felbontási csak a helyi virtuális hálózaton belül. Felhasználók is beállíthatja azonban virtuális gépeken futó, mint egy virtuális hálózati DNS-kiszolgálóiról az peered virtuális hálózatok futó.

## <a name="service-chaining"></a>A szolgáltatás láncolás
Felhasználók beállíthatják, mutasson a virtuális gépeken futó peered virtuális hálózatok, a "következő ugrás" IP-címet, felhasználó által definiált útvonal táblázatok, a jelen cikk az ábrán látható módon. Ez lehetővé teszi a felhasználóknak elérése láncolás szolgáltatás, keresztül, amely szerint irányíthatják egy virtuális készülék peered virtuális hálózaton keresztül felhasználói útvonal táblák futtató egyik virtuális hálózatból forgalmat.

Felhasználók is hatékonyan építhet központi sugaras típus környezetekben, ahol az a központi üzemeltetheti infrastruktúra összetevők, például egy hálózati virtuális készülék. Azt, valamint a központi virtuális hálózat futó készülékek forgalom csak egy részhalmazát, majd peer a a sugaras virtuális hálózatok is. Röviden VNet peering lehetővé teszi, hogy a "felhasználó által definiált útvonal táblázat" a következő ugrás IP-címét a peered virtuális hálózaton virtuális gép IP-címe lesz.

## <a name="gateways-and-on-premises-connectivity"></a>Átjárók és a helyszíni kapcsolat
Minden virtuális hálózati, függetlenül attól, hogy peered egy másik virtuális hálózattal továbbra is saját átjáró van, és csatlakozás helyszíni használatával. Felhasználók is beállítható [VNet-VNet kapcsolatok](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) átjárók segítségével akkor is, ha a virtuális hálózatok peered vannak.

Virtuális hálózati összekapcsolhatóságának mindkét lehetőségei vannak beállítva, amikor a forgalmat a virtuális hálózatok között halad-e a peering konfiguráció keresztül (Ez azt jelenti, hogy az Azure Gerinchez keresztül).

Virtuális hálózatok peered vannak, ha felhasználók is beállíthatja az átjáró a peered virtuális hálózaton a helyszíni hálózaton átvitt pontként. Ebben az esetben a virtuális hálózat távoli az átjárók használó saját átjáró nem lehet. Egy virtuális hálózati beállíthatja, hogy csak egy átjárót. Ez lehet egy helyi átjáró vagy egy távoli átjáró (a peered virtuális hálózat), az alábbi képen látható módon.

Átjáró hálózaton átvitt peering kapcsolatának virtuális hálózatot használ, az erőforrás-kezelő modell, és a klasszikus telepítési modell használata nem támogatott. A kapcsolatban részt vevő peering mindkét virtuális hálózatok kell egy átjáró hálózaton átvitt az erőforrás-kezelő telepítési modell használata.

A virtuális hálózatokat egyetlen a készült Azure ExpressRoute kapcsolat által megosztott peered vannak, ha a forgalom közöttük peering kapcsolaton keresztül halad-e (Ez azt jelenti, hogy az Azure Gerinchez hálózaton keresztül). Felhasználók továbbra is használhat helyi átjárók minden virtuális hálózati a helyszíni áramkör csatlakozni. Azt is megteheti azok használni egy megosztott átjárót, és állítsa be a kapcsolatot a helyszíni hálózaton átvitt.

![VNet peering hálózaton átvitt](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Kiépítése
Kiemelt művelet VNet peering. Érdemes csoportban a VirtualNetworks névtér külön függvényt. Egy felhasználó adott tartalomvédelmi peering engedélyezni kell megadni. Egy felhasználó, aki rendelkezik írási és olvasási hozzáférést a virtuális hálózat mindezen jogok automatikusan örökli.

A felhasználó, aki vagy rendszergazda vagy peering lehetővé teszi jogosultsággal rendelkező felhasználó egy másik VNet peering művelet kezdeményezhet. Ha a másik oldalon peering egyező kér, és az egyéb feltételek teljesülése, a peering jön létre.

Keresse meg a "Következő lépések" című szakaszt a virtuális hálózatok között peering VNet létrehozásával kapcsolatos további tudnivalók a cikkeket.

## <a name="limits"></a>Korlátai
Vannak olyan a virtuális egyetlen hálózat áll rendelkezésre peerings számával. [Azure hálózati korlátozások](../azure-subscription-service-limits.md#networking-limits) vonatkoznak a további információt.

## <a name="pricing"></a>Árak
A Véleményezés időszakban ingyenesen lesz VNet peering. Után megjelent, lesz a névleges díjat be- és kilépési, amelyik a peering forgalmat. További tudnivalókért tekintse át az [ár, oldal](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Következő lépések
- A [virtuális hálózatok között peering beállítása](virtual-networks-create-vnetpeering-arm-portal.md).
- Tudjon meg többet [NSGs](virtual-networks-nsg.md).
- Tudjon meg többet a [felhasználó által definiált útvonalak és IP-továbbítás](virtual-networks-udr-overview.md).
