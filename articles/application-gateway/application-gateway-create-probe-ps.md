<properties
   pageTitle="Erőforrás-kezelő PowerShell használatával hozzon létre egy egyéni vizsgálati alkalmazás átjáró |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy egyéni vizsgálati alkalmazás átjáró, az erőforrás-kezelő PowerShell használatával"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Hozzon létre egy egyéni vizsgálati Azure alkalmazás átjáró az Azure erőforrás-kezelő PowerShell használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-probe-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-probe-ps.md)
- [Azure klasszikus PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Lépés: 1

Bejelentkezés-AzureRmAccount segítségével hitelesítést végezni.

    Login-AzureRmAccount

### <a name="step-2"></a>Lépés: 2

Jelölje be az előfizetések a fiókhoz.

    Get-AzureRmSubscription

### <a name="step-3"></a>3 lépés

A használandó Azure előfizetések kiválasztása. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Lépés: 4

Erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja) létrehozása.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ezen a helyen az erőforrások erőforráscsoport az alapértelmezett hely szolgál. Győződjön meg arról, hogy minden parancs az alkalmazás átjáró létrehozása az azonos erőforráscsoport.

A fenti példában létrehozott egy úgynevezett "appgw-RG" és "US nyugati" hely erőforráscsoport.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Hozzon létre egy virtuális hálózati és az alkalmazás átjáró alhálózat

Az alábbi lépésekkel hozzon létre egy virtuális hálózati és az alkalmazás átjáró alhálózat.

### <a name="step-1"></a>Lépés: 1


A cím tartomány 10.0.0.0/24 hozzárendelése egy virtuális hálózati létrehozásához használandó alhálózat változó.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Lépés: 2

Hozzon létre egy virtuális hálózati csoport "appgw-rg erőforrás" a "appgwvnet" nevű alhálózat 10.0.0.0/24 az előtag 10.0.0.0/16 használ, a nyugati USA-beli területhez tartozik.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>3 lépés

Rendelje hozzá a következő lépésekkel, és hozzon létre egy alkalmazás átjáró-alhálózat változó.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Hozzon létre egy nyilvános IP-címet az előtér-konfiguráció


Hozzon létre egy nyilvános IP-erőforrás erőforrás csoport "appgw-rg" USA-beli nyugati régió "publicIP01".

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Egy egyéni vizsgálati alkalmazás átjáró konfigurációs objektum létrehozása

Az alkalmazás átjáró létrehozása előtt beállítása az összes konfigurációs elemek. Az alábbi lépésekkel létrehozása a konfigurációs elemek, amelyek egy alkalmazás átjáró erőforrás van szükség.

### <a name="step-1"></a>Lépés: 1

Hozzon létre egy alkalmazás gateway IP-konfiguráció "gatewayIP01" nevű. Átjáró alkalmazás indításakor felveszi a beállított alhálózat IP-címet, és az IP-címek a háttéradatbázist IP-készletre irányítja a hálózati forgalmának engedélyezésére. Ne feledje, hogy az egyes példányok vegye egy IP-címet.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Lépés: 2


IP-címek "pool01" nevű háttéradatbázist IP-cím készlet konfigurálása "134.170.185.46, 134.170.188.221,134.170.185.50". Ezek az értékek az IP-címek mentsen a az előtér-végponton érkező forgalmat. A fenti hozzáadása a saját alkalmazás IP-cím végpontok IP-címek cseréje

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>3 lépés


Az egyéni vizsgálati ebben a lépésben be van állítva.

A paraméterek, használja a következők:

- **Intervallum** - vizsgálati intervallum ellenőrzést a másodpercben megadott állítja be.
- **Időkorlát** - határozza meg, hogy a vizsgálati időtúllépését egy HTTP válasz jelölőnégyzetet.
- **-Hostname (állomásnév) és az elérési út** - teljes URL-címe, amely a példány a állapotáról alkalmazás átjáró által indított. Például ha egy webhely http://contoso.com/, majd az egyéni vizsgálati konfigurálhatja a "http://contoso.com/path/custompath.htm" vizsgálati ellenőrzésére, hogy HTTP sikeres választ.
- **UnhealthyThreshold** - szeretne megjelölni, mint a háttéradatbázist példány szükséges sikertelen a HTTP válaszok számát *megsérült*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Lépés: 4

