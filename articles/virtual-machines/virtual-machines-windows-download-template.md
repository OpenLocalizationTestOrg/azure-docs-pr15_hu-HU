<properties
    pageTitle="A virtuális kép létrehozása az Azure virtuális |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre egy általános virtuális képe egy meglévő, az erőforrás-kezelő telepítési modell készült Azure virtuális"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Töltse le a sablont egy virtuális

A portálon vagy a PowerShell használatával Azure-ban létrehozott egy virtuális, erőforrás-kezelő sablon automatikusan létrejön. Ezzel a sablonnal gyorsan egyszerűen duplikálhatja a telepítés is használhatja. A sablon összes az erőforrások erőforráscsoport információkat tartalmaz. Virtuális géphez Ez azt jelenti, a sablon tárolók mindent, amely a virtuális az erőforráscsoport, köztük a hálózati erőforrásokat támogató jön létre.

## <a name="download-the-template-using-the-portal"></a>Töltse le a sablont a portálon

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Egy központi menüben jelölje ki a **virtuális gépeken futó**.
3. Jelölje ki a virtuális gép a listából.
5. Jelölje be az **automatizálási parancsfájl**.
6. Jelölje ki a **letölteni** , és a .zip fájl mentése a helyi számítógépre.
7. Nyissa meg a .zip fájlt, és egy mappát a fájlokat. A .zip fájl fogja tartalmazni:
    
    - Deploy.ps1
    - Deploy.SH 
    - deployer.RB
    - DeploymentHelper.cs
    - Parameters.JSON
    - Template.JSON

.Json fájl, a sablon.
    
## <a name="download-the-template-using-powershell"></a>Töltse le a sablont a PowerShell használatával

A [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) parancsmaggal a .json sablonfájl is letöltheti. Használhatja a `-path` paraméter használatával adja meg a fájl nevét és elérési útját a .json fájl. Ez a példa bemutatja, hogyan töltse le a sablont az erőforráscsoport nevű **myResourceGroup** a helyi számítógépre **C:\users\public\downloads** mappájába.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Következő lépések

Sablonok használata erőforrások telepítésével kapcsolatos további tudnivalókért lásd: az [erőforrás-kezelő sablon forgatókönyv](../resource-manager-template-walkthrough.md).