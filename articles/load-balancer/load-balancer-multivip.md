<properties
   pageTitle="Mutiple VIP egy felhőalapú szolgáltatás"
   description="MultiVIP és több VIP beállítása egy felhőalapú szolgáltatásba – áttekintés"
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

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Több VIP egy felhőalapú szolgáltatás konfigurálása

A nyilvános interneten Azure felhőszolgáltatások hozzáférhet Azure által biztosított IP-cím használatával. A nyilvános IP-cím hivatkozik, amely egy virtuális (ezek olyan virtuális IP), mivel az Azure terheléselosztó kapcsolódik, és a felhőbeli szolgáltatástól belül példányok virtuális gép (virtuális). Egy egyetlen virtuális segítségével virtuális példányokban belül egy felhőalapú szolgáltatásba is elérheti.

Vannak azonban olyan esetek, amelyekben szükség lehet egynél több virtuális bejegyzésként mutasson a ugyanazt a felhőalapú szolgáltatást. A felhőalapú szolgáltatásba, például is tartalmazhat több minden hely egy másik vevő üzemelteti SSL-kapcsolat, 443, az alapértelmezett portja a segítségével szükség van, vagy bérlői webhelyek. Ebben az esetben egy másik nyilvános szemben lévő IP-cím minden webhelyhez van szükség. Az alábbi ábrán egy tipikus több bérlői webes szolgáltatója, az azonos nyilvános port több SSL-tanúsítványok szükség mutatja be.

![Több virtuális SSL eset](./media/load-balancer-multivip/Figure1.png)

A fenti példában összes VIP használhatja ugyanazt nyilvános portot (443-as) és forgalmat a rendszerünk átirányítja egy vagy több terheléselosztás VMs a felhőbeli szolgáltatástól, az összes webhelyeket tároló belső IP-címét az egyedi magánjellegű port.

>[AZURE.NOTE] Egy másik helyzet több VIP használatát igénylő több SQL AlwaysOn elérhetősége csoport hallgatók ugyanazok a virtuális gépeken futó a Technical.

VIP dinamikus, ami azt jelenti, hogy a tényleges IP-cím, a felhőbeli szolgáltatástól rendelt idő után megváltoznak alapértelmezés szerint. Megakadályozná, hogy egy virtuális foglalhat a szolgáltatáshoz. Fenntartott VIP kapcsolatos további tudnivalókért lásd: a [Fenntartott nyilvános IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Kérjük, [IP-cím árak](https://azure.microsoft.com/pricing/details/ip-addresses/) további információkat talál a VIP árak és fenntartott IP-címei.

Is a felhőalapú szolgáltatások által használt VIP ellenőrizze a PowerShell használatával, valamint hozzáadása és eltávolítása VIP, egy virtuális zárólap szeretne társítani és a egy adott virtuális terheléselosztási konfigurálása.

## <a name="limitations"></a>Korlátozások

Ekkor több virtuális funkció csak az alábbi esetekben:

- **Csak a IaaS**. Több virtuális csak engedélyezése VMs tartalmazó felhőszolgáltatások. A szerepkör-példányok PaaS esetek több virtuális nem használhatók.
- **Csak a PowerShell**. Több virtuális csak a PowerShell használatával kezelheti.

Ezek a korlátozások ideiglenes, és előfordulhat, hogy bármikor módosíthatja. Ellenőrizze, hogy a lapon a jövőbeli módosítások ellenőrzéséhez látogatnia.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>A virtuális hozzáadása egy felhőalapú szolgáltatásba

Adja hozzá egy virtuális a szolgáltatásban, futtassa az alábbi PowerShell-parancsot:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Ez a parancs az alábbi példa hasonló eredményt jeleníti meg:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>A virtuális eltávolítása egy felhőalapú szolgáltatásba

Ha el szeretné távolítani a virtuális a szolgáltatásban, a fenti példában hozzáadni, futtassa az alábbi PowerShell-parancsot:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] Ha rendelkezik a hozzá tartozó végpontok, csak egy virtuális eltávolíthatja.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Hogyan virtuális adatok kinyerése egy felhőalapú szolgáltatásba

Egy felhőalapú szolgáltatásba társított VIP beolvasásához, futtassa az alábbi PowerShell parancsprogramot:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

A parancsprogram hasonlóan, mint az alábbi példa eredménye jeleníti meg:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

Ebben a példában a felhőalapú szolgáltatást tartalmaz a 3-as VIP:

- **Vip1** az alapértelmezett virtuális, tudható, mert a IsDnsProgrammedName értéke igaz.
- **Vip2** és **Vip3** nem használják nem rendelkeznek a bármely IP-címet. Csak lesz, ha a virtuális található zárólap társít.

>[AZURE.NOTE] Az előfizetés csak ráterheljük további VIP társítva zárólap után. Árak kapcsolatos további tudnivalókért olvassa el az [IP-cím árak](https://azure.microsoft.com/pricing/details/ip-addresses/)című témakört.

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Hogy miként társítható egy virtuális egy végpontra

Társítani egy virtuális kattintson egy végpontra egy felhőalapú szolgáltatásba, futtassa az alábbi PowerShell-parancsot:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

A parancs a virtuális *Vip2* nevű *80-as*port és a hivatkozásokat, hogy a virtuális nevű *myVM1* a egy felhőalapú szolgáltatásba, *TCP* használata port *8080* *myService* nevű kapcsolódó zárólap hoz létre.

Ellenőrizze a konfigurációt, futtassa az alábbi PowerShell-parancsot:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

A kimeneti formátumban a következőhöz hasonló:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Egy adott virtuális a terheléselosztás engedélyezése

Egy egyetlen virtuális társíthat több virtuális gépeken futó terheléselosztási céljából. Ha például ha egy felhőalapú szolgáltatásba, *myService*nevű, és két virtuális gépeken futó *myVM1* és *myVM2*nevű. És az a felhő szolgáltatásban több VIP, az egyik *Vip2*nevű. Ha szeretné biztosítani, hogy minden forgalom port *81* *Vip2* a kiegyensúlyozott *myVM1* és *myVM2* a port *8181*, futtassa az alábbi PowerShell parancsprogramot között:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Frissítheti a használatára, egy másik virtuális terheléselosztó is. Ha az alábbi PowerShell-parancsot, például fog a terheléselosztási használni egy virtuális Vip1 nevű beállítása módosítása:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Következő lépések

[Azure terheléselosztási napló elemzésének](load-balancer-monitor-log.md)

[Internetes szemben lévő betöltés terheléselosztó – áttekintés](load-balancer-internet-overview.md)

[Internetes terheléselosztó első lépések](load-balancer-get-started-internet-arm-ps.md)

[Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)

[Fenntartott IP REST API-hoz](https://msdn.microsoft.com/library/azure/dn722420.aspx)