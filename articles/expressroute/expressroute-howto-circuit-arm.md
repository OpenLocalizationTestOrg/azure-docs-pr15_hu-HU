<properties
   pageTitle="Létrehozása és módosítása az készült ExpressRoute áramkör erőforrás-kezelő és a PowerShell használatával |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan hozhat létre, kiépítése, ellenőrizze, módosítása, törlése és az készült ExpressRoute áramkör deprovision."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Létrehozása és módosítása az készült ExpressRoute áramkör

> [AZURE.SELECTOR]
[Azure portál - erőforrás-kezelő](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - erőforrás-kezelő](expressroute-howto-circuit-arm.md)
[PowerShell - klasszikus](expressroute-howto-circuit-classic.md)


Ez a cikk ismerteti a készült Azure ExpressRoute áramkör létrehozása a Windows PowerShell-parancsmagok és az erőforrás-kezelő Azure telepítési modell használatával. Ez a cikk is bemutatja, hogyan a kapcsolat állapotának ellenőrzése, telepítse azt, vagy törölheti vagy deprovision azt.

**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Első lépések


- Az Azure PowerShell-modul legújabb verziójának beszerzése (legalább 1.0 verziójú). Hogyan állítható be a PowerShell-modulok használni a számítógép lépésenkénti útmutatót találhat kövesse a [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md).

- Tekintse át a [előfeltételei](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurációs megkezdése előtt.

## <a name="create-and-provision-an-expressroute-circuit"></a>Hozzon létre, és egy készült ExpressRoute áramkör kiépítése

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Jelentkezzen be az Azure-fiókjába, és jelölje ki azt az előfizetést

A konfiguráció megkezdéséhez jelentkezzen be az Azure-fiók. PowerShell kapcsolatos további tudnivalókért olvassa el a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)című témakört. Használja az alábbi példák csatlakozni:

    Login-AzureRmAccount

Jelölje be a fiók az előfizetések:

    Get-AzureRmSubscription

Jelölje ki azt az előfizetést, amelyet az készült ExpressRoute áramkör létrehozása:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. a lista beszerzése az támogatott szolgáltatók, helyek és sávszélessége

Mielőtt hoz létre egy készült ExpressRoute áramkör, szüksége támogatott kapcsolódási szolgáltatók, a helyek és a sávszélesség-beállítások listája.

A PowerShell-parancsmag `Get-AzureRmExpressRouteServiceProvider` adja eredményül ezt az információt, amely újabb lépéseket kell megadnia:

    Get-AzureRmExpressRouteServiceProvider

Ellenőrizze, hogy ha a kapcsolat szolgáltatója nem szerepel a listában. Jegyezze le a következő információkat, mert szüksége lesz rá később a kapcsolatok létrehozásakor:

- név

- PeeringLocations

- BandwidthsOffered

Most már készen készült ExpressRoute áramkör létrehozása.   

### <a name="3-create-an-expressroute-circuit"></a>3. a készült ExpressRoute áramkör létrehozása

Ha még nincs erőforráscsoport, egy a készült ExpressRoute áramkör létrehozása előtt kell létrehoznia. Ezt a következő parancs futtatásával teheti meg:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


A következő példa bemutatja, hogyan hozhat létre egy 200 MB készült ExpressRoute áramkör keresztül Equinix szilícium völgyi. Ha más szolgáltatója és más beállításokat használ, helyettesítő ezeket az információkat, hogy a kérést. Az alábbi példa egy új szolgáltatás kulcs felkérés el:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Győződjön meg arról, hogy a helyes Termékváltozat réteg és Termékváltozat család megadása:

- Termékváltozat réteg határozza meg, hogy egy készült ExpressRoute normál vagy a készült ExpressRoute prémium bővítmény engedélyezve van. Megadhatja, hogy *szabványos* a szokásos Termékváltozat vagy *prémium* beszerzése a prémium bővítményt.

- Termékváltozat család határozza meg, hogy a számlázási típusát. A forgalmi díjas mobilinternet-előfizetéssel és *Unlimiteddata* *Metereddata* az egy adatforgalmi is megadhat. Megjegyzés:, hogy a számlázási típusát a módosíthatja *Metereddata* *Unlimiteddata*, de nem módosíthatja a típus *Unlimiteddata* a *Metereddata*.


>[AZURE.IMPORTANT] A készült ExpressRoute áramkör fog egy szolgáltatás kulcs kibocsátott pillanatától számlát kapni. Győződjön meg arról, hogy ha a kapcsolat szolgáltatója a áramkör kiépítése készen áll a művelet végrehajtása.

