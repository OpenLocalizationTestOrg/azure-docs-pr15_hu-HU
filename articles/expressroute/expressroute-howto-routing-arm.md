<properties
   pageTitle="Egy készült ExpressRoute áramkör továbbítás beállítása |} Microsoft Azure"
   description="Ez a cikk végigvezeti a létrehozásához, és a lehetőséget választva személyes, nyilvános és a Microsoft-készült ExpressRoute áramkör peering kiépítési. Ez a cikk is bemutatja, hogyan ellenőrizheti az állapotát, frissíthet és törölhet az áramkör peerings."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Létrehozása és módosítása az készült ExpressRoute áramkör Útválasztás


> [AZURE.SELECTOR]
[Azure portál - erőforrás-kezelő](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - erőforrás-kezelő](expressroute-howto-routing-arm.md)
[PowerShell - klasszikus](expressroute-howto-routing-classic.md)



Az alábbiakban ismertetjük, létrehozása és kezelése egy PowerShell-és az erőforrás-kezelő Azure telepítési modell készült ExpressRoute áramkör útválasztás beállításának lépéseit.  Az alábbi lépésekkel is bemutatja, hogyan nézze meg, állapot, frissítés, törlés és egy készült ExpressRoute áramkör peerings deprovision. 


**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfigurációs vonatkozó követelmények

- Szüksége lesz a legújabb verzióját az Azure PowerShell-modult, 1.0 vagy újabb verziója. 
- Győződjön meg arról, hogy ellenőrzését a [Előfeltételek](expressroute-prerequisites.md) lapon, a [útválasztási követelményeinek](expressroute-routing.md) lap és a [munkafolyamatok](expressroute-workflows.md) lap konfigurációs megkezdése előtt.
- Az aktív készült ExpressRoute áramkör kell rendelkeznie. Kövesse az utasításokat létrehozása [egy készült ExpressRoute áramkör](expressroute-howto-circuit-arm.md) , és a folytatás előtt engedélyezve van a kapcsolat szolgáltatója áramkör van. A készült ExpressRoute áramkör ki láthatja az alábbiakban ismertetett parancsmag kiépített és engedélyezett állapotban kell lennie.

Ezeket az utasításokat a létrehozott réteget 2 adatkapcsolat szolgáltatásokat kínáló szolgáltatókkal áramkörök csak vonatkoznak. Ha használja a szolgáltató kínáló felügyelt Layer 3 szolgáltatások (általában egy IPVPN, például MPLS), a kapcsolat szolgáltatója konfigurálása, és meg útválasztás kezelése.

>[AZURE.IMPORTANT] Az adatkezelési portálon keresztül szolgáltatók állítható be peerings azt jelenleg nem helyüket. Ez a képesség hamarosan engedélyezéséről dolgozunk. Ellenőrizze a szolgáltatás szolgáltatónál BGP peerings konfigurálása előtt.

Beállíthatja, hogy egy, a két, vagy egy készült ExpressRoute áramköri az összes három peerings (Azure saját, Azure nyilvános és Microsoft). Peerings beállíthatja úgy dönt, tetszőleges sorrendben. Azonban gondoskodnia kell arról, hogy minden peering egyszerre csak egy konfigurációja befejezéséhez. 

## <a name="azure-private-peering"></a>A magánjellegű Azure peering

Ez a szakasz ismertető hozhat létre, kérjen, frissítése és törlése az Azure-készült ExpressRoute áramkör személyes peering adatokat. 

### <a name="to-create-azure-private-peering"></a>A magánjellegű Azure peering létrehozása

