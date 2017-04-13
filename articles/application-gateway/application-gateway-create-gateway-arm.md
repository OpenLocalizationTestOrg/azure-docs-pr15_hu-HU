<properties
   pageTitle="Hozzon létre vagy és az alkalmazás átjáró törlése Azure erőforrás-kezelő használatával |} Microsoft Azure"
   description="Ezen az oldalon útmutatás hozhat létre, beállítása, indítása és az Azure alkalmazás átjáró törlése Azure erőforrás-kezelő használatával"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Hozzon létre vagy és az alkalmazás átjáró törlése Azure erőforrás-kezelő használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-gateway-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klasszikus PowerShell](application-gateway-create-gateway.md)
- [Erőforrás-kezelő Azure-sablon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure alkalmazás átjáró az egy réteg-7 terheléselosztó. A feladatátvétel HTTP-kérések teljesítmény továbbítása különböző kiszolgálók között, akár a helyszíni vagy felhőalapú biztosít. Alkalmazás átjáró sok teszi alkalmazás kézbesítési vezérlő LÉPETT funkciói, köztük a HTTP terheléselosztás cookie-alapú munkamenet affinitás, Secure Sockets Layer (SSL) kiürítése, egyéni állapot szondákat, több webhelyen támogatása és számos más. Ha a támogatott szolgáltatások listáját, látogasson el az [Alkalmazás átjáró – áttekintés](application-gateway-introduction.md)

Ez a cikk végigvezeti a hozhat létre, beállítása, indítása és az alkalmazás átjáró törlése.

>[AZURE.IMPORTANT] Mielőtt az Azure erőforrások dolgozik, fontos, megértéséhez, hogy a Azure jelenleg van-e a két környezetben modellek: az erőforrás-kezelő és klasszikus. Győződjön meg arról, hogy megismeri [telepítési modellek és eszközök](../azure-classic-rm.md) bármely Azure erőforrás használata előtt. Megtekintheti a különböző eszközök dokumentációjában tetején látható ez a cikk a Tabulátorok gombra kattintva. A dokumentum bemutatja, hogy egy alkalmazás átjáró létrehozása az Azure erőforrás-kezelő használatával. Klasszikus verzióját használja, nyissa meg a [PowerShell használatával telepítés klasszikus alkalmazás átjáró létrehozása](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Első lépések

1. A webes Platform telepítő használatával telepítse az Azure PowerShell-parancsmagok legújabb verzióját. Töltse le, és telepítse a legújabb verzióját a **Windows PowerShell** szakaszáról [letöltések oldalt](https://azure.microsoft.com/downloads/).
2. Ha egy meglévő virtuális hálózat, válasszon egy meglévő üres alhálózat vagy alhálózat használatra kizárólag a meglévő virtuális hálózatban szerint az alkalmazás átjáró létrehozása. Nem telepíthető az alkalmazás átjáró, mint az erőforrások szeretné telepíteni az alkalmazást átjáró mögött virtuális másik hálózathoz.
3. Az alkalmazás átjáró használandó konfigurálható kiszolgálókon léteznie kell, vagy a virtuális hálózaton vagy egy nyilvános IP virtuális létrehozott végpontjaikat vannak rendelve.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mi az az alkalmazás átjáró létrehozásához szükséges?

- **Háttéradatbázis kiszolgáló készlet:** A háttér-kiszolgálók IP-címek listája. Az IP-címek szerepel a listában vagy tartozzanak a virtuális alhálózathoz, vagy egy nyilvános IP virtuális kell lennie.
- **Háttéradatbázist készlet kiszolgálóbeállítások:** Minden készlet beállításokat, például a port, protokoll és cookie-alapú affinitás tartalmaz. Ezeket a beállításokat is tartozik egy csoportba, és alkalmazott összes kiszolgálón a készlet belül.
- **Előtér-port:** Ez a portja a nyilvános olyan portot alkalmazás átjáró megnyitott. Forgalmat a port találatok, és kattintson az egyik a háttéradatbázist kiszolgálók átirányítását.
- **Figyelő:** A figyelő előtér-port, protokoll van (Http vagy Https, ezek az értékek-és nagybetűk), és az SSL-tanúsítvány neve (ha az SSL beállítása kiürítése).
- **Szabály:** A szabály figyelő, a háttéradatbázist kiszolgáló készlet köti és határozza meg, mely a forgalmat szeretné irányítani, amikor találatok száma a egy adott figyelő háttéradatbázist kiszolgáló készlet.

## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása

Az Azure klasszikus és Azure erőforrás-kezelő közötti különbség a sorrendben, amelyben az alkalmazás átjáró és az elemeket, amelyeket kell beállítania létrehozása.

Az erőforrás-kezelő-alkalmazás átjáró ellenőrizze az összes elem külön-külön beállítva, és, majd írja az alkalmazás átjáró erőforrás létrehozása.

Az alábbi táblázat azokat a lépéseket, hogy az alkalmazás átjáró létrehozásához szükséges.

## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport az erőforrás-kezelő létrehozása

Győződjön meg arról, hogy az Azure PowerShell legújabb verzióját használja. További információ a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)címen érhető el.

### <a name="step-1"></a>Lépés: 1

Bejelentkezés az Azure

    Login-AzureRmAccount

A hitelesítő adatokkal hitelesítést végezni kéri.

### <a name="step-2"></a>Lépés: 2

Jelölje be az előfizetések a fiókhoz.

    Get-AzureRmSubscription

### <a name="step-3"></a>3. lépés

A használandó Azure előfizetések kiválasztása.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Lépés: 4

Erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja) létrehozása.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ezen a helyen erőforrásában található erőforrás csoport alapértelmezett helye szolgál. Győződjön meg arról, hogy minden parancs az alkalmazás átjáró létrehozásához használja az azonos erőforráscsoport.

A fenti példában létrehozott egy úgynevezett "appgw-RG" és "US nyugati" hely erőforráscsoport.

>[AZURE.NOTE] Állítsa be az alkalmazás átjáró egy egyéni vizsgálati van szüksége, ha a [létrehozása egy PowerShell használatával egyéni szondákat alkalmazás átjárónak](application-gateway-create-probe-ps.md)témakörben találhat. Nézze meg az [egyéni szondákat és a rendszerállapot figyelése](application-gateway-probe-overview.md) további információt.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Hozzon létre egy virtuális hálózati és az alkalmazás átjáró alhálózat

A következő példa bemutatja, hogyan hozhat létre virtuális hálózat erőforrás-kezelő használatával.

### <a name="step-1"></a>Lépés: 1

A cím tartomány 10.0.0.0/24 rendelhet a alhálózat változó virtuális hálózat létrehozására szolgál.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Lépés: 2

Hozzon létre egy erőforrás csoport "appgw-rg" a "appgwvnet" nevű a nyugati USA-beli területhez tartozik, az előtag 10.0.0.0/16 használata alhálózat 10.0.0.0/24 virtuális hálózaton.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>3 lépés

Rendelje hozzá a következő lépésekkel, és hozzon létre egy alkalmazás átjáró-alhálózat változó.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Hozzon létre egy nyilvános IP-címet az előtér-konfiguráció

Hozzon létre egy nyilvános IP-erőforrás erőforrás csoport "appgw-rg" USA-beli nyugati régió "publicIP01".

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Konfigurációs objektum alkalmazás átjáró létrehozása

Az alkalmazás átjáró létrehozása előtt be kell állítania az összes konfigurációs elemek. Az alábbi lépésekkel létrehozása a konfigurációs elemek, amelyek egy alkalmazás átjáró erőforrás van szükség.

### <a name="step-1"></a>Lépés: 1

Hozzon létre egy alkalmazás gateway IP-konfiguráció "gatewayIP01" nevű. Átjáró alkalmazás indításakor felveszi a beállított alhálózat IP-címet, és továbbítja az IP-címek a háttéradatbázist IP készletben hálózati forgalmának engedélyezésére. Ne feledje, hogy az egyes példányok vegye egy IP-címet.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Lépés: 2

IP-címek "pool01" nevű háttéradatbázist IP-cím készlet konfigurálása "134.170.185.46, 134.170.188.221,134.170.185.50". E IP-címei az IP-címek mentsen a az előtér-végponton érkező forgalmat. Lecseréli az előző IP-címek hozzáadása a saját alkalmazás IP-cím végpontok.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>3 lépés

Állítsa be a terheléselosztás forgalmat a háttéradatbázist készletben alkalmazás átjáró beállítása "poolsetting01".

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Lépés: 4

Állítsa be az előtér-IP-portot "frontendport01" nevű a nyilvános IP-végpontot.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>5 lépésben

Az előtér-IP-konfiguráció "fipconfig01" nevű létrehozása és hozzárendelése a nyilvános IP-címet az előtér-IP-konfiguráció.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>6 lépés

Létrehozása a figyelő neve "listener01" és az előtér-port az előtér-IP-konfiguráció társítani.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Lépés 7

A betöltés terheléselosztó útválasztás szabály létrehozása nevű "rule01", amely konfigurálja a betöltés terheléselosztó viselkedését.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Lépés 8

Állítsa be az alkalmazás átjáró példány méretét.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. Standard_Small Standard_Medium és Standard_Large közül választhat.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Hozzon létre egy alkalmazás átjáró új-AzureRmApplicationGateway használatával

Hozzon létre egy alkalmazás átjáró konfigurációs összes elemet a fenti lépéseket. Ebben a példában az alkalmazás átjáró neve "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Lépés 9

Az alkalmazás átjáró DNS és virtuális részleteinek lekérése az nyilvános IP-erőforráshoz csatolt az alkalmazás átjáró.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Az alkalmazás átjáró törlése

Ha törölni szeretne egy alkalmazás átjáró, kövesse az alábbi lépéseket:

### <a name="step-1"></a>Lépés: 1

Az alkalmazás átjáró objektum e-azt egy "$getgw" változó társítani.

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Lépés: 2

Az alkalmazás átjáró leállítása a **Stop-AzureRmApplicationGateway** használatával.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Miután az alkalmazás átjáró leállítva állapotba kerül, távolítsa el a szolgáltatást az **Eltávolítás-AzureRmApplicationGateway** parancsmag használatával.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] A **-kényszerítse** kapcsoló akkor használható a eltávolítása megerősítő üzenet elrejtése.

