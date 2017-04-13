<properties
    pageTitle="Azure Monitor tevékenység naplók használata Azure szolgáltatásállapot nyomon követése |} Microsoft Azure"
    description="Megtudhatja, hogy Azure észlelt a teljesítmény csökkenés vagy szolgáltatás nézhet elébe. "
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Azure Monitor tevékenység naplók használata Azure szolgáltatásállapot nyomon követése

Azure publicizes minden alkalommal, amikor egy szolgáltatás megszakítása és a teljesítmény csökkenés van. Megkeresheti ezeket az Azure-portálon az eseményeket, és használhatja a [REST API -t](https://msdn.microsoft.com/library/azure/dn931927.aspx) vagy a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) a skype_for_businesshoz események programozás útján eléréséhez.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Keresse meg a szolgáltatás állapota naplók az előfizetéshez

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).

2. A **Kezdőlap** látnia kell a **szolgáltatás állapota**nevű csempére. Kattintson rá.

    ![Kezdőlap](./media/insights-service-health/Insights_Home.png)

3. Látható a lista összes a régiók Azure-ban. Kattintson a bármely régió kattintva jelenítse meg a szolgáltatási incidenssel, amely van hatással a előfizetések közül az elmúlt 24 óra a megjelenítő tevékenységnapló lekérdezés.

    ![Tevékenység naplója előfizetési szolgáltatás állapota](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. A táblázat esemény gombra kattintva megtekintheti a egyes esemény részleteit.

5. Módosítsa az **időszak** egy hosszabb időkeret megjelenítéséhez.

## <a name="next-steps"></a>Következő lépések

* [Monitor elérhetőségét, és a weblap növeléséhez](../application-insights/app-insights-monitor-web-app-availability.md) alkalmazás Hírcsatornájában láthatja, hogy ha a lapon, a nem működik.
