<properties 
   pageTitle="Árak réteg javaslatok Azure SQL-adatbázis" 
   description="Árak módosításakor az Azure-portálon réteg javaslatok árak rétegek vannak, feltéve hogy ajánlott a réteg, amely a leginkább alkalmas rendszert futtató egy meglévő Azure SQL-adatbázis a terhelést. Árak rétegek SQL-adatbázis szolgáltatás réteg és a teljesítmény szintjét ismertetik." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>SQL-adatbázis árak réteg javaslatok

 Árak réteg javaslatok ajánlja fel a szolgáltatási réteg és a teljesítmény szintet, amely egy meglévő Azure SQL-adatbázis terhelést futtatásához a legmegfelelőbb.

> [AZURE.NOTE] Árak réteg javaslatok csak rendelkezésre állnak a webről és az üzleti és rugalmas adatbázis készletek – és csak az [Azure portál](https://portal.azure.com/)érhető el.


Réteg javaslatok első árak közben az alábbi műveleteket:

- [Szolgáltatás réteg és a teljesítmény szintjének módosítása (árak réteg) SQL-adatbázishoz](sql-database-scale-up.md)
- [Azure SQL server V12 frissítése](sql-database-upgrade-server-portal.md)
- Tallózással keresse meg a V12 kiszolgáló. Lásd: az [SQL-adatbázis árak réteg javaslatok](sql-database-service-tier-advisor.md).
- [Rugalmas adatbázis-készlet létrehozása](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>– Áttekintés

Az SQL-adatbázis-szolgáltatás elemzi aktuális teljesítmény és a szolgáltatás követelmények felmérése korábbi Erőforrás kihasználtsága SQL-adatbázishoz révén. Ezenkívül a minimális elfogadható szolgáltatási réteg méretét az adatbázist, és [üzleti folytonosságot](sql-database-business-continuity.md) engedélyezett funkciók alapján lesz meghatározva. 

Ez az információ elemzése van, és a szolgáltatási réteg és a teljesítmény szintet, amely a leginkább alkalmas az adatbázis tipikus terhelést fut, és fenntartása aktuális szolgáltatáskészlete ajánlott.

- A szolgáltatás megvizsgálja az előző 15-30 nap korábbi adata (erőforrás-kihasználtság, az adatbázis mérete és adatbázis tevékenységek), és erőforrások elfogyasztott összegét és a jelenleg elérhető szolgáltatási rétegek és teljesítményszint tényleges korlátozások összehasonlítása hajt végre.
- Adatok elemzése a 15 második intervallumokba van, és minden intervallum resultset kategóriába esik, a meglévő szolgáltatási réteg be, és teljesítményszint, amely a leginkább alkalmas kezelése, hogy resultset terhelést.
- Ezek 15 másodperc minták vannak majd összesíti az nagyobb 15-30 nap elemzésben és optimálisan képes kezelni a terhelést a korábbi 95 %-át a szolgáltatási réteg és a teljesítmény szintet ajánlott.

### <a name="recommendations"></a>Javaslatok

Az adatbázis használatát alapján, jelenleg 2 kategóriába sorolhatók is lehet feltárása javaslatok:


| Javaslat | Leírás |
| :--- | :--- |
| Frissítés | Frissíteni egy új réteget. |
| Nem érhető el | Adatbázis szükséges minimális terhelési vagy a tevékenység körülbelül 35 napot. Nincs elegendő adatok megadására érvényes ajánlást. |

## <a name="getting-pricing-tier-recommendations"></a>Az első árak réteg javaslatok

Réteg javaslatok árak jelöljön ki egy meglévő adatbázis webes vagy vállalati beszerzése, kattintson a **minden elérhető beállítás**, majd **árak réteg (méretarány DTUs)**. (Árak réteg javaslatok szintén kapható, amikor Ön [V12 frissítése Azure SQL server](sql-database-upgrade-server-portal.md).)

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **Tallózás** > **SQL-adatbázisait**.
4. Kattintson az adatbázist, amely meg szeretné jeleníteni a való megjelöléséről az **SQL-adatbázisait** lap:

    ![Jelölje ki az adatbázisban][1]

5. Az adatbázis lap a válassza az **összes beállítást** , majd koppintson a **árak réteg (méretarány DTUs)**.


7. A **réteg árak ajánlott** nyissa meg, ahol a javasolt réteg gombra, és kattintson a módosítása, hogy a réteg **kiválasztása** gombra.

    ![Iratkozzon fel az előzetes verzió][4]

8. Másik lehetőségként kattintson a **Részletek használatát** , nyissa meg a **Árak réteg ajánlási részletek** lap, ahol megtekintheti a javasolt réteg az adatbázishoz, az aktuális és ajánlott rétegek, és a korábbi Erőforrás kihasználtsága elemzés grafikonon szolgáltatás összehasonlítása.

    ![Iratkozzon fel az előzetes verzió][5]



## <a name="summary"></a>Összefoglalás

Árak réteg javaslatok adja meg az eszköz minden SQL-adatbázis telemetriai adatok összegyűjtése és a legjobb szolgáltatás szint/teljesítmény szintű kombináció egy adatbázis tényleges teljesítmény igényeinek, és a szolgáltatás követelmények alapján ajánlása az automatizált. Árak bármely webes és az üzleti adatbázisok réteg javaslatok megjelenítéséhez kattintson a beállítások lap **árak réteg (méretarány DTUs)** .



## <a name="next-steps"></a>Következő lépések

Attól függően, hogy az adott adatbázis részleteit való frissítés, illetve visszalépéssel kapcsolatos elvégzéséhez általában nem kerül sor azonnal. A portálon értesítések fog szolgálni, mint az adatbázis áttűnések a vele új réteg, vagy a frissítési állapot figyelheti a fő adatbázist az SQL adatbáziskiszolgálóhoz [sys.dm_operation_status (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/dn270022.aspx) megtekintés lekérdezésével.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