Ha ellenőrizni szeretné, hogy a szolgáltatás megszűnt, a **Get-AzureRmApplicationGateway** parancsmag is használhatja. Ez a lépés nem szükség.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

## <a name="get-application-gateway-dns-name"></a>Átjáró DNS alkalmazásnév beszerzése

Az átjáró létrehozása után a következő lépésként az előtér-alapú kommunikációhoz konfigurálása. Egy nyilvános IP használata esetén az alkalmazás átjáró dinamikusan hozzárendelt DNS nevét, amely nem rövid van szükség. Annak biztosítása érdekében a felhasználóknak az alkalmazás átjáró egy CNAME rekordot is találati használható az nyilvános végpontot, az alkalmazás átjáró mutasson. Az [egyéni tartománynevet az Azure-ban konfigurálása](../cloud-services/cloud-services-custom-domain-name-portal.md). Ehhez beolvashatja részleteit, az alkalmazás átjáró, valamint a társított IP/DNS nevét a PublicIPAddress elem az alkalmazás átjáró csatolt használatával. Az alkalmazás átjáró tartománynév használandó hozhat létre egy CNAME rekordot, amely a két webalkalmazások a DNS-névhez mutat. A rekordok felhasználása nem javasolt, mert a alkalmazás átjáró újra kell indítani a virtuális is megváltoztatható.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Következő lépések

Ha szeretné az SSL kiürítése konfigurálása, [konfigurálása az SSL-alkalmazás átjáró, kiürítése](application-gateway-ssl.md)című témakört.

Az alkalmazás átjáró használata egy belső terheléselosztó beállítása, című témakörben olvashat [létrehozása egy alkalmazás átjárónak belső terheléselosztó (ILB)](application-gateway-ilb.md).

Ha azt szeretné, hogy további információt az általános betöltése terheléselosztó beállítások, jelenik meg:

- [Azure terheléselosztó](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure forgalom Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
