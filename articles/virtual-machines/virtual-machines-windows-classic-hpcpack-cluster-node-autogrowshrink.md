<properties
 pageTitle="Automatikus méretezéssel HPC Pack fürt csomópontok |} Microsoft Azure"
 description="Automatikusan a nagyobb, és a kisebb HPC csomag fürt számítási csomópontok Azure száma"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatikusan a nagyobb, és az Azure HPC csomag fürt erőforrások zsugorítása a terhelést a fürt szerint




Ha Azure "burst" csomópontok beállítaná a HPC csomag fürt, vagy létrehozhat egy HPC csomag fürthöz Azure VMs, célszerű lehet automatikusan a nagyobb vagy kisebb Azure számítási erőforrások, például csomópontok vagy az aktuális terhelést a fürt szerint magmintákat száma lehetőséget. Lehetővé teszi, hogy az Azure erőforrások hatékonyabban használja, és szabályozhatja a költségeket.
Ehhez a HPC csomag fürt tulajdonság **AutoGrowShrink**beállítása. Azt is megteheti futtassa az **AzureAutoGrowShrink.ps1** HPC PowerShell-parancsprogramot, amely a HPC csomag telepítve van.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Is jelenleg is automatikusan csak a nagyobb és a Windows Server operációs rendszert futtató HPC csomag számítási csomópontok kisebb.

## <a name="set-the-autogrowshrink-cluster-property"></a>A AutoGrowShrink fürt tulajdonság

### <a name="prerequisites"></a>Előfeltételek

* **HPC Pack 2012 R2 frissítés 2 vagy újabb fürt** – központi csomópont lehet használatát vagy a helyszíni telepített vagy egy Azure virtuális. Lásd: [a hibrid fürtre HPC Pack beállítása](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) egy helyszíni fő csomópont és Azure "burst" csomópontok használatbavételéhez. Lásd: a [HPC csomag IaaS telepítési parancsfájlt](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) gyorsan telepítésére az Azure VMs HPC csomag fürt.


