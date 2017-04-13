<properties
   pageTitle="Rendelkezésre állási csoport hallgatók – Microsoft Azure mindig konfigurálása"
   description="Elérhetőség csoport hallgatók az erőforrás-kezelő Azure modellre, egy belső terheléselosztó használata egy vagy több IP-címek konfigurálása"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Egy vagy több mindig a elérhetősége csoport hallgatók - erőforrás-kezelő konfigurálása 

Ez a témakör bemutatja, hogy miként két dolgot kell tennie:

- Hozzon létre egy belső terheléselosztó az SQL Server-elérhetősége csoportok PowerShell-parancsmagok használata.

- További IP-címek hozzáadása egy terheléselosztó több SQL Server rendelkezésre állási csoport támogatása. 

Az SQL Server-elérhetősége csoport figyelő egy virtuális hálózati nevet, amely az ügyfélgépek hogyan kapcsolódnak az elsődleges és másodlagos egy adatbázisban elérése érdekében. Azure virtuális gépeken egy terheléselosztó a értesülnie IP-címét tartalmazza. A terheléselosztó a forgalom átirányítása a vizsgálati port figyel az SQL Server példányát. A legtöbb esetben egy rendelkezésre állási csoport egy belső terheléselosztó használja. Azure belső terheléselosztó üzemeltetheti egy vagy több IP-címet. Minden IP-cím egy adott vizsgálati portot használja. A dokumentum megtudhatja, hogy miként hozhat létre egy új terheléselosztó és IP-címek hozzáadása egy meglévő terheléselosztó az SQL Server-elérhetőség csoportok PowerShell-lel. 

