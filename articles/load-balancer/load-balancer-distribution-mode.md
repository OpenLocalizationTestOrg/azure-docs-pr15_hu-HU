<properties
   pageTitle="Terheléselosztó terjesztési mód beállítása |} Microsoft Azure"
   description="Azure terhelési terheléselosztó terjesztési módja a forrás IP affinitás támogatási konfigurálása"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Az eloszlás módja terheléselosztó konfigurálása

## <a name="hash-based-distribution-mode"></a>Kivonat alapú terjesztési mód

Az alapértelmezett terjesztési algoritmus az 5-rekordhoz (forrás IP-cím, Forrásport cél IP, Célport protokoll típusa) kivonat forgalom hozzárendelése elérhető kiszolgálók. Csak egy átviteli munkamenetből ragadósság biztosít. Az adott munkamenetben csomagok irányítja át azokat a adatközpont IP (DIP) példányt az terheléselosztása végpontot mögött. Az ügyfél új munkamenet indítása ugyanazt a forrás IP-címe, amikor a forrásport változik, és egy másik DIP végpontot ugorhat a forgalmat.

![alapú kivonat terheléselosztó](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Szám 1-5-sor bármelyik eleme ki.

## <a name="source-ip-affinity-mode"></a>Forrás IP-affinitású

Forrás IP affinitás (más néven munkamenet affinitás vagy ügyfélaffinitás IP) nevű egy másik terjesztési módban van. Azure terheléselosztó beállítható úgy, hogy a 2-sor bármelyik eleme (forrás IP-címe, cél IP) vagy a 3-rekord (forrás IP-címe, cél IP, Protocol) segítségével forgalom feleltesse meg a rendelkezésre álló kiszolgálókat. Forrás IP-Címének affinitás használatával az azonos ügyfélszámítógép kezdeményezett kapcsolatok az azonos DIP végpontot ugrik.

Az alábbi ábra szemlélteti a 2-sor bármelyik eleme konfiguráció. Figyelje meg, a 2-sor bármelyik eleme működésének a terheléselosztó virtuális géphez 1 (VM1), amely ezután mentésben VM2 és VM3 keresztül.

![munkamenet affinitás](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Szám 2-2-sor bármelyik eleme ki.

Forrás IP affinitás megoldja-nem kompatibilis az Azure terheléselosztó és távoli asztali (asztali) átjáró között. Most már hozhat létre egy távoli asztali átjáró farm egy egyetlen felhőszolgáltatásában.

Egy másik használati eset a médiafájlok feltöltés hol az adatok feltöltésének UDP keresztül történik, de a vezérlő sík TCP keresztül érhető el:

- Egy ügyfél először kezdeményez egy TCP-munkamenet terheléselosztása nyilvános címre, kap irányítja át egy adott DIP a csatorna is aktív Lync-kapcsolat állapota marad.
- Egy új UDP-munkamenetet az azonos által kezdeményezett az azonos terheléselosztása nyilvános végpontra ügyfélszámítógép, az elvárásoknak itt, hogy az azonos DIP végpontot is átirányításához a kapcsolat, mint az előző TCP-kapcsolatot, hogy media feltölteni futtatható a magas átviteli vezérlő csatorna TCP keresztül is megőrzésével.

>[AZURE.NOTE] A set terheléselosztás megváltozásakor (eltávolításával vagy egy virtuális gép hozzáadása), az ügyfél kérelmek eloszlás van recomputed. Új kapcsolatokat a meglévő ügyfelek ugyanarra a kiszolgálóra végződik nem függhet. Ezenkívül forrás IP-Címének terjesztési affinitású okozhat a forgalom az egyenlőtlenségi feltételen alapuló terjesztési. Ügyfelei proxy mögött egy egyedi ügyfélalkalmazás lehet tekinteni.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurálása forrás IP-Címének affinitás beállításainak terheléselosztó

A virtuális gépeken futó a PowerShell használatával időtúllépés beállításainak módosítása:

Zárólap Azure virtuális gép vehet fel, és betöltés terheléselosztó terjesztési módjának beállítása

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

A 2-sor bármelyik eleme (forrás IP-címe, cél IP) terheléselosztás, a 3-rekord (forrás IP-címe, cél IP, protocol) terheléselosztás és nincs sourceIPProtocol Ha azt szeretné, hogy az alapértelmezett működés 5-sor bármelyik eleme terheléselosztás sourceIP LoadBalancerDistribution beállíthatók.

A következő segítségével beolvashatja egy végpont betöltés terheléselosztó terjesztési üzemmód:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

Ha a LoadBalancerDistribution elem nem szerepel a Azure terheléselosztó az alapértelmezett 5-sor bármelyik eleme algoritmus használja.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Az eloszlás módja van beállítva egy terheléselosztása végpont beállítása

Ha végpontok részét terheléselosztása végpontot, a telepítési mód be kell állítani a terheléselosztása végpont megadása:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Felhőbeli szolgáltatások konfigurálására terjesztési módjának megváltoztatása

A .NET-2,5 (a novemberben kiadott) Azure SDK frissítése a felhőalapú szolgáltatás is élvezheti. Végpont beállításainak Cloud Services történnek a .csdef. Annak érdekében, hogy a betöltés terheléselosztó terjesztési mód Cloud Services-telepítés frissítése, a telepítési frissítése szükség.
Íme egy példa .csdef végpont beállítások:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
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

## <a name="api-example"></a>Példa API

A Szolgáltatáskezelés API segítségével terheléselosztási terheléselosztó is beállíthatja. Ügyeljen arra, hogy adja hozzá a `x-ms-version` fejléce verziójára van állítva `2014-09-01` vagy újabb verziójában.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Frissítse a beállításokat az adott terheléselosztás beállítás környezetben

#### <a name="request-example"></a>Példa kérése

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

A LoadBalancerDistribution értéke lehet sourceIP a 2-sor bármelyik eleme affinitás, a 3-sor bármelyik eleme affinitás sourceIPProtocol vagy a nincs (nincs affinitás. azaz az 5-rekord)

#### <a name="response"></a>Válasz

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Következő lépések

[Belső betöltés terheléselosztó – áttekintés](load-balancer-internal-overview.md)

[Első lépések az internetes terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
