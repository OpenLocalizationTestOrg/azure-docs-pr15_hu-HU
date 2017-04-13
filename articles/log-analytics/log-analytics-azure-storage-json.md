<properties
    pageTitle="Azure a diagnosztikai naplók napló Analytics segítségével elemezheti |} Microsoft Azure"
    description="Log Analytics erről a naplókat Azure blob-tároló JSON formátumban diagnosztikai naplók írása Azure szolgáltatásból."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Log Analytics használatával Azure a diagnosztikai naplók elemzése

Log Analytics gyűjthet a naplókat, írja be [a diagnosztikai naplók Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) blob-tárolóhoz JSON formátumban a következő Azure szolgáltatások:

+ Automatizálási (előzetes verzió)
+ Fő tárolóból elemre (előzetes verzió)
+ Alkalmazás átjáró (előzetes verzió)
+ Hálózati biztonsági csoport (előzetes verzió)

Az alábbi szakaszok végigvezetik Önt a PowerShell használatával:

+ Az egyes erőforrásokhoz tárhelyről naplógyűjtés napló elemzést konfigurálása  
+ A napló Analytics megoldás az Azure szolgáltatás engedélyezése

Log Analytics összegyűjtheti ezek az erőforrások adatait, mielőtt Azure diagnosztika engedélyezve kell lennie. Használja a `Set-AzureRmDiagnosticSetting` parancsmag naplózás engedélyezéséhez.

Olvassa el az alábbi cikkekben további információt a diagnosztikai naplózás engedélyezése:

+ [Fő tárolóból elemre](../key-vault/key-vault-logging.md)
+ [Alkalmazás átjáró](../application-gateway/application-gateway-diagnostics.md)
+ [Hálózati biztonsági csoport](../virtual-network/virtual-network-nsg-manage-log.md)

Ez a dokumentáció is tartalmaz adatokat is meg:

+ Hibaelhárítási adatok gyűjtése
+ Adatgyűjtés leállítása

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Log Analytics gyűjthetők össze az Azure a diagnosztikai naplók konfigurálása

Naplók szolgáltatásokra gyűjteni, és a megoldás a naplókat ábrázolásához engedélyezése PowerShell-parancsfájlokat használatával történik.

Az alábbi példában lehetővé teszi, hogy bejelentkezik az összes támogatott források

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Az egyes erőforrásokhoz akkor a fenti konfiguráció az Azure portálról a [diagnosztikai naplók áttekintése az Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)ismertetett lépések végrehajtásához.

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Log Analytics gyűjthetők össze az Azure a diagnosztikai naplók konfigurálása

PowerShell parancsprogramot modul, amely a két parancsmagok napló Analytics konfigurálása megkönnyítése exportálja nyújtott:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`beviteli kéri, és tudja beállítani az egyszerű konfigurációk
2. `Add-AzureDiagnosticsToLogAnalytics`a Lync-bemeneteként erőforrások tart, és majd beállítja a napló Analytics  

### <a name="pre-requisites"></a>Előzetes követelmények

1. Azure PowerShell 1.0.8 verzióval vagy újabb verziót a működési háttérismeretek parancsmagok.
  - [Telepítse és állítsa be a Azure PowerShell hogyan](../powershell-install-configure.md)
  - A parancsmagok verziójától ellenőrzése:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnosztikai naplózás az Azure erőforrás figyelni kívánt van beállítva. Használat `Set-AzureRmDiagnosticSetting` vagy [Használata napló Analytics Azure tároló fiókokból adatokat szeretne gyűjteni](log-analytics-azure-storage.md) , hogyan engedélyezheti a diagnosztika hivatkozik.
3. [Log Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) -munkaterület  
4. A AzureDiagnosticsAndLogAnalytics PowerShell-modult
  - Töltse le a [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) modul PowerShell-dokumentumtárból

### <a name="option-1-run-the-interactive-configuration-scripts"></a>1 a beállítás: A interaktív konfigurációs parancsfájlok futtatása

Nyissa meg a PowerShell és futtatásával:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Rendelkezésre álló beállítások listája látható és adni egy kérdés, hogy a kívánt beállítást.
Program megkérdezi, hogy az egyes az alábbi beállításokat:

+ Erőforrás típusa (Azure szolgáltatások) a naplógyűjtés
+ Erőforrás-példányokat a naplógyűjtés
+ Jelentkezzen be az adatokat szeretne gyűjteni Analytics-munkaterület

A parancsfájl futtatása után meg kell jelennie napló Analytics rekordjaihoz körülbelül 30 perc után az új diagnosztikai adatok tárolóhoz íródott. Ha a rekordok nem érhetők el, miután ez esetben olvassa el az alábbi hibaelhárítási szakaszra.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>: 2 összeállítása: erőforrások listája, és adja meg azokat a konfigurációs parancsmag

Információforrások, amelyek Azure diagnosztika engedélyezve van, és kattintson az erőforrások átadni a konfigurációs parancsmag listájának készíthet.

További információ a parancsmag futtatásával látható `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


