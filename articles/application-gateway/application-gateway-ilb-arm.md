<properties
   pageTitle="Létrehozása és konfigurálása a egy alkalmazás átjáró belső terheléselosztó (ILB) az Azure-kezelővel |} Microsoft Azure"
   description="Ezen az oldalon útmutatás hozhat létre, beállítása, indítása és az Azure alkalmazás átjárónak belső terheléselosztó (ILB) az Azure erőforrás-kezelő törlése"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Hozzon létre egy belső terheléselosztó (ILB) alkalmazás átjárónak Azure erőforrás-kezelő használatával

> [AZURE.SELECTOR]
- [Azure klasszikus lépések](application-gateway-ilb.md)
- [Erőforrás-kezelő PowerShell-lépések](application-gateway-ilb-arm.md)

Azure alkalmazás átjáró beállíthatók egy internetes virtuális, vagy nem érhető el az interneten, más néven belső terhelés (ILB) terheléselosztó zárólap belső zárólap. Az átjáró beállítása egy ILB, akkor lehet hasznos, belső a vállalati verziós alkalmazások, amelyek nem érhetők el az interneten. Is használható szolgáltatások és a több szálon alkalmazáson belül rétegek, ülnie biztonsági oszlopazonosító jobboldali, amely nem érhető el az interneten, de továbbra is szükség ciklikus betöltése terjesztési, munkamenet ragadósság vagy Secure Sockets Layer (SSL) lemondási.

Ez a cikk végigvezeti a lépéseket követve állítson be egy alkalmazás átjáró egy ILB.

## <a name="before-you-begin"></a>Első lépések

