<properties
   pageTitle="Adjon meg egy alkalmazás átjárót, az SSL kiürítése klasszikus telepítési használatával |} Microsoft Azure"
   description="Ebben a cikkben utasításokat követve hozzon létre egy alkalmazás átjáró SSL kiürítése az Azure klasszikus telepítési modell használatával."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Adjon meg egy alkalmazás átjárót, az SSL kiürítése a klasszikus telepítési modell használatával

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Azure erőforrás-kezelő PowerShell](application-gateway-ssl-arm.md)
-[Azure klasszikus PowerShell](application-gateway-ssl.md)

Azure alkalmazás átjáró beállítható úgy, hogy megszünteti a költséges SSL visszafejtés tevékenységek fordulhat elő, a webes farm elkerülése érdekében az átjáró Secure Sockets Layer (SSL) munkamenetet. Az SSL kiürítése is egyszerűbbé teszi az előtér-kiszolgáló beállítása és kezelése a webalkalmazás.

## <a name="before-you-begin"></a>Első lépések

1. A webes Platform telepítő használatával telepítse az Azure PowerShell-parancsmagok legújabb verzióját. Töltse le, és telepítse a legújabb verzióját a **Windows PowerShell** szakaszáról [letöltések oldalt](https://azure.microsoft.com/downloads/).
2. Ellenőrizheti, hogy az érvényes alhálózat munka virtuális hálózat. Ügyeljen arra, hogy ne virtuális gépeken futó vagy felhőalapú telepítések az alhálózathoz használja. Az alkalmazás átjáró virtuális hálózati alhálózat önmagában kell lennie.
3. Az alkalmazás átjáró használandó konfigurálható kiszolgálókon léteznie kell, vagy a virtuális hálózaton vagy egy nyilvános IP virtuális létrehozott végpontjaikat vannak rendelve.

Tartományi kiürítése SSL-alkalmazás átjáró, hajtsa végre sorrendben az alábbi lépéseket:

1. [Az alkalmazás átjáró létrehozása](#create-an-application-gateway)
2. [Az SSL-tanúsítványok feltöltése](#upload-ssl-certificates)
3. [Az átjáró beállítása](#configure-the-gateway)
4. [Az átjáró konfiguráció beállítása](#set-the-gateway-configuration)
5. [Az átjáró indítása](#start-the-gateway)
6. [Ellenőrizze az átjáró állapota](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása

Az átjáró létrehozásához használja a **New-AzureApplicationGateway** parancsmag, az értékek lecserélése saját. Számlázás az átjáró nem indul el ezen a ponton. Számlázás az átjáró sikeresen elindul, amikor egy későbbi lépés kezdődik.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Ellenőrizze, hogy az átjáró létrejött, a **Get-AzureApplicationGateway** parancsmag is használhatja.

A minta, *Leírás*, *InstanceCount*és *GatewaySize* is választható paraméterek. *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. A kis- és nagy más elérhető értékek vannak. *VirtualIPs* *Dns_név* látható és üres, mert az átjáró még nem kezdődött. Ezek az értékek létrehozott után az átjáró futó állapotban van.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Az SSL-tanúsítványok feltöltése

**Hozzáadása-AzureApplicationGatewaySslCertificate** segítségével töltse fel az alkalmazás átjáró a tanúsítvány *pfx* formátumban. A tanúsítvány neve a felhasználó által választott neve, és az alkalmazás átjáró belül egyedinek kell lennie. A tanúsítvány ezt a nevet az alkalmazás átjáró összes tanúsítvány adatkezelési műveletek hivatkozik.

Ez a következő példa bemutatja a parancsmag, az értékek helyettesítése a mintában a saját.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Ezután ellenőrzése a tanúsítvány feltöltés. A **Get-AzureApplicationGatewayCertificate** parancsmag használata.

Ez a példa bemutatja a parancsmag az első sorban a kimenet követi.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] A tanúsítvány jelszót nem lehet 4 – 12 karaktereket, betűket és számokat között. Speciális karakterek nem használhatók.

## <a name="configure-the-gateway"></a>Az átjáró beállítása

Az alkalmazás átjáró konfigurációja, ahol a több érték. Az értékek is kötni együtt a konfiguráció létrehozásához.

Az értékek a következők:

- **Háttéradatbázis kiszolgáló készlet:** A háttér-kiszolgálók IP-címek listája. Az IP-címek szerepel a listában vagy tartozzanak a virtuális alhálózathoz, vagy egy nyilvános IP virtuális kell lennie.
- **Háttéradatbázist készlet kiszolgálóbeállítások:** Minden készlet beállításokat, például a port, protokoll és cookie-alapú affinitás tartalmaz. Ezeket a beállításokat is tartozik egy csoportba, és alkalmazott összes kiszolgálón a készlet belül.
- **Előtér-port:** Ez a portja a nyilvános olyan portot alkalmazás átjáró megnyitott. Forgalmat a port találatok, és kattintson az egyik a háttéradatbázist kiszolgálók átirányítását.
- **Figyelő:** A figyelő előtér-port, protokoll van (Http vagy Https, ezek az értékek-és nagybetűk), és az SSL-tanúsítvány neve (ha az SSL beállítása kiürítése).
- **Szabály:** A szabály a figyelő és a háttér-kiszolgálón készlet köti és határozza meg, mely a forgalmat szeretné irányítani, amikor találatok száma a egy adott figyelő háttéradatbázist kiszolgáló készlet. Jelenleg csak az *egyszerű* szabály támogatott. Az *egyszerű* szabály egy ciklikus terheléselosztási.

**További beállítási jegyzetek**

Az SSL-tanúsítványok beállítása a protokoll **HttpListener** módosítsa *Https* (kis-és nagybetűk). A **SslCert** elem **HttpListener** bekerül az előző SSL-tanúsítványok csoportban a feltöltés használt azonos néven értékűre értékkel. Az előtér-port frissíteni kell a 443-as.

**Ahhoz, hogy a cookie-alapú affinitás**: az alkalmazás átjáró beállítható úgy, hogy biztosítása, hogy egy ügyfél munkamenetből kérelmet mindig átirányításához a azonos virtuális a webes farmban. Ebben az esetben befejeződött, hogy a munkamenet cookie-k, amely lehetővé teszi, hogy az átjáró megfelelően irányítsa át a forgalom az. Ahhoz, hogy a cookie-alapú affinitás, állítsa **CookieBasedAffinity** *engedélyezve* **BackendHttpSettings** eleme.



A konfiguráció állíthat össze,: hozzon létre egy konfigurációs objektum vagy konfigurációs XML-fájl használatával.
A konfiguráció Egyenletszerkesztővel konfigurációs XML-fájl használatával, használja az alábbi példa:

**Konfigurációs XML-minta**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>Az átjáró konfiguráció beállítása

Ezután beállíthatja az alkalmazás átjáró. A **Set-AzureApplicationGatewayConfig** parancsmag is használhatja, a konfigurációs objektummal vagy konfigurációs XML-fájl segítségével.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Az átjáró indítása

Az átjáró beállítása után a **Kezdés-AzureApplicationGateway** parancsmag használatával indítsa el az átjárót. Az alkalmazás átjáró számlázási megkezdi az átjáró sikeres indítása után.


**Megjegyzés:** A **Kezdés-AzureApplicationGateway** parancsmag befejezéséhez legfeljebb 15-20 percet igénybe vehet.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Ellenőrizze az átjáró állapota

A **Get-AzureApplicationGateway** parancsmag használatával ellenőrizze az átjáró állapotát. Ha **Start-AzureApplicationGateway** sikeres volt az előző lépésben, *állapot* futnia kell, és *VirtualIPs* és *Dns_név* érvényes értékeket kell rendelkeznie.

Ez a példa bemutatja a-alkalmazás átjáró, amely felfelé, fut, és készen áll forgalmat.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Következő lépések


Ha azt szeretné, hogy további információt az általános betöltése terheléselosztó beállítások, jelenik meg:

- [Azure terheléselosztó](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure forgalom Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)