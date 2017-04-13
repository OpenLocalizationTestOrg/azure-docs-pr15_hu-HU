<properties
   pageTitle="Hozzon létre egy belső terheléselosztó cloud Services a klasszikus telepítési modell |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy belső terheléselosztó a klasszikus telepítési modell PowerShell használatával"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Első lépések megtételében (klasszikus) egy belső terheléselosztó cloud Services

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Belső terheléselosztó cloud Services konfigurálása

Belső terheléselosztó virtuális gépeken futó és a felhőbeli szolgáltatások használata támogatott. Belső betöltés terheléselosztó zárólap létrehozott egy felhőalapú szolgáltatásba, amely egy virtuális területi hálózatán kívülről csak a felhőbeli szolgáltatástól belül elérhető lesz.

A belső betöltés terheléselosztó beállítások alkalmazása a felhőbeli szolgáltatástól, az első bevezetésének kibocsátása során be kell állítani a minta alább látható módon.

>[AZURE.IMPORTANT] Az alábbi lépésekkel futtatásához a előfeltétel, hogy az a felhő példányban már létrehozott egy virtuális hálózaton. Szüksége lesz a virtuális hálózati és hozhat létre, a belső terheléselosztás alhálózat nevét.

### <a name="step-1"></a>Lépés: 1

Nyissa meg a szolgáltatás konfigurációs fájl (.cscfg) a felhőben telepítéshez a Visual Studióban, és adja hozzá a következő szakasz létrehozása a belső terheléselosztás csoportban az utolsó "`</Role>`" a hálózati konfigurálásról elemet.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Helyezzen el a hálózati konfigurációs fájl hogyan jelenik meg az értékeket. A példában szereplő tegyük fel, "test_vnet" nevű egy alhálózat 10.0.0.0/24 test_subnet és statikus IP-cím 10.0.0.4 című alhálózat hozott létre. A terheléselosztó testLB neve.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

A betöltés terheléselosztó séma kapcsolatos további tudnivalókért olvassa el a [terheléselosztó hozzáadása](https://msdn.microsoft.com/library/azure/dn722411.aspx)című témakört.

### <a name="step-2"></a>Lépés: 2


Módosítsa a végpontok hozzáadása a belső terheléselosztás definition (.csdef)-fájlt. A pillanattól fogva egy szerepkör-példány jön létre, a szolgáltatás-definíciós fájl hozzáadja a szerepkör a belső terheléselosztás szeretne.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Követően az értékeket a fenti példában helyezzen el az értékeket a szolgáltatás-definíciós fájlt.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

A hálózati forgalmának engedélyezésére fog osztható a 80-as port használatának bejövő felkérést dolgozó szerepkör példányok is a 80-as port küldene testLB terheléselosztó használatával.


## <a name="next-steps"></a>Következő lépések

[Betöltési terheléselosztó terjesztési mód forrás IP affinitás konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)