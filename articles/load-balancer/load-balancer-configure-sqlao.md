<properties
   pageTitle="Tartományi terheléselosztó SQL mindig |} Microsoft Azure"
   description="SQL mindig dolgozhat a terheléselosztó, és hogyan hozhat létre SQL végrehajtásához terheléselosztó powershell kihasználhatja konfigurálása"
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

# <a name="configure-load-balancer-for-sql-always-on"></a>Tartományi terheléselosztó SQL mindig

Az SQL Server AlwaysOn rendelkezésre állási csoportok most ILB futtatható. Rendelkezésre állási csoport használata SQL Server flagship megoldás magas rendelkezésre állásának és katasztrófa helyreállítás. Az elérhetőség csoport figyelő ügyfélalkalmazások teszi lehetővé az elsődleges replika, függetlenül a konfigurációs kópiájába számát zökkenőmentes csatlakozni.

Figyelő (DNS) neve van hozzárendelve egy terheléselosztás IP-címet, és Azure-féle terheléselosztó a kópiakészlet arra utasítja az elsődleges kiszolgáló bejövő forgalmat.

Az SQL Server AlwaysOn (figyelő) végpontok ILB támogatás is használhatja. Most a kisegítő lehetőségek a figyelő hozzáférésük van, és IP-címe a terheléselosztás közül választhat egy adott alhálózat virtuális hálózata (VNet).

Figyelő, az SQL server-végpontot ILB alkalmazással (pl. Server = tcp:ListenerName, 1433; adatbázis = adatbázisnév) csak érhető el:

- Szolgáltatások és VMs virtuális ugyanabba a hálózatba
- Szolgáltatások és VMs csatlakoztatott helyszíni hálózatról
- Szolgáltatások és az összekapcsolt VNets VMs

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Ábra: 1 – internetes terheléselosztó konfigurált SQL AlwaysOn

## <a name="add-internal-load-balancer-to-the-service"></a>A szolgáltatás belső terheléselosztó hozzáadása

1. A következő példa azt egy virtuális hálózat, amely tartalmazza az "Alhálózat-1" nevű alhálózat fog konfigurálni:

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Adja meg a minden virtuális ILB terheléselosztása végpontok

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    A fenti példában van 2 virtuális úgynevezett "sqlsvc1" és "sqlsvc2" futtatása a felhőben szolgáltatás "Sqlsvc". Miután létrehozta a ILB "DirectServerReturn" kapcsolóval, adja hozzá terheléselosztása végpontok konfigurálása a hallgatók a elérhetősége csoportok SQL engedélyezése a ILB.

Az SQL-AlwaysOn kapcsolatos további tudnivalókért lásd: a [Configure egy belső terheléselosztó egy AlwaysOn elérhetősége csoport Azure-ban](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Lásd még:

[Első lépések az internetes terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
