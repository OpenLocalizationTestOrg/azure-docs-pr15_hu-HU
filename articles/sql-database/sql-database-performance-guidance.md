<properties
    pageTitle="Azure SQL-adatbázishoz, és a teljesítmény egyetlen adatbázisok |} Microsoft Azure"
    description="Ez a cikk segíthetnek annak eldöntésében, hogy melyik szolgáltatási réteg választhatja ki az alkalmazás. Azt is optimalizálhatja az alkalmazás hozhatja ki a legtöbbet a Azure SQL-adatbázis módjai javasolja."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL-adatbázishoz, és a teljesítmény egyetlen adatbázisok

Azure SQL-adatbázis kínál három [szolgáltatási rétegek](sql-database-service-tiers.md): egyszerű, normál és Premium. Minden egyes szolgáltatási réteg szigorúan elkülöníti az erőforrások az SQL-adatbázis használhatja, és garantálja szolgáltatás szintnek kiszámítható teljesítményét. Ebben a cikkben kínálunk, amely segít eldönteni, az alkalmazás a szolgáltatási réteg útmutatást. Is ölelik módon, hogy optimalizálhatja a az alkalmazás szerez Azure SQL-adatbázisból.

>[AZURE.NOTE] Ez a cikk a teljesítmény útmutatást Azure SQL-adatbázisban egyetlen adatbázisok koncentrál. Teljesítmény rugalmas adatbázis készletek kapcsolódó című témakörben olvashat [Rugalmas adatbázis készletek árát és a teljesítmény szempontjai](sql-database-elastic-pool-guidance.md). Figyelje meg, azonban, amelyet az eszközbeállítási ajánlások, a jelen cikkben számos rugalmas adatbázis-készletben adatbázisokra vonatkoznak, és hasonló teljesítmény előnyök.

