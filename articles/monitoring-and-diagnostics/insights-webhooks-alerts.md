<properties
    pageTitle="Azure metrikus értesítések konfigurálása webhooks |} Microsoft Azure"
    description="Azure riasztások más nem Azure rendszerek átirányítása."
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
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Egy webhook Azure metrikus értesítés beállítása

Webhooks lehetővé teszi az Azure értesítést átirányítása az egyéb rendszerek utáni feldolgozás vagy egyéni műveletek. Értesítés a egy webhook segítségével irányítja a Szöveges üzenet, jelentkezzen be a hibák, keresztüli Csevegés/üzenetküldési szolgáltatások csoport értesítést vagy végezze el az egyéb műveletek tetszőleges számú szolgáltatásokhoz. Ez a cikk ismerteti a webhook Azure metrikus értesítés beállítása és a tartalom szeretne egy webhook HTTP bejegyzés néz ki. Az Azure tevékenység napló a telepítő és -sémafájlok információkat jelenít meg értesítést (események értesítés), [ehelyett a lapon látható](./insights-auditlog-to-webhook-email.md).

Azure riasztások HTTP POST a JSON-ban a riasztási tartalma formátumot, az alábbi egy webhook URI az értesítésre létrehozásakor Ön által meghatározott séma. Ez a URI érvényes HTTP vagy HTTPS-végpont kell lennie. Azure bejegyzések egy kérelemre egy bejegyzést, értesítés aktiválásakor.

## <a name="configuring-webhooks-via-the-portal"></a>A portálon keresztüli webhooks konfigurálása

Felvehet, és frissítse a webhook URI a létrehozás és frissítés riasztások képernyőn a [portálon](https://portal.azure.com/).

![Riasztási szabály hozzáadása](./media/insights-webhooks-alerts/Alertwebhook.png)

A közzétételi egy webhook URI az [Azure PowerShell-parancsmagok](./insights-powershell-samples.md#create-alert-rules), [Platformok CLI](./insights-cli-samples.md#work-with-alerts)vagy [Azure Monitor REST API -t](https://msdn.microsoft.com/library/azure/dn933805.aspx)jelzést is beállítható.

## <a name="authenticating-the-webhook"></a>A webhook hitelesítés

A webhook hitelesíthet használ, kövesse az alábbi módszerek egyikét:

1. **Jogkivonat-alapú hitelesítés** – a webhook URI pl. mentett jogkivonat azonosítóval. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Alapszintű hitelesítés** – a webhook URI menti a program a felhasználónevet és jelszót, pl.. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Tartalom séma

A KÜLDÉS művelet tartalmazza a következő JSON-tartalom és -sémafájlok összes metrikus-alapú nyomon követése riasztásokkal parancsát.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| A mező | Kötelező | Rögzített értékhalmazt | Jegyzetek |
| :-------------| :-------------   | :-------------   | :-------------   |
|állapot|Y|"Aktív", "A rendszer"|A figyelmeztetés elhagyja a feltételek alapján állapotát állította be.|
|környezet| Y | | Az értesítés környezetben.|
|időbélyeg| Y | | Az idő, amelynél a figyelmeztetés volt.|
|azonosító | Y | | Minden szabályt egyedi azonosítót tartalmaz.|
|név               |Y                  |                   | A riasztási nevét.|
|Leírás        |Y                  |                           |Az értesítés leírását.|
|conditionType      |Y                  |"Metrikus", az "Esemény"          |Értesítések kétféle támogatottak. Egy mérőszám feltétel alapján a másik pedig a tevékenység naplója esemény alapján. Ez az érték segítségével ellenőrzése, ha a figyelmeztető alapuló metrikus és esemény.|
|feltétel          |Y                  |                           | Keressen az adott mezők alapján a conditionType.|
|metricName         |Metrikus értesítések  |                           |A mérőszám, amely meghatározza, hogy a szabály figyeli neve.|
|metricUnit         |Metrikus értesítések  |"Bájt", "BytesPerSecond", "Megszámolása", "CountPerSecond", "Százalék", "Másodperc"|     A beállítást, a mérőszám sorolja fel. [Engedélyezett értékek listája megtalálható](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |Metrikus értesítések  |                           |A tényleges érték a figyelmeztetést kiváltó mérőszám.|
|küszöbérték          |Metrikus értesítések  |                           |A küszöb érték, amelynél az értesítésre aktiválva van.|
|Ablakméret         |Metrikus értesítések  |                           |Az időtartam, amely riasztási figyelése a küszöbértékét alapján. 5 perc és 1 nap között kell lennie. ISO 8601 időtartam formátumban.|
|timeAggregation    |Metrikus értesítések  |"Átlagos", "Vezetéknév", "Maximum", "Minimum", "Nincs", "összesen" | Hogyan gyűjtött adatok időbeli használható együtt. Az alapértelmezett érték átlagát. [Engedélyezett értékek listája megtalálható](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|műveleti jel           |Metrikus értesítések  |                           |A operátor beállítása küszöbértéknél mérőszám aktuális adatainak összehasonlításához.|
|subscriptionId     |Y                  |                           |Azure előfizetés azonosítójával.|
|resourceGroupName  |Y                  |                           |Az erőforráscsoport a probléma által sújtott erőforrás nevét.|
|Erőforrásnév       |Y                  |                           |A probléma által sújtott erőforrás erőforrás nevét.|
|Erőforrástípus       |Y                  |                           |A probléma által sújtott erőforrás erőforrástípus.|
|resourceId         |Y                  |                           |Erőforrás-azonosító a probléma által sújtott erőforrás.|
|resourceRegion     |Y                  |                           |Régió vagy az erőforrás-probléma által sújtott helyen.|
|portalLink         |Y                  |                           |Közvetlen hivatkozás a portál erőforrás összefoglaló lapra.|
|Tulajdonságok         |N                  |Nem kötelező.                   |A Set `<Key, Value>` párban (azaz `Dictionary<String, String>`), amely tartalmazza az esemény részleteit. A Tulajdonságok mező kitöltése nem kötelező. Egy egyéni felhasználói felületi vagy logikai app-alapú munkafolyamat-felhasználók adhat meg a tartalom keresztül továbbíthatók kulcs/értékét. Az alternatív átadni az egyéni tulajdonságok vissza az webhook módja a webhook uri magát (a lekérdezés paraméterei) keresztül|


>[AZURE.NOTE] A Tulajdonságok mező csak beállítható, hogy a [Azure Monitor REST API -t](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="next-steps"></a>Következő lépések

- További tudnivalók az Azure értesítések és a videó az [Azure riasztások integráció a PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080) webhooks
- [Azure riasztások Azure automatizálási parancsfájlokat (Runbooks) végrehajtása](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Logika alkalmazást használni a via Twilio SMS küldése az Azure értesítés](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Üzenet küldése egy tartalékidő Azure jelzést logika alkalmazás segítségével](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Üzenet küldése egy Azure sorba Azure jelzést logika alkalmazás segítségével](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
