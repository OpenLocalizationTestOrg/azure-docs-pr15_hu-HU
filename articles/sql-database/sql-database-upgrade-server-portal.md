<properties
    pageTitle="Az Azure portálon Azure SQL-adatbázis V12 frissítés |} Microsoft Azure"
    description="Megtudhatja, hogyan frissítése a Microsoft Azure SQL-adatbázis V12 például üzleti és a webes adatbázis frissítése, az adatbázis áttelepítése közvetlenül egy rugalmas adatbázis készlet az Azure portálon V11 kiszolgáló frissítéséről."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Az Azure portálon Azure SQL-adatbázis V12 frissítése


> [AZURE.SELECTOR]
- [Azure portál](sql-database-upgrade-server-portal.md)
- [A PowerShell](sql-database-upgrade-server-powershell.md)


SQL-adatbázis V12 legújabb verziója, a meglévő kiszolgálók frissítése SQL-adatbázis V12 ajánlott.
SQL-adatbázis V12 számos [előnye a az előző verzióhoz képest](sql-database-v12-whats-new.md) , többek között foglalja magában:

- Az SQL Server megnövelt kompatibilitásának.
- Továbbfejlesztett prémium teljesítmény és új teljesítményszint.
- [Rugalmas adatbázis készletek](sql-database-elastic-pool.md).

Ebben a cikkben utasításokat-re történő frissítésének meglévő SQL-adatbázis V11 kiszolgálók és adatbázisok SQL-adatbázis V12.

V12 való frissítés során fog frissítene bármely webes és az üzleti adatbázis egy új szolgáltatási réteg, üzleti és a webes adatbázis-re történő frissítésének Útbaigazítás számításba veszi.

Ezenkívül egy [készlet rugalmas adatbázis](sql-database-elastic-pool.md) áttelepítése lehet költség hatékonyabb, mint (árak rétegek) egyes teljesítményszint egyetlen adatbázisok frissítése. Készletek is egyszerűbbé teszik a adatbázis kezelése, mert csak akkor van szüksége a készlet, hanem külön-külön az egyes adatbázisokat a teljesítmény szintjeinek kezelése teljesítmény beállításainak kezelése. Ha több kiszolgálókon adatbázisok fontolja meg, áthelyezi őket ugyanarra a kiszolgálóra történő, és üzembe őket egy készlet nyújtotta előnyök kihasználása. Könnyen [adatbázisok V11 kiszolgálókról közvetlenül a PowerShell használatá rugalmas adatbázis-készletek automatikus áttelepítése](sql-database-upgrade-server-powershell.md). A portál V11 adatbázisok telepítheti át a készlet is használhatja, de a portálon már rendelkeznie kell a V12 kiszolgáló-készlet létrehozása. Útmutatást a jelen cikk az-készlet létrehozása a kiszolgáló-re frissítés után [, amely egy készlet előnyeivel adatbázisok](sql-database-elastic-pool-guidance.md)esetén találhatók.

Ne feledje, hogy a az adatbázisok online marad, és továbbra is működnek a frissítési művelet során. Az ideiglenes kapcsolatot az adatbázis leválasztása új teljesítményszint tényleges áttűnés időpontjában akkor fordulhat elő, amely általában körülbelül 90 másodperc nagyon kis ideig, de szerint az 5 perc lehet. Az alkalmazás rendelkezik [kezelésére szolgáló kapcsolatot végződnek tranziens hibafa](sql-database-connectivity-issues.md) majd esetén elegendő és vírusvédelmet kapcsolatok megszakadnak a frissítés végén.

SQL-adatbázis V12 való frissítés nem vonható vissza. Frissítés után a kiszolgáló nem állítható vissza V11.

V12 történő frissítés után [szolgáltatás réteg javaslatok](sql-database-service-tier-advisor.md) és [rugalmas készlet teljesítménnyel kapcsolatos szempontok](sql-database-elastic-pool-guidance.md) nem azonnal lesz elérhető mindaddig, amíg a szolgáltatás rendelkezik-e az új kiszolgálón a munkaterhelésekből kiértékelése időt. V11 kiszolgáló ajánlási előzmények nem vonatkozik V12 kiszolgálók, nem őrzi meg.

## <a name="prepare-to-upgrade"></a>Felkészülés a frissítése

