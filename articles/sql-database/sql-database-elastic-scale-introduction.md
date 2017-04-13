<properties
    pageTitle="Az Azure SQL-adatbázis méretezési out |} Microsoft Azure"
    description="A Service (szoftver) fejlesztők szoftver egyszerűen készíthet rugalmas, méretezhető adatbázisok a felhőben, ezek az eszközök használatával"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Méretezés kifelé az Azure SQL-adatbázis

Azure SQL-adatbázisait a **Rugalmas** Adatbáziseszközök egyszerűen méretezése. Az eszközök és szolgáltatások segítségükkel **Azure SQL-adatbázis** gyakorlatilag korlátlan adatbázis forrásai létrehozása megoldások tranzakció alapú munkaterhelésekből és különösen a szoftvert, szolgáltatásalkalmazások (szoftver). Rugalmas adatbázis-szolgáltatásokra van tevődik össze a következőket:

* [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md): A ügyfél tárban lehetővé teszi, hogy hozhat létre és sharded adatbázisok kezelése.  [Első lépések rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md)című témakör tartalmaz.
* [Rugalmas adatbázis felosztása és egyesítése eszköz](sql-database-elastic-scale-overview-split-and-merge.md): adatok áthelyezése sharded adatbázisok között. Ez akkor hasznos, az adatok áthelyezése a több elem bérlői adatbázis egyetlen-bérlői adatbázis (vagy fordítva). Lásd: [Rugalmas adatbázis felosztása és egyesítése eszköz oktatóprogram](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Rugalmas adatbázis feladatok](sql-database-elastic-jobs-overview.md) (előzetes verzió): Azure SQL-adatbázisait nagyszámú kezelése a feladatok használatával. Egyszerűen hajtanak végre felügyeleti például séma módosításai, hitelesítő adatok kezelése, hivatkozási adatok frissítések, teljesítmény adatgyűjtés vagy bérlői (ügyfél) telemetriai webhelycsoport feladatainak használatával.
* [Rugalmas adatbázis-lekérdezés](sql-database-elastic-query-overview.md) (előzetes verzió): lehetővé teszi, hogy több adatbázisok nyúló Transact-SQL-lekérdezés futtatása. Ezzel kapcsolatot jelentéskészítési eszközök, például az Excel, PowerBI, Tableau és stb.
* [Rugalmas tranzakciók](sql-database-elastic-transactions-overview.md): Ezzel a funkcióval számos Azure SQL-adatbázis-adatbázisok átterjedő tranzakciók futtatása. Rugalmas adatbázis-tranzakciók ADO .NET használatával .NET-alkalmazások érhetők el, és integrálása a már jól ismert alkalmazásprogramozási felület az [osztályok System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx)használatával.

Az alábbi ábra mutatja egy architektúrája, amely tartalmazza az adatbázisok gyűjtemény viszonyított **Rugalmas adatbázis-szolgáltatásokra** .

Ez az ábra színeinek az adatbázis sémák jelenítik meg. Az azonos színt adatbázisok megosztása ugyanarra a sémára.

1. **Azure SQL-adatbázisait** halmazának sharding architektúra használatával Azure van közzétéve.
2. A **Rugalmas adatbázis ügyfél tár** shard kezelésére szolgál.
3. Az adatbázisok csak egy részhalmazát kerülnek be egy **Rugalmas adatbázis készlet**. (Lásd: [Mi az a készlet?](sql-database-elastic-pool.md)).
4. Egy **Rugalmas adatbázis feladat** futtatása ütemezett vagy adatbázisokra ellen parancsfájlok alkalmi T-SQL nyelvben.
5. A **felosztása és egyesítése eszköz** adatok áthelyezése egy shard másikra használatos.
6. A **Rugalmas adatbázis-lekérdezés** írja be a lekérdezés shard megadása adatbázisokra nyúló teszi lehetővé.
7. **Rugalmas tranzakciók** lehetővé teszi, hogy több adatbázisok átterjedő tranzakciók futtatását. 


![Rugalmas Adatbáziseszközök][1]