1. A webes Platform telepítő használatával telepítse az Azure PowerShell-parancsmagok legújabb verzióját. Töltse le, és telepítse a legújabb verzióját a **Windows PowerShell** szakaszáról [letöltések oldalt](https://azure.microsoft.com/downloads/).
2. Létrehozhat egy virtuális hálózati és alhálózat alkalmazás átjáró. Ügyeljen arra, hogy ne virtuális gépeken futó vagy felhőalapú telepítések az alhálózathoz használja. Alkalmazás átjáró virtuális hálózati alhálózat önmagában kell lennie.
3. Az alkalmazás átjáró használandó konfigurálható kiszolgálókon léteznie kell, vagy a virtuális hálózaton vagy egy nyilvános IP virtuális létrehozott végpontjaikat vannak rendelve.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mi az az alkalmazás átjáró létrehozásához szükséges?


- **Háttéradatbázist kiszolgáló készlet:** A háttér-kiszolgálók IP-címek listája. Az IP-címek szerepel a listában a virtuális hálózatot, de az alkalmazás átjáró különböző alhálózat kell tartoznia, vagy egy nyilvános IP virtuális kell lennie.
- **Háttéradatbázist készlet kiszolgálóbeállítások:** Minden készlet beállításokat, például a port, protokoll és cookie-alapú affinitás tartalmaz. Ezeket a beállításokat is tartozik egy csoportba, és alkalmazott összes kiszolgálón a készlet belül.
- **Előtér-port:** Ez a portja a nyilvános olyan portot alkalmazás átjáró megnyitott. Forgalmat a port találatok, és kattintson az egyik a háttéradatbázist kiszolgálók átirányítását.
- **Figyelő:** A figyelő előtér-port, protokoll van (Http vagy Https, ezek nagy-és kisbetűket), és az SSL-tanúsítvány neve (ha az SSL beállítása kiürítése).
- **Szabály:** A szabály a figyelő és a háttér-kiszolgálón készlet köti és határozza meg, mely a forgalmat szeretné irányítani, amikor találatok száma a egy adott figyelő háttéradatbázist kiszolgáló készlet. Jelenleg csak az *egyszerű* szabály használata támogatott. Az *egyszerű* szabály egy ciklikus terheléselosztási.



## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása

Az Azure klasszikus és Azure erőforrás-kezelő közötti különbség a sorrendben, amelyben az alkalmazás átjáró és az elemeket, amelyeket kell beállítania létrehozása.
Az erőforrás-kezelő van minden olyan elem, amelyek a egy alkalmazás átjáró egyenként beállítva, és közzéteheti a alkalmazás átjáró erőforrás létrehozása.


Íme egy alkalmazás átjáró létrehozásához szükséges lépéseket:

1. Erőforráscsoport az erőforrás-kezelő létrehozása
2. Hozzon létre egy virtuális hálózati és az alkalmazás átjáró alhálózat
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

Hozzon létre egy új erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ez az erőforrások erőforráscsoport az alapértelmezett helye szolgál. Győződjön meg arról, hogy minden parancs az alkalmazás átjáró létrehozásához használja az azonos erőforráscsoport.

A fenti példában létrehozott egy úgynevezett "appgw-rg" és "US nyugati" hely erőforráscsoport.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Hozzon létre egy virtuális hálózati és az alkalmazás átjáró alhálózat

A következő példa bemutatja, hogyan hozhat létre virtuális hálózat erőforrás-kezelő használatával:

### <a name="step-1"></a>Lépés: 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

A cím tartomány 10.0.0.0/24 rendel egy virtuális hálózati létrehozásához használandó alhálózat változó.

### <a name="step-2"></a>Lépés: 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Ezzel létrehoz egy virtuális hálózati csoport "appgw-rg erőforrás" a "appgwvnet" nevű alhálózat 10.0.0.0/24 az előtag 10.0.0.0/16 használ, a nyugati USA-beli területhez tartozik.

### <a name="step-3"></a>3 lépés

    $subnet = $vnet.subnets[0]

Az alhálózathoz objektum rendel a következő lépésekkel $subnet változó.

## <a name="create-an-application-gateway-configuration-object"></a>Konfigurációs objektum alkalmazás átjáró létrehozása

### <a name="step-1"></a>Lépés: 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Ezzel létrehozott egy alkalmazás gateway IP-konfiguráció "gatewayIP01" nevű. Átjáró alkalmazás indításakor felveszi a beállított alhálózat IP-címet, és az IP-címek a háttéradatbázist IP-készletre irányítja a hálózati forgalmának engedélyezésére. Ne feledje, hogy az egyes példányok vegye egy IP-címet.


### <a name="step-2"></a>Lépés: 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Ezzel a IP-címek "pool01" nevű háttéradatbázis IP-cím készlet "134.170.185.46, 134.170.188.221,134.170.185.50". Láthatóvá válnak a IP-címek mentsen a az előtér-végponton érkező forgalmat. A fenti hozzáadása a saját alkalmazás IP-cím végpontok IP-címek cseréje

### <a name="step-3"></a>3 lépés

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Ez a háttéradatbázist készletbe alkalmazás átjáró beállítása "poolsetting01" a betöltés az kiegyensúlyozott hálózati forgalmának engedélyezésére állítja be.

### <a name="step-4"></a>Lépés: 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Ezzel az előtér-IP-portot "frontendport01" nevű a ILB.

### <a name="step-5"></a>5 lépésben

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Az előtér-IP-konfiguráció "fipconfig01" nevű hoz létre, és a társít a virtuális alhálózathoz egy privát IP.

### <a name="step-6"></a>6 lépés

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

A "listener01" nevű figyelő hoz létre, és a az előtér-port az előtér-IP-konfiguráció társít.

### <a name="step-7"></a>Lépés 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Ezzel létrehoz, hogy a betöltés terheléselosztó útválasztási szabály neve "rule01", amely a betöltés terheléselosztó viselkedés konfigurálja.

### <a name="step-8"></a>Lépés 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Ez az alkalmazás átjáró példány méretének konfigurálása

>[AZURE.NOTE]  *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. Standard_Small Standard_Medium és Standard_Large közül választhat.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Hozzon létre egy alkalmazás átjáró új-AzureApplicationGateway használatával

Létrehoz egy alkalmazás átjáró konfigurációs összes elemet a fenti lépéseket. Ebben a példában az alkalmazás átjáró neve "appgwtest".


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Ez létrehoz egy alkalmazás átjáró konfigurációs összes elemet a fenti lépéseket. A példában az alkalmazás átjáró neve "appgwtest".


## <a name="delete-an-application-gateway"></a>Az alkalmazás átjáró törlése

Ha törölni szeretne egy alkalmazás átjáró, kell hajtsa végre a következő sorrendben:

1. A **Stop-AzureRmApplicationGateway** parancsmag használatával leállítása az átjárót.
2. Az **Eltávolítás-AzureRmApplicationGateway** parancsmag használatával az átjáró törlése.
3. Győződjön meg arról, hogy az átjáró el lett távolítva a **Get-AzureApplicationGateway** parancsmag használatával.


### <a name="step-1"></a>Lépés: 1

Az alkalmazás átjáró objektum felvenni, és azt egy "$getgw" változó társítani.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Lépés: 2

Használja a **Leállítás-AzureRmApplicationGateway** le szeretné állítani az alkalmazás átjáró. Ez a példa bemutatja a **Stop-AzureRmApplicationGateway** parancsmagot az első sorban a kimenet követi.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Miután az alkalmazás átjáró leállítva állapotba kerül, távolítsa el a szolgáltatás **Eltávolítása-AzureRmApplicationGateway** parancsmag használatával.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] A **-kényszerítése** kapcsoló akkor használható a eltávolítása megerősítő üzenet elrejtése.


Ha ellenőrizni szeretné, hogy a szolgáltatás megszűnt, a **Get-AzureRmApplicationGateway** parancsmag is használhatja. Ez a lépés nem szükség.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Következő lépések

Ha szeretné az SSL kiürítése konfigurálása, [konfigurálása az SSL-alkalmazás átjáró, kiürítése](application-gateway-ssl.md)című témakört.

Az alkalmazás átjáró használata egy ILB beállítása, című témakörben olvashat [létrehozása egy alkalmazás átjárónak belső terheléselosztó (ILB)](application-gateway-ilb.md).

Ha azt szeretné, hogy további információt az általános betöltése terheléselosztó beállítások, jelenik meg:

- [Azure terheléselosztó](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure forgalom Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
