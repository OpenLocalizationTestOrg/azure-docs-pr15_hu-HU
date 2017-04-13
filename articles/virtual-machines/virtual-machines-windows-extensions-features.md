<properties
 pageTitle="Virtuális gép extensions és szolgáltatások |} Microsoft Azure"
 description="Megtudhatja, hogy milyen bővítmények Azure virtuális gépeken futó mi azt adja meg vagy javítása szerint csoportosított érhető el."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Tudnivalók a virtuális gép extensions és szolgáltatások

## <a name="azure-vm-extensions"></a>Azure virtuális bővítmények

Azure virtuális gép bővítmények kis alkalmazásokat, amelyek bejegyzés telepítési konfigurációs telepített és automatizálási tevékenység a Azure virtuális gépeken futó. Például ha egy virtuális számítógépre telepítve van, a víruskereső szoftver protection és az Docker konfigurációs szoftver van szüksége, egy virtuális bővítmény az alábbi feladatok elvégzésére használható. Azure virtuális bővítmények az Azure CLI, PowerShell, futtatható erőforrás kezelése sablonok és az Azure-portálra. Bővítmények kapcsolt új virtuális gép telepítéshez, vagy bármelyik meglévő rendszer futtatása.

A dokumentum ez előfeltételekről Azure virtuális gép bővítmény, és útmutatást a választható virtuális bővítések feltárása ismerteti. 

## <a name="azure-vm-agent"></a>Azure virtuális Agent

Az Azure virtuális ügynök az Azure virtuális gép és az Azure háló vezérlő közötti kölcsönhatás kezeli. A virtuális agent feladata telepítésével és Azure virtuális gépeken futó, beleértve a virtuális bővítmények futtatása kezelésével sok funkcionális tulajdonságát. Az Azure virtuális ügynök Azure gyűjtemény képek előre telepítve van, és a támogatott operációs rendszerekre telepíthető. 

Támogatott operációs rendszerek és a telepítési utasításokat a további tudnivalókért lásd [Azure virtuális gép ügynök](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Virtuális bővítmények felfedezése

Számos különböző virtuális bővítmények használata Azure virtuális gépeken futó érhetők el. Teljes listájának megtekintéséhez futtassa a következő parancsot az Azure CLI, a hely helyettesítve a helyre.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Közös virtuális bővítmények

|Kiterjesztés   |Leírás   |További információ   |
|---|---|---|
|A Windows egyéni parancsfájl-bővítmény  | Parancsfájlok szemben az Azure virtuális gép futtatása  |[A Windows egyéni parancsfájl-bővítmény](./virtual-machines-windows-extensions-customscript.md)   |
|A Windows DSC bővítmény | A PowerShell DSC (kívánt állapot konfigurációja) bővítményt.  | [Docker virtuális bővítmény](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Azure diagnosztika bővítmény | Azure diagnosztika kezelése |[Azure diagnosztika bővítmény](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
