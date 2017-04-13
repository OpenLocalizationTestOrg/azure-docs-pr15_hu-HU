<properties
   pageTitle="Első lépések megtételében szemben lévő terheléselosztó klasszikus telepítési modell a felhőbeli szolgáltatásokhoz internetes |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó felhőszolgáltatások klasszikus telepítési modellje"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Első lépések megtételében egy internetes terheléselosztó cloud Services

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó Azure erőforrás-kezelő használatával](load-balancer-get-started-internet-arm-cli.md).

Felhőszolgáltatások egy terheléselosztó vannak automatikusan beállítva, és a testre szabható a modell keresztül.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Hozzon létre egy terheléselosztó szolgáltatás definíciós fájl használatával

A .NET-2,5 frissítése a felhőalapú szolgáltatás Azure SDK is élvezheti. Végpont beállításainak cloud services [szolgáltatás definíciós](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef fájlban történik.

A következő példa bemutatja, hogyan van konfigurálva a felhőben telepítés servicedefinition.csdef fájl:

-Ellenőrzést a snipet az .csdef fájl egy felhőalapú környezetben által generált megjelenik portok HTTP 10000 10001 és 10002 portot használja a külső végpontot.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Jelölje be a felhőbeli szolgáltatásokhoz terheléselosztó állapot állapotához


Az alábbi képen egy állapot vizsgálati:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

A terheléselosztó egyesíti a végpont információkat, és az információkat a vizsgálati hozhat létre egy URL-címet, a szolgáltatás állapotát jelző lekérdezésére használható VM}:80/Probe.aspx http://{DIP formájában.

A szolgáltatás észleli, periodikus szondákat az ugyanazt a címet. Ez az Ön a csomópontra a érkező, amelyen a virtuális gépen fut, a állapot vizsgálati kérelmet.
A szolgáltatás a terheléselosztó átvéve, hogy a szolgáltatás nem megfelelő HTTP 200 állapotkódot válaszoljon rendelkezik. Bármely más HTTP állapotkód (például 503) közvetlenül a Forgatás ki a virtuális gép tart.

A vizsgálati definíció vezérli a vizsgálati gyakoriságának is. A fenti esetben a terheléselosztó van számlálása minden 5 másodperc végpontot. Nem érkezett válasz pozitív 10 másodperc (két vizsgálati intervallumok), ha a vizsgálati lefelé tekinti, és a virtuális gép ki a Forgatás származik. Hasonlóképpen ha a szolgáltatás nem elforgatás és pozitív válasz érkezik, a szolgáltatás van váltson vissza Forgatás azonnal. A szolgáltatás megfelelő és sérült között van hullámzó, ha a terheléselosztó is szeretné késleltetése a szolgáltatás Forgatás újratelepítést, amíg a megfelelő számú szondákat már.

Jelölje be a szolgáltatás definition séma a [rendszerállapot vizsgálati](https://msdn.microsoft.com/library/azure/jj151530.aspx) további információt.

## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

