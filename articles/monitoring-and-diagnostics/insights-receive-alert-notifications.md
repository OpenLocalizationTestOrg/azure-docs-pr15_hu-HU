<properties
    pageTitle="Az Azure-szolgáltatások értesítések fogadása |} Microsoft Azure"
    description="Értesíteni riasztási szabályok feltételek teljesülése esetén."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="receive-alert-notifications"></a>Értesítések fogadása

Az ellenőrző mértékek vagy események, a Azure szolgáltatások alapján értesítés kaphat.

Egy mérőszám érték figyelmeztetést amikor egy adott mérőszám értékének metszéspontja küszöbértéket hozzárendelve, a szabályt aktívvá válik és küldhet értesítést. Az esemény egy szabályt szabály értesítés küldése *minden* esemény, vagy, csak akkor, ha a megadott számú események fordulhat elő.

Amikor létrehoz egy szabályt, válassza a beállítások szeretne küldeni az e-mailben értesítést a szolgáltatás-rendszergazda, és további rendszergazdák vagy egy további rendszergazdát, hogy adhatja meg. A értesítő e-mailt küldi el a szabály aktívvá válik, valamint egy figyelmeztető feltétel megoldódott esetén.

A [REST API -t](https://msdn.microsoft.com/library/azure/dn931945.aspx) vagy a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) segítségével konfigurálhatja és kapcsolatos tájékozódáshoz riasztási szabályok programozás útján.

## <a name="create-an-alert-rule"></a>Hozzon létre egy szabályt

1. A [portálon](https://portal.azure.com/)kattintson a **Tallózás gombra**, majd egy erőforrást, amely érdekli figyelése.

2. Kattintson a a **Műveletek** lens a **riasztási szabályok** csempére.

3. Kattintson a **Hozzáadás értesítés** parancsra.

    ![Értesítés hozzáadása](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. Nevezze el a szabályt, és válasszon egy leírást, amely megjelenik az értesítő e-mailt.

5. Ha bejelöli a **Mértékek** feltételt és a mérőszám érték küszöbértékét fogja választhat. Ez az Azure használja a monitor és ábra riasztási tevékenység időtartamának.

    ![A feltétel és küszöbérték](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. Válassza az **események**elemet, és értesítést kap, amikor egy bizonyos esemény történik is.

    ![Események](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Végezetül megadhatja felelős rendszergazdák értesítő e-mailt küldhet.

**Mentés**gombra kattintva, néhány percen belül fogja értesítés, amikor a kiválasztott mérőszám meghaladja.

## <a name="managing-your-alert-rules"></a>A figyelmeztetési szabályok kezelése

Miután létrehozott egy szabályt, megtekintheti az értesítés előnézetét küszöb és összehasonlítása az előző nap a mérőszám.

![Események](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Ezt a szabályt, és **engedélyezése** vagy **letiltása** , ha szeretné ideiglenes lemondhatja kapcsolatos értesítések természetesen szerkesztheti.

## <a name="next-steps"></a>Következő lépések

* [Kattintson az értesítések konfigurálása webhooks](insights-webhooks-alerts.md) különböző csatornák útvonal értesítések
* [Monitor szolgáltatás mértékek](insights-how-to-customize-monitoring.md) és a szolgáltatás nem érhető el, és válaszol.
* A [Figyelés és diagnosztika engedélyezése](insights-how-to-use-diagnostics.md) részletes magas-gyakoriság mértékek gyűjt a szolgáltatás.
* [Monitor elérhetőségét, és a weblap növeléséhez](../application-insights/app-insights-monitor-web-app-availability.md) alkalmazás Hírcsatornájában láthatja, hogy ha a lapon, a nem működik.
* [Alkalmazás teljesítményének figyelése](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné, ha meg szeretné érteni, hogy pontosan hogyan a kód végrehajtásához a felhőben.
* [Események megtekintése és naplók](insights-debugging-with-events.md) mindent megtudhatja, hogy történt a szolgáltatásban.
* [Szolgáltatásállapot nyomon követése](insights-service-health.md) megtudhatja, hogy Azure észlelt csökkenés vagy szolgáltatás kiküszöbölése teljesítményét.
