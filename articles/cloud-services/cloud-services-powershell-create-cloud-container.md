<properties
   pageTitle="Hozzon létre egy felhőalapú szolgáltatás tároló PowerShell |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan hozhat létre egy felhőalapú szolgáltatás tároló PowerShell. A tároló webhelyen és dolgozó szerepkörök tárolja."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Hozzon létre egy üres felhőalapú szolgáltatást tároló az Azure PowerShell paranccsal
Ez a cikk ismerteti, hogyan Azure PowerShell-parancsmagok használata Cloud Services tároló gyors létrehozásához. Kérjük, kövesse az alábbi lépéseket:

1. Telepítse a Microsoft Azure PowerShell-parancsmag a [Azure PowerShell letöltési](http://aka.ms/webpi-azps) lapjáról.
2. Nyissa meg a PowerShell a parancssor ikonra.
3. A [Hozzáadás-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) való bejelentkezéshez használt.

    > [AZURE.NOTE] További útmutatás az Azure PowerShell-parancsmag telepítése és csatlakoztatása az Azure előfizetését tekintse át a [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md).

4. A **New-AzureService** parancsmag használatával hozzon létre egy üres Azure felhőalapú szolgáltatást tároló.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Kövesse az ebben a példában a parancsmag meghívásához:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Az Azure felhőszolgáltatásba létrehozásával kapcsolatos további tudnivalókért futtatása:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Következő lépések

 * A felhőalapú szolgáltatás telepítésének kezelése, tanulmányozza a [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Eltávolítás-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)és [Beállítása – AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) parancsokat. Is nézze meg [felhőszolgáltatások beállításáról](cloud-services-how-to-configure.md) további információt.

 * Azure a felhőalapú szolgáltatás projekt közzététele, a [felhőbeli szolgáltatástól Azure-ban a folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md)hivatkozhat a **PublishCloudService.ps1** kód minta.
