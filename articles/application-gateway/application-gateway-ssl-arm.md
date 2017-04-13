<properties
   pageTitle="Adjon meg egy alkalmazás átjárót, az SSL kiürítése Azure erőforrás-kezelővel |} Microsoft Azure"
   description="Ez az oldal ismerteti a képernyőn megjelenő utasításokat követve hozzon létre egy alkalmazás átjáró SSL kiürítése Azure erőforrás-kezelő használatával"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Adjon meg egy alkalmazás átjárót, SSL kiürítése az Azure erőforrás-kezelő használatával

> [AZURE.SELECTOR]
-[Azure portál](application-gateway-ssl-portal.md)
-[Azure erőforrás-kezelő PowerShell](application-gateway-ssl-arm.md)
-[Azure klasszikus PowerShell](application-gateway-ssl.md)

 Azure alkalmazás átjáró beállítható úgy, hogy megszünteti a költséges SSL visszafejtés tevékenységek fordulhat elő, a webes farm elkerülése érdekében az átjáró Secure Sockets Layer (SSL) munkamenetet. Az SSL kiürítése is egyszerűbbé teszi az előtér-kiszolgáló beállítása és kezelése a webalkalmazás.

## <a name="before-you-begin"></a>Első lépések

1. A webes Platform telepítő használatával telepítse az Azure PowerShell-parancsmagok legújabb verzióját. Töltse le, és telepítse a legújabb verzióját a **Windows PowerShell** szakaszáról [letöltések oldalt](https://azure.microsoft.com/downloads/).
2. Létrehozhat egy virtuális hálózati és az alkalmazás átjáró alhálózat. Ügyeljen arra, hogy ne virtuális gépeken futó vagy felhőalapú telepítések az alhálózathoz használja. Alkalmazás átjáró virtuális hálózati alhálózat önmagában kell lennie.
3. A kiszolgálókat, konfigurálja úgy, hogy az alkalmazás átjáró használata léteznie kell, vagy a virtuális hálózaton vagy egy nyilvános IP virtuális létrehozott végpontjaikat vannak rendelve.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mi az az alkalmazás átjáró létrehozásához szükséges?


- **Háttéradatbázist kiszolgáló készlet:** A háttér-kiszolgálók IP-címek listája. Az IP-címek szerepel a listában vagy tartozzanak a virtuális alhálózathoz, vagy egy nyilvános IP virtuális kell lennie.
- **Háttéradatbázist készlet kiszolgálóbeállítások:** Minden készlet beállításokat, például a port, protokoll és cookie-alapú affinitás tartalmaz. Ezeket a beállításokat is tartozik egy csoportba, és alkalmazott összes kiszolgálón a készlet belül.
- **Előtér-port:** Ez a portja a nyilvános olyan portot alkalmazás átjáró megnyitott. Forgalmat a port találatok, és kattintson az egyik a háttéradatbázist kiszolgálók átirányítását.
- **Figyelő:** A figyelő előtér-port, protokoll van (Http vagy Https, ezekkel a beállításokkal-és nagybetűk), és az SSL-tanúsítvány neve (ha az SSL beállítása kiürítése).
- **Szabály:** A szabály a figyelő és a háttér-kiszolgálón készlet köti és határozza meg, mely a forgalmat szeretné irányítani, amikor találatok száma a egy adott figyelő háttéradatbázist kiszolgáló készlet. Jelenleg csak az *egyszerű* szabály használata támogatott. Az *egyszerű* szabály egy ciklikus terheléselosztási.

**További beállítási jegyzetek**

Az SSL-tanúsítványok beállítása a protokoll **HttpListener** módosítsa *Https* (kis-és nagybetűk). A **SslCertificate** elem **HttpListener** bekerül a változó értékével be van állítva az SSL-tanúsítvány. Az előtér-port frissíteni kell a 443-as.

**Ahhoz, hogy a cookie-alapú affinitás**: az alkalmazás átjáró beállítható úgy, hogy biztosítása, hogy egy ügyfél munkamenetből kérelmet mindig átirányításához a azonos virtuális a webes farmban. Ebben az esetben befejeződött, hogy az a munkamenet cookie-k, amely lehetővé teszi, hogy az átjáró irányítsa át a forgalmának megfelelően. Ahhoz, hogy a cookie-alapú affinitás, állítsa **CookieBasedAffinity** *engedélyezve* **BackendHttpSettings** eleme.


## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása

A klasszikus Azure telepítési modell és Azure erőforrás-kezelő közötti különbség a sorrendben, a létrehozott egy alkalmazás átjáró és az elemeket, amelyeket kell beállítania.

Az erőforrás-kezelő minden összetevő-alkalmazás átjáró külön-külön és beállított majd helyezze át az alkalmazás átjáró erőforrás létrehozása.


Íme egy alkalmazás átjáró létrehozásához szükséges lépéseket:

1. Erőforráscsoport az erőforrás-kezelő létrehozása
2. Virtuális hálózati, alhálózat és nyilvános IP az alkalmazás átjáró létrehozása
3. Konfigurációs objektum alkalmazás átjáró létrehozása
4. Hozzon létre egy alkalmazás átjáró erőforrást


## <a name="create-a-resource-group-for-resource-manager"></a>Erőforráscsoport az erőforrás-kezelő létrehozása

Győződjön meg arról, hogy a PowerShell mód az Azure erőforrás-kezelő parancsmagok használatával válthat. További információ a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)címen érhető el.

### <a name="step-1"></a>Lépés: 1

    Login-AzureRmAccount

### <a name="step-2"></a>Lépés: 2

Jelölje be az előfizetések a fiókhoz.

    Get-AzureRmSubscription

A hitelesítő adatokkal hitelesítést végezni kéri.<BR>

