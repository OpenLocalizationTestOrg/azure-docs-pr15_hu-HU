<properties
   pageTitle="Naplógyűjtés Azure diagnosztika segítségével |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogy miként állíthat be Azure diagnosztikai naplók gyűjt a rendszerű az Azure Service háló fürtre."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Naplógyűjtés Azure diagnosztika segítségével

> [AZURE.SELECTOR]
- [A Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Az Azure Service háló fürt futtat, esetén célszerű az egy központi helyen összes csomópontjának naplógyűjtés. A naplók kellene egy központi helyen segítségével elemezheti és a fürt, vagy az alkalmazások és szolgáltatások fürt fut a problémák elhárítása.

Töltse fel és naplógyűjtés egy úgy, hogy az Azure diagnosztika bővítménnyel feltöltések megjelenítése az Azure-tárolóhoz naplók. A naplók sem közvetlenül a tárhely, amely hasznos lehet. De olvassa el az események tárhelyről, és helyezze el őket a termék például [Napló Analytics](../log-analytics/log-analytics-service-fabric.md), [Rugalmas keresési](service-fabric-diagnostic-how-to-use-elasticsearch.md)vagy egy másik napló elemzése megoldás használhatja egy külső folyamat.

## <a name="prerequisites"></a>Előfeltételek
Ezek az eszközök kell használni a dokumentumban hajtsa végre a műveleteket:

* [Azure diagnosztika](../cloud-services/cloud-services-dotnet-diagnostics.md) (Azure Felhőbeli szolgáltatásokhoz kapcsolódó, de rendelkezik a megfelelő információkkal és példákkal)
* [Azure erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](../powershell-install-configure.md)
* [Azure erőforrás-kezelő ügyfél](https://github.com/projectkudu/ARMClient)
* [Erőforrás-kezelő Azure-sablon](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Log források, érdemes lehet összegyűjtése
- **Szolgáltatás háló naplók**: szabványos eseményvezérelt Tracing a Windows (esemény-nyomkövetés) és EventSource csatornák a platform által kibocsátott. Naplók többféle egyike lehet:
  - Műveleti események: a szolgáltatás háló platform végrehajtott műveletekhez naplók. Az alkalmazások és szolgáltatások, csomópont állapotának megváltozásakor és a frissítési információk létrehozásának többek között.
  - [Megbízható szereplők modell események programozása](service-fabric-reliable-actors-diagnostics.md)
  - [Megbízható szolgáltatások modell események programozása](service-fabric-reliable-services-diagnostics.md)
- **Alkalmazás események**: a szolgáltatáskód által kibocsátott és a Visual Studio sablonok leírt EventSource segítő osztály használatával írt eseményeket. A hatékony szövegalkotás naplók a levelezőprogramból további tudnivalókért lásd: [Monitor egy helyi számítógép zónában fejlesztési beállítása a services diagnosztizálása és](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>A diagnosztikai bővítmény telepítése
Az első lépés a naplók gyűjteni, hogy az egyes a VMs a szolgáltatás háló fürt a diagnosztika bővítmény telepítése. A diagnosztikai bővítmény gyűjti össze a naplók a minden virtuális, és feltöltések megjelenítése az őket a tárterület-fiókjába, megadott feltételeknek. A lépések kissé alapján hogy használja-e az Azure portálja vagy az erőforrás-kezelő Azure változnak. A lépéseket is függően változnak, hogy a telepítés fürt létrehozásának része, vagy fürt, amely már létezik az. Lássuk a lépéseket minden esetben.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>A diagnosztikai bővítmény fürt létrehozása a portálon keresztül részeként terjesztése
A diagnosztikai bővítmény telepítése a VMs a fürt fürt létrehozása részeként, az alábbi képen látható, a diagnosztikai beállítások panel kell használnia. Ahhoz, hogy megbízható szereplők vagy megbízható szolgáltatások esemény webhelycsoport, győződjön meg arról, hogy diagnosztika értéke **a** (alapértelmezett beállítás). Miután létrehozta a fürt, ezeket a beállításokat a portál használatával nem módosítható.

![Azure diagnosztika beállításainak fürt elrejtésével a portálon](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Az Azure támogatási csapat *igényel* támogatási naplózza bármely támogatási kérelmek létrehozott megoldásához. Ezeket a naplókat gyűjtve valós idejű, és a tárhely fiókokra erőforráscsoport létrehozott tárolja. Alkalmazásszintű események a diagnosztika beállításainak konfigurálása Az esemény hozzáadása a [Megbízható szereplők](service-fabric-reliable-actors-diagnostics.md) események, [Megbízható Services](service-fabric-reliable-services-diagnostics.md) -eseményekre és néhány rendszer szintű szolgáltatás háló esemény Azure tárolója tárolja.

Terméktől, például a [Rugalmas keresés](service-fabric-diagnostic-how-to-use-elasticsearch.md) vagy a saját folyamat elérheti az események a tárterület-fiókból. Jelenleg nincs lehetőség szűrve, vagy az események, a táblázat küldött tisztogassák. Ha egy folyamat események eltávolítása a táblázat nem alkalmazza, a táblázat folytatja a nagyobb.

A csoport létrehozásakor a portál használatával erősen ajánlott letöltése a sablon *előtt, kattintson * *az OK*** a fürt létrehozásához. A részletekért olvassa el az [állítsa be a szolgáltatás háló fürtre egy erőforrás-kezelő Azure-sablon segítségével](service-fabric-cluster-creation-via-arm.md). Mivel a portál használatával nem lehet néhány módosítani kell a gyűjteményt, hogy végezze el a módosításokat később.

Sablonok a portálon az alábbi lépésekkel exportálhatja. Ezek a sablonok azonban használni, mivel előfordulhat, hogy biztosítanak-null értékeket, amelyekhez nem tartozik a szükséges információkat nehezebben lehet.

1. Nyissa meg az erőforráscsoport.
2. Kattintson a **Beállítások** a beállítások panel megjelenítéséhez.
3. Jelölje ki a **környezetekben** a telepítési előzmények ablaktábla megjelenítéséhez.
4. Jelölje ki a üzembe helyezési a részletek megjelenítéséhez.
5. Jelölje be a **Sablon exportálása** a sablonok munkaablak megjelenítéséhez.
6. Válassza a **fájl mentése** a sablont, a paraméter és a PowerShell-fájlokat tartalmazó .zip fájl exportálása.

Exportálja a fájlokat, miután kell a módosítások. Szerkessze a parameters.json fájlt, és távolítsa el az **adminPassword** elemet. Ennek hatására a figyelmeztető üzenet a jelszót, a telepítési parancsfájlt futtatásakor. Amikor futtat, a telepítési parancsfájlt, lehet, ha a paraméter null értékek kijavítása.

A letöltött sablont használni a konfiguráció frissítése:

1. Mappa helyi számítógépen való kibontásához.
2. Módosítsa az új konfiguráció megfelelően tartalmát.
3. Indítsa el a PowerShell, és módosítsa a mappát, ahová a tartalmat.
4. Futtassa a **deploy.ps1** , és töltse ki az előfizetés azonosítója, a csoport erőforrásnév (ugyanaz a neve, a beállítások frissítése kell használni) és egyedi telepítési nevét.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Fürt létrehozása részeként a diagnosztika bővítmény telepítése Azure erőforrás-kezelő használatával
Erőforrás-kezelővel fürt létrehozásához kell a fürt létrehozása előtt vegye fel a diagnosztikai konfigurációja JSON a teljes fürt erőforrás-kezelő sablonba. Az erőforrás-kezelő sablon minták részeként hozzáadott diagnosztika konfigurációs öt-virtuális fürt erőforrás-kezelő űrlapsablonok biztosítunk. Ezen a helyen Azure minták gyűjteményben lévő lásd: [öt-csomópont fürt diagnosztika erőforrás-kezelő sablon mintával](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Az erőforrás-kezelő sablon diagnosztika beállítás megtekintéséhez nyissa meg a azuredeploy.json fájl- és **IaaSDiagnostics**keresése. Fürt létrehozása a sablon használatával, jelölje ki a elérhető az előző hivatkozásra **az Azure Deploy** gombot.

Azt is megteheti, letöltheti az erőforrás-kezelő minta, fel, és fürt létrehozása a módosított sablon használatával a `New-AzureRmResourceGroupDeployment` egy Azure PowerShell-ablak parancsot. Lásd: a parancs a sikeres paraméterek a következő kódot. A cikke [Deploy az erőforrás-kezelő Azure-sablon erőforráscsoport](../resource-group-template-deploy.md)erőforráscsoport telepítéséről a PowerShell használatával részletes tájékoztatást.

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>A diagnosztikai bővítmény telepítése a meglévő fürthöz
Ha nincs telepítve diagnosztika meglévő fürthöz van, vagy létező konfiguráció módosítani szeretné, felvehet, és frissíteni. Módosítsa a erőforrás-kezelő sablont, amely a meglévő fürt létrehozása, és töltse le a sablont a portálról ismertetett módon. Az alábbi feladatok elvégzéséhez módosítani a template.json fájlt.

Új tároló erőforrás hozzáadni a sablonhoz a források részben hozzáadásával.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Ezután hozzáadása a paraméterek szakasz csak a tárterület-fiók definíciók után között `supportLogStorageAccountName` és `vmNodeType0Name`. Cserélje le a helyőrző szöveg *fióknév tárolási helye* a tárterület-fiók nevére.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Frissítse a `VirtualMachineProfile` szakaszt a következő kódot a bővítmények tömbön belüli hozzáadásával template.json fájl. Ügyeljen arra, hogy egy vesszőt hozzáadása elején vagy végén, attól függően, hogy hol vannak beszúrva.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Miután leírtak szerint módosítsa a template.json fájl, tegye közzé újra az erőforrás-kezelő sablon. Ha a sablon lett exportált, a deploy.ps1 fájl futtatása addig a sablont. Miután telepít, győződjön meg arról, hogy **ProvisioningState** **sikeres**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Frissítse a diagnosztika összegyűjtése és az új EventSource csatornák naplók feltöltése
A diagnosztikai naplók gyűjt a új EventSource csatornák megjelenítő, amelyre telepíteni, hajtsa végre ugyanezeket a lépéseket, ahogy az [előző szakaszban](#deploywadarm) egy meglévő fürt diagnosztika telepítéshez új alkalmazás frissítéséhez.

Frissítse a `EtwEventSourceProviderConfiguration` szakasz template.json fájl hozzáadása a bejegyzéseket, ha a konfigurációs alkalmazása előtt új EventSource csatornák frissítése használatával a `New-AzureRmResourceGroupDeployment` PowerShell-parancsot. A forrás neve a Visual Studio által generált ServiceEventSource.cs fájlban a kód részeként történik.

Ha a forrás neve saját Eventsource, például adja meg a következő kódot szeretne helyezni a saját Eventsource eseményeinek MyDestinationTableName nevű táblába.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Teljesítmény számláló vagy eseménynaplók összegyűjtéséhez módosítsa az erőforrás-kezelő sablon létrehozása [a Windows virtuális készülék figyelés és diagnosztika egy erőforrás-kezelő Azure-sablon segítségével](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)megadott példák használatával. Ezután tegye közzé újra az erőforrás-kezelő sablon.

## <a name="next-steps"></a>Következő lépések
Részletesebb milyen események kell kinéznie közben kapcsolatos problémák elhárítása című cikkben talál részletes a diagnosztikai események kibocsátott [Megbízható szereplők](service-fabric-reliable-actors-diagnostics.md) és [Megbízható szolgáltatásokat](service-fabric-reliable-services-diagnostics.md).


## <a name="related-articles"></a>Kapcsolódó cikkek
* [Megtudhatja, hogyan gyűjthetők össze a teljesítmény számláló vagy naplók a diagnosztika bővítmény segítségével](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [A napló Analytics szolgáltatás háló megoldás](../log-analytics/log-analytics-service-fabric.md)
