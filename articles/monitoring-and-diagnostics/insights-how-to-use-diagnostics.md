<properties
    pageTitle="Engedélyezés figyelemmel kísérésére és a diagnosztikai Microsoft Azure-ban |} Microsoft Azure "
    description="Megtudhatja, hogy miként állíthatja be az Azure-ban az erőforrások diagnosztika."
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

# <a name="enable-monitoring-and-diagnostics"></a>Figyelés és diagnosztika engedélyezése

Az [Azure-portálon](https://portal.azure.com)beállíthatja a sokoldalú, gyakori, figyelés és a diagnosztikai adatok az erőforrásokról. A [REST API -t](https://msdn.microsoft.com/library/azure/dn931932.aspx) vagy a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) konfigurálása a diagnosztika programozás útján is használhatja.

Egy tetszés szerinti tárterület-fiókba mentett diagnosztika, figyelemmel kísérése és metrikus adatok Azure-ban. Ez lehetővé teszi, hogy bármilyen kívánt szerszámok elolvasni az adatokat, a tárhely Intézőből külső szerszámok készült Power BI használata.

## <a name="when-you-create-a-resource"></a>Amikor hoz létre egy erőforrás

A legtöbb szolgáltatás teszik lehetővé a diagnosztika első létrehozásakor őket az [Azure-portálon](https://portal.azure.com).

1. Nyissa meg az **Új** , és válassza az erőforrás érdeklik.

2. Válassza a **nem kötelező beállítást**.
    ![Diagnosztikai lap](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Jelölje ki a **Diagnosztika**, és kattintson **a**gombra. Szüksége lesz, válassza ki a használni kívánt diagnosztika menti tároló fiókot. Akkor fogja kell fizetnie normál adatsebesség-tárhely és a tranzakciók diagnosztika küldésekor tárterület-fiókjába.

4. Kattintson az **OK gombra** , és hozzon létre az erőforrás.

## <a name="change-settings-for-an-existing-resource"></a>Meglévő erőforrás-beállítások módosítása

Ha már létrehozott egy erőforrást, és meg szeretné változtatni a diagnosztikai beállítások (például adatgyűjtés szintjének módosítása), megteheti, hogy az Azure-portálon jobbra.

1. Nyissa meg az erőforrásra, és kattintson a **Beállítások** parancsra.

2. Jelölje ki a **Diagnosztika**.

3. A **diagnosztikai** lap minden lehetséges diagnosztika és az adott erőforrás felügyeleti webhelycsoport adatokat tartalmaz. Az egyes erőforrások **adatmegőrzési** szabály az adatok jelenjenek meg a tárterület-fiókból is megadhatja.
    ![Tárterület diagnosztika](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Miután a választott a beállításokat, kattintson a **Mentés** parancsot. Eltarthat egy kicsit közben figyelemmel kísérésére jelenik meg, ha engedélyezi, először az adatokat.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Adatok gyűjtése az virtuális gépeken futó kategóriái
A virtuális gépeken futó mértékek és a naplók lesz egy perces időközönként, így mindig a legfrissebb információkat a számítógép kapcsolatban.

- **Egyszerű mértékek** : állapot mértékek kapcsolatban a virtuális gép, például a processzor- és
- **A hálózati és webes mértékek** : a hálózati kapcsolatok és webszolgáltatások mérőszámok
- **.NET-mértékek** : a virtuális gépen futó a .NET és ASP.NET alkalmazásokkal kapcsolatos mérőszámok
- **SQL-mértékek** : Ha futtatja a Microsoft SQL-szolgáltatás, a teljesítménymutatók
- **A Windows eseménynaplók alkalmazás** : az alkalmazás csatorna küldött Windows-események
- **A Windows rendszer eseménynaplók** : Windows eseményeket, a rendszer csatorna küldött. Ez a [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409)az összes eseményének is tartalmaz.
- **A Windows biztonsági eseménynaplók** : a biztonsági csatorna küldött Windows-események
- **Diagnosztikai naplók-infrastruktúrát** : a diagnosztikai webhelycsoport infrastruktúra kapcsolatos naplózás
- **IIS naplók** : vonatkozó az IIS-kiszolgáló

Ne feledje, hogy jelenleg nem támogatottak, bizonyos Linux elosztása, és a vendégként való bekapcsolódáshoz Agent telepítenie kell a virtuális gépen.

## <a name="next-steps"></a>Következő lépések

* Mértékek vagy [fogadás értesítések](insights-receive-alert-notifications.md) működési események fordulhat elő, amikor cross küszöbértéket.
* [Monitor szolgáltatás mértékek](insights-how-to-customize-monitoring.md) és a szolgáltatás nem érhető el, és válaszol.
* A szolgáltatás skála igény szerinti alapján győződjön meg arról, hogy a [automatikusan átméretezheti példányok száma](insights-how-to-scale.md) .
* [Alkalmazás teljesítményének figyelése](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné, ha meg szeretné érteni, hogy pontosan hogyan a kód végrehajtásához a felhőben.
* [Események megtekintése és naplók](insights-debugging-with-events.md) mindent megtudhatja, hogy történt a szolgáltatásban.
* [Szolgáltatásállapot nyomon követése](insights-service-health.md) megtudhatja, hogy Azure észlelt a teljesítmény csökkenés vagy szolgáltatás nézhet elébe.
