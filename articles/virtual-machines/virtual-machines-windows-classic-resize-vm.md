<properties
    pageTitle="Méretezze át a klasszikus Windows virtuális |} Microsoft Azure"
    description="Méretezze át a klasszikus telepítési modell, Azure Powershell használatával létrehozott Windows virtuális géphez."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>A Windows virtuális a klasszikus telepítési modell létrehozott átméretezése

Ez a cikk bemutatja, hogyan méretezheti át a Windows virtuális, a klasszikus telepítési modell Azure Powershell használatával létrehozott.

Amikor az azt jelenti, hogy egy virtuális átméretezheti, vannak két fogalmak, amelyek szabályozhatja a rendelkezésre álló méretezze át a virtuális gép méretű cellatartományt. Az első fogalma a régió, amelyen telepítve van a virtuális. Régióban elérhető virtuális méretek csoportban az Azure régiók weblap Services lapja van. A második fogalma a fizikai hardver jelenleg szolgáltatója a virtuális. VMs üzemeltető fizikai kiszolgálókon vannak csoportosítva fürt közös fizikai eszközök kapacitása. Egy virtuális méretének módosítása metódusát függően eltér, ha a kívánt új virtuális méretet a hardver fürt jelenleg szolgáltatója a virtuális által támogatott.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Is [átméretezése az erőforrás-kezelő telepítési modell létrehozott egy virtuális](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>A fiók hozzáadása

Azure Powershellhez készült Azure erőforrások klasszikus meg kell adnia. Hajtsa végre az alábbi lépéseket követve állítsa be az Azure PowerShell klasszikus erőforrások kezelésére.

1. Írja be a PowerShell-parancssorában `Add-AzureAccount` , és kattintson az **ENTER billentyűt**. 
2. Írja be az e-mail címet az Azure-előfizetéséhez társított, és kattintson a **Tovább**gombra. 
3. Írja be a fiókjához tartozó jelszót. 
4. Kattintson a **Bejelentkezés**gombra. 


## <a name="resize-in-the-same-hardware-cluster"></a>A hardver azonos fürt átméretezése

Elérhető a hardver fürt szolgáltatója a virtuális csökkentése egy virtuális átméretezéséhez hajtsa végre az alábbi lépéseket.

1. Futtassa a virtuális méretű érhető el, a hardver fürt szolgáltatója a felhőbeli szolgáltatástól, amely tartalmazza a virtuális felsoroló alábbi PowerShell-parancsot.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. A következő parancsokat méretezze át a virtuális.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>A hardver új fürthöz átméretezése

Méretezze át a nem érhető el, a hardver fürt szolgáltatója a virtuális, az a felhő csökkentése egy virtuális szolgáltatás és az összes VMs a felhőszolgáltatásában kell újra létre kell hozni. Minden felhőszolgáltatásba egyetlen hardver fürtre üzemelteti, így a felhőalapú szolgáltatást az összes VMs kell lennie a hardver fürthöz által támogatott méretet. A következőképpen fog ismertetik, hogyan lehet egy virtuális átméretezéséhez törlése, és ezután újra létrehozni a felhőalapú szolgáltatást.

1. Futtassa a régióban elérhető virtuális méretű felsoroló alábbi PowerShell-parancsot. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Jegyezze fel a minden virtuális beállításait a felhőszolgáltatásában, mely tartalmazza a virtuális való kell átméretezni. 
3. Törölje a felhőbeli szolgáltatástól, amely megőrzi a lemez minden virtuális parancs kiválasztása az összes VMs.
4. Hozza létre újból a virtuális szeretne a kívánt méretet virtuális lehet átméretezni.
5. Hozza létre ismét minden más VMs, amely egy virtuális méret érhető el, a hardver fürt most szolgáltatója a felhőbeli szolgáltatástól felhőalapú szolgáltatás.

Az törlése és újbóli létrehozása egy felhőalapú szolgáltatásba, egy új virtuális méret funkcióval mintaparancsfájl megtalálható [Itt](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Következő lépések

- [Az erőforrás-kezelő telepítési modell létrehozott egy virtuális átméretezése](virtual-machines-windows-resize-vm.md).