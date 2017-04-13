<properties
    pageTitle="Mindig a elérhetőség csoportok egy külső figyelő beállítása |} Microsoft Azure"
    description="Ebben az oktatóanyagban végigvezeti az mindig a elérhetősége csoport figyelő létrehozására, amely a külső felekkel hozzáférhető a nyilvános virtuális IP-cím, a társított felhőalapú szolgáltatás használatával Azure-ban."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Állítsa be a mindig a elérhetősége csoportok egy külső figyelő Azure-ban

> [AZURE.SELECTOR]
- [Belső figyelő](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Külső figyelő](virtual-machines-windows-classic-ps-sql-ext-listener.md)

Ez a témakör bemutatja, hogyan egy mindig a elérhetősége csoport külső felekkel elérhető az interneten figyelő konfigurálása. Ez történik lehetséges a felhőbeli szolgáltatástól **Nyilvános virtuális IP (virtuális)** cím társítása a figyelő.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Az elérhetőség csoport kópiák, amelyek a helyszíni csak, csak az Azure tartalmazhatnak, illetve a helyszíni és Azure is kiterjedő hibrid konfigurációk. Azure kópiák találhatók a azonos régión belüli vagy a több régiókat több virtuális hálózatok (VNets). Az alábbi lépésekkel tegyük fel, már [konfigurált egy elérhetőség csoport](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , de nem állította be a figyelő.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Útmutatások és a külső hallgatók korlátai

Telepítésekor a felhőalapú szolgáltatás nemi segítség virtuális cím használatával, vegye figyelembe az alábbi útmutatásokat kapcsolatban az elérhetőség csoport figyelő Azure-ban:

- Az elérhetőség csoport figyelő a Windows Server 2008 R2, a Windows Server 2012-ben és a Windows Server 2012 R2 támogatott.

- Az ügyfélalkalmazás egy másik felhőalapú szolgáltatásba, mint az elérhetőség csoport VMs tartalmazó kell lennie. Azure nem támogatja az ügyfél- és kiszolgálóoldali közvetlen kiszolgáló visszatérési ugyanazt a felhőalapú szolgáltatást.

- Alapértelmezés szerint a jelen cikkben ismertetett lépések bemutatják, hogyan konfigurálása értesülnie egy felhőalapú szolgáltatás virtuális IP (virtuális) címet. Azonban érdemes lefoglalása és több virtuális címet a felhőalapú szolgáltatás hozzon létre. Lehetővé teszi, hogy a cikkben ismertetett lépésekkel hozhatja létre több hallgatók társított összes egy másik virtuális létrehozásához. Több virtuális címre létrehozásával kapcsolatos további tudnivalókért olvassa el a [Több VIP egy felhőalapú szolgáltatásba](../load-balancer/load-balancer-multivip.md)című témakört.

- Hibrid környezet figyelő hoz létre, ha a helyszíni hálózaton rendelkeznie kell a nyilvános internetkapcsolat a webhely VPN az Azure virtuális hálózattal mellett. Az Azure alhálózat a az elérhetőség csoport figyelő csak a nyilvános IP-címét a megfelelő felhőbeli szolgáltatástól megközelíthető.

- Hozzon létre egy külső figyelő a azonos felhőalapú szolgáltatást, ahová is telepítve van egy belső figyelő a belső betöltés terheléselosztó (ILB) használata nem támogatott.

## <a name="determine-the-accessibility-of-the-listener"></a>A kisegítő lehetőségek a figyelő meghatározása

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Ez a cikk koncentrál létrehozása **külső terheléselosztási**használó figyelő. Ha azt szeretné, hogy a virtuális hálózathoz privát figyelő, az ebben a cikkben egy [ILB rendelkező figyelő](virtual-machines-windows-classic-ps-sql-int-listener.md) beállításának lépéseit verziója jelenik meg

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Hoz létre virtuális végpontok terheléselosztás kiszolgálóval közvetlen visszatérési

Külső terheléselosztás használ a virtuális a felhőbeli szolgáltatástól, amelyen a VMs nyilvános virtuális IP-címét. Így nem kell létrehozni vagy a terheléselosztó ebben az esetben konfigurálása.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Az alábbi PowerShell-parancsprogramot másol egy egyszerű szövegszerkesztő programban, és állítsa a változók értékein a (alapértelmezett kapott egyes paraméterek) környezetnek megfelelően. Figyelje meg, hogy ha az elérhetőség csoport átnyúlik Azure régiók, futtatnia kell a parancsfájl egyszer a felhőalapú szolgáltatás és az adott adatközponthoz tartományban található csomópontok minden adatközpontban.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. A változók már megadta, másolja a parancsfájl kattintással indítsa el az Azure PowerShell-munkamenetet a szövegszerkesztőben. Ha továbbra is láthatók a kérdés >>, írja be ismét az ENTER ellenőrizze, hogy a parancsprogram kezdődik.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Ellenőrizze, hogy KB2854082 szükség van-e telepítve

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Nyissa meg a tűzfalportokat elérhetősége csoport csomópontjai

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Az elérhetőség csoport figyelő létrehozása

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. A külső terheléselosztás, be kell szereznie a felhőbeli szolgáltatástól, amely tartalmazza a kópiák nyilvános virtuális IP-címét. Jelentkezzen be az Azure klasszikus portálon. Nyissa meg a felhőbeli szolgáltatástól, amely tartalmazza az elérhetőség csoport virtuális. Az **Irányítópult** nézetének megnyitása.

3. Megjegyzés: a **Nyilvános virtuális IP (virtuális) címet**a cím. Ha a megoldás VNets átnyúlik, ismételje meg ezt a lépést minden felhőalapú szolgáltatás, amely egy virtuális kópia üzemeltető tartalmazza.

4. A VMs egyik egy egyszerű szövegszerkesztő programban másolja be az alábbi PowerShell-parancsprogramot, és jelölje be a változók a korábban feljegyzett értéket.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Egyszer, a változók állította, emelt a Windows PowerShell-ablak megnyitása, majd másolja a parancsfájl szövegszerkesztőben és illessze be kattintással indítsa el az Azure PowerShell-munkamenetet. Ha továbbra is láthatók a kérdés >>, írja be ismét az ENTER ellenőrizze, hogy a parancsprogram kezdődik.

1. Ismételje meg ezt a minden virtuális. A parancsfájl az IP-cím erőforrás konfigurálja az IP-címet a felhőalapú szolgáltatás, és további paramétereket állítja be, például a vizsgálati port. Amikor az IP-cím erőforrás online állapotba kerül, akkor is majd válaszolhat a vizsgálati port a lekérdezési a az oktatóprogram korábbi részében létrehozott terheléselosztás végpontot.

## <a name="bring-the-listener-online"></a>Jelenítse meg a figyelő online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Elemek nyomon követése

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Az elérhetőség csoport figyelő (belül az azonos VNet) teszt

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Az elérhetőség csoport figyelő vizsgálata (az interneten)

A virtuális hálózatán kívülről érkező a figyelő eléréséhez kell használnia terheléselosztás külső/nyilvános (ebben a témakörben ismertetett) ILB, nem pedig annak vagyis csak hozzáférhető az azonos VNet belül. A kapcsolati karakterláncban adja meg a felhőben szolgáltatás neve. Ha például ha egy felhőalapú szolgáltatásba, és a név *mycloudservice*, a sqlcmd utasítás a következő lesz az alábbi képlettel történik:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Az előző példában eltérően SQL-hitelesítés kell használni, mivel a hívó az interneten keresztül nem használható windows-hitelesítés. További tudnivalókért lásd: [mindig a elérhetősége csoport az Azure virtuális: ügyfél kapcsolódási esetek](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). SQL-hitelesítést használ, ügyeljen az példányon mindkét hoz létre az azonos bejelentkezés. Elérhetőség csoportokkal bejelentkezések hibaelhárítási kapcsolatos további tudnivalókért olvassa el a [bejelentkezések megfeleltetése és használni szeretne csatlakozni a többi, és elérhetőség adatbázisok adatbázis-felhasználó SQL foglalt](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx)című témakört.

Ügyfelek kell adnia a mindig a replikák esetén a különböző alhálózathoz, **MultisubnetFailover = True** kapcsolati karakterláncban. Ennek eredményeképpen a párhuzamos kapcsolatfelvételi a különböző alhálózathoz kópiájába. Megjegyzés: Ebben az esetben tartalmaz egy keresztté-régió mindig a rendelkezésre állási csoport telepítés.

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
