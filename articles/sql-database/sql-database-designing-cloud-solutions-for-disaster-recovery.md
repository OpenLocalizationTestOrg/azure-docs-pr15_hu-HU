<properties
   pageTitle="A felhő katasztrófa helyreállítási megoldások – SQL adatbázis aktív Geo replikációs |} Microsoft Azure"
   description="Megtudhatja, hogy miként tervezése a felhőben katasztrófa helyreállítási megoldások üzleti folytonosságot tervezési geo replikációs alkalmazás adatok biztonsági mentése Azure SQL-adatbázis használata."
   keywords="a felhő vészhelyreállítás, a katasztrófa helyreállítási megoldások, a biztonságimásolat-készítő alkalmazás, a geo-replikáció, üzleti folytonosságot tervezése"
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Az aktív Geo replikációs használata SQL-adatbázis felhő vészhelyreállítás-alkalmazások tervezése


> [AZURE.NOTE] [Aktív Geo replikációs](sql-database-geo-replication-overview.md) adatbázisokra az összes rétegek most érhető el.



Megtudhatja, hogy miként [Aktív Geo replikációs](sql-database-geo-replication-overview.md) segítségével tervezés adatbázis-alkalmazások rugalmassá területi hibák és katasztrofális kimaradások SQL-adatbázisban. Üzleti folytonosságot tervezés, érdemes megfontolni a alkalmazás telepítési topológiát, a szolgáltatásiszint-szerződés céloz, forgalom időtartama és a költségeket. Ez a cikk azt tekintse meg a közös alkalmazás mintázatok, és előnyeinek és az egyes beállítások kompromisszumok megvitatása. Információ az aktív Geo-replikáció rugalmas készletek, című témakörből [rugalmas készlet Vészhelyreállítási stratégiák meghatározása](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Tervező mintát 1: cloud vészhelyreállítás közös található adatbázis készítésének aktív-passzív telepítésének

Ez a beállítás akkor alkalmazásokhoz a következő jellemzőkkel a legmegfelelőbb:

+ Egy egyetlen Azure tartomány aktív példány
+ Erős írási és olvasási (RW) hozzáférés az adatokhoz való függőség
+ Az alkalmazás logika és az adatbázis kapcsolatának határokon-régió nem elfogadható miatt időtartama és forgalom költség    

Ebben az esetben a telepítési Keresőalkalmazás topológiája van optimalizálva kezelése a regionális katasztrófák, ha az összes alkalmazásösszetevők hatással vannak, és szükség egy egységként segítségével. A földrajzi redundancia az alkalmazás logika és az adatbázis replikált egy másik területére, de nem használható az alkalmazás terhelést a normál feltételek alapján. Az alkalmazás, a másodlagos régióban kell beállítania, hogy egy SQL-kapcsolati karakterláncot, a másodlagos adatbázis használata. Adatforgalom manager [feladatátvevő útválasztási](../traffic-manager/traffic-manager-configure-failover-routing-method.md)-metódus használatára van beállítva.  

> [AZURE.NOTE] [Azure forgalom manager](../traffic-manager/traffic-manager-overview.md) egész Ez a cikk csak a ábra célokra használják. Bármely terheléselosztási megoldást, amely támogatja a feladatátvevő útválasztási módszert is használhatja.    

Mellett a fő szolgáltatásalkalmazás-példányok érdemes egy kis [dolgozó szerepkör alkalmazás](cloud-services-choose-me.md#tellmecs) , amely az elsődleges adatbázis figyeli periodikus az SQL-T csak olvasható (RO) parancsokat azáltal üzembe helyezése. Szeretne automatikusan elindítani a feladatátvételt, az alkalmazás felügyeleti konzolban értesítés létrehozása vagy mindkettőt felhasználhatja azt. Győződjön meg arról, hogy figyelése nincs hatással van régió szintű kimaradások, kell telepíteni az ellenőrzési szolgáltatásalkalmazás-példányok egyes régiókra, és minden csatlakozik az elsődleges az elsődleges tartomány és a másodlagos adatbázisban a helyi tartományban lévő példánya van. 

> [AZURE.NOTE] Mindkét legyen aktív, és alkalmazások figyelése elsődleges és másodlagos adatbázisok probe. Az utóbbi használható hiba feltárása a másodlagos terület és értesítés, amikor az alkalmazás nem védett.     

Az alábbi ábra mutatja az ebben a konfigurációban egy üzemszünetek előtt.

![Az SQL-adatbázis Geo-replikáció konfiguráció. A felhő vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Után egy elsődleges régióban üzemszünetek a felügyeleti alkalmazás észleli az, hogy az elsődleges adatbázist nem érhető el, értesítés és. Attól függően, hogy az alkalmazás SLA megadhatja, hogy hány egymást követő felügyeleti szondákat fejeződjön be azt az egy adatbázis üzemszünetek deklarálása előtt. Az alkalmazás végpontot, és az adatbázis összehangolt feladatátvételének eléréséhez rendelkeznie kell a felügyeleti alkalmazás, hajtsa végre az alábbi lépéseket:

1. [Az elsődleges végpontot állapotának frissítése](https://msdn.microsoft.com/library/hh758250.aspx) annak érdekében, hogy a végpontot feladatátvételt válthat.
2. Hívás kezdeményezése [adatbázis feladatátvevő](sql-database-geo-replication-portal.md)másodlagos adatbázis.

Után feladatátvételt az alkalmazás dolgozza fel a felhasználói kérések a másodlagos régióban, de mivel az elsődleges adatbázis most lesz a másodlagos régióban marad közös található adatbázis-kezelő. Ebben az esetben a következő diagram ábra mutatja. Az összes diagramon folytonos vonalak jelzik a aktív kapcsolat, pontozott vonalak jelzik a felfüggesztett kapcsolatok és a stop jelek jelzik művelet indítók.


![GEO-replikáció: Másodlagos adatbázis való áttérés. Alkalmazás adatok biztonsági másolatot.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Egy üzemszünetek a másodlagos régióban történik, ha az elsődleges és a másodlagos adatbázis között a replikáció hivatkozás fel van függesztve, és a felügyeleti alkalmazás regisztrálja, hogy az elsődleges adatbázis van téve értesítés. Az alkalmazás teljesítményének nincs hatással van, de az alkalmazás működésének elérhető, és ezért újabb kockázatára jelenik meg a két régió sikertelen egymás után.

> [AZURE.NOTE] Csak javasoljuk, hogy egyetlen DR terület telepítési konfigurációt. Ennek oka az, a legtöbb az Azure geographies két sorterület van. Ezek a konfigurációk nem lesz az alkalmazás megóvni a két régió katasztrofális betöltő. Valószínű esemény, például egy hiba visszaállíthatja az adatbázisok használatával [geo-visszaállítási művelet](sql-database-disaster-recovery.md#recovery-using-geo-restore)harmadik területen.

A üzemszünetek szünteti meg, amikor a rendszer automatikusan szinkronizálja a másodlagos adatbázis a az elsődleges. A szinkronizálás során, az elsődleges teljesítmény kissé hatással lehetnek attól függően, hogy a szinkronizálni kívánt adatok mennyiségét. A következő diagramon azt szemlélteti, hogy egy üzemszünetek a másodlagos régióban.

![Másodlagos adatbázis elsődleges szinkronizálva. A felhő vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


A tervezés mintát főbb **előnyei** vannak:

+ Az SQL-kapcsolati karakterláncot az egyes régiókra értéke az alkalmazást a telepítés során, és nem változtatja meg a feladatátvétel után.
+ Alkalmazás feladatátvevő nincs hatással van az alkalmazás teljesítményének és az adatbázis mindig közös helyezkednek el.

A fő **útján** , hogy a felesleges alkalmazás példány a másodlagos régióban csak vészhelyreállítás használatos.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Tervező mintát 2: aktív-aktív telepítésének alkalmazás terheléselosztás
Felhőalapú katasztrófa helyreállítási beállítás alkalmazásokhoz a következő jellemzőkkel a legmegfelelőbb:

+ Adatbázis magas arányát olvas írása
+ Adatbázis írási késés nincs hatással a végfelhasználói élmény  
+ Csak olvasható logika is kell választani az írási és olvasási logika különböző kapcsolati karakterlánc használatával
+ Csak olvasható logika nem függő teljesen szinkronizálva a legújabb frissítésekben adatok  

Az alkalmazások az alábbi jellemzőkkel rendelkeznek, ha több szolgáltatásalkalmazás-példányok a különböző területei között a végfelhasználói kapcsolatok terheléselosztási javíthatja a teljesítményt, és a végfelhasználói élmény. Alkalmazza a terheléselosztás, egyes régiókra kell rendelkeznie az alkalmazás aktív példánya és az írási és olvasási (RW) logika a elsődleges régióban az elsődleges adatbázishoz csatlakozik. A csak olvasható (RO) logika ugyanabban a régióban alkalmazás példányt másodlagos adatbázis kell csatlakoznia. Forgalom manager [ciklikus útválasztás](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) vagy [Teljesítmény útválasztás](../traffic-manager/traffic-manager-configure-performance-routing-method.md) használatára, [végpontot figyelése](../traffic-manager/traffic-manager-monitoring.md) engedélyezve van az egyes alkalmazás példány létre kell hozni.

Ahogy mintát #1 vegye figyelembe a hasonló felügyeleti alkalmazás telepítése. De eltérően mintát #1, a felügyeleti alkalmazás nem felelős a végpontot feladatátvételi el.

> [AZURE.NOTE] A minta egynél több másodlagos adatbázis használ, miközben a formátumú másodlagos zónák közül csak a korábban feljegyzett okokból feladatátvételi szeretné használható. A minta a másodlagos csak olvasható hozzáférésre van szüksége, mert aktív Geo replikációs van szükség.

Forgalom manager kell konfigurálni a teljesítmény elérése érdekében útválasztás irányítsa át a felhasználó-kapcsolatot az alkalmazás-példány legközelebb a felhasználó földrajzi helyét. A következő diagramon azt szemlélteti, hogy ez a beállítás előtt egy üzemszünetek.
![Nincs üzemszünetek: alkalmazás legközelebbi továbbítás teljesítményét. GEO-replikáció.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Ha egy adatbázis üzemszünetek elsődleges régióban lép fel, kezdeményez az elsődleges, a másodlagos régiók egyikéhez adatbázis feladatátvevő az elsődleges adatbázis helyének módosítása. A forgalom kezelő automatikusan a kapcsolat nélküli végpontot kizárja az útválasztási táblából, de továbbra is a továbbítás a végfelhasználói forgalmat a hátralévő online példányok. Mivel az elsődleges adatbázis most egy másik régióbeli, online összes előfordulását módosítania kell a írási és olvasási SQL kapcsolati karakterláncot az új elsődleges csatlakozni. Fontos, hogy a módosítást az adatbázis feladatátvevő indítása előtt. A csak olvasható SQL kapcsolati karakterláncot kell mindaddig változatlan azok mindig mutasson az adatbázis ugyanabban a régióban. A feladatátvevő lépések a következők:  

1. Mutasson az új elsődleges írási és olvasási SQL-kapcsolat karakterláncok módosítása.
2. A kijelölt másodlagos adatbázis hívás [kezdeményezése adatbázis](sql-database-geo-replication-portal.md)átadni sorrendben.

Az alábbi ábra szemlélteti a feladatátvétel után az új konfiguráció.
![Konfigurációs feladatátvétel után. A felhő vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Abban az esetben, ha egy, a másodlagos régiók egyikében üzemszünetek a forgalom kezelő automatikusan eltávolítja a kapcsolat nélküli végpontot az adott régióban az útválasztási táblából. A replikáció csatornát, a másodlagos adatbázishoz az adott régióban fel van függesztve. A hátralévő régiók további felhasználói forgalom juthat, ebben az esetben, mert az alkalmazás teljesítményének hatással van a üzemszünetek során. Miután a üzemszünetek szünteti meg, a másodlagos adatbázis, a probléma által sújtott régióban azonnal szinkronizálja a rendszer az az elsődleges. A szinkronizálás során, az elsődleges teljesítmény némileg hatással lehetnek attól függően, hogy a szinkronizálni kívánt adatok mennyiségét. A következő diagramon azt szemlélteti, hogy egy üzemszünetek egy, a másodlagos régiók.

![Másodlagos régióban üzemszünetek. A felhő vészhelyreállítás - Geo-replikáció.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

A tervezés mintát legfontosabb **előnye** , hogy az alkalmazás terhelést méretezheti át több formátumú másodlagos zónák a végfelhasználói optimális teljesítmény eléréséhez. Ezzel a lehetőséggel a **kompromisszumok** a következők:

+ A szolgáltatásalkalmazás-példányok és az adatbázis kapcsolatának írási és olvasási van eltérő időtartama és költség
+ Alkalmazás teljesítményének hatással van a üzemszünetek során
+ Szolgáltatásalkalmazás-példányok szükségesek az SQL-kapcsolati karakterlánc dinamikusan módosíthatja az adatbázis feladatátvétel után.  

> [AZURE.NOTE] Speciális munkaterhelésekből, például az feladatok, az üzletiintelligencia-eszközök vagy a biztonsági jelentéskészítés kiürítése egy hasonló módon használható. Általában az alábbi feladatok felhasználása jelentős adatbázis-erőforrások tehát azt ajánljuk, hogy jelöljön ki a másodlagos adatbázisok valamelyikével azok a teljesítmény szintű várható terhelését megfeleltetett.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Tervező mintát 3: aktív-passzív telepítésének adatok megőrzése  
Ez a beállítás akkor alkalmazásokhoz a következő jellemzőkkel a legmegfelelőbb:

+ Az adatvesztés áll a nagy vállalati kockázata. Az adatbázis feladatátvevő csak akkor használható utolsó lehetőségként, ha a üzemszünetek állandó.
+ Az alkalmazás egy ideig működhet "csak olvasható módban".

A minta az alkalmazás csak olvasható módban, a másodlagos adatbázishoz való csatlakozáskor vált. Az alkalmazás logika a elsődleges régióban közös található, az elsődleges adatbázissal, és olvasásra és írásra (RW) is működik. Az alkalmazás logika a másodlagos régióban közös található, a másodlagos adatbázissal, és csak olvasható módban (RO) készen áll.  Adatforgalom manager [feladatátvevő útválasztás](../traffic-manager/traffic-manager-configure-failover-routing-method.md) használatára, [végpontot figyelése](../traffic-manager/traffic-manager-monitoring.md) mindkét szolgáltatásalkalmazás-példányok engedélyezve létre kell hozni.

A következő diagramon azt szemlélteti, hogy ez a beállítás előtt egy üzemszünetek.
![Aktív-passzív telepítési átadása előtt. A felhő vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

A forgalom kezelő elsődleges régió kapcsolódási hibát észlel, amikor vált a megfelelő felhasználói forgalom automatikusan az alkalmazás-példányt, a másodlagos régióban. A mintázattal fontos, hogy **nem** kezdeményezzen adatbázis feladatátvevő után a üzemszünetek lép fel. Az alkalmazás, a másodlagos régióban aktiválva van, és csak olvasható módban, a másodlagos webes adatbázis működik. Ezt az alábbi ábra mutatja.

![Üzemszünetek: Alkalmazás írásvédett módban van. A felhő vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Az elsődleges régióban üzemszünetek szünteti meg, miután forgalom manager észleli a kapcsolat a visszaállítás elsődleges régióban, és a felhasználói forgalom vált vissza az alkalmazás-példányt, az elsődleges területen. Alkalmazás példányhoz megjelenni, és az elsődleges adatbázist írási és olvasási módban működik.

> [AZURE.NOTE] Mivel a minta a másodlagos csak olvasható hozzáférésre van szüksége a aktív Geo replikációs van szükség.

Abban az esetben, ha egy, a másodlagos régióban üzemszünetek a forgalom kezelő megjelöli a alkalmazás végpontot, az elsődleges régióban teljesítménye csökkent, és a replikáció csatorna fel van függesztve. A üzemszünetek azonban nincs hatással a üzemszünetek során az alkalmazás teljesítményének. Miután a üzemszünetek szünteti meg, a másodlagos adatbázis azonnal szinkronizálja a rendszer az elsődleges. A szinkronizálás során, az elsődleges teljesítmény némileg hatással lehetnek attól függően, hogy a szinkronizálni kívánt adatok mennyiségét.

![Üzemszünetek: Másodlagos adatbázis. A felhő vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

A tervezés mintát számos **előnye**van:

+ Az adatok elvesztését, elkerülhetők a az ideiglenes kimaradások során.
+ Nem kell hozzá, hogy a felügyeleti alkalmazások telepítése, mint forgalom manager váltja ki a helyreállítás.
+ Legrövidebb leállás attól függ, hogy milyen gyorsan a forgalom manager észleli a csatlakozási hiba állítható be a csak.

**Kompromisszumok** a következők:

+ Alkalmazás képes a csak olvasható módban kell lennie.
+ Szükség van dinamikusan tér át, csak olvasható-adatbázishoz csatlakoztatva van.
+ Az írási és olvasási műveletek újbóli helyreállítási az elsődleges régió, ami azt jelenti, hogy teljes adatokhoz való hozzáférés letilthatja az óra, vagy akár nap szükséges.

> [AZURE.NOTE] Abban az esetben, ha egy állandó szolgáltatás üzemszünetek régióban kézzel kell adatbázis feladatátvevő aktiválása és fogadja el az adatok elvesztését. Az alkalmazás írási és olvasási hozzáférést az adatbázishoz a másodlagos régióban fog működni.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Üzleti folytonosságot tervezés: az alkalmazás Tervezés fülre, az felhő vészhelyreállítás

Az adott felhő katasztrófa helyreállítási stratégia is kombinálásával vagy kiterjesztése a következő tervezés mintázatok az alkalmazás az igényeinek leginkább megfelelő.  A korábban említett szeretne az ügyfelekkel és a telepítés Keresőalkalmazás topológiája szolgáltatásiszint-szerződés a kiválasztott stratégia alapul. Útmutató az Ön döntése érdekében az alábbi táblázat a választási lehetőségek, a becsült adatvesztés alapján hasonlítja össze vagy helyreállítási mutasson a cél (Készletben), és a becsült helyreállítási idő (Beszúrása).

| Minta | KÉSZLETBEN | BESZÚRÁSA
| :--- |:--- | :---
| Aktív-passzív telepítésének vészhelyreállítás közös található adatbázist access programmal | Olvasási és írási hozzáférés < 5 (mp) | Hiba észlelése időpontja, a feladatátvevő API-hívás + tesztelése alkalmazás ellenőrzése
| Aktív – aktív telepítésének alkalmazás terheléselosztás | Olvasási és írási hozzáférés < 5 (mp) | Hiba észlelése idő + feladatátvevő API-hívás + SQL-kapcsolati karakterlánc módosítása + tesztelése alkalmazás ellenőrzése
| Aktív-passzív telepítésének adatok megőrzése | Ha csak olvasási hozzáférést < 5 sec írási és olvasási hozzáférést = nulla | Ha csak olvasási hozzáférést = kapcsolódási hiba észlelése idő + alkalmazás ellenőrzése <br>Olvasási és írási hozzáférés = idő csökkentése érdekében a üzemszünetek

## <a name="next-steps"></a>Következő lépések

- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
- Információ az aktív Geo-replikáció rugalmas készletek, című témakörből [rugalmas készlet Vészhelyreállítási stratégiák meghatározása](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
