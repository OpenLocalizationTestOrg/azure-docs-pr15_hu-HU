<properties
    pageTitle="Rövid útmutató az Azure Monitor PowerShell minták. | Microsoft Azure"
    description="Azure Monitor szolgáltatások, például Automatikus méretezéssel, értesítések, webhooks és keresés tevékenység naplók elérése a PowerShell használatával."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure Monitor PowerShell rövid útmutató az első minták

Ez a példa a segítségével elérheti az Azure Monitor szolgáltatások PowerShell-parancsait cikk mutatja. Azure Monitor teszi Automatikus méretezéssel Felhőszolgáltatások, virtuális gépeken futó és Web Apps alkalmazások és értesítéseket küldeni, vagy felhívhatja őket webes URL-címek értékek konfigurált telemetriai adatok alapján.

>[AZURE.NOTE] Azure monitorra "Azure háttérismeretek" nevezett új nevét, amíg 2016 Sept 25. Azonban a névtér, és így az alábbi parancsok továbbra is tartalmazzák a "háttérismeretek".

## <a name="set-up-powershell"></a>Állítsa be a PowerShell
Ha még nem tette meg, állítsa be a PowerShell fusson a számítógépen. További tudnivalókért lásd: [telepítése és beállítása PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Ez a cikk példáiban

A cikkben szereplő példák bemutatják, hogyan használhatja az Azure Monitor parancsmagok. Is áttekintheti az Azure Monitor PowerShell-parancsmagok az [Azure Monitor (háttérismeretek) parancsmagok](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx)teljes listáját.


## <a name="sign-in-and-use-subscriptions"></a>Jelentkezzen be és használja az előfizetések

Első lépésként jelentkezzen be az Azure előfizetés.

```PowerShell
Login-AzureRmAccount
```

Ehhez az, hogy jelentkezzen be. Miután tesz, a fiók TenantId és alapértelmezett előfizetés azonosítója jelenik meg. Az Azure parancsmag működik az alapértelmezett előfizetés keretében. Tekintse meg az előfizetések listáját, hozzáférést, a következő parancsot használja.

```PowerShell
Get-AzureRmSubscription
```

A munkaidő-környezet másik előfizetést módosításához használja a következő parancsot.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Előfizetési naplókat beolvasása
Használja a `Get-AzureRmLog` parancsmag.  Az alábbiakban néhány hétköznapi példát.

Naplóbejegyzések beolvasása ez idő vagy dátum bemutatására:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Egy dátum/idő tartományba eső naplóbejegyzések kap:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Az adott erőforrás csoport kaphat a naplóbejegyzések:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Naplóbejegyzések első egy dátum/idő tartományba eső adott erőforrás szolgáltatóról:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Egy adott hívó az összes naplóbejegyzések kap:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

A következő parancsot a legutóbbi 1000 események beolvassa a napló:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`számos további paramétereket támogatja. Lásd: a `Get-AzureRmLog` hivatkozás további információt.

>[AZURE.NOTE] `Get-AzureRmLog`csak a 15 nap előzmény nyújt. A **- MaxEvents** paraméter használatával lehetővé teszi az utolsó N események 15 nap túl a lekérdezés. Az access események 15 napnál régebbi használja a REST API-t vagy a SDK (C# minta használatával a SDK). Ha nem szerepel a **Kezdés időpontja**, majd az alapértelmezett értéke **Befejezés időpontja** mínusz egy órával. Ha nem szerepel a **Befejezés időpontja**, az alapértelmezett érték pontos idő. Minden alkalommal UTC szerepelnek.

## <a name="retrieve-alerts-history"></a>Értesítések előzmények beolvasása
Az összes felhasználó események megtekintéséhez az Azure erőforrás-kezelő naplók, használja az alábbi példák is lekérdezhetők.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Egy adott szabályt az előzmények megtekintéséhez, használhatja a `Get-AzureRmAlertHistory` parancsmag, a szabályt az erőforrás-azonosító át.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

A `Get-AzureRmAlertHistory` parancsmag különböző paraméterek támogatja. További tudnivalókért lásd: a [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>A figyelmeztetési szabályok információk lekérése
A következő parancsok végrehajtása az összes "montest" nevű erőforráscsoport működésbe lépnek.

A szabályt minden tulajdonságainak megtekintése:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Erőforráscsoport a minden riasztást beolvasása:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Lekérdezi a cél erőforrás összes riasztási szabályok. Az összes riasztási szabályok például egy virtuális meg.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`további paramétereket támogatja. Lásd: a [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) további információt.

## <a name="create-alert-rules"></a>A figyelmeztetési szabályok létrehozása
Használhatja a `Add-AlertRule` parancsmag létrehozása, módosítása vagy egy figyelmeztetést letiltja.

Létrehozhat e-mailek és webhook tulajdonságok `New-AzureRmAlertRuleEmail` és `New-AzureRmAlertRuleWebhook`, illetve. A figyelmeztetést parancsmagot a hozzárendelése ezek a műveletek, a **Műveletek** tulajdonsága a szabályt.

A következő szakaszban arról, hogy miként hozhat létre egy szabályt különböző paraméterekkel minta tartalmazza.

### <a name="alert-rule-on-a-metric"></a>Figyelmeztetést metrikával
Az alábbi táblázat ismerteti a paraméterek és használata egy mérőszám értesítés létrehozásához használt értékeket.


|paraméter|érték|
|---|---|
|név|  simpletestdiskwrite|
|A riasztási szabály helye|   Kelet-Amerikai Egyesült Államok|
|ResourceGroup| montest|
|TargetResourceId|  /Subscriptions/S1/resourceGroups/montest/Providers/Microsoft.COMPUTE/virtualMachines/testconfig|
|A létrehozott riasztás MetricName|   \PhysicalDisk (_Total) \Disk írások/sec. Lásd: a `Get-MetricDefinitions` arról, hogy miként beolvasni a pontos metrikus neve alatti parancsmag|
|műveleti jel|  GreaterThan|
|Határérték (count/sec a a mérőszám)|    1|
|Ablakméret (Igen)|  00:05:00|
|összesítő (statisztikai átlagos száma, ebben az esetben használja mérőszám)|  Átlag|
|egyéni e-mailek (karakterlánc tömb)|'foo@example.com','bar@example.com'|
|e-mailt küldjenek a tulajdonosok, a munkatársak és a olvasók|    -SendToServiceOwners|

E-mailek művelet létrehozása

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Hozzon létre egy Webhook művelet

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

A szabályt a Processzor % mérőszám egy klasszikus virtuális létrehozása

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

A figyelmeztetési szabály beolvasása

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

A Hozzáadás értesítés parancsmag frissíti a szabály is, ha már létezik egy szabályt, az adott tulajdonságait. Tiltsa le egy szabályt, helyezze el a paraméter **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>A naplózási naplózási esemény értesítés

>[AZURE.NOTE] Ez a funkció is előzetes verzióban.

Ebben az esetben meg fog e-mail küldése, amikor egy webhely sikeresen elindul, az erőforrás csoport *abhingrgtest123*az előfizetésem.

Az e-mailek szabály beállítása

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Webhook szabály beállítása

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

A szabály létrehozása az esemény

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

A figyelmeztetési szabály beolvasása

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

A `Add-AlertRule` parancsmag lehetővé teszi, hogy a különböző paramétereket. További tudnivalókért lásd: a [Hozzáadás-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Lista beszerzése az elérhető mértékek értesítések
Használhatja a `Get-AzureRmMetricDefinition` parancsmag egy adott erőforrás összes mértékek listájának megtekintéséhez.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Az alábbi példa létrehoz egy táblát, amelyben a mérőszám nevét, és azt a beállítást.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Egy teljes listát az lehetőségeiről `Get-AzureRmMetricDefinition` [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx)címen érhető el.


## <a name="create-and-manage-autoscale-settings"></a>Hozzon létre és Automatikus méretezéssel beállításainak kezelése
Erőforrás, például egy webalkalmazás virtuális, felhőalapú szolgáltatás vagy virtuális skála megadása állhat, csak egy Automatikus méretezéssel beállítást.
Egyes Automatikus méretezéssel beállítások azonban több profil is rendelkezik. Egy, a méretarány teljesítmény-alapú profilok és egy második ütemezett például profil alapján. Az egyes profilok van konfigurálva, a több szabállyal. Automatikus méretezéssel kapcsolatos további tudnivalókért lásd: [Automatikus méretezéssel az alkalmazások hogyan](../cloud-services/cloud-services-how-to-scale.md).

A lépések fogjuk használni a következők:

1. Hozzon létre szabály(ok).
2. A profilok korábban létrehozott szabályokat megfeleltetése profil létrehozása.
3. Nem kötelező: Automatikus méretezéssel értesítések létrehozása webhook és e-mailek tulajdonságok.
4. Hozzon létre egy Automatikus méretezéssel beállítása a cél erőforrás nevét a profilok és az előző lépésekben létrehozott értesítések összekapcsolásával.

Az alábbi példák bemutatják, hogyan hozhat létre egy Automatikus méretezéssel beállítása egy virtuális-skála megadása a Windows operációs rendszert megszámlálása a Processzor kihasználtsági mérőszám alapján.

Első lépésként hozzon létre egy szabályt, amely skála blokkolás példány darab növekedésével.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Ezután hozzon létre egy szabályt, amely skála be, egy példány count csökkentése.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Ezután hozzon létre egy profilt a szabályok az.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Hozzon létre egy webhook tulajdonságot.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Az Automatikus méretezéssel beállításához, beleértve a levelezés és a korábban létrehozott webhook az értesítési tulajdonság létrehozása

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Végül hozza létre a profillal, amely fölött létrehozott hozzáadása az Automatikus méretezéssel beállítás.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Automatikus méretezéssel beállítások kezelésével kapcsolatos további tudnivalókért lásd: a [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Automatikus méretezéssel előzmények
A következő példa bemutatja, hogy miként tekintheti meg a legutóbbi Automatikus méretezéssel és riasztási eseményeket. Log naplózási kereséssel Automatikus méretezéssel előzményeinek megtekintése.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Használhatja a `Get-AzureRmAutoScaleHistory` parancsmag Automatikus méretezéssel előzmények beolvasásához.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

További tudnivalókért lásd: a [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Az Automatikus méretezéssel beállítás részleteinek megtekintése
Használhatja a `Get-Autoscalesetting` parancsmag beolvasni a Automatikus méretezéssel beállításról további tudnivalókat.

A következő példa bemutatja az összes Automatikus méretezéssel beállítások részleteit, az erőforrás csoport "myrg1".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

A következő példa bemutatja az erőforrás csoport "myrg1" az összes Automatikus méretezéssel beállítások és kifejezetten az Automatikus méretezéssel beállítása "MyScaleVMSSSetting" nevű részletes tudnivalókat.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Távolítsa el az Automatikus méretezéssel beállítást.
Használhatja a `Remove-Autoscalesetting` parancsmag törli az Automatikus méretezéssel beállítást.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>A naplókat napló profilok kezelése

*Log profil* létrehozása, és a naplókat egy tároló fiókkal adatok exportálása és az adatmegőrzés adhatja hozzá. Az adatokat tetszés szerint az esemény központi is adatfolyamként. Látható, hogy ez a funkció a kép és, akkor csak hozhat létre egy napló profil előfizetésenként. Használhatja a következő parancsmagok az aktuális verzióra szóló előfizetéssel kell létrehozni vagy kezelni a napló profilok. Megadhatja az adott előfizetés is. Bár a PowerShell alapértelmezés szerint a jelenlegi előfizetését, bármikor módosíthatja, hogy segítségével `Set-AzureRmContext`. Beállíthatja a naplókat útvonal adatokat tároló fiók vagy esemény központi belül az adott előfizetés. Adatok íródott JSON formátumban blob-fájlként.

### <a name="get-a-log-profile"></a>A napló profilok beszerzése
A meglévő napló profilok lehívására, használja a `Get-AzureRmLogProfile` parancsmag.

### <a name="add-a-log-profile-without-data-retention"></a>Adatmegőrzés nélkül napló profil hozzáadása

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Log profil eltávolítása

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Adatmegőrzés a napló profil hozzáadása

Megadhatja a **- RetentionInDays** tulajdonság napokat, mint a pozitív egész, ahol az adatok megőrződnek.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Adatmegőrzési és EventHub napló profil hozzáadása
Mellett az adatok továbbítása a tárterület-fiókjába, akkor is is adatfolyam-, esemény-hubon keresztül csatlakozott. Figyelje meg, hogy a előzetes verzióval és a tárhely fiókbeállítás kötelező, de esemény központi konfigurációs nem kötelező.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Állítsa be a diagnosztikai naplók
Sok Azure szolgáltatás nyújt további naplók és telemetriai, például Azure hálózati biztonsági csoportokat, a szoftver terheléselosztókkal, a kulcs tárolóból elemre, a Azure keresési szolgáltatás, és összefüggés-alkalmazásokat, és beállítható úgy, hogy az adatok mentése a Azure tárterület-fiókjában. Ez a művelet csak egy erőforrás szintjén hajtható végre, és az ugyanabban a régióban cél erőforrásként hol a diagnosztika beállítás be van állítva a tárterület-fiókot kell lennie.

### <a name="get-diagnostic-setting"></a>Diagnosztikai beállítás beszerzése

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnosztikai beállítást.

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Adatmegőrzési nélkül diagnosztikai beállítás engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Az adatmegőrzési diagnosztikai beállítás engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Az adatmegőrzési egy adott napló kategória diagnosztikai beállítás engedélyezése

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
