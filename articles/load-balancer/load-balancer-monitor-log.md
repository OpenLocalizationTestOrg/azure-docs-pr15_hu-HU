<properties
   pageTitle="Figyelje műveletek, események és számláló terheléselosztó |} Microsoft Azure"
   description="Megtudhatja, hogy miként riasztási események engedélyezése és állapot állapot naplózásának az Azure terheléselosztó probe"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Log elemzésének Azure terheléselosztó (előzetes verzió)

Különböző típusú naplók Azure-ban kezelése és hibaelhárítása terheléselosztókkal használja. Ezeket a naplókat egy része a portálon keresztül is elérhető. Az összes naplók Azure blob-tárolóhoz kiolvasott, és nézett meg a különböző eszközök, például az Excel vagy a PowerBI. Akkor talál további információt a különböző típusú naplók az alábbi listából.

- **Naplók:** [Azure naplókat](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (korábbi nevén működési naplók) használatával megtekintheti az összes művelet a Azure előfizetés(ek) és állapotuk elé. Naplókat alapértelmezés szerint engedélyezve vannak, és megtekintheti az Azure-portálon.
- **Értesítés az eseménynaplók:** A napló használatával megtekintheti, hogy milyen terheléselosztó riasztásainak felvetett. A állapota a terheléselosztó öt percenként gyűjtött. Ha a betöltés terheléselosztó riasztási esemény akkor következik be, a napló csak írott.
- **Állapot vizsgálati naplók:** A napló keressen vizsgálati állapot ellenőrzése állapotát, hány példánya kapcsolódik az internethez a betöltés terheléselosztó háttéradatbázist és százalékos aránya virtuális gépeken futó hálózati forgalmának engedélyezésére fogad a terheléselosztó is használhatja. A napló a vizsgálati esemény állapotváltozás íródott.

>[AZURE.IMPORTANT] Jelentkezzen be az analitikai jelenleg csak az internetes terheléselosztókkal működik. Naplók csak érhetők el az erőforrás-kezelő telepítési modell forrásokkal. A klasszikus telepítési modell erőforrások naplók nem használható. A telepítési modellek kapcsolatos további tudnivalókért olvassa el a [erőforrás-kezelő Pivotbeli üzembe és a klasszikus telepítési](../../articles/resource-manager-deployment-model.md)című témakört.

## <a name="enable-logging"></a>A naplózás engedélyezése

Naplózás minden erőforrás-kezelő erőforrás esetén automatikusan aktiválja. Esemény és állapot vizsgálati el ezeket a naplók elérhető adatok gyűjtése naplózás engedélyezése szüksége. A naplózás engedélyezése kövesse az alábbi lépéseket.

Bejelentkezés az [Azure-portálon](http://portal.azure.com). Ha még nem rendelkezik egy terheléselosztó, [Hozzon létre egy terheléselosztó](load-balancer-get-started-internet-arm-ps.md) , a folytatás előtt.

1. Az-portálon kattintson a **Tallózás gombra**.
2. Jelölje ki a **terheléselosztókkal**.

    ![-portál és-terheléselosztó](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Jelölje ki egy meglévő terheléselosztó >> **Összes beállítást**.
4. A neve alatt látható a terheléselosztó párbeszédpanel jobb oldalán **Figyelés**görgessen le, kattintson a **Diagnosztika lehetőségre**.

    ![portál - terhelés-terheléselosztó-beállítások](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. A **Diagnosztika** ablak **állapot**jelölje ki **a**.
6. Kattintson a **tárterület-fiókra**.
7. A **Naplók**jelöljön ki egy meglévő tárterület-fiókot vagy hozzon létre egy újat. A csúszka használatával azt állapíthatja meg, hogy hány napig esemény dat fog megmaradjanak az eseménynaplók. 8. Kattintson a **Mentés**gombra.

    ![Portál - diagnosztikai naplók](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] naplókat nincs szüksége egy külön tárterület-fiókkal. A használja az esemény és állapot tárolási vizsgálati naplózás merülnek fel, amelyekre kezelési költséget.

## <a name="audit-log"></a>Napló

A napló alapértelmezés szerint jön létre. A naplók 90 napon megmaradnak az Azure-féle eseménynaplók áruházban. További tudnivalók a naplók az [események megtekintése és naplók](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) cikk olvasásával.

## <a name="alert-event-log"></a>Riasztási eseménynaplójában talál.

A napló csak jön létre, ha azt engedélyezte a egy / terhelési terheléselosztó alap. Az események naplózása JSON formátumban, és a naplózás engedélyezésekor a megadott tárterület-fiókjában tárolt. Az alábbi képen az esemény.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

A JSON eredménye a *eventname* tulajdonság, amely leírja a létrehozott jelzést terheléselosztó okát jeleníti meg. Ebben az esetben a létrehozott figyelmeztető TCP port Erőforrásfogyás okozta forrás IP hálózati Címfordítást (SNAT) korlátai miatt volt.

## <a name="health-probe-log"></a>Állapot vizsgálati napló

A napló csak jön létre, ha azt engedélyezte a egy / terhelési terheléselosztó módon részletes fenti. Az adatok a tárterület-fiókot a naplózás engedélyezésekor a megadott vannak tárolva. "Háttérismeretek naplók, loadbalancerprobehealthstatus" nevű tároló létrejön, és a következő adatokat a rendszer rögzíti:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

A JSON eredménye a Tulajdonságok mezőben látható vizsgálati állapot állapotára vonatkozó alapadatok. A *dipDownCount* tulajdonság a háttéradatbázist, amelyek nem kapnak a hálózati forgalmának engedélyezésére miatt sikertelen vizsgálati válaszok példányok száma látható.

## <a name="view-and-analyze-the-audit-log"></a>Megtekintése és elemzése a napló

Megtekintheti és elemzéséhez naplózási napló használata az alábbi módszerek egyikével:

- **Azure eszközök:** Adatok beolvasása a naplókat Azure PowerShell, az Azure parancssor CLI (), az Azure REST API-t vagy az Azure előzetes portálon keresztül. Lépésenkénti útmutatást adunk mindegyik módszernek vannak részletes [naplózás műveletek az erőforrás-kezelő](../../articles/resource-group-audit.md) című témakörben.
- **Power BI:** Nem már rendelkezik egy [Power BI](https://powerbi.microsoft.com/pricing) -fiókkal, ingyenesen is próbálja. [Azure naplókat meg tartalom a Power BI csomag](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), elemezheti az adatokat az előre beállított irányítópultok, vagy testre szabható nézetek az igényeinek, az igényeknek megfelelően alakíthatja.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Megtekintése és elemzése az állapot vizsgálati és eseménynaplójának

Tárterület csatlakozni, valamint a JSON naplóbejegyzések az eseményt, és állapota vizsgálati naplók szüksége. Miután letöltötte a JSON-fájlokat, akkor a CSV és az Excel, a PowerBI és más adatok képi megjelenítés eszköz alakíthatók.

>[AZURE.TIP] Ha ismeri a Visual Studio és alapfogalmak állandók és C# változók értékének módosítása, a [naplófájl konverter eszközök](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github elérhető is használhatja.

## <a name="additional-resources"></a>További források

- [Megjelenítés a Power bi Azure-naplókat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbejegyzés.
- [Megtekintése és elemzése a Power BI és az egyéb Azure naplókat](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbejegyzést.

## <a name="next-steps"></a>Következő lépések

[Betöltési terheléselosztó szondákat ismertetése](load-balancer-custom-probe-overview.md)
