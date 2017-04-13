<properties
   pageTitle="Átméretezheti a szolgáltatás háló fürtre vagy kicsinyítés |} Microsoft Azure"
   description="A szolgáltatás háló fürtre vagy kicsinyítés méretezze át a megfelelő igény szerinti automatikus méretezése szabályok az egyes csomópont típusa/virtuális skála megadásával. Hozzáadása vagy eltávolítása a szolgáltatás háló fürtre csomópontok"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>A szolgáltatás háló fürtre vagy kicsinyítés automatikus méretezése szabályok használatával méretezése

Virtuális gép skála beállítja az Azure számítási erőforrás, amelyek segítségével üzembe helyezéséhez és kezeléséhez tárolt virtuális gépeken futó gyűjteménye. Minden definiált szolgáltatás háló fürt csomópont típusa egy külön virtuális méretarányra beállított van beállítva. Minden csomópont típusa is majd átméretezi a el egymástól függetlenül van különböző csoportjaihoz nyitott portokat és beállíthatja, hogy a különböző kapacitás mértékek. További információ azt a [szolgáltatást háló nodetypes](service-fabric-cluster-nodetypes.md) dokumentumban. A szolgáltatás háló a fürt csomópont típus virtuális skála épülnek, a kódmentes állítja be, mivel kell az egyes csomópont típusa/virtuális skála automatikus méretezése szabályokat állíthat be.

>[AZURE.NOTE] Az előfizetés elég magmintákat hozzáadása az új VMs a fürt alkotó kell rendelkeznie. Jelenleg nem modell érvényesítése, így idő telepítési hiba, ha bármelyik a kvóta korlátozások vannak találati.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>A csomópont típusa/virtuális kiválasztásához méretezése beállítása

Jelenleg Ön nem sikerül a virtuális skála beállítja a portálon automatikus méretezése szabályokat adni, így tudassa velünk a csomópont-típus listában, majd adjon hozzá automatikus méretezése szabályok őket a Azure PowerShell (1.0 +) használatával.

A lista beszerzése az virtuális skála megadása a fürt alkotó, futtassa a következő parancsmagok:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Automatikus méretezése szabályok beállítása a csomópont típusa/virtuális skála megadása

