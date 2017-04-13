<properties
   pageTitle="A klasszikus telepítési modell PowerShell használatával hozzon létre egy egyéni vizsgálati alkalmazás átjáró |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy egyéni vizsgálati alkalmazás átjáró, a klasszikus telepítési modell PowerShell használatával"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Hozzon létre egy egyéni vizsgálati Azure alkalmazás átjáró (klasszikus) megismerkedhet a PowerShell használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-probe-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-probe-ps.md)
- [Azure klasszikus PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása

Az alkalmazás átjáró létrehozása:

1. Hozzon létre egy alkalmazás átjáró erőforrás.
2. Hozzon létre egy konfigurációs XML-fájlba vagy konfigurációs objektum.
3. Hajtsa végre az újonnan létrehozott alkalmazás átjáró erőforrás a konfigurációt.

### <a name="create-an-application-gateway-resource"></a>Hozzon létre egy alkalmazás átjáró erőforrást

Az átjáró létrehozásához használja a **New-AzureApplicationGateway** parancsmag, az értékek lecserélése saját. Számlázás az átjáró nem indul el ezen a ponton. Számlázás az átjáró sikeresen elindul, amikor egy későbbi lépés kezdődik.

Az alábbi példa létrehoz egy alkalmazás átjáró, "testvnet1" és "alhálózat-1" nevű alhálózat nevű virtuális hálózat segítségével.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Ellenőrizze, hogy az átjáró létrejött, a **Get-AzureApplicationGateway** parancsmagot is használhatja.

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. Kicsi, közepes és nagy közül választhat.

 *VirtualIPs* *Dns_név* látható és üres, mert az átjáró még nem kezdődött. Ezek a jönnek létre után az átjáró futó állapotban van.

## <a name="configure-an-application-gateway"></a>Az alkalmazás átjáró beállítása

Az alkalmazás átjáró XML- vagy konfigurációs objektum használatával adhatja meg.

## <a name="configure-an-application-gateway-by-using-xml"></a>XML-alkalmazás átjáró konfiguráljon

A következő példában használatával XML-fájlban minden alkalmazás átjáró beállítást, és hajtsa végre a őket az alkalmazás átjáró erőforráshoz.  

### <a name="step-1"></a>Lépés: 1

Másolja az alábbi szöveget a Jegyzettömbbe.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Módosíthatja az értékeket a konfigurációs elemek a zárójelek között. Mentse a fájlt kiterjesztés .xml.

A következő példa bemutatja, hogyan konfigurációs fájl segítségével állítsa be az alkalmazás átjáró terheléselosztó HTTP-forgalmat a nyilvános 80-as port, és a 80-as port háttéradatbázist között két IP-cím használatával egy egyéni vizsgálati küldéséhez a hálózati forgalmának engedélyezésére.

>[AZURE.IMPORTANT] A protocol Http vagy Https elem kis-és nagybetűk.

Új elem konfigurációs \<Probe\> adva egyéni szondákat konfigurálása.

A konfiguráció paraméterek a következők:

- **Név** - egyéni vizsgálati hivatkozás nevét.
- **Protocol (protokoll)** - használt protokoll (lehetséges értékei HTTP vagy HTTPS).
- **A Host** és **elérési útját** – az alkalmazás átjáró határozza meg a példány állapotát jelző által indított teljes URL-CÍMÉT az elérési. Például ha egy webhely http://contoso.com/, majd az egyéni vizsgálati konfigurálhatja a "http://contoso.com/path/custompath.htm" vizsgálati ellenőrzésére, hogy HTTP sikeres választ.
- **Intervallum** - vizsgálati intervallum ellenőrzést a másodpercben megadott állítja be.
- **Időkorlát** - határozza meg, hogy a vizsgálati időtúllépését egy HTTP válasz jelölőnégyzetet.
- **UnhealthyThreshold** - szeretne megjelölni, mint a háttéradatbázist példány szükséges sikertelen a HTTP válaszok számát *megsérült*.

A hivatkozott vizsgálati nevét a <BackendHttpSettings> rendelhet, hogy melyik háttéradatbázist készlet konfigurációs egyéni vizsgálati beállításokat használja.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Egy meglévő alkalmazás átjáró felvétele egy egyéni vizsgálati konfiguráció

Három lépésből-alkalmazás átjáró szoftver jelenlegi konfigurációjának megfelelően módosítása: a jelenlegi XML konfigurációs fájl első, módosíthatja, hogy egy egyéni vizsgálati és az alkalmazás átjáró beállítása az új XML-beállításokkal.

### <a name="step-1"></a>Lépés: 1

Az XML-fájl első get-AzureApplicationGatewayConfig használatával. Exportálja a konfiguráció XML módosítani kell, adja hozzá a vizsgálati beállítást.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Lépés: 2

Nyissa meg az XML-fájlt egy szövegszerkesztőben. Adja hozzá a `<probe>` szakasz után `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

Az XML backendHttpSettings szakaszban adja meg a vizsgálati nevét az alábbi példában látható módon:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Az XML-fájl mentése

### <a name="step-3"></a>3 lépés

Frissítse az új XML-fájlt az alkalmazás átjáró konfigurációjának **Set-AzureApplicationGatewayConfig**használatával. Az alkalmazás átjáró frissítése az új konfiguráció.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Következő lépések

Ha szeretné konfigurálni Secure Sockets Layer (SSL) kiürítése, [konfigurálása az SSL-alkalmazás átjáró, kiürítése](application-gateway-ssl.md)című témakört.

Az alkalmazás átjáró használata egy belső terheléselosztó beállítása, című témakörben olvashat [létrehozása egy alkalmazás átjárónak belső terheléselosztó (ILB)](application-gateway-ilb.md).
