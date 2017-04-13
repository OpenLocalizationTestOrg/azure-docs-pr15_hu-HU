<properties
    pageTitle="A Microsoft Azure értesítések áttekintése |} Microsoft Azure"
    description="Értesítések lehetővé teszi Azure erőforrás mértékek, események és naplók figyelése és a Értesítés kérése a megadott feltétel teljesül."
    authors="rboucher"
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Értesítések, a Microsoft Azure-ban – áttekintés


A cikkből megtudhatja, milyen értesítések, juttatások, és hogyan kell – első lépések használja őket.  

## <a name="what-are-alerts"></a>Mik azok az értesítéseket?
Értesítések felügyeleti Azure erőforrás mértékek eseményeket, vagy naplók, majd értesítést kapjanak, ha a megadott feltétel teljesül módszer.

Kaphatnak a riasztások alapján:

- **Metrikus értékek**: Ez az értesítés eseményindítók, ha egy adott mérőszám értékének metszéspontja, amelyek mindkét irányba hozzárendelése küszöbértéket. Ez azt jelenti, akkor mindkét eseményindítók amikor először teljesül a feltétel és majd utána mikor amely feltétel van már nem teljesül.
- **Tevékenység naplóbeli események**: Ez az értesítés válthatnak minden eseményt, vagy csak akkor, ha a megadott számú események fordul elő.

## <a name="alerts-in-different-azure-services"></a>Különböző Azure szolgáltatások riasztásai

Értesítések át más szolgáltatásokat, például érhetők el:

- **Alkalmazás háttérismeretek**: lehetővé teszi, hogy próba- és metrikus értesítések webes. Lásd: [alkalmazás háttérismeretek az értesítések beállítása](../application-insights/app-insights-alerts.md) elemre, és [Monitor elérhetősége webhelyekkel megegyező növeléséhez](../application-insights/app-insights-monitor-web-app-availability.md).
- **Log Analytics (műveletek felügyeleti csomag)**: lehetővé teszi, hogy a diagnosztikai naplók napló Analytics útválasztás. Tevékenységek kezelése csomagja lehetővé teszi, hogy metrikus, bejelentkezés, és más riasztási. További információ [a napló Analytics értesítéseket](../log-analytics/log-analytics-alerts.md)szeretné látni.  
- **Azure Monitor**: lehetővé teszi, hogy a riasztások metrikus értékek és a tevékenység naplóbeli események alapján. Azure Monitor az [Azure Monitor REST API -t](https://msdn.microsoft.com/library/dn931943.aspx)tartalmaz.  További tudnivalókért olvassa el a [használata az Azure-portálra, PowerShell, vagy a parancssor értesítések létrehozása](insights-alerts-portal.md)című témakört.

## <a name="alert-actions"></a>Riasztási műveletek
Beállíthatja, hogy jelzést tegye az alábbiakat:

- Értesítő e-mailek küldése, a szolgáltatás-rendszergazda, további rendszergazdák vagy további e-mail címek, megadott feltételeknek.
- Hívja fel a webhook, amely lehetővé teszi, a további automatizálási műveletek indítása.

 ![Értesítések magyarázata](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Következő lépések

A figyelmeztetési szabályok és konfigurálja az használatával kapcsolatos tájékozódáshoz:

- [Azure portál](insights-alerts-portal.md)
- [A PowerShell](insights-alerts-powershell.md)
- [Parancssori kezelőfelületről](insights-alerts-command-line-interface.md)
- [Azure Monitor REST API-val](https://msdn.microsoft.com/library/azure/dn931945.aspx)
