<properties
   pageTitle="Hozzon létre egy belső terheléselosztó PowerShell használata a klasszikus telepítési modell |} Microsoft Azure"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Első lépések megtételében PowerShell használatával belső terheléselosztó (klasszikus)

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Hozzon létre egy belső terheléselosztó virtuális gépeken futó beállítása

Egy belső betöltés terheléselosztó beállítása és a kiszolgáló küld a forgalmat, létrehozásához tegye a következőket van:

1. Hozzon létre egy példányának belső terheléselosztás, amely a bejövő forgalom betöltése a kiszolgálóin terheléselosztás meg kell végpont lesz.

1. Adja meg, hogy fog kapni a bejövő forgalom virtuális gépeken futó megfelelő végpontok.

1. Állítsa be a kiszolgáló, amely a forgalmat a forgalom küldésére a belső terheléselosztás példány virtuális IP (virtuális) címének osztható küldi.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Lépés: 1: Hozzon létre egy belső terheléselosztás példány

Egy meglévő felhőalapú szolgáltatásba vagy egy felhőalapú szolgáltatásba, a területi virtuális hálózat rendszerbe létrehozhat egy belső terheléselosztás példány a következő Windows PowerShell-parancsokkal:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Ne feledje, hogy a [Hozzáadás-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-parancsmag használata DefaultProbe paraméter megadása használja-e. Újabb paraméterrel készletek kapcsolatos további tudnivalókért olvassa el a [Hozzáadás-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx)című témakört.

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Lépés: 2: A belső terheléselosztás példány végpontok hozzáadása

Lássunk egy példát:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>3 lépés: A kiszolgálókat, hogy a forgalmat küldeni az új belső terheléselosztás végpontot konfigurálása

Állítsa be a kiszolgálókat, amelyek a forgalom fog használni az új IP-címet (a virtuális) a belső terheléselosztás példány osztható van. Ez az a cím, a belső terheléselosztás példány figyel. A legtöbb esetben kell csak hozzáadása vagy módosítása a DNS-rekordot a virtuális belső terheléselosztás példány.

A belső terheléselosztás példány kibocsátása során megadott IP-címét, ha már rendelkezik a virtuális. Egyéb esetben megjelenik a virtuális az alábbi parancsok állnak rendelkezésre:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Ezek a parancsok használata, töltse ki az értékeket, és távolítsa el a < és >. Lássunk egy példát:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


A Get-AzureInternalLoadBalancer parancs a képernyőről vegye figyelembe az IP-címet, és végezze el a szükséges módosításokat a kiszolgálón vagy a DNS-rekordjait, és győződjön meg arról, hogy forgalom elküldi, amint a virtuális.

>[AZURE.NOTE]A Microsoft Azure platform felügyeleti esetek számos egy statikus, nyilvánosan átirányítható IPv4-címet használja. Az IP-cím 168.63.129.16. Az IP-cím nem kell letiltotta bármely a tűzfalak által, mert szokatlan viselkedést tapasztal okozhat.
>Részletez Azure belső terheléselosztás IP-cím használja a terheléselosztó a szondákat figyelése határozza meg a rendszerállapot a virtuális gépeken futó terheléselosztása meg. Ha hálózati biztonsági csoport forgalmának korlátozása egy belső terheléselosztás beállítása Azure virtuális gépeken futó használja, vagy egy virtuális alhálózathoz alkalmazott, győződjön meg arról, hogy a hálózati biztonsági szabály szerepel-e 168.63.129.16 forgalmának engedélyezésére.


## <a name="example-of-internal-load-balancing"></a>Példa a belső terheléselosztás

Álljon meg a vége a folyamat leállítása a két példa konfigurációk terheléselosztás készlet létrehozása, tanulmányozza az alábbi szakaszok.

### <a name="an-internet-facing-multi-tier-application"></a>Szemben lévő több szálon alkalmazás internetes

Szeretne megadni egy sor olyan internetes webkiszolgálón terheléselosztása adatbázis szolgáltatást. Kiszolgálók mindkét Azure felhőszolgáltatásában van közzétéve. TCP-port 1433 webes kiszolgáló-alapú forgalmat kell osztják két virtuális gépeken futó az adatbázis réteg között. Ábra 1 mutatja be.

![Az adatbázis réteg belső terheléselosztás beállítása](./media/load-balancer-internal-getstarted/IC736321.png)


A konfiguráció a következőkből áll:

- A meglévő felhőalapú szolgáltatást a virtuális gépeken futó szolgáltatója mytestcloud neve.

- A két meglévő adatbázis-kiszolgáló neve D1, DB2.

- A webes réteg webkiszolgálón csatlakozni az adatbázis-kiszolgálók, az adatbázis réteg a magánjellegű IP-cím használatával. Egy másik, hogy a virtuális-hálózathoz használni a saját DNS- és egy belső betöltés terheléselosztó beállítása egy rekord manuálisan regisztrálni.

Az alábbi parancsok **ILBset** nevű új belső terheléselosztás példány beállítása, és a megfelelő a két adatbázis-kiszolgálók virtuális gépeken futó végpontok hozzáadása:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Távolítsa el egy belső terheléselosztás konfigurálása

Zárólap virtuális gép eltávolítása egy belső betöltés terheléselosztó példányával, az alábbi parancsokat használhatja:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Ezek a parancsok használata, töltse ki az értékeket, és eltávolítása a < és >.

Lássunk egy példát:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Ha el szeretne távolítani egy belső betöltés terheléselosztó példány egy felhőalapú szolgáltatásba, használja az alábbi parancsokat:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Ezek a parancsok használata, adja a értéket, és távolítsa el a < és >.

Lássunk egy példát:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>További információ a belső betöltés terheléselosztó parancsmagok


További információ a belső terheléselosztás parancsmagok juthat, a Windows PowerShell-parancssorában futtassa az alábbi parancsokat:

- Segítség új AzureInternalLoadBalancerConfig-teljes

- Segítség hozzáadása AzureInternalLoadBalancer-teljes

- Get-help Get-AzureInternalLoadbalancer-teljes

- Segítség eltávolítása-AzureInternalLoadBalancer-teljes

## <a name="next-steps"></a>Következő lépések

[Betöltési terheléselosztó terjesztési mód forrás IP affinitás konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)