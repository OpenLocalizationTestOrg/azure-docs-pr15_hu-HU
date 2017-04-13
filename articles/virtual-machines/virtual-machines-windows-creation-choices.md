<properties
    pageTitle="A Windows virtuális létrehozása különböző módokon |} Microsoft Azure"
    description="Erőforrás-kezelő Windows virtuális gép létrehozása különböző módokon listája."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Erőforrás-kezelő Windows virtuális gép létrehozása különböző módokon

Azure virtuális gépeken futó alkalmasak a különböző felhasználók és a céljára létrehozhat egy virtuális gép eltérő lehetőségeket kínál. Ez azt jelenti, hogy szükséges a virtuális gép és hogyan hozhat létre, akkor lehetőség közül választhat. Ez a cikk biztosít a választási lehetőségek és a hivatkozások utasításokat.

## <a name="azure-portal"></a>Azure portál

Próbálja ki a virtuális gép egyszerűen az Azure portálon, különösen akkor, ha azt még csak most kezdi az Azure. 

[A portálon Windows operációs rendszert futtató virtuális gép létrehozása](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Sablon

Virtuális gépeken futó igénylő erőforrások kombinációját (például egy elérhetőségének beállítása és a tárterület-fiók). Nem pedig annak az üzembe helyezése, és kezeléséről az egyes erőforrásokhoz külön-külön, létrehozhat egy erőforrás-kezelő Azure-sablon üzembe helyezése, és az összes erőforrás egyetlen, összehangolt művelet rendelkezések.

- [Az erőforrás-kezelő sablonnal Windows virtuális gép létrehozása](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Ha inkább egy parancs rendszerhéj dolgozik, az Azure PowerShell is használhatja.

- [Hozzon létre egy Windows virtuális PowerShell használatával](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Visual Studio segítségével összeállítása, kezelése és üzembe helyezése a Azure eszközzel VMs Visual Studio és a Azure SDK csomagjában talál.

[Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

