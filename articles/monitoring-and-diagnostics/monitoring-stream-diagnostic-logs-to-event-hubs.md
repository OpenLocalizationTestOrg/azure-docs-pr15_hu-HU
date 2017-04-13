<properties
    pageTitle="Adatfolyam-esemény hubok Azure diagnosztikai naplók |} Microsoft Azure"
    description="Megtudhatja, hogy miként adatfolyam-esemény hubok Azure diagnosztikai naplók."
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
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Adatfolyam-esemény hubok Azure diagnosztikai naplók

**[A diagnosztikai naplók Azure](monitoring-overview-of-diagnostic-logs.md)** a is közeli valós időben való a beépített "Exportálása a esemény hubok" beállítást használó, a portálon vagy a szolgáltatás Bus szabály azonosítója, a diagnosztikai beállítással az Azure PowerShell-parancsmagok vagy a Azure CLI engedélyezésével alkalmazást folyamatosan lejátszott.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Mire alkalmas a diagnosztikai naplók és esemény hubok
Íme néhány módszer a továbbított képesség használhatja a diagnosztikai naplók:

- **Naplózás 3 fél és telemetriai rendszerek adatfolyam naplók** – az idő múlásával a folyamatos átvitelű esemény hubok válik az eljárás a diagnosztikai naplók pipe harmadik fél SIEMs be, és jelentkezzen be a analytics-megoldások.

- **Nézet szolgáltatásállapot PowerBI "meleg elérési út" adatok folyamatos** – használatával esemény hubokba, a megjelenítő Értékáram-elemzés és a PowerBI, egyszerűen átalakíthatja a diagnosztikai adatok közelében valós idejű háttérismeretek az az Azure Services. [Ez a cikk dokumentáció remek áttekintést, hogy miként és állíthatja be az esemény hubok, adatfeldolgozás Értékáram-elemzés, a PowerBI-kimenetként](../stream-analytics/stream-analytics-power-bi-dashboard.md). Íme néhány tipp a diagnosztikai naplók első beállítása:
    - Az esemény veszik fel a diagnosztikai naplók kategória szükséges automatikusan létrejön, jelölje be a jelölőnégyzetet a portálon vagy engedélyezése, Powershellen keresztül, így azt szeretné, jelölje be a szolgáltatás Bus névtér "háttérismeretek-" kezdetű nevű esemény hubokon
    - Íme egy példa Értékáram-elemzés a lekérdezésre, egyszerűen csak az összes a naplóadatokat a PowerBI tábla elemzésére használható:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Egy egyéni telemetriai és naplózás platform** – Ha akkor már van-e egy custom-built telemetriai platform vagy csak egy épület gondolat, nagyon méretezhető közzététel előfizetés jellegét esemény hubok lehetővé teszi, hogy a diagnosztikai naplók rugalmasan ingest. [Lásd: Dan Rosanova tartozó útmutató használatával az itt globális skála telemetriai platformot esemény hubok](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>A diagnosztikai naplók streaming engedélyezése
Lehetősége van engedélyezni a folyamatos átvitelű a diagnosztikai naplók programozás útján, a portálon keresztül, vagy a [Azure Monitor REST API -t](https://msdn.microsoft.com/library/azure/dn931943.aspx). Mindkét esetben válassza a szolgáltatás Bus Namespace, és egy esemény hubok a névtér engedélyezi a napló kategórián jön létre. Diagnosztikai **Napló kategória** egy típusú erőforrás gyűjthet naplót. Szeretné, hogy egy adott erőforrás az Azure-portálon csoportban a Diagnosztika lap összegyűjtése napló kategóriák is választhat.

![Log kategóriák a portálon](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Engedélyezése és a folyamatos átvitelű számítási erőforrások (például VMs vagy szolgáltatás háló), [más beállítási lépéseket kell](../event-hubs/event-hubs-streaming-azure-diags-data.md)a diagnosztikai naplók.

### <a name="via-powershell-cmdlets"></a>VIA PowerShell-parancsmagok
Ha engedélyezni szeretné a folyamatos átvitelű keresztül az [Azure PowerShell-parancsmagok](insights-powershell-samples.md), használhatja a `Set-AzureRmDiagnosticSetting` ezeket a paramétereket parancsmagnak:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

A szolgáltatás Bus szabály azonosítója egy olyan karakterlánc, a következő formátumban: `{service bus resource ID}/authorizationrules/{key name}`, például `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Azure CLI keresztül
Ha engedélyezni szeretné a folyamatos átvitelű keresztül az [Azure CLI](insights-cli-samples.md), használhatja a `insights diagnostic set` parancs jelennek meg:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

A PowerShell-parancsmag a című cikkben ismertetett módon használja ugyanazt a formátumot szolgáltatás Bus szabály azonosítója.

###<a name="via-azure-portal"></a>Azure portálon keresztül
Ha engedélyezni szeretné a folyamatos átvitelű az Azure-portálon keresztül, nyissa meg azt a diagnosztika beállításainak egy erőforrás, és válassza a "Exportálása esemény-hubon keresztül csatlakozott."

![Esemény hubok a portálon exportálása](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Adja meg, jelölje ki egy meglévő szolgáltatási Bus Namespace. A kijelölt névtér lesz, ha az esemény hubok (Ha ez az első alkalommal adatfolyam a diagnosztikai naplók) létrehozott vagy részére (Ha már vannak információforrások, amelyek vannak a folyamatos átvitelű, hogy a névtér a napló kategória), és a házirend azt határozza meg az adatfolyam mechanizmusa jogosultságai. Ma a folyamatos átvitelű azon egy esemény hubokhoz kezelése, az Olvasás és a Küldés engedélyek szükségesek. Hozzon létre, vagy módosíthatja a szolgáltatás Bus Namespace szolgáltatás Bus Namespace megosztott access házirendek az "Konfigurálása" lapon a klasszikus portálon. A diagnosztikai beállítások frissítéséhez az ügyfél ListKey engedéllyel kell rendelkeznie a szolgáltatási Bus engedélyezési szabályhoz.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hogyan használja fel a naplóadatokat származó esemény hubok?
Az alábbiakban a kimeneti mintaadatokat az esemény hubokon:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Elem neve | Leírás                                            |
|--------------|--------------------------------------------------------|
|rekordok       | Tömb összes naplóbeli események a tartalomban.            |
|idő          | Az idő az esemény bekövetkezett.                      |
|kategória      | Log kategória: Ez az esemény.                           |
|resourceId    | Erőforrás-azonosító az erőforrás által generált eseményt. |
|operationName | A művelet neve.                                 |
|szint         | Nem kötelező. Log esemény szintjét mutatja.               |
|Tulajdonságok    | Az esemény tulajdonságai.                               |


Megtekintheti a összes erőforrás-szolgáltatót, amely támogatja a folyamatos átvitelű esemény-hubon keresztül csatlakozott [Itt](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Következő lépések
- [További információ: a diagnosztikai naplók Azure](monitoring-overview-of-diagnostic-logs.md)
- [Esemény hubok – első lépések](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
