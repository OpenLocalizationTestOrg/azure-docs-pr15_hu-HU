<properties
    pageTitle="A tároló fiók figyelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként figyelheti a tárhely fiók Azure-ban az Azure portál használatával."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>A tároló fiókot az Azure-portálon figyelése

## <a name="overview"></a>– Áttekintés

Figyelheti a tárhely fiókját az [Azure-portálon](https://portal.azure.com). Tárhely fiókját a portálon keresztül figyelemmel kísérésére konfigurálásakor a Azure tároló nyomon követheti a mértékek, a fiók, majd újra bejelentkezik a kérelem adatainak [Tárolására Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) használja.

> [AZURE.NOTE]Többletköltségek felügyeleti adatok az [Azure portál](https://portal.azure.com)vizsgálja kapcsolódnak. További tudnivalókért lásd: <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">Analytics tárhely és a számlázásra</a>. <br />

> Azure fájltároló jelenleg támogatja Analytics tárolási mértékek azonban egyelőre nem támogatják a naplózás. Lehetősége van engedélyezni a mértékek az Azure fájltároló az [Azure-portálon](https://portal.azure.com)keresztül.

> A zóna felesleges tároló (ZRS) replikációs típusú tároló számla a mértékek vagy naplózás képesség engedélyezve van a problémának jelenleg nincs. 

> Tárterület Analytics- és egyéb eszközök segítségével azonosítása, diagnosztizálása és Azure tároló kapcsolatos hibák elhárítása a részletes útmutató olvassa el a [Monitor, diagnosztizálása, és a Microsoft Azure tároló – problémamegoldás](storage-monitoring-diagnosing-troubleshooting.md)című témakört.


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Útmutató: állítsa be a tárterület-fiók ellenőrzése

1. Az [Azure-portálon](https://portal.azure.com)kattintson a **tároló**elemre, és kattintson a tárolás fiók nevére kattintva nyissa meg az irányítópult.

2. Kattintson a **Konfigurálás**gombra, majd görgessen le a blob, táblázatok és várólista szolgáltatások **figyelése** beállításait.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. **Figyelés**állítsa be az olyan szintű figyelő és az adatmegőrzési az egyes szolgáltatásokhoz:

    -  A felügyeleti szint megadásához válassza ki az alábbiak egyikét:

      **Minimális** – például a bejövő adatok/kilépési, elérhetőségét, időtartam és sikeres százalékértékeket blob, tábla, illetve várólista szolgáltatások vannak összesíti tárolja mértékek.

      **Részletes** - mellett a minimális mértékek gyűjti össze ugyanazok a mértékek minden tároló műveletre az Azure tároló szolgáltatás API-val. Részletes mértékek engedélyezése alkalmazás műveletek során előforduló problémák közelebb elemzését.

      **Ki** - kikapcsolja figyelése. Meglévő adatok figyelése állandó – az adatmegőrzési időszak végén.

- Az adatmegőrzési házirendet, beállításához **adatmegőrzési (napokban)**, írja be az adatokat, amely megőrzi az 1-től 365 napra napok számát. Ha nem szeretné, hogy az adatmegőrzési házirend beállítása, írja be a nulla. Nincs adatmegőrzési házirend, érdemes felfelé ellenőrző adatok törlését. Azt javasoljuk, hogy mennyi ideig szeretné megőrizni a tárhely analytics adatainak a fiókjához, hogy a rendszer áttérhet törölheti régi, még nem használt analytics-adatok alapján adatmegőrzési házirend beállítása.

4. Amikor a megfigyeléssel konfiguráció elkészült, kattintson a **Mentés**gombra.

El kell indítania az megjelenik az irányítópulton és a **Monitor** lap után egy órába-adatok figyelése.

Konfigurálja az figyelése tárhelyet fiókot, amíg nem felügyeleti van gyűjtött adatokat, és az irányítópult és a **Monitor** lapon a mértékek diagramok üres.

A felügyeleti szintek és az adatmegőrzési házirendek beállítása után megadhatja, amely a rendelkezésre álló mérési módja miatt az [Azure-portálon](https://portal.azure.com)Lync és a mértékek diagramok ábrázolandó mely mértékek. Minden felügyeleti szintjén az alapértelmezés szerinti mértékek jelenik meg. **Mértékek felvétele** segítségével hozzáadása vagy eltávolítása a mértékek a mértékek listából.

Mértékek a tárhely fiók $MetricsTransactionsBlob, $MetricsTransactionsTable, $MetricsTransactionsQueue és $MetricsCapacityBlob nevű négy táblákban tárolja. További tudnivalókért olvassa el a [Tárolási Analytics mértékek kapcsolatos](http://msdn.microsoft.com/library/azure/hh343258.aspx)című témakört.


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Útmutató: az irányítópulton figyelemmel kísérésére testreszabása

Az irányítópulton lehetősége van legfeljebb hat mértékek szeretne ábrázolni a mértékek kilenc elérhető mértékek diagramot. Az egyes szolgáltatásokhoz (blob tábla és várólista) az elérhetőség, sikeres százalékos és kérések teljes mértékek érhetők el. A mérési módja miatt érhető el az irányítópulton azonosak figyelemmel kísérésére minimális vagy részletes.

1. Az [Azure-portálon](https://portal.azure.com)kattintson a **tároló**elemre, és kattintson a nevére kattintva nyissa meg az irányítópult tárterület-fiók.

2. Ha módosítani szeretné a mértékek, a diagramon ábrázolt, kövesse az alábbi műveletek egyikét:

    - Egy új mérőszám hozzáadása a diagramhoz, kattintson a táblázat alatt a diagram metrikus fejléce színes jelölőnégyzetét.

    - Kattintson a diagramon ábrázolt metrikával elrejtéséhez törölje a jelet a metrikus fejléc színes jelölőnégyzetét.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. A diagramon alapértelmezés szerint jeleníti meg a trendek, minden egyes metrikus ( **relatív** választógomb a diagram tetején) csak az aktuális érték megjelenítése. **Abszolút**választva megtekintheti az abszolút érték Y tengely megjelenítéséhez.

4. A időtartomány módosítása a mértékek diagram jeleníti meg, jelölje be 6 órával 24 óra vagy napján tetején látható a diagramon.


## <a name="how-to-customize-the-monitor-page"></a>Útmutató: a Monitor lap testreszabása

**Monitor** lapján megtekintheti a skype_for_businesshoz mértékek a tárterület-fiókjához.

- A tárterület-fiók van konfigurálva minimális ellenőrzése, ha például a bejövő adatok/kilépési, elérhetőségét, időtartam és sikeres százalékértékek mértékek a blob, a táblázat és a várólista szolgáltatások vannak összesíti.

- Ha a tárterület-fiók van konfigurálva részletes figyelése, egyes tárolási műveletek kívül a szolgáltatás szintű összegzések finomabb felbontásban a mértékek érhetők el.

Az alábbi eljárások segítségével válassza ki, mely tárolási mértékek tekintheti meg a mértékek diagramok, illetve a **Monitor** lapon megjelenő. A webhelycsoport, összesítési és nyomon követési adatok tárolására fiók tárolására ezeket a beállításokat nem vonatkoznak.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Útmutató: mértékek elhelyezése a mértékek táblázatban


1. Az [Azure-portálon](https://portal.azure.com)kattintson a **tároló**elemre, és kattintson a nevére kattintva nyissa meg az irányítópult tárterület-fiók.

2. Kattintson a **képernyő**.

    Megnyílik a **Monitor** lap. A mértékek táblázat alapértelmezés szerint a mértékek figyelemmel kísérésére elérhető egy részét jeleníti meg. Az ábrán az alapértelmezett képernyő tárterület-fiók a részletes figyelése három szolgáltatások be van állítva. **Mértékek felvétele** segítségével válassza ki a mértékek, az összes rendelkezésre álló mértékek figyelni kívánt.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Ha bejelöli a mértékek, fontolja meg költségeket. Vannak tranzakció és kilépési költségeket társított frissítése felügyeleti jeleníti meg. További tudnivalókért lásd: [Analytics tárhely és a számlázásra](http://msdn.microsoft.com/library/azure/hh360997.aspx).

3. Kattintson a **Mértékek hozzáadása**gombra.

    A minimális figyelése a használható összesítő mértékek vannak a lista tetején. Ha a jelölőnégyzet be van jelölve, a mérőszám megjelenik a mértékek listában.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Mutasson egy görgetősáv, amely további mértékek görgessen nézetbe húzással is megjelenítheti a párbeszédpanel jobb oldalán.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Kattintson a lefelé mutató nyílra kattintva bontsa ki a lista műveletek a mérőszám megfelelően változik, amelyet fel szeretne venni egy mérőszám. Jelölje ki az egyes műveletek, hogy meg szeretné tekinteni a mértékek táblázatban az [Azure-portálon](https://portal.azure.com).

    Az alábbi ábrán a engedélyezési hiba százalékos mérőszám ki van bontva.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Miután kiválasztotta az összes szolgáltatás mértékek, kattintson az OK (jelölő) a ellenőrzési beállítások frissítése. A kijelölt mérési módja miatt bekerülnek a mértékek táblázatra.

7. Ha törölni szeretne egy mérőszám a táblázat, kattintson a jelölje ki a mérőszám, és válassza a **Törlés metrikus**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Útmutató: a Monitor lapon a mértékek diagram testreszabása

1. A tárterület-fiókot, a mérőszámok táblázatban az a **Monitor** lapján jelölje ki a mértékek diagram ábrázolandó legfeljebb 6 mértékek. Egy mérőszám kijelöléséhez kattintson a bal oldalon a jelölőnégyzetet. Ha el szeretne távolítani egy mérőszám arra a diagramra, törölje a jelet a jelölőnégyzetből.

2. Váltás a relatív (az utolsó érték csak akkor jelenik meg) és abszolút értékek (Y tengely jelenik meg) között arra a diagramra, jelölje be a **relatív** és **abszolút** tetején látható a diagramon.

3.  Ha módosítani szeretné az idő a mértékek diagram jeleníti meg, jelölje be a **6 órával**, **24 óra**vagy **7 nap** a diagram tetején tartományban.



## <a name="how-to-configure-logging"></a>Útmutató: a naplózás beállítása

Az egyes a rendelkezésre álló tárhely fiókjával (blob tábla és várólista) tároló szolgáltatást mentheti a diagnosztikai naplók olvasási kérelmeket, valamint kérések írása és kérések törlése, és az adatmegőrzési beállíthatja az egyes szolgáltatások.

1. Az [Azure-portálon](https://portal.azure.com)kattintson a **tároló**elemre, és kattintson a nevére kattintva nyissa meg az irányítópult tárterület-fiók.

2. Kattintson a **Konfigurálás**gombra, és a billentyűzeten a lefelé mutató nyíl használatával görgessen le a **naplózás**.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Az egyes szolgáltatásokhoz (blob tábla és várólista) állítsa be a következőket:

    - Bejelentkezési kérelem típusú: olvasható kérések, kérések írása és kérések törlése.

    - A naplózott adatok megőrzéséhez napok számát. Írja be a nulla, ha nem szeretné, hogy az adatmegőrzési házirend beállítása. Adatmegőrzési szabály nem állít be, érdemes felfelé a naplókat törlését.

4. Kattintson a **Mentés**gombra.

A diagnosztikai naplók menti a program egy blob-tárolóban nevű $logs a tárterület-fiókjában. A $logs tároló elérésével kapcsolatos további tudnivalókért lásd [Kapcsolatban az tároló Analytics naplózás](http://msdn.microsoft.com/library/azure/hh343262.aspx).