Ezek a három Azure SQL-adatbázis szolgáltatási rétegek, amelyek közül választhat (teljesítmény adatbázis kapacitása egységek vagy [DTUs](sql-database-what-is-a-dtu.md)mértékegysége:

- **Egyszerű**. Az egyszerű szolgáltatás réteg ajánlatok jó teljesítményt előreláthatóság-adatbázisonként, óra óra alatt. Egyszerű adatbázis-kellő erőforrások támogatja a jó teljesítményt több egyidejű kérelmek nem rendelkező kis adatbázisban.
- **Szabványos**. A szokásos szolgáltatás réteg nagyobb teljesítmény előreláthatóság kínál, és az szerepel-e több egyidejű kérelmek, például a munkacsoport és a webes alkalmazásokban létrehozott adatbázisok hatványra. Ha úgy dönt, hogy a szokásos szolgáltatás réteg adatbázis, az adatbázis-alkalmazás kiszámíthatóan teljesítmény alapján méretét perc perc alatt.
- **Prémium verzióban**. A támogatási szolgáltatás réteg biztosít kiszámíthatóan teljesítmény elérése érdekében második,-prémium adatbázisonként második fölé. Ha úgy dönt, hogy a támogatási szolgáltatás réteg, az adatbázis-alkalmazás, a csúcs betöltés arra az adatbázisra alapján méretét. A terv eltávolítja esetek, amelyek teljesítmény eltérése okozhatják a kis lekérdezések késés érzékeny műveletek a vártnál hosszabb időt vehet igénybe. A modell nagyban egyszerűbbé teszi a fejlesztés és a termék érvényességi ciklus alkalmazásokat, amelyek erős kimutatások csúcs erőforrásigények, teljesítmény eltérése és lekérdezési időtartam szükséges.

Az egyes szolgáltatási réteg beállíthatja, a teljesítményszint, így Önnek nem kell a lehetősége, hogy csak a szükséges kapacitás vásárolni. Azt is megteheti [módosítsa a beosztását](sql-database-scale-up.md), felfelé vagy lefelé, amikor terhelést megváltozik. Az adatbázis terhelést hangosítva a vissza-iskolai vásárlási időszak alatt, előfordulhat, hogy beállított ideje, szeptember – július például növelje meg az adatbázist a teljesítmény szintjét. A csúcs évszak befejeződésekor csökkentheti. Kis méretűre optimalizálása a felhőalapú környezetben a szezonalitást a cége szerint fizet. Ez a modell is jól szoftver termék megjelenés ciklus működik. A próba-csoport előfordulhat, hogy lefoglalhat kapacitás, miközben azt tesztelése fut, és engedje fel, hogy a kapacitás tesztelés befejezésekor. Egy kapacitás kérelem modell fizet kapacitás szüksége lenne rá, és elkerülése érdekében, lehet, hogy a ritkán használt dedikált erőforrások kiadások.

## <a name="why-service-tiers"></a>Miért szolgáltatás rétegek?

Minden adatbázisban terhelést eltérő, bár a szolgáltatási rétegek célja számára teljesítmény előre kiszámítható teljesítmény több szinten. Nagyméretű adatbázis erőforrásigények ügyfelek nagyobb kijelölt számítógépes környezetben is dolgozhat.

### <a name="common-service-tier-use-cases"></a>Közös szolgáltatási réteg használjon esetekben

#### <a name="basic"></a>Egyszerű

- **Akkor most kezdett az Azure SQL-adatbázis**. Alkalmazások fejlesztésének gyakran szereplő nagy teljesítményű szintek nem szükséges. Egyszerű adatbázisok adatbázis fejlesztési egy alacsony ár pontján ideális környezetben is.
- **Egyetlen felhasználó adatbázis van**. Egyetlen felhasználó általában társítása egy adatbázis-alkalmazások nincs magas feldolgozási és a teljesítmény követelményeknek. Ezeket az alkalmazásokat az egyszerű szolgáltatási réteg jelentkezők.

#### <a name="standard"></a>Normál

- **Az adatbázis több egyidejű kérelmek rendelkezik**. Alkalmazások, hogy az általában egyszerre több felhasználónak kell nagyobb teljesítményszint. Webhelyeket, mérsékelt forgalom vagy több erőforrást igénylő részlegszintű alkalmazások mindegyike legalkalmasabbak a szokásos szolgáltatás réteg.

#### <a name="premium"></a>Támogatás

Támogatási szolgáltatás réteg használata legtöbbször valamelyike jellemzők:

- A **magas csúcs betöltése**. Olyan alkalmazás, amely sok Processzor, memória vagy bemeneti és kimeneti (I/O), a műveletek elvégzéséhez szükséges egy saját, magas teljesítményszint szükséges. Például egy adatbázis-művelet ismert több Processzormagok használhatnak hosszabb ideig is a támogatási szolgáltatás réteg csatlakozni kívánó.
- **Sok egyidejű kérelmek**. Néhány adatbázis-alkalmazások a szolgáltatás sok egyidejű kérelmek, például amikor egy nagy forgalma felszolgálásához egy webhelyet, amely tartalmaz. Egyszerű és a normál szolgáltatási rétegek egyidejű kérelmek egy adatbázis számának korlátozása. Több kapcsolat igénylő alkalmazások kellene válassza ki a megfelelő foglalás méretének kezeléséhez szükséges kérések maximális száma.
- **Kis késés**. Egyes alkalmazások kell garantálja az adatbázisból választ minimális időben. Ha egy adott tárolt eljárás nevezzük egy szélesebb ügyfél során, lehet, hogy annak követelménye, hogy egy adott hívás vissza legfeljebb 20 ezredmásodpercben az idő 99 %-át. A következő típusú alkalmazás előnyei a prémium szolgáltatási réteg, győződjön meg arról, hogy a szükséges számítógépes power érhető el.

A szolgáltatás szinten van szüksége az SQL-adatbázis attól függ, hogy minden erőforrás dimenzió csúcs betöltés követelményei. Egyes alkalmazások használata egy erőforrást egy trivial mennyiségű, de más erőforrások jelentős követelményeik vannak.

## <a name="service-tier-capabilities-and-limits"></a>Szolgáltatási réteg funkciókkal és korlátai
Szolgáltatás réteg és a teljesítmény szintenként társítva a különböző korlátai és a teljesítmény jellemzőiket befolyásolják. Az alábbi táblázat összefoglalja az egyetlen adatbázis e jellemzők.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

A következő szakaszokkal van használatába, ezek a korlátok kapcsolatos részletes tájékoztatást.

### <a name="maximum-in-memory-oltp-storage"></a>Maximális a memóriában OLTP tárhely

A **sys.dm_db_resource_stats** nézet segítségével figyelemmel kísérheti az Azure a memóriában tároló használja. További információt a figyelése a [Monitor a memóriában OLTP tároló](sql-database-in-memory-oltp-monitoring.md)témakörben olvashat.

>[AZURE.NOTE] Azure a memóriában online tranzakció feldolgozása (OLTP) előzetes verzió jelenleg csak az egyetlen adatbázisok használata támogatott. Nem módosítható vele az adatbázisokkal az rugalmas adatbázis készletek.

### <a name="maximum-concurrent-requests"></a>Maximális egyidejű kérelmek

Egyidejű kérelmek számának megtekintéséhez az SQL-adatbázis a Transact-SQL-lekérdezés futtatása:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

A terhelést a egy helyszíni SQL Server-adatbázis elemezni, módosítsa a lekérdezés az adott adatbázis szűrni szeretné kielemezni. Ha például egy adatbázis nevű helyi adatbázis esetén a Transact-SQL-lekérdezés számát adja vissza egyidejű kérelmek az adott adatbázis:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Ez a csak egy ponton pillanatkép időben. Úgy juthat az terhelést és egyidejű kérelem követelmények megértéséhez, kell sok minták összegyűjtése idővel.

### <a name="maximum-concurrent-logins"></a>Maximális egyidejű bejelentkezések

A felhasználó alkalmazás minták és a gyakoriság bejelentkezések arról megszerezni elemezheti. Valós életből terhelések tesztkörnyezetben győződjön meg arról, hogy akkor esetén nem szerezze meg ezzel vagy más témákat ölelik fel a jelen cikkben korlátai is futtatni. Egyetlen lekérdezésbe vagy dinamikus kezelése nézetek (DMV), hogy meg tudja jeleníteni egyidejű bejelentkezési megszámolja vagy előzmények nem.

Ha több ügyfél ugyanazt a kapcsolati karakterláncot, a szolgáltatás hitelesíti a minden jelentkezzen be. Ha 10 felhasználó egyidejű csatlakozni adatbázis az ugyanazon felhasználónév és jelszó használatával, a 10 egyidejű bejelentkezések lenne. Ezt a korlátot csak a bejelentkezési és a hitelesítéshez hosszának vonatkozik. Ha az azonos 10 felhasználó egymás után csatlakozik az adatbázishoz, egyidejű bejelentkezések száma soha nem lenne 1-nél nagyobb.

>[AZURE.NOTE] Ezt a korlátot jelenleg nem vonatkoznak azokra az adatbázisokra rugalmas adatbázis készletek.

### <a name="maximum-sessions"></a>Maximális munkamenetek

Az aktuális aktív munkamenet számának megtekintéséhez az SQL-adatbázis a Transact-SQL-lekérdezés futtatása:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Ha egy helyszíni SQL Server terhelést esetén elemzése, a lekérdezés szűkítheti az adott adatbázis módosítása A lekérdezés segítségével eldöntheti, hogy az adatbázis esetleges munkamenet van szükség, ha tervezi, áthelyezése a Azure SQL-adatbázis.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Ezek a lekérdezések újra, lépjen vissza az a pont és az idő száma. Ha több minták adott idő alatt, a legjobb ismertetése a munkamenet használata fognak rendelkezésére állni.

Az SQL-adatbázis-elemzés múltbeli statisztika munkamenetek elérheti. A lekérdezés **sys.resource_stats**, és használja a **active_session_count** oszlopot. Ez a nézet használatáról további információt a következő szakaszban olvashat.

## <a name="monitor-resource-use"></a>Erőforrás-használat figyelése
Két nézet segítséget nyújtanak a szolgáltatási réteg viszonyított SQL-adatbázishoz erőforrás-használat figyelése:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Minden SQL-adatbázisban a [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) nézetben is használhatja. A **sys.dm_db_resource_stats** nézet a legutóbbi erőforrás-használati adatok a szolgáltatási réteg viszonyított jeleníti meg. Processzor, az adatok I/O, a naplófájl írások és a memória átlagos százalékos 15 másodpercenként a rendszer rögzíti, és 1 óra esetében is megőrződnek.

Mivel ez a nézet finomabb figyelmébe az erőforrás-használat, használja a **sys.dm_db_resource_stats** első bármely aktuális állapot elemzéshez vagy hibaelhárítási. A lekérdezés például megmutatja, az aktuális adatbázis átlagos és a legnagyobb erőforrás használata az elmúlt egy órában keresztül:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Más lekérdezések esetén lévő példák [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats

A **fő** adatbázist [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) nézetében, amelyek segítséget nyújtanak az SQL-adatbázis az adott szolgáltatás réteg és a teljesítmény szintjén a teljesítmény figyelését további információt tartalmaz. Az adatok összegyűjtött 5 percenként és körülbelül 35 napig karbantartása. Ez a nézet akkor lehet hasznos, hogyan alkalmazza a az SQL-adatbázis az erőforrások hosszú távú korábbi elemzéshez.

A következő ábra a Processzor erőforrás használatának prémium adatbázis a P2 teljesítményszint minden olyan órában hetente jeleníti meg. A diagram hétfőn elindul, 5 munkanapok jeleníti meg, és megjeleníti a hétvégi, ha sok kisebb történik meg az alkalmazást.

![SQL-adatbázis erőforrás-használat](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Az adatok, ez az adatbázis által jelzett ugyanúgy, mint 50 százalékos terhelés Processzor csúcs Processzor használata viszonyított a P2 teljesítményszint (kedd délig). Ha Processzor a meghatározó tényező az alkalmazás az erőforrás-profilhoz, akkor előfordulhat, hogy döntse el, hogy P2-e a megfelelő teljesítményszint annak biztosítására, hogy a terhelést a mindig illeszkedik. Adott idő alatt a nagyobb alkalmazás által elvárt esetén célszerű az egy további erőforrás puffer van, hogy az alkalmazás minden eddiginél nem éri a teljesítmény szintű korlátot. Ha a teljesítmény növelése segítséget nyújthat adatbázis nincs elég power kérések feldolgozása hatékony, különösen a késés érzékeny környezetben ismertetjük ügyfél látható hibák elkerülése érdekében. Példa egy adatbázist, amely támogatja az adatbázis-hívások eredményén alapuló weblapokra fest alkalmazást.

Figyelje meg, hogy más alkalmazás típusú előfordulhat, hogy másképp értelmezi ugyanazon a diagramon. Például az alkalmazások próbál adatfeldolgozás bérszámfejtő minden nap és az ugyanezen a diagramon, "köteg feladat" modell ilyen típusú előfordulhat, hogy végre finom P1 teljesítmény szinten lévő. A P1 teljesítményszint P2 teljesítmény szintre 200 DTUs összehasonlítva 100 DTUs tartalmaz. A P1 teljesítményszint biztosít a P2 teljesítményszint fele ellátása. Igen P2 Processzor használatban 50 %-át egyenlő P1 100 %-os Processzor használatban. Ha az alkalmazás nincs időtúllépései, előfordulhat, hogy nem számít, hogy a feladat befejezéséhez 2,5 óraszám vagy 2 megnyitja Ha ma kapja meg. Az alkalmazás, ebbe a kategóriába valószínűleg egy P1 teljesítményszint is használhatja. Használatba veheti azt, hogy nincsenek-e az erőforrás-használat esetén alsó, így minden "nagy csúcs" Előfordulhat, hogy spill a csatornák egyikébe a napon belül nap során időszakok. A P1 teljesítményszint mindaddig, amíg a feladatok befejezheti a minden nap hosszú időt valószínűleg helyes-e az adott alkalmazás (és mentés pénz).

Azure SQL-adatbázis biztosítja a **sys.resource_stats** nézetben, a **fő** minden server-adatbázis aktív adatbázisonként erőforrás-információiról felhasznált. Az adatokat a táblázat program összesíti az 5 perc intervallumok. Az adatok Basic, normál és Premium szolgáltatási rétegek jelennek meg a táblázatban, ezért ezeket az adatokat még hasznosabbá közelében-valós idejű analysis helyett korábbi analysis több, mint 5 percig is tarthat. A lekérdezés **sys.resource_stats** adatbázis a legutóbbi előzményének megtekintéséhez és ellenőrzése, hogy a választott foglalás kézbesítve a teljesítmény-e meg a kívánt nézetet szükség esetén.

>[AZURE.NOTE] Meg kell csatlakoznia az a logikai SQL-adatbázis kiszolgálója a lekérdezés **sys.resource_stats** az alábbi példákban a **fő** adatbázist.

Ez a példa megtudhatja, hogyan az adatokat, ebben a nézetben van téve:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![A sys.resource_stats katalógus nézet](./media/sql-database-performance-guidance/sys_resource_stats.png)

A következő példa bemutatja, a **sys.resource_stats** katalógus nézet használatával hogyan az SQL-adatbázis az erőforrásokat használó adatainak többféleképpen:

1. A múlt héten erőforrás vizsgálata az adatbázis userdb1 használni, a lekérdezés futtatását is lehetővé teszi:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Értékelni mennyire a terhelést illeszkedik-e a teljesítményszint, kell az erőforrás-mértékek az egyes funkcióival Lehatolás: Processzor, Olvasás, írások, munkatársak számát és a munkamenetek számát. Az alábbiakban a módosított lekérdezések **sys.resource_stats** jelentése az alábbi erőforrás mértékek átlagos és maximális értékek:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Ezekkel az információkkal kapcsolatban az átlag és maximális értékek minden egyes erőforrás mérőszám felmérheti, milyen jól illeszkedik az a terhelést a kiválasztott teljesítményszint. Általában **sys.resource_stats** átlagos értékeket az szemben a cél méret használata egy jó eredeti ad. Az elsődleges mérési meghajtóra kell tenni. Példát előfordulhat, hogy kell használni a szokásos szolgáltatás réteg S2 teljesítményszint. Átlagát százalékértékek használható Processzor- és I/O olvasás írások 40 százalék alatt, a dolgozók átlagos száma nem éri el 50 és munkamenetek átlagos száma nem éri el 200. A terhelést a S1 teljesítményszint előfordulhat, hogy elférjen. Nagyon egyszerűen tekintheti meg, hogy az adatbázis elférjen a dolgozó és a munkamenet korlátai. Megjelenítéséhez, hogy az adatbázis illeszkedik teljesítmény alacsonyabb szinten Processzor, tehát felolvassa és írások, az alsó teljesítményszint az aktuális teljesítményszint DTU számú DTU száma osztva, és az eredményt megszorozza 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Az eredmény a két teljesítményszint százalékban relatív teljesítmény különbségét lesz. Az erőforrás-használati nem haladja meg ezt az értéket, ha a terhelést a alsó teljesítményszint előfordulhat, hogy elférjen. Azonban kell tekintse meg az erőforrás-használati értékek minden tartomány, és meghatározzák, hogy, százalékkal, milyen gyakran az adatbázis terhelést az alsó teljesítményszint elférjen tenné. Az alábbi lekérdezés exportálja az igazítás százalékos egy erőforrás dimenzió, 40 százalékos, hogy azt ebben a példában a küszöbértékét alapján:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Az adatbázis szolgáltatás szintű cél (ZAT) alapján, eldöntheti, hogy a terhelést a alsó teljesítményszint illeszkedik-e. Ha az adatbázis terhelést ZAT 99,9 % és a fenti lekérdezés értékeket adja vissza összes három erőforrás dimenzió 99,9 %-nál nagyobb, valószínűleg a terhelést a alsó teljesítményszint elfér.

    Megjeleníti az igazítás százalékos ad is kell-e lépjen a következő magasabb teljesítményszint felel meg a ZAT betekintést. Userdb1 például a múlt héten a következő Processzor használatát mutatja be.

  	| Átlagos Processzor százalék | Maximális Processzor százalék |
  	|---|---|
  	| 24.5 | 100,00 |

    Az átlag Processzor az szeretne jól illeszkedik a teljesítményszint az adatbázis teljesítményszint határértékén negyedéves. De a legnagyobb értéket jeleníti meg, hogy az adatbázis eléri a teljesítményszint korlátot. Szükség van a következő magasabb teljesítményszint áthelyezése? Keresse meg, hogy miként számú alkalommal a terhelést eléri 100 %-os, és hasonlítsa össze az adatbázis terhelést ZAT van.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Ha ezt a lekérdezést ad eredményül 99,9 %-kisebb, mint bármely a három erőforrás méretek, fontolja meg, vagy nagyobb teljesítmény felsőfokon áthelyezése vagy technikákat alkalmazás beállítása segítségével csökkentse a terhelést a SQL-adatbázishoz.

4. Ebben a gyakorlatban is figyelembe veszi a tervezett terhelést növelése a jövőben.

## <a name="tune-your-application"></a>Az alkalmazás finomhangolása

Hagyományos helyszíni SQL Server-alapú kezdeti kapacitás tervezés során gyakran van elválasztva egy alkalmazást futtató gyártási folyamata a. Először hardver- és licencet vásárolt, és utána teljesítményhangolás befejeződött. Azure SQL-adatbázis használata esetén célszerű az alkalmazás fut, és azt finombeállítása folyamata interweave. Fizet a kapacitás igény mintája is beállításakor az alkalmazás későbbi növekedési tervek, amelyeket gyakran helytelen az alkalmazáshoz feltételezésekkel alapján hardveren overprovisioning helyett most szükséges minimális erőforrások használhatja. Egyes ügyfelek előfordulhat, hogy nem szeretné, az alkalmazások finomhangolása, és ehelyett hardver-erőforrások overprovision. Ezt a megközelítést célszerű lehet, ha nem szeretné módosítani a fő alkalmazások elfoglalt időszakára. De az alkalmazás beállítása kis méretűre erőforrásigények és alsó havi számlák Azure SQL-adatbázisban a szolgáltatási rétegek használatakor.

### <a name="application-characteristics"></a>Alkalmazás jellemzők

Bár javítható teljesítményének stabilitás és a előreláthatóság az alkalmazáshoz készült Azure SQL-adatbázis szolgáltatási rétegek, ajánlott eljárásokat segítséget nyújt az alkalmazás jobban kihasználhatja az erőforrásokat a teljesítmény szinten finomhangolása. Bár a sok alkalmazás teljesítménybeli nyereség egyszerűen való átállításával teljesítmény magasabb szintre vagy szolgáltatási réteg, egyes alkalmazások kell további finombeállítása szolgáltatás magasabb szintű összekapcsolhatók. Teljesítmény növelése érdekében fontolja meg a további alkalmazások beállítása a alkalmazásokat, amelyek e jellemzőkkel rendelkeznek:

- **Alkalmazások, amelyek miatt "chatty" viselkedés lelassulhat**. Chatty alkalmazások ügyeljen fölösleges access adatműveleteket, amely érzékenyek a hálózati késés. Előfordulhat, hogy módosítania az ilyen típusú alkalmazásokat adatműveleteket access SQL-adatbázishoz számának csökkentése érdekében. Alkalmazás teljesítményének javíthatja például technikák, például alkalmi lekérdezések kötegelés vagy áthelyezése a lekérdezések tárolt eljárások segítségével. További tudnivalókért olvassa el a [Köteg lekérdezések](#batch-queries)című témakört.
- **Adatbázis-intenzív terhelést, amely egy teljes egyetlen gépi által nem támogatott**. Előfordulhat, hogy a adatbázisok, amelyekre a legmagasabb prémium teljesítményszint forrásai élvezhetik el a terhelést a méretezés. További tudnivalókért lásd: [határokon adatbázis - sharding](#cross-database-sharding) és [funkcionális szétválasztás](#functional-partitioning).
- **Alkalmazások, amelyek optimálisnál lekérdezések**. Alkalmazások, különösen az adat-hozzáférési réteg, rosszul van beállítva, lekérdezések, előfordulhat, hogy nem élvezhetik teljesítmény magasabb szintre. Ide tartoznak a WHERE záradékot helyét, az indexek a hiányzó vagy statisztika elavult lekérdezéseket. Ezeket az alkalmazásokat élvezhetik szokásos lekérdezés teljesítményének javítása technikákat alkalmaz. További tudnivalókért lásd: a [Hiányzó indexek](#missing-indexes) és a [lekérdezés javítása és adatait](#query-tuning-and-hinting).
- **Alkalmazások, amelyek optimálisnál adatok eléréséhez tervezés**. Belső adatok access verzió-ellenőrzési problémák, például deadlocking, rendelkező alkalmazások nem lehet kihasználni teljesítmény magasabb szintre. Fontolja meg az Azure SQL-adatbázis elleni ciklikus utakat csökkentését az Azure gyorsítótár-szolgáltatás vagy egy másik gyorsítótárazási technológiát ügyféloldali gyorsítótárazási adatok alapján. Lásd: [alkalmazás réteg gyorsítótárazás](#application-tier-caching).

## <a name="tuning-techniques"></a>Módszerek beállítása
Ebben a részben, amelyeket hozzá megnézi az egyes technikákat Azure SQL-adatbázis szerezhet a legjobb teljesítmény elérése érdekében az alkalmazás, és indítsa el a legalacsonyabb szintű lehetséges teljesítményének finomhangolása használható. Néhány, az alábbi eljárások egyezik-e hagyományos SQL Server finombeállítása ajánlott eljárások, de más alkalmazásokat adott Azure SQL-adatbázishoz. Egyes esetekben vizsgálja meg a felhasznált erőforrások az adatbázis további finomhangolása és bővítése a hagyományos SQL Server technikákat, amelyekkel Azure SQL-adatbázis használata területek kereséséhez.

### <a name="azure-portal-tools"></a>Azure portál eszközök
Két eszközök, amelyek segítséget nyújtanak az Azure portálon elemzése és teljesítménnyel kapcsolatos problémák megoldása az SQL-adatbázissal találhatók:

- [Lekérdezési teljesítmény betekintést](sql-database-query-performance.md)
- [SQL-adatbázis Advisor](sql-database-advisor.md)

Az Azure portál mindkét ezek az eszközök és azok használatáról további információt tartalmaz. Hatékony diagnosztizálása és javítása problémák, azt javasoljuk, hogy először próbál az eszközök, az Azure-portálon. Azt javasoljuk, hogy a manuális beállítása, hogy ölelik ezután hiányoznak az index és a lekérdezés beállítása, bizonyos esetekben az alábbi módszerek használja.

### <a name="missing-indexes"></a>Hiányzó indexek
Az adatbázis-teljesítményének OLTP közös probléma a fizikai adatbázisterv vonatkozik. Gyakran adatbázis sémák tervezett, és tesztelje a méretarány (akár a betöltés vagy az adatok mennyiségi) teljesült. Sajnos a lekérdezéstervet teljesítményének előfordulhat, hogy elfogadható, kattintson a kisméretű, de jelentősen csökkentheti a termelési szintű adaton csoportban. Ez a probléma leggyakoribb forrásának hiányoznak a megfelelő indexek igényeinek kielégítéséhez, szűrők és egyéb korlátozásokkal egy lekérdezésben. Hiányzó indexek jegyzékfájlok táblázatként gyakran, tekintse át, ha az index seek is elegendő.

Ebben a példában a kijelölt lekérdezés csomagot a beolvasott képet használ, amikor egy seek elegendő:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![A hiányzó indexek a lekérdezéstervet](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL-adatbázis segítséget nyújtanak a Keresés és javítás közös hiányzó indexelendő feltételeket. Azure SQL-adatbázis beépített DMVs, amelyben indexet szeretne jelentősen csökkenti a lekérdezés futtatásához becsült költség lekérdezés fordítások megtekintése: Lekérdezés végrehajtásakor SQL-adatbázis nyomon követi a gyakoriságának minden lekérdezéstervet megy végbe, és nyomon követi a becsült távolság a végrehajtó lekérdezéstervet és az imagined között hol tárgymutató volt. Használhatja ezeket DMVs gyorsan kitalálhatják a megosztott melyik az fizikai adatbázisterv módosításai javíthatja az adatbázis és a valós terhelést általános terhelést költségét.

Használhatja ezt a lekérdezést potenciális hiányzó indexek ki szeretné számítani:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

Ebben a példában a lekérdezés eredménye a javaslatok:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Létrehozás, után, hogy ugyanabban a SELECT utasításban választja ki egy másik csomagra, mely egy seek használja helyett a beolvasott képet, és végrehajtja a terv hatékonyabban:

![A korrigált indexek a lekérdezéstervet](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

A fő betekintést érték, hogy a megosztott, örvend rendszert I/O kapacitása korlátozottabb, mint egy dedikált kiszolgáló számítógépre. Nincs támogatás szükségtelen bemenet-, hogy előnyeinek maximális kihasználásához a rendszer a a DTU az Azure SQL-adatbázis szolgáltatási rétegek a teljesítmény szintenként a kis méretre. Választási lehetőségek jelentősen növelheti a késés egyedi lekérdezésekhez megfelelő fizikai adatbázisterv javítása a kapacitásának egyidejű kérelmek egy időosztás egységet kezelt, és a költségek, a lekérdezésnek megfelelő szükséges összezárása. A hiányzó index DMVs kapcsolatos további tudnivalókért olvassa el a [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx)című témakört.

### <a name="query-tuning-and-hinting"></a>Lekérdezés beállítása és adatait
A lekérdezés-optimalizáló Azure SQL-adatbázisban hasonlít a hagyományos SQL Server-lekérdezés optimalizáló. A lekérdezések beállítása és érvelés megismerése gyakorlati tanácsok a legtöbb a lekérdezés-optimalizáló modell korlátozásai vonatkoznak Azure SQL-adatbázis. Ha optimalizálhatja a lekérdezések Azure SQL-adatbázisban, a további előnye összesítő erőforrás igények csökkentése jelenhet meg. Lehet, hogy az alkalmazás untuned egyenértékű-nél kisebb költség mellett futtatható, mivel a teljesítmény alacsonyabb szinten lévő futtatását is lehetővé teszi.

Példa a leggyakoribb SQL Server-alapú és amelyek Azure SQL-adatbázis is érinti hogyan a lekérdezés-optimalizáló "bitaláírásait" paramétereket. A lekérdezés-optimalizáló fordításkor, kiértékeli a paraméter határozza meg, hogy akkor hozhat létre egy további optimális lekérdezéstervet aktuális értékét. Bár ezt a stratégiát gyakran vezethet, amelyek jelentősen gyorsabb nélkül ismert paraméterértékeket lefordított terv lekérdezés csomagra, jelenleg működik imperfectly mindkét SQL Server-alapú és Azure SQL-adatbázisban. Előfordul, hogy a paraméter értéke nem felszippantásra, és előfordul, hogy a paraméter felszippantásra van, de a létrehozott terv optimálisnál paraméterértékeket a terhelést a teljes csoportját. A Microsoft, hogy adja meg a szándék elérését segítő további szándékosan és a paraméterek elemzés alapértelmezett viselkedésének felülbírálása magában foglalja a lekérdezési javaslatok (irányelvek). Gyakran javaslatok használata esetén háríthatja el olyan esetben, amelyben az SQL Server- vagy SQL Azure-adatbázis alapértelmezés egy adott felhasználói terhelés hiányos.

A következő példa bemutatja, hogyan hozhat létre a lekérdezés processzor optimálisnál, mind a teljesítmény és erőforrásigények van egy másik csomagra. Ebben a példában is látható, hogy a lekérdezés emlékeztető használata esetén csökkentheti a lekérdezés futtatása idő- és erőforrás követelmények az SQL-adatbázis:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

A telepítő kód táblát hoz létre, amely magában ferde adatok terjesztési. Az optimális lekérdezéstervet eltérő alapján melyik paramétert ki van jelölve. Sajnos a terv gyorsítótárazás viselkedése nem mindig fordítsa újra a lekérdezést a leggyakoribb paraméter értéke alapján. Igen lehetőség egy optimálisnál terv gyorsítótárazott és a használt hány érték, akkor is, ha egy másik csomagra átlagosan terv célszerűbb lehet is. Ekkor a lekérdezéstervet hoz létre, amelyek megegyeznek, azzal a különbséggel, hogy senkinek a speciális lekérdezési emlékeztető két tárolt eljárások.

**A példában rész 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**A példában rész 2**

(Azt javasoljuk, hogy legalább 10 perc, mielőtt elkezdené az például 2 részét várja meg, hogy az eredmények erősen különböznek az eredményül kapott telemetriai adatokat a.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Ez a példa minden részét megpróbál futtatni egy paraméteres utasítást 1000 időpontok (a megfelelő betöltés használata próba adatkészletben egyesíthetők létrehozásához). Azt végrehajtja a tárolt eljárások, a lekérdezés processzor megvizsgálja az eljárás (paraméter "elemzés") az első fordításkor átadott paraméter. A processzor gyorsítótárát az eredményül kapott tervet, és újabb meghívásához, még akkor is, ha a paraméter értéke különböző használja. Minden olyan esetben lehet, hogy nem használható a optimális terv. Időnként szükség útmutató a optimalizáló és egy másik csomagra jobb, ha az adott esetben, hanem az átlagos esetben az, hogy mikor lefordított először a lekérdezés kiválasztása. Ebben a példában a kezdeti tervet létrehoz egy "kép" tervet, amely beolvassa az összes sort, amely a paraméter megfelel az egyes értékek keresése:

![A lekérdezés által beolvasott képet-csomagot használ finombeállítása](./media/sql-database-performance-guidance/query_tuning_1.png)

Az eljárás hajtja használja az 1 értéket, mivel az eredményül kapott terv optimális az 1 értéket, de az a tábla minden más érték optimálisnál volt. Az eredmény valószínűséggel nem megfelelő módon véletlenszerűen, válassza a minden terv, mivel a terv lassabban hajt végre, és további az erőforrásokat használó mintha.

Ha a teszt a `SET STATISTICS IO` beállítása `ON`, a logikai Keresés ebben a példában a munkát a színfalak mögött. Láthatja, hogy nincsenek-e a terv (ez nem hatékony, ha az átlagos esetben csak egy sor visszatérési) által elvégzett 1,148 Olvasás:

![A lekérdezés beállítása egy logikai keresés használatával](./media/sql-database-performance-guidance/query_tuning_2.png)

A második rész a példában a lekérdezés emlékeztető megállapítani, hogy az adott érték a fordítás során használandó optimalizáló használja. Ebben az esetben azt kezd a lekérdezés processzor figyelmen kívül hagyja a az érték, amely paraméterként van, és helyette Átvéve `UNKNOWN`. Ez egy érték hivatkozik, amelynek az átlagos gyakoriság (figyelmen kívül hagyása ferdeség), a táblázatban Az eredményül kapott csomagja egy seek-alapú másik csomagra gyorsabb, és kevesebb erőforrást, ebben a példában 1 részén átlagosan, mint a csomagot használ:

![Lekérdezés beállítása egy lekérdezés tipp segítségével](./media/sql-database-performance-guidance/query_tuning_3.png)

Megjelenik a **sys.resource_stats** táblázatban az effektus (nincs a időpontot, a vizsgálat, és amikor a az adatokat a táblázat feltölti végrehajtása késleltetést). Ez például rész 1, a 22:25:00 időkeret során futtatni, és rész 2 22:35:00 hajtja végre. Figyelje meg, hogy a korábbi időkeret további források használja, hogy időkeret, mint a későbbi (Ennek oka terv hatékonyságának javítása).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![A lekérdezés mintaeredmény finombeállítása](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Bár ez a példa a mennyiségi szándékosan kis, optimálisnál paraméterek hatása lehet jelentős, különösen a összeállítására. A különbség a rendkívüli esetekben lehet gyors esetek másodpercig és a munkaidőt lassú az esetek között.

Ellenőrizheti, hogy **sys.resource_stats** határozza meg, hogy az erőforrás vizsgálat használja-e egy másik próba-nél több vagy kevesebb erőforrásokat. Ha az adatok összehasonlításához, külön a időzítését: azt vizsgálja, hogy még nem ugyanabban az 5 perc ablakban a **sys.resource_stats** nézetben a. A gyakorlatban célja használt erőforrások mennyiségét minimalizálásához, nem pedig a csúcs erőforrások összezárása. Általánosságban elmondható késés kódját valamilyen optimalizálása is csökkenti erőforrás-felhasználás. Győződjön meg arról, hogy szükség-e az alkalmazás a módosításokat, és, hogy a módosításokat nem negatív hatással mások által esetleg használt lekérdezési javaslatok az alkalmazás a felhasználói élmény.

Ha egy terhelést tartozik egy ismétlődő lekérdezéseket, gyakran van ilyesmire lehetőség rögzítheti és érvényesíteni a terv választási lehetőségek, a optimalizálási, mert azt meghajtók az adatbázis tárolásához szükséges minimális erőforrás méret mértékegységet. Miután érvényesítés, időnként meg újra a segít győződjön meg arról, hogy azok nem rendelkezik-e csökkent a tervek. További információ a [lekérdezési javaslatok (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms181714.aspx)talál.

### <a name="cross-database-sharding"></a>Adatbázis-határokon sharding
Azure SQL-adatbázis örvend hardver fut, mert a kapacitás korlátozások egy adatbázis kisebb, mint a hagyományos helyszíni SQL Server történő telepítés esetén is. Egyes ügyfelek sharding módszerek segítségével adatbázis-műveletek elosztva több adatbázisok, amikor a művelet nem férnek el egy adatbázis Azure SQL-adatbázisban keretén belül. A legtöbb használók sharding technikákat Azure SQL-adatbázisban a egyetlen dimenzióhoz adataik keresztül több adatbázis felosztása Ezt a megközelítést frissítenie kell, hogy a OLTP alkalmazások gyakran hajtsa végre az egyetlen sorból vagy a sorok a sémában kisebb csoport vonatkozó tranzakciók megértéséhez.

>[AZURE.NOTE] SQL-adatbázis most nyújt segítséget sharding a tár. További információ a [Rugalmas adatbázis ügyfél tár áttekintése](sql-database-elastic-database-client-library.md)című témakörben találhat.

Például ha egy adatbázis ügyfél nevét, a sorrend és a rendelés részletei (például a hagyományos példa Northwind adatbázist részét képező SQL Server-), is feloszthatja ezeket az adatokat több adatbázis egy ügyfél, a kapcsolódó sorrend és a rendelés részletei adatainak csoportosításával. Akkor is garantálja, hogy az ügyfél adatait egyetlen adatbázisban maradjon. Az alkalmazás volna felosztása különböző vevők hatékony terjesztése a betöltés keresztül több adatbázis-adatbázisok között Sharding, az ügyfelek nem csak zárni az adatbázis maximális méretkorlátot, de Azure SQL-adatbázis is dolgozhat, amelyek jelentősen nagyobb, mint a másik teljesítményszint határain mindaddig, amíg minden egyes adatbázis illeszkedik a DTU munkaterhelésekből.

Bár az adatbázis sharding nem csökkentheti a összesítő erőforráshoz kapacitás megoldás, is hatékonyan lehet, hogy vannak-e több adatbázis elosztva nagyon nagy megoldások támogatása. Egy másik teljesítményszint a támogatási igen nagy, a nagy erőforrásigények "hatékony" adatbázisok adatbázisonként futtatható.

### <a name="functional-partitioning"></a>Funkcionális szétválasztás
Az SQL Server-felhasználók gyakran összevonhatja egyetlen adatbázisban számos függvény. Például az alkalmazások kezelheti a készletet az üzletek logika van, az adatbázis nyomon követése a vételi megrendelések, tárolt eljárások és hó végi jelentéskészítés kezelése indexelt vagy materializált nézeteknek készlethez kapcsolódó logika esetleg. Ez a módszer megkönnyíti az adatbázis-műveletek, például a biztonsági másolat felügyelete, de is igényel, hogy a méretezés a hardver kezelheti a csúcs betöltés végig az alkalmazás minden funkcióját.

A méretezési architektúrája Azure SQL-adatbázis használata esetén célszerű fel szeretne osztani egy másik adatbázisba alkalmazás különböző függvények. Ez az eljárás használatával minden alkalmazás méretezze át egymástól függetlenül. Az alkalmazások válik busier (és növeli a betöltés az adatbázisban), a rendszergazda választhatja ki minden függvényhez a független teljesítményszint az alkalmazás. A korlátot, e architektúra alkalmazás lehet nagyobb, mint egy örvend egyetlen számítógépre képes kezelni, mert a betöltés több számítógépen van-e elosztva.

### <a name="batch-queries"></a>Köteg lekérdezések
Nagy mennyiségű adat elérhessék-alkalmazásokhoz gyakori alkalmi lekérdezés, válaszidő jelentős mértékű van töltött hálózati forgalmat a alkalmazás réteg és a Azure SQL-adatbázis réteg között. Akkor is, ha az alkalmazás és a Azure SQL-adatbázissal az azonos adatközpontban, a hálózati késés a kettő közötti lehet kell nagyítva által nagyszámú adat access műveletek. A hálózat csökkentése ciklikus utazás közben az adatokat az access műveletekhez, fontolja meg inkább a beállítást a alkalmi lekérdezések vagy köteg, illetve a tárolt eljárásokat állíthat össze. Az alkalmi lekérdezések köteg akkor, ha Ön elküldheti több lekérdezés egy nagy köteg egyetlen utazás az Azure SQL-adatbázishoz. A tárolt eljárás alkalmi lekérdezések lefordításához, mintha azokat meg köteg érhető el is ugyanazt az eredményt. A tárolt eljárás használatával is biztosít az esélye annak, hogy a lekérdezés tervek Azure SQL-adatbázisban gyorsítótárazás, így újból felhasználhatja a tárolt eljárás, hogy növekvő előnyeit.

Egyes alkalmazások írási igényű. Előfordul, hogy a teljes I/O terheltsége adatbázis csökkentheti írások közös köteg annak figyelembe vételével. Ez általában egyszerűen, tárolt eljárások és alkalmi kötegekben automatikus-jóváhagyás tranzakcióinak helyett explicit tranzakciók használatával. Használható különféle technikákat, amelyekkel értékelése olvassa el a [Azure SQL-adatbázis alkalmazások technikák Batching](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx)című témakört. Kísérletezés a saját terhelést kötegelés a megfelelő modell található. Győződjön meg arról, ha meg szeretné érteni, hogy a modell némileg eltérő egységessége garanciákkal előfordulhat, hogy rendelkezik-e. Egységesebb és a teljesítmény kompromisszumok megfelelő kombinációja keresése találja a megfelelő terhelést erőforrás-használat a lehető legkevesebb szükséges.

### <a name="application-tier-caching"></a>Alkalmazás szintű gyorsítótárazás
Néhány adatbázis-alkalmazások olvasható-nehéz munkaterhelésekből van. Rétegek gyorsítótárazás csökkentheti a betöltés az adatbázishoz, és előfordulhat, hogy lecsökkentheti az adatbázis használatával Azure SQL-adatbázis szükséges teljesítményszint. [Azure vgx.dll gyorsítótár](https://azure.microsoft.com/services/cache/)-, az olvasás-nehéz terhelési, ha erről az adatok egyszer (vagy talán minden alkalmazás szintű a számítógépen, attól függően, hogy miként van konfigurálva egyszer), majd tárolja az SQL-adatbázis kívüli adatokat. Ez az adatbázis-terhelés (Processzor és olvasási I/O) csökkentheti, de nincs hatással lévő egységessége, mivel előfordulhat, hogy az adatokat olvas be a gyorsítótár nincs szinkronban a az adatbázisban tárolt adatokat. Sok alkalmazásban néhány ellentmondás szintjét elfogadható, bár ez nem minden munkaterhelésekből igaz. Alkalmazás követelményeket teljesen ismerje meg az alkalmazás szintű gyorsítótárazási stratégia végrehajtása előtt.

## <a name="next-steps"></a>Következő lépések

- Szolgáltatási rétegek kapcsolatos további tudnivalókért lásd: az [SQL-adatbázis beállítások és a teljesítmény](sql-database-service-tiers.md)
- Rugalmas adatbázis készletek kapcsolatos további tudnivalókért lásd: [Mi az, hogy az Azure-adatbázis rugalmas készlet?](sql-database-elastic-pool.md)
- A teljesítmény és rugalmas adatbázis készletek információt a cikkből megtudhatja, [egy rugalmas adatbázis készlet szempontok](sql-database-elastic-pool-guidance.md)