## <a name="why-use-the-tools"></a>Miért érdemes használni az eszközök?

Eléréséhez rugalmasság és arányának felhő alkalmazások volt túl bonyolult feladat VMs és blob-tárolóhoz – egyszerűen hozzáadása vagy kivonása, vagy növelheti a power. De ez az állapot-nyilvántartó adatkezelési relációs adatbázisok bonyolulttá maradt. Kihívásokkal kiderült forgatókönyvekben:

* A növő, és a terhelést a relációs adatbázisok részét kapacitása zsugorításához.
* Interaktív adatok – például egy kifejezetten elfoglalt vége-ügyfél (bérlő esetében) meghatározott részhalmazát érintő esetleg felmerülő kezelése.

Hagyományos például a következő esetekben van javított szerint az alkalmazás támogatja a nagyméretű adatbázis Servers kapcsolatos. Ez a beállítás azonban a felhőben, ahol minden feldolgozás történik, az előre definiált örvend hardveren korlátozott. Ehelyett terjesztésére adatokat, és sok azonos strukturált adatbázis keresztül (méretezési mintát "sharding" néven ismert) hagyományos skála telefonos megközelítésnek költség és rugalmasság alternatív biztosít.

## <a name="horizontal-and-vertical-scaling"></a>Vízszintes és függőleges méretezési

Az alábbi ábrán az vízszintes és függőleges méretét beosztását, amelyek az egyszerű módszer a rugalmas adatbázisok megadhat.

![Vízszintes és függőleges Scaleout][2]

Vízszintes méretezés hivatkozik hozzáadásával és eltávolításával adatbázisok érdekében módosítsa a kapacitás, illetve általános teljesítményt. Ezt is nevezik "méretezés". Sharding, amelyben adatok azonos strukturált-adatbázisok körében particionálva gyakori módja a vízszintes méretezés végrehajtásához.  

Függőleges méretezés hivatkozik növelésével vagy csökkentésével teljesítményével kapcsolatos egy adott adatbázis – ez más néven "méretezését."

A legtöbb felhő nagyméretű adatbázis-alkalmazásokat az alábbi két stratégiák együttes fogja használni. Például a szoftver szolgáltatásalkalmazás, előfordulhat, hogy segítségével hozhatók létre új vége-ügyfelek Méretezés vízszintes és függőleges méretezést engedélyezése minden vége ügyfél adatbázis a nagyobb vagy kisebb erőforrások, szükség szerint a terhelést a.

* Vízszintes méretezés kezeli a [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md)használatához.

* Függőleges méretezés hajtható végre Azure PowerShell-parancsmagok használata a szolgáltatási réteg módosítása, illetve egy rugalmas készlet az adatbázisok írva.

## <a name="sharding"></a>Sharding

*Sharding* a osztania nagy mennyiségű adatot azonos strukturált független adatbázisok számos különböző technika. Érdemes különösen népszerű a felhő szoftverfejlesztő szoftver létrehozása a befejezési ügyfelei vagy a vállalkozások egy szolgáltatást (szoftver) szeretne rendelni. Ezeket a befejezési ügyfeleket gyakran "bérlők" nevezik. Sharding szükséges bármely számos oka lehet:  

* Adatok mennyiségét fájl túl nagy ahhoz, hogy elférjenek a korlátokat a egy adatbázis
* A teljes terhelését tranzakció kapacitásának meghaladja a egy adatbázis funkcióinak
* Bérlők megkövetelheti fizikai elkülönítési egymástól, így külön adatbázisok minden bérlő van szükség.
* Adatbázis különböző szakaszok eltérő geographies megfelelőség, a teljesítmény vagy a geopolitikai okokból tárolnak szükséges.

Más esetben, például az adatok elosztott eszközökről bevitel sharding használható töltse ki az adatbázisok, amelyben temporally csoportja. Ha például egy másik adatbázist is dedikált napi és heti. Ebben az esetben a sharding kulcs lehet egy egész szám, amely a dátum (jelen az összes sorban sharded táblázatot), és a lekérdezések egy adott időtartományban adatok beolvasása az alkalmazás azon részhalmazát jelenti az adatbázisok kiterjedő a szóban forgó tartományt kell irányítani.

