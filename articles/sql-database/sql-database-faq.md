<properties 
   pageTitle="Azure SQL-adatbázis – gyakori kérdések" 
   description="Válaszok a gyakori kérdések ügyfelekre, és kérdezze felhő adatbázisok és Azure SQL-adatbázissal, a Microsoft relációs adatbázis-kezelő rendszer (RDBMS) és adatbázis szolgáltatásként a felhőben." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>SQL-adatbázis – gyakori kérdések

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Az SQL-adatbázis használatát hogyan nem jelenik meg a számlán? 
SQL-adatbázis számlák a kiszámíthatóan óradíj alapján a szolgáltatási réteg + teljesítményszint egyetlen adatbázisok vagy egy rugalmas adatbázis készlet eDTUs. Tényleges használatát számított és arányosított óránként, a számla előfordulhat, hogy jelenjenek törtek az óra. Például ha egy adott hónapban a 12 órával adatbázis létezik, a számla 0,5 nappal használatát jeleníti meg. Ezenkívül szolgáltatási rétegek + teljesítményszint és egy készlet eDTUs megszakadnak a a számlázási egyszerűbben lásd: az egyes egyetlen hónap használt adatbázis napok számát.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Mi történik, ha egy adatbázis aktív kisebb, mint egy órával vagy egy újabb szolgáltatási réteg kisebb, mint egy óra használja?
Vannak az egyes a legmagasabb szolgáltatási réteg létezik adatbázis óra + adott órát, függetlenül attól, kihasználtsága vagy az adatbázis volt-e az active kisebb, mint egy órával alkalmazott teljesítményszint számlát kapni. Például ha hozzon létre egy adatbázist, és 5 perccel később törölheti a számla egy adatbázis óra díjat tükrözi. 

Példák
    
- Hozzon létre egy egyszerű adatbázist, és ezután azonnal frissíteni szabványos S1, ha az előfizetést terhelő a szabványos S1 díjazott az első órát.

- Ha egy adatbázis Basicben prémium délután 10 és 1:35 szerint befejezi frissítés a következő napon meg vannak alapdíjával díjazott a prémium kezdve 1:00 de. 

