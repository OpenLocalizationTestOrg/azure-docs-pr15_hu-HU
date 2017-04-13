<properties
   pageTitle="Hozzon létre vagy és az alkalmazás átjáró törlése |} Microsoft Azure"
   description="Ezen az oldalon útmutatás hozhat létre, beállítása, indítása és az Azure alkalmazás átjáró törlése"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Hozzon létre vagy és az alkalmazás átjáró törlése

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-gateway-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klasszikus PowerShell](application-gateway-create-gateway.md)
- [Erőforrás-kezelő Azure-sablon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure alkalmazás az átjáró az egy réteg-7 terheléselosztó. A feladatátvétel HTTP-kérések teljesítményt továbbítása különböző kiszolgálók között, akár a helyszíni vagy felhőalapú biztosít. Alkalmazás átjáró sok teszi alkalmazás kézbesítési vezérlő LÉPETT funkciói, köztük a HTTP terheléselosztás cookie-alapú munkamenet affinitás, Secure Sockets Layer (SSL) kiürítése, egyéni állapot szondákat, több webhelyen támogatása és számos más. Ha a támogatott szolgáltatások listáját, látogasson el az [Alkalmazás átjáró – áttekintés](application-gateway-introduction.md)

Ez a cikk végigvezeti a hozhat létre, beállítása, indítása és az alkalmazás átjáró törlése.

## <a name="before-you-begin"></a>Első lépések

1. A webes Platform telepítő használatával telepítse az Azure PowerShell-parancsmagok legújabb verzióját. Töltse le, és telepítse a legújabb verzióját a **Windows PowerShell** szakaszáról [letöltések oldalt](https://azure.microsoft.com/downloads/).
2. Ha egy meglévő virtuális hálózat, jelölje ki egy meglévő üres alhálózat, vagy hozzon létre egy új alhálózat a meglévő virtuális hálózat kizárólag használatra szerint az alkalmazás átjáró. Nem telepíthető az alkalmazás átjáró, mint az erőforrások szeretné telepíteni az alkalmazás átjáró mögött, kivéve, ha vnet peering használatos virtuális másik hálózathoz. További tételével [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)
3. Ellenőrizze, hogy érvényes alhálózat munka virtuális hálózattal. Ügyeljen arra, hogy ne virtuális gépeken futó vagy felhőalapú telepítések az alhálózathoz használja. Az alkalmazás átjáró virtuális hálózati alhálózat önmagában kell lennie.
3. Az alkalmazás átjáró használandó konfigurálható kiszolgálókon léteznie kell, vagy a virtuális hálózaton vagy egy nyilvános IP virtuális létrehozott végpontjaikat vannak rendelve.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mi az az alkalmazás átjáró létrehozásához szükséges?

A **New-AzureApplicationGateway** parancs használatakor az alkalmazás átjáró beállítást ezen a ponton van-e beállítva, és az újonnan létrehozott erőforrás van konfigurálva XML vagy egy konfigurációs objektumot használva.

Az értékek a következők:

- **Háttéradatbázist kiszolgáló készlet:** A háttér-kiszolgálók IP-címek listája. Az IP-címek szerepel a listában vagy tartozzanak a virtuális alhálózathoz, vagy egy nyilvános IP virtuális kell lennie.
- **Háttéradatbázist készlet kiszolgálóbeállítások:** Minden készlet beállításokat, például a port, protokoll és cookie-alapú affinitás tartalmaz. Ezeket a beállításokat is tartozik egy csoportba, és alkalmazott összes kiszolgálón a készlet belül.
- **Előtér-port:** Ez a portja a nyilvános olyan portot alkalmazás átjáró megnyitott. Forgalmat a port találatok, és kattintson az egyik a háttéradatbázist kiszolgálók átirányítását.
- **Figyelő:** A figyelő előtér-port, protokoll van (Http vagy Https, ezek az értékek-és nagybetűk), és az SSL-tanúsítvány neve (ha az SSL beállítása kiürítése).
- **Szabály:** A szabály a figyelő és a háttér-kiszolgálón készlet köti és határozza meg, mely a forgalmat szeretné irányítani, amikor találatok száma a egy adott figyelő háttéradatbázist kiszolgáló készlet.

## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása

Az alkalmazás átjáró létrehozása:

1. Hozzon létre egy alkalmazás átjáró erőforrás.
2. Hozzon létre egy konfigurációs XML-fájlba vagy konfigurációs objektum.
3. Hajtsa végre az újonnan létrehozott alkalmazás átjáró erőforrás a konfigurációt.

>[AZURE.NOTE] Állítsa be az alkalmazás átjáró egy egyéni vizsgálati van szüksége, ha a [létrehozása egy PowerShell használatával egyéni szondákat alkalmazás átjárónak](application-gateway-create-probe-classic-ps.md)témakörben találhat. Nézze meg az [egyéni szondákat és a rendszerállapot figyelése](application-gateway-probe-overview.md) további információt.

![Forgatókönyv példa][scenario]

### <a name="create-an-application-gateway-resource"></a>Hozzon létre egy alkalmazás átjáró erőforrást

Az átjáró létrehozásához használja a **New-AzureApplicationGateway** parancsmag, az értékek lecserélése saját. Számlázás az átjáró nem indul el ezen a ponton. Számlázás az átjáró sikeresen elindul, amikor egy későbbi lépés kezdődik.

Az alábbi példa létrehoz egy alkalmazás átjáró, "testvnet1" és "alhálózat-1" nevű alhálózat nevű virtuális hálózat segítségével.


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Leírás*, *InstanceCount*és *GatewaySize* is választható paraméterek.

Ellenőrizze, hogy az átjáró létrejött, a **Get-AzureApplicationGateway** parancsmagot is használhatja.

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. Kicsi, közepes és nagy közül választhat.

*VirtualIPs* *Dns_név* látható és üres, mert az átjáró még nem kezdődött. Ezek a jönnek létre után az átjáró futó állapotban van.

## <a name="configure-the-application-gateway"></a>Az alkalmazás átjáró beállítása

Az alkalmazás átjáró XML- vagy konfigurációs objektum használatával adhatja meg.

## <a name="configure-the-application-gateway-by-using-xml"></a>Az alkalmazás átjáró konfigurálása XML használatával

A következő példában használatával XML-fájlban minden alkalmazás átjáró beállítást, és hajtsa végre a őket az alkalmazás átjáró erőforráshoz.  

### <a name="step-1"></a>Lépés: 1  

Másolja az alábbi szöveget a Jegyzettömbbe.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Módosíthatja az értékeket a konfigurációs elemek a zárójelek között. Mentse a fájlt kiterjesztés .xml.

>[AZURE.IMPORTANT] A protocol Http vagy Https elem kis-és nagybetűk.

A következő példa bemutatja, hogyan állíthatja be az alkalmazás átjáró konfigurációs fájl segítségével. A példa betöltés HTTP-forgalmat a nyilvános 80-as port egyenlege, és elküldi hálózati forgalmának engedélyezésére 80-as port háttéradatbázist között két IP-címet.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Lépés: 2

Ezután adja meg az alkalmazás átjáró. A **Set-AzureApplicationGatewayConfig** parancsmag használata egy konfigurációs XML-fájlt.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Állítsa be az alkalmazás átjáró konfigurációs objektum segítségével

A következő példa bemutatja, hogy miként konfigurálható az alkalmazás átjáró konfigurációs objektumok használatával. Minden konfigurációs elemek legyen konfigurálva külön-külön és adódik hozzá alkalmazás átjáró konfigurációs objektum, majd. Miután létrehozta a konfigurációs objektum, paranccsal a **Set-AzureApplicationGateway** véglegesítse a konfigurációt a korábban létrehozott alkalmazás átjáró erőforrás.

>[AZURE.NOTE] Mielőtt minden konfigurációs objektum értéket rendeli, kell deklarálhatnak, hogy milyen típusú objektum tárterületet használ PowerShell. Az első sor létrehozása az egyes elemeket határozza meg, milyen Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (objektumnév) használják.

### <a name="step-1"></a>Lépés: 1

Minden egyes konfigurációs elemek létrehozása

Hozzon létre az előtér-IP, az alábbi példában látható módon.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Az előtér-port létrehozása, az alábbi példában látható módon.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

A háttér-kiszolgálón készlet létrehozása.

 Adja meg az IP-címek bekerülnek a háttéradatbázist kiszolgáló kvótáját a következő példában látható módon.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Kövesse a $server objektum az értékeket a háttéradatbázist készlet objektumra ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Hozzon létre a háttéradatbázist server készlet beállítást.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Hozzon létre a figyelő.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Hozza létre a szabályt.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Lépés: 2

Minden egyes konfigurációs elemek hozzárendelése alkalmazás átjáró konfigurációs objektum ($appgwconfig).

Az előtér-IP-cím hozzáadása a konfiguráción.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Az előtér-port hozzáadása a konfiguráción.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

A háttéradatbázist kiszolgáló készlet hozzáadása a konfiguráción.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

A konfiguráció adja hozzá a háttéradatbázist készlet beállítást.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Adja hozzá a figyelő a konfiguráción.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

A szabály hozzáadása a konfiguráción.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>3 lépés

Hajtsa végre a konfigurációs objektum átjáró alkalmazási erőforrás **Set-AzureApplicationGatewayConfig**használatával.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Az átjáró indítása

Az átjáró beállítása után a **Kezdés-AzureApplicationGateway** parancsmag használatával indítsa el az átjárót. Az alkalmazás átjáró számlázási megkezdi az átjáró sikeres indítása után.

> [AZURE.NOTE] A **Kezdés-AzureApplicationGateway** parancsmag befejezéséhez legfeljebb 15-20 percet igénybe vehet.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Ellenőrizze az átjáró állapota

A **Get-AzureApplicationGateway** parancsmag használatával ellenőrizze az átjáró állapotát. Ha **Start-AzureApplicationGateway** sikeres volt az előző lépésben, *állapot* futnia kell, és *virtuális* és *Dns_név* érvényes értékeket kell rendelkeznie.

A következő példa bemutatja egy alkalmazás átjáró, amely legfeljebb, fut, és készen áll a forgalom készítése szánt `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Az alkalmazás átjáró törlése

Az alkalmazás átjáró törlése:

1. Használja a **Leállítás-AzureApplicationGateway** parancsmag le szeretné állítani az átjárót.
2. Az **Eltávolítás-AzureApplicationGateway** parancsmag használatával az átjáró törlése.
3. Győződjön meg arról, hogy az átjáró el lett távolítva a **Get-AzureApplicationGateway** parancsmag használatával.

A következő példa bemutatja a **Stop-AzureApplicationGateway** parancsmagot, az első sorban, a kimeneti követ.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Miután az alkalmazás átjáró leállítva állapotba kerül, távolítsa el a szolgáltatás **Eltávolítása-AzureApplicationGateway** parancsmag használatával.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Ha ellenőrizni szeretné, hogy a szolgáltatás megszűnt, a **Get-AzureApplicationGateway** parancsmag is használhatja. Ez a lépés nem szükség.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Következő lépések

Ha szeretné az SSL kiürítése konfigurálása, [konfigurálása az SSL-alkalmazás átjáró, kiürítése](application-gateway-ssl.md)című témakört.

Az alkalmazás átjáró használata egy belső terheléselosztó beállítása, című témakörben olvashat [létrehozása egy alkalmazás átjárónak belső terheléselosztó (ILB)](application-gateway-ilb.md).

Ha azt szeretné, hogy további információt az általános betöltése terheléselosztó beállítások, jelenik meg:

- [Azure terheléselosztó](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure forgalom Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png