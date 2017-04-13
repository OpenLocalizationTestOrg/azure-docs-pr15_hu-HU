<properties
    pageTitle="Diagnosztikai a PowerShell használatá Azure Cloud Services engedélyezése |} Microsoft Azure"
    description="Megtudhatja, hogy miként diagnosztika PowerShell használatával cloud Services engedélyezése"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Diagnosztikai a PowerShell használatá Azure Cloud Services engedélyezése

Diagnosztikai adatok, például az alkalmazás a naplók összegyűjtheti teljesítmény számláló egy felhőalapú szolgáltatásba, az Azure diagnosztika kiterjesztés használatával a stb. Ez a cikk ismerteti az Azure diagnosztika bővítmény engedélyezése egy felhőalapú szolgáltatásba, PowerShell használatával.  Megtudhatja, [hogy miként telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md) a szükséges ez a cikk a előfeltételeihez.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Egy felhőalapú szolgáltatás üzembe helyezése részeként diagnosztika bővítmény engedélyezése

Ez a módszer a jó folyamatos integrációs típusú esetek, ahol a diagnosztika bővítmény engedélyezése üzembe helyezése a felhőbeli szolgáltatástól részeként. Amikor hoz létre egy új, felhőalapú szolgáltatás telepítési engedélyezheti a diagnosztika bővítmény átadása a *ExtensionConfiguration* paraméterben a [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) parancsmag céljából. A *ExtensionConfiguration* paraméter tömb lehet a diagnosztikai beállítások a [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) parancsmaggal hozható létre.

A következő példa bemutatja, hogyan engedélyezheti egy felhőalapú szolgáltatás WebRole és WorkerRole diagnosztika mindegyik a másik diagnosztika konfiguráció problémákat.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Ha a diagnosztika konfigurációs fájl egy StorageAccount elemet tároló fiók néven határozza meg, majd az új-AzureServiceDiagnosticsExtensionConfig parancsmag automatikusan tároló fiók fogja használni. Ez a munkát a tárterület-fiókot kell lennie ugyanabban az előfizetésben, mint a felhőalapú szolgáltatás üzembe helyezéséhez.

Azure SDK 2.6 kezdve a MSBuild által generált konfigurációs bővítményfájlok tegye közzé a kimenet tartalmazni fogja a tárhely fióknév diagnosztika konfigurációs karakterláncban meg a szolgáltatás konfigurációs fájl (.cscfg) alapján. Az alábbi parancsfájl bemutatja, hogyan elemzi a közzététel a kimenet a bővítmény konfigurációs fájl, és állítsa be a minden szerepkörhöz diagnosztika bővítmény a felhőalapú szolgáltatás telepítésekor.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

A diagnosztikai kiterjesztésű a Felhőszolgáltatásba automatikus deploymnts Visual Studio Online használja egy hasonló módon. [Közzététel-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) lásd: a teljes példa.

Ha nincs StorageAccount a diagnosztika konfigurációban megadott, majd meg kell átadni a parancsmagot a StorageAccountName paraméter. Ha a StorageAccountName paramétert ad meg, majd a parancsmag mindig használja a tárterület-fiókot, amely a paraméter, és nem egy a diagnosztika konfigurációs fájl a megadott érték van megadva.

Ha a diagnosztika tárterület-fiókot a Felhőbeli szolgáltatástól másik előfizetést, majd szeretne kifejezetten átadni a StorageAccountName és StorageAccountKey paraméterek parancsmag. A StorageAccountKey paraméter nincs szükség esetén a diagnosztika tároló fiók ugyanabban az előfizetésben, a parancsmag automatikusan a lekérdezés és a kulcs értékét állítja be, amikor a diagnosztika bővítmény engedélyezésével. Azonban egy másik előfizetést a diagnosztika tárterület-fiók esetén a parancsmag lehet nem tudja a kulcs letöltésére, majd kell adnia a StorageAccountKey paraméterrel billentyűt.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Egy meglévő felhőalapú szolgáltatást a diagnosztika bővítmény engedélyezése

A [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) parancsmag engedélyezése és frissítheti a egy felhőalapú szolgáltatásba, amely már fut a diagnosztikai konfigurációja is használhatja.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Ismerkedés a jelenlegi diagnosztika bővítmény konfiguráció
A [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) parancsmagot segítségével az aktuális diagnosztika adatokat egy felhőalapú szolgáltatásba.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Diagnosztikai bővítmény eltávolítása
Kapcsolja ki a egy felhőalapú szolgáltatásba diagnosztika [Eltávolítása-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) parancsmag is használhatja.

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Ha engedélyezte a *Set-AzureServiceDiagnosticsExtension* vagy a *szerepkör* paraméter nélkül a *New-AzureServiceDiagnosticsExtensionConfig* diagnosztika bővítmény majd eltávolíthatja a bővítmény *Eltávolítása-AzureServiceDiagnosticsExtension* használata a *szerepkör* paraméter nélkül. Ha a *szerepkör* paraméter használta, amikor a bővítmény engedélyezésével majd meg kell is használható, ha a bővítmény eltávolítása.

A diagnosztikai bővítmény eltávolítása minden egyes szerepkör:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Következő lépések

- További használatáról az Azure diagnosztikai és más technikákkal hibáinak elhárítása című témakörben olvashat [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](cloud-services-dotnet-diagnostics.md).
- A [Diagnosztikai konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) diagnosztika bővítményhez különböző xml konfigurációk lehetőségeket ismerteti.
- Megtudhatja, hogy miként engedélyezhet a diagnosztika bővítmény virtuális gépeken futó, lásd: a [Figyelés és Azure erőforrás-kezelő sablonnal diagnosztika Windows virtuális készülék létrehozása](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
