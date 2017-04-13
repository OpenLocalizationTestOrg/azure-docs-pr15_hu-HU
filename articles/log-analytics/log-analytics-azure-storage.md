<properties
    pageTitle="A napló Analytics áttekintése Azure tároló adatok gyűjtése |} Microsoft Azure"
    description="Azure erőforrások írhat naplókról és a mértékek Azure tárterület-fiókjába, gyakran Azure diagnosztika segítségével. Log Analytics adat index és a kereshető legyen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Azure tároló adatgyűjtést napló Analytics – áttekintés

Azure sok erőforrást is tudják az írás naplók és mértékek Azure tárterület-fiókjába. Log Analytics is az adatok felhasználása és egyszerűvé válik az Azure erőforrások figyelése.

Azure tároló írni egy erőforrás előfordulhat, hogy Azure diagnosztika használja, vagy van a saját módja az adatok írásához a. Az adatok írhatók különböző formátumokban a következő helyek valamelyikén:

+ Azure-táblából
+ Azure blob
+ EventHub

Log Analytics írja be az adatoknak [a diagnosztikai naplók Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)Azure szolgáltatások támogat. Ezenkívül a napló Analytics naplókról és a különböző formátumú és helyek mértékek kimeneti más szolgáltatásokat támogatja.  

>[AZURE.NOTE] Normál Azure adatsebesség tárhely és a tranzakciók, amikor elküldi a tárterület-fiók diagnosztika és mikor napló Analytics adatokat olvas be a tárterület-fiókjából terheli.

![Azure tárterület-diagram](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Támogatott Azure erőforrások

Log Analytics gyűjt adatokat a következő Azure erőforrásokat:

| Erőforrás típusa | Naplók (diagnosztikai kategóriák) | Log Analytics megoldás |
| --------------------------------------- | -------------------------------- | --------------- |
| Alkalmazás Hírcsatornájában | Elérhetőség <br> Egyéni események <br> A kivételek <br> Kérések <br> | Alkalmazás háttérismeretek (előzetes verzió) |
| API-kezelés | | *nincs lehetőség* (Előzetes verzió) |
| Automatizálási <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (előzetes verzió) |
| Fő tárolóból elemre <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (előzetes verzió) |
| Alkalmazás átjáró <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (előzetes verzió) |
| Hálózati biztonsági csoport <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (előzetes verzió) |
| Szolgáltatás háló                          | ETWEvent <br> Műveleti esemény <br> Megbízható szereplő esemény <br> Megbízható szolgáltatás esemény| ServiceFabric (előzetes verzió) |
| Virtuális gépeken futó | Linux Syslog <br> A Windows-eseménynaplózás <br> IIS napló <br> A Windows ETWEvent | *nincs lehetőség* |
| Webes szerepkörök <br> Dolgozó szerepkörök | Linux Syslog <br> A Windows-eseménynaplózás <br> IIS napló <br> A Windows ETWEvent | *nincs lehetőség* |

>[AZURE.NOTE] A nyomon követés Azure virtuális gépeken futó (Linux és Windows), javasoljuk, a [naplófájl Analytics virtuális bővítmény](log-analytics-azure-vm-extension.md)telepítése. A agent mélyebb hírcsatornájában, mint a tárhely írt diagnosztika a virtuális gépeken biztosít.

Segítséget nyújthat nekünk MOBILE a [Visszajelzés lap](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy)szavazás szerint elemezheti az további naplók fontossági.


- Ha többet szeretne megtudni, hogyan napló Analytics elolvashatja a naplókat, amelyek támogatják a [diagnosztikai naplók Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)Azure szolgáltatásokból lásd: az [Azure elemzése a diagnosztikai naplók napló Analytics használatával](log-analytics-azure-storage-json.md) :
  - Azure kulcs tárolóból elemre
  - Azure automatizálási
  - Alkalmazás átjáró
  - Hálózati biztonsági csoportok
- Lásd: [használata blob-tárolóhoz IIS és táblatároló eseményekre vonatkozóan:](log-analytics-azure-storage-iis-table.md) Ha többet szeretne tudni, hogyan napló Analytics elolvashatja a naplókat Azure táblatárolóhoz vagy blob-tároló írt IIS naplók adott írási diagnosztika Services együtt:
  - Szolgáltatás háló
  - Webes szerepkörök
  - Dolgozó szerepkörök
  - Virtuális gépeken futó


Alkalmazás háttérismeretek a saját kép, és azt használja a folyamatos exportálás blob-tároló. Bekapcsolódás a személyes kép, lépjen kapcsolatba a Microsoft Account team, vagy a részletek, a [Visszajelzés webhely](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights)hivatkozik.

## <a name="next-steps"></a>Következő lépések

- [Azure elemzése a diagnosztikai naplók napló Analytics használatával](log-analytics-azure-storage-json.md) olvassa el a naplók az Azure blob-tárolóhoz JSON formátumban, hogy írható diagnosztika szolgáltatásokat.
- [Használata blob-tárolóhoz IIS és táblatároló eseményekre vonatkozóan](log-analytics-azure-storage-iis-table.md) olvassa el a naplók az Azure táblatároló vagy IIS naplók blob-tároló írt e írási diagnosztika-szolgáltatások.
- [Megoldások engedélyezése](log-analytics-add-solutions.md) az adatok betekintést nyújt.
- [Használja a keresési lekérdezések](log-analytics-log-searches.md) hozzáadva elemezheti az adatokat.
