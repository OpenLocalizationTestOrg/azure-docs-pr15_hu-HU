<properties
    pageTitle="Állítsa be az SSL-házirend és végpont SSL alkalmazás átjáró |} Microsoft Azure"
    description="Ez a cikk leírja, hogy miként állítható be végpont SSL az alkalmazás átjáró Azure erőforrás-kezelő PowerShell használatával"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>SSL-házirend és végpont SSL konfigurálása alkalmazás átjáró PowerShell használatával

## <a name="overview"></a>– Áttekintés

Alkalmazás átjáró forgalom végpont titkosítását támogatja. Alkalmazás átjáró végzi az SSL-kapcsolat a az alkalmazás átjáró leállítása. Az átjáró majd útválasztási szabályok vonatkozik a forgalmat, újra titkosítja a csomagot, és továbbítja az alapján meghatározott útválasztási szabályoknak megfelelő kódmentes a csomagot. Az érintett webkiszolgálóra válaszának végig a ugyanezt az eljárást, vissza kell a végfelhasználó.

Egy másik funkció az adott alkalmazás átjáró támogatja az letiltása bizonyos az SSL protokoll verzióit. Alkalmazás átjáró támogatja a következő protokoll verzió letiltása TLSv1.0, TLSv1.1 és TLSv1.2.

> [AZURE.NOTE] SSL 2.0-s és az SSL 3.0 alapértelmezés szerint ki vannak kapcsolva, és nem lehet engedélyezni. Nem biztonságos számítanak, és nem jelennek meg az alkalmazás átjáró használandó

![Példa képe][scenario]

## <a name="scenario"></a>Eset

Ebben az esetben megismerheti, hogyan hozhat létre egy alkalmazás átjáró végpont SSL-kapcsolaton keresztül PowerShell használatával.

Ebben az esetben fognak:

- "Appgw-rg" nevű erőforrás csoport létrehozása
- Hozzon létre egy 10.0.0.0/16 fenntartott CIDR szövegrészt a "appgwvnet" nevű virtuális hálózat.
- Hozzon létre két alhálózat "appgwsubnet" és "appsubnet" nevezik.
- Bizonyos az SSL protokoll letiltó végpont SSL-titkosítás támogató kis alkalmazás átjáró létrehozása.

## <a name="before-you-begin"></a>Első lépések

Végpont SSL-alkalmazás átjáró konfigurálásához egy tanúsítványt az átjáró szükség, és tanúsítványra szükség a kódmentes kiszolgálókhoz. Az átjárótanúsítványt titkosítása és visszafejteni az SSL használatával küldött forgalmat használják. Az átjárótanúsítványt kell lennie a személyes információkat Exchange (pfx) formátumban. Ez a fájlformátum lehetővé teszi, hogy a titkos kulcs exportálni, amely szerint az alkalmazás átjáró a titkosítási és forgalom visszafejtése végrehajtásához szükséges.

A kódmentes végpont ssl-titkosítás alkalmazás átjáró whitelisted kell lennie. Erre a háttérkiszolgálókon nyilvános oklevél feltöltése az alkalmazás átjáró. Ezzel biztosíthatja, hogy az alkalmazás átjáró csak kommunikál ismert háttér-kiszolgálói példány. A további titkosítja a végpontok közötti kommunikációt.

Ez a folyamat az alábbi lépésekkel szemlélteti:

## <a name="create-the-resource-group"></a>Az erőforráscsoport létrehozása

Ez a szakasz végigvezeti az erőforrás csoportot, amely tartalmazza az alkalmazás átjáró hoz létre.

### <a name="step-1"></a>Lépés: 1

Jelentkezzen be az Azure-fiókjába.

    Login-AzureRmAccount

### <a name="step-2"></a>Lépés: 2

Jelölje ki azt az előfizetést, ebben az esetben használható.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>3 lépés

Erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja) létrehozása.

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Hozzon létre egy virtuális hálózati és az alkalmazás átjáró alhálózat

Az alábbi példa létrehoz egy virtuális hálózati és két alhálózat. Egy alhálózat az alkalmazás átjáró tárolására szolgál. A többi alhálózat a szolgáltatója a webalkalmazás háttérkiszolgálókon szolgál.

### <a name="step-1"></a>Lépés: 1

