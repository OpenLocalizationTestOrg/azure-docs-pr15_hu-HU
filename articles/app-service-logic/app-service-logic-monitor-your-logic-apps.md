<properties 
    pageTitle="Figyelheti az Azure App szolgáltatásban összefüggés-alkalmazások |} Microsoft Azure" 
    description="Logika alkalmazás tette megtekintése" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>A logikai alkalmazások figyelése

[Összefüggés-alkalmazás létrehozása](app-service-logic-create-a-logic-app.md)után megtekintheti a teljes előzményeit végrehajtása során az Azure-portálon.  Lync-események valós idejű is beállíthatja szolgáltatásai, például Azure diagnosztika és Azure értesítések és riasztási eseményekre vonatkozóan, például "Ha több mint 5 futtatása nem után egy órával."

## <a name="monitor-in-the-azure-portal"></a>Az Azure-portálon monitor

Az előzmények megtekintéséhez válassza a **Tallózás gombra**, és válassza a **Logika alkalmazások**. Az előfizetés az összes logika alkalmazások listája.  Jelölje ki a figyelni kívánt logika alkalmazást.  Láthatja, hogy minden műveletek és logika alkalmazással kapcsolatos előfordult indítók listáját.

![– Áttekintés](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Létezik néhány szakaszok meg ez a lap hasznosak lehetnek:

- **Összefoglaló** sorolja fel, **az összes elindul** , és a **Kiváltó ok mező előzmények**
    - Futtatja a lista **összes futtatja** a logika alkalmazás legújabb verzióját.  Kattintson a Futtatás kapcsolatos részletekért sor, vagy kattintson a további futtatja felsoroló csempére.
    - **Eseményindító előzmények** logika alkalmazással kapcsolatos eseményindító tevékenység sorolja fel.  Eseményindító tevékenység sikerült az új adatok (például szeretné látni, ha egy új fájlt FTP hozzáadva), "Kihagyott" ellenőrzi a "Sikeres" tehát minden olyan esetben egy logikai alkalmazás visszaadott adat, vagy "nem" sikerült megfelelő konfigurációban a hibát.
- **Diagnosztikai** lehetővé teszi, hogy a futtatókörnyezet részletek és események megtekintése és előfizetés ilyen [Azure értesítések](#adding-azure-alerts)

>[AZURE.NOTE] A többi belül a logika alkalmazás szolgáltatás titkosítja összes futtatókörnyezet részletek és eseményeket. Egy felhasználó a nézetben kérésre csak azok vannak visszafejti. Az események való hozzáférést is szabályozhatja által Azure Role-Based Access vezérlő (RBAC).

### <a name="view-the-run-details"></a>A futtatási részleteinek megtekintése

Futó feladatok listáját futtatása **állapotát**, a **Kezdés időpontja**és az adott **időtartam** látható. Jelölje be a részletek megtekintéséhez, futtassa a sor.

A felügyeleti nézet megmutatja, a Futtatás, a ráfordítások és a kimeneti értékeket lépésein, és bármelyik hibaüzenetek jelennek meg, hogy occurre kell.

![Futtatás és műveletek](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Ha bármilyen további részletekre, például a Futtatás **Korrelációazonosító** (azaz olyan használható REST API-val) kell, kattintson a a **Részletek futtatása** gombra.  Az összes lépéseket, az állapot és a Futtatás bemenetben/kimeneti értékeket tartalmazó beállítás.

## <a name="azure-diagnostics-and-alerts"></a>Azure diagnosztika és értesítések

A részleteket az Azure-portál és -REST API-val fenti által biztosított, mellett az Azure diagnosztika használandó gazdag további részletek és hibakeresés összefüggés-alkalmazás is beállíthatja.

1. Kattintson a logika alkalmazás lap **Diagnosztika** szakasza
1. Kattintson a **Diagnosztikai beállítások** konfigurálása
1. Adatok elhelyezése egy esemény központi vagy a tárterület-fiók beállítása

    ![Azure diagnosztika beállításainak](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Azure riasztások hozzáadása

Miután diagnosztika be van állítva, minden olyan esetben, ha bizonyos küszöbértéket vannak Metszéspont Azure riasztások is hozzáadhat.  Jelölje ki a **riasztások** csempére és a **Hozzáadás értesítés**a **Diagnosztika** lap.  Ez azt ismerteti, hogy segítségével számos különböző küszöbértékek és mértékek alapján értesítés beállítása.

![Azure riasztási mérőszámok](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

Beállíthatja a **feltétel** **küszöb**és **időszak** tetszés szerint.  Végül állítsa be az értesítés küldése e-mail címre, vagy egy webhook állíthat be.  Is használhatja az [eseményindító kérést](../connectors/connectors-native-reqres.md) egy logikai alkalmazásban futtatni szeretne a figyelmeztető üzenet is látható (például [tartalékidő tegye közzé](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [küldje el a szöveget](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app), vagy [Írjon be egy sorba üzenetet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)dolgot kell tennie).

### <a name="azure-diagnostics-settings"></a>Azure diagnosztika beállításainak

Az alábbi események tartalmaznak a logika alkalmazás és a jelennek állapota esemény részleteit.  Íme egy példa *ActionCompleted* esemény:

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

A két tulajdonság, amely különösen hasznosak nyomon követése és ellenőrzése, hogy *clientTrackingId* és *trackedProperties*.  

#### <a name="client-tracking-id"></a>A nyomon követés ügyfél-azonosító

Az ügyfél-azonosító követés érték fog összehangolására események logika alkalmazás futtatni, például egy logikai app részeként nevű beágyazott munkafolyamatok keresztül.  Az azonosító fog lehet automatikusan létrehozott Ha nincs megadva, de manuálisan adhatja meg az ügyfél-azonosító nyomkövetési az eseményindító megkerülhetők egy `x-ms-client-tracking-id` élőfej a azonosító értéket az eseményindító összehívásban (kérés eseményindító, HTTP eseményindító vagy webhook eseményindító).

#### <a name="tracked-properties"></a>A nyomon követett tulajdonságai

A nyomon követett tulajdonságok alakzatot a munkafolyamat-definíciót bemenetben nyomon követésére a műveletek vagy a kimeneti értékeket a diagnosztikai adatok adhatók meg.  Ez akkor lehet hasznos, ha például egy "rendelés azonosítója" az a telemetriai adatokat nyomon szeretne.  Nyomon követett tulajdonság hozzáadásához helyezze el a `trackedProperties` művelet tulajdonságát.  A nyomon követett tulajdonságok lehet egyetlen nyomon követése egy egyetlen műveletek ráfordítások és a kimeneti értékeket, de használhatja a `correlation` az események között a Futtatás műveletek összehangolására tulajdonságait.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>A megoldások bővítése

A telemetriai az esemény központi vagy tárolás be más szolgáltatásaival, például [Műveletek Management csomagja](https://www.microsoft.com/cloud-platform/operations-management-suite), az [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/)és a [Power BI](https://powerbi.com) szeretné, hogy valós időben nyomon követése a integrációs munkafolyamatok is élvezheti.

## <a name="next-steps"></a>Következő lépések
- [Gyakori példák és -esetek logika alkalmazásai](app-service-logic-examples-and-scenarios.md)
- [Logika alkalmazás telepítési sablon létrehozása](app-service-logic-create-deploy-template.md)
- [Nagyvállalati integrációs szolgáltatások](app-service-logic-enterprise-integration-overview.md)
