<properties
    pageTitle="Események megtekintése és naplók"
    description="Megtudhatja, hogy miként talál, amelyek az Azure-előfizetése eseményeket."
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
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Események megtekintése és naplók

Azure erőforrások végzett összes művelet teljesen naplózza a Azure erőforrás-kezelő, létrehozása és törlése a megadásának és vonhat vissza az access. Megkeresheti ezeket az Azure-portálon a naplókat, és használhatja a [REST API -t](https://msdn.microsoft.com/library/azure/dn931927.aspx) vagy a [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) a skype_for_businesshoz események programozás útján eléréséhez.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Keresse meg az Azure-előfizetése érintő események

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **Tallózás gombra** , és jelölje be a **naplókat**.  
    ![Tallózással keresse meg a központi](./media/insights-debugging-with-events/Insights_Browse.png)
3. Ekkor megnyílik egy, a megjelenítő van hatással a előfizetések közül az elmúlt 7 napig eseményeket lap be. Felső részén található érték áttekintheti, hogy milyen adatokat szintenként és alatt, amely a naplók teljes listáját:  ![összes eseményének](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] Megtekintheti egy adott előfizetés 500 legutóbbi események csak az Azure-portálon.

4. Bármely naplóbejegyzés megtekintéséhez az események, hogy be, hogy kattint. Ha például amikor rendszerbe valamit, amit egy erőforrás csoportot, számos különböző erőforrásokat előfordulhat, hogy kell létrehozott vagy módosított. Az egyes láthatja:
    * A **szint** az esemény – például tudott valamit, amit csak (**tájékoztatások**) nyomon követéséhez, vagy ha valamit, amit nem megfelelő kell tudni a (**hiba**) leállt.
    * Az **állapot** - a végleges állapot általában lesz **sikerült** vagy **nem sikerült**, de **elfogadott** -hosszan futó műveleteket is lehet.
    * *Ha* az esemény történt.
    * A művelet, *ki* kell elvégezni, ha bárki. A felhasználók nem az összes műveleteket, néhány végzi kódmentes szolgáltatások, hogy szeretné nem egy **hívó**.
    * A **Korrelációs azonosító** az esemény – Ez az adott műveletek tartozó egyedi azonosítója.

5. Innen láthatja el a Részletek lap részletei határozzák meg az esemény megjelenítéséhez.

    ![Erőforrás-csoportok](./media/insights-debugging-with-events/Insights_EventDetails.png)

    **Nem sikerült** eseményeket ezen az oldalon általában tartalmaz egy **Részállapot** és hibakeresés céljából hasznos részleteket tartalmazó **Tulajdonságok** szakasz.

## <a name="filter-to-specific-logs"></a>Szűrés adott naplók

Egy adott személyhez vagy egy bizonyos típusú vonatkozó események megjelenítéséhez kattintson a **Szűrés** parancs a naplózási naplók lap is szűrheti. A szűrő lap módosíthatja az **idő időtartamát** , a naplózás a naplók lap is használhatja.

Miután ez a parancs gombra kattint, egy új lap nyílik meg:

![Szűrő](./media/insights-debugging-with-events/Insights_EventFilter.png)

Négyféle szűrők:

1. Előfizetés keretében
2. **Erőforráscsoport**
3. Egy **erőforrás típusa**
4. Egy adott **erőforrás** - ezt meg kell múltbeli a teljes az erőforrás érdeklik, *Az erőforrás-azonosító*

Ezeken kívül is szűrheti események ki az eseményt előidéző, illetve, az esemény szintjét.

Miután végzett kiválaszthatja, hogy mit szeretne látni, kattintson a **frissítés** gombra a lap alján.

## <a name="monitor-events-impacting-specific-resources"></a>Adott erőforrások érintő monitor események

1. Kattintson a **Tallózás** az erőforrás érdeklik. Megtekintheti az összes a naplókat egy teljes **erőforráscsoport**.
2. Kattintson az erőforrás lap görgessen lefelé, amíg meg nem találja a **események** csempére.  
    ![Események csempével](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Kattintson a csak a kijelölt erőforrás szűrve események megtekintéséhez, hogy csempére. A **Szűrés** parancs használatával a időtartomány módosítása vagy adott szűrők alkalmazásával.

## <a name="next-steps"></a>Következő lépések

* A [riasztási értesítéseket](insights-receive-alert-notifications.md) esemény történik.
* [Monitor szolgáltatás mértékek](insights-how-to-customize-monitoring.md) és a szolgáltatás nem érhető el, és válaszol.
* [Szolgáltatásállapot nyomon követése](insights-service-health.md) megtudhatja, hogy Azure észlelt a teljesítmény csökkenés vagy szolgáltatás nézhet elébe.  
