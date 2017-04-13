<properties
   pageTitle="Létrehozása és módosítása az készült ExpressRoute áramkör a klasszikus telepítési modell és a PowerShell használatával |} Microsoft Azure"
   description="Ez a cikk végigvezeti a létrehozásához, és egy készült ExpressRoute áramkör kiépítési. Ez a cikk is bemutatja, hogyan nézze meg, állapot, frissítés, törlés és a áramkör deprovision."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Létrehozása és módosítása az készült ExpressRoute áramkör

> [AZURE.SELECTOR]
[Azure portál - erőforrás-kezelő](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - erőforrás-kezelő](expressroute-howto-circuit-arm.md)
[PowerShell - klasszikus](expressroute-howto-circuit-classic.md)

Ebben a cikkben megismerkedhet a készült Azure ExpressRoute áramkör létrehozása PowerShell-parancsmagok és a klasszikus telepítési modell lépéseit. Ez a cikk is megtudhatja, hogy miként nézze meg, állapot, frissítés, törlés és az készült ExpressRoute áramkör deprovision.

**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Első lépések

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. a Előfeltételek és munkafolyamat cikkek áttekintése

Győződjön meg arról, hogy ellenőrzését [előfeltételei](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurációs megkezdése előtt.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. a telepítse az Azure PowerShell-modul legújabb verziója 

Kövesse a [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md) lépésenkénti útmutatót találhat az Azure PowerShell-modulok használni a számítógép beállításával.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. Jelentkezzen be az Azure-fiókjába, és jelöljön ki egy előfizetést

1. Futtassa a következő parancsmagot egy jogú Windows PowerShell-parancssorában használata:

        Add-AzureAccount
2. A bejelentkezési képernyőn megjelenő jelentkezzen be a fiókjába.

3. Ismerkedés a előfizetések listáját.

        Get-AzureSubscription
4. Válassza ki a használni kívánt előfizetést.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Hozzon létre, és egy készült ExpressRoute áramkör kiépítése

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. a PowerShell-modulok importálása készült ExpressRoute

 Ha még nem tette meg, importálnia kell az Azure és készült ExpressRoute modulok be a PowerShell-munkamenetet abból a készült ExpressRoute parancsmagok használatának megkezdéséhez. A modulok importálása a helyet, azok telepített a helyi számítógépre. Attól függően, hogy a használta a modulokat módszert a hely lehet ugyanaz, mint az alábbi példában látható. Ha szükséges, módosítsa a példában.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. a lista beszerzése az támogatott szolgáltatók, helyek és sávszélessége

Mielőtt hoz létre egy készült ExpressRoute áramkör, szüksége támogatott kapcsolódási szolgáltatók, a helyek és a sávszélesség-beállítások listája.

A PowerShell-parancsmag `Get-AzureDedicatedCircuitServiceProvider` adja eredményül ezt az információt, amely újabb lépéseket kell megadnia:

    Get-AzureDedicatedCircuitServiceProvider

Ellenőrizze, hogy ha a kapcsolat szolgáltatója nem szerepel a listában. Jegyezze le a következő információkat, mert szüksége lesz rá később a kapcsolatok létrehozásakor:

- név

- PeeringLocations

- BandwidthsOffered

Most már készen készült ExpressRoute áramkör létrehozása.         

### <a name="3-create-an-expressroute-circuit"></a>3. a készült ExpressRoute áramkör létrehozása

A következő példa bemutatja, hogyan hozhat létre egy 200 MB készült ExpressRoute áramkör keresztül Equinix szilícium völgyi. Ha más szolgáltatója és más beállításokat használ, helyettesítő ezeket az információkat, hogy a kérést.

>[AZURE.IMPORTANT] A készült ExpressRoute áramkör fog egy szolgáltatás kulcs kibocsátott pillanatától számlát kapni. Győződjön meg arról, hogy ha a kapcsolat szolgáltatója a áramkör kiépítése készen áll a művelet végrehajtása.


Az alábbi példa egy új szolgáltatás kulcs felkérés el:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Vagy, ha szeretne egy készült ExpressRoute áramkör létrehozását a prémium bővítmény, az alábbi példa. Keresse meg a [Készült ExpressRoute – gyakori kérdések](expressroute-faqs.md) a prémium bővítmény olvashat bővebben.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


A válasz tartalmazni fogja a szolgáltatás billentyűt. Szerezheti be az összes paramétert részletes leírását a következő futtatásával:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. a lista összes készült ExpressRoute áramkörök

Futtatását is lehetővé teszi a `Get-AzureDedicatedCircuit` lista beszerzése az Ön által létrehozott összes készült ExpressRoute áramkörök parancsot:


    Get-AzureDedicatedCircuit

A válasz a következő példa hasonló lesz:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Ezek az információk bármikor használatával meghallgathatja a `Get-AzureDedicatedCircuit` parancsmag. Az összes áramkörök paraméter nélkül a hívás sorolja fel. A *ServiceKey* mező szerepelni fognak a szolgáltatás használatával.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Szerezheti be az összes paramétert részletes leírását a következő futtatásával:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. küldése a szolgáltatás billentyűt a kapcsolat szolgáltatója a kiépítéséhez


*ServiceProviderProvisioningState* információt nyújt az aktuális állapotát kiépítési-szolgáltató oldalán. *Állapot* Ez a témakör a Microsoft egymás állapotát. További információt a kiépítési államok áramkör a [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) témakört is.

Amikor létrehoz egy új készült ExpressRoute áramkör, a kapcsolat a következő állapotban lesz:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


A kapcsolat az alábbi állam ugrik, ha a kapcsolat szolgáltatója webhelyet, amely lehetővé teszi Önnek:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Az készült ExpressRoute áramkör szeretné használni, akkor a következő állapotban kell lennie:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. rendszeres időközönként ellenőrizze, hogy az állapot és a áramkör kulcs állapota

Lehetővé teszi, hogy ha a szolgáltató engedélyezett-e a áramkör. A kapcsolat beállítása után *ServiceProviderProvisioningState* *Provisioned* szerint jelennek meg az alábbi példában látható módon:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. a útválasztási konfiguráció létrehozása

Hivatkozhat a [készült ExpressRoute áramkör útválasztási konfigurációs (létrehozása és módosítása áramkör peerings)](expressroute-howto-routing-classic.md) cikkben részletes útmutatást.

>[AZURE.IMPORTANT] Ezeket az utasításokat tartalmazó réteget 2 kapcsolatszolgáltatások szolgáltatókkal létrehozott áramkörök csak vonatkoznak. Egy szolgáltatót, amely felajánlja a felügyelt használata layer 3 szolgáltatások (általában egy IP VPN, például MPLS), a kapcsolat szolgáltatója fog konfigurálása és kezelése a továbbítás meg.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. hivatkozás egy készült ExpressRoute áramkör virtuális hálózaton

Ezután a virtuális hálózati a készült ExpressRoute áramkört ábrázoló mutató hivatkozás Olvassa el [a virtuális hálózatokhoz csatolása készült ExpressRoute áramkörök](expressroute-howto-linkvnet-classic.md) lépésenkénti útmutatást. Ha kell hozzon létre egy virtuális hálózati készült ExpressRoute a klasszikus telepítési modellben használva, olvassa el a [készült ExpressRoute virtuális hálózat létrehozása](expressroute-howto-vnet-portal-classic.md) utasításokat.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Bevezetés az készült ExpressRoute áramkör állapotát

Ezek az információk bármikor használatával meghallgathatja a `Get-AzureCircuit` parancsmag. Az összes áramkörök paraméter nélkül a hívás sorolja fel.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Egy adott készült ExpressRoute áramkör információkat úgy is megnyithatja, hogy a szolgáltatás kulcs paraméterként a hívás átadása:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Szerezheti be az összes paramétert részletes leírását a következő futtatásával:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Az készült ExpressRoute áramkör módosítása

Az készült ExpressRoute áramkör bizonyos tulajdonságai módosíthatók érintő kapcsolat nélkül.

Nincs legrövidebb leállás az alábbi műveleteket végezheti el:

- Engedélyezheti vagy letilthatja a készült ExpressRoute áramkör prémium készült ExpressRoute bővítményt.
- A sávszélesség a készült ExpressRoute áramkör növelése Figyelje meg, hogy a Visszalépés a kapcsolat a sávszélesség-ról nem támogatott.
- A mérési terv átállítása forgalmi adatok adatforgalmi. Figyelje meg, hogy a mérési csomagot az adatforgalmi forgalmi adatokat nem lehet módosítani.
- Engedélyezze, és tiltsa le a *Klasszikus működés engedélyezése*.

Keresse meg a [Készült ExpressRoute gyakran ismételt kérdések](expressroute-faqs.md) korlátai és érvényes korlátozások további információt.

### <a name="to-enable-the-expressroute-premium-add-on"></a>Ahhoz, hogy a készült ExpressRoute prémium bővítmény

A meglévő kapcsolat az alábbi PowerShell-parancsmag használatával engedélyezheti a készült ExpressRoute prémium bővítmény:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

A kapcsolat ettől a készült ExpressRoute prémium bővítmény funkció engedélyezve lesz. Figyelje meg, hogy azt a program ekkor elkezdi a prémium bővítmény képesség számlázás, amint az a parancs sikeres futtatását.

### <a name="to-disable-the-expressroute-premium-add-on"></a>A készült ExpressRoute prémium bővítmény letiltása

>[AZURE.IMPORTANT] Ez a művelet sikertelen lehet nagyobb, mint a megengedett a szabványos áramkör erőforrások használata.

Vegye figyelembe az alábbiakat:

- Győződjön meg arról, hogy a áramkör csatolva virtuális hálózattal argumentuma szám 10-nél kisebb előtt vissza az prémium standard léptetheti. Ha nem művelet sikertelen lesz a frissítési kérést, és is a díjtételek számlát kapni.

- Az összes virtuális hálózatok más geopolitikai régiókban szétválasztása Ha nem művelet sikertelen lesz a frissítési kérést, és is a díjtételek számlát kapni.

- Az útvonal táblázat kisebb, mint 4000 útvonalak magánjellegű peering kell lennie. 4000 útvonalak nagyobb az útvonal táblázat méretét, ha a BGP munkamenet húzhatja lesz, és nem kell újra engedélyezve, amíg alatti 4000 Ugrás hirdetett prefixumokban számát.

A meglévő kapcsolatok készült ExpressRoute prémium bővítményt letilthatja az alábbi PowerShell-parancsmag használatával:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>A készült ExpressRoute áramkör sávszélesség frissítése

Jelölje be a szolgáltató által támogatott sávszélesség lehetőségei a [Készült ExpressRoute – gyakori kérdések](expressroute-faqs.md) . Bármilyen méretű, amely nagyobb mint a már meglévő áramkör méretének mindaddig, amíg a fizikai port (amelyeken a kapcsolat jön létre) lehetővé teszi, hogy is választhat.

>[AZURE.IMPORTANT] A sávszélesség-készült ExpressRoute áramkör zavartalanul nem csökkentése. Visszalépés a sávszélesség-ról kell, hogy a készült ExpressRoute áramkör deprovision, és ezután reprovision egy új készült ExpressRoute áramkör.

Miután úgy dönt, hogy milyen méretű van szüksége, a következő paranccsal méretezze át a áramkör:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

A kapcsolat fog van már méretű a Microsoft oldalon. Kapcsolatba kell lépnie a csatlakozási szolgáltató frissítése konfigurációk újranyitásához felel meg ezt a módosítást. Figyelje meg, hogy azt a program ekkor elkezdi a számlázás, ettől kezdve a frissített sávszélesség lehetőséget.

Ha megjelenik a következő hiba, amikor a áramkör sávszélesség növelése, akkor ott nem elegendő a sávszélesség a fizikai port, ahol a meglévő kapcsolat jön létre a bal. Ha törli a áramkör, és hozzon létre egy új kapcsolat a szükséges méretét. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Megszüntetés, és egy készült ExpressRoute áramkör törlése

Vegye figyelembe az alábbiakat:

- A művelethez a készült ExpressRoute áramkör az összes virtuális hálózatok szétválasztása Ellenőrizze, hogy van-e bármilyen virtuális hálózatok, amely a áramkör vannak csatolva, ha a művelet sikertelen lesz.

- Ha a készült ExpressRoute áramkör service provider kiépítési állapot **létesítése** vagy **Provisioned** a szolgáltatót a kapcsolat az oldalon deprovision kell dolgozni. Erőforrások lefoglalása és bill, amíg a szolgáltató befejeződik, a áramkör megszüntetés, és értesítést küld a kapcsolatfelvételi folytatjuk.

- Ha a szolgáltató van leépítve az (a szolgáltatás szolgáltató kiépítési állapotának értéke **nincs kiépítve**) áramkör törölje a áramkör. Ezzel leállítja a áramkör számlázását.

A készült ExpressRoute áramkör törölheti a következő parancs futtatásával:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Következő lépések

Miután létrehozta a áramkör, ügyeljen, tegye a következőket:

- [Létrehozása és módosítása a készült ExpressRoute áramkör továbbítása](expressroute-howto-routing-classic.md)
- [A virtuális hálózat csatolása a készült ExpressRoute áramkör](expressroute-howto-linkvnet-classic.md)