A válasz a szolgáltatás kulcs tartalmaz. Szerezheti be az összes paramétert részletes leírását a következő futtatásával:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. a lista összes készült ExpressRoute áramkörök

Úgy juthat az Ön által létrehozott összes készült ExpressRoute kapcsolatok listáját, futtassa a `Get-AzureRmExpressRouteCircuit` parancsot:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

A válasz jelenik meg a következőhöz hasonló:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Ezek az információk bármikor használatával meghallgathatja a `Get-AzureRmExpressRouteCircuit` parancsmag. Az összes áramkörök hívást nincs paraméterekkel sorolja fel. A *ServiceKey* mező szerepelni fognak a szolgáltatás használatával:


    Get-AzureRmExpressRouteCircuit


A válasz jelenik meg a következőhöz hasonló:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Szerezheti be az összes paramétert részletes leírását a következő futtatásával:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. küldése a szolgáltatás billentyűt a kapcsolat szolgáltatója a kiépítéséhez

*ServiceProviderProvisioningState* -szolgáltató oldalán kiépítési aktuális állapotát információt tartalmaz. Állapot Ez a témakör a Microsoft egymás állapotát. További információt a kiépítési államok áramkör a [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) témakört is.

Amikor létrehoz egy új készült ExpressRoute áramkör, a kapcsolat a következő állapotban lesz:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



A kapcsolat változni fog a következő állapotát a kapcsolat szolgáltatója webhelyet, amely lehetővé teszi Önnek:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Szeretné használni az készült ExpressRoute áramkör a következő állapotban kell lennie:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. rendszeres időközönként ellenőrizze, hogy az állapot és a áramkör kulcs állapota

Az állapot és a áramkör kulcs állapotának ellenőrzése lehetővé teszi, hogy ha a szolgáltató engedélyezett-e a áramkör. Miután a kapcsolat van konfigurálva, *ServiceProviderProvisioningState* *Provisioned*, mint jelenik meg az alábbi példában látható módon:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


A válasz jelenik meg a következőhöz hasonló:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                    }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. a útválasztási konfiguráció létrehozása

Részletes útmutató ismertető [készült ExpressRoute áramkör útválasztási konfigurációs](expressroute-howto-routing-arm.md) hozhat létre és módosíthat áramkör peerings.


>[AZURE.IMPORTANT] Ezeket az utasításokat tartalmazó réteget 2 kapcsolatszolgáltatások szolgáltatókkal létrehozott áramkörök csak vonatkoznak. Egy szolgáltatót, amely felajánlja a felügyelt használata layer 3 szolgáltatások (általában egy IP VPN, például MPLS), a kapcsolat szolgáltatója fog konfigurálása és kezelése a továbbítás meg.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. hivatkozás egy készült ExpressRoute áramkör virtuális hálózaton

Ezután a virtuális hálózati a készült ExpressRoute áramkört ábrázoló mutató hivatkozás A [virtuális hálózatok Linking készült ExpressRoute áramkörök](expressroute-howto-linkvnet-arm.md) cikk dolgozik, az erőforrás-kezelő telepítési modell használata

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Bevezetés az készült ExpressRoute áramkör állapotát

Ezek az információk bármikor használatával meghallgathatja a `Get-AzureRmExpressRouteCircuit` parancsmag. Az összes áramkörök hívást nincs paraméterekkel sorolja fel.

    Get-AzureRmExpressRouteCircuit


A válasz a következőhöz hasonló lesz:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Egy adott készült ExpressRoute áramkör információkat kaphat megkerülhetők erőforrás csoport nevét és áramkör paraméterként a hívásba:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


A válasz jelenik meg a következőhöz hasonló:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Szerezheti be az összes paramétert részletes leírását a következő futtatásával:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Az készült ExpressRoute áramkör módosítása

Az készült ExpressRoute áramkör bizonyos tulajdonságai módosíthatók érintő kapcsolat nélkül.

Nincs legrövidebb leállás az alábbi műveleteket végezheti el:

- Engedélyezheti vagy letilthatja a készült ExpressRoute áramkör prémium készült ExpressRoute bővítményt.
- A sávszélesség a készült ExpressRoute áramkör növelése Figyelje meg, hogy a Visszalépés a kapcsolat a sávszélesség-ról nem támogatott.
- A mérési terv átállítása forgalmi adatok adatforgalmi. Figyelje meg, hogy a mérési csomagot az adatforgalmi forgalmi adatokat nem lehet módosítani.
-  Engedélyezze, és tiltsa le a *Klasszikus működés engedélyezése*.

Korlátozások és érvényes korlátozások a további tudnivalókért tekintse át a [Készült ExpressRoute – gyakori kérdések](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Ahhoz, hogy a készült ExpressRoute prémium bővítmény

A meglévő kapcsolat használatával az alábbi PowerShell kódtöredékének engedélyezheti a készült ExpressRoute prémium bővítmény:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


A kapcsolat ettől a készült ExpressRoute prémium bővítmény funkció engedélyezve lesz. Figyelje meg, hogy azt meg is kezdi a számlázás, a prémium bővítmény képesség, amint az a parancs sikeres futtatását.

### <a name="to-disable-the-expressroute-premium-add-on"></a>A készült ExpressRoute prémium bővítmény letiltása

>[AZURE.IMPORTANT] Ez a művelet sikertelen lesz információforrások, amelyek a nagyobb, mint a megengedett a szabványos áramkör használata.

Vegye figyelembe az alábbiakat:

- Mielőtt, vissza léptetheti prémium a szabványos, győződjön meg róla, hogy a áramkör hozzákapcsolhatja virtuális hálózatok értéke kisebb, mint 10. Ha nem ezek, kérését a frissítés sikertelen lesz, és fog számlázás, prémium kamatlába mellett.

- Az összes virtuális hálózatok más geopolitikai régiókban szétválasztása Ha nem ezek, kérését a frissítés sikertelen lesz, és fog számlázás, prémium kamatlába mellett.

- Az útvonal táblázat kisebb, mint 4000 útvonalak magánjellegű peering kell lennie. Az útvonal táblázat mérete 4000 útvonalak nagyobb, ha a BGP munkamenet beilleszti, és nem kell újra engedélyezve, amíg hirdetett prefixumokban száma alatti 4000 kerül.

A meglévő kapcsolat készült ExpressRoute prémium bővítményt letilthatja az alábbi PowerShell-parancsmag használatával:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>A készült ExpressRoute áramkör sávszélesség frissítése

Támogatott sávszélesség-beállítások a szolgáltató olvassa el a [Készült ExpressRoute – gyakori kérdések](expressroute-faqs.md). Bármilyen nagyobb, mint a már meglévő áramkör méretének méretet is választhat.

>[AZURE.IMPORTANT] A sávszélesség-készült ExpressRoute áramkör zavartalanul nem csökkentése. Visszalépés a sávszélesség ról megköveteli a készült ExpressRoute áramkör deprovision, és ezután reprovision egy új készült ExpressRoute áramkör.

Miután úgy dönt, hogy milyen méretű van szüksége, használja a következő parancs méretezze át a áramkör:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


A kapcsolat fog méretű, a Microsoft oldalon. Ezután kapcsolatba kell lépnie a csatlakozási szolgáltató frissítése konfigurációk újranyitásához felel meg ezt a módosítást. Miután elvégezte ezt az értesítést, azt is kezdi a számlázás, a frissített sávszélesség lehetőséget.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Ugrás a Termékváltozat a forgalmi korlátlan

Módosíthatja az készült ExpressRoute áramkör SKU az alábbi PowerShell kódtöredék használatával:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>A klasszikus és erőforrás-kezelő környezetekben való hozzáférés szabályozása  

Tekintse át [az erőforrás-kezelő telepítési adatmodellhez klasszikus áthelyezése készült ExpressRoute áramkörök](expressroute-howto-move-arm.md)utasításait.  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Megszüntetés, és egy készült ExpressRoute áramkör törlése

Vegye figyelembe az alábbiakat:

- A készült ExpressRoute áramköri az összes virtuális hálózatok szétválasztása Ha nem sikerül a művelet, ellenőrizze, hogy ha bármelyik virtuális hálózatok a áramkör vannak csatolva.

- Ha a készült ExpressRoute áramkör service provider kiépítési állapot **létesítése** vagy **Provisioned** a szolgáltatót a kapcsolat az oldalon deprovision kell dolgozni. Erőforrások lefoglalása és bill, amíg a szolgáltató befejeződik, a áramkör megszüntetés, és értesítést küld a kapcsolatfelvételi folytatjuk.

- Ha a szolgáltató van leépítve az (a szolgáltatás szolgáltató kiépítési állapotának értéke **nincs kiépítve**) áramkör törölje a áramkör. Ezzel leállítja a áramkör számlázását

A készült ExpressRoute áramkör törölheti a következő parancs futtatásával:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Következő lépések

Miután létrehozta a áramkör, ügyeljen, tegye a következőket:

- [Létrehozása és módosítása a készült ExpressRoute áramkör továbbítása](expressroute-howto-routing-arm.md)
- [A virtuális hálózat csatolása a készült ExpressRoute áramkör](expressroute-howto-linkvnet-arm.md)