### <a name="step-3"></a>3 lépés

A használandó Azure előfizetések kiválasztása. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Lépés: 4

Erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja) létrehozása.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ez a beállítás alapértelmezett helyeként erőforrások erőforráscsoport használatos. Győződjön meg arról, hogy minden parancs az alkalmazás átjáró létrehozásához használja az azonos erőforráscsoport.

A fenti példában létrehozott egy úgynevezett "appgw-RG" és "US nyugati" hely erőforráscsoport.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Hozzon létre egy virtuális hálózati és az alkalmazás átjáró alhálózat

A következő példa bemutatja, hogyan hozhat létre virtuális hálózat erőforrás-kezelő használatával:

### <a name="step-1"></a>Lépés: 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Ez a példa a cím tartomány 10.0.0.0/24 rendel egy virtuális hálózati létrehozásához használandó alhálózat változó.

### <a name="step-2"></a>Lépés: 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Ez a példa létrehoz egy erőforrás csoport "appgw-rg" a "appgwvnet" nevű a nyugati USA-beli területhez tartozik, az előtag 10.0.0.0/16 használata alhálózat 10.0.0.0/24 virtuális hálózati.

### <a name="step-3"></a>3 lépés

    $subnet = $vnet.Subnets[0]

Ez a példa az alhálózathoz objektum rendel a következő lépésekkel $subnet változó.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Hozzon létre egy nyilvános IP-címet az előtér-konfiguráció

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Az alábbi példa létrehoz egy nyilvános IP-erőforrás erőforrás csoport "appgw-rg" USA-beli nyugati régió "publicIP01".


## <a name="create-an-application-gateway-configuration-object"></a>Konfigurációs objektum alkalmazás átjáró létrehozása

### <a name="step-1"></a>Lépés: 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Az alábbi példa létrehoz egy alkalmazás gateway IP-konfiguráció "gatewayIP01" nevű. Átjáró alkalmazás indításakor felveszi a beállított alhálózat IP-címet, és az IP-címek a háttéradatbázist IP-készletre irányítja a hálózati forgalmának engedélyezésére. Ne feledje, hogy az egyes példányok vegye egy IP-címet.

### <a name="step-2"></a>Lépés: 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Ez a példa konfigurálja az IP-címek "pool01" nevű háttéradatbázist IP-cím készlet "134.170.185.46, 134.170.188.221,134.170.185.50." Ezek az értékek az IP-címek mentsen a az előtér-végponton érkező forgalmat. Az IP-címek az előző példában szereplő cserélje le a webes alkalmazás végpontok IP-címét.

### <a name="step-3"></a>3 lépés

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Ez a példa alkalmazás átjáró beállítása "poolsetting01" terheléselosztás hálózati forgalmat a háttéradatbázist készletben állítja be.

### <a name="step-4"></a>Lépés: 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Ez a példa konfigurálja az előtér-IP-portot "frontendport01" nevű a nyilvános IP-végpontot.

### <a name="step-5"></a>5 lépésben

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Ez a példa konfigurálja az SSL-kapcsolat használt tanúsítvány. A tanúsítvány kell lennie .pfx formátumban, és a jelszó 4 – 12 karakter közé kell lennie.

### <a name="step-6"></a>6 lépés

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Ez a példa az előtér-IP-konfiguráció "fipconfig01" nevű hoz létre, és az előtér-IP-beállítása a nyilvános IP-címmel társít.

### <a name="step-7"></a>Lépés 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Ez a példa listener01"figyelő neve" hoz létre, és az előtér-port az előtér-IP-beállítások és a tanúsítvány társít.

### <a name="step-8"></a>Lépés 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Ez a példa a betöltés terheléselosztó útválasztási szabály neve "rule01", amely a betöltés terheléselosztó viselkedés konfigurálja hoz létre.

### <a name="step-9"></a>Lépés 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Ez a példa konfigurálja az alkalmazás átjáró példány méretét.

>[AZURE.NOTE]  *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. Standard_Small Standard_Medium és Standard_Large közül választhat.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Hozzon létre egy alkalmazás átjáró új-AzureApplicationGateway használatával

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Ez a példa létrehoz egy alkalmazás átjáró konfigurációs összes elemet a fenti lépések. A példában az alkalmazás átjáró neve "appgwtest".

## <a name="get-application-gateway-dns-name"></a>Átjáró DNS alkalmazásnév beszerzése

Az átjáró létrehozása után a következő lépésként az előtér-kapcsolati konfigurálása. Egy nyilvános IP használata esetén az alkalmazás átjáró dinamikusan hozzárendelt DNS nevét, amely nem rövid van szükség. Annak biztosítása érdekében a felhasználóknak az alkalmazás átjáró egy CNAME rekordot is találati használható az alkalmazás átjáró a nyilvános végpont mutasson. Az [egyéni tartománynevet az Azure-ban konfigurálása](../cloud-services/cloud-services-custom-domain-name-portal.md). Ehhez beolvashatja részleteit, az alkalmazás átjáró, valamint a társított IP/DNS nevét a PublicIPAddress elem az alkalmazás átjáró csatolt használatával. Az alkalmazás átjáró tartománynév használandó hozhat létre egy CNAME rekordot, amely a két webalkalmazások a DNS-névhez mutat. A rekordok felhasználása nem javasolt, mert a alkalmazás átjáró újra kell indítani a virtuális is megváltoztatható.
    
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

Belső terheléselosztó (ILB) használata egy alkalmazás átjáró beállítása, című témakörben olvashat [létrehozása egy alkalmazás átjárónak belső terheléselosztó (ILB)](application-gateway-ilb.md).

Ha azt szeretné, hogy további információt az általános betöltése terheléselosztó beállítások, jelenik meg:

- [Azure terheléselosztó](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure forgalom Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
