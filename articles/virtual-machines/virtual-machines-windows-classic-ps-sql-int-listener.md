<properties
    pageTitle="Egy ILB figyelő az elérhetőség csoportok mindig beállítása |} Microsoft Azure"
    description="Ebben az oktatóanyagban a klasszikus telepítési modell készült az erőforrásokat használó, és létrehoz egy mindig a elérhetősége csoport figyelő egy belső betöltés terheléselosztó (ILB) használatával Azure-ban."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Állítsa be a mindig a elérhetősége csoportok egy ILB figyelő Azure-ban

> [AZURE.SELECTOR]
- [Belső figyelő](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Külső figyelő](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan egy mindig a elérhetősége csoport figyelő konfigurálása egy **Belső betöltés terheléselosztó (ILB)**segítségével.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erőforrás-kezelő modell egy ILB figyelő egy mindig a elérhetősége csoport konfigurálásához a [konfigurálása egy belső terheléselosztó egy mindig a elérhetősége csoport Azure-ban](virtual-machines-windows-portal-sql-alwayson-int-listener.md)című témakört.


Az elérhetőség csoport kópiák, amelyek a helyszíni csak, csak az Azure tartalmazhatnak, illetve a helyszíni és Azure is kiterjedő hibrid konfigurációk. Azure kópiák találhatók a azonos régión belüli vagy a több régiókat több virtuális hálózatok (VNets). Az alábbi lépésekkel tegyük fel, már [konfigurált egy elérhetőség csoport](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , de nem állította be a figyelő.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Útmutatások és belső hallgatók korlátai
Megjegyzés: az elérhetőség csoport figyelő ILB használatával Azure-ban kattintson az alábbi útmutatásokat:

- Az elérhetőség csoport figyelő a Windows Server 2008 R2, a Windows Server 2012-ben és a Windows Server 2012 R2 támogatott.

- Csak egy belső elérhetősége csoport figyelő támogatott egy felhőalapú szolgáltatásba, mert a figyelő úgy van konfigurálva, hogy a ILB, és nem csak egy ILB egy felhőalapú szolgáltatásba. Azonban célszerű lehet létrehozni, több külső hallgatók. További tudnivalókért lásd: a [Configure egy külső figyelő mindig a elérhetőség csoportok Azure-ban](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Hozzon létre egy belső figyelő az azonos felhőalapú szolgáltatás, ahol is telepítve van egy külső figyelő nyilvános virtuális a felhőalapú szolgáltatást használ a nem támogatott.

## <a name="determine-the-accessibility-of-the-listener"></a>A kisegítő lehetőségek a figyelő meghatározása

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Ez a cikk koncentrál egy **Belső betöltés terheléselosztó (ILB)**használó figyelő létrehozása. Ha szüksége van egy nyilvános és külső figyelő, a jelen cikk, amely egy [külső figyelő](virtual-machines-windows-classic-ps-sql-ext-listener.md) beállításának lépéseit verziója jelenik meg

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Hoz létre virtuális végpontok terheléselosztás kiszolgálóval közvetlen visszatérési

A ILB először létre kell hoznia a belső terheléselosztó. Ez a parancsfájl, az alábbi történik.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. **ILB**meg kell rendelje hozzá a statikus IP-címet. Első lépésként vizsgálja meg a jelenlegi VNet konfigurációt a következő parancs futtatásával:

        (Get-AzureVNetConfig).XMLConfiguration

1. Jegyezze fel a alhálózat, amely tartalmazza a VMs, amely a replikák szolgáltató **alhálózat** nevét. Ez a parancsfájl **$SubnetName** paraméterben lesz.

1. Jegyezze fel a **VirtualNetworkSite** neve és a kezdő **AddressPrefix** , amely tartalmazza a VMs, amely a replikák szolgáltató alhálózat. Keresse meg a rendelkezésre álló IP-címének átadása mindkét értéket a **Próba-AzureStaticVNetIP** parancsot, és a **AvailableAddresses**vizsgálata. Például ha a VNet *MyVNet* nevű és a volna egy alhálózat címet, amely a lépések az *172.16.0.128*tartományt, és a következő parancs lenne a rendelkezésre álló címeit felsoroló:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Válasszon egyet a rendelkezésre álló címeket, és használja azt a következő parancsfájl **$ILBStaticIP** paramétere.

3. Az alábbi PowerShell-parancsprogramot másol egy egyszerű szövegszerkesztő programban, és a változók értékein, hogy megfeleljen a környezet (Megjegyzés: egyes paraméterek alapértelmezett kapott) beállítása. Figyelje meg, hogy a meglévő telepítések affinitás csoportok használó nem vehet fel a ILB. ILB követelményeiről további tudnivalókért olvassa el a [Belső terheléselosztó](../load-balancer/load-balancer-internal-overview.md)című témakört. Is ha az elérhetőség csoport átnyúlik Azure régiók, futtatnia kell a parancsfájl egyszer minden adatközpontban a felhőalapú szolgáltatás és az adott adatközponthoz tartományban található csomópontok.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. A változók már megadta, másolja a parancsfájl kattintással indítsa el az Azure PowerShell-munkamenetet a szövegszerkesztőben. Ha továbbra is láthatók a kérdés >>, írja be ismét az ENTER ellenőrizze, hogy a parancsprogram kezdődik. Megjegyzés:

>[AZURE.NOTE] Az Azure klasszikus portálon nem támogatja a belső terheléselosztó most, így nem jelenik meg a ILB vagy az Azure klasszikus portálon a végpontok. Azonban **Get-AzureEndpoint** adja vissza belső IP-címet, ha a terheléselosztó fut. rajta. Ellenkező esetben az eredmény üres.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Ellenőrizze, hogy KB2854082 szükség van-e telepítve

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Nyissa meg a tűzfalportokat elérhetősége csoport csomópontjai

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Az elérhetőség csoport figyelő létrehozása

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. ILB az IP-cím, a belső betöltés terheléselosztó (ILB) korábban létrehozott kell használnia. A következő parancsfájl használatával szerezze be a PowerShell IP-címet.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. A VMs egyik másolja be a PowerShell parancsprogramot az operációs rendszerének egy egyszerű szövegszerkesztő programban, és jelölje be a változók a korábban feljegyzett értéket.

    A Windows Server 2012 vagy újabb használja a következőt:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    A Windows Server 2008 R2 használja a következőt:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Egyszer, a változók állította, emelt a Windows PowerShell-ablak megnyitása, majd másolja a parancsfájl szövegszerkesztőben és illessze be kattintással indítsa el az Azure PowerShell-munkamenetet. Ha továbbra is láthatók a kérdés >>, írja be ismét az ENTER ellenőrizze, hogy a parancsprogram kezdődik.

2. Ismételje meg ezt a minden virtuális. A parancsfájl az IP-cím erőforrás konfigurálja az IP-címet a felhőalapú szolgáltatás, és további paramétereket állítja be, például a vizsgálati port. Amikor az IP-cím erőforrás online állapotba kerül, akkor is majd válaszolhat a vizsgálati port a lekérdezési a az oktatóprogram korábbi részében létrehozott terheléselosztás végpontot.

## <a name="bring-the-listener-online"></a>Jelenítse meg a figyelő online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Elemek nyomon követése

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Az elérhetőség csoport figyelő (belül az azonos VNet) teszt

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