1. A PowerShell-modult importálása készült ExpressRoute.
    
    Telepítse a legújabb PowerShell telepítő [PowerShell](http://www.powershellgallery.com/) gyűjteményből kell, majd az erőforrás-kezelő Azure modulok importálása a PowerShell-munkamenetet abból a készült ExpressRoute parancsmagok használatának megkezdése. Meg kell PowerShell futtatása rendszergazdaként.

        Install-Module AzureRM

        Install-AzureRM

    Az összes ismert szemantikai verziót tartományon belül AzureRM.* modulok importálása

        Import-AzureRM

    Válassza a modul a ismert szemantikai verziót tartományon belül is egyszerűen importálhatja 
        
        Import-Module AzureRM.Network 

    Bejelentkezés a fiókba

        Login-AzureRmAccount

    Jelölje ki az előfizetést készült ExpressRoute áramkör létrehozása
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Hozzon létre egy készült ExpressRoute áramkör.
    
    Kövesse az utasításokat [készült ExpressRoute áramkör](expressroute-howto-circuit-arm.md) létrehozása és a kapcsolat szolgáltatója kiépítve. 

    Ha a kapcsolat szolgáltatója kínál felügyelt Layer 3 szolgáltatások, kérheti a csatlakozási szolgáltató ahhoz, hogy Azure magánjellegű peering meg. Ebben az esetben nem kell a következő szakaszokban található utasításokat. Ha a kapcsolat szolgáltatója nem kezeli a áramkört ábrázoló létrehozása után, továbbítás kövesse az alábbi utasításokat. 

3. Jelölje be a készült ExpressRoute áramkör győződjön meg arról, hogy már kiépítve.

    Meg kell először ellenőrizze, hogy ha a készült ExpressRoute áramkör Kiépítéstől és is engedélyezve van. Lásd a lenti példát.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    A válasz lesz, az alábbihoz hasonló:

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


4. Állítsa be az Azure magánjellegű peering a kapcsolat.

    Győződjön meg arról, hogy az alábbi elemek előtt, folytassa a következő lépésekkel:

    - Egy /30 alhálózat az elsődleges hivatkozás. Ez nem virtuális hálózatok fenntartott címterület részének kell lennie.
    - Egy /30 alhálózat a másodlagos hivatkozás. Ez nem virtuális hálózatok fenntartott címterület részének kell lennie.
    - Egy érvényes virtuális ID peering a megállapításához. Biztosítani, hogy nincs más peering a a kapcsolat használja ugyanazt az virtuális azonosítót.
    - A peering SZÁMKÉNT. 2 bájtos és 4 bájtos SZÁMKÉNT is használhatja. Magánjellegű használható-e peering SZÁMKÉNT. Győződjön meg arról, hogy nem használ 65515.
    - Ha úgy dönt, hogy közül válasszon egy MD5 kivonat. **Ez a lépés nem kötelező**.
    
    Azure magánjellegű peering a kapcsolat konfigurálásához az alábbi parancsmagot futtathatja.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Az alábbi parancsmag is használhatja, ha úgy dönt, hogy egy MD5 kivonat használja.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Győződjön meg arról, hogy a AS számot peering ASN, nem vevő ASN szerint adja meg.

### <a name="to-view-azure-private-peering-details"></a>Azure magánjellegű peering részleteinek megtekintése

Elérheti a következő parancsmaggal konfigurációs részletei

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Azure személyes peering konfigurációs frissítése

Frissítheti a következő parancsmagot a konfiguráció bármely részét. Az alábbi példában a áramkör virtuális azonosítója frissül a 100 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>A magánjellegű Azure peering törlése

A következő parancsmagot a peering konfigurációs eltávolíthatja.

>[AZURE.WARNING] Győződjön meg arról, hogy az összes virtuális hálózatok ezzel a parancsmaggal futtatása előtt a készült ExpressRoute áramkör a nélküli is. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure nyilvános peering

Ez a szakasz ismertető hozhat létre, kérjen, frissítése és törlése az Azure-készült ExpressRoute áramkör nyilvános peering adatokat.

### <a name="to-create-azure-public-peering"></a>Azure nyilvános peering létrehozása

1. A PowerShell-modult importálása készült ExpressRoute.
    
    Telepítse a legújabb PowerShell telepítő [PowerShell](http://www.powershellgallery.com/) gyűjteményből kell, majd az erőforrás-kezelő Azure modulok importálása a PowerShell-munkamenetet abból a készült ExpressRoute parancsmagok használatának megkezdése. Meg kell PowerShell futtatása rendszergazdaként.

        Install-Module AzureRM

        Install-AzureRM

    Az összes ismert szemantikai verziót tartományon belül AzureRM.* modulok importálása

        Import-AzureRM

    Válassza a modul a ismert szemantikai verziót tartományon belül is egyszerűen importálhatja 
        
        Import-Module AzureRM.Network 

    Bejelentkezés a fiókba

        Login-AzureRmAccount

    Jelölje ki az előfizetést készült ExpressRoute áramkör létrehozása
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Hozzon létre egy készült ExpressRoute áramkör.
    
    Kövesse az utasításokat [készült ExpressRoute áramkör](expressroute-howto-circuit-arm.md) létrehozása és a kapcsolat szolgáltatója kiépítve. 

    Ha a kapcsolat szolgáltatója kínál felügyelt Layer 3 szolgáltatások, kérheti a csatlakozási szolgáltató ahhoz, hogy Azure nyilvános peering meg. Ebben az esetben nem kell a következő szakaszokban található utasításokat. Ha a kapcsolat szolgáltatója nem kezeli a áramkört ábrázoló létrehozása után, továbbítás kövesse az alábbi utasításokat.

3. Jelölje be annak érdekében, hogy már kiépítve készült ExpressRoute áramkör.

    Meg kell először ellenőrizze, hogy ha a készült ExpressRoute áramkör Kiépítéstől és is engedélyezve van. Lásd a lenti példát.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    A válasz lesz, az alábbihoz hasonló:

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

4. Állítsa be az Azure nyilvános peering a kapcsolat.

    Győződjön meg arról, hogy az alábbi információk további folytatás előtt.

    - Egy /30 alhálózat az elsődleges hivatkozás. Érvényes nyilvános IPv4 előtagot kell lennie.
    - Egy /30 alhálózat a másodlagos hivatkozás. Érvényes nyilvános IPv4 előtagot kell lennie.
    - Egy érvényes virtuális ID peering a megállapításához. Biztosítani, hogy nincs más peering a a kapcsolat használja ugyanazt az virtuális azonosítót.
    - A peering SZÁMKÉNT. 2 bájtos és 4 bájtos SZÁMKÉNT is használhatja.
    - Ha úgy dönt, hogy közül válasszon egy MD5 kivonat. **Ez a lépés nem kötelező**.
    
    Azure nyilvános peering a kapcsolat konfigurálásához az alábbi parancsmagot futtatása

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Az alábbi parancsmagot használhatja, ha úgy dönt, hogy egy MD5 kivonat használata

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Győződjön meg arról, hogy a AS számot peering ASN és az ügyfél ASN adja meg.

### <a name="to-view-azure-public-peering-details"></a>Azure nyilvános peering részleteinek megtekintése

Elérheti a következő parancsmaggal konfigurációs részletei

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Azure nyilvános peering konfigurációs frissítése

Frissítheti a következő parancsmagot a konfiguráció bármely részét

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

A helyi hálózat Azonosítójával a áramkör 200 frissül a fenti példában 600.

### <a name="to-delete-azure-public-peering"></a>Azure nyilvános peering törlése

Az alábbi parancsmag futtatásával eltávolíthatja a peering konfigurálása

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>A Microsoft peering

Ez a szakasz ismertető hozhat létre, kérjen, frissítése és törlése a Microsoft-készült ExpressRoute áramkör peering beállításokat. 

### <a name="to-create-microsoft-peering"></a>A Microsoft peering létrehozása

1. A PowerShell-modult importálása készült ExpressRoute.
    
    Telepítse a legújabb PowerShell telepítő [PowerShell](http://www.powershellgallery.com/) gyűjteményből kell, majd az erőforrás-kezelő Azure modulok importálása a PowerShell-munkamenetet abból a készült ExpressRoute parancsmagok használatának megkezdése. Meg kell PowerShell futtatása rendszergazdaként.

        Install-Module AzureRM

        Install-AzureRM

    Az összes ismert szemantikai verziót tartományon belül AzureRM.* modulok importálása

        Import-AzureRM

    Válassza a modul a ismert szemantikai verziót tartományon belül is egyszerűen importálhatja 
        
        Import-Module AzureRM.Network 

    Bejelentkezés a fiókba

        Login-AzureRmAccount

    Jelölje ki az előfizetést készült ExpressRoute áramkör létrehozása
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Hozzon létre egy készült ExpressRoute áramkör.
    
    Kövesse az utasításokat [készült ExpressRoute áramkör](expressroute-howto-circuit-arm.md) létrehozása és a kapcsolat szolgáltatója kiépítve. 

    Ha a kapcsolat szolgáltatója kínál felügyelt Layer 3 szolgáltatások, kérheti a csatlakozási szolgáltató ahhoz, hogy Azure magánjellegű peering meg. Ebben az esetben nem kell a következő szakaszokban található utasításokat. Ha a kapcsolat szolgáltatója nem kezeli a áramkört ábrázoló létrehozása után, továbbítás kövesse az alábbi utasításokat.

3. Jelölje be annak érdekében, hogy már kiépítve készült ExpressRoute áramkör.

    Meg kell először ellenőrizze, hogy ha a készült ExpressRoute áramkör Kiépítéstől és is engedélyezve van. Lásd a lenti példát.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    A válasz lesz, az alábbihoz hasonló:

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
4. Állítsa be a áramkör peering Microsoft.

    Győződjön meg arról, hogy az alábbi információk folytatás előtt.

    - Egy /30 alhálózat az elsődleges hivatkozás. Ez lehet érvényes nyilvános IPv4 előtagot az Ön tulajdonában és regisztrált egy RIR / BMR.
    - Egy /30 alhálózat a másodlagos hivatkozás. Ez lehet érvényes nyilvános IPv4 előtagot az Ön tulajdonában és regisztrált egy RIR / BMR.
    - Egy érvényes virtuális ID peering a megállapításához. Biztosítani, hogy nincs más peering a a kapcsolat használja ugyanazt az virtuális azonosítót.
    - A peering SZÁMKÉNT. 2 bájtos és 4 bájtos SZÁMKÉNT is használhatja.
    - Közzététel a prefixumokban: meg kell adnia azt tervezi, hogy a BGP munkamenet fölé meghirdetése összes előtagot listáját. Csak nyilvános IP cím prefixumokban elfogadott. Ha szeretne küldeni egy sor olyan prefixumokban elküldheti vesszővel tagolt listáját. Ezek a prefixumokban regisztrálva kell lenniük, egy RIR / BMR.
    - Ügyfél ASN: Ha hirdetési prefixumokban a peering SZÁMKÉNT nem regisztrált, megadhatja a regisztrált, amelyhez AS számot. **Ez a lépés nem kötelező**.
    - Útválasztási rendszerleíró neve: Megadhatja a RIR / BMR szemben, amely a szám és a prefixumokban regisztrált.
    - MD5 kivonat, ha úgy dönt, hogy közül válasszon. **Ez a lépés nem kötelező.**
    
    Futtatását is lehetővé teszi a következő parancsmagot a áramkör peering Microsoft konfigurálása

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>A Microsoft peering részleteket

A következő parancsmaggal konfigurációs adatok elérheti.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Microsoft peering-beállítások frissítése

Frissítheti a következő parancsmagot a konfiguráció bármely részét.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>A Microsoft peering törlése

A következő parancsmagot a peering konfigurációs eltávolíthatja.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Következő lépések

A következő lépésben [egy VNet szeretne egy készült ExpressRoute áramkör hivatkozásra](expressroute-howto-linkvnet-arm.md).

-  Munkafolyamatok készült ExpressRoute kapcsolatos további tudnivalókért olvassa el a [készült ExpressRoute munkafolyamatok](expressroute-workflows.md)című témakört.

-  Áramköri peering kapcsolatos további tudnivalókért olvassa el a [készült ExpressRoute áramkörök és útválasztási tartományok](expressroute-circuit-peerings.md)című témakört.

-  Virtuális hálózatokkal való használatáról további információt a [virtuális hálózati áttekintése](../virtual-network/virtual-networks-overview.md)című témakörben találhat.

