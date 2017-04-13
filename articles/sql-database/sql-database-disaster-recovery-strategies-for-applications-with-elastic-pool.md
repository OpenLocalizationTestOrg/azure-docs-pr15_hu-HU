<properties 
   pageTitle="Felhőalapú megoldások tervezése használata SQL-adatbázis Geo-replikáció vészhelyreállítás |} Microsoft Azure"
   description="Megtudhatja, hogy miként vészhelyreállítás a felhőben megoldás tervezése a megfelelő feladatátvevő minta kiválasztásával."
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
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Vészhelyreállítási stratégiák meghatározása az SQL-adatbázis rugalmas készlet használó alkalmazásai 

Azt a tanultakhoz rendelkezik, győződjön meg arról, hogy felhőszolgáltatások nem biztos és katasztrofális események állnak az évek során is, valamint történik. SQL-adatbázis többféle funkciókat nyújt az alábbi események végrehajtásakor az üzleti folytonosságot az alkalmazás. [Rugalmas készletek](sql-database-elastic-pool.md) és önálló adatbázisok katasztrófa helyreállítási lehetőségeket azonos típusú támogatja. Ez a cikk ismerteti a több DR stratégiák, a elasztikus-készletek kihasználhatja az SQL-adatbázis üzleti folytonosságot funkciókról.

Ez a cikk alkalmazásában fogjuk használni a kanonikus szoftver külső alkalmazás minta:

<i>Modern felhő alapján a webes alkalmazás rendelkezések egy SQL-adatbázis minden felhasználó számára. A külső felhasználók nagyszámú rendelkezik, és ezért használja az sok adatbázis neve bérlői adatbázisok. Mivel a bérlői adatbázisok általában előre tevékenység mintázatok, a külső egy rugalmas készlet, hogy az adatbázis költség nagyon kiszámíthatóan hosszabb idő alatt használja. A rugalmas készlet is egyszerűbbé teszi a teljesítmény kezelését, amikor a felhasználó tevékenység napra. A bérlői adatbázisok mellett az alkalmazás is használja több adatbázis biztonsági felhasználói profilok kezelése, összegyűjtése szokásai stb. Az egyes bérlők elérhetősége nincs hatással a teljes az alkalmazás elérhetőségét. Azonban az elérhetőségéről és az adatbázisok kezelése teljesítménye fontos az alkalmazás függvény, és kapcsolat nélküli adatbázisok kezelése esetén a teljes alkalmazás offline állapotban.</i>  

A papír a többi fog témákat ölelik fel a DR stratégiák kiterjedő esetek az üzenetekhez szigorú elérhetősége követelményeknek költség bizalmas indítási alkalmazásokból cellatartományban.  

## <a name="scenario-1-cost-sensitive-startup"></a>Eset: 1. Bizalmas indítási költség

<i>Indítási üzleti vagyok, és vagyok rendkívül költség bizalmas.  Telepítési és az alkalmazás kezelésének egyszerűsítése érdekében szeretném, és telepítve van egy korlátozott szolgáltatásiszint-szerződés egyes ügyfelek hajlandó vagyok. De szeretnék, a teljes soha nem offline állapotban, győződjön meg arról, hogy az alkalmazás.</i>

Egyszerűség kötelező kielégítéséhez kell üzembe adatbázisokra bérlői egy tetszés szerinti Azure régióban rugalmas készletbe, és mint geo replikált önálló adatbázis(ok) management adatbázis(ok) üzembe. Bérlők vészhelyreállítás használja a geo visszaállítása, amelyek külön költség nélkül megtalálható. Az adatkezelési adatbázisok elérhetősége érdekében legyen a geo replikált egy másik területére (lépés: 1). A katasztrófa helyreállítási konfiguráció ebben az esetben a folyamatban lévő költség megegyezik a másodlagos adatbázis(ok) teljes költségét. Ebben a konfigurációban a következő diagramon van szemlélteti.

