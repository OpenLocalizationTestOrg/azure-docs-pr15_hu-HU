<properties
   pageTitle="Szolgáltatás háló csomópont típus és virtuális skála beállítása |} Microsoft Azure"
   description="Hogyan szolgáltatás háló csomópont típus virtuális skála készletek kapcsolódnak, és hogyan távoli csatlakoznia kell egy virtuális skála megadása példány vagy csomópont ismerteti."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Szolgáltatás háló csomópont típus és a virtuális gép skála készletek közötti kapcsolat

Virtuális gép skála készletei olyan az Azure kiszámítania erőforrás üzembe helyezéséhez és kezeléséhez tárolt virtuális gépeken futó gyűjteménye is használhatja. Minden definiált szolgáltatás háló fürt csomópont típusa egy külön virtuális skála megadása van állítva. Csomópont típusonként majd kell méretezett vagy lefelé független, különböző csoportjaihoz nyitott portokat van, és beállíthatja, hogy a különböző kapacitás mértékek.

Az alábbi képernyőfelvételen látható, amelynek kétféle csomópont fürt: FrontEnd és Kódmentes.  Minden csomópont öt csomópontok rendelkezik.

![Képernyőkép: a két csomópont típus tartalmazó fürt][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Virtuális skála megadása példányok megfeleltetése csomópontok

Amint felett látható, a virtuális skála megadása példányok indítsa el a példány 0, majd az Ugrás felfelé. A számozás tükröződik a neveket. Ha például BackEnd_0 a Kódmentes virtuális skála megadása-példány 0. Öt példányok BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 és BackEnd_4 nevű e adott virtuális skála megadása tartalmaz.

Akkor virtuális skála beállítása új példány jön létre. Az új virtuális skála megadása példány neve általában lesz a virtuális skála Set name + a következő számú példánya. Ebben a példában érdemes BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Virtuális skála leképezése terheléselosztókkal meg minden csomópont típusa/virtuális skála megadása

Ha telepítette a fürt a portálról, vagy az erőforrás-kezelő űrlapsablonok kapott azt, majd ha kap egy erőforrás csoportban az összes erőforrás listájának használt majd látni fogja a terheléselosztókkal az egyes virtuális skála megadása vagy csomópontra.

A név hasonló lenne: **LB -&lt;NodeType neve&gt;**. Ha például LB-sfcluster4doc-0, a képernyőképen látható módon:


![Erőforrások][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Egy virtuális skála megadása példány vagy csomópont csatlakozás távoli
Minden definiált fürt csomópont típusa egy külön virtuális skála megadása van állítva.  Ez azt jelenti, hogy a csomópont-típusok megadhat felfelé vagy lefelé egymástól függetlenül és különböző virtuális termékváltozatok állhat. Egyetlen példány VMs eltérően a virtuális skála megadása példányok egy virtuális IP-címének a saját nem kapja. Úgy lehet egy kicsit nehéz IP-címének keres, és olyan portot távoli használható egy adott példányhoz csatlakozni.

Az alábbiakban az alábbi lépések is fel őket.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Lépés: 1: Megtudhatja, hogy a virtuális IP-címe a csomópont típusát, és ezután RDP hálózati címfordító Eszközig bejövő szabályok

Ahhoz, hogy, kell az erőforrás definícióját **Microsoft.Network/loadBalancers**részeként definiált bejövő hálózati Címfordítást szabályok értéket.

Az portálon nyissa meg a betöltés terheléselosztó lap, majd a **Beállítások**.

![LBBlade][LBBlade]


A **Beállítások**területen kattintson a **hálózati címfordító Eszközig bejövő szabályok**. Ez most nyújt az IP-cím és az első példányt virtuális skála megadása csatlakozás távoli használható port. Az alábbi képernyőképen működik **104.42.106.156** és **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Lépés: 2: Keresse meg a távoli használható port csatlakoznia kell az adott virtuális skála megadása példány/csomóponthoz

Ehhez a dokumentumhoz a korábbi lehet beszélgetett hogyan feleltesse meg a virtuális skála megadása példányok csomópontok. Fogjuk használni, amely a pontos port megállapítása.

A portokat növekvő sorrendben, a virtuális skála megadása példány van rendelve. úgy, hogy a példában a FrontEnd csomópont típus, az egyes példányok öt portokat a következő. most már kell tennie az ugyanazt a virtuális skála megadása példány hozzárendelését.

|**Virtuális skála példány beállítása**|**Port**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>3 lépés: Az adott virtuális skála megadása példány csatlakozás távoli

Az alábbi képernyőképen segítségével távoli asztali kapcsolat a FrontEnd_1 csatlakozni:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>A RDP port tartomány értékek módosítása

### <a name="before-cluster-deployment"></a>Fürt telepítés előtt

Amikor úgy állítja be az erőforrás-kezelő sablonnal fürt, akkor a tartomány megadhatja a **inboundNatPools**.

Nyissa meg az erőforrás definícióját **Microsoft.Network/loadBalancers**. Amely a megtalálta a **inboundNatPools**leírását.  Cserélje ki a *frontendPortRangeStart* és *frontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Fürt telepítés után
Ez egy kicsit bonyolultabb, és az első újrahasznosított VMs vonhat. Most már akkor állítja be az új értékeket Azure PowerShell használatával. Győződjön meg arról, hogy az Azure PowerShell 1,0 vagy újabb verziója telepítve van-e a számítógépen. Nem ezt előtt, ha e erősen javasolt kövesse az című cikk lépéseit [telepítése és konfigurálása az Azure PowerShell.](../powershell-install-configure.md)

Jelentkezzen be az Azure-fiókjába. A PowerShell-parancsot valamiért nem sikerül, ha ellenőrizni kell, hogy rendelkezik-e Azure PowerShell megfelelően telepítve.

```
Login-AzureRmAccount
```

Futtassa a következő a terheléselosztó a részleteket, és megjelenik a leírás **inboundNatPools**az értékeket:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

A kívánt értékeket most állítsuk *frontendPortRangeEnd* és *frontendPortRangeStart* .

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Következő lépések

- [A "Üzembe bárhonnan" funkció és Azure felügyelt fürt összehasonlítása – áttekintés](service-fabric-deploy-anywhere.md)
- [Biztonsági csoport](service-fabric-cluster-security.md)
- [Szolgáltatás háló SDK és első lépések](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
