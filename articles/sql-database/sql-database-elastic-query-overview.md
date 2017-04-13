<properties
    pageTitle="Azure SQL-adatbázis rugalmas adatbázis-lekérdezések áttekintése |} Microsoft Azure"
    description="A lekérdezés rugalmas szolgáltatás áttekintése"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure SQL-adatbázis rugalmas adatbázis a lekérdezések áttekintése (előzetes verzió)

Az adatbázis rugalmas lekérdezés szolgáltatás (előzetes verzió) lehetővé teszi, hogy több adatbázisok az Azure SQL-adatbázis (SQLDB) nyúló Transact-SQL-lekérdezés futtatása. Lehetővé teszi a táblázatok távoli eléréséhez, és a Microsoft és harmadik fél eszközök (az Excel, PowerBI, Tableau, stb.) az adatok rétegek több adatbázisokkal kiterjedő lekérdezés határokon adatbázis-lekérdezések végrehajtásához. Ezzel a szolgáltatással méretezése nagy adatok rétegek SQL-adatbázisban lekérdezéseket, és jelenítse meg az eredményeket az üzleti üzletiintelligencia-jelentéseket.

## <a name="documentation"></a>Dokumentáció

* [Első lépések az idegen adatbázis-lekérdezések](sql-database-elastic-query-getting-started-vertical.md)
* [Jelentés méretezett kivételének felhő adatbázisok között](sql-database-elastic-query-getting-started.md)
* [(Vízszintesen particionálva) sharded felhő adatbázisok kiterjedő lekérdezés](sql-database-elastic-query-horizontal-partitioning.md)
* [Különböző sémával (függőlegesen particionálva) felhő adatbázisok kiterjedő lekérdezés](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_végrehajtása \_távoli](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Miért érdemes használni a rugalmas lekérdezések?

**Azure SQL-adatbázis**

Teljesen a T-SQL Azure SQL-adatbázisok kiterjedő lekérdezés. Ez csak olvasható lekérdezése a távoli adatbázis tesz lehetővé. Ezzel a megoldással a jelenlegi helyszíni SQL Server ügyfeleknek alkalmazások három és négy part neveket vagy a csatolt kiszolgáló SQL-adatbázis áttelepítése beállítást.

**Elérhető a normál szint** A szokásos teljesítmény réteg mellett a prémium verzió teljesítményének réteg rugalmas lekérdezés támogatott. Című alatti előzetes korlátozások a teljesítmény korlátai alsó teljesítmény rétegek a.

**Leküldéses távoli adatbázishoz**

Rugalmas lekérdezések most közzétehetik SQL-paraméter a távoli adatbázis végrehajtásához.

**Tárolt eljárás végrehajtása**

Hajtsa végre a távoli tárolt eljárás hívások és a távoli függvények használatával [sp\_végrehajtása \_távoli](https://msdn.microsoft.com/library/mt703714).

**Rugalmas**

Külső tábla rugalmas lekérdezés most egy másik séma vagy egy táblázatnevet távoli tábla is hivatkozhat.

## <a name="elastic-database-query-scenarios"></a>Rugalmas adatbázis-lekérdezés felhasználási területei

A cél, lekérdezésekor olyan esetek, amikor több adatbázisok közreműködés általános eredményének sorok megkönnyítése érdekében. A lekérdezés vagy a felhasználó vagy alkalmazás közvetlenül vagy közvetve útján, amelyeket az adatbázis csatlakoztatott eszközök állnak. Ez akkor különösen akkor hasznos, ha létrehozása kereskedelmi adatok vagy Üzletiintelligencia-integrációs eszközök jelentéseket – vagy bármely alkalmazásban, amely nem módosítható. Rugalmas lekérdezéshez több adatbázisait SQL Server-kapcsolat ismerős felületet eszközök, például az Excel, a PowerBI, Tableau vagy Cognos keresztül is lekérdezhetők.
Egy rugalmas lekérdezést lehetővé teszi, hogy az adatbázis-lekérdezések SQL Server Management Studio vagy Visual Studio által kibocsátott teljes gyűjteményt egyszerű hozzáférést, és idegen adatbázis-lekérdezés entitás keretrendszer vagy más ORM környezetek elősegíti. Ábra 1 forgatókönyvön, ahol egy létező felhő alkalmazás (amely a [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md)) hoz létre egy méretezett kivételének adatok rétegben, és idegen adatbázis-jelentéshez használatos egy rugalmas lekérdezést jeleníti meg.

**Ábra 1** Méretezett kivételének adatok rétegben használt rugalmas adatbázis-lekérdezés

![Méretezett kivételének adatok rétegben használt rugalmas adatbázis-lekérdezés][1]

Rugalmas lekérdezés forgatókönyvet az alábbi topológiák jellemzi:

* **Függőleges szétválasztás – határokon adatbázis - lekérdezések** (1 topológia): az adatok között egy adatok réteg adatbázisok számos függőlegesen particionálva. Általában táblák különböző csoportjaihoz különböző adatbázisok állnak. Ez azt jelenti, hogy a séma eltér a különböző adatbázisok. Például minden olyan tábla készlet találhatók egy adatbázis második adatbázis minden könyvelési kapcsolódó tábla várakozó. A topológia közös használati eset szükség egy kiterjedő lekérdezés vagy jelentések összeállításához több adatbázisokat a táblázatok között.
* **Vízszintes szétválasztás – Sharding** (Topológia 2): adatok vízszintesen particionálva sorok terjesztheti ki méretezett adatok keresztül réteg. Az ezt a megközelítést a séma megegyezik a minden résztvevő adatbázisra érvényesek lesznek. Ezt a megközelítést "sharding" néven is ismert. Sharding hajtható végre, és a felügyelt (1) az rugalmas adatbázis használata eszközök, tárak és (2) önmaga-sharding. Egy rugalmas lekérdezést a lekérdezés vagy jelentések összeállítása shards számos különböző szolgál.

> [AZURE.NOTE] Rugalmas adatbázis-lekérdezés a legjobban alkalmi jelentéskészítési esetek, ahol a legtöbb feldolgozása végezheti el az adatok rétegben. Nehéz jelentéskészítési munkaterhelésekből vagy adatok raktári esetek összetettebb lekérdezések esetén is fontolja meg inkább [Azure SQL-adatraktár](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Adatbázis-lekérdezés rugalmas topológiák

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>1 topológia: Függőleges szétválasztás – határokon adatbázis-lekérdezések

Coding indításához olvassa el a [több adatbázis - lekérdezés (függőleges szétválasztás) – első lépések](sql-database-elastic-query-getting-started-vertical.md)című témakört.

Egy rugalmas lekérdezés is használható, hogy a rendelkezésre álló egyéb SQLDB adatbázisokhoz SQLDB adatbázisban tárolt adatokhoz. Ezzel a lekérdezések hivatkozni bármely távoli SQLDB adatbázisból származó táblázatot egy adatbázisból. Az első lépésként meghatározása a külső adatforrás minden egyes távoli adatbázishoz. A külső adatforrás határozza meg a helyi adatbázis, amelyhez képest a távoli adatbázis lévő táblázatok eléréséhez szükséges. Nincs módosítás szükség a távoli adatbázishoz. Ha tipikus függőleges particionáló esetben hol különböző adatbázisok van-e másik sémák rugalmas lekérdezések használható gyakori használati eset, például a hivatkozás adatokhoz való hozzáférés végrehajtása és a lekérdezés határokon adatbázis.

**Hivatkozási adatok**: topológia 1 hivatkozás adatkezelés szolgál. Az alábbi ábrán a két tábla (T1 és T2) hivatkozási adatok megmaradnak egy dedikált adatbázisban. Rugalmas lekérdezés használata, most már hozzáférhet a táblázatok T1 és T2 távolról más adatbázisokból az ábrán látható módon. 1, ha a hivatkozás táblázatok kis- és távoli lekérdezések hivatkozás táblába használata topológia szelektív predikátumok van.

**Ábra 2** Függőleges szétválasztás - rugalmas query-lekérdezés hivatkozási adatok használata

![Függőleges szétválasztás - rugalmas query-lekérdezés hivatkozási adatok használata][3]

**Idegen adatbázis - lekérdezés**: elasztikus engedélyezése használati eset igénylő több SQLDB adatbázisok közötti lekérdezése kérdezi le. 3 ábrán látható négy különböző adatbázisok: CRM, készlet, HR és termékek. Az adatbázisok valamelyikével végrehajtott lekérdezések is egy vagy összes a többi adatbázisok hozzáférésre van szükségük. Rugalmas lekérdezés használata, beállíthatja az adatbázis-ebben az esetben néhány egyszerű DDL-utasításokat az egyes a négy adatbázisok futtatásával. Ez egyszeri konfigurálása után egy távoli tábla teljes egészében egyszerűen, egy helyi tábla hivatkozik, a T-SQL-lekérdezések vagy az Üzletiintelligencia-eszközeiről. Ezt a megközelítést ajánlott, ha a távoli lekérdezések nem eredményt nagy.

**Ábra 3** Függőleges szétválasztás - különböző adatbázisok elasztikus query-lekérdezés használata

![Függőleges szétválasztás - különböző adatbázisok elasztikus query-lekérdezés használata][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>2 topológia: Vízszintes szétválasztás – sharding

Rugalmas lekérdezés feladatok jelentéskészítési fölé egy sharded vízszintesen, azaz partícionáltuk, adatok réteg használatához egy [Rugalmas adatbázis shard térkép](sql-database-elastic-scale-shard-map-management.md) az adatok réteg adatbázisok ábrázolásához. Általában csak egyetlen shard térkép használatos ebben az esetben, és a belépési pontjához lekérdezések jelentéskészítéshez rugalmas lekérdezés funkciók dedikált adatbázis szolgál. Csak a kijelölt adatbázist a shard térkép hozzáférésre van szüksége. Ábra 4 szemlélteti a topológiája és a térképpel rugalmas lekérdezés adatbázis és shard konfigurációját. Az adatbázisok az adatok réteg lehet edition vagy bármely Azure SQL-adatbázis verziója. Többet szeretne tudni a rugalmas adatbázis ügyfél és a shard térképek létrehozása olvassa el a [térkép management Shard](sql-database-elastic-scale-shard-map-management.md)című témakört.

**A 4-es szám** Vízszintes szétválasztás - rugalmas lekérdezéssel sharded adatok rétegek fölé tartalmazó jelentések készítéséhez

![Vízszintes szétválasztás - rugalmas lekérdezéssel sharded adatok rétegek fölé tartalmazó jelentések készítéséhez][5]

> [AZURE.NOTE] A kijelölt rugalmas lekérdezés adatbázist SQL-adatbázis v12 adatbázis kell lennie. Vannak a saját maguk shards nincs korlátozva.

[Első lépések a vízszintes szétválasztás (sharding) rugalmas adatbázis-lekérdezés](sql-database-elastic-query-getting-started.md)coding indításához megtekintése

## <a name="implementing-elastic-database-queries"></a>Végrehajtási rugalmas az adatbázis-lekérdezések

A lépéseket a vízszintes és függőleges particionáló felhasználási területei rugalmas lekérdezés végrehajtásához az alábbi szakaszok ismertetik. Azok is érdemes a különböző particionáló forgatókönyvek részletesebb dokumentációjában találhat.

### <a name="vertical-partitioning---cross-database-queries"></a>Függőleges szétválasztás - határokon adatbázis-lekérdezések

Az alábbi lépéseket a függőleges particionáló felhasználási területei ugyanarra a sémára SQLDB távoli adatbázis található táblázat hozzáférést igénylő rugalmas az adatbázis-lekérdezések beállítása:

*    [Hozzon létre fő kulcs](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Adatbázis létrehozása az HITELESÍTŐ hatóköre](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [A külső ADATFORRÁS létrehozása és LEGÖRDÜLŐ](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource **RDBMS** típusú
*    [Külső tábla létrehozása/LEGÖRDÜLŐ](https://msdn.microsoft.com/library/dn935021.aspx) táblanév

Után a DDL kimutatások fut, a távoli tábla "táblanév", mintha egy helyi tábla is elérheti. Azure SQL-adatbázis automatikusan nyitja meg a távoli adatbázishoz csatlakozhatna, kezeli a kérelmet, a távoli adatbázishoz, és az eredményt ad.
További információt a lépéseket a függőleges particionáló eset [Rugalmas lekérdezés függőleges szétválasztás](sql-database-elastic-query-vertical-partitioning.md)találhatók.  

### <a name="horizontal-partitioning---sharding"></a>Vízszintes szétválasztás - sharding

Az alábbi lépéseket kell beállítania vízszintes particionáló felhasználási területei használatával egy táblázat hozzáférést igénylő található rugalmas az adatbázis-lekérdezések (általában) több távoli SQLDB adatbázisok:

*    [Hozzon létre fő kulcs](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Adatbázis létrehozása az HITELESÍTŐ hatóköre](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Az adatok réteg használata a rugalmas adatbázis ügyfél tárban, amely [shard térkép](sql-database-elastic-scale-shard-map-management.md) létrehozása.   
*    [A külső ADATFORRÁS létrehozása és LEGÖRDÜLŐ](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource **SHARD_MAP_MANAGER** típusú
*    [Külső tábla létrehozása/LEGÖRDÜLŐ](https://msdn.microsoft.com/library/dn935021.aspx) táblanév

Ezeket a lépéseket kell végrehajtani, miután érheti el a vízszintesen particionált táblázat "táblanév", mintha egy helyi tábla. Azure SQL-adatbázis automatikusan megnyílik a párhuzamos több kapcsolat a táblák fizikailag tárolási helyére a távoli adatbázis, a távoli adatbázist kérelmeket dolgozza fel, és az eredményt ad.
További információt a lépéseket a vízszintes particionáló eset [Rugalmas lekérdezés vízszintes szétválasztás](sql-database-elastic-query-horizontal-partitioning.md)találhatók.

## <a name="t-sql-querying"></a>Kétmintás T-SQL-lekérdezés
Miután meghatározta a külső adatforrások és a külső táblázatok, a rendszeres SQL Server kapcsolati karakterláncot az adatbázisok csatlakozhat, ahol a külső táblák megadott is használhatja. Ezt követően futtathatja T-SQL-utasításait a külső táblák fölé a kapcsolat az alábbiakban ismertetett korlátozások. További információk és az SQL-T lekérdezések példák a található a dokumentáció témakörök [szétválasztás vízszintes](sql-database-elastic-query-horizontal-partitioning.md) és [Függőleges szétválasztás](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Csatlakozási eszközök
Normál SQL Server kapcsolati karakterláncot az alkalmazások és a BI vagy adatok integrációs eszközök csatlakozhat külső táblák létrehozott adatbázisok is használhatja. Győződjön meg arról, hogy az eszköz adatforrásként az SQL Server támogatott. Miután létrejött, olvassa el a lekérdezés rugalmas adatbázis és a külső táblákat az adott adatbázis ugyanúgy, mint bármely más SQL Server-adatbázis csatlakoztatása a eszközzel, a módon érheti el.

> [AZURE.IMPORTANT] A rugalmas lekérdezések használja az Azure Active Directory Authentication jelenleg nem támogatott.

## <a name="cost"></a>Költség

Rugalmas lekérdezés is tartalmazni fogja az Azure SQL-adatbázis adatbázisok költségét. Figyelje meg, hogy a távoli adatbázis esetén különböző adatközpont, mint a rugalmas lekérdezés végpont topológiák támogatottak, de a távoli adatbázisból származó adatok kilépési rendszeres [díjak Azure](https://azure.microsoft.com/pricing/details/data-transfers/)-előfizetést terhelő.

## <a name="preview-limitations"></a>Előzetes korlátai
* Az első rugalmas lekérdezés futtatása is igénybe vehet a szabványos teljesítmény rétegben néhány percet. Ez esetben szükség a Rugalmas lekérdezési funkciókat; betöltése nagyobb teljesítmény rétegek a teljesítmény betöltése javítja.
* Külső adatforrások vagy a külső táblák SSMS vagy SSDT parancsfájlok még nem támogatott.
* Az Importálás/Exportálás SQL DB még nem támogatja a külső adatforrások és a külső táblákat. Akkor használja az importálás/exportálás, ha közvetlen ezeknek az objektumoknak az exportálás előtt, és majd hozza létre őket importálása után.
* Rugalmas adatbázis-lekérdezés jelenleg csak támogatja külső tábla írásvédett elérését. Azonban felhasználhatja funkciója az SQL-T az adatbázis ahol a külső tábla meg van adva. Ez akkor lehet hasznos, pl. továbbra is fennáll a használ, például ideiglenes eredmények, jelölje be a < column_list > < local_table > IMPORTÁLÁSA, vagy olvassa el a külső táblák rugalmas lekérdezés-adatbázisban tárolt eljárások meghatározásához.
* Kivételével nvarchar(max) üzleti már nem támogatott külső tábla definíciók. Megoldás is a távoli adatbázishoz, amely az üzleti típus nvarchar(max) az árnyékot nézet létrehozása, meghatározása a külső táblázat fölé helyett az alap táblázat a nézetben és majd leadott azt az eredeti üzleti típusát a lekérdezések.
* Oszlop statisztika keresztül külső táblák jelenleg nem támogatott. Táblázatok statisztika használata támogatott, de kell manuálisan kell létrehozni.

## <a name="feedback"></a>Visszajelzés
Ossza meg a felhasználói felület véleményezhessék rugalmas lekérdezések nekünk az Disqus az alábbi, az MSDN-fórumok vagy Stackoverflow. Azt érdeklik összes típusokat, illetve hogy a szolgáltatás (hibák, durva élek, szolgáltatás szünetek).

## <a name="more-information"></a>További információ

A következő dokumentumok talál további információt a több adatbázis-lekérdezés és függőleges particionáló esetek:

* [Idegen adatbázis-lekérdezés és a függőleges particionáló – áttekintés](sql-database-elastic-query-vertical-partitioning.md)
* Próbálja meg, hogy a teljes lépésenkénti oktatóprogram percben futó működő példát: [több adatbázis - lekérdezés (függőleges szétválasztás) – első lépések](sql-database-elastic-query-getting-started-vertical.md).


Itt érhető el a vízszintes szétválasztás és sharding esetek további információt:

* [Vízszintes szétválasztás és sharding – áttekintés](sql-database-elastic-query-horizontal-partitioning.md)
* Próbálja meg, hogy a teljes lépésenkénti oktatóprogram percben futó működő példát: [első lépések a vízszintes szétválasztás (sharding) rugalmas adatbázis-lekérdezés](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