![Ábra 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Abban az esetben, ha az elsődleges régióban üzemszünetek a lépések ahhoz, hogy az alkalmazás online mutatja be a következő ábra szerint.

- Azonnal feladatátvevő kezelése DR régió adatbázisok (2). 
- Módosítása a kapcsolati karakterláncot az alkalmazás mutasson a DR területhez tartozik. Az összes új számlákat és a bérlői adatbázisok létrejön a DR térség. A meglévő ügyfelek jelenik meg az adataikhoz átmenetileg nem érhető el.
- A rugalmas készlet létrehozása ugyanazt a konfigurációt, mint az eredeti készlet (3). 
- Geo-visszaállítási hozhat létre a bérlői webhelyemen másolatait, adatbázisok (4). Fontolja meg, az egyes visszaállítása el a végfelhasználói kapcsolatok, vagy néhány más alkalmazás adott prioritás séma használata.

Ezen a ponton a alkalmazás vissza online a DR térség, de egyes ügyfelek az adatok elérésekor fog tapasztalni késleltetés.

![Ábra 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Ha a üzemszünetek ideiglenes volt, akkor lehet, hogy az elsődleges régió fog helyreállítani Azure előtt a DR tartományban lévő összes visszaállítása teljes. Ebben az esetben célszerű téve, az alkalmazás az elsődleges régió áthelyezése. A folyamat lépéseket az a következő diagramon ábrázolt.
 
- Az összes nyitott geo-visszaállítási kérelmek visszavonása.   
- Az elsődleges régióra (5) management adatbázis(ok) feladatátvétel. Megjegyzés: A régió helyreállítás régi eredményezi automatikusan váltak formátumú másodlagos zónák. Most már azok kapcsol szerepkörök újra. 
- Módosítása a kapcsolati karakterláncot az alkalmazás mutasson az elsődleges régió. Most már minden új partnerek és a bérlői adatbázisok létrejön az elsődleges területen. Néhány meglévő ügyfelek jelenik meg az adataikhoz átmenetileg nem érhető el.   
- Minden adatbázisban beállíthatja az DR készletben annak érdekében, hogy nem lehet módosítani a DR régióban (6) csak olvasható. 
- Megváltozott a helyreállítás óta DR készletben adatbázisonként átnevezése vagy törlése a megfelelő adatbázisok az elsődleges készletben (7). 
- Másolja a vágólapra a frissített adatbázisokat a DR készletből a elsődleges kvótáját (8). 
- Törölje a DR készlet (9)

Ezen a ponton a alkalmazás lesz online adatbázisokkal összes bérlői webhelyen elérhető az elsődleges készletben elsődleges régióban.

![Ábra 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

A fő **előnyös** e stratégia az adatok réteg redundancia alacsony folyamatban lévő költségét. Biztonsági mentést a program automatikusan veszi SQL-adatbázis szolgáltatás nincs alkalmazás átírása és külön költség nélkül.  A költség csak akkor, amikor a rugalmas adatbázisok szolgáltatáscsomagot merül fel. A **csökkentés** , hogy a teljes helyreállítás az összes bérlői adatbázisok jelentős időt vesz igénybe. Az összes függ visszaállítása a DR térség fog kezdeményez száma és a bérlői adatbázisok teljes méret. Akkor is, ha mások fölé a néhány bérlők visszaállítása projektekkel gyorskorcsolyázásban az összes a többi visszaállítása a szolgáltatás tulajdonjogáért, és azt a meglévő ügyfelek adatbázisok általános hatása minimalizálásához szabályozása ugyanabban a régióban kezdeményezett. Ezenkívül a helyreállítás a bérlői adatbázisok nem indítható mindaddig, amíg az új rugalmas készlet a DR térség jön létre.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Eset: 2. Többszintű szolgáltatással elért alkalmazás 

<i>A többszintű szolgáltatás ajánlatok és más SLA próba ügyfelek és vevők kifizetéséhez elért szoftver-alkalmazások vagyok. A próbaidőszak ügyfeleknek kell a lehető költségek csökkentése. Próbaidőszakos ügyfelek is igénybe vehet az állásidőt, de szeretnék annak valószínűségét csökkentése. A kifizető ügyfeleknek bármely legrövidebb leállás nézetbeli kockázat. Ezért el szeretném, hogy biztosan, hogy fizetésre leírtakat mindig hozzáférhetnek az adataikhoz.</i> 

A támogatási ebben az esetben, meg kell elválasztja a próbaverzió bérlők fizetett bérlők be külön rugalmas készletek helyezésével. A próbaidőszak ügyfelek szeretné, hogy egy bérlői és helyreállítási hosszabb idő az alsó SLA alsó eDTU. A fizető vevők lenne a bérlői webhelyen és egy újabb SLA egy újabb eDTU erőforráskészlethez tartozik. A legalacsonyabb helyreállítási idő biztosítása, a kifizető ügyfelek bérlői adatbázisok kell geo replikált. Ebben a konfigurációban a következő diagramon van szemlélteti. 

![A 4-es szám](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Az első eset, ahogy az adatkezelési adatbázis(ok) igazán aktív lesz így használni egy különálló geo replikált adatbázist, (1). Ezzel biztosíthatja az új ügyfél előfizetések, profil frissítések és más adatkezelési műveletek kiszámíthatóan teljesítményét. A régió, amelyben az elsődleges, a kezelés adatbázistípus(ok) található lesz az elsődleges régió, és a régió, a kezelés adatbázistípus(ok) a formátumú másodlagos zónák találhatók a DR területhez tartozik.

A fizető vevők bérlői adatbázisok elsődleges régióban kiépítve "fizetett" készletben fog rendelkezni az aktív adatbázisok. A másodlagos készletébe a DR térség azonos nevű kell rendelkezni. Minden egyes bérlői lenne geo replikált a másodlagos kvótáját (2). A gyors helyreállítás feladatátvevő segítségével összes bérlői adatbázisok ezzel engedélyezi. 

Egy üzemszünetek az elsődleges régió fordul elő, ha a helyreállítási lépéseket ahhoz, hogy az alkalmazás online a következő diagram mutatják be:

![Ábra 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Azonnal átveszi az adatkezelési adatbázis(ok) DR régió (3).
- Az alkalmazás kapcsolati karakterláncot, mutasson a DR régió módosítása Most már minden új partnerek és a bérlői adatbázisok létrejön a DR térség. A meglévő próba vevők jelenik meg az adataikhoz átmenetileg nem érhető el.
- Feladatátvevő a kifizetett bérlő adatbázisok azonnal visszaállítása (4) elérhetőségének DR régióban készletébe. Mivel a feladatátvételi rövid metaadatok szintű módosítás akkor fontolja meg egy optimalizálás, ahol az egyéni feladatátadás igény által kezdeményezett a végfelhasználói kapcsolatok. 
- Ha a másodlagos készlet eDTU mérete kisebb, mint az elsődleges volt, mert a másodlagos adatbázisok elegendő kapacitása ahhoz, hogy a változás naplók feldolgozása, miközben a formátumú másodlagos zónák voltak, azonnal növelni kell a készlet kapacitás most az összes bérlők (5) a teljes terhelést igazodik. 
- Az új rugalmas készlet létrehozása ugyanazt a nevet és ugyanazt a konfigurációt a DR régióban a próbaverzió ügyfelek adatbázisok (6). 
- A próbaidőszak ügyfelek készlet létrehozása után a geo-visszaállítási használatával állítsa vissza az egyes próbaidőszakos bérlői webhely adatbázisok be az új készlet (7). Fontolja meg, az egyes visszaállítása el a végfelhasználói kapcsolatok, vagy néhány más alkalmazás adott prioritás séma használata.

Ezen a ponton a alkalmazás vissza online állapotban a DR térség. Az összes kifizető ügyfelek hozzáférést adataikat, miközben a próbaverzió ügyfelek fog tapasztalni késleltetés, az adatok elérésekor.

Ha az elsődleges régió van helyreállított Azure *után* szerint az alkalmazás az adott régióban az alkalmazást futtató továbbra is a DR régióban visszaállítását, vagy beállíthatja úgy, hogy az elsődleges régió vissza nem. Ha az elsődleges régió helyreállított *előtt* a feladatátvevő befejeződött, vegye figyelembe visszavétele azonnal. A visszaállás lépéseket a következő ábra szemlélteti: 
 
![Ábra 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Az összes nyitott geo-visszaállítási kérelmek visszavonása.   
- Feladatátvétel management adatbázis(ok) (8). A régió helyreállítás a régi elsődleges automatikusan a másodlagos vált. Most már az lesz az elsődleges újra.  
- A kifizetett bérlő feladatátvevő adatbázisok (9). Hasonlóképpen a régió helyreállítás régi eredményezi automatikusan válik a formátumú másodlagos zónák. Most már ismét válnak eredményezi. 
- Állítsa a visszaállított próba adatbázisok, amelyek módosultak a DR térség csak olvasható (10).
- Óta a helyreállítás próba ügyfelek DR készletben adatbázisonként átnevezése vagy törlése a megfelelő adatbázist a próba-ügyfelek elsődleges készletben (11). 
- Másolja a vágólapra a frissített adatbázisokat a DR készletből a elsődleges kvótáját (12). 
- A DR készlet (13) törlése 

> [AZURE.NOTE] A feladatátvevő művelet nem aszinkron. A helyreállítási idő csökkentése érdekében fontos, hogy legalább 20 adatbázisok kötegekben hajtsa végre a bérlői adatbázisok feladatátvevő parancsot. 

A fő **előnyös** e stratégia, hogy a legnagyobb SLA kifizető az ügyfeleknek biztosít. Is garantálja, hogy az új kísérletek blokkolatlan-e, amint a próbaverzió DR készlet jön létre. A **csökkentés** , hogy ez a beállítás növelik-e a bérlői adatbázisok teljes költségét a költség a másodlagos DR készlet fizetett ügyfeleknek. Ezenkívül a másodlagos készlet van egy másik méretét, ha a kifizető ügyfelek fog tapasztalni alsó teljesítmény feladatátvétel után az erőforráskészlet frissítése a DR térség befejeztéig. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Eset: 3. Földrajzilag elosztott alkalmazás többszintű szolgáltatással

<i>Van egy többszintű szolgáltatás ajánlatok elért szoftver alkalmazást. Szeretném ajánlja fel a nagyon olyan szigorú szolgáltatásiszint-szerződés a kifizetett ügyfelek és kockázatminimalizálás ütközési amikor kimaradások oka, hogy még rövid megszakítás okozhatják ügyfél kapcsolatos. Rendkívül fontos, hogy a kifizető ügyfelek mindig is hozzáférhetnek az adataikhoz. A kísérletek ingyenes, és egy SLA nem ajánlja fel a próbaidőszak alatt.</i> 

Ebben az esetben támogatja, rendelkeznie kell három külön rugalmas készletek. Két egyenlő méretű készletek a magas eDTUs adatbázisonként tartalmaz a kifizetett ügyfelek bérlői adatbázisok két különböző régiókban kell kell építenie. A harmadik készletet, a próbaverzió bérlők tartalmazó szeretné, hogy egy alsó eDTUs adatbázisonként, és a két régió egyikében kell építenie.

A legalacsonyabb helyreállítási idő alatt kimaradások fizető vevők bérlői webhely zökkenőmentes adatbázisok kell lennie, az elsődleges adatbázisok a két régióban 50 %-os geo replikált. Hasonlóképpen az egyes régiókra kellene a másodlagos adatbázisok 50 %-át. Ezzel a módszerrel Ha terület offline állapotban a kifizetett ügyfelek adatbázisok csak 50 %-os volna hatással lehet, és szeretné átadni, hogy. A többi adatbázisok volna változatlanok maradnak. Ez a beállítás van alábbi ábra szemlélteti:

![A 4-es szám](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Az előző jelenik meg, ahogy az adatkezelési adatbázis(ok) igazán aktív lesz, konfigurálja a őket, különálló geo replikált adatbázis(ok) (1). Ezzel biztosíthatja az új ügyfél előfizetések, a profil frissítéseket és a más adatkezelési műveletek kiszámíthatóan teljesítményét. A régió lenne management adatbázis(ok) elsődleges régió, és a régió B fogja használni a kezelés adatbázistípus(ok) helyreállítási.

A fizető vevők bérlői adatbázisok lesz is geo replikált, de elsődleges és formátumú másodlagos zónák régió a és B (2) terület elosztva. Ezzel a módszerrel a bérlői elsődleges adatbázisokat a üzemszünetek hatással a többi régió áttérni is, és lesznek elérhetők. A bérlői adatbázisok más felében egyáltalán nem lesz hatással. 

A következő ábra bemutatja a helyreállítási lépéseket kell elvégezni, ha egy üzemszünetek fordul elő, az régió válaszok parancsra.

![Ábra 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Azonnal átveszi az adatkezelési adatbázisok területére B (3).
- Az alkalmazás kapcsolati karakterláncot, mutasson a b módosítása, hogy az új fiók és a bérlői adatbázisok létrehozása területen B és a meglévő bérlői adatbázisok találhatók van, valamint az adatkezelési adatbázis(ok) tartományban lévő management adatbázis(ok) módosítása. A meglévő próba vevők jelenik meg az adataikhoz átmenetileg nem érhető el.
- Feladatátvevő a kifizetett bérlő adatbázisok kvótáját régióban B azonnal visszaállítása (4) elérhetőségének 2. Mivel a feladatátvételi rövid metaadatok szintű módosítás akkor fontolja meg egy optimalizálás, ahol az egyéni feladatátadás igény által kezdeményezett a végfelhasználói kapcsolatok. 
- Most óta a készlet 2 csak elsődleges adatbázist, a teljes terhelést a készletben növelik, így azonnal növelni kell a eDTU méretét (5) tartalmaz. 
- Az új rugalmas készlet létrehozása ugyanazt a nevet és ugyanazt a konfigurációt, a B régióban a próbaverzió ügyfelek adatbázisok (6). 
- A készlet létrehozása után az egyes próbaidőszakos bérlői webhely adatbázis visszaállítása (7) készletbe geo-visszaállítási használatával. Fontolja meg, az egyes visszaállítása el a végfelhasználói kapcsolatok, vagy néhány más alkalmazás adott prioritás séma használata.


> [AZURE.NOTE] A feladatátvevő művelet nem aszinkron. A helyreállítási idő csökkentése érdekében fontos, hogy legalább 20 adatbázisok kötegekben hajtsa végre a bérlői adatbázisok feladatátvevő parancsot. 

Ezen a ponton régió b az alkalmazás visszatérés online üzemmódba Az összes kifizető ügyfelek hozzáférést adataikat, miközben a próbaverzió ügyfelek fog tapasztalni késleltetés, az adatok elérésekor.

Ha A régió van helyreállított is kell-e döntse el kívánja-e régió B használata próba ügyfelei vagy visszaállás való használatáról a próba-ügyfelek készlet régióban válaszok parancsra. Több feltétel lehet próbaidőszakos bérlői webhely adatbázisokat a helyreállítás óta módosított százalékát. Függetlenül attól, hogy döntés szüksége lesz a kifizetett bérlők között két készletek újra egyenleg. a következő ábra bemutatja a folyamat, ha a próbaidőszakos bérlői webhely-adatbázisok nem vissza régió válaszok parancsra.  
 
![Ábra 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Az összes nyitott geo-visszaállítási kérelmek próba DR kvótáját visszavonása.   
- Feladatátvétel az adatbázist (8). A régió helyreállítás a régi elsődleges volna automatikusan vált, a másodlagos. Most már az lesz az elsődleges újra.  
- Jelölje ki a mely fizetett bérlői adatbázisok vissza az erőforráskészlet 1 és kezdeményezése áttérni azok (9) formátumú másodlagos zónák meghiúsul. A régió helyreállítás készletben 1 adatbázisokra formátumú másodlagos zónák automatikusan vált. Most már 50 %-át őket elsődleges ismét válik. 
- Az eredeti eDTU (10)-2 készlet méretének csökkentése.
- Összes beállítása csak olvasható (11) próba adatbázisokat a régióban B vissza.
- Minden egyes készletben próba DR óta a helyreállítás megváltozott adatbázishoz átnevezése vagy törlése a megfelelő adatbázist a próbaverzió elsődleges készletben (12). 
- Másolja a vágólapra a frissített adatbázisokat a DR készletből a elsődleges kvótáját (13). 
- Törölje a DR készlet (14) 

A fő **előnyökkel jár** a stratégia a következők:

- A legtöbb olyan szigorú SLA támogat kifizető az ügyfeleknek, biztos lehet benne, hogy egy üzemszünetek nem hatással lehet a bérlői adatbázisok több, mint 50 %-át. 
- Biztosítja, hogy az új kísérletek blokkolatlan-e, amint a DR készlet pontosan a helyreállítás során létre. 
- A készlet kapacitás hatékonyabb használata lehetővé teszi, a másodlagos adatbázisokat a készlet 1 és 2 készlet 50 %-os garantált kevesebb aktív, majd az elsődleges adatbázisok lesz.

A fő **kompromisszumok** a következők:

- Adatkezelési adatbázis(ok) elleni CRUD műveletek csatlakozik a régió, mint A régió B csatlakozik, az elsődleges, a kezelés adatbázistípus(ok) elleni végrehajtandó felhasználóknak felhasználóknak fog rendelkezni az alsó késés.
- Az adatkezelési adatbázis összetettebb tervezés igényel. Például az egyes bérlői rekordok rendelkeznie kell feladatátvétel és visszaállás során módosítani kell, hogy a hely címke.  
- A fizető vevők szokottnál alsó teljesítmény merülhetnek fel a készlet frissítés régióban B befejeztéig. 

## <a name="summary"></a>Összefoglalás

Ez a cikk a Vészhelyreállítási stratégiák meghatározása a szoftver külső több bérlői alkalmazás által használt adatbázis réteg számára koncentrál. A kiválasztott stratégia alapján kell az egyéni igényeknek az alkalmazást, például a vállalati modell, a SLA szeretne ügyfeleinek kínálhat, költségvetés kényszer stb... Egyes leírt stratégia így azt teheti tájékozott döntés előnyeinek és csökkentés ismertet. Az adott alkalmazás is, valószínűleg fogja tartalmazni más Azure összetevők. Tekintse át az üzleti folytonosságot útmutatást, és a velük a adatbázis réteg visszanyerése téve. Az adatbázis-alkalmazások Azure-ban helyreállítási kezelésével kapcsolatos további információért olvassa el a [tervezése felhő megoldások vészhelyreállítás](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Következő lépések

- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
