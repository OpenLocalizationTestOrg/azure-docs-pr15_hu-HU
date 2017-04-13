<properties 
   pageTitle="Létrehozása és konfigurálása az alkalmazás átjáró a virtuális hálózatban belső terheléselosztó (ILB) |} Microsoft Azure"
   description="Ezen az oldalon az Azure alkalmazás átjáró beállítása egy belső terheléselosztás végponttal útmutatás"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Hozzon létre egy alkalmazás átjáró belső terheléselosztó (ILB)

> [AZURE.SELECTOR]
- [Azure klasszikus lépések](application-gateway-ilb.md)
- [Erőforrás-kezelő Powershell-lépések](application-gateway-ilb-arm.md)

Alkalmazás átjáró beállíthatók virtuális IP-internetes vagy egy belső végpontot, nem érhető el az interneten, más néven belső betöltés terheléselosztó (ILB) végpontot. Az átjáró beállítása egy ILB, akkor lehet hasznos, nem érhető el interneten belső a vállalati verziós alkalmazások. Érdemes emellett hasznos szolgáltatások/rétegek a több szálon alkalmazásban, amely helyezkedik el a nem érhető el interneten biztonsági oszlopazonosító jobboldali, de továbbra is a ciklikus terheléselosztási, munkamenet ragadósság vagy SSL lemondási igénylik. Ez a cikk végigvezeti a lépéseket követve állítson be egy alkalmazás átjáró egy ILB.

## <a name="before-you-begin"></a>Első lépések

1. Telepítse az Azure PowerShell-parancsmagok használata a webes Platform telepítő legújabb verzióját. Töltse le, és telepítse a legújabb verziót, [oldal letöltése](https://azure.microsoft.com/downloads/) **A Windows PowerShell** szakaszában.
2. Ellenőrizze, hogy érvényes alhálózat munka virtuális hálózattal.
3. Ellenőrizheti, hogy kódmentes kiszolgálók a virtuális hálózaton, vagy egy nyilvános IP/virtuális rendelt.


Az alkalmazás átjáró létrehozásához tegye a következőket a sorrendben. 

1. [Az alkalmazás átjáró létrehozása](#create-a-new-application-gateway)
2. [Az átjáró beállítása](#configure-the-gateway)
3. [Az átjáró konfiguráció beállítása](#set-the-gateway-configuration)
4. [Az átjáró indítása](#start-the-gateway)
4. [Ellenőrizze az átjáró](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása:

**Az átjáró**, használja a `New-AzureApplicationGateway` parancsmag, az értékek lecserélése saját. Figyelje meg, hogy az átjáró számlázási nem indul el, ezen a ponton. Számlázás az átjáró sikeresen elindul, amikor egy későbbi lépés kezdődik.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Ellenőrzéséhez** , hogy az átjáró jött létre, használhatja a `Get-AzureApplicationGateway` parancsmag. 

A minta, *Leírás*, *InstanceCount*és *GatewaySize* is választható paraméterek. *InstanceCount* alapértelmezett értéke 2 10 maximális érték. *GatewaySize* alapértelmezett értéke közepes. A kis- és nagy más elérhető értékek vannak. *Virtuális* *Dns_név* látható és üres, mert az átjáró még nem kezdődött. Ezek a jönnek létre után az átjáró futó állapotban van. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Az átjáró beállítása

Az alkalmazás átjáró konfigurációja, ahol a több érték. Az értékek is kötni együtt a konfiguráció létrehozásához.
 
Az értékek a következők:

- **Kódmentes server készlet:** A kódmentes kiszolgálók IP-címek listája. Az IP-címek szerepel a listában vagy a VNet alhálózat kell tartoznia, vagy egy nyilvános IP virtuális kell lennie. 
- **Kódmentes készlet kiszolgálóbeállítások:** Minden készlet beállításokat, például a port, protokoll és cookie-alapú affinitás tartalmaz. Ezeket a beállításokat is tartozik egy csoportba, és alkalmazott összes kiszolgálón a készlet belül.
- **Frontend Port:** Ez a portja a megnyitott alkalmazás átjáró nyilvános portot. Forgalmat a port találatok, és kattintson az egyik kódmentes kiszolgálókon átirányítását.
- **Figyelő:** A figyelő frontend port, protokoll van (Http vagy Https, ezek nagy-és kisbetűket), és az SSL-tanúsítvány neve (ha az SSL beállítása kiürítése). 
- **Szabály:** A szabály a figyelő és a kódmentes kiszolgáló készlet köti, és milyen kódmentes kiszolgáló készlet a forgalmat irányítani a találatok száma a egy adott figyelő amikor megadja. Jelenleg csak az *egyszerű* szabály használata támogatott. Az *egyszerű* szabály egy ciklikus terheléselosztási.

A konfiguráció állíthat össze,: hozzon létre egy konfigurációs objektum vagy konfigurációs XML-fájl használatával. A konfiguráció Egyenletszerkesztővel konfigurációs XML-fájl használatával, használja az alábbi minta.



Vegye figyelembe az alábbiakat:


- A *FrontendIPConfigurations* elem az alkalmazás átjáró konfigurálása a egy ILB szempontjából ILB adatait ismerteti. 

- "Magánjellegű" kell állítani a Frontend IP- *típus*

- A kívánt belső IP, amelyen az átjáró kap a forgalmat a *StaticIPAddress* kell beállítani. Ne feledje, hogy a *StaticIPAddress* elem nem kötelező. Ha nem elérhető belső IP-címre a telepített alhálózat beállítása, van kiválasztva. 

- Az érték a megadott *FrontendIPConfiguration* *neve* elem a HTTPListener *FrontendIP* eleme használandó a FrontendIPConfiguration hivatkozik.

 **Konfigurációs XML-minta**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Az átjáró konfiguráció beállítása

Ezután beállíthatja az alkalmazás átjáró fogja. Használhatja a `Set-AzureApplicationGatewayConfig` parancsmagot a konfigurációs objektum, vagy egy konfigurációs XML-fájlt. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Az átjáró indítása

Miután az átjáró van konfigurálva, a `Start-AzureApplicationGateway` parancsmag az átjáró indításához. Az alkalmazás átjáró számlázási megkezdi az átjáró sikeres indítása után. 


> [AZURE.NOTE] A `Start-AzureApplicationGateway` parancsmag előfordulhat, hogy legfeljebb 15-20 percet igénybe vehet. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Ellenőrizze az átjáró állapota

Használja a `Get-AzureApplicationGateway` parancsmag ellenőrizze az átjáró állapotát. *Kezdés-AzureApplicationGateway* sikeres volt az előző lépésben, ha az állapot kell *fut*, és a virtuális és Dns_név van, hogy érvényes értékeket. Ez a példa bemutatja a parancsmag az első sorban a kimenet követi. Az ebben a példában az átjáró fut, és készen áll forgalom. 

> [AZURE.NOTE] Az alkalmazás átjáró fogadja el a beállított ILB végpont 10.0.0.10 ebben a példában a forgalom van beállítva.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Következő lépések


Ha azt szeretné, hogy további információt az általános betöltése terheléselosztó beállítások, jelenik meg:

- [Azure terheléselosztó](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure forgalom Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