Rendelje hozzá a-címtartományokat esetében az alhálózathoz használható az alkalmazás átjáró magát.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Alkalmazás átjáró beállított alhálózat megfelelően kell méretezni. Az alkalmazás átjáró legfeljebb 10 előfordulását beállíthatók. Egyes példányok megnyitja az alhálózathoz 1 IP-címét. Túl kicsi alhálózat negatív befolyásolhatja a méretezés-alkalmazás átjáró kifelé.

### <a name="step-2"></a>Lépés: 2

Rendelje hozzá a Kódmentes cím készlet használandó cím cellatartományokba.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>3 lépés

Hozzon létre egy virtuális hálózati a alhálózat az előző lépésben megadott.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Lépés: 4

Beolvasása a virtuális hálózati és alhálózat erőforrást kell használni az alábbi lépéseket:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Hozzon létre egy nyilvános IP-címet az előtér-konfiguráció

Hozzon létre egy nyilvános IP-erőforrás használható az alkalmazás átjáró. A nyilvános IP-címet használja a következő lépésre.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Alkalmazás átjáró nem támogatja a megadott tartomány címkével ellátott létrehozott nyilvános IP-cím használata. Csak nyilvános, dinamikusan létrehozott tartomány címkével ellátott IP-cím használata támogatott. Ha szüksége van egy könnyen megjegyezhető dns nevet az alkalmazás átjáró, ajánlott alias használni egy cname rekordot.

## <a name="create-an-application-gateway-configuration-object"></a>Konfigurációs objektum alkalmazás átjáró létrehozása

Az alkalmazás átjáró létrehozása előtt be kell állítania az összes konfigurációs elemek. Az alábbi lépésekkel létrehozása a konfigurációs elemek, amelyek egy alkalmazás átjáró erőforrás van szükség.

### <a name="step-1"></a>Lépés: 1

Hozzon létre egy alkalmazás gateway IP-konfiguráció, ez a beállítás milyen alhálózat, használja az alkalmazás átjáró állítja be. Átjáró alkalmazás indításakor felveszi a beállított alhálózat IP-címet, és továbbítja az IP-címek a háttéradatbázist IP készletben hálózati forgalmának engedélyezésére. Ne feledje, hogy az egyes példányok vegye egy IP-címet.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Lépés: 2

Hozzon létre egy előtér-IP-konfigurációja, ez a beállítás rendel egy személyes vagy nyilvános IP-címet az előtér-az alkalmazás átjáró. A következő lépés az előző lépésben nyilvános IP-címét az előtér-IP-konfiguráció társít.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>3 lépés

Állítsa be a háttéradatbázist IP-cím készlet kódmentes webkiszolgálón IP-címét. E IP-címei az IP-címek mentsen a az előtér-végponton érkező forgalmat. Az alábbi IP-címek hozzáadása a saját alkalmazás IP-cím végpontok cseréje

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Egy teljes tartománynevét (FQDN) érték is érvényes IP-címének a kódmentes kiszolgálók helyett a - BackendFqdns kapcsoló használatával.

### <a name="step-4"></a>Lépés: 4

Állítsa be a nyilvános IP-végpont az előtér-IP-portot. Ez a portja a végfelhasználók csatlakozó port.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>5 lépésben

Állítsa be az alkalmazás átjáró a tanúsítvány. A tanúsítvány visszafejtése és újbóli titkosítsa az alkalmazás átjáró a forgalom szolgál.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] Ez a példa konfigurálja az SSL-kapcsolat használt tanúsítvány. A tanúsítvány kell lennie .pfx formátumban, és a jelszó 4 – 12 karakter közé kell lennie.

### <a name="step-6"></a>6 lépés

A HTTP-figyelő az alkalmazás átjáró létrehozása Rendelje hozzá a előtér ip konfigurációs, portokkal és ssl használni kívánt tanúsítványt.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Lépés 7

Töltse fel a tanúsítvány használható az engedélyezett ssl kódmentes készlet erőforrásokat.

