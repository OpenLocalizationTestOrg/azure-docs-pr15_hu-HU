<properties
 pageTitle="Virtuális gép extensions és szolgáltatások |} Microsoft Azure"
 description="Megtudhatja, hogy milyen bővítmények Azure virtuális gépeken futó mi azt adja meg vagy javítása szerint csoportosított érhető el."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Tudnivalók a virtuális gép extensions és szolgáltatások

## <a name="azure-vm-extensions"></a>Azure virtuális bővítmények

Azure virtuális gép bővítmények kis alkalmazásokat, amelyek bejegyzés telepítési konfigurációs telepített és automatizálási tevékenység a Azure virtuális gépeken futó. Például ha egy virtuális számítógépre telepítve van, a víruskereső szoftver protection és az Docker konfigurációs szoftver van szüksége, egy virtuális bővítmény az alábbi feladatok elvégzésére használható. Azure virtuális bővítmények az Azure CLI, PowerShell, futtatható erőforrás kezelése sablonok és az Azure-portálra. Bővítmények kapcsolt új virtuális gép telepítéshez, vagy bármelyik meglévő rendszer futtatása.

A dokumentum ez előfeltételekről Azure virtuális gép bővítmény, és útmutatást a választható virtuális bővítések feltárása ismerteti. 

## <a name="azure-vm-agent"></a>Azure virtuális Agent

Az Azure virtuális ügynök az Azure virtuális gép és az Azure háló vezérlő közötti kölcsönhatás kezeli. A virtuális agent feladata telepítésével és Azure virtuális gépeken futó, beleértve a virtuális bővítmények futtatása kezelésével sok funkcionális tulajdonságát. Az Azure virtuális ügynök Azure gyűjtemény képek előre telepítve van, és a támogatott operációs rendszerekre telepíthető. 

Támogatott operációs rendszerek és a telepítési utasításokat a további tudnivalókért lásd [Azure Linux ügynök útmutatója](./virtual-machines-linux-agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Virtuális bővítmények felfedezése

Számos különböző virtuális bővítmények használata Azure virtuális gépeken futó érhetők el. Teljes listájának megtekintéséhez futtassa a következő parancsot az Azure CLI, a hely helyettesítve a helyre.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Közös virtuális bővítmények

|Kiterjesztés   |Leírás   |További információ   |
|---|---|---|
|Egyéni parancsfájl bővítményének Linux  | Parancsfájlok szemben az Azure virtuális gép futtatása  |[Egyéni parancsfájl bővítményének Linux](./virtual-machines-linux-extensions-customscript.md)   |
|Docker bővítmény |A Docker szolgáltatás támogatja a távoli Docker parancsokat a telepítést.  | [Docker virtuális bővítmény](./virtual-machines-linux-dockerextension.md)  |
|Virtuális Access bővítmény | Ismét elérni az Azure virtuális gépen  |[Virtuális Access bővítmény](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Azure diagnosztika bővítmény |Azure diagnosztika kezelése |[Azure diagnosztika bővítmény](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