Ha a fürt többféle csomópontot, majd ismételje meg az egyes csomópont típusok/virtuális skála állít be, hogy szeretne-e (vagy kicsinyítés) méretezni. Figyelembe csomópontok számának, hogy az automatikus méretezés beállítása előtt rendelkeznie. A kiválasztott megbízhatósági szint hajtja, a lehető legkevesebb csomópontot, amely a elsődleges csomópont típus kell rendelkeznie. További információ a [megbízhatósági szint](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Méretezés lefelé az elsődleges csomópontok kisebb, mint a minimális száma helyezése írja be a stabil fürt vagy leállíthatja azt. Az adatok elvesztését, az alkalmazások és a rendszer szolgáltatásokra eredményezhet.

Az automatikus méretezése funkció jelenleg nem hajtja a terhelést, amelyek az alkalmazások előfordulhat, hogy jelentéskészítés, a szolgáltatás háló. Így kap automatikus méretezése tisztán alapú a teljesítmény szerint pillanatnyilag, amelyek mindegyike a virtuális által kibocsátott számláló beállítása példányok méretezze át.  

Hajtsa végre az alábbi utasításokat [állíthatja be az egyes virtuális skála automatikus méretezése](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] A forgatókönyv lefelé skálával kivéve, ha a csomópont típusa Gold vagy ezüst tartóssági szintű rendelkezik, kell fordulnia a [eltávolítása-ServiceFabricNodeState parancsmag](https://msdn.microsoft.com/library/azure/mt125993.aspx) megfelelő csomópont nevű.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>Manuális VMs hozzáadása egy csomópont típusa/virtuális skála megadása

Kövesse a [rövid útmutató az Office-sablonok](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) módosítani szeretné a számát az egyes Nodetype VMs minta/utasításait. 

>[AZURE.NOTE] A VMs hozzáadása időt vesz igénybe, így nem a várt érték lennie a pillanatnyi kiegészítése. Úgy is az időt, engedélyezze a replikáinak érhető el a virtuális kapacitás előtt több mint 10 perc kapacitás hozzáadása / elhelyezett get-példányokat szolgáltatás megtervezése.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Távolítsa el manuálisan az elsődleges csomópont típusa/virtuális skála megadása VMs

>[AZURE.NOTE] Írja be a elsődleges csomópontot a fürt szolgáltatást háló rendszer futtatása Így soha nem leállítása vagy lefelé a csomópont-típusokat példányok száma méretezni kisebb, mint a megbízhatósági szint milyen igényli. [A megbízhatóság rétegek itt részleteibe](service-fabric-cluster-capacity.md)vonatkozik. 

Hajtsa végre az alábbi lépések egy virtuális példány egyszerre szeretne. Ez a rendszer szolgáltatások (és az állapot-nyilvántartó szolgáltatások) kell leállítása az eltávolítani kívánt virtuális példány és a többi csomóponton létrehozott új kópia tesz lehetővé.

1. Futtassa a [Letiltás-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) leképezés "RemoveNode" letiltása a szeretné, hogy távolítsa el (a legmagasabb, hogy csomópont típus) csomópontot.

2. Futtassa a [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) kattintva győződjön meg arról, hogy a csomópont van valóban-re áttelepített letiltja. Ha nem, várja meg, amíg a csomópont le van tiltva. Ez a lépés nem hurry.

2. Kövesse a [rövid útmutató az Office-sablonok](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) módosítani szeretné a számát VMs egy adott Nodetype a minta/utasításait. Eltávolított példány a legmagasabb virtuális példány. 

3. Ismételje meg az 1-3-as szükséges, de nem méretezheti lefelé az elsődleges csomópont típusú kisebb, mint a megbízhatósági szint igényli példányok száma. [A megbízhatóság rétegek itt részleteibe](service-fabric-cluster-capacity.md)vonatkozik. 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Távolítsa el manuálisan az elsődleges csomópont típusú/virtuális skála megadása a VMs

>[AZURE.NOTE] Állapot-nyilvántartó szolgáltatáshoz mindig ki kell karbantartása elérhetőségét, és a szolgáltatás állapota megőrzése csomópontok megadott számú szüksége van. A nagyon minimum, a szükséges csomópontok számának egyenlő a partíciót szolgáltatás cél replika beállítása számát. 

A végrehajtás az alábbi lépések egy virtuális példány szüksége van egy időben. Ez lehetővé teszi, hogy a rendszer szolgáltatások (és az állapot-nyilvántartó szolgáltatások) kell leállítása az eltávolítani kívánt virtuális példányon, és új kópiák létrehozott egyéb where.

1. Futtassa a [Letiltás-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) leképezés "RemoveNode" letiltása a szeretné, hogy távolítsa el (a legmagasabb, hogy csomópont típus) csomópontot.

2. Futtassa a [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) kattintva győződjön meg arról, hogy a csomópont van valóban-re áttelepített letiltja. Ha nem várja meg, amíg a csomópont le van tiltva. Ez a lépés nem hurry.

2. Kövesse a [rövid útmutató az Office-sablonok](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) módosítani szeretné a számát VMs egy adott Nodetype a minta/utasításait. Ezzel eltávolítja a legmagasabb virtuális példány. 

3. Ismételje meg az 1-3-as szükséges, de nem méretezheti lefelé az elsődleges csomópont típusú kisebb, mint a megbízhatósági szint igényli példányok száma. [A megbízhatóság rétegek itt részleteibe](service-fabric-cluster-capacity.md)vonatkozik.

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>A szolgáltatás háló Intézőben tapasztalhatja viselkedése

Ha a szolgáltatás háló Intéző fürt skála tükrözni fogja a fürt részét képező (virtuális méretarányra beállított példányok) csomópontok számának.  Jó helyen jár akkor fürt le, akkor jelenik meg az eltávolított csomópont/virtuális példány sérült állapotba jelenik meg, kivéve, ha a megfelelő csomópont nevű [eltávolítása-ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) hívja.   

Az alábbiakban a magyarázat a problémát.

A szolgáltatás háló Intézőben csomópontok tükröződés milyen szolgáltatás háló rendszer szolgáltatások (FM kifejezetten) tudja, hogy a fürt volna/van csomópontok számának kapcsolatban. Ha a meghatározott virtuális skála méretezéséhez a virtuális törölt, de FM rendszer szolgáltatás továbbra is lapon a csomópont (rendelt a törölt virtuális) vissza érkezzenek. Így szolgáltatás háló Explorer továbbra is megjeleníti, hogy csomópont (bár a rendszerállapot hiba vagy ismeretlen is lehet).

Győződjön meg arról, hogy csomópont el kell távolítani egy virtuális eltávolításakor, hogy két lehetőség áll rendelkezésére:

1) Válassza ki a Gold vagy ezüst tartóssági szintet (elérhető hamarosan) csomópontot a fürt típusok, amely lehetővé teszi az infrastruktúra-integráció. Amely majd automatikusan eltávolítja a csomópontok a rendszerállapot szolgáltatások (FM) akkor.
[A részletes tudnivalókat itt tartóssági szintek](service-fabric-cluster-capacity.md) hivatkozik

2) A virtuális példány mérete, miután, kell fordulnia a [eltávolítása-ServiceFabricNodeState parancsmag](https://msdn.microsoft.com/library/mt125993.aspx).

>[AZURE.NOTE] Szolgáltatás háló fürt csak bizonyos számú csomópontok kell be a mindig rendelkezésre álljon, és az állapot - néven "kvórum megőrzése" megőrzése Igen akkor általában nem biztonságos leállítása a fürt gépeken futó, kivéve, ha először hajtotta végre [teljes biztonsági másolatot készíteni a állapotát](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Következő lépések
Olvassa el az alábbi módon is tervezési fürt beosztását, fürt frissítése és szolgáltatások szétválasztás megtudhatja:

- [A fürt kapacitás megtervezése](service-fabric-cluster-capacity.md)
- [Fürt frissítések](service-fabric-cluster-upgrade.md)
- [Maximális méret partíciót állapot-nyilvántartó szolgáltatások](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