Állítsa be az alkalmazás átjáró beállítása "poolsetting01" a forgalmat a háttéradatbázist készletben. Ezt a lépést, amely a háttéradatbázist készlet válasz egy alkalmazás átjáró felkérésre időtúllépési konfiguráció tartalmaz. Háttéradatbázis választ egy időkorlátot találatok, amikor az alkalmazás átjáró lemondja a kérést. Ez az érték, amely csak a háttéradatbázist választ ellenőrzések probe vizsgálati időtúllépés különbözik.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>5 lépésben

Állítsa be az előtér-IP-portot "frontendport01" nevű a nyilvános IP-végpontot.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>6 lépés

Az előtér-IP-konfiguráció "fipconfig01" nevű létrehozása és hozzárendelése a nyilvános IP-címet az előtér-IP-konfiguráció.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Lépés 7

Létrehozása a figyelő neve "listener01" és az előtér-port az előtér-IP-konfiguráció társítani.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Lépés 8

A betöltés terheléselosztó útválasztási szabály létrehozása nevű "rule01", amely konfigurálja a betöltés terheléselosztó viselkedését.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Lépés 9

Állítsa be az alkalmazás átjáró példány méretét.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. Standard_Small Standard_Medium és Standard_Large közül választhat.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Hozzon létre egy alkalmazás átjáró új-AzureRmApplicationGateway használatával

Hozzon létre egy alkalmazás átjáró konfigurációs összes elemet a fenti lépéseket. Ebben a példában az alkalmazás átjáró neve "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Egy vizsgálati hozzáadása egy meglévő alkalmazás átjáró

Ha egy egyéni vizsgálati felvenni egy meglévő alkalmazás átjáró négy lépést.

### <a name="step-1"></a>Lépés: 1

Az alkalmazás átjáró erőforrás betöltése változó PowerShell **Get-AzureRmApplicationGateway**használatával.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Lépés: 2

Egy vizsgálati vehet fel a meglévő átjáró konfigurációt.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


A példában az egyéni vizsgálati kereséséhez URL-cím elérési út contoso.com/path/custompath.htm 30 másodpercenként van beállítva. 8 sikertelen vizsgálati kérelmek maximális száma egy időtúllépési küszöbérték 120 másodpercig van beállítva.

### <a name="step-3"></a>3 lépés

A vizsgálati hozzáadása a háttéradatbázist készlet beállítások és az időtúllépés használatával **-Set-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Lépés: 4

A konfiguráció mentésének az alkalmazás átjáró **Set-AzureRmApplicationGateway**használatával.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Egy vizsgálati eltávolítása egy meglévő alkalmazás átjáró

Az alábbiakban a lépéseket, ha el szeretne távolítani egy egyéni vizsgálati egy meglévő alkalmazás átjáró.

### <a name="step-1"></a>Lépés: 1

Az alkalmazás átjáró erőforrás betöltése változó PowerShell **Get-AzureRmApplicationGateway**használatával.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Lépés: 2

Távolítsa el a vizsgálati konfiguráció az alkalmazás átjáró **Eltávolítása-AzureRmApplicationGatewayProbeConfig**használatával.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>3 lépés

A háttéradatbázist készlet eltávolítása a vizsgálati és az időtúllépés beállításoknál frissítése használatával **-Set-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Lépés: 4

A konfiguráció mentésének az alkalmazás átjáró **Set-AzureRmApplicationGateway**használatával. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

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

Ismerje meg, hogyan konfigurálása SSL mert megtalálhatók az [SSL kiürítése konfigurálása](application-gateway-ssl-arm.md)

