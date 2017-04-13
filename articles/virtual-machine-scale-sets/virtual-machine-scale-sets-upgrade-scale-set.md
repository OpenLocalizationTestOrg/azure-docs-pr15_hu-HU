<properties
    pageTitle="A virtuális gép skála beállítja az alkalmazások terjesztése |} Microsoft Azure"
    description="A virtuális gép skála készletek-alkalmazások terjesztése"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Frissítse a virtuális gép skála megadása

Ez a cikk azt ismerteti, hogyan akkor is bevezetésére OS frissítés az Azure virtuális gép méretarányra bármely legrövidebb leállás nélkül. Ebben a környezetben OS frissítés miként módosíthatja a vagy Termékváltozat az operációs rendszer verziója, illetve az egyéni kép URI foglalja magában. Frissítése nélkül legrövidebb leállás eszközök frissítése nem pedig annak egyszerre virtuális gépeken futó egy egyenként vagy csoportosan (például egy hibafa tartomány egyszerre). Ezzel a módszerrel bármely nem frissítik a virtuális gépeken futó megtartása verziót.

Ellentmondás elkerülése érdekében vegyük megkülönböztetni háromféle operációs rendszer frissítése, érdemes lehet végrehajtani:

- Módosíthatja a verzió és a platform kép Termékváltozat. Például módosítása Ubuntu 14.04.2-LTS verziót a 14.04.201506100 14.04.201507060, vagy módosítja a Ubuntu 15.10/legújabb Termékváltozat 16.04.0-LTS/latest. Ebben az esetben a jelen cikkben vonatkozik.

- Beépített módosításáról az egyéni kép új verziója mutató URI (**Tulajdonságok** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **kép** > **uri**). Ebben az esetben a jelen cikkben vonatkozik.

- A virtuális gép belül az operációs rendszer javítása (Ez többek között a javítás telepítéséhez és futtatásához a Windows Update). Ebben az esetben is támogatott, de nem vonatkozik a jelen cikkben.

Az első két beállítás követelményei támogatott ez a cikk hatálya alá. Hozzon létre egy új skála, hajtsa végre a harmadik beállítást állítsa kell.

Virtuális gép skála beállítja az [Azure Service háló](https://azure.microsoft.com/services/service-fabric/) fürt részeként telepített itt nem vonatkoznak.

A egyszerű sorrendjének módosítása az operációs rendszer verziója/Termékváltozat platform kép vagy az egyéni kép URI a következőképpen néz ki:

1. Ismerkedés a virtuális gép méretarányra beállított modell.

2. A verzió, Termékváltozat vagy URI az az érték a modell módosítása.

3. Frissíti a modellt.

4. Végezze el a virtuális gépeken futó skála megadása a *manualUpgrade* hívást. Ebben a lépésben csak akkor jelentősége, ha *upgradePolicy* a **kézi** beállítás a skála megadása. **Automatikus**beállított, a virtuális gépeken futó frissítése egyszerre, így okozó legrövidebb leállás.


A háttér-információk szem előtt a lássuk, hogyan állítsa be a PowerShell és a REST API segítségével skálával verziójának sikerült frissíteni. Ezek a példák az esetekre platform kép, de ez a témakör nyújt elegendő információt, hogy ezt a folyamatot, egyéni képpé alkalmazkodik.

## <a name="powershell"></a>A PowerShell##

Ebben a példában az új verzió 4.0.20160229 beállítása a Windows virtuális gép méretaránnyal frissíti. Miután frissítette a modellt, a frissítés egy virtuális gép példány egyszerre igen.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Ha az egyéni képen egy platform image verzió ehelyett URI frissít, cserélje ki a "az új verzió beállítása" sor valamit, például a következőket:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>A REST API-val

Az alábbiakban néhány Python példák, amelyek az Azure REST API-OS verziójú frissítés bevezetésére. A lightweight [azurerm](https://pypi.python.org/pypi/azurerm) könyvtár Azure REST API-csomagolópapír függvények használatával is végezze el a skála megadása a modellben egy helyezése a frissített modell követ a GET. Is megtekintik virtuális gép példányok nézetek azonosítja a virtuális gépeken futó frissítése tartomány.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) , amely egy virtuális gépen futó méretarányra az operációs rendszer frissítése bevezetésére Python parancsfájl egyszerre egy frissítés tartomány meg.

![A virtuális gépeken futó vagy az update domain kiválasztására Vmssupgrade parancsfájl](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Ez a parancsfájl lehetővé teszi adott virtuális gépeken futó frissíteni, vagy adjon meg egy frissítés tartományt. Miként módosíthatja a kép platform verziót, illetve az egyéni kép URI támogat.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) -e egy virtuális gép skála készletek általános célú szerkesztője megjelenítő virtuális gép állapot egy heatmap, ahol a egy sor egy frissítés tartományt jelöli. Egyebek mellett a modell a méretarányra beállított frissíteni egy új verzió, Termékváltozat vagy egyéni kép URI, és válasszon a hibafa tartományok frissítéséhez. Ha így tesz, az összes a virtuális gépeken futó frissítése tartomány frissítése az új modellre. Megteheti azt is megteheti, közbeni frissítés lehetőség a tétel méretét alapján.  

Az alábbi képernyőképen látható skálával Ubuntu 14.04-2LTS 14.04.201507060 verzió beállítása a modell. Számos további lehetőségek van hozzáadva ezzel az eszközzel, mivel ez a kép felvételének.

![Egy skála megadása a Ubuntu 14.04-2LTS Vmsseditor modell](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

**Frissítése** , majd **Első részletei**gombra kattint, a UD 0 virtuális gépeken futó kezdje frissítése.

![Folyamatban lévő Vmsseditor megjelenítő frissítés](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
