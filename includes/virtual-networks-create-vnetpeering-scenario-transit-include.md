## <a name="service-chaining---transit-through-peered-vnet"></a>Szolgáltatás egybefűzés - peered VNet keresztül

Annak ellenére, hogy a rendszer útvonalak használatát automatikusan a telepítéshez forgalom megkönnyíti, vannak, amelyben meg szeretné adni egy virtuális készülék keresztül csomagok útválasztás esetek.
Ebben az esetben nincsenek két VNets HubVNet és VNet1, ahogy az alábbi ábrán egy előfizetés. Hálózati virtuális Appliance(NVA) VNet HubVNet a rendszerbe. Miután létrejött a VNet peering HubVNet és VNet1 között, állítsa be a felhasználó által definiált utakat, és adja meg a következő ugrás a NVA a HubVNet.

![NVA hálózaton átvitt](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Az az egyszerűség kedvéért feltételezik itt az összes VNets ugyanabban az előfizetésben. De keresztté-előfizetés forgatókönyvet is működik.

A fő hálózaton átvitt Útválasztás engedélyezése tulajdonsága az "Továbbított forgalom engedélyezése" paramétert. Ez lehetővé teszi, hogy elfogadja vagy a forgalmat a és a NVA elküldése a peered VNet.  