Az azt jelenti, hogy egy belső terheléselosztó több IP-címek hozzárendelése új Azure és csak az erőforrás-kezelő modell érhető el. A feladat telepíthető az erőforrás-kezelő modell Azure virtuális gépeken futó SQL Server elérhetősége csoport van szükség. Mindkét SQL Server virtuális gépeken futó elérhetősége mindazon kell tartoznia. A [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) automatikusan az elérhetőség csoport létrehozásához a Azure erőforrás-kezelő is használhatja. Ez a sablon automatikusan létrehozza a elérhetőség csoportban, a belső terheléselosztó is beleértve. Ha inkább, azt is megteheti [kézi megadása egy AlwaysOn rendelkezésre állási csoport](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Ez a témakör igényel, hogy az elérhetőség csoportok már vannak-e beállítva.  

Kapcsolódó témakörök tartalmaznak:

- [Azure virtuális (grafikus) AlwaysOn rendelkezésre állási csoportok beállítása](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [VNet-VNet kapcsolat beállítása erőforrás-kezelő Azure és a PowerShell használatával](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>A Windows tűzfal konfigurálása

Állítsa be az SQL Server-hozzáférés engedélyezése a Windows tűzfal. Szüksége lesz a tűzfalat, hogy engedélyezze a portokat használja-e az SQL Server-példányt, valamint a figyelő vizsgálati által használt port TCP-kapcsolatok konfigurálása. A részletes útmutatást a [konfigurálása a Windows tűzfal az adatbázis motor eléréséhez](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1)című témakört. Hozzon létre egy bejövő szabályt az SQL Server port és a vizsgálati port.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Példa parancsfájl: Hozzon létre egy belső terheléselosztó PowerShell

> [AZURE.NOTE] Ha létrehozott az elérhetőség csoport a [Microsoft sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) a betöltés, nem kell ezt a lépést. 

Az alábbi PowerShell parancsprogramot létrehoz egy belső terheléselosztó, a terheléselosztás szabályok konfigurálása és IP-címet a terheléselosztó állítja be. Futtassa a parancsfájlt, nyissa meg a Windows PowerShell ISE és a parancsprogram beillesztése a parancsprogram-ablakban. Használat `Login-AzureRMAccount` történő bejelentkezéshez PowerShell. Ha több Azure előfizetéssel rendelkezik, `Select-AzureRmSubscription ` beállítása az előfizetést. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Példa parancsfájl: IP-címének hozzáadása egy meglévő terheléselosztó szolgáltatást a PowerShell használatával

Egynél több rendelkezésre állási csoport használatához PowerShell-lel való további IP-címének hozzáadása egy meglévő terheléselosztó. Minden IP-cím saját terheléselosztási szabályt, a vizsgálati portokkal és az első olyan portot szükséges.

Az előtér port az alkalmazások használata az SQL Server-példányt port. IP-címek elérhetősége különböző csoportokhoz használhatja ugyanazt a előtér portot.

>[AZURE.NOTE] Az SQL Server rendelkezésre állási csoportok a minden IP-cím egy adott vizsgálati port használatát igényli. Például egy terheléselosztó több IP-cím vizsgálati portot 59999 használ, nincs más IP-címeket, hogy terheléselosztó vizsgálati port 59999 használható. 

- Korlátozások terheléselosztó információt talál **magánjellegű elülső befejezése per terheléselosztó IP** [Hálózati korlátozások - Azure erőforrás-kezelő](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits)csoportban.

- Elérhetősége csoport korlátai című témakörből [korlátozások (elérhetőség csoportok)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

A következő parancsfájl ad egy meglévő terheléselosztó új IP-címet. Frissítse a változók környezetben. A ILB a figyelő portot használja, a betöltés terheléselosztó előtér port. A port lehet az SQL Server figyel portot. Az alapértelmezett példány az SQL Server Ez a port 1433. Terheléselosztási szabály egy elérhetősége csoport a szövegen kívüli IP (visszatérési közvetlen kiszolgáló) igényel, ezért a hátsó utolsó port ugyanaz, mint az előtér-port.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Állítsa be a betöltés terheléselosztó IP-címet használja a fürthöz 

A következő lépésként a figyelő konfigurálása a fürt, valamint a figyelő online állapotba. Ehhez tegye a következőket: 

1. Az elérhetőség csoport figyelő a Feladatátvevőfürt létrehozása  

2. Jelenítse meg a figyelő online

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. a elérhetősége csoport figyelő létrehozása a Feladatátvevőfürt

Ebben a lépésben a Feladatátvevőfürt a Feladatátvevőfürt-kezelő hozzáadása egy ügyfél-hozzáférési pontot, és PowerShell használatával állítsa be az fürt erőforrás a vizsgálati port meghallgatásához. 

1. RDP segítségével a Azure virtuális gépen, amelyen az elsődleges replika csatlakozhat. 

2. Nyissa meg a Feladatátvevőfürt-kezelő.

3. Jelölje ki a **hálózatok** csomópontot, és jegyezze fel a fürt hálózat nevét. Ezt a nevet a `$ClusterNetworkName` változó a PowerShell-parancsprogramot.

4. Bontsa ki a csoport nevét, és kattintson a **szerepköröket**.

5. A **szerepkörök** ablakban kattintson a jobb gombbal a elérhetősége csoport nevét, és válassza a **Erőforrás hozzáadása** > **Ügyfél-hozzáférési ponthoz**.

6. A **név** mezőbe hozzon létre egy nevet a új figyelő, majd kattintson kétszer a **Tovább gombra** , és kattintson a **Befejezés gombra**. Nem hozza a figyelő vagy az erőforrás online ezen a ponton.
 
    Az új értesülnie neve nevet a hálózatnak alkalmazások által használt SQL Server elérhetősége csoportjának adatbázisokhoz való csatlakozáshoz.

7. Kattintson az **erőforrások** fülre, majd bontsa ki az imént létrehozott ügyfél-hozzáférési pont. Kattintson a jobb gombbal az IP-erőforráshoz, és válassza a Tulajdonságok parancsot. Jegyezze fel az IP-címet. Ez a név, a használandó a `$IPResourceName` változó a PowerShell-parancsprogramot.

8. **IP-cím** csoportban kattintson a **Statikus IP-címet** , majd állítsa be a statikus IP-cím ugyanazt a címet használta az Azure-portálra a betöltés terheléselosztó IP-cím beállításakor. 

9. Tiltsa le a NetBIOS a cím, és kattintson az **OK gombra**. Ha a megoldás több Azure VNets fogja át, ismételje meg ezt a lépést az egyes IP-erőforrásokhoz. 

10. Ellenőrizze az SQL Server rendelkezésre állási csoport erőforrás az IP-cím függ. Az erőforrás felülete kattintson jobb gombbal az **Egyéb erőforrások**csoportjában az **erőforrások** lap. 

11. Az elsődleges kópia jelenleg üzemeltető csomóponton nyissa meg a egy jogú PowerShell ISE, és illessze be az alábbi parancsok új parancsfájl. A **függőségeket** lapon kattintson a figyelő nevét.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Frissítse a változók, és futtassa az IP-cím és a port konfigurálása az új értesülnie PowerShell-parancsprogramot.

 >[AZURE.NOTE] Ha az SQL Server külön régióban, a PowerShell-parancsprogramot kétszer futtatásához szükséges. Az első alkalommal a fürt hálózat nevét, fürt IP erőforrás nevét, és a terheléselosztó IP-címe az első erőforráscsoport betöltéséhez. A második alkalommal, amikor a fürt hálózat nevét, fürt IP erőforrás neve és a terheléselosztó IP-címet a második erőforráscsoport betöltéséhez.

Ekkor a fürt megjelenik egy elérhetősége csoport figyelő erőforrás.

## <a name="2-bring-the-listener-online"></a>2. a figyelő online állapotba

Az elérhetőség csoport figyelő erőforrással beállítva áttelepítheti a figyelő online, hogy az alkalmazás elérhetősége csoportjában a figyelő az adatbázisok csatlakozhat.

1. Nyissa meg újra a Feladatátvevőfürt-kezelő. Bontsa ki a **szerepkörök** , és jelölje ki az elérhetőség csoportban. Az **erőforrások** lapon kattintson a jobb gombbal a figyelő nevét, és válassza a **Tulajdonságok parancsot**.

1. Kattintson a **függőségek** fülre. Több felsorolt erőforrások esetén ellenőrizze, hogy az IP-címek van-e és, függőségek. Kattintson az **OK gombra**.

1. Kattintson a jobb gombbal a figyelő nevét, majd kattintson **Az Online állapotba**.

1. Miután a figyelő online, az **erőforrások** fülre, kattintson a jobb gombbal az elérhetőség csoportban, és válassza a **Tulajdonságok parancsot**.

1. Hozzon létre egy függőség a figyelő név erőforrás (nem az IP-címhez erőforrások mezőben). Kattintson az **OK gombra**.

1. Indítsa el az SQL Server Management Studio, és csatlakoztassa az elsődleges replika.

1. Nyissa meg azt a **AlwaysOn magas elérhetőség** | **rendelkezésre állási csoportok** | **rendelkezésre állási csoport hallgatók**. 

1. Ekkor megjelennek a Feladatátvevőfürt-kezelő létrehozott figyelő nevét. Kattintson a jobb gombbal a figyelő nevét, és válassza a **Tulajdonságok parancsot**.

1. A **Port** mezőbe írja be a portszámot az elérhetőség csoport értesülnie a korábban használt $EndpointPort használatával (1433 volt az alapértelmezett), majd kattintson az **OK gombra**.

Most már az erőforrás-kezelő üzemmód futásának Azure virtuális gépeken futó SQL Server elérhetőség csoport. 

## <a name="test-the-connection-to-the-listener"></a>Tesztelje a kapcsolatot a figyelő

A kapcsolat tesztelése:

1. SQL Server virtuális ugyanabba a hálózatba, de nem rendelkezik a replika RDP. Ez lehet az egyéb SQL Server a fürt.

2. **Sqlcmd** segédprogrammal tesztelje a kapcsolatot. Például a következőt egy **sqlcmd** kapcsolatot létesít az elsődleges replika a Windows-hitelesítés figyelő keresztül:

    ```
    sqlmd -S <listenerName> -E
    ```

    Ha a figyelő használ-e olyan portot kívül az alapértelmezett port (1433), adja meg a portot a kapcsolati karakterlánc. A következő sqlcmd parancs például csatlakozik a port 1435 figyelő: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

A SQLCMD kapcsolat automatikusan kapcsolódik az SQL Server-példányt tartson bármelyik tárolja az elsődleges replika. 

>[AZURE.NOTE] Győződjön meg róla, hogy a megadott port nyissa meg a mindkét SQL Server tűzfalon. Mindkét kiszolgáló megkövetelése a olyan portot használt egy bejövő szabályt. [Hozzáadás vagy a tűzfalat szabály szerkesztése](http://technet.microsoft.com/library/cc753558.aspx) talál további információt. 

## <a name="guidelines-and-limitations"></a>Útmutatások és korlátai

Vegye figyelembe az alábbi útmutatásokat elérhetősége csoport figyelő használatával belső Azure-ban a terheléselosztó:

- Az egy belső terheléselosztó csak érheti el a figyelő a virtuális hálózaton belül.

## <a name="for-more-information"></a>További információ

További információ [az Azure virtuális manuálisan csoport konfigurálása mindig a elérhetősége](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>PowerShell-parancsmagok

Az alábbi PowerShell-parancsmag használatával hozzon létre egy belső terheléselosztó az Azure virtuális gépeken futó.

- `New-AzureRmLoadBalancer`létrehoz egy terheléselosztó. [Új-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) talál további információt. 

- `New-AzureRMLoadBalancerFrontendIpConfig`létrehoz egy olyan terheléselosztó előtér-IP-beállítása. [Új-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) talál további információt.

- `New-AzureRmLoadBalancerRuleConfig`létrehoz egy terheléselosztó szabály konfigurációt. [Új-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) talál további információt. 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`létrehoz egy kódmentes készlet konfigurációt egy terheléselosztó. [Új-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) talál további információt. 

- `New-AzureRmLoadBalancerProbeConfig`létrehoz egy terheléselosztó vizsgálati konfigurációt. [Új-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) talál további információt.

Ha egy terheléselosztó eltávolítása egy Azure erőforráscsoport, használja az `Remove-AzureRmLoadBalancer`. További tudnivalókért lásd: az [Eltávolítás-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).
