<properties
    pageTitle="Első lépések a Azure Monitor |} Microsoft Azure"
    description="Az első lépések Azure Monitor használatával bepillantást az erőforrások működését, és a művelet végrehajtása elhagyja adatok alapján."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Azure Monitor – első lépések

Azure monitorra a platform szolgáltatása, amely egyetlen helyről Azure erőforrások ellenőrzésére. Azure monitorral ábrázolhatja, lekérdezés, irányítása, archiválni, és a mértékek és a források az Azure-ból érkező naplók művelet végrehajtása. Ezeket az adatokat a Monitor portál lap, [Monitor PowerShell-parancsmagok](./insights-powershell-samples.md) [Platformok CLI](insights-cli-samples.md)és [Azure Monitor REST API -khoz](https://msdn.microsoft.com/library/dn931943.aspx)dolgozhat. Ebben a témakörben végigvezetjük Azure Monitor, a portál használata a bemutató főbb elemeinek néhány.

1. A portálon nyissa meg azt a **További szolgáltatások** , és keresse meg a **Monitor** lehetőséget. Kattintson a csillagra beállítás hozzáadása a Kedvencek listába, hogy az mindig könnyen hozzáférhető, a bal oldali navigációs sávján.

    ![A szolgáltatások listában figyelése](./media/monitoring-get-started/monitor-more-services.png)

2. Kattintson a **Monitor** lehetőségre kattintva nyissa meg a **Monitor** lap. Ez a lap összegyűjti az összes a megfigyeléssel és az adatfájl egy összesített nézetbe. Azt először nyitja meg a **tevékenység naplója** szakaszhoz.

    ![Monitor lap navigációs](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] A fent megjelenő **szolgáltatás értesítéseket** és a **csoportok értesítési** beállítások csak akkor jelenik azoknak, akik csatlakoztak az ezek a funkciók a magánjellegű előnézetét.

    Azure Monitor három alapvető kategóriába-adatok figyelése van: A **tevékenység naplója**, a **Mértékek**és a **diagnosztikai naplók**.

3. Kattintson a **tevékenység jelentkezzen** annak érdekében, hogy a tevékenység naplója szakaszban látható.

    ![Tevékenység naplója lap](./media/monitoring-get-started/monitor-act-log-blade.png)

    A [**Tevékenységnapló**](./monitoring-overview-activity-logs.md) erőforrások az előfizetése összes műveleteket ismerteti. A tevékenységnapló használ, eldöntheti, a "mi, ki, és mikor" létrehozott, frissíthet és törölhet az előfizetés erőforrásainak műveleteket. Például a tevékenységnapló kiderül, hogy mikor leállt webalkalmazást, és ki leállította. Tevékenység naplóbeli események is a platform tárolt elérhető 90 napig lekérdezéséhez.
   
    Létrehozása és mentse a gyakori szűrők lekérdezések, majd a legfontosabb lekérdezések portál irányítópultra rögzít, így mindig tudni fogja Ha adott feltételeknek megfelelő események történtek.

4. Az adott erőforrás csoport nézet szűrése a múlt héten fölé, majd kattintson a **Mentés** gombra.

    ![Tevékenység naplója lekérdezés mentése](./media/monitoring-get-started/monitor-act-log-save.png)

5. Kattintson a **PIN-kód** gombra.

    ![Kattintson a PIN-kódjának tevékenységnapló](./media/monitoring-get-started/monitor-act-log-pin.png)

    Ez a forgatókönyv a nézeteket a legtöbb a egy irányítópult kiemelt is. Ezzel az elemzéssel hozhat létre egy egyetlen információforrás műveleti adatok a szolgáltatások. 

6. Térjen vissza az irányítópult. Most már láthatja, hogy a lekérdezés (és a találatok) megjelenik az irányítópult. Ez akkor hasznos, ha azt szeretné, hogy gyorsan történtek nemrégiben előfizetését, pl. hangsúlyos műveleteket. Új szerepkör van beállítva, vagy egy virtuális törölték.

    ![A tevékenységnapló irányítópult kiemelt](./media/monitoring-get-started/monitor-act-log-db.png)

7. Térjen vissza a **Monitor** csempét, és kattintson a **Mértékek** szakaszra. Először jelölje ki az erőforrás szűrés, és válassza a beállítások legördülő használata a lap tetején.

    ![Erőforrás-mértékek szűrése](./media/monitoring-get-started/monitor-met-filter.png)

    Azure összes erőforrás [**Mértékek**](./monitoring-overview-metrics.md)elhelyezése. Ebben a nézetben az egyetlen minden mérőszám üveg egyetlen ablaktáblában, akkor egyszerűen megértéséhez, hogy hogyan az erőforrások hajt végre.

8. Ha be van jelölve egy erőforrást, az összes rendelkezésre álló mértékek a lap bal oldalán lévő jelennek meg. Több mértékek egyszerre diagram metrikus kiválasztásával, és módosíthatja a diagram típusának és időtartományt. Megtekintheti az összes metrikus értesítés beállítása erőforrás is.

    ![Metrikus lap](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Néhány mértékek csak érhetők el, mivel az [Alkalmazás az összefüggéseket](../application-insights/app-insights-overview.md) és/vagy Windows vagy Linux Azure diagnosztika az erőforrás.

9. Miután Boldog a diagramot, a **PIN-kód** gomb segítségével rögzít azt az irányítópult.

10. Térjen vissza a **Monitor** lap, és válassza a **diagnosztikai naplók**.

    ![A diagnosztikai naplók lap](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**A diagnosztikai naplók**](monitoring-overview-of-diagnostic-logs.md) kibocsátott naplók *által* egy erőforrás az adott erőforráshoz működéséről elődcelláinak. Hálózati biztonsági csoport szabály számláló és logika alkalmazás munkafolyamat naplók mindegyike a diagnosztikai naplók az mindkét típusát. Ezek a naplók tárterület-fiókjában tárolt, esemény-hubon keresztül csatlakozott folyamatosan lejátszott és [Napló Analytics](../log-analytics/log-analytics-overview.md)küldött. Log Analytics a Microsoft műveleti üzletiintelligencia-termék speciális keresési és riasztási.
   
    A portál megtekintése, és az összes erőforrás szűrésre az előfizetés azonosítása, ha engedélyezve van a diagnosztikai naplók is.

11. Kattintson a diagnosztikai naplók lap az erőforrás. Ha a tárterület-fiók a diagnosztikai naplók küldése tárolja, láthatja, ingyenesen letöltheti közvetlenül óránkénti naplók listáját.

    ![A diagnosztikai naplók egy erőforrás](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    A **Diagnosztikai beállítások**, amelyek lehetővé teszi, megadhatja és módosíthatja a beállításokat az archiválás tárterület-fiókjába, az esemény hubok streaming, vagy elküldheti a napló Analytics munkaterületre is kattinthat.

    ![A diagnosztikai naplók engedélyezése](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Diagnosztikai naplók napló Analytics beállítása, ha, majd kereshet őket a portálon **napló keresési** szakaszában.

12. Nyissa meg a **riasztások** a Monitor lap című szakaszát.

    ![a nyilvános riasztások lap](./media/monitoring-get-started/monitor-alerts-nopp.png)

    Itt is minden [**értesítések**](./monitoring-overview-alerts.md) kezelése az Azure erőforrások. Ide tartoznak a mértékek, a tevékenység naplóbeli események (a személyes előzetes verzió), a alkalmazás háttérismeretek webes vizsgálatok (helyek) és a alkalmazás háttérismeretek megelőző diagnosztika értesítéseket. Értesítések válthat küldendő e-mailben vagy HTTP POST webhook URL-címre.
   
13. Kattintson a **metrikus értesítés hozzáadása** értesítés létrehozása parancsra.

    ![metrikus értesítés hozzáadása](./media/monitoring-get-started/monitor-alerts-add.png)

    Kattintson az irányítópult, ahol egyszerűen megnézheti az állapotát bármikor jelzést is rögzíthet.

14. A Monitor szakasz az [Alkalmazás háttérismeretek](../application-insights/app-insights-overview.md) alkalmazások és a [Napló Analytics](../log-analytics/log-analytics-overview.md) management megoldások mutató hivatkozásokat is tartalmaz. Ezek a Microsoft termékeihez mély funkciók integrálása az Azure Monitor van.

15. Ha nem az alkalmazás az összefüggéseket vagy napló Analytics, valószínűleg, hogy rendelkezik-e a jelenlegi nyomon, naplózás és figyelmeztető termékek partnerség Azure Monitor. Lásd: a [partnerek, oldal](./monitoring-partners.md) teljes listáját és integráció útmutatót.

Ezekkel a lépésekkel és rögzítése az irányítópultra minden releváns mozaik, az alkalmazások és az ehhez hasonló infrastruktúra átfogó nézeteket hozhat létre:

![Azure Monitor irányítópult](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Következő lépések
- Olvassa el az [Azure Monitor áttekintése](./monitoring-overview.md)
