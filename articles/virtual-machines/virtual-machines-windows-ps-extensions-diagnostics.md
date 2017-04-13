<properties
    pageTitle="Azure diagnosztika ahhoz, hogy egy virtuális gépen fut a Windows PowerShell-lel |} Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Megtudhatja, hogy miként Azure diagnosztika engedélyezése egy virtuális gépen fut a Windows PowerShell használatával"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Azure diagnosztika engedélyezése egy virtuális gépen fut a Windows PowerShell használatával

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure diagnosztika, amely lehetővé teszi, hogy a telepített alkalmazások diagnosztikai adatok gyűjtése Azure belül a lehetőség. Diagnosztikai adatok, például az alkalmazás naplók és a teljesítmény számláló gyűjt a az Azure virtuális gép (virtuális) Windows operációs rendszert futtató a diagnosztika kiterjesztést is használhatja. Ez a cikk ismerteti a Windows PowerShell-lel való a diagnosztika bővítmény engedélyezése egy virtuális. Megtudhatja, [hogy miként telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md) a szükséges ez a cikk a előfeltételeihez.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>A diagnosztikai bővítmény engedélyezése, ha használja az erőforrás-kezelő telepítési modell

A diagnosztikai bővítmény engedélyezheti a bővítmény konfiguráció ad hozzá az erőforrás-kezelő sablon létrehozása a Windows virtuális keresztül az Azure erőforrás-kezelő telepítési modell közben. Lásd: [létrehozása a Windows virtuális készülék figyelés és diagnosztika az erőforrás-kezelő Azure-sablon segítségével](virtual-machines-windows-extensions-diagnostics-template.md).

Ha engedélyezni szeretné a diagnosztika bővítmény meg egy meglévő virtuális, az erőforrás-kezelő telepítési modell keresztül létrehozott, a [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShell-parancsmag alább látható módon is használhatja.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* , amely tartalmazza az XML-ben, a diagnosztikai konfigurációja, ahogy az alábbi [Példa](#sample-diagnostics-configuration) a fájl elérési útját.  

Ha a diagnosztika konfigurációs fájl egy **StorageAccount** elemet tároló fiók néven határozza meg, majd a *Set-AzureRMVMDiagnosticsExtension* parancsfájl automatikusan beállítja a diagnosztika bővítmény diagnosztikai adatok küldése a fiókhoz társított tároló. Ez a munkát a tárterület-fiókot kell lennie ugyanabban az előfizetésben, mint a virtuális.

Ha nincs **StorageAccount** a diagnosztika konfigurációban megadott, majd meg kell átadni a parancsmagot a *StorageAccountName* paraméter. Ha a *StorageAccountName* paramétert ad meg, majd a parancsmag mindig használja a tárterület-fiókot a paraméterben megadott, és nem a megadott a diagnosztika konfigurációs fájl.

Ha a diagnosztika tárterület-fiókot a virtuális a másik előfizetést, majd szeretne kifejezetten átadni a *StorageAccountName* és *StorageAccountKey* paraméterek parancsmag. A *StorageAccountKey* paraméter nincs szükség esetén a diagnosztika tároló fiók ugyanabban az előfizetésben, a parancsmag automatikusan a lekérdezés és a kulcs értékét állítja be, amikor a diagnosztika bővítmény engedélyezésével. Azonban egy másik előfizetést a diagnosztika tárterület-fiók esetén a parancsmag lehet nem tudja a kulcs letöltésére, majd kell adnia a *StorageAccountKey* paraméterrel billentyűt.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

A diagnosztikai bővítmény engedélyezve van egy virtuális, miután a [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) parancsmaggal elérheti a jelenlegi beállításokat.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

A parancsmaggal *PublicSettings*, amely tartalmazza az XML-konfiguráció Base64 kódolású formátumban adja eredményül. Az XML olvasásához kell dekódolását azt.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Az [Eltávolítás-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) parancsmagot a diagnosztika bővítmény eltávolítása a virtuális használható.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>A diagnosztikai bővítmény engedélyezése, ha a a klasszikus telepítési modell használata

A [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) parancsmag ahhoz, hogy a egy virtuális, amelyet a klasszikus telepítési modell keresztül létrehozhat egy diagnosztika bővítmény is használhatja. A következő példa bemutatja, hogyan hozhat létre egy új virtuális keresztül a klasszikus telepítési modell a diagnosztika kiterjesztésű engedélyezve van.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Ahhoz, hogy a diagnosztika bővítmény meg egy meglévő virtuális a klasszikus telepítési modell keresztül létrehozott, először segítségével a [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) parancsmagot a virtuális konfiguráció első. Frissítse a virtuális konfiguráció tartalmazza a diagnostics kiterjesztése a [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) parancsmag használatával. Végezetül alkalmazása a frissített konfiguráció a virtuális [Frissítés-AzureVM](https://msdn.microsoft.com/library/mt589121.aspx)használatával.

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Minta diagnosztikai konfigurációja

A következő XML a diagnosztikai nyilvános konfigurációja, a fenti parancsfájlok használható. Ez a példa konfiguráció különböző teljesítmény számláló átvitele diagnosztika tárhely a fiókot, az alkalmazás, biztonságot és a Windows-eseménynaplók rendszer csatornák eredő hibák és a diagnosztikai infrastruktúra naplók a hibákat.

A konfiguráció kell frissíteni kell a következők lehetnek:

- A **Mértékek** elemet *resourceID* attribútumának a virtuális az erőforrás-azonosító frissítésre szorul.
    - Az erőforrás-azonosító is alakítható ki a következő mintát használatával: "/resourceGroups//előfizetések / {*az előfizetés a virtuális az előfizetés azonosítója*:} {*resourcegroup nevét a virtuális*} / {*virtuális neve*} providers/Microsoft.Compute/virtualMachines/".
    - Például ha az előfizetés, amelyen a virtuális fut az előfizetés azonosítója **11111111-1111-1111-1111-111111111111**, az erőforrás-csoport nevét az erőforráscsoport **MyResourceGroup**, és a virtuális nevem **MyWindowsVM**, majd *resourceID* értékét a következő lesz:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - További információt olvashat mértékek jönnek létre a teljesítmény számláló és mérőszámok konfigurációjának [tárolási mértékek táblázat Azure diagnosztika](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage)találhat.

- Az **StorageAccount** elemet kell frissíteni kell a diagnosztika tároló fiók nevére.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Következő lépések
- További használatáról az Azure diagnosztika lehetőséget, és más technikákkal hibáinak elhárítása című témakörben olvashat [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Diagnosztikai beállítások séma](https://msdn.microsoft.com/library/azure/mt634524.aspx) diagnosztika bővítményhez különböző XML konfigurációk lehetőségeket ismerteti.
