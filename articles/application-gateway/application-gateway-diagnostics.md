<properties 
   pageTitle="Az access és a teljesítmény naplókról és a mértékek figyelése alkalmazás átjáró |} Microsoft Azure"
   description="Megtudhatja, hogy miként engedélyezheti és az alkalmazás átjáró Access és a teljesítmény naplók kezelése"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Diagnosztikai naplózás és mérőszámok alkalmazás átjáró

Azure lehetővé teszi a Lync naplózási és mérőszámok erőforrás

A [**naplózás**](#enable-logging-with-powershell) - naplózás lehetővé teszi, hogy a teljesítmény, az access és a más naplók mentése vagy egy erőforrás az ellenőrzés céljából az elfogyasztott.

[**Mértékek**](#metrics) - alkalmazás átjáró jelenleg még egy mérőszám. A mérőszám azt méri, az alkalmazás átjáró bájt másodpercenként a kapacitása.

Különböző típusú naplók Azure-ban kezelése és hibaelhárítása alkalmazás átjárók használja. Ezeket a naplókat egy része a portálon keresztül is elérhető, és az összes naplók Azure blob-tárolóhoz kiolvasott, aztán megjelenése a különböző eszközök, például a [Napló Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), az Excel és a PowerBI. Többet is megtudhat a különböző típusú naplók az alábbi listából:

- **Naplók:** [Azure naplókat](../monitoring-and-diagnostics/insights-debugging-with-events.md) (korábbi nevén működési naplók) használatával megtekintheti az összes művelet Azure-előfizetése, és állapotuk elé. Naplókat alapértelmezés szerint engedélyezve vannak, és megtekintheti az Azure előzetes portálon.
- **Naplók elérése:** A napló alkalmazás átjáró access mintát megtekintéséhez és elemzéséhez a fontos információkat hívó IP, kért, URL-válasz késés, például vissza a kódot, bájt és kicsinyítés is használhatja. Access napló minden 300 másodperc gyűjtött. A napló alkalmazás átjáró példányonként egy rekordot tartalmaz. Az alkalmazás átjárópéldány azonosítania kell magát "instanceId" tulajdonságban.
- **Teljesítménybeli napló:** A napló használatával megtekintheti, hogyan alkalmazás átjárópéldány hajt végre. A napló rögzíti a teljesítményadatok alapon egy példány többek között a teljes kérelem served, átviteli bájtban, kérések teljes served, hibás kérés darab, a megfelelő és sérült háttéradatbázis példányok száma. Teljesítménybeli napló 60 másodpercenként gyűjtött.
- **Tűzfal naplók:** Az észlelési vagy a megakadályozási mód-alkalmazás átjáró webes alkalmazás tűzfallal beállított keresztül bejelentkezett kéréseket megtekintése a napló is használhatja.

>[AZURE.WARNING] Naplók csak érhetők el az erőforrás-kezelő telepítési modell forrásokkal. A klasszikus telepítési modell erőforrások naplók nem használhat. Egy jobban megértéséhez két modellek, az [erőforrás-kezelő Pivotbeli üzembe és a klasszikus telepítési](../resource-manager-deployment-model.md) cikk hivatkozást.

## <a name="enable-logging-with-powershell"></a>A PowerShell naplózás engedélyezése

Naplózás minden erőforrás-kezelő erőforrás esetén automatikusan aktiválja. Engedélyezni kell a hozzáférés és a teljesítmény el ezeket a naplók elérhető adatok gyűjtése naplózás. A naplózás engedélyezése olvassa el az alábbi lépéseket: 

1. Megjegyzés: a naplóadatokat tároló tároló fiókja erőforrás-azonosító. Az űrlap lenne: /subscriptions/\<subscriptionId\>/resourceGroups/\<erőforráscsoport neve\>/providers/Microsoft.Storage/storageAccounts/\<tároló fióknév\>. Az előfizetés bármely tárterület-fiók használható. Az információk keresése az előnézeti portal segítségével.

    ![Megtekintése a portálon - alkalmazás átjáró diagnosztika](./media/application-gateway-diagnostics/diagnostics1.png)

2. Megjegyzés: az alkalmazás átjáró erőforrás-azonosító mely naplózás engedélyezésük. Az űrlap lenne: /subscriptions/\<subscriptionId\>/resourceGroups/\<erőforráscsoport neve\>/providers/Microsoft.Network/applicationGateways/\<alkalmazás átjáró neve\>. Ezek az információk keresése a betekintő portal segítségével.

    ![Megtekintése a portálon - alkalmazás átjáró diagnosztika](./media/application-gateway-diagnostics/diagnostics2.png)

3. Az alábbi powershell-parancsmag használatával, a diagnosztikai naplózás engedélyezése:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] naplókat nincs szüksége egy külön tárterület-fiókkal. Az access és a teljesítmény naplózás tárhely használatának szolgáltatás költségeket vonz.

## <a name="enable-logging-with-azure-portal"></a>Azure Portal segítségével naplózás engedélyezése

### <a name="step-1"></a>Lépés: 1

Nyissa meg az erőforrás az Azure-portálon. Kattintson **a diagnosztikai naplók**. Ha ez az első alkalommal diagnosztika konfigurálása a lap néz ki az alábbi képen:

Alkalmazás átjáró 3 naplók érhetők el.

- Az Access napló
- Teljesítménybeli napló
- Tűzfal napló

Kattintson a **Diagnosztika bekapcsolása** adatgyűjtés indítása gombra.

![diagnosztikai beállítás lap][1]

### <a name="step-2"></a>Lépés: 2

Kattintson a **Diagnosztika beállításainak** lap a beállításokat, hogy hogyan a diagnosztikai naplók értékre. Ebben a példában a napló analytics a naplókat tárolására szolgál. **Log Analytics** a munkaterület konfigurálása csoportban kattintson a **Konfigurálás** gombra. Esemény hubok és tároló fiókkal, valamint a diagnosztikai naplók mentése használható.

![diagnosztikai lap][2]

### <a name="step-3"></a>3. lépés

Válassza a meglévő MOBILE munkaterülethez, vagy hozzon létre egy újat. Ez a példa egy már meglévő használatos.

![Mobile-munkaterületek][3]

### <a name="step-4"></a>Lépés: 4

Amikor elkészült, erősítse meg a beállításokat, és kattintson a **Mentés** gombra a beállítások mentéséhez.

![kijelölés megerősítése][4]

## <a name="audit-log"></a>Napló

Ez a napló (korábbi nevén a "működési napló") generálja Azure alapértelmezés szerint.  A naplók 90 napon megmaradnak az Azure-féle eseménynaplók áruházban. További tudnivalók a naplók az [események megtekintése és naplók](../monitoring-and-diagnostics/insights-debugging-with-events.md) cikk olvasásával.

## <a name="access-log"></a>Az Access napló

A napló csak jön létre, ha azt engedélyezte a egy alkalmazás átjáró alap, mint az előző lépésben részletes per. Az adatok a tárterület-fiókot a naplózás engedélyezésekor a megadott vannak tárolva. Minden egyes access alkalmazás átjáró be van jelentkezve JSON formátumban, az alábbi példában látható módon:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Teljesítménybeli napló

A napló csak jön létre, ha engedélyezte a egy alkalmazás átjáró alap, mint az előző lépésben részletes per. Az adatok a tárterület-fiókot a naplózás engedélyezésekor a megadott vannak tárolva. A következő adatokat a rendszer rögzíti:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Tűzfal napló

A napló csak jön létre, ha engedélyezte a egy per alkalmazás átjáró alap az előző lépéseket ismertetett módon. A napló is szükséges, hogy a webes alkalmazás tűzfal-alkalmazás átjáró kell állítani. Az adatok a tárterület-fiókot a naplózás engedélyezésekor a megadott vannak tárolva. A következő adatokat a rendszer rögzíti:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Megtekintése és elemzése a napló

Megtekintheti és elemzéséhez naplózási napló használata az alábbi módszerek egyikével:

- **Azure eszközök:** Adatok beolvasása a naplókat Azure PowerShell, az Azure parancssor CLI (), az Azure REST API-t vagy az Azure előzetes portálon keresztül.  Lépésenkénti útmutatást adunk mindegyik módszernek vannak részletes [naplózás műveletek az erőforrás-kezelő](../resource-group-audit.md) című témakörben.
- **Power BI:** Ha még nincs egy [Power BI](https://powerbi.microsoft.com/pricing) -fiókkal, akkor az ingyenes is próbálkozhat. [Azure naplókat tartalom készült Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) segítségével elemezheti az adatokat az előre beállított irányítópultokat használata a-, illetve testre szabhatja.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Megtekintése és elemzése az access, a teljesítmény és a tűzfalat napló

Azure [Napló Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) gyűjthet a számláló és a eseménynaplójának fájljait, ahol a Blob-tároló fiók megjelenítések és elemzése a naplók hatékony keresési lehetőségeket is.

Is csatlakozhat fiókjához tárhely és a JSON naplóbejegyzések, az access és a teljesítmény naplók beolvasásához. Miután letöltötte a JSON-fájlokat, akkor a CSV és az Excel, a PowerBI és más adatok képi megjelenítés eszköz alakíthatók.

>[AZURE.TIP] Ha ismeri a Visual Studio és alapfogalmak állandók és C# változók értékének módosítása, a [naplófájl konverter eszközök](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github elérhető is használhatja.

## <a name="metrics"></a>Mértékek

Mértékek lehetővé teszi az egyes Azure erőforrások, ahol megtekintheti a portál teljesítményét számláló. Alkalmazás átjáró egy mérőszám érhető el ez a cikk írása idején. Ez a mérőszám átviteli, és a portálon is láthatja. Nyissa meg az alkalmazás átjáró, majd kattintson a **Mértékek**.  Átviteli az értékek megjelenítése a **rendelkezésre álló mértékek** szakaszban jelölje ki. Az alábbi képen egy példa az adatok jelennek meg más idő tartományait használható szűrőkkel megtekintheti.

Mértékek támogatja az aktuális listájának megtekintéséhez keresse fel az [Azure monitorral rendelkező támogatott mérőszámok](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![metrikus megtekintése][5]

## <a name="alert-rules"></a>A figyelmeztetési szabályok

A figyelmeztetési szabályok indítható el az erőforrás mértékek alapján. Ez azt jelenti, hogy az alkalmazás átjárót, értesítés is hívja fel a webhook vagy e-mailek rendszergazda esetén az alkalmazás átjáró a átviteli fölött, alatt vagy a küszöbértéket idő adott időszakra vonatkozóan.

A következő példa azt ismerteti, hogy egy szabályt, amely e-mailt küld egy rendszergazdának után egy átviteli küszöb nem tettek létrehozásának folyamatát.

### <a name="step-1"></a>Lépés: 1

Kattintson a **metrikus értesítés hozzáadása** a start gombra. Ez a lap is az a mértékek lap érhető el.

![a figyelmeztetési szabályok lap][6]

### <a name="step-2"></a>Lépés: 2

A **szabály hozzáadása** lap töltse ki a nevét, a feltételt, és a szakaszok értesítést lehetőséget, és kattintson az **OK gombra** a feladónak.

A **feltétel** sorkijelölőre lehetővé teszi a 4 értéket, **nagyobb, mint**, **nagyobb vagy egyenlő**, **kisebb, mint**vagy **kisebb vagy egyenlő**.

Az **időszak** Adatkijelölő lehetővé teszi, hogy az 5 perc 6 órával időszak választ.

**E-mailek tulajdonosok, a munkatársai, és a olvasói** kiválasztásával az e-mailt dinamikus lehet a felhasználók férnek hozzá az adott erőforrás alapján. Egyéb esetben felhasználók vesszővel elválasztott listája biztosítható, hogy a **további rendszergazdai email(s)** rendelés_részletei.mennyiség.

![a szabály lap hozzáadása][7]

Ha a küszöbértékét megsértése esetén e-mailben megérkezik az alábbi képen egy hasonló:

![e-mailek megsértése küszöbérték][8]

A riasztások listája látható egy mérőszám riasztás létrehozás és áttekintést nyújt a figyelmeztetési szabályokat után.

![figyelmeztetést megtekintése][9]

Többet szeretne tudni az értesítéseket, látogasson el a [fogadás értesítések](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

További információ a webhooks, és hogyan használhatja őket az értesítések megérthető, látogasson el a [egy webhook a Azure metrikus értesítés beállítása](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Következő lépések

- A számláló és a [Napló Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) eseménynaplók ábrázolása 
- [Megjelenítés a Power bi Azure-naplókat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbejegyzés.
- [Megtekintése és elemzése a Power BI és az egyéb Azure naplókat](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbejegyzést.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png