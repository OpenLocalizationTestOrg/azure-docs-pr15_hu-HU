<properties
   pageTitle="Log Analytics riasztási REST API-t"
   description="A napló Analytics riasztási REST API segítségével értesítések műveletek Management csomagja (MOBILE) létrehozása és kezelése.  Ebben a cikkben részleteit, az API-val és a példák számos különböző műveletek elvégzéséhez."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Log Analytics riasztási REST API

A napló Analytics riasztási REST API segítségével értesítések műveletek Management csomagja (MOBILE) létrehozása és kezelése.  Ebben a cikkben részleteit, az API-val és a példák számos különböző műveletek elvégzéséhez.

A napló Analytics keresési REST API RESTful és azok webböngészőn keresztül az Azure erőforrás-kezelő REST API-t. A dokumentum találja példák hol érhető el az API segítségével [ARMClient](https://github.com/projectkudu/ARMClient), a Megnyitás parancssori eszköz, amely megkönnyíti az Azure erőforrás-kezelő API meghívása PowerShell parancssorából. A ARMClient és a PowerShell használata a napló Analytics keresési API eléréséhez sok lehetőségek közül. Ezeket az eszközöket is használhatja a RESTful Azure erőforrás-kezelő API segítségével MOBILE munkaterületek kezdeményezhetnek, és végezze el a keresés parancsainak őket. Az API kimeneti fog találatokat, hogy Ön JSON formátumban, lehetővé téve a keresési eredmények programozás útján számos különböző módon használatát.

## <a name="prerequisites"></a>Előfeltételek
Jelenleg riasztások csak hozhatja létre a napló Analytics mentett keresés.  További információt a [Napló keresési REST API -t](log-analytics-log-search-api.md) is hivatkozhat.

## <a name="schedules"></a>Kimutatások
Mentett keresés beállíthatja, hogy egy vagy több ütemezést. Az ütemezés határozza meg, hogy milyen gyakran a keresés futtatása és az eredményhalmaz, amelyen a feltétel azonosítjuk időszakot.
Ütemezés tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság  | Leírás |
|:--|:--|
| Intervallum | Milyen gyakran a keresés futtatásához. Percekben. |
| QueryTimeSpan | Az eredményhalmaz, amelyen a feltétel kiértékelt időszakot. Egyenlő vagy nagyobb, mint intervallum kell lennie. Percekben. |
| Verzió | A API verzióját használja.  Jelenleg a beállítása mindig 1. |

Például érdemes megvizsgálni elvégezve 15 percet, és egy időszak 30 perc egy esemény lekérdezést. Ebben az esetben 15 percenként a lekérdezés szeretné futtatni, és szeretné kell figyelmeztet, ha a feltétel úgy, hogy az IGAZ felett továbbra is a 30 perc lett létrehozva.

### <a name="retrieving-schedules"></a>Ütemezés beolvasása
A Get-metódus segítségével beolvashatja egy mentett keresés az összes ütemtervét.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

A Get-metódus ütemezés azonosítóval használható egy adott ütemtervet mentett keresés beolvasásához.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Az alábbiakban ütemezett minta választ.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>A kimutatás létrehozása
Egy egyedi ütemezés azonosítóval helyezése módszer használatával hozzon létre egy új kimutatást.  Ne feledje, hogy két ütemezések nem ugyanazt az Azonosítót még akkor is, ha társítva különböző mentett keresések.  Ütemezett létrehozásakor a MOBILE konzolban GUID létrejön az ütemezési azonosítóját.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Az ütemezés módosítása
Egy meglévő ütemezés azonosítójú ugyanarra a mentett keresésre helyezése módszer segítségével módosíthatja, hogy az ütemezés.  A kérés törzsében tartalmaznia kell a etag az ütemezés.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Ütemezés törlése
A törlés módszer használata egy ütemezési azonosítója ütemezett törlése.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Műveletek
Ütemezett beállíthatja, hogy több művelet. Művelet határozhatják meg egy vagy több folyamatok végrehajtásához, például egy levélküldés vagy egy runbook indítása, illetve határozhat határozza meg, amikor a keresési eredmények feltételeknek bizonyos küszöbértéket.  Néhány műveletet határozza meg is, hogy a folyamatok megtörténik a küszöbértékét teljesülése esetén.

Az összes műveletek tárolja az a tulajdonságokat az alábbi táblázatban.  Különböző típusú figyelmeztetés van más további tulajdonságot, amely az alábbiakban ismertetjük.

| A tulajdonság | Leírás |
|:--|:--|
| Típus | A tevékenység típusát.  A lehetséges értékek jelenleg értesítés és Webhook. |
| név | Az értesítés megjelenítendő nevét. |
| Verzió | A API verzióját használja.  Jelenleg a beállítása mindig 1. |

### <a name="retrieving-actions"></a>Műveletek végrehajtása
A Get-metódus segítségével beolvashatja ütemezett összes művelet.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

A Get-módszer a művelet azonosítójú használható egy adott művelet ütemezett beolvasásához.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Létrehozásakor vagy szerkesztésekor műveletek
Hozzon létre egy új tevékenység az ütemterv egyedi művelet azonosítójú helyezése módszert használni.  Művelet a MOBILE konzolban létrehozásakor GUID van-e a művelet azonosítóját.

Egy meglévő művelet azonosítójú ugyanarra a mentett keresésre helyezése módszer segítségével módosíthatja, hogy az ütemezés.  A kérés törzsében tartalmaznia kell a etag az ütemezés.

A kérés formátum hozhat létre új művelet, ezek a példák találhatók az alábbi szakaszok művelettípus változik.

### <a name="deleting-actions"></a>Műveletek törlése
Módszert a törlési művelet azonosítójú művelet törléséhez.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Riasztási műveletek
Ütemezett egy és csak egy műveletet kell rendelkeznie.  Riasztási műveletek valamelyike az alábbi táblázat a szakaszok.  Minden egyes részletesen az alábbi témakörben olvashat.

| Szakasz | Leírás |
|:--|:--|
| Küszöbérték | A művelet futtatásakor feltételeit. |  
| EmailNotification | Címzetteknek küldenek levelet, több. |
| Remediation | Indítsa el a runbook Azure automatizálási próbálja meg azonosított probléma javításához. |

#### <a name="thresholds"></a>Küszöbérték
Riasztási művelet egyetlen küszöb kell rendelkeznie.  Ha egy mentett keresés eredménye megegyezik a társított, hogy a Keresés művelet küszöbérték, a művelet bármely más folyamatok futnak.  Művelet is tartalmazhat, csak egy küszöbérték, hogy más típusú, amelyek nem tartalmaznak-küszöbértékek műveleteinek is használható.

Küszöbértékek tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| Műveleti jel | Küszöb összehasonlítására operátor. <br> gt = nagyobb, mint <br> lt = kisebb, mint |
| Érték | A küszöbértékét értékét. |

Például érdemes megvizsgálni a 15 percet, egy időszak 30 percig és egy 10-nél nagyobb küszöbérték elvégezve egy esemény lekérdezést. Ebben az esetben 15 percenként a lekérdezés szeretné futtatni, és szeretné is figyelmeztet, ha létrehozott egy 30 perc módosítva fölé 10 események visszaadott.

Az alábbiakban csak egy küszöbérték a művelet minta választ.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Egy egyedi művelet azonosítóval helyezése módszer használatával hozzon létre egy új küszöb tevékenység ütemezett.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Egy meglévő művelet azonosítójú helyezése módszer segítségével módosíthatja egy ütemezett küszöb művelet.  A kérés törzsében tartalmaznia kell végrehajtani a etag.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>Értesítő e-mailt
Értesítő e-mailek egy vagy több címzettnek szóló e-mailben.  Az tulajdonságokat az alábbi táblázat tartalmazzák.

| A tulajdonság | Leírás |
|:--|:--|
| A címzettek | E-mail címek listája. |
| Tárgy | A levelek tárgyát. |
| Melléklet | Mellékletek jelenleg nem támogatottak, így mindig lesz, ez az "Nincs" értéket. |

Az alábbiakban küszöbértéket az e-mailben értesítést művelet minta választ.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Egy egyedi művelet azonosítóval helyezése módszer használatával hozzon létre egy új e-mail tevékenység ütemezett.  A következő példa hoz e-mailben értesítést küszöbérték, így az üzenetet küldi el a mentett keresés eredménye-nál nagyobb küszöbértéket.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Egy meglévő művelet azonosítójú helyezése módszer segítségével módosíthatja a ütemezett e-mail művelet.  A kérés törzsében tartalmaznia kell végrehajtani a etag.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Remediation műveletek
Remediations indítsa el a runbook Azure automatizálási, próbálja meg a figyelmeztető jelölt probléma megoldására.  Létre kell hoznia egy webhook esetében a egy remediation művelet használt runbook és adja meg a URI a WebhookUri tulajdonságban.  Ez a művelet a MOBILE konzolon hoz létre, amikor egy új webhook automatikusan létrejön a runbook.

Remediations az alábbi táblázat tartalmazza az a tulajdonságokat.

| A tulajdonság | Leírás |
|:--|:--|
| RunbookName | A runbook neve. Ennek meg kell egyeznie a közzétett runbook konfigurálva a automatizálási megoldás a MOBILE munkaterületen található automatizálás fiók. |
| WebhookUri | A webhook URI.
| Lejárat | A lejárati dátumát és időpontját a webhook.  Ha a webhook nincs egy lejárati, majd ez lehet bármely érvényes jövőbeli dátumot. |

Az alábbiakban a remediation művelet végrehajtása a küszöbértéket minta választ.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Egy egyedi művelet azonosítóval helyezése módszer használatával hozzon létre egy új remediation tevékenység ütemezett.  A következő példa hoz egy remediation küszöbérték, így a runbook azonnal elindul, amikor a mentett keresés eredménye túllépi a küszöbértékét.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Egy meglévő művelet azonosítójú helyezése módszer segítségével módosíthatja a remediation művelet ütemezett.  A kérés törzsében tartalmaznia kell végrehajtani a etag.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Példa
Az alábbiakban a teljes példa hozhat létre egy új e-mailben értesítést.  Ezzel létrehozott egy új kimutatást küszöb és e-mailek tartalmazó művelet együtt.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook műveletek
Webhook műveletek végrehajtás indítása hívja fel az URL-címet, és tetszés szerint a küldendő hasznos megadásával.  Ezek hasonlóak Remediation műveletek kivéve azokat, amelyek a folyamatok Azure automatizálási runbooks nem hivatkozhat webhooks az azt jelenti.  Ezenkívül biztosít további lehetőséget kínál a hasznos a távoli folyamat, kézbesítése.

Webhook műveletek nem rendelkezik egy küszöbérték, de inkább hozzá kell adni egy figyelmeztető műveletet, amelyek küszöbértéke tartalmazó ütemezett.  Több Webhook művelet az összes futtatandó küszöbértéket teljesülése esetén is hozzáadhat.

Webhook műveletek az alábbi táblázat tartalmazza az a tulajdonságokat.

| A tulajdonság | Leírás |
|:--|:--|
| WebhookUri | A levelek tárgyát. |
| CustomPayload | Saját tartalom a webhook kapni.  A formátum a Mi a webhook meglepő függ. |

Az alábbiakban webhook művelet, és amelyek küszöbértéke társított riasztási művelet minta választ.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Hozzon létre vagy webhook művelet szerkesztése
Egy egyedi művelet azonosítóval helyezése módszer használatával hozzon létre egy új webhook tevékenység ütemezett.  Az alábbi példa létrehoz egy Webhook művelet, és egy figyelmeztető műveletet tartalmazó küszöbérték, hogy a webhook aktiválódik, amikor a mentett keresés eredménye túllépi a küszöbértékét.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Egy meglévő művelet azonosítójú helyezése módszer segítségével módosíthatja a webhook művelet ütemezett.  A kérés törzsében tartalmaznia kell végrehajtani a etag.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Következő lépések

- Log Analytics a [Napló keresés REST API -t](log-analytics-log-search-api.md) használja.
