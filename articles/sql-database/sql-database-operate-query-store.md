<properties
   pageTitle="Azure SQL-adatbázisban működési lekérdezés áruházból"
   description="Megtudhatja, hogy miként működnek a lekérdezésben tároló Azure SQL-adatbázisban"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>A lekérdezés tároló Azure SQL-adatbázisban működő 

Lekérdezés áruházból Azure-ban egy teljes körű felügyelt adatbázis-szolgáltatás, amely folyamatosan gyűjti össze, és bemutatja az összes lekérdezésekkel kapcsolatos részletes előzmények. Lekérdezés áruházból, mint a-verzió repülőgép nézetbeli adatok író, amelyek jelentősen lekérdezés teljesítményhiba-elhárítási, mind a felhőalapú egyszerűsíti és a helyszíni ügyfelek lehet megfontolni. Ez a cikk ismerteti az adott szempontok alapján működő lekérdezés áruházból Azure-ban. A lekérdezés előre gyűjtött adatok használatával, gyorsan diagnosztizálása és teljesítménybeli problémáinak megoldása és azok üzleti összpontosul több időt tölt a így. 

Lekérdezés áruházból mióta [világszerte elérhető](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) Azure SQL-adatbázisban November, 2015. Lekérdezés áruházból az alapja-teljesítmény elemzése és a szolgáltatások, például [SQL-adatbázis Advisor és a teljesítmény irányítópult](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)beállítása. Ez a cikk közzétételi időpillanatban lekérdezés áruházból fut 200,000-nél több felhasználó adatbázisokkal az Azure-több hónapig zavartalanul lekérdezés kapcsolatos információk összegyűjtése.

> [AZURE.IMPORTANT] A Microsoft éppen lekérdezés áruházból aktiválása az összes Azure SQL-adatbázisok (meglévő és új). 

## <a name="optimal-query-store-configuration"></a>Optimális lekérdezés áruházból konfigurálása

Ez a szakasz ismerteti, amelyek a lekérdezés áruház és a függő szolgáltatások, például [SQL-adatbázis Advisor és a teljesítmény irányítópult](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)megbízható működése optimális konfigurációt alapértékeit. Alapértelmezett beállítás a folytonos adatot a webhelycsoportban, amely minimális időmennyiséget ki/READ_ONLY állapotban van optimalizálva.

| Konfiguráció | Leírás | Alapértelmezett | Megjegyzés |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Adja meg a lekérdezés áruházból tehet z ügyfél adatbázis belüli adatokat szóköz korlátját | 100 | Új adatbázisoknál vannak érvényben |
| INTERVAL_LENGTH_MINUTES | Határozza meg, amely során gyűjtött futtatókörnyezet statisztika lekérdezés csomagok összesíti és állandó időkeret méretének. Minden aktív lekérdezéstervet van legfeljebb egy sor az ebben a konfigurációban megadott ideje | 60   | Új adatbázisoknál vannak érvényben |
| STALE_QUERY_THRESHOLD_DAYS | Időalapú karbantartása házirendet, amely meghatározza az adatmegőrzési időszak inaktív lekérdezések és állandó futtatókörnyezet statisztika | 30 | Vannak érvényben az új és az előző alapértelmezett (367) esetén |
| SIZE_BASED_CLEANUP_MODE | Itt adhatja meg, hogy az automatikus adatok karbantartása kerül sor lekérdezés tárolási adatok mérete megközelíti a korlátot | AUTOMATIKUS | Vannak érvényben adatbázisokra |
| QUERY_CAPTURE_MODE | Itt adhatja meg, hogy minden lekérdezés vagy csak egy részhalmazát lekérdezések nyomon | AUTOMATIKUS | Vannak érvényben adatbázisokra |
| FLUSH_INTERVAL_SECONDS | Adja meg a maximális időtartama rögzített futtatókörnyezet statisztika megmaradnak a memóriában, lemezre kiürítése előtt | 900 | Új adatbázisoknál vannak érvényben |
||||||

> [AZURE.IMPORTANT] Ezeket az alapértelmezett beállításokat a rendszer automatikusan alkalmazza az utolsó szakaszban lekérdezés áruházból aktiválás az összes Azure SQL-adatbázisait (lásd az előző fontos megjegyzés). Felfelé mindezek után Azure SQL-adatbázis nem kell konfigurációs értékek módosítása, felhasználók által meghatározott kivéve, ha negatív hatással elsődleges terhelést vagy a lekérdezés tároló megbízható műveletek.

Ahhoz, hogy az egyéni beállításokkal tetszés visszaállítása az eredeti állapotába konfigurációs [Adatbázis módosítása a lekérdezés tár beállításai](https://msdn.microsoft.com/library/bb522682.aspx) segítségével. Olvassa el a [Gyakorlati tanácsok a lekérdezés áruházzal](https://msdn.microsoft.com/library/mt604821.aspx) megtudhatja, hogyan felső választotta-e a beállítandó optimális érdekében.

## <a name="next-steps"></a>Következő lépések

[SQL-adatbázis teljesítmény betekintést](sql-database-performance.md)

## <a name="additional-resources"></a>További források

A további információkat kivétel az alábbi cikkekben:

- [Az adatbázis egy nézetbeli adatok hangfelvevővel](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [A lekérdezés tároló használatával teljesítmény figyelése](https://msdn.microsoft.com/library/dn817826.aspx)

- [Lekérdezés áruház használati felhasználási területei](https://msdn.microsoft.com/library/mt614796.aspx)

- [A lekérdezés tároló használatával teljesítmény figyelése](https://msdn.microsoft.com/library/dn817826.aspx) 
