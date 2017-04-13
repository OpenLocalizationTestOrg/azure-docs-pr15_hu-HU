<properties
    pageTitle="Figyelése és kezelése egy rugalmas adatbázis készlet Azure Portal |} Microsoft Azure"
    description="Ismerje meg az Azure-portálra, és az SQL-adatbázis beépített üzletiintelligencia használata kezelheti, figyelésére és a jobb-adatbázis teljesítményének optimalizálása, és kezelheti a költség méretezhető rugalmas adatbázis erőforráskészlethez tartozik."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Figyelése és kezelése egy rugalmas adatbázis készlet az Azure portálján

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-pool-manage-portal.md)
- [A PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [AZ SQL-T](sql-database-elastic-pool-manage-tsql.md)


Az Azure portal segítségével figyelésére és egy rugalmas adatbázis készlet és a készlet az adatbázisok kezelése. A portálról figyelheti egy rugalmas készlet és az adatbázisok készlethez belül. Egy sor olyan módosításokat végezni a rugalmas készlet is, és elküldése az összes módosítás egy időben. Ezek a változások közé hozzáadása vagy eltávolítása az adatbázisok, rugalmas készlet beállítások módosítása, vagy az adatbázis-beállítások módosítása.

Az alábbi ábra mutatja egy példa rugalmas készlet. A nézet tartalmazza:

*  Erőforrás-kihasználtság a rugalmas készlet mind az adatbázisokat a készletében szereplő figyelemmel kísérésére diagramok.
*  A **beállítás** készlet gombra, és végezze el a módosításokat a rugalmas készletébe.
*  Az **adatbázis létrehozása** gomb, amely létrehoz egy új adatbázist, és hozzáadja őket az aktuális rugalmas készlet.
*  Rugalmas feladatok, amely segít nagyszámú adatbázisok kezelheti a Transact-SQL-parancsfájlok futó adatbázisokra listában.

![Készlet megtekintése][2]

Haladjon végig az ebben a cikkben leírt lépéseket, szüksége lesz egy SQL server Azure-ban legalább egy adatbázist, és egy rugalmas készlet. Ha nem rendelkezik egy rugalmas készlet, olvassa el a [készlet létrehozása](sql-database-elastic-pool-create-portal.md); Ha nem egy adatbázist, olvassa el az [SQL-adatbázis oktatóprogram](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Rugalmas készlet figyelése

Egy adott készlet lásd: az erőforrás-kihasználtság láthatja el. Alapértelmezés szerint a készlet kattintva jelenítse meg az utolsó óraként tárolására és eDTU használatát van beállítva. A diagram beállítható úgy, hogy a különböző mértékek jelenítsen meg különböző idő windows.

1. Jelölje ki a készlet végezhető.
2. Diagram **Rugalmas készlet figyelése** a felirata **erőforrás-kihasználtság**. Kattintson arra a diagramra.

    ![Rugalmas készlet figyelése][3]

    A **metrikus** lap nyílnak meg, a megadott mérési módja miatt részletes nézete megjelenítő fölé a megadott időkeret.   

    ![Metrikus lap][9]

### <a name="to-customize-the-chart-display"></a>Ha testre szeretné szabni a diagrammal

Módosíthatja a diagram és a metrikus lap egyéb mértékek, például a Processzor százalékos, adatok IO százalékos és használt napló IO százalék megjelenítéséhez.

2. A metrikus lap kattintson a **Szerkesztés**gombra.

    ![Kattintson a Szerkesztés gombra.][6]

- A **Diagram szerkesztése** lap jelölje ki egy új idő (óra, ma, a múltbeli vagy korábbi hét), vagy kattintson az **egyéni** jelölje ki bármelyik dátumtartomány utolsó két hétben. Válassza ki a diagram (sáv vagy vonal), majd jelölje be a Lync-erőforrások.

> Megjegyzés: Csak a mértékegység azonos mértékek megjeleníthetők a diagram egy időben. Például ha bejelöli a "eDTU százalék" majd csak fogja tudni válassza az egyéb mértékek százalék mértékegységre lehetőséget.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Kattintson **az OK**gombra.


## <a name="elastic-database-monitoring"></a>Rugalmas adatbázis figyelése

Egyes adatbázisokat is ellenőrizhető az esetleges problémákat tapasztal.

1. **Rugalmas adatbázis felügyeletet igényel**, a mérőszámok öt adatbázisok megjelenítő diagram van. Alapértelmezés szerint a diagram jeleníti meg a felső 5 adatbázisokat a készletben által átlagos eDTU használatát az elmúlt órában. Kattintson arra a diagramra.

    ![Rugalmas készlet figyelése][4]

2. Az **Adatbázis erőforrás-kihasználtság** lap jelenik meg. Ez a részletes adatok megjelenítése az adatbázis használatát a készletben biztosít. A rács alsó részén a lap segítségével, választhat bármely adatbázis(ok) a készletben annak használatát megjelenítése a diagramon (legfeljebb 5 adatbázisok). Is testre szabhatja a **diagram szerkesztése**gombra kattintva a diagramon megjelenített mértékek és az idő ablakban.

    ![Erőforrás-kihasználtság lap adatbázis][8]

### <a name="to-customize-the-view"></a>Ha testre szeretné szabni a nézetet

1. Az **adatbázis erőforrás-kihasználtság** lap kattintson a **diagram szerkesztése**elemre.

    ![Kattintson a Szerkesztés diagram](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. Diagram **szerkesztése** lap jelölje ki egy új idő (óra múltbeli vagy elmúlt 24 óra), vagy kattintson az **egyéni** az elmúlt 2 hét megjelenítéséhez jelölje be egy másik napot.

    ![Kattintson az egyéni](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Kattintson a jelölje ki az adatbázisok összehasonlításakor használni egy másik mérőszám **adatbázisokból összehasonlítása** legördülő listája.

    ![A diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Jelölje be a Lync-adatbázisok

Az **Adatbázis erőforrás-kihasználtság** lap adatbázis listájában megtalálhatja bizonyos adatbázisok megjeleníti a lapokról, a listában, vagy írja be az adatbázis neve. A jelölőnégyzet használatával jelölje ki az adatbázist.

![Lync-adatbázisok keresése][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Értesítés készlet erőforrás hozzáadása

Információforrások, amelyek az e-mail küldése a személyek vagy URL-cím végpontokhoz riasztási karakterláncok, amikor az erőforrás találatok beállított kihasználtsági küszöbértéket szabályokat vehet.

> [AZURE.IMPORTANT]Erőforrás-kihasználtság rugalmas készletek figyelése tartalmaz legalább 20 perc késés. Rugalmas készletek vonatkozó értesítések legfeljebb 30 percig beállítása jelenleg nem támogatott. Ezeket az értesítéseket pontra beállítása rugalmas készletek (paraméter neve "-ablakméret" PowerShell API) legfeljebb 30 percig nem lehet indítani. Győződjön meg arról, hogy ezeket az értesítéseket kell megadni rugalmas készletek használata időtartama (ablakméret) legalább 30 percet vesz igénybe.

**Értesítés hozzáadása bármely erőforrás:**

1. Kattintson a **metrikus** lap megnyitásához kattintson a **Hozzáadás értesítés**az **erőforrás-kihasználtság** diagramra, és töltse ki az (**erőforrás** van automatikusan állíthat be az erőforráskészlet használata) **riasztási szabály hozzáadása** lap.
2. Írja be a **nevét** és **leírását** , amely azonosítja a riasztást, és a címzettek.
3. Válassza ki a listából a felhasználó kívánt **metrikus** .

    A diagram dinamikusan mutatja, hogy könnyebben választhassa küszöbértéket mérőszám erőforrás-kihasználtság.

4. Válasszon egy **feltételt** (nagyobb, mint kisebb, mint, stb) és **küszöbértéknél**.
5. Kattintson az **OK gombra**.



## <a name="move-a-database-into-an-elastic-pool"></a>Adatbázis áthelyezése egy rugalmas készlet

Akkor felvehet és eltávolíthat adatbázisok egy meglévő készletből. Az adatbázisok más készletek lehet. Jó helyen jár csak adhat hozzá adatbázisok, amelyek logikai ugyanarra a kiszolgálóra.

1. A készlet lap, a **rugalmas adatbázisok** csoportban kattintson a **konfigurálása készlet**parancsra.

    ![Kattintson a készlet konfigurálása][1]

2. Kattintson a **Konfigurálás készlet** lap **Készlet hozzáadása**.

    ![Kattintson a Hozzáadás a készlet](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Az **adatbázisok hozzáadása** a lap jelölje ki az adatbázis vagy a készlet hozzáadása adatbázisok. Ezután kattintson a **Kijelölés**gombra.

    ![Jelölje ki az adatbázisok hozzáadása](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    A **Configure készlet** lap most megjeleníti az Ön által megadott adható meg a **függőben lévő**állapota az adatbázist.

    ![Függőben lévő készlet kiegészítések](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. A **beállítás a készlet lap**kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Adatbázis áthelyezése egy rugalmas készlet kívül

1. Válassza a **Configure készlet** lap az adatbázist, vagy ha el szeretné távolítani az adatbázisok.

    ![az adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Kattintson a **Készlet eltávolítása**gombra.

    ![az adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    A **Configure készlet** lap most listája a kijelölt eltávolítjuk meg a **függőben lévő**állapota az adatbázist.

    ![előzetes adatbázis hozzáadása és eltávolítása](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. A **beállítás a készlet lap**kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Egy készlet teljesítmény beállításainak módosítása

Figyelheti az erőforrás-kihasználtság erőforráskészlethez tartozik, ahogy azt tapasztalhatja, hogy szükség van-e az egyes módosítások. A készlet esetleg a teljesítmény vagy tároló korlátok módosítás igényeknek megfelelően. Esetleg módosítani szeretné a készletben az adatbázis beállításait. A készlet beállításai a legjobb teljesítmény és a költség egyenlege megszerezni bármikor módosíthatja. Lásd: [Amikor egy rugalmas adatbázis készlet kell használni?](sql-database-elastic-pool-guidance.md) további információt.

**A eDTUs vagy tárolási korlátokat egy készlet és a eDTUs adatbázisonként módosítása:**

1. Nyissa meg a **készlet konfigurálása** lap.

    **Rugalmas adatbázis készlet beállításai**csoportban csúszkával vagy a készlet beállításainak módosítása.

    ![Rugalmas készlet erőforrás-kihasználtság](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Ha a beállítás módosul, megjelenítésének módosítása a becsült havi költségét jeleníti meg.

    ![Új havi költség és erőforráskészlet frissítése](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Hozzon létre és rugalmas feladatok kezelése

Rugalmas feladatok adhatja meg a Transact-SQL nyelvben parancsprogramokat futtassanak adatbázisok a készlet bármelyik száma. Hozzon létre új feladatokat, vagy a portálon meglévő projektek kezeléséhez.

![Hozzon létre és rugalmas feladatok kezelése][5]


Mielőtt használatba feladatok, rugalmas feladatok-összetevők telepítése, és adja meg a hitelesítő adatait. További információ a [Rugalmas adatbázis feladatok áttekintése](sql-database-elastic-jobs-overview.md)című témakörben találhat.

Lásd: a [Méretezés ki az Azure SQL-adatbázis](sql-database-elastic-scale-introduction.md): rugalmas adatbázis eszközök segítségével méretezési, az adatok áthelyezése, lekérdezés, vagy hozzon létre a tranzakciók.



## <a name="additional-resources"></a>További források

- [Rugalmas készlet SQL-adatbázis](sql-database-elastic-pool.md)
- [A portál és-rugalmas adatbázis-készlet létrehozása](sql-database-elastic-pool-create-csharp.md)
- [A PowerShell-rugalmas adatbázis-készlet létrehozása](sql-database-elastic-pool-create-powershell.md)
- [Egy rugalmas adatbázis-készlet létrehozása a és C#](sql-database-elastic-pool-create-csharp.md)
- [Rugalmas adatbázis készletek árát és a teljesítmény megfontolandó szempontok](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
