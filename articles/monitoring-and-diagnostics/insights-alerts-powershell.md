<properties
    pageTitle="Az Azure-szolgáltatások értesítések létrehozása a PowerShell használatával |} Microsoft Azure"
    description="Azure értesítések, válthatnak értesítések és automatizálási, a megadott feltételek teljesülése esetén létrehozása a PowerShell használatával."
    authors="rboucher"
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Az Azure-szolgáltatások értesítések létrehozása a PowerShell használatával

> [AZURE.SELECTOR]
- [Portál](insights-alerts-portal.md)
- [A PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan állíthat be a PowerShell használatával Azure értesítéseket.  

Az ellenőrző mértékek vagy események, a Azure szolgáltatások alapján értesítés kaphat.

- **Metrikus értékek** – a riasztási eseményindítók, ha egy adott mérőszám értékének metszéspontja mindkét irányba hozzárendelése küszöbértéket. Ez azt jelenti, akkor mindkét eseményindítók amikor először teljesül a feltétel és majd utána mikor amely feltétel van már nem teljesül.    
- **Tevékenység naplóbeli események** - értesítés válthatnak *minden* esemény, illetve csak bizonyos számú események fordul elő.

Hajtsa végre a következő, amikor elindítja azt, hogy jelzést adhatja meg:

- a szolgáltatás-rendszergazda, és további rendszergazdák értesítő e-mailek küldése
- e-mailt küldjenek további e-mailek megadott feltételeknek.
- hívja fel a webhook
- Indítsa el az Azure runbook (csak az Azure portál) végrehajtása

Állítsa be, és riasztási szabályok használatával kapcsolatos tájékozódáshoz

- [Azure portál](insights-alerts-portal.md)
- [A PowerShell](insights-alerts-powershell.md)
- [parancssori kezelőfelületről](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-val](https://msdn.microsoft.com/library/azure/dn931945.aspx)


További információért mindig írhat ```get-help``` , és kattintson a keresett PowerShell-parancsot.

## <a name="create-alert-rules-in-powershell"></a>A PowerShell riasztási szabályok létrehozása

1. Bejelentkezés az Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Lista beszerzése, az előfizetések egyikével rendelkezik érhető el. Ellenőrizze, hogy a megfelelő előfizetéssel rendelkező működik. Ha nem, állítsa be a megfelelőt kimenetét használatával `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Erőforráscsoport meglévő szabályok listáját, használja az alábbi parancsot:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Szabály létrehozása több fontos információkat vehet először van szükség. 
    - Értesítés beállítása szeretné az erőforrás az **Erőforrás-azonosító**
    - Az adott erőforrás elérhető **metrikus definíciók**

    Az erőforrás-azonosító megszerezni egy úgy, hogy az Azure portálon használja. Feltételezve, hogy az erőforrás már létrehozott, jelölje ki azt a portálon. A Tovább gombra a lap, kattintson a *Tulajdonságok parancsra* a *Beállítások* szakaszban. Az erőforrás-azonosító mező a következő lap. Egy másik úgy, hogy az [Azure erőforrás Explorer](https://resources.azure.com/).

    Egy példa egy webalkalmazás erőforrás-azonosító

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Használható `Get-AzureRmMetricDefinition` egy adott erőforrás összes metrikus definíciótípus listájának megtekintéséhez.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Az alábbi példa létrehoz egy táblát, amelyben a mérőszám neve és a egységet adott metrikus.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Egy teljes listát az elérhető lehetőségekről a Get-AzureRmMetricDefinition futtassa a Get-MetricDefinitions érhető el.


5. A következő példa beállítja a webhely erőforrás értesítés. A riasztási eseményindítók, amikor minden forgalom az 5 perc és újra forgalmat beérkezéskor 5 percig egységes kap.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Webhook létrehozásához, és küldjön e-mailek, amikor az értesítés eseményindítók, először létre az e-mailek és/vagy webhooks. Kattintson közvetlenül a szabály létrehozása később az - műveletek címkével ellátott és az alábbi példán látható módon. Nem lehet rendelni webhook vagy e-mailek, a már létrehozott PowerShell szabályok.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. A tevékenység naplója egy bizonyos feltétel figyelmeztetést kiváltó létrehozásához használja az alábbi képernyő parancsok

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    A tevékenység az esemény egy típusú - OperationName felel meg. Többek között *Microsoft.Compute/virtualMachines/delete* és *microsoft.insights/diagnosticSettings/write*.

    Paranccsal a PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) beszerzése lehetséges operationNames listáját. Az Azure portal segítségével váltakozva, a lekérdezés a tevékenység naplója, és konkrét múltbeli szóló értesítés létrehozása kívánt műveleteket. A műveletek rövid névvel grafikus napló nézetben látható. A JSON a bejegyzés keresni, és kijjebb OperationName értékét.   

8. Győződjön meg arról, hogy az értesítések létrehozott megfelelően megjeleníti az egyes szabályokat.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Az értesítések törlése Ezek a parancsok a korábban létrehozott, a jelen cikkben szabályok törlése

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Következő lépések

* , [Áttekintést kaphat a Azure figyelése](monitoring-overview.md) többek között a típusú adatok összegyűjtése és figyelése.
* További tudnivalók [az értesítések konfigurálása webhooks](insights-webhooks-alerts.md).
* További tudnivalók az [Azure automatizálási Runbooks](..\automation\automation-starting-a-runbook.md).
* Ismerkedés [a diagnosztikai naplók összegyűjtési áttekintése](monitoring-overview-of-diagnostic-logs.md) részletes magas-gyakoriság mértékek összegyűjtheti az Ön szolgáltatása.
* Ismerkedés a [Mértékek webhelycsoport áttekintése](insights-how-to-customize-monitoring.md) és a szolgáltatás nem érhető el, és válaszol.