- **Az összes webhelyen és üzleti adatbázisok frissítése**: tudnivalók az [összes webes és üzleti adatbázisok frissítése](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) rész alatt, illetve nem látom [Monitor és kezelése egy rugalmas adatbázis készlet (PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Áttekintése és felfüggeszti a replikáció Geo**: Ha be van állítva az Azure SQL-adatbázis Geo-replikáció kell a dokumentum a szoftver jelenlegi konfigurációjának megfelelően és [leállítása Geo-replikáció](sql-database-geo-replication-portal.md#remove-secondary-database). A frissítés befejezése után újra az adatbázist Geo-replikáció.
- **Nyissa meg az alábbi portokat az Azure virtuális ügyfelek esetén**: Ha az ügyfélprogram SQL-adatbázis V12 csatlakozik, az ügyfél-Azure virtuális gépen (virtuális) futása közben, meg kell nyitnia 11000-11999 és 14000-14999 a virtuális a port tartományok. Részletekért olvassa el [az SQL-adatbázis V12 portokat](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>A frissítés megkezdése

1. A kiszolgáló [Azure portál](https://portal.azure.com/) tallózással szeretné frissíteni a **Tallózás gombra**kattintva > **SQL-kiszolgálók**gombra, majd a 2.0-s verzió kiszolgáló frissíti.
2. Jelölje ki a **Legújabb SQL-adatbázis frissítése**, majd válassza **a kiszolgáló frissítése**.

      ![kiszolgáló frissítése][1]

3. A kiszolgáló frissítése a legújabb SQL-adatbázis Update pedig állandó vissza nem vonható. Erősítse meg a frissítés, írja be a kiszolgáló nevét, és kattintson az **OK gombra**.

## <a name="upgrade-all-web-and-business-databases"></a>Az összes webhelyen és üzleti adatbázisok frissítése

Ha a kiszolgáló bármely webes vagy vállalati adatbázisok frissítenie kell azokat. SQL-adatbázis V12 való frissítés során egy új szolgáltatási réteg webes és üzleti adatbázisokra frissül.    

Az SQL-adatbázis-szolgáltatás, amely segít a frissítésével kapcsolatban, javasolja, egy megfelelő szolgáltatás réteg és a teljesítmény szintet (árak réteg)-adatbázisonként. A szolgáltatás a legjobb réteg futtatásához a meglévő adatbázist terhelést a korábbi használatát az adatbázis elemzésével javasolja.

3. Válassza a **a kiszolgáló frissítése** a lap minden adatbázist, tekintse át, és a válassza ki a javasolt árak az első csoportba tartozó frissítésének. Keresse meg a rendelkezésre álló árak rétegek mindig, és jelölje ki azt, amely a leginkább illik a környezetben.


     ![adatbázisok][2]


7. A javasolt réteg parancsra kattintást követően választhat a a **Válassza ki a árak réteg** lap, ahol jelölje be az egy réteg, és kattintson a módosítása, hogy a réteg **kiválasztása** gombra. Jelölje ki egy új réteg-webhelyen vagy a vállalati adatbázisonként.

    Fontos tudni, hogy az SQL-adatbázis nem zárolt be bármilyen adott szolgáltatás réteg vagy a teljesítmény szinten, így az adatbázis módosítása követelményeinek, akkor könnyen módosíthatja a rendelkezésre álló szolgáltatási rétegek és teljesítményszint között. Erre valójában Basic, normál és SQL-adatbázisait prémium számláját az órát, és méretezése adatbázisonként felfelé vagy lefelé a 24 órás időszakon belül 4 alkalommal lehetősége van.

    ![javaslatok][6]


Minden adatbázisban a kiszolgálón jogosultak után készen áll a frissítés megkezdése

## <a name="confirm-the-upgrade"></a>Erősítse meg a frissítés

3. Ha a kiszolgálón az adatbázisok nem frissíthető szeretne **Írja be a kiszolgáló neve** ellenőrizze, hogy szeretne-e a frissítés végrehajtása, és kattintson **az OK**gombra.

    ![frissítés ellenőrzése][3]


4. A frissítés elindítja és megjeleníti a folyamatban lévő értesítési. A frissítési folyamat kezdeményezni. Attól függően, hogy az adott adatbázisok részleteit V12 való frissítés időt vehet igénybe néhány. Ez idő alatt a kiszolgálón adatbázisokra online marad, de a kiszolgáló és az adatbázis-kezelés műveleteket korlátozott lesz.

    ![folyamatban lévő frissítése][4]

    Az új teljesítményének tényleges áttűnés idején ideiglenes kapcsolatot az adatbázis leválasztása szint akkor fordulhat elő, a nagyon kicsi időtartama (általában másodpercben). Ha az alkalmazás tranziens hiba kezelésének (újrapróbálkozási logika) a kapcsolat végződnek akkor elegendő, ha a frissítés végén kapcsolatok megszakadnak vírusvédelmet.

5. A frissítési művelet befejezése után a **Legújabb frissítésben** lap **engedélyezve**jeleníti meg.

    ![V12 engedélyezve][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Az adatbázisok áthelyezése egy rugalmas adatbázis készlet

Az [Azure portál](https://portal.azure.com/) keresse meg a V12 kiszolgáló, és kattintson a **Hozzáadás készlet**.

– vagy –

Ha megjelenik egy üzenet, amely arról tájékoztat, **Ide kattintva megtekintheti a javasolt rugalmas adatbázis készletek kiszolgáló**, kattintson rá, egyszerűen létrehozhat egy készletet, a kiszolgáló adatbázisok optimalizált. A részletekért olvassa [árát és teljesítménybeli szempontok rugalmas adatbázis-készlet](sql-database-elastic-pool-guidance.md).

![A kiszolgáló készlet hozzáadása][7]

Kövesse az útmutatást követve hozza létre a készlet [létrehozása egy rugalmas adatbázis készlet](sql-database-elastic-pool.md) cikkben.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Lync-adatbázisok SQL-adatbázis V12-re frissítés után

>[AZURE.IMPORTANT] Az SQL Server Management Studio (SSMS) kihasználhatja az új v12 funkciók legújabb verziójára frissít. [Az SQL Server Management Studio letöltés] (https://msdn.microsoft.com/library/mt238290.aspx).

A frissítés után aktívan figyelése annak érdekében, hogy a kívánt teljesítmény alkalmazások futtatott az adatbázis, és kattintson a kívánt beállításokat, optimalizálása.

Nemcsak a nyomon követés egyes adatbázisokat, figyelheti a [Monitor, felügyeletét látja el, és a méret egy rugalmas adatbázis készlet az Azure portálján](sql-database-elastic-pool-manage-portal.md) rugalmas adatbázis-készletek vagy a [PowerShell](sql-database-elastic-pool-manage-powershell.md).


**Erőforrás felhasználási adatok:** Egyszerű, normál és Premium adatbázisok erőforrás felhasználási adatok keresztül is elérhető a [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV a felhasználói adatbázisban. A DMV biztosít a valós idejű erőforrás felhasználási adatait a 15 második Granularitás az előző óra művelet közelében. A DTU százalékos felhasználás intervallumhoz számítja ki, hogy a maximális százalékos egységarányt felhasználás Processzor, IO, és jelentkezzen be méretek közül. A következő lekérdezés számítja ki az utolsó óra fölé átlagos DTU százalékos felhasználás:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

További felügyeleti információk:

- [Egyetlen adatbázisok azure SQL-adatbázis teljesítmény útmutatást](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Rugalmas adatbázis-készlet árát és teljesítménybeli szempontok](sql-database-elastic-pool-guidance.md).
- [Figyelés Azure SQL-adatbázis kezelése dinamikus nézetek használata](sql-database-monitoring-with-dmvs.md)




**Értesítések:** Beállíthat "Figyelmeztetés" az Azure-portálon értesítést a DTU felhasználás frissített adatbázisok közelít bizonyos magas szintű. Adatbázis-értesítések beállítási az Azure-portálon különböző teljesítménymutatók, például a DTU, Processzor, IO és Log for lehet. Tallózással keresse meg az adatbázist, és válassza ki **a figyelmeztetési szabályok** a **Beállítások** lap.

Például beállíthatja, hogy be egy e-mailben riasztás "DTU százalékos" Ha átlagos DTU százalékos értéke meghaladja a 75 %-át az utolsó 5 perc alatt. Értesítések beállításáról további információt a hivatkozott riasztási értesítést szeretne [kapni](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) .





## <a name="next-steps"></a>Következő lépések

- [Javaslatok készlet és -készlet létrehozása a ellenőrzése](sql-database-elastic-pool-create-portal.md).
- [Az adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása](sql-database-scale-up.md).



## <a name="related-links"></a>Kapcsolódó hivatkozások

- [SQL-adatbázis V12 újdonságai](sql-database-v12-whats-new.md)
- [Megtervezése és a Felkészülés a SQL-adatbázis V12 frissítése](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
