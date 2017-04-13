<properties
   pageTitle="Új vagy meglévő alkalmazás átjáró webes alkalmazás tűzfal beállítása |} Microsoft Azure"
   description="Ez a cikk egy meglévő vagy új alkalmazás átjáró webes alkalmazás tűzfal használatának megkezdése módját ismerteti."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Egy új vagy meglévő alkalmazás átjáró webes alkalmazás tűzfal konfigurálása

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-web-application-firewall-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-web-application-firewall-powershell.md)

A webes alkalmazás tűzfal (WAF) Azure alkalmazás átjáró webalkalmazások közös webes célú támadásokkal például SQL-beszúrás, a webhelyközi parancsfájlok célú támadásokkal és a munkamenet kihasználásának védelmet nyújt.

Azure alkalmazás az átjáró az egy réteg-7 terheléselosztó. A feladatátvétel HTTP-kérések teljesítményt továbbítása különböző kiszolgálók között, akár a helyszíni vagy felhőalapú biztosít. Alkalmazás számos alkalmazás kézbesítési vezérlő LÉPETT funkciói, köztük a HTTP terheléselosztás, cookie-alapú munkamenet affinitás, akkor Secure Sockets Layer (SSL) kiürítése, egyéni állapot szondákat, több webhelyen támogatása és számos más. Ha a támogatott szolgáltatások listáját, látogasson el az alkalmazás átjáró – áttekintés

A következő cikk azt mutatja [hozzáadása egy meglévő alkalmazás átjáró webes alkalmazás tűzfalat](#add-web-application-firewall-to-an-existing-application-gateway) , és [egy webes alkalmazás tűzfal használó alkalmazás átjáró létrehozása](#create-an-application-gateway-with-web-application-firewall).

![Példa képe][scenario]

## <a name="waf-configuration-differences"></a>WAF konfigurációs különbségek

Elolvasott [létrehozása PowerShell-alkalmazás átjárónak](application-gateway-create-gateway-arm.md), ha ismeri a Termékváltozat beállítások konfigurálása az alkalmazás átjáró létrehozásakor. WAF megadása egy alkalmazás átjáró SKU konfigurálásakor további beállításokat is tartalmaz. Vannak nem további végzett módosítások alkalmazása átjáró magát.

**Termékváltozat** - nélkül WAF támogatja a normál alkalmazás az átjárók **szabványos\_kis**, **szabványos\_közepes**, és **szabványos\_nagy** méretét. WAF bevezetésével, amelyek két további termékváltozatban, **WAF\_közepes** és **WAF\_nagy**. A kis alkalmazás átjárók WAF nem támogatott.

**Réteg** - elérhető értékek **Standard** vagy **WAF**. Amikor a webes alkalmazás tűzfalat használ, **WAF** kell választani.

**Mód** – Ez a beállítás nem WAF mód. engedélyezett értékei **észlelése** és **megakadályozása**. Ha WAF észlelési módban van beállítva, a naplófájl összes veszélyek tárolja. Megelőzése módban események továbbra is van-e jelentkezve, de a támadó 403 jogosulatlan választ kap a alkalmazás átjáró.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Webes alkalmazás tűzfal hozzáadása egy meglévő alkalmazás átjáró

Győződjön meg arról, hogy az Azure PowerShell legújabb verzióját használja. További információ a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)címen érhető el.

### <a name="step-1"></a>Lépés: 1

Jelentkezzen be az Azure-fiókjába.

    Login-AzureRmAccount

### <a name="step-2"></a>Lépés: 2

Jelölje ki azt az előfizetést, ebben az esetben használható.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>3 lépés

Az átjáró webes alkalmazás tűzfal hozzáadni kívánt beolvasásához.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Lépés: 4

Állítsa be a webes alkalmazás tűzfal termékváltozat. A rendelkezésre álló méret **WAF\_nagy** és **WAF\_közepes**. Webes alkalmazás tűzfal használatakor a réteg kell **WAF**, a kapacitás kell hagynia a termékváltozat beállításakor.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>5 lépésben

A következő példa definiálva, adja meg a WAF beállításokat:

A **WafMode** beállítás a rendelkezésre álló értékei megelőzése és a kimutatás.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>6 lépés

Az alkalmazás átjáró frissítése az előző lépésben definiált beállításokat.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Ez a parancs az alkalmazás átjáró frissíti a webes alkalmazás tűzfallal. Ajánlott [Alkalmazás átjáró diagnosztika](application-gateway-diagnostics.md) megértéséhez, hogy miként tekintheti meg az alkalmazás átjáró naplók megtekintéséhez. WAF biztonsági jellegét, miatt naplók szükség rendszeresen ellenőrizni kell a biztonsági testtartását a webalkalmazások megérthető.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Hozzon létre egy alkalmazás átjáró tűzfallal weben alkalmazás

Az alábbi lépéseket a teljes folyamat elejétől a végéig hozhat létre egy alkalmazás átjáró tűzfallal weben alkalmazás végigvezeti Önt.

Győződjön meg arról, hogy az Azure PowerShell legújabb verzióját használja. További információ a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)címen érhető el.

### <a name="step-1"></a>Lépés: 1

Bejelentkezés az Azure

    Login-AzureRmAccount

A hitelesítő adatokkal hitelesítést végezni kéri.

### <a name="step-2"></a>Lépés: 2

Jelölje be az előfizetések a fiókhoz.

    Get-AzureRmSubscription

### <a name="step-3"></a>3 lépés

A használandó Azure előfizetések kiválasztása.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Lépés: 4

Erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja) létrehozása.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ezen a helyen az erőforrások erőforráscsoport az alapértelmezett hely szolgál. Győződjön meg arról, hogy minden parancs az alkalmazás átjáró létrehozásához használja az azonos erőforráscsoport.

Az előző példában létrehozott egy úgynevezett "appgw-RG" és "US nyugati" hely erőforráscsoport.

>[AZURE.NOTE] Állítsa be az alkalmazás átjáró egy egyéni vizsgálati van szüksége, ha a [létrehozása egy PowerShell használatával egyéni szondákat alkalmazás átjárónak](application-gateway-create-probe-ps.md)témakörben találhat. Nézze meg az [egyéni szondákat és a rendszerállapot figyelése](application-gateway-probe-overview.md) további információt.

### <a name="step-5"></a>5 lépésben

Rendelje hozzá a-címtartományokat esetében az alhálózathoz használható az alkalmazás átjáró magát.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Az alkalmazás alhálózat 28 maszk bittel legalább kell rendelkeznie. Ezt az értéket a az alkalmazás átjárópéldány-alhálózat hagyja a 10 elérhető címet. Az egy kisebb alhálózat, előfordulhat, hogy nem az alkalmazás átjáró több példány a jövőben adni.

### <a name="step-6"></a>6 lépés

Rendelje hozzá a Kódmentes cím készlet használandó cím cellatartományokba.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Lépés 7

Egy-a megelőző alhálózat az erőforráscsoport virtuális hálózaton lépésben létrehozott létrehozása: [az erőforrás-csoport létrehozása](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Lépés 8

Beolvasása a virtuális hálózati és alhálózat erőforrást kell használni az alábbi lépéseket:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Lépés 9

Hozzon létre egy nyilvános IP-erőforrás használható az alkalmazás átjáró. A nyilvános IP-cím szerepel a következő lépések egyikét:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Alkalmazás átjáró nem támogatja a megadott tartomány címkével ellátott létrehozott nyilvános IP-cím használata. Csak nyilvános, dinamikusan létrehozott tartomány címkével ellátott IP-cím használata támogatott. Ha szüksége van egy könnyen megjegyezhető dns nevet az alkalmazás átjáró, ajánlott alias használni egy cname rekordot.

### <a name="step-10"></a>10 lépés

Az alkalmazás átjáró létrehozása előtt be kell állítania az összes konfigurációs elemek. Az alábbi lépésekkel létrehozása a konfigurációs elemek, amelyek egy alkalmazás átjáró erőforrás van szükség.

Hozzon létre egy alkalmazás gateway IP-konfiguráció, ezzel beállíthatja, hogy milyen alhálózat az alkalmazás átjáró használja. Átjáró alkalmazás indításakor felveszi a beállított alhálózat IP-címet, és továbbítja az IP-címek a háttéradatbázist IP készletben hálózati forgalmának engedélyezésére. Ne feledje, hogy az egyes példányok vegye egy IP-címet.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Lépés 11

Állítsa be a háttéradatbázist IP-cím készlet kódmentes webkiszolgálón IP-címét. E IP-címei az IP-címek mentsen a az előtér-végponton érkező forgalmat. Az alábbi IP-címek hozzáadása a saját alkalmazás IP-cím végpontok cseréje

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Lépésben 12

Töltse fel a tanúsítvány használható az engedélyezett ssl kódmentes készlet erőforrásokat.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Lépés a 13-mal

Az alkalmazás átjáró háttéradatbázist http beállításainak konfigurálása Rendelje hozzá a tanúsítvány feltölteni az előző lépésben a HTTP-beállításait.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Lépés 14

Állítsa be a nyilvános IP-végpont az előtér-IP-portot. Ez a portja a végfelhasználók csatlakozó port.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Lépés 15

Hozzon létre egy előtér-IP-konfigurációja, ez a beállítás rendel egy személyes vagy nyilvános IP-címet az előtér-az alkalmazás átjáró. A következő lépés az előző lépésben nyilvános IP-címét az előtér-IP-konfiguráció társít.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>A 16

Állítsa be az alkalmazás átjáró a tanúsítvány. A tanúsítvány visszafejtése és újbóli titkosítsa az alkalmazás átjáró a forgalom szolgál.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Lépés 17

A HTTP-figyelő az alkalmazás átjáró létrehozása Rendelje hozzá a előtér ip konfigurációs, portokkal és ssl használni kívánt tanúsítványt.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Lépés a 18

Betöltési terheléselosztó útválasztási szabály létrehozása, amely konfigurálja a betöltés terheléselosztó viselkedését. Ebben a példában egy egyszerű ciklikus szabály jön létre.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Lépés 19

Állítsa be az alkalmazás átjáró példány méretét.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Választhat **WAF\_közepes** és **WAF\_nagy**, amikor mindig **WAF**WAF használata esetén a réteg. Kapacitás argumentum értéke 1 és 10 közé.

### <a name="step-20"></a>Lépés 20

A mód konfigurálása WAF, elfogadható értékek **megelőzése** és a **Kimutatás**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Lépés 21

Hozzon létre egy alkalmazás átjáró konfigurációs összes elemet a fenti lépéseket. Ebben a példában az alkalmazás átjáró neve "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Megtudhatja, hogy miként bejelentkezni az események vagy miatt nem webes alkalmazás tűzfallal megtalálhatók az [Alkalmazás átjáró diagnosztika](application-gateway-diagnostics.md) a diagnosztikai naplózás beállítása


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png