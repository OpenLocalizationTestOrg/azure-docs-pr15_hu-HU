<properties
   pageTitle="Állítsa be a betöltés terheléselosztó TCP üresjárati időtúllépés |} Microsoft Azure"
   description="Állítsa be a betöltés terheléselosztó TCP üresjárati időkorlát"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Azure terheléselosztó TCP üresjárati időtúllépés beállításainak konfigurálása

Az alapértelmezett beállítás Azure terheléselosztó az üresjárati időtúllépés beállítása 4 perc tartalmaz. Ha hosszabb, mint az időkorlát inaktivitás ponttal, nem nem biztos, hogy a TCP- vagy HTTP-munkamenet karbantartása az ügyfél és a felhőalapú szolgáltatás között.

A kapcsolat bezárásakor az ügyfélalkalmazás jelenhet meg az alábbi hibaüzenet: "a kapcsolat lezárva: tartandó várt kapcsolatot a kiszolgáló által lezárva."

A TCP életben általános gyakorlat környezetbe. Ez az eljárás továbbra is a kapcsolat aktív hosszabb ideig. További tudnivalókért lásd: ezek a [példák .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). A életben engedélyezve, a csomagok elküldése a kapcsolaton inaktivitás időszakokban. Ezek a életben csomagok győződjön meg arról, hogy az üresjárati időkorlát soha nem ér, és a kapcsolat hosszabb ideig karbantartása.

Ez a beállítás csak a bejövő kapcsolatok használható. A kapcsolat adatvesztés elkerüléséhez állítsa be a TCP életben kisebb, mint az üresjárati időkorlát intervallum, vagy az üresjárati időkorlát növelése. A támogatási olyan esetek, hozzáadtunk egy konfigurálható üresjárati időtúllépés támogatása. Most már beállíthatja, 4-es és 30 percig időtartamára.

TCP életben működik tökéletesen, mert az esetek, amikor elem leírási_idő nem áll kényszer. Nem ajánlott mobilalkalmazások. A TCP életben mobile alkalmazásban használatával a telep eszköz gyorsabb.

![TCP-időtúllépését](./media/load-balancer-tcp-idle-timeout/image1.png)

Az alábbi szakaszok ismertetik, hogyan lehet üresjárati időtúllépés beállításainak megváltoztatása a virtuális gépeken futó és a felhőszolgáltatásokba.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>A TCP-időtúllépése konfigurálása a példány szintű nyilvános IP-15 perccel

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`nem kötelező. Ha nem állít be, az alapértelmezett időtúllépési érték a 4 perc. A elfogadható időtúllépés tartománya 4-es és 30 percig.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Az üresjárati időkorlátját állítja zárólap Azure virtuális gépen létrehozásakor

Zárólap időtúllépés beállításnak, használja az alábbiakat:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Az üresjárati időtúllépés konfigurációs beolvasásához, használja az alábbi parancsot:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>A TCP-időtúllépése meg egy terheléselosztás végpont megadása

Ha végpontok részét terheléselosztás végpontot, TCP-időtúllépését meg kell terheléselosztás végpont megadása. Példa:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Cloud services időtúllépés beállításainak módosítása

A felhőalapú szolgáltatás frissítése az Azure SDK is használhatja. A .csdef fájl módosítások felhőszolgáltatások végpont beállításait. A telepítés frissítése frissítése a TCP-időtúllépését egy felhőalapú szolgáltatásba a telepítéshez szükséges. Kivétel, ha a TCP-időtúllépés meg van adva csak egy nyilvános IP. Nyilvános IP-beállítások a .cscfg fájlban, és üzembe update vagy a frissítés frissítheti őket.

A .csdef változások végpont beállítások a következők:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

A határidő beállítása a nyilvános IP-címei az .cscfg változások a következők:

    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
        <InstanceAddress roleName="VMRolePersisted">
        <PublicIPs>
          <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
        </PublicIPs>
        </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="rest-api-example"></a>Példa a REST API-val

A TCP üresjárati időtúllépés beállíthatja a Szolgáltatáskezelés API segítségével. Győződjön meg arról, hogy a `x-ms-version` fejléce verziójára van állítva `2014-06-01` vagy újabb verziójában. A megadott terheléselosztás beviteli végpontok környezetben összes virtuális gépeken beállításainak frissítése.

### <a name="request"></a>Kérés

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Válasz

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Következő lépések

[Belső betöltés terheléselosztó – áttekintés](load-balancer-internal-overview.md)

[Első lépések az internetes terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)