> [AZURE.NOTE] Az alapértelmezett vizsgálati nyilvános kulcs megszerzi a háttéradatbázist IP-címet az **alapértelmezett** SSL kötelezővé, és miben más az a nyilvános kulcs értékét, itt kapott nyilvános kulcs értékét. A beolvasott nyilvános kulcs nem feltétlenül a kívánt webhelyet, amelyre forgalmat a **Ha** használ host az élőfej- és SNI a háttéradatbázist flow. Ha kétségei vannak, keresse fel a vissza vége erősítse meg, melyik tanúsítványt használja az **alapértelmezett** SSL kötés https://127.0.0.1/. Ebben a szakaszban a nyilvános kulcs a kérelem használata Ha a host-fejlécek és SNI a HTTPS-kötések, és nem jelenik meg a választ, és a tanúsítvány kézi böngésző-összehívásokban való https://127.0.0.1/ a telefon vissza végén, be kell állítania egy alapértelmezett SSL kötés, a telefon vissza végén. Ha még nem úgy, szondákat meghiúsul, és a háttéradatbázist kell nem lesznek whitelisted.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] A tanúsítvány ebben a lépésben megadott kell a nyilvános kulcs a a pfx tanúsítvány a kódmentes rajta. A tanúsítvány (nem a legfelső szintű tanúsítvány) telepíteni a kiszolgálón lévő kódmentes exportálhatja. CER formázhatja, és ebben a lépésben használni. Ezt a lépést whitelists az alkalmazás átjáró kódmentes.

### <a name="step-8"></a>Lépés 8

Az alkalmazás átjáró háttéradatbázist http beállításainak konfigurálása Rendelje hozzá a tanúsítvány felöltött az előző lépésben a HTTP-beállításait.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Lépés 9

Betöltési terheléselosztó útválasztási szabály létrehozása, amely konfigurálja a betöltés terheléselosztó viselkedését. Ebben a példában egy egyszerű ciklikus szabály jön létre.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>10 lépés

Állítsa be az alkalmazás átjáró példány méretét.  A rendelkezésre álló méret **szabványos\_kis**, **szabványos\_közepes**, és **szabványos\_nagy**.  A beosztását a rendelkezésre álló értékei 1 – 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Az 1-példányok száma tesztelési célú lehet kiválasztani. Fontos tudnia, hogy, hogy minden példányok száma a két példánya nem vonatkozik a SLA, és ezért nem ajánlott. Kis átjárók vannak fejlesztők vizsgálathoz, és nem termelési célra használható.

### <a name="step-11"></a>Lépés 11

Konfigurálja az SSL házirendet, az alkalmazás átjáró használandó. Alkalmazás átjáró támogatja az azt jelenti, hogy bizonyos az SSL protokoll verziói letiltása.

A következők közül lehet letiltani protocol-verzió listáját.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

A következő példa letiltja TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Az alkalmazás átjáró létrehozása

Az előző lépéseket, hoz létre az alkalmazás átjáró. Az átjáró létrehozása a következő időigényes áll.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Az SSL protokoll verzió egy meglévő alkalmazás átjáró letiltása

A fenti lépések juthat végpont ssl alkalmazás létrehozása, és bizonyos az SSL protokoll verziói letiltása keresztül. A következő példa egy meglévő alkalmazás átjáró bizonyos SSL házirendek letiltja.

### <a name="step-1"></a>Lépés: 1

Az alkalmazás átjáró frissítése beolvasásához.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Lépés: 2

Az SSL-házirend megadása A következő példában TLSv1.0 és TLSv1.1 le van tiltva.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>3 lépés

Végül frissítse az átjárót. Fontos, hogy ne feledje, hogy ezt a lépést utolsó időigényes tevékenység. Ha elkészült, a végpont ssl alkalmazás átjáró van beállítva.

    $gw | Set-AzureRmApplicationGateway

## <a name="get-application-gateway-dns-name"></a>Átjáró DNS alkalmazásnév beszerzése

Az átjáró létrehozása után a következő lépésként az előtér-kapcsolati konfigurálása. Egy nyilvános IP használata esetén az alkalmazás átjáró dinamikusan hozzárendelt DNS nevét, amely nem rövid van szükség. Annak biztosítása érdekében a felhasználóknak az alkalmazás átjáró egy CNAME rekordot is találati használható az alkalmazás átjáró a nyilvános végpont mutasson. Az [egyéni tartománynevet az Azure-ban konfigurálása](../cloud-services/cloud-services-custom-domain-name-portal.md). Ehhez beolvashatja részleteit, az alkalmazás átjáró, valamint a társított IP/DNS nevét a PublicIPAddress elem az alkalmazás átjáró csatolt használatával. Az alkalmazás átjáró tartománynév használandó hozhat létre egy CNAME rekordot, amely a két webalkalmazások a DNS-névhez mutat. A rekordok felhasználása nem javasolt, mert a a virtuális alkalmazás átjáró újraindításra változhat.
    
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

Tudnivalók a webes alkalmazás tűzfalon keresztül alkalmazás átjáró a webalkalmazások biztonsága hardening megtalálhatók az [Webalkalmazások tűzfal – áttekintés](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
