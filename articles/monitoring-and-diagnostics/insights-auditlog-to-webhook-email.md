<properties
    pageTitle="Egy webhook Azure tevékenységnapló értesítések beállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként tevékenységnapló értesítésekkel webhooks hívni. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Egy webhook az Azure tevékenységnapló értesítések konfigurálása

Webhooks lehetővé teszi az Azure értesítést átirányítása az egyéb rendszerek utáni feldolgozás vagy egyéni műveletek. Értesítés a egy webhook segítségével irányítja a Szöveges üzenet, jelentkezzen be a hibák, keresztüli Csevegés/üzenetküldési szolgáltatások csoport értesítést vagy végezze el az egyéb műveletek tetszőleges számú szolgáltatásokhoz. Ez a cikk ismerteti a webhook Azure tevékenységnapló értesítés beállítása és a tartalom szeretne egy webhook HTTP bejegyzés néz ki. További információt a telepítő és -sémafájlok Azure metrikus riasztás, [ehelyett a lapon látható](insights-webhooks-alerts.md). Is beállíthatja tevékenységnapló értesítés küldése e-mailek aktiválásakor.

>[AZURE.NOTE] Ez a funkció jelenleg előzetes verzióban, így várt változó minőségű és a teljesítmény.

Beállíthatja az [Azure PowerShell-parancsmagok](insights-powershell-samples.md#create-alert-rules), [Platformok CLI](insights-cli-samples.md#work-with-alerts)vagy [Azure Monitor REST API -t](https://msdn.microsoft.com/library/azure/dn933805.aspx)jelzést tevékenységnapló.

## <a name="authenticating-the-webhook"></a>A webhook hitelesítés
Használati webhook hitelesíthet használ, kövesse az alábbi módszerek egyikét:

1. **Jogkivonat-alapú hitelesítés** – a webhook URI pl. mentett jogkivonat azonosítóval. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Alapszintű hitelesítés** – a webhook URI menti a program a felhasználónevet és jelszót, pl.. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Tartalom séma
A KÜLDÉS művelet tartalmazza a következő JSON-tartalom és -sémafájlok minden tevékenység naplója-alapú nyomon követése riasztásokkal parancsát. Ez a séma hasonlít a metrikus értesítések által használt.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Elem neve       |Leírás|
|---                |---|
|állapot             |A használt metrikus nyomon követése riasztásokkal parancsát. Mindig beállítása "aktiválására" tevékenységnapló nyomon követése riasztásokkal parancsát.|
|környezet            |Az esemény környezetében.|
|resourceProviderName|A probléma által sújtott erőforrás erőforrás-szolgáltató.|
|conditionType      |Mindig "esemény".|
|név               |A riasztási szabály nevét.|
|azonosító                 |Erőforrás-azonosító figyelmeztetés.|
|Leírás        |Az értesítés létrehozása közben beállított riasztási leírása|
|subscriptionId     |Azure előfizetés azonosítójával.|
|időbélyeg          |Az idő, amelynél az eseményre az Azure szolgáltatás által feldolgozott a kérelem hozta létre.|
|resourceId         |Erőforrás-azonosító a probléma által sújtott erőforrás.|
|resourceGroupName  |A csoport nevét, az erőforrás a probléma által sújtott erőforrás|
|Tulajdonságok         |A Set `<Key, Value>` párban (azaz `Dictionary<String, String>`), amely tartalmazza az esemény részleteit.|
|esemény              |Az esemény metaadatokat tartalmazó elemet.|
|engedély      |Az esemény RBAC tulajdonságait. Általában ezek közé tartozik, a "művelet", "szerepkör" és "hatókör."|
|kategória           |Az esemény kategóriát. Támogatott értékek a következők: felügyeleti, riasztást, biztonsági, ServiceHealth, ajánlási.|
|hívóazonosító             |A felhasználó, aki elvégezni a műveletet, UPN igénylése vagy az elérhetőség alapján SPN igénylése e-mail címe. Null bizonyos rendszer hívások lehet.|
|correlationId      |Általában egy GUID karakterlánc-formátumban. Események correlationId ugyanazon nagyobb művelet tartoznak, és általában megosztása egy correlationId.|
|eventDescription   |Az esemény leírása statikus szöveggé.|
|eventDataId        |Egyedi azonosító az eseményt.|
|eventSource        |Azure szolgáltatás vagy infrastruktúra által generált az esemény nevét.|
|httpRequest        |Általában tartalmazza a "clientRequestId", "clientIpAddress" és "módszer" (a HTTP-metódus pl. ELHELYEZNI).|
|szint              |Az alábbi értékek közül: "Kritikus", "Hiba", "Figyelmeztetés", "Tájékoztatások" és "Részletes."|
|operationId        |Általában GUID az egyetlen művelet megfelelő események között.|
|operationName      |A művelet neve.|
|Tulajdonságok         |Az esemény tulajdonságai.|
|állapot             |Karakterlánc. A művelet állapota. Közös értékek a következők: "Lépések", "Folyamatban", "Sikeres", "Nem sikerült", "Aktív", "A rendszer".|
|Részállapot          |A HTTP-állapotkód a megfelelő többi hívások általában tartalmazza. Egyéb leíró egy részállapot karakterláncok is tartalmazhat. Közös részállapot értékek a következők: az OK gombra (HTTP állapotkód: 200), létrehozott (HTTP állapotkód: 201), elfogadott (HTTP állapotkód: 202), a tartalom nem (HTTP állapotkód: 204), hibás kérés (HTTP állapotkód: 400), nem található (HTTP állapotkód: 404), ütközés (HTTP állapotkód: 409), belső kiszolgálóhiba (HTTP állapotkód: 500), szolgáltatás nem érhető el (HTTP állapotkód: 503), átjáró időtúllépése (HTTP állapotkód: 504)|

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók a tevékenységnapló](monitoring-overview-activity-logs.md)
- [Azure riasztások Azure automatizálási parancsfájlokat (Runbooks) végrehajtása](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Logika App használata az Azure jelzést Twilio keresztül egy Szöveges üzenet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Ez a példa metrikus nyomon követése riasztásokkal parancsát, de tevékenységnapló jelzést végezhető módosítani kell.
- [Logika App használata az Azure jelzést tartalékidő üzenet küldése](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Ez a példa metrikus nyomon követése riasztásokkal parancsát, de tevékenységnapló jelzést végezhető módosítani kell.
- [Logika App használata egy üzenetet küldeni szeretné az Azure várólista Azure értesítés](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Ez a példa metrikus nyomon követése riasztásokkal parancsát, de tevékenységnapló jelzést végezhető módosítani kell.