* **Az Azure-ban egy központi csomópont fürt** – Ha a HPC csomag IaaS telepítési parancsfájlt hozza létre, a **AutoGrowShrink** fürt tulajdonság kapcsolhatja be a fürt konfigurációs fájl AutoGrowShrink beállítást. További információ a dokumentációjában tartozó [parancsfájl letöltési](https://www.microsoft.com/download/details.aspx?id=44949). 

    A **AutoGrowShrink** fürt tulajdonság azt is megteheti, engedélyezése után a fürt PowerShell-parancsokkal HPC az alábbi szakaszban ismertetett való telepítéséhez. Felkészülés a, hajtsa végre az alábbi lépéseket:
    1. Az Azure management-tanúsítvány beállítása, a központi csomóponton és Azure-előfizetésben. A próba-telepítéshez HPC csomag telepíti a központi csomópontra Microsoft HPC Azure alapértelmezett önaláírt tanúsítványt használja, és egyszerűen csak töltse fel a tanúsítvány Azure-előfizetéséhez. Beállítások és a lépéseket a [TechNet könyvtár útmutatást](https://technet.microsoft.com/library/gg481759.aspx)talál.
    2. **A központi csomópontra a Regedit** , nyissa meg a HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, és adjon hozzá egy új karakterlánc értéket. A "Ujjlenyomat" érték nevet adja, és az adatokat az ujjlenyomatot a tanúsítvány az 1 értéket.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC PowerShell szolgáló parancsok a AutoGrowShrink tulajdonság

Következő vannak minta HPC PowerShell-parancsait, **AutoGrowShrink** beállítása és finomhangolása a további paraméterekkel viselkedését. [AutoGrowShrink paraméterek](#AutoGrowShrink-parameters) lásd a jelen cikk beállítások megadása a teljes listát. 

A parancsok indítsa el a HPC PowerShell központi csomóponton rendszergazdaként.

**Ahhoz, hogy a AutoGrowShrink tulajdonság**

    Set-HpcClusterProperty –EnableGrowShrink 1

**A AutoGrowShrink tulajdonság letiltása**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Nagyítás intervallum módosításához percben**

    Set-HpcClusterProperty –GrowInterval <interval>

**A Lekicsinyítve időköz módosítása percben**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**A jelenlegi konfigurációjának AutoGrowShrink megtekintése**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink paraméterei

Az alábbiakban AutoGrowShrink paraméterek, amely a **Set-HpcClusterProperty** parancs használatával módosítható.

* **EnableGrowShrink** – Váltás a engedélyezése vagy letiltása a **AutoGrowShrink** tulajdonságot.
* **ParamSweepTasksPerCore** – egy core növekedéséhez számszerű takarítás tevékenységek száma. Az alapértelmezett érték egy core Feladatonkénti növekedéséhez. 
 
    >[AZURE.NOTE] HPC csomag QFE KB3134307 **ParamSweepTasksPerCore** **TasksPerResourceUnit**változik. A feladat erőforrástípus alapján és a csomópontot, szoftvercsatorna vagy core lehet.
    
* **GrowThreshold** - indíthatja el az automatikus NÖV várólistás feladatok küszöbértéknél. Az alapértelmezett érték 1, ami azt jelenti, hogy, ha vannak várólistás állapotú, 1 vagy több feladat automatikusan a nagyobb csomópontot.
* **GrowInterval** - időközt percben automatikus NÖV indíthatja el. Az alapértelmezett érték 5 percig tart.
* **ShrinkInterval** - időköz percben szeretne elindítani az automatikus méretének csökkentése. Az alapértelmezett érték 5 percig tart. |}
* Használaton **ShrinkIdleTimes** - folyamatos ellenőrzi, hogy a Lekicsinyítve, hogy a csomópontok számát. Az alapértelmezett érték 3 időpontokat. Például ha az 5 perc, a **ShrinkInterval** HPC csomag annak vizsgálata, hogy a csomópont üresjárati 5 percenként. Ha a csomópontok üresjárati állapotú folyamatos 3 (15 perc) ellenőrzése után, HPC csomag csomópontra zsugorodik össze.
* **ExtraNodesGrowRatio** – az üzenet továbbítása felület (MPI) feladatok növekedéséhez csomópontok további százalékát. Az alapértelmezett érték 1, ami azt jelenti, hogy HPC csomag megnő csomópontok MPI feladatok 1 %-kal. 
* **GrowByMin** – Váltás a jelezheti a feladat szükséges minimális erőforrásokat alapuló kibővítését házirend. Az alapértelmezett érték hamis, ami azt jelenti, hogy HPC csomag megnő csomópontok feladatokhoz, a feladatok szükséges erőforrások maximális száma alapján.
* A nagyobb folyamat **SoaJobGrowThreshold** - küszöbértékét, a bejövő SOA kérelmek indíthatja el az automatikus. Az alapértelmezett érték 50000.  
    
    >[AZURE.NOTE] Ez a paraméter támogatott HPC csomag 2012 R2 frissítés 3 kezdve.
    
* **SoaRequestsPerCore** – egy core növekedéséhez kérelmek bejövő SOA száma. Az alapértelmezett érték 20000.  

    >[AZURE.NOTE] Ez a paraméter támogatott HPC csomag 2012 R2 frissítés 3 kezdve.

### <a name="mpi-example"></a>Példa MPI

Alapértelmezés szerint HPC csomag megnő 1 %-os (**ExtraNodesGrowRatio** értéke 1) MPI feladatokhoz további csomópontot. Oka az, hogy MPI megkövetelheti több csomópontot, és a feladat csak futtatható, amikor készen áll az összes csomópontot. Csomópontok Azure indításakor alkalmanként egy csomópont előfordulhat, hogy van szüksége több, mint másoknak okozó más csomópontok való felkészítése csomópontra várakozás közben üresjárati kell elindítani. További csomópontok növekvő, a HPC Pack csökkenti a erőforrás várakozási időt, és esetleg menti a költségeket. Ha növelni szeretné (például a 10 %) MPI feladatokhoz további csomópontok százaléka, hasonló parancs futtatása

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>Példa SOA

Alapértelmezés szerint **SoaJobGrowThreshold** 50000 van állítva, és **SoaRequestsPerCore** 200000-rel van beállítva. Ha egy SOA feladatot 70000 kéréseivel elküldéséhez lesz egy várólistás feladatok és bejövő felkérést 70000. Ebben az esetben HPC csomag megnő a várólistás feladatok és a bejövő felkérést, 1 core (70000 és 50000) ra nő / alapvető 20000 = 1, teljes fognak nőni a SOA feladat 2 magmintákat így.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Futtassa a AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Előfeltételek

* **HPC 2012 R2 frissítés 1. szervizcsomagjának vagy újabb fürt** – a **AzureAutoGrowShrink.ps1** parancsfájl % CCP_HOME % tároló mappában van telepítve. Központi csomópont lehet használatát vagy a helyszíni telepített vagy egy Azure virtuális. Lásd: [a hibrid fürtre HPC Pack beállítása](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) egy helyszíni fő csomópont és Azure "burst" csomópontok használatbavételéhez. Lásd: a [HPC Pack IaaS telepítési parancsfájlt](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) gyorsan terjesztése az Azure VMs HPC Pack fürt, vagy [Azure quickstart útmutató sablont](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure PowerShell 0.8.12** - jelenleg a parancsfájl attól függ, hogy Azure PowerShell adott verziójában. Ha egy újabb verzióját futtatja a központi csomópontra, lehet, ha vissza léptetheti Azure PowerShell [verzió 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) futtatása. 

* **Azure burst csomópontokkal fürt** - futtassa a HPC csomagot hol ügyfélszámítógépen, vagy a központi csomópontra. Ha fut az ügyfélszámítógépen, győződjön meg arról, hogy be van-e a változó $env: CCP_SCHEDULER megfelelően mutasson a központi csomópontot. Azure "burst" csomópontok már hozzá kell adni a fürt, de nem rendszerbe állapotú lehetnek.


* **Fürt telepítését az Azure VMs** - futtatása a központi csomópontra virtuális, mert a **Kezdés-HpcIaaSNode.ps1** amelytől függ, és a **Stop-HpcIaaSNode.ps1** parancsprogramot, amely vannak telepítve van. A parancsfájlok továbbá szükséges az Azure management tanúsítványt, vagy tegye közzé a fájl (lásd: [kezelése egy HPC Pack fürthöz Azure-ban található csomópontok számítja ki.](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Gondoskodjon arról, hogy a számítási csomópont VMs van szüksége a már hozzáadott a fürt. Leállítva állapotú lehetnek.

### <a name="syntax"></a>Szintaxis

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Paraméterek

 * **NodeTemplates** – a csomópont-sablonok azoknak a nagyobb, és a kisebb csomópontok hatókörének megadásához. Ha nincs megadva (az alapértelmezett érték @()), **AzureNodes** csomópont csoportjának csomópontjait hatókör is, ha **NodeType** AzureNodes értéket tartalmaz, és **ComputeNodes** csomópont csoportjának csomópontjait esetén hatókör **NodeType** értéke ComputeNodes.

 * **JobTemplates** - növekedéséhez csomópontok hatókörének megadásához a projektsablonok nevét.

 * **NodeType** - csomópontot a nagyobb, és a kisebb típusát. Támogatott értékei a következők:

     * **AzureNodes** – az Azure PaaS (burst) csomópontot a korábbi helyszíni vagy Azure IaaS fürt.

     * **ComputeNodes** - számítási csomópontjának VMs csak egy Azure IaaS fürthöz.

* **NumOfQueuedJobsPerNodeToGrow** - csomópont növekedéséhez szükséges várólistás feladatok száma.

* **NumOfQueuedJobsToGrowThreshold** - várólistás feladatok, a megnövesztés megkezdéséhez küszöb számát.

* **NumOfActiveQueuedTasksPerNodeToGrow** - csomópont növekedéséhez szükséges aktív várólistás feladatok száma. Ha **NumOfQueuedJobsPerNodeToGrow** 0-nál nagyobb érték van megadva, ez a paraméter figyelmen kívül hagyja.

* **NumOfActiveQueuedTasksToGrowThreshold** - aktív várólistás feladatok, a megnövesztés megkezdéséhez küszöb számát.

* **NumOfInitialNodesToGrow** – Ha hatókör csomópontjait **Nem rendszerbe** vagy **Leállítva (Deallocated)**növekedéséhez csomópontok kezdeti minimális száma.

* **GrowCheckIntervalMins** – az intervallum között ellenőrzi, hogy a nagyobb, percben.

* **ShrinkCheckIntervalMins** – az intervallum percben ellenőrzi, hogy a kisebb között.

* **ShrinkCheckIdleTimes** - folyamatos számát ( **ShrinkCheckIntervalMins**elválasztva) szempontú vizsgálatok igazodjon jelzik, hogy a csomópontok üresjárati.

* **UseLastConfigurations** – az előző konfigurációk, a rendszer argumentum.

* **ArgFile**– a argumentum fájl mentéséhez, és frissítse a parancsfájl futtatásához használt nevét.

* **LogFilePrefix** – a naplófájl neve előtagot. Megadhatja, hogy az elérési. A napló alapértelmezés szerint az aktuális munkakönyvtár írása.

### <a name="example-1"></a>Példa 1

Az alábbi példa az alapértelmezett AzureNode webhelysablonnal a nagyobb, és automatikusan zsugorítása rendszerbe Azure burst csomópontok állítja be. Ha az összes csomópontok kezdetben **Nem rendszerbe** állapotú, legalább 3 csomópontok elindított. Ha várólistás feladatok száma meghaladja a 8, a parancsfájl elindítja csomópontok, mindaddig, amíg a saját várólistás feladatok **NumOfQueuedJobsPerNodeToGrow**arányát ezernél. Ha egy csomópont kiderül, hogy a 3 egymást követő üresjárati alkalommal üresjárati, leállt.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Példa 2

Az alábbi példa az Azure számítási csomópontot a nagyobb és zsugorodik össze automatikusan az alapértelmezett ComputeNode sablon rendszerbe VMs állítja be.
A feladatok állítható be a fájlsablont feladat hatókörének áttekintése a terhelést a fürt definiálni. Ha minden csomópontok kezdetben vannak leállítja, legalább 5 csomópontok elindított. Az aktív várólistás feladatok száma meghaladja a 15, ha a parancsfájl elindítja csomópontok, mindaddig, amíg a ezernél **NumOfActiveQueuedTasksPerNodeToGrow**aktív várólistás feladatok arányát. Ha egy csomópont kiderül, hogy a 10 egymást követő üresjárati alkalommal üresjárati, leállt.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