- Ha az adatbázis prémium az egyszerű vissza léptetheti a 11:00 de. 2:15 délután befejezése, majd az adatbázis alapdíjával díjazott a prémium 3:00 óráig, amíg utáni amelynek azt terheli az egyszerű kamatlába mellett.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Hogyan nem rugalmas adatbázis készlet használatát jelennek meg számlájának, és mi történik, amikor egy készlet eDTUs?
Rugalmas adatbázis készlet költségeket jelennek meg a számla rugalmas DTUs (eDTUs), a használati [árak](https://azure.microsoft.com/pricing/details/sql-database/)lapon készlet eDTUs alatt lépésekben. Adatbázis-ingyenesek rugalmas adatbázis készletek nem. Vannak minden erőforráskészlethez tartozik megtalálható a legmagasabb eDTU függetlenül használatát vagy a készlet volt-e az active kisebb, mint egy órával órával számlát kapni. 

Példák

- Ha 200 eDTUs egy szabványos rugalmas adatbázis készlet létrehozását a 11:18 óra, öt adatbázisok hozzáadása a készlet, az előfizetést terhelő 200 eDTUs az egész óraként, 11 délelőtt kezdődően keresztül a nap maradéka.
- A nap 2:05 órai, a adatbázis 1 megkezdi a 50 eDTUs használata más hosszan az állandó a napjáig. 2 – 5 adatbázisok ingadozik 0 és 80 eDTUs között. A nap során felvett foglalnak le változó eDTUs egész nap öt más adatbázisokat. 2 nap egy teljes nap 200 eDTU a számlát kapni. 
- Órai, 3, napon hozzáadhat egy másik 15 adatbázisok. Adatbázis processzorhasználata rájuk a csoporttal, a pontra, ahol úgy dönt, hogy a készlet 200 – 400 eDTUs növelése 8:05 délután 200 eDTU szintre díjak voltak érvényben addig, amíg a du. 8 és a hátralévő 4 óra 400 eDTUs-ra nő. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Hogyan működik a használja az Active Geo-replikáció rugalmas adatbázis-készletben jelennek meg számlájának?
Egyetlen adatbázisoktól eltérően [Az aktív Geo replikációs](sql-database-geo-replication-overview.md) használata rugalmas adatbázisok nem közvetlen hatást számlázási.  Csak az előfizetést terhelő a eDTUs kiépítéstől az egyes a készletek (készlet elsődleges és másodlagos készlet)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Milyen hatással számlájának a naplózási szolgáltatás használatát? 
Naplózás az SQL-adatbázis szolgáltatás nincs extra a beépített költség és Basic, normál és Premium adatbázisok érhető el. Azonban a naplókat tárolhat, a naplózási funkció által használt tárterület Azure-fiók és táblák és Azure-tárolóban lévő sorok megadása alkalmazni alapján a napló méretét.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Hogyan találhatom meg a megfelelő szolgáltatási réteg és a teljesítmény szintet a egyetlen és rugalmas adatbázis készletek? 
Néhány eszközök is elérhető. 

- A helyszíni adatbázisok esetén javasoljuk az adatbázisok és a szükséges DTUs a [DTU méretező advisor](http://dtucalculator.azurewebsites.net/) használatával, és felmérése-készletek rugalmas adatbázis több adatbázishoz.
- Ha egy adatbázis előnyös a erőforráskészlethez tartozik, az Azure-féle intelligens motor ajánlja egy rugalmas adatbázis készlet, ha azt láthatja, hogy egy korábbi használatát minta, amely szükségessé teszi. Lásd: [Monitor és kezelése egy rugalmas adatbázis készlet Azure Portal](sql-database-elastic-pool-manage-portal.md). További információ arról, hogy miként műveletek automatikus elvégzéséhez, akkor olvassa el a [Rugalmas adatbázis-készlet árát és teljesítménybeli szempontok](sql-database-elastic-pool-guidance.md)
- Látható, hogy kell hívnia egy adatbázis felfelé vagy lefelé, lásd: [egyetlen adatbázisok teljesítmény útmutatást](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Milyen gyakran is egy adatbázis szolgáltatás réteg vagy a teljesítmény szintjének módosítása? 
A V12 adatbázisok módosíthatja a szolgáltatási réteg (között Basic, normál és Premium) vagy a teljesítményszint belül egy szolgáltatási réteg (például s2 S1) gyakran kívánt. A régebbi verziójú adatbázisok módosíthatja a szolgáltatási réteg vagy a teljesítmény szintet négyszer 24 órás időszakban összesen.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Milyen gyakran módosíthatja a készlet per eDTUs? 
Olyan gyakran van szüksége.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Mennyi ideig tart egy adatbázis szolgáltatás réteg vagy a teljesítmény szintjének módosítása vagy egy adatbázis áthelyezése egy rugalmas adatbázis készlet-és kijelentkezés? 
A szolgáltatás réteg adatbázis módosítása, és a készlet-és kijelentkezés áthelyezése szükséges háttér műveletként platformon másolandó az adatbázist. A szolgáltatási réteg módosítása is eltarthat néhány percig órákat az adatbázisok méretétől függően. Mindkét esetben az adatbázisok továbbra is online állapotának leolvasása az áthelyezés során. Egyetlen adatbázisok módosításával kapcsolatos részletekért olvassa el [a szolgáltatási réteg adatbázis módosítása](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Mikor érdemes használni egy adatbázist és rugalmas adatbázisok összehasonlítása? 
Az általános rugalmas adatbázis készletek készült tipikus [szoftverek,-a-szolgáltatási (szoftver) alkalmazás mintát](sql-database-design-patterns-multi-tenancy-saas-applications.md), ha az ügyfél vagy a bérlő használati egy adatbázis található. Egyes adatbázisokat vásárlása, illetve felel meg a változó és-adatbázisonként igény szerinti maximális overprovisioning gyakran nem költségének hatékony. Készletek, az Ön kezeli a készlet csoportos teljesítményének, és az adatbázisok automatikus méretezése felfelé vagy lefelé. 

Azure-féle intelligens motor javasolja-adatbázisok erőforráskészlethez tartozik, ha mintát használatát igényli azt. A részletekért olvassa [SQL-adatbázis árak réteg javaslatok](sql-database-service-tier-advisor.md). Részletes egyetlen és rugalmas adatbázisok közötti kiválasztása című témakörben olvashat [Rugalmas adatbázis készletek árát és a teljesítmény szempontjai](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Mit jelent a biztonsági másolat tárolási kiépített adatbázis maximális tárterület 200 %-os van? 
Biztonsági másolat tárolási az automatizált adatbázis biztonsági mentése az [pont-a-idő-visszaállítási használt társított tárolására](sql-database-recovery-using-backups.md#-point-in-time-restore) és [Geo-visszaállítása](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL-adatbázis biztonsági mentési tárterületét külön költség nélkül a maximális kiépített adatbázis tárolási 200 %-os biztosít. Például ha egy szabványos DB példánya kiépített DB méretű 250 GB, rendelkezésre álló 500 GB-nyi biztonsági másolat tárolási külön költség nélkül. Ha az adatbázis meghaladja a megadott biztonsági másolat tárolási, megadhatja az adatmegőrzési időszak csökkentése Azure ügyfélszolgálat megkeresésével, vagy a extra biztonságimásolat-olvasásra földrajzilag felesleges tároló (TS-GRS) díjazott számlát kapni fizetni. További információt a TS-GRS számlázás a tárhely árak részletei című részben találja.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Lehet az új szolgáltatási rétegek vagyok áthelyezése a webes/üzleti mire van szükségem tudnivalók?
Azure SQL-webhely és a vállalati adatbázis most már vannak inaktív. A Basic, normál, prémium és elasztikus rétegek cserélje le a leköszönő webes és az üzleti adatbázis. Azt is, érdemes elolvasnia a áttűnés időszak további – gyakori kérdések. [Webes és üzleti Edition sunset gyakori kérdések](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Mi az, hogy egy várható replikációs eltérés verzióazonosítójának geo adatbázis belül az azonos Azure földrajzi két területek között?  
Azt is jelenleg támogató öt másodperc egy Készletben, és a replikáció eltérés megszűnt-nél kisebb, hogy mikor helyezkedik el az Azure-ban a geo másodlagos ajánlott párosított régió és ugyanabban a szolgáltatás rétegben.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>ActiveX-vezérlőket egy várható replikációs eltérés geo-másodlagos ugyanabban a régióban az elsődleges adatbázis létrehozásakor  
Tapasztalati adatok alapján, nem áll túl sok belüli-régió és a régió közötti replikációs eltérés közötti különbség, amikor az Azure ajánlott párosított régió használják. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Ha a két régió közötti hálózati hiba az újraküldés logikája működése Geo replikációs beállításakor?  
Ha egy kapcsolat bontása, minden 10 másodperc újra létrehozni a kapcsolatok újra azt.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Mire van lehetőségem annak biztosítására, hogy az elsődleges adatbázison kritikus módosítása replikált-e?
A geo másodlagos aszinkron replikájának, és azt nem tudják teljes szinkronizálás a következővel az elsődleges tarthatja. Azonban elvégezheti a szükséges ahhoz, hogy a replikáció fontos változásairól (például a jelszó frissítések) szinkronizálná módszert. Kényszerített szinkronizálási akadályozza a hívó szál mindaddig, amíg az összes végrehajtott tranzakciók replikált hatással van a teljesítmény. A részletekért olvassa el a [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx)című témakört. 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Milyen eszközök állnak rendelkezésre a Lync-a replikáció eltérés az elsődleges adatbázis és geo-másodlagos között?
Azt jelenítik meg a valós idejű replikációs eltérés az elsődleges adatbázis és geo-másodlagos keresztül egy DMV között. A részletekért olvassa el a [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx)című témakört.
