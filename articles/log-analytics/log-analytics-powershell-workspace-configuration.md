<properties
    pageTitle="PowerShell-lel való létrehozása és konfigurálása a napló Analytics munkaterület |} Microsoft Azure"
    description="Analytics-használati adatok jelentkezzen kiszolgálókról a helyszíni, vagy a felhő infrastruktúra. Gépi adatgyűjtés Azure diagnosztika létrehozásakor az Azure-tárhelyről."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Log Analytics PowerShell használatával kezelése

Használhatja a [Log Analytics PowerShell-parancsmagok] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) végrehajtásához különböző függvényekre napló Analytics, a parancssorból vagy parancsfájlt részeként.  A feladatokat végezheti el a PowerShell példája:

+ Munkaterület létrehozása
+ Hozzáadása vagy eltávolítása a probléma megoldásán
+ Importálás és exportálás mentett keresések
+ Egy számítógép csoport létrehozása
+ A Windows agent az IIS naplók számítógépekről gyűjteménye engedélyezése
+ Számítógépeken Linux és a Windows teljesítmény számláló összegyűjtése
+ Syslog Linux számítógépeken eseményeinek összegyűjtése 
+ Események gyűjt a Windows-eseménynaplók
+ Egyéni események naplógyűjtés
+ A log analytics agent hozzáadása egy Azure virtuális gépen
+ Log analytics Azure diagnosztika eszközzel gyűjtött adatok indexeléséhez konfigurálása


Ebben a cikkben két mintakódok, amelyek bemutatják a PowerShell a végezheti el függvényeket részét.  A hivatkozott [Log Analytics PowerShell parancsmaggal hivatkozás] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) más funkciókhoz.

> [AZURE.NOTE] Log Analytics műveleti hírcsatornájában, amely Miért célszerű az nevét a parancsmagok a korábban hívták meg.

## <a name="prerequisites"></a>Előfeltételek

A PowerShell használata a napló Analytics-munkaterület, kell rendelkeznie:

+ Egy Azure-előfizetést, és 
+ Az Azure napló Analytics munkaterület csatolja az Azure-előfizetéséhez.

Ha létrehozott egy MOBILE munkaterület, de azt nem még kapcsolódik, az Azure előfizetéssel hozhat létre a hivatkozást:

+ Az Azure-portálon
+ A MOBILE portálon vagy 
+ A Get-AzureRmOperationalInsightsLinkTargets és a New-AzureRmOperationalInsightsWorkspace parancsmag használatával.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Létrehozása és konfigurálása a napló Analytics-munkaterület

A következő parancsfájl példa azt mutatja be, hogy miként:

1.  Munkaterület létrehozása
2.  A rendelkezésre álló megoldások listája
3.  A munkaterület megoldások hozzáadása
4.  Mentett importálási keresések
5.  Mentett exportálás keresések
6.  Egy számítógép csoport létrehozása
7.  A Windows agent az IIS naplók számítógépekről gyűjteménye engedélyezése
8.  Logikai lemez perf számláló összegyűjtése Linux számítógépekről (% használt Inodes; Ingyenes megabájt; % Használt terület; / Mp átvitelek; / Mp Olvasás; Lemezre írása/sec)
9.  Syslog események összegyűjtése Linux számítógépekről
10. Hiba és figyelmeztetés események az alkalmazás eseménynaplójában Windows számítógépekről összegyűjtése
11. A memória rendelkezésre álló memória teljesítmény számláló gyűjt a Windows rendszerű számítógépeken
12. Egyéni naplózási összegyűjtése 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Log Analytics Azure diagnosztika indexelendő konfigurálása 

Azure erőforrások agentless figyelemmel kísérése, az erőforrások kell van engedélyezve, és írja be a tárterület-fiókjába konfigurált Azure diagnosztika. Log Analytics majd beállítható úgy, hogy a naplógyűjtés a tárterület-fiókból. Információforrások, amelyek kell tennie a fenti konfiguráció tartalmazza:

+ Klasszikus felhőszolgáltatások (webes és dolgozó szerepkörök)
+ A szolgáltatás háló fürt
+ Hálózati biztonsági csoportok
+ Főbb tárolókban és 
+ Alkalmazás átjárók

A PowerShell használatával napló Analytics munkaterület beállítása egy másik Azure előfizetésekről naplógyűjtés Azure előfizetés.

Az alábbi példában látható útmutató:

1.  A meglévő tárterület-fiókokat és a napló Analytics adatainak fog index helyek lista
2.  Tárterület-fiókból olvasható konfiguráció létrehozása
3.  Frissítse az újonnan létrehozott beállításokat adatait indexelni további helyekről
4.  Az újonnan létrehozott konfiguráció törlése

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Következő lépések

- [Véleményezés napló Analytics PowerShell-parancsmagok](http://msdn.microsoft.com/library/mt188224.aspx) konfigurálni a napló Analytics PowerShell használatáról további információt.