Sharding akkor működik a legjobban, ha lehet, hogy az alkalmazás minden tranzakció korlátozott sharding kulcs egyetlen értéket. Amely biztosítja, hogy az összes tranzakció helyi adott adatbázishoz.

## <a name="multi-tenant-and-single-tenant"></a>Több elem bérlői és egyetlen-bérlői webhelyen

Egyes alkalmazások használata a legegyszerűbb megoldás különálló adatbázis létrehozása az egyes bérlői. Ez az **egyetlen bérlői sharding mintát** , ahol elkülönítési, az erőforrás a bérlő granularitása a méretezés és biztonsági mentési és visszaállítási lehetősége. Egyetlen bérlői sharding adatbázisonként tartozik egy adott bérlői azonosító érték (vagy a vevő elsődlegeskulcs-értékének), de billentyűre nem mindig kell szerepelnie az adatok magát. Feladata, az alkalmazás és a megfelelő adatbázis – minden kérés irányítja, és az ügyfél tár egyszerűsítheti meg.

![Több elem bérlői és egyetlen bérlői][4]

Mások esetek csomag több bérlők együtt az adatbázisok helyett elkülönítése őket külön adatbázisba. Ez a **több elem bérlői sharding mintát** tipikus –, és arra, hogy az alkalmazások kezelése nagyszámú nagyon kicsi bérlők vezethetik. A több elem bérlői sharding az adatbázis-táblázatokban a sorok összes készült végrehajtása egy bérlői azonosítója azonosító vagy sharding billentyűt. Ismét a alkalmazás réteg a felelős a továbbítás a megfelelő adatbázis egy bérlői kérést, és ez a rugalmas adatbázis ügyfél tár használható. Ezeken kívül minden bérlői hozzáférhetnek – részletekért sorok lásd: a [több bérlői alkalmazások rugalmas Adatbáziseszközök és sor szintű biztonsági](sql-database-elastic-tools-multi-tenant-row-level-security.md)szűrés sor szintű biztonsági használható. Adatok között adatbázisok továbbterjesztése szükség lehet a több elem bérlői sharding mintázattal, és ezzel megkönnyítheti a rugalmas adatbázis felosztása és egyesítése eszközt. Többet szeretne tudni a szoftver alkalmazásokhoz rugalmas készletek tervezés mintázatok, lásd: [Tervezési mintázatok az Azure SQL-adatbázis több bérlői szoftver alkalmazásokhoz](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Helyezze át az adatokat több egyetlen-bérleti adatbázisokhoz

A szoftver-alkalmazás létrehozásakor célszerű tipikus kínálatát leendő ügyfelek, a szoftver a próbaverzió. Ebben az esetben célszerű költséghatékony több bérlői adatbázis használandó adatokat. Jó helyen jár amikor egy potenciális ügyfél változik, egyetlen-bérlői adatbázis jobb, mivel jobb teljesítményt biztosít. Ha a felhasználói adatok is létrehozott, a próbaidőszak alatt, a [felosztása és egyesítése eszköz](sql-database-elastic-scale-overview-split-and-merge.md) segítségével helyezze át az adatokat a több elem bérlő az új egyetlen-bérlői adatbázis.

## <a name="next-steps"></a>Következő lépések

Egy minta alkalmazást, amely bemutatja az ügyfél-tárba olvassa el a [rugalmas Datababase eszközök – első lépések](sql-database-elastic-scale-get-started.md)című témakört.

Létező adatbázisokat eszközeivel, akkor olvassa el a [meglévő adatbázis áttelepítése méretezési](sql-database-elastic-convert-to-use-elastic-tools.md)konvertálni.

Részletei határozzák meg a készlet rugalmas adatbázis megtekintéséhez [árát és teljesítménybeli szempontok rugalmas adatbázis-készlet](sql-database-elastic-pool-guidance.md)talál, vagy új készlet létrehozása az [oktatóprogram](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