További részletek kereshetők MOBILE [Napló Analytics PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Ha erőforrás és munkaterületek különböző Azure előfizetések, válthat közöttük segítségével szükség szerint`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
A parancsfájl futtatása után meg kell jelennie napló Analytics rekordjaihoz körülbelül 30 perc után az új diagnosztikai adatok tárolóhoz íródott. Ha a rekordok nem érhetők el, miután ez esetben olvassa el az alábbi hibaelhárítási szakaszra.  

>[AZURE.NOTE] A konfiguráció nem láthatók, az Azure-portálon. Konfiguráció használata ellenőrizheti a `Get-AzureRmOperationalInsightsStorageInsight` parancsmag.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Log Analytics leállítása a gyűjtő Azure a diagnosztikai naplók

A napló Analytics konfiguráció egy erőforrás törléséhez használja a `Remove-AzureRmOperationalInsightsStorageInsight` parancsmag.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Azure a diagnosztikai naplók hibaelhárítási konfigurációja

*A probléma*

Klasszikus tároló bejelentkezik egy erőforrás beállításakor a következő hiba jelenik meg:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Megoldás*

Jelentkezzen be az API-t a klasszikus telepítési modell`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Az Azure a diagnosztikai naplók hibaelhárítási adatok gyűjtése

Ha az adatok nem láthatók a napló Analytics Azure erőforrás, használhatja az alábbi hibaelhárítási lépéseket:

+ Adatok halad a tárterület-fiók ellenőrzése
+ Ellenőrizze a szolgáltatás a napló Analytics megoldást engedélyezve van
+ Ellenőrizze, hogy napló Analytics olvasható tárhelyről
+ A tároló fiókkulcs frissítése

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Adatok halad van a tárterület-fiók ellenőrzése

Ellenőrizheti, hogy ha adatok halad-e a tárterület-fiókjába, egy eszközzel, amely lehetővé teszi, hogy a böngészési Azure tárhelyek (például a Visual Studio) vagy a PowerShell használatával.

Keresse meg a tárterület-fiókot az erőforrás jelentkezzen be az alábbi PowerShell-lel való kell megadni:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

A Azure diagnosztika által használt tárterület tároló van a formázott neve:  

`insights-logs-<Category>`

Keresse meg a [tárterület-parancsmagok használata jelölje be a legutóbbi adatokat tároló](../storage/storage-powershell-guide-full.md) további információk a tárterület-fiók a tartalom megtekintése.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Ellenőrizze a szolgáltatás a napló Analytics megoldást engedélyezve van

Ha `Add-AzureDiagnosticsToLogAnalyticsUI`, a helyes napló Analytics-megoldás esetén automatikusan aktiválja azt.

Annak ellenőrzéséhez, ha engedélyezve van-e a probléma megoldásán, futtassa az alábbi PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Ha a megoldás nincs engedélyezve, engedélyezheti újra az alábbi PowerShell használatával:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Keresse meg a ahhoz, hogy az egyes erőforrások: a megoldás, használja az alábbi PowerShell (Ez a változó érhető el a modul importálását követően):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Ellenőrizze, hogy napló Analytics olvasható tárhelyről

Ha további Azure erőforrások ad hozzá, kell a diagnosztikai naplózás őket a engedélyezése és beállítása a napló Analytics-őket.
Jelölje be a napló Analytics úgy van konfigurálva, hogy az naplógyűjtés erőforrásainak és tároló fiókok, használja az alábbi PowerShell:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Frissítés a tárhely billentyűt

Ha módosítja a tárterület-fiókom a kulcsot, a napló Analytics konfiguráció kell az új kulcs frissülni.
A napló Analytics konfiguráció frissítheti az alábbi PowerShell használatával új használatával:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Keresse meg a tárhely betekintést, használja a `Get-AzureRmOperationalInsightsStorageInsight` parancsmag, korábbi példa látható módon.

## <a name="next-steps"></a>Következő lépések

- [Használata blob-tárolóhoz IIS és táblatároló eseményekre vonatkozóan](log-analytics-azure-storage-iis-table.md) olvassa el a naplók az Azure táblatároló vagy IIS naplók blob-tároló írt e írási diagnosztika-szolgáltatások.
- [Megoldások engedélyezése](log-analytics-add-solutions.md) az adatok betekintést nyújt.
- [Használja a keresési lekérdezések](log-analytics-log-searches.md) hozzáadva elemezheti az adatokat.
