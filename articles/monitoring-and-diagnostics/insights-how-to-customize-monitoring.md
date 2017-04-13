<properties
    pageTitle="A Microsoft Azure mértékek áttekintése |} Microsoft Azure"
    description="Megtudhatja, hogy miként testreszabhatja a megfigyeléssel diagramok Azure-ban."
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

# <a name="overview-of-metrics-in-microsoft-azure"></a>Mértékek Microsoft Azure-ban – áttekintés

Azure szolgáltatások nyomon követheti a fő mértékek, amelyek lehetővé teszik a Lync-állapota, teljesítmény, rendelkezésre állásának és a szolgáltatások használatát. Az Azure-portálon tekinthet meg ezeket a mértékek, és használhatja a [REST API -t](https://msdn.microsoft.com/library/azure/dn931930.aspx) vagy a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) a skype_for_businesshoz mérőszámok programozás útján eléréséhez.

Egyes szolgáltatások előfordulhat, diagnosztika bekapcsolása minden mértékek megjelenítéséhez. Mások virtuális gépeken futó, például mértékek egyszerű halmazának fog kapni, de van szüksége ahhoz, hogy a teljes beállítása magas-gyakoriság mértékek. Lásd: a [Figyelés és diagnosztika engedélyezése](insights-how-to-use-diagnostics.md) további információt.

## <a name="using-monitoring-charts"></a>Ellenőrző diagramok használatával

Diagramot tudjon készíteni a mértékek közül bármelyik időszakban, kiválaszthatja, hogy őket.

1. Az [Azure-portálon](https://portal.azure.com/)kattintson a **Tallózás gombra**, majd egy erőforrást, amely érdekli figyelése.

2. A **Figyelés** szakasz tartalmazza az egyes Azure erőforrásokhoz a legfontosabb mérési módja miatt. Például webalkalmazást megegyezik **-összehívások és a hibák**, ahol virtuális gépen szeretné, hogy a **lemez olvasása és írása**és **Processzor százalékos** :  ![figyelés lens](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. A minden diagram gombra kattintva láthatja a **metrikus** lap. Kattintson a lap mellett a diagram van egy olyan táblát, megtekintheti az összesítések, a mérőszámok (például átlag, a legkisebb és legnagyobb, a kiválasztott időtartomány felett). Alatt, amely az erőforrás a riasztási szabályok vonatkoznak.
    ![Metrikus lap](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. A vonalak, amelyek testreszabásához kattintson a diagram **szerkesztése** gombjára, vagy a **diagram szerkesztése** parancs a metrikus lap.

5. Kattintson a lekérdezés szerkesztése lap három műveletet végezheti el:
    - A időtartomány módosítása
    - Az alábbi módokon közötti váltás sáv- és sor
    - Válassza a másik metics ![lekérdezés szerkesztése](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Időtartomány módosítása ugyanolyan egyszerű, mint egy másik tartomány (például a **Múltbeli óra**) kijelölését, és kattintson a **Mentés** elemre a lap alján. **Egyéni**, amely lehetővé teszi, hogy minden időszak alatt válassza az utóbbi 2 hétben is választhat. Megtekintheti például a teljes két héttel, vagy, tegnapi csak 1 óra értéket. Írja be a szövegmezőbe írja be egy másik óra.
    ![Egyéni időtartomány](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. A időtartomány alatt, munkakör válassza ki a mértékek, ha meg szeretné jeleníteni a diagram tetszőleges számú.

8. Ha kattintson a Mentés gombra a módosítások az adott erőforráshoz menti. Ha például két virtuális gépeken futó van, és módosíthatja a diagram egyik, ha nincs hatással a többi.

## <a name="creating-side-by-side-charts"></a>Egymás mellett diagramok létrehozása

A hatékony testre szabott a portálon is hozzáadhat annyi diagramok van szüksége.

1. A lap tetején a **…** menüben kattintson a **csempék felvétele**:  
    ![Menü hozzáadása](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Majd válassza ki a diagram közül választhat a **gyűjtemény** a képernyő jobb oldalán lévő:  ![gyűjtemény](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Ha nem látja a kívánt mérőszám, mindig felveheti egy előre definiált mértékek és **szerkesztése** a diagramra kattintva jelenítse meg a szükséges mérőszám.

## <a name="monitoring-usage-quotas"></a>Használati kvótájának figyelése

A legtöbb mértékek trendjét, az idő, de bizonyos adatok, például használati kvótájának, küszöbértéket információk pont idejét.

Is megtekintheti a vonatkozó kvóták rendelkező erőforrások lap használati kvótájának:

![Használat](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Például a mértékek, használhatja a [REST API -t](https://msdn.microsoft.com/library/azure/dn931963.aspx) vagy a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) a skype_for_businesshoz használati kvótájának programozás útján eléréséhez.

## <a name="next-steps"></a>Következő lépések

* A [riasztási értesítéseket](insights-receive-alert-notifications.md) egy mérőszám metszéspontja küszöbértéket.
* A [Figyelés és diagnosztika engedélyezése](insights-how-to-use-diagnostics.md) részletes magas-gyakoriság mértékek gyűjt a szolgáltatás.
* Az [Automatikus méretezés példányok száma](insights-how-to-scale.md) és a szolgáltatás nem érhető el, és válaszol.
* [Alkalmazás teljesítményének figyelése](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné, ha meg szeretné érteni, hogy pontosan hogyan a kód végrehajtásához a felhőben.
* [A JavaScript-alkalmazások és a weblapok alkalmazás háttérismeretek](../application-insights/app-insights-web-track-usage.md) segítségével ügyfél analytics kapcsolatban, látogasson el az weblapon a böngészőben.
* [Monitor elérhetőségét, és a weblap növeléséhez](../application-insights/app-insights-monitor-web-app-availability.md) alkalmazás Hírcsatornájában láthatja, hogy ha a lapon, a nem működik.
