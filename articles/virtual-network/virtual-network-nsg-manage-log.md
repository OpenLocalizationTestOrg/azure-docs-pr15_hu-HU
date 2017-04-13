<properties
   pageTitle="Figyelje műveletek, események és számláló NSGs |} Microsoft Azure"
   description="Megtudhatja, hogy miként számláló, események és műveleti NSGs naplózásának engedélyezése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/14/2016"
   ms.author="jdial" />

#<a name="log-analytics-for-network-security-groups-nsgs"></a>Log elemzésének hálózati biztonsági csoportok (NSGs)

Különböző típusú naplók Azure-ban kezelése és hibaelhárítása NSGs használja. Ezeket a naplókat egy része a portálon keresztül is elérhető, és az összes naplók Azure blob-tárolóhoz kiolvasott, aztán megjelenése a különböző eszközök, például a [Napló Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), az Excel és a PowerBI. Akkor talál további információt a különböző típusú naplók az alábbi listából.

- **Naplók:** [Azure naplókat](../monitoring-and-diagnostics/insights-debugging-with-events.md) (korábbi nevén működési naplók) használatával megtekintheti az összes művelet a Azure előfizetés(ek) és állapotuk elé. Naplókat alapértelmezés szerint engedélyezve vannak, és megtekintheti az Azure előzetes portálon.
- **Eseménynaplók:** Milyen NSG szabályok érvényesek VMs és a MAC-címe alapján példány szerepkörök megtekintése a napló is használhatja. A szabályok állapota 60 másodpercenként gyűjtött.
- **Naplók számláló:** A napló segítségével megtekintése hányszor NSG szabályok volt alkalmazott megtagadhatja vagy forgalmának engedélyezésére.

>[AZURE.WARNING] Naplók csak érhetők el az erőforrás-kezelő telepítési modell forrásokkal. A klasszikus telepítési modell erőforrások naplók nem használható. Jobb megértése két modellek, az [erőforrás-kezelő Pivotbeli üzembe és a klasszikus telepítési](../resource-manager-deployment-model.md) témakört a hivatkozást.

##<a name="enable-logging"></a>A naplózás engedélyezése
Naplózás esetén automatikusan aktiválja a minden alkalommal minden erőforrás-kezelő erőforrás. Esemény és számláló el ezeket a naplók elérhető adatok gyűjtése naplózás engedélyezése szüksége. Naplózás engedélyezéséhez, kövesse az alábbi lépéseket.

1.  Bejelentkezés az [Azure-portálon](https://portal.azure.com). Ha még nem rendelkezik a meglévő hálózat biztonsági csoportot, [Hozzon létre egy NSG](virtual-networks-create-nsg-arm-ps.md) , a folytatás előtt.

2.  Az előnézet-portálon kattintson a **Tallózás** >> **hálózati biztonsági csoportokat**.

    ![Előzetes portál - hálózati biztonsági csoportok](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. Jelölje ki a meglévő hálózat biztonsági csoportot.

    ![Előzetes portál - hálózati biztonsági csoport beállításai](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. A **Beállítások** lap kattintson a **Diagnosztika lehetőségre**, és a **diagnosztikai** ablakban melletti **állapot**, kattintson **a**
5. Kattintson a **Beállítások** lap a **Tárterület-fiókot**, és jelölje ki egy meglévő tároló fiókot vagy hozzon létre egy újat.  

>[AZURE.INFORMATION] naplókat nincs szüksége egy külön tárterület-fiókkal. Esemény és szabály naplózás tárhely használatának merülnek fel, amelyekre kezelési költséget.

6. A legördülő listában csak a **Tárhely fiók**csoportjában válassza ki, hogy jelentkezzen be az esemény vagy számláló, és kattintson a **Mentés**.

    ![Előzetes portál - diagnosztikai naplók](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## <a name="audit-log"></a>Napló
Ez a napló (korábbi nevén a "működési napló") generálja Azure alapértelmezés szerint.  A naplók 90 napon megmaradnak az Azure-féle eseménynaplók áruházban. További tudnivalók a naplók az [események megtekintése és naplók](../monitoring-and-diagnostics/insights-debugging-with-events.md) cikk olvasásával.

## <a name="counter-log"></a>A számláló napló
A napló csak akkor jön létre, ha azt engedélyezte alapon egy per NSG részletes felett. Az adatok a tárterület-fiókot a naplózás engedélyezésekor a megadott vannak tárolva. Erőforrások alkalmazott szabályok be van jelentkezve JSON formátumot, az alább látható módon.

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## <a name="event-log"></a>Eseménynaplójának
A napló csak akkor jön létre, ha azt engedélyezte alapon egy per NSG részletes felett. Az adatok a tárterület-fiókot a naplózás engedélyezésekor a megadott vannak tárolva. A következő adatokat a rendszer rögzíti:

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## <a name="view-and-analyze-the-audit-log"></a>Megtekintése és elemzése a napló
Megtekintheti és elemzéséhez naplózási napló használata az alábbi módszerek egyikével:

- **Azure eszközök:** Adatok beolvasása a naplókat Azure PowerShell, az Azure parancssor CLI (), az Azure REST API-t vagy az Azure előzetes portálon keresztül.  Lépésenkénti útmutatást adunk mindegyik módszernek vannak részletes [naplózás műveletek az erőforrás-kezelő](../resource-group-audit.md) című témakörben.
- **Power BI:** Egy [Power BI](https://powerbi.microsoft.com/pricing) -fiókkal már nem lehet próbálja ki az ingyenes. [Azure naplókat tartalom készült Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/) segítségével elemezheti az adatokat az előre beállított irányítópultokat használata a-, illetve testre szabhatja.

## <a name="view-and-analyze-the-counter-and-event-log"></a>Megtekintése és elemzése a számláló és az Eseménynapló

Azure [Napló Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) gyűjthet a számláló és a eseménynaplójának fájljait, ahol a Blob-tároló fiók megjelenítések és elemzése a naplók hatékony keresési lehetőségeket is.

Is csatlakozhat fiókjához tárhely és a JSON naplóbejegyzések az eseményt, és a számláló naplók beolvasásához. Miután letöltötte a JSON-fájlokat, akkor a CSV és az Excel, a PowerBI és más adatok képi megjelenítés eszköz alakíthatók.

>[AZURE.TIP] Ha ismeri a Visual Studio és alapfogalmak állandók és C# változók értékének módosítása, a [naplófájl konverter eszközök](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github elérhető is használhatja.

## <a name="next-steps"></a>Következő lépések

- A számláló és a [Napló Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) eseménynaplók ábrázolása
- [Megjelenítés a Power bi Azure-naplókat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbejegyzés.
- [Megtekintése és elemzése a Power BI és az egyéb Azure naplókat](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbejegyzést.
