<properties
    pageTitle="Azure a diagnosztikai naplók áttekintése |} Microsoft Azure"
    description="Megtudhatja, Mik azok a diagnosztikai naplók Azure, és hogyan használhatja őket az Azure-erőforrás előforduló események megértéséhez."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Azure a diagnosztikai naplók áttekintése
**A diagnosztikai naplók Azure** elődcelláinak sokoldalú, gyakori működéséről az adott erőforrás egy erőforrás által kibocsátott naplók. Ezek a naplók tartalma változik erőforrás típusa (például a Windows rendszer eseménynaplók kategóriák közül a diagnosztikai naplókban VMs és blob, tábla, illetve várólista naplók tároló fiókok a diagnosztikai naplók kategóriába) és a [tevékenység naplója (korábbi nevén napló vagy működési napló)](monitoring-overview-activity-logs.md), az előfizetés erőforrásaira végrehajtott műveletek betekintést nyújt eltérnek. Nem minden erőforrások támogatja az itt leírt a diagnosztikai naplók új típusú. Az alábbi támogatott szolgáltatások listáját jeleníti meg, hogy melyik erőforrástípus támogatja az új a diagnosztikai naplók.

![A diagnosztikai naplók logikai elhelyezkedése](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Mire alkalmas a diagnosztikai naplók
Íme néhány szolgáltatás, amit a diagnosztikai naplók végezhető:

- **Tárterület-fiók** naplózási vagy manuális ellenőrzést mentheti őket. Az adatmegőrzési idő (nap) **Diagnosztikai beállítások**használatával adhatja meg.
- [Adatfolyam- **Esemény hubok** őket](monitoring-stream-diagnostic-logs-to-event-hubs.md) egy külső szolgáltatásnak vagy egyéni analytics megoldást, például a PowerBI szempontjából.
- A [MOBILE napló Analytics](../log-analytics/log-analytics-azure-storage-json.md) elemzése őket

## <a name="diagnostic-settings"></a>Diagnosztikai beállítások
A diagnosztikai naplók nem számítási erőforrások állíthatók be diagnosztikai beállítások. Egy erőforrás vezérlőelem **Diagnosztikai beállítások** :

- Ha a diagnosztikai naplók küldjük (tárterület-fiókot, esemény hubok és MOBILE napló Analytics).
- Log kategóriák küldjük.
- Mennyi ideig kell megőrizni napló kategórián tárterület-fiókjával – a nulla napok visszatartás azt jelenti, hogy naplók örökkévalóság megmaradnak. Egyéb esetben ez az érték terjedhet 1 2147483647-ig. Ha az adatmegőrzési házirendek vannak beállítva, de a naplók tárolása a tárterület-fiók le van tiltva (például ha csak az esemény hubok vagy MOBILE lehetőség ki van jelölve), az adatmegőrzési házirendek hatástalan.

Ezekkel a beállításokkal egyszerűen megtörténik a Diagnosztika lap egy erőforrás az Azure-portálon keresztül, keresztül Azure PowerShell és CLI parancsok vagy keresztül az [Azure Monitor REST API -t](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [AZURE.WARNING] A diagnosztikai naplók és mérőszámok számítási erőforrások (például VMs vagy szolgáltatás háló) használata [a külön kisalkalmazások konfigurálása és a kimeneti értékeket listáját](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Hogyan engedélyezhető a diagnosztikai naplók gyűjteménye
A diagnosztikai naplók gyűjteménye engedélyezhető részeként erőforrás létrehozása vagy egy erőforrás az erőforrás lap a portálon keresztüli létrehozását követően. A diagnosztikai naplók bármely pontján Azure PowerShell vagy CLI parancsot, vagy az Azure Monitor REST API használatával is engedélyezheti.

> [AZURE.TIP] Ezeket az utasításokat előfordulhat, hogy nem vonatkoznak közvetlenül minden erőforrás. Kattintson a speciális lépéseket, előfordulhat, hogy bizonyos erőforrástípus vonatkozó megérteni a lap alján a séma hivatkozásokra kattintva tájékozódhat.

[Ebből a cikkből megtudhatja, hogyan használhatja az erőforrás sablon diagnosztikai beállítások engedélyezése, egy erőforrás létrehozásakor](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>A diagnosztikai naplók engedélyezése a portálon
A diagnosztikai naplók engedélyezheti az Azure-portálon, a Windows vagy Linux Azure diagnosztika bővítmény engedélyezésével számítási erőforrástípus létrehozásakor:

1.  Nyissa meg az **Új** , és válassza az erőforrás érdeklik.
2.  Után az alapvető beállítások konfigurálása és az értekezletmeghívásban a méretet a **Beállítások** lap, a **Figyelés**, jelölje ki a **engedélyezve** , és válasszon ki egy hol szeretné tárolni a diagnosztikai naplók tároló fiókot. Tárterület-fiókhoz diagnosztika küldésekor-előfizetést terhelő normál adatsebesség-tárhely és a tranzakciók.

    ![A diagnosztikai naplók engedélyezése erőforrás létrehozása során.](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Kattintson az **OK gombra** , és hozzon létre az erőforrás.

Nem számítási erőforrások engedélyezheti a diagnosztikai naplók az Azure-portálon egy erőforrás az alábbiak szerint létrehozása után:

1.  Nyissa meg a az erőforrás lap, és nyissa meg a **Diagnosztika** lap.
2.  Kattintson **a** gombra, és válassza a tárterület-fiók és/vagy esemény központi.

    ![A diagnosztikai naplók engedélyezése erőforrás létrehozása után](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  A **Naplók**válassza a **Napló kategóriák** , akinek szeretne gyűjteni, vagy adatfolyam.
4.  Kattintson a **Mentés**gombra.

### <a name="enable-diagnostic-logs-via-powershell"></a>A diagnosztikai naplók PowerShell engedélyezése
Ha engedélyezni szeretné a diagnosztikai naplók Azure PowerShell-parancsmagok keresztül, használja az alábbi parancsokat.

Ahhoz, hogy a diagnosztikai naplók tárterület-fiókjában tárolására, ez a parancs használata:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

A tároló Account ID az erőforrás-azonosító, amelyre el szeretné küldeni a naplókat a tárterület-fiókom.

Ha engedélyezni szeretné a folyamatos átvitelű a diagnosztikai naplók esemény-hubon keresztül csatlakozott, ez a parancs használata:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

A szolgáltatás Bus szabály azonosítója egy olyan karakterlánc, a következő formátumban: `{service bus resource ID}/authorizationrules/{key name}`.

Ha engedélyezni szeretné a diagnosztikai naplók küldése napló Analytics-munkaterületre, ez a parancs használata:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] A WorkspaceId paramétert a október verzióban nem érhető el. A November verzióban elérhetővé válik.

A napló Analytics munkaterület azonosító az Azure-portálon szerezhet be.

Ezek a paraméterek több kimeneti beállítások is összevonhatja.

### <a name="enable-diagnostic-logs-via-cli"></a>A diagnosztikai naplók keresztül CLI engedélyezése
Ahhoz, hogy a diagnosztikai naplók keresztül az Azure CLI, használja az alábbi parancsokat:

Ahhoz, hogy a diagnosztikai naplók tárterület-fiókjában tárolására, ez a parancs használata:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

A tároló Account ID az erőforrás-azonosító, amelyre el szeretné küldeni a naplókat a tárterület-fiókom.

Ha engedélyezni szeretné a folyamatos átvitelű a diagnosztikai naplók esemény-hubon keresztül csatlakozott, ez a parancs használata:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

A szolgáltatás Bus szabály azonosítója egy olyan karakterlánc, a következő formátumban: `{service bus resource ID}/authorizationrules/{key name}`.

Ha engedélyezni szeretné a diagnosztikai naplók küldése napló Analytics-munkaterületre, ez a parancs használata:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] A workspaceId paramétert a október verzióban nem érhető el. A November verzióban elérhetővé válik.

A napló Analytics munkaterület azonosító az Azure-portálon szerezhet be.

Ezek a paraméterek több kimeneti beállítások is összevonhatja.

### <a name="enable-diagnostic-logs-via-rest-api"></a>A diagnosztikai naplók keresztül REST API engedélyezése
A Azure Monitor REST API diagnosztikai beállítások módosításához lásd: [Ehhez a dokumentumhoz](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Diagnosztikai beállítások kezelése a portálon

Annak érdekében, hogy győződjön meg arról, hogy az összes az erőforrások vannak helyesen beállítva a diagnosztikai beállítások, nyissa meg azt a portálon az **ellenőrzési** lap, és nyissa meg a **Diagnosztikai naplók** lap.

![A diagnosztikai naplók fel a portálon](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Előfordulhat, hogy a "További szolgáltatások" elemre kattintva keresse meg az ellenőrzési lap.

Ez a lap megtekintése és szűrése, amelyek támogatják a diagnosztikai naplók annak ellenőrzéséhez, hogy biztosítanak-diagnosztika engedélyezve van, és mely tárterület-fiók, esemény központi és/vagy napló Analytics-munkaterület ezeket a naplók olyan lépés, hogy az összes erőforrás.

![A diagnosztikai naplók lap eredmények portálon](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Erőforrás kattint, megjelenik a összes naplók, amely a tárterület-fiókjában tárolt, és kapcsolja ki vagy a diagnosztikai beállítások módosítása ad. Kattintson a letöltés ikonra kattintva töltse le a naplókat egy adott időszakra.

![A diagnosztikai naplók lap egy erőforrás](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] A diagnosztikai naplók csak ebben a nézetben jelenik meg, és kell letöltésre elérhetővé, ha diagnosztikai beállítások követve mentheti őket egy tárterület-fiókkal állította be.

A **Diagnosztikai beállítások** hivatkozására kattintva a diagnosztikai beállítások lap, ahol engedélyezése, letiltása és a kijelölt erőforrás diagnosztikai beállítások módosítása vele.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Támogatott szolgáltatások és -sémafájlok a diagnosztikai naplók
A diagnosztikai naplók sémát az erőforrás- és kategória függően változik. Az alábbiakban a támogatott szolgáltatások és a sémában.

| Szolgáltatás                       | Séma és dokumentumok                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Szoftver terheléselosztó     |    [Log elemzésének Azure terheléselosztó (előzetes verzió)](../load-balancer/load-balancer-monitor-log.md)             |
|    Hálózati biztonsági csoportok    |    [Log elemzésének hálózati biztonsági csoportok (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Alkalmazás átjárók       |    [Diagnosztikai naplózás alkalmazás átjáró](../application-gateway/application-gateway-diagnostics.md)     |
|    Fő tárolóból elemre                  |    [Azure kulcs tárolóra naplózás](../key-vault/key-vault-logging.md)                                                 |
|    Azure keresés               |    [Engedélyezése és keresési forgalom Analytics használata](../search/search-traffic-analytics.md)                         |
|    Tó adattárhoz            |    [A diagnosztikai naplók az Azure tó adattár elérése](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Adatok tó Analytics        |    [A diagnosztikai naplók elérése Azure adatok tó elemzéséhez](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Logika alkalmazások                 |    Séma nem érhető el.                                                                                         |
|    Azure köteg                |    [Azure köteg diagnosztikai naplózás](../batch/batch-diagnostics.md)                                              |
|    Azure automatizálási           |    [Azure automatizálási napló elemzésének](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Esemény központi                  |    Séma nem érhető el.                                                                                         |
|    Szolgáltatás Bus                |    Séma nem érhető el.                                                                                         |
|    Értékáram-elemzés           |    Séma nem érhető el.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Támogatott napló kategóriák egy erőforrás típusa

|Erőforrás típusa|Kategória|Kategória megjelenítendő név|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Feladat naplók|
|Microsoft.Automation/automationAccounts|JobStreams|Feladat adatfolyamok|
|Microsoft.Batch/batchAccounts|ServiceLog|Szolgáltatás a naplók|
|Microsoft.DataLakeAnalytics/accounts|Naplózás|Naplókat|
|Microsoft.DataLakeAnalytics/accounts|Kérések|Naplók kérése|
|Microsoft.DataLakeStore/accounts|Naplózás|Naplókat|
|Microsoft.DataLakeStore/accounts|Kérések|Naplók kérése|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archiválás naplók|
|Microsoft.EventHub/namespaces|OperationalLogs|Műveleti naplók|
|Microsoft.KeyVault/vaults|AuditEvent|Naplókat|
|Microsoft.Logic/workflows|WorkflowRuntime|Munkafolyamat-futtatókörnyezet diagnosztikai események|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Hálózati biztonsági csoport esemény|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Hálózati biztonsági csoport szabály számláló|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Hálózati biztonsági csoport szabály áramlás esemény|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Riasztási események terheléselosztó betöltése|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Terheléselosztó vizsgálati állapot állapotához|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Alkalmazás átjáró Access napló|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Alkalmazás átjáró teljesítménybeli napló|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Alkalmazás átjáró tűzfal napló|
|Microsoft.Search/searchServices|OperationLogs|A művelet naplók|
|Microsoft.ServerManagement/nodes|RequestLogs|Naplók kérése|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Műveleti naplók|
|Microsoft.StreamAnalytics/streamingjobs|Végrehajtási|Végrehajtási|
|Microsoft.StreamAnalytics/streamingjobs|Szerzői|Szerzői|

## <a name="next-steps"></a>Következő lépések
- [**Esemény hubok** adatfolyam diagnosztikai naplók](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [A Azure Monitor REST API diagnosztikai beállítások módosítása](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [A naplók a MOBILE napló Analytics elemzése](../log-analytics/log-analytics-azure-storage-json.md)
