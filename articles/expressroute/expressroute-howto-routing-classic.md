<properties
   pageTitle="A PowerShell használatá klasszikus telepítési modell egy készült ExpressRoute áramkör továbbítás beállítása |} Microsoft Azure"
   description="Ez a cikk végigvezeti a létrehozásához, és a lehetőséget választva személyes, nyilvános és a Microsoft-készült ExpressRoute áramkör peering kiépítési. Ez a cikk is bemutatja, hogyan ellenőrizheti az állapotát, frissíthet és törölhet az áramkör peerings."
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
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Létrehozása és módosítása az készült ExpressRoute áramkör Útválasztás

> [AZURE.SELECTOR]
[Azure portál - erőforrás-kezelő](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - erőforrás-kezelő](expressroute-howto-routing-arm.md)
[PowerShell - klasszikus](expressroute-howto-routing-classic.md)



Az alábbiakban ismertetjük, létrehozása és kezelése egy PowerShell és a klasszikus telepítési modell készült ExpressRoute áramkör útválasztás beállításának lépéseit. Az alábbi lépésekkel is bemutatja, hogyan nézze meg, állapot, frissítés, törlés és egy készült ExpressRoute áramkör peerings deprovision.


**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfigurációs vonatkozó követelmények

- Szüksége lesz az Azure PowerShell-modul legújabb verzióját. A legújabb PowerShell-modult letöltheti a PowerShell szakaszából az [Azure letöltési lapját](https://azure.microsoft.com/downloads/). Kövesse a lépésenkénti útmutatót találhat a [miként telepítheti és Azure PowerShell beállítása](../powershell-install-configure.md) lapon az Azure PowerShell-modulok használni a számítógép beállításával. 
- Győződjön meg arról, hogy ellenőrzését a [Előfeltételek](expressroute-prerequisites.md) lapon, a [útválasztási követelményeinek](expressroute-routing.md) lap és a [munkafolyamatok](expressroute-workflows.md) lap konfigurációs megkezdése előtt.
- Az aktív készült ExpressRoute áramkör kell rendelkeznie. Az utasításokat követve [Hozzon létre egy készült ExpressRoute áramkör](expressroute-howto-circuit-classic.md) , és a folytatás előtt engedélyezve van a kapcsolat szolgáltatója áramkör van. A készült ExpressRoute áramkör ki láthatja az alábbiakban ismertetett parancsmag kiépített és engedélyezett állapotban kell lennie.

>[AZURE.IMPORTANT] Ezeket az utasításokat a létrehozott réteget 2 adatkapcsolat szolgáltatásokat kínáló szolgáltatókkal áramkörök csak vonatkoznak. Ha használja a szolgáltató kínáló felügyelt Layer 3 szolgáltatások (általában egy IPVPN, például MPLS), a kapcsolat szolgáltatója konfigurálása, és meg útválasztás kezelése.

Beállíthatja, hogy egy, a két, vagy egy készült ExpressRoute áramköri az összes három peerings (Azure saját, Azure nyilvános és Microsoft). Peerings beállíthatja úgy dönt, tetszőleges sorrendben. Azonban gondoskodnia kell arról, hogy minden peering egyszerre csak egy konfigurációja befejezéséhez. 

## <a name="azure-private-peering"></a>A magánjellegű Azure peering

Ez a szakasz ismertető hozhat létre, kérjen, frissítése és törlése az Azure-készült ExpressRoute áramkör személyes peering adatokat. 

### <a name="to-create-azure-private-peering"></a>A magánjellegű Azure peering létrehozása

1. **A PowerShell-modult importálása készült ExpressRoute.**
    
    Importálnia kell az Azure és készült ExpressRoute modulok be a PowerShell-munkamenetet abból a készült ExpressRoute parancsmagok használatának megkezdéséhez. A következő parancsokat az Azure és készült ExpressRoute modulok importálása a PowerShell-munkamenetet.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Hozzon létre egy készült ExpressRoute áramkör.**
    
    Kövesse az utasításokat [készült ExpressRoute áramkör](expressroute-howto-circuit-classic.md) létrehozása és a kapcsolat szolgáltatója kiépítve. Ha a kapcsolat szolgáltatója kínál felügyelt Layer 3 szolgáltatások, kérheti a csatlakozási szolgáltató ahhoz, hogy Azure magánjellegű peering meg. Ebben az esetben nem kell a következő szakaszokban található utasításokat. Ha a kapcsolat szolgáltatója nem kezeli a áramkört ábrázoló létrehozása után, továbbítás kövesse az alábbi utasításokat. 

3. **Jelölje be a készült ExpressRoute áramkör győződjön meg arról, hogy már kiépítve.**

    Meg kell először ellenőrizze, hogy ha a készült ExpressRoute áramkör Kiépítéstől és is engedélyezve van. Lásd a lenti példát.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Győződjön meg arról, hogy a áramkör Provisioned és engedélyezve, látható. Ha ez nem működik beszerzése a kapcsolat a szükséges állapotába, és állapota a csatlakozási szolgáltatójánál.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Állítsa be az Azure magánjellegű peering a kapcsolat.**

    Győződjön meg arról, hogy az alábbi elemek előtt, folytassa a következő lépésekkel:

    - Egy /30 alhálózat az elsődleges hivatkozás. Ez nem virtuális hálózatok fenntartott címterület részének kell lennie.
    - Egy /30 alhálózat a másodlagos hivatkozás. Ez nem virtuális hálózatok fenntartott címterület részének kell lennie.
    - Egy érvényes virtuális ID peering a megállapításához. Biztosítani, hogy nincs más peering a a kapcsolat használja ugyanazt az virtuális azonosítót.
    - A peering SZÁMKÉNT. 2 bájtos és 4 bájtos SZÁMKÉNT is használhatja. Magánjellegű használható-e peering SZÁMKÉNT. Győződjön meg arról, hogy nem használ 65515.
    - Ha úgy dönt, hogy közül válasszon egy MD5 kivonat. **Ez a lépés nem kötelező**.
    
    Azure magánjellegű peering a kapcsolat konfigurálásához az alábbi parancsmagot futtathatja.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Az alábbi parancsmag is használhatja, ha úgy dönt, hogy egy MD5 kivonat használja.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Győződjön meg arról, hogy a AS számot peering ASN, nem vevő ASN szerint adja meg.

### <a name="to-view-azure-private-peering-details"></a>Azure magánjellegű peering részleteinek megtekintése

Elérheti a következő parancsmaggal konfigurációs részletei

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Azure személyes peering konfigurációs frissítése

Frissítheti a következő parancsmagot a konfiguráció bármely részét. Az alábbi példában a áramkör virtuális azonosítója frissül a 100 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>A magánjellegű Azure peering törlése

A következő parancsmagot a peering konfigurációs eltávolíthatja.

>[AZURE.WARNING] Győződjön meg arról, hogy az összes virtuális hálózatok ezzel a parancsmaggal futtatása előtt a készült ExpressRoute áramkör a nélküli is. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure nyilvános peering

Ez a szakasz ismertető hozhat létre, kérjen, frissítése és törlése az Azure-készült ExpressRoute áramkör nyilvános peering adatokat.

### <a name="to-create-azure-public-peering"></a>Azure nyilvános peering létrehozása

1. **A PowerShell-modult importálása készült ExpressRoute.**
    
    Importálnia kell az Azure és készült ExpressRoute modulok be a PowerShell-munkamenetet abból a készült ExpressRoute parancsmagok használatának megkezdéséhez. A következő parancsokat az Azure és készült ExpressRoute modulok importálása a PowerShell-munkamenetet. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Készült ExpressRoute áramkör létrehozása**
    
    Kövesse az utasításokat [készült ExpressRoute áramkör](expressroute-howto-circuit-classic.md) létrehozása és a kapcsolat szolgáltatója kiépítve. Ha a kapcsolat szolgáltatója kínál felügyelt Layer 3 szolgáltatások, kérheti a csatlakozási szolgáltató ahhoz, hogy Azure nyilvános peering meg. Ebben az esetben nem kell a következő szakaszokban található utasításokat. Ha a kapcsolat szolgáltatója nem kezeli a áramkört ábrázoló létrehozása után, továbbítás kövesse az alábbi utasításokat.

3. **Győződjön meg arról, hogy már kiépítve készült ExpressRoute áramkör ellenőrzése**

    Meg kell először ellenőrizze, hogy ha a készült ExpressRoute áramkör Kiépítéstől és is engedélyezve van. Lásd a lenti példát.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Győződjön meg arról, hogy a áramkör Provisioned és engedélyezve, látható. Ha ez nem működik beszerzése a kapcsolat a szükséges állapotába, és állapota a csatlakozási szolgáltatójánál.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Azure nyilvános peering a kapcsolat beállítása**

    Győződjön meg arról, hogy az alábbi információk további folytatás előtt.

    - Egy /30 alhálózat az elsődleges hivatkozás. Érvényes nyilvános IPv4 előtagot kell lennie.
    - Egy /30 alhálózat a másodlagos hivatkozás. Érvényes nyilvános IPv4 előtagot kell lennie.
    - Egy érvényes virtuális ID peering a megállapításához. Biztosítani, hogy nincs más peering a a kapcsolat használja ugyanazt az virtuális azonosítót.
    - A peering SZÁMKÉNT. 2 bájtos és 4 bájtos SZÁMKÉNT is használhatja.
    - Ha úgy dönt, hogy közül válasszon egy MD5 kivonat. **Ez a lépés nem kötelező**.
    
    Azure nyilvános peering a kapcsolat konfigurálásához az alábbi parancsmagot futtatása

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Az alábbi parancsmagot használhatja, ha úgy dönt, hogy egy MD5 kivonat használata

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Győződjön meg arról, hogy a AS számot peering ASN és az ügyfél ASN adja meg.

### <a name="to-view-azure-public-peering-details"></a>Azure nyilvános peering részleteinek megtekintése

Elérheti a következő parancsmaggal konfigurációs részletei

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Azure nyilvános peering konfigurációs frissítése

Frissítheti a következő parancsmagot a konfiguráció bármely részét

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

A helyi hálózat Azonosítójával a áramkör 200 frissül a fenti példában 600.

### <a name="to-delete-azure-public-peering"></a>Azure nyilvános peering törlése

Az alábbi parancsmag futtatásával eltávolíthatja a peering konfigurálása

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>A Microsoft peering

Ez a szakasz ismertető hozhat létre, kérjen, frissítése és törlése a Microsoft-készült ExpressRoute áramkör peering beállításokat. 

### <a name="to-create-microsoft-peering"></a>A Microsoft peering létrehozása

1. **A PowerShell-modult importálása készült ExpressRoute.**
    
    Importálnia kell az Azure és készült ExpressRoute modulok be a PowerShell-munkamenetet abból a készült ExpressRoute parancsmagok használatának megkezdéséhez. A következő parancsokat az Azure és készült ExpressRoute modulok importálása a PowerShell-munkamenetet.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Készült ExpressRoute áramkör létrehozása**
    
    Kövesse az utasításokat [készült ExpressRoute áramkör](expressroute-howto-circuit-classic.md) létrehozása és a kapcsolat szolgáltatója kiépítve. Ha a kapcsolat szolgáltatója kínál felügyelt Layer 3 szolgáltatások, kérheti a csatlakozási szolgáltató ahhoz, hogy Azure magánjellegű peering meg. Ebben az esetben nem kell a következő szakaszokban található utasításokat. Ha a kapcsolat szolgáltatója nem kezeli a áramkört ábrázoló létrehozása után, továbbítás kövesse az alábbi utasításokat.

3. **Győződjön meg arról, hogy már kiépítve készült ExpressRoute áramkör ellenőrzése**

    Meg kell először ellenőrizze, hogy ha a készült ExpressRoute áramkör Provisioned és engedélyezett állapotban van-e.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Győződjön meg arról, hogy a áramkör Provisioned és engedélyezve, látható. Ha ez nem működik beszerzése a kapcsolat a szükséges állapotába, és állapota a csatlakozási szolgáltatójánál.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **A kapcsolat peering Microsoft konfigurálása**

    Győződjön meg arról, hogy az alábbi információk folytatás előtt.

    - Egy /30 alhálózat az elsődleges hivatkozás. Ez lehet érvényes nyilvános IPv4 előtagot az Ön tulajdonában és regisztrált egy RIR / BMR.
    - Egy /30 alhálózat a másodlagos hivatkozás. Ez lehet érvényes nyilvános IPv4 előtagot az Ön tulajdonában és regisztrált egy RIR / BMR.
    - Egy érvényes virtuális ID peering a megállapításához. Biztosítani, hogy nincs más peering a a kapcsolat használja ugyanazt az virtuális azonosítót.
    - A peering SZÁMKÉNT. 2 bájtos és 4 bájtos SZÁMKÉNT is használhatja.
    - Közzététel a prefixumokban: meg kell adnia azt tervezi, hogy a BGP munkamenet fölé meghirdetése összes előtagot listáját. Csak nyilvános IP cím prefixumokban elfogadott. Ha szeretne küldeni egy sor olyan prefixumokban elküldheti vesszővel tagolt listáját. Ezek a prefixumokban regisztrálva kell lenniük, egy RIR / BMR.
    - Ügyfél ASN: Ha hirdetési prefixumokban a peering SZÁMKÉNT nem regisztrált, megadhatja a regisztrált, amelyhez AS számot. **Ez a lépés nem kötelező**.
    - Útválasztási rendszerleíró neve: Megadhatja a RIR / BMR szemben, amely a szám és a prefixumokban regisztrált.
    - Egy MD5 kivonat, ha úgy dönt, hogy közül válasszon. **Ez a lépés nem kötelező.**
    
    Futtatását is lehetővé teszi a Microsoft pering a kapcsolat konfigurálásához az alábbi parancsmagot

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>A Microsoft peering részleteinek megtekintése

A következő parancsmaggal konfigurációs adatok elérheti.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Microsoft peering-beállítások frissítése

Frissítheti a következő parancsmagot a konfiguráció bármely részét.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>A Microsoft peering törlése

A következő parancsmagot a peering konfigurációs eltávolíthatja.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Következő lépések

[Hivatkozás egy VNet szeretne egy készült ExpressRoute áramkör](expressroute-howto-linkvnet-classic.md)a Tovább gombra.


-  Munkafolyamatok kapcsolatos további tudnivalókért olvassa el a [készült ExpressRoute munkafolyamatok](expressroute-workflows.md)című témakört.
-  Áramköri peering kapcsolatos további tudnivalókért olvassa el a [készült ExpressRoute áramkörök és útválasztási tartományok](expressroute-circuit-peerings.md)című témakört.

