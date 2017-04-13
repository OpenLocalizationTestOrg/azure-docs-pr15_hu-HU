<properties
    pageTitle="A PowerShell használatával beállítási alkalmazás háttérismeretek az Azure-ban |} Microsoft Azure"
    description="Azure diagnosztika beállítás a függőleges vonás alkalmazás mélyebb automatizálása"
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Az Azure web app alkalmazás háttérismeretek beállítása a PowerShell használatával

[Microsoft Azure](https://azure.com) lehet [Azure diagnosztika küldésére beállított](app-insights-azure-diagnostics.md) [Visual Studio alkalmazásban az összefüggéseket](app-insights-overview.md). A diagnosztikai Azure Cloud Services és Azure VMs vonatkoznak. A telemetriai, amely az alkalmazásból, az alkalmazás az összefüggéseket SDK használatával küldhessenek kiegészíti. A folyamat, új erőforrások létrehozása az Azure automatizálása részeként diagnosztika PowerShell használatával is beállíthatja.

## <a name="azure-template"></a>Azure-sablon

Ha a web app Azure-ban és az erőforrások, erőforrás-kezelő Azure-sablon segítségével létrehozott, a alkalmazás háttérismeretek Ez az erőforrások csomópontra hozzáadásával is beállíthatja:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`– az alkalmazás az összefüggéseket erőforrás nevét
* `myWebAppName`-a web app azonosítója


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Egy felhőalapú szolgáltatás üzembe helyezése részeként diagnosztika bővítmény engedélyezése

A `New-AzureDeployment` parancsmag paramétere `ExtensionConfiguration`, amely megnyitja a diagnosztikai beállítások tömb. Ezek hozhatók létre az `New-AzureServiceDiagnosticsExtensionConfig` parancsmag. Példa:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Egy meglévő felhőalapú szolgáltatást a diagnosztika bővítmény engedélyezése

Egy meglévő szolgáltatást használja `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Ismerkedés a jelenlegi diagnosztika bővítmény konfiguráció

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Diagnosztikai bővítmény eltávolítása

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Ha engedélyezte a diagnosztika kiterjesztése a következők egyikét használja `Set-AzureServiceDiagnosticsExtension` vagy `New-AzureServiceDiagnosticsExtensionConfig` a szerepkör paramétert, akkor távolíthatja el a bővítmény használatával `Remove-AzureServiceDiagnosticsExtension` a szerepkör paraméter nélkül. Ha a szerepkör paraméter használta, amikor a bővítmény engedélyezésével majd meg kell is használható, ha a bővítmény eltávolítása.

A diagnosztikai bővítmény eltávolítása minden egyes szerepkör:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Lásd még:

* [Azure figyelése Cloud Services alkalmazás háttérismeretek az alkalmazások](app-insights-cloudservices.md)
* [Azure diagnosztika küldése alkalmazás mélyebb](app-insights-azure-diagnostics.md)
* [Értesítések beállítása automatizálása](app-insights-powershell-alerts.md)

