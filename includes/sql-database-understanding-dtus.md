Az adatbázis-tranzakción egység (DTU), amely alapján a való életben mérték adatbázisok a relatív power SQL-adatbázisban a Mértékegység: az adatbázis-tranzakciók. Azt tartott egy sor olyan műveleteket, amelyek esetében egy online tranzakció (OLTP) kérés feldolgozása, és kattintson a hány tranzakció fejezhető másodpercenként a teljes mértékben mérni feltételek betöltött tipikus (Ez a rövid, erről a tengely adatait a [javasolt áttekintése](../articles/sql-database/sql-database-benchmark-overview.md)). 

Például 1750 DTUs prémium P11 adatbázis nyújt további DTU x 350 mint 5 DTUs egyszerű adatbázis power számítja ki. 

![Bevezetés az SQL-adatbázishoz: egy adatbázis DTUs réteg és szintjét.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Ha egy meglévő SQL Server-adatbázishoz, a külső eszközben, [Az Azure SQL-adatbázis DTU Számológép](http://dtucalculator.azurewebsites.net/)segítségével felvenni a teljesítményszint becslése, és előfordulhat, hogy az adatbázis Azure SQL-adatbázisban réteg szolgáltatás.

### <a name="dtu-vs-edtu"></a>DTU eDTU összehasonlítása

Az egyetlen adatbázisok DTU közvetlenül a eDTU rugalmas adatbázisok megfelelője. Egyszerű rugalmas adatbázis erőforráskészlethez tartozik egy adatbázist, például a maximum 5 eDTUs kínál. Ez az egyszerű adatbázisként azonos teljesítményét. A különbség, hogy amíg azt a rugalmas adatbázis nem felhasználása bármely eDTUs a készletből. 

![Bevezetés az SQL-adatbázishoz: réteg szerinti rugalmas készletek.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Egyszerű példa: segítséget nyújt. Egy egyszerű rugalmas adatbázis készlet 1000 DTUs rendelkező vennie, és 800 adatbázisok húzhatja azt. Mindaddig, amíg csak 200 800 adatbázisok használata érdekli bármely pontján idő (5 DTU X 200 = 1000), a nem gombra kattint a készlet kapacitása, és nem csökkentheti a adatbázis teljesítményét. Ebben a példában az áttekinthetőség egyszerűsített. A valós matematikai egy kicsit bonyolultabb. A portál a matematikai jelent meg, és lehetővé teszi a korábbi adatbázis használatát alapján ajánlása. Lásd: [árát és teljesítménybeli szempontok rugalmas adatbázis-készlet](../articles/sql-database/sql-database-elastic-pool-guidance.md) megtudhatja, hogyan működik a a javaslatok, illetve műveletek automatikus elvégzéséhez. 
