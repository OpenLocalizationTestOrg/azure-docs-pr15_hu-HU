<properties
 pageTitle="HPC csomag fürt számítási csomópontok kezelése |} Microsoft Azure"
 description="Tudnivalók a PowerShell parancsprogramot eszközök hozzáadása, eltávolítása, indítása és leállítása HPC Pack fürt számítási csomópontok Azure-ban"
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

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>A szám és a számítási csomópontok Azure-ban HPC csomag fürt elérhetőségének kezelése

Ha létrehozott egy HPC csomag fürthöz Azure VMs, érdemes lehet egyszerűen hozzáadása, eltávolítása, (rendelkezés) indítása és leállítása (deprovision) számítási számos VMs fürt csomópontjának a módjai. Hajtsa végre az alábbi műveleteket, futtassa a központi csomópontra virtuális telepített Azure PowerShell-parancsfájlokat. A parancsfájlok segítséget nyújt a szám és a HPC csomag fürt erőforrások elérhetőségének ellenőrzése, megadhatja, hogy költségeket.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Előfeltételek

* **Azure VMs HPC csomag fürt** – egy HPC csomag fürthöz létrehozása a klasszikus telepítési modell legalább használatával HPC 2012 R2 frissítés 1. szervizcsomagjának. A telepítési például automatizálhatja az Azure piactéren elérhető és az Azure PowerShell-parancsprogramot, az aktuális HPC csomag virtuális kép használatával. Információk és Előfeltételek című témakörben talál [a HPC csomag IaaS telepítési parancsfájlt egy HPC fürtre létrehozása](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    A telepítés után keresse meg a csomópont management parancsfájlok a % CCP\_KEZDŐLAP % bin mappát a központi csomópontra. A parancsfájlok mindegyike rendszergazdaként kell futtatni.

* **Azure közzététele beállítások fájl vagy management tanúsítvány** - kell tennie a központi csomópontra a következők valamelyikét:

    * **Importálás az Azure közzététele beállításfájl**. Ehhez a fő csomópont futtassa a következő Azure PowerShell-parancsmagok:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **A központi csomópontra a Azure management tanúsítvány konfigurálása**. Ha a .cer fájl, importálja a CurrentUser\My tárolóban található, és futtassa a következő Azure PowerShell-parancsmag Azure környezetben (AzureCloud vagy AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Számítási csomópont VMs hozzáadása

A **Hozzáadás-HpcIaaSNode.ps1** forgatókönyvvel számítási csomópontok hozzáadása.

### <a name="syntax"></a>Szintaxis
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Paraméterek

* **ServiceName** - nevére a felhőben, hogy új számítási csomópont VMs hozzáadódik szolgáltatás.

* **ImageName** - szerezhető be az Azure klasszikus portál vagy az Azure PowerShell-parancsmag **Get-AzureVMImage**keresztül Azure virtuális kép nevét. A kép az alábbi feltételeknek kell megfelelnie:

    1. A Windows operációs rendszert telepítve kell lennie.

    2. HPC Pack számítási csomópont szerepkör telepítve kell lennie.

    3. A kép a személyes kép felhasználói kategória nem nyilvános Azure virtuális kép kell lennie.

* **Mennyiség** - számítási csomópontot, hogy felvegyék VMs számát.

* **InstanceSize** – a számítási csomópont VMs méretét.

* **DomainUserName** – az új VMs csatlakozik a tartományhoz használt tartomány felhasználó nevét.

* **DomainUserPassword** – a tartomány felhasználó jelszavát.

* **NodeNameSeries** (nem kötelező) - kiosztási mintát a számítási csomópontok. A formátumot kell &lt; *legfelső szintű\_neve*&gt;&lt;*indítása\_szám*&gt;%. Ha például MyCN % 10 %-os eszközökkel adatsort MyCN11 kezdve számítási csomópont nevét. Ha nincs megadva, a parancsfájl a sorozat a HPC fürt elnevezési konfigurált csomópontot.

### <a name="example"></a>Példa

Az alábbi példa összeadja a felhőalapú szolgáltatás *hpcservice1*, a virtuális kép *hpccnimage1*alapján 20 méret nagy számítási csomópont VMs.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Számítási csomópont VMs eltávolítása

Távolítsa el az **Eltávolítás-HpcIaaSNode.ps1** forgatókönyvvel számítási csomópontot.

### <a name="syntax"></a>Szintaxis

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Paraméterek

* **Név** - eltávolítjuk fürt csomópontok nevét. Helyettesítő karakterek használhatók. A set paraméternév nevét. Nem lehet paramétert a **neve** és a **csomópontot** .

* **Csomópont** - HpcNode az objektumot el kell távolítani a csomópontok szerezhető be a HPC PowerShell-parancsmag [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)keresztül. A set paraméternév csomópontot. Nem lehet paramétert a **neve** és a **csomópontot** .

* **DeleteVHD** (nem kötelező) - társított lemezt a VMs eltávolított törlése beállítást.

* **Kötelező** (nem kötelező) - beállítás használatával offline állapotban HPC csomópontok kényszeríti előtt eltávolításukat.

* **Erősítse meg** (nem kötelező) – a parancs végrehajtása jóvá kell hagynia Rákérdezés a használni.

* **WhatIf** - ismertetik, hogy milyen lenne fordulhat elő, ha végrehajtotta a ténylegesen a parancs végrehajtása nélkül beállítást.

### <a name="example"></a>Példa

Az alábbi példa a kapcsolat nélküli csomópontok kezd kezdődő nevű *HPCNode-CN -* és eltávolítja a csomópontok és a kapcsolódó lemezt őket.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Indítsa el a számítási csomópont VMs

Indítsa el a **Kezdés-HpcIaaSNode.ps1** parancsfájl számítási csomópontok.

### <a name="syntax"></a>Szintaxis

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Paraméterek

* **Név** - elindításának fürt csomópontok nevét. Helyettesítő karakterek használhatók. A set paraméternév nevét. Nem lehet paramétert a **neve** és a **csomópontot** .

* **Csomópont**- a HpcNode objektum indítható el, hogy a csomópontok szerezhető be a HPC PowerShell-parancsmag [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)keresztül. A set paraméternév csomópontot. Nem lehet paramétert a **neve** és a **csomópontot** .

### <a name="example"></a>Példa

A következő példa elindítja csomópontok kezdődő nevű *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Számítási csomópont VMs leállítása

Állítsa le a számítási csomópontok a **Stop-HpcIaaSNode.ps1** forgatókönyvvel.

### <a name="syntax"></a>Szintaxis

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Paraméterek


* **Név**- nevek fürt csomópontok leállt. Helyettesítő karakterek használhatók. A set paraméternév nevét. Nem lehet paramétert a **neve** és a **csomópontot** .

* **Csomópont** - a HpcNode objektum állítható le, hogy a csomópontok szerezhető be a HPC PowerShell-parancsmag [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)keresztül. A set paraméternév csomópontot. Nem lehet paramétert a **neve** és a **csomópontot** .

* **Kötelező** (nem kötelező) - beállítás használatával offline állapotban HPC csomópontok kényszeríti őket leállítása előtt.

### <a name="example"></a>Példa

Az alábbi példa a kapcsolat nélküli csomópontok kezd kezdődő nevű *HPCNode-CN -* majd a csomópontok leáll.

Leállítás-HPCIaaSNode.ps1 – név HPCNodeCN-*-előírása

## <a name="next-steps"></a>Következő lépések

* Ha azt szeretné, hogy automatikusan a nagyobb vagy kisebb aszerint, hogy az aktuális terhelést feladatok és a feladatok megjelenítése a fürt fürt csomópontok oly módon, [automatikusan a nagyobb, és a HPC csomag fürt erőforrásokat megfelelően fürt terhelését Azure-ban a kisebb](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)témakörben talál.
