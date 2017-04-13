<properties
   pageTitle="Azure SQL-adatbázis több bérlői alkalmazások elkülönítési és hatékonyság hoz létre."
   description="Megtudhatja, hogyan hozza létre a SQL-adatbázis több bérlői alkalmazások"
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
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Hozza létre a több elem bérlői alkalmazások elkülönítési és a hatékonyság az Azure SQL-adatbázissal

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Rugalmas készletek kihasználhatja és hatékonyabb több bérlői alkalmazások készítése

Ha Ön a szoftver fejlesztők írása a több elem bérlői-at, és vevőknek kezelése, gyakran gondoskodnia kompromisszumok vevői teljesítmény, kezelése és biztonsági. Az Azure SQL adatbázis rugalmas adatbázis-készletek, ha már van, hogy a biztonságos. Ezek készletek segítséget nyújt a kezelése és több bérlői alkalmazások figyelése és szerezni egyik-ügyfél-adatbázis-elkülönítési előnyei. Lásd: a [Tervező mintázatok több bérlői szoftver alkalmazások Azure SQL-adatbázissal](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![Szerkesztés-többszörös-bérlői-alkalmazások](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatikus méretezés szabályozhatja

Készletek automatikus méretezése teljesítmény és tárolókapacitással rendelkezik adatbázisoknál rugalmas menet közben. A teljesítmény erőforráskészlethez tartozik rendelt szabályozhatja, hozzáadása vagy eltávolítása rugalmas adatbázisok igény, és rugalmas adatbázisok teljesítménye meghatározása a készlet teljes költsége megtartásával. Ez azt jelenti, hogy nem kell aggódnia amiatt, hogy az egyes adatbázisokat használatát kezelése.

[Olvassa el a dokumentáció](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>A környezet intelligens kezelése

Beépített méretező javaslatok ezzel kapcsolatban beérkező azonosítása adatbázisokra, amelyek készletek előnyös. Ezek a javaslatok "" lehetőségelemzés rövid optimalizálása a teljesítmény céloknak való teszi lehetővé. Rich teljesítmény figyelés és irányítópultok hibaelhárítási segítséget korábbi készlet kihasználtsági ábrázolása.

[Olvassa el a dokumentáció](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>A teljesítmény és igényekhez ár

Egyszerű, szabványos, és prémium készletek közli, hogy a teljesítményt, tárhely és beállítások árak széles skálája. Készletek legfeljebb 400 rugalmas adatbázisok is tartalmazhat. Rugalmas adatbázisok automatikus méretezése legfeljebb 1000 rugalmas adatbázis tranzakció egységek (eDTU) is.

[Olvassa el a dokumentáció](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Rugalmas eszközök

Rugalmas-készletek kívül vannak SQL-adatbázis funkcióval segíti a működési tevékenységek kezelése több adatbázisok között:

**Idegen adatbázis-lekérdezések és a jelentés végrehajtása**  
[Rugalmas adatbázis-lekérdezés](sql-database-elastic-query-overview.md) lehetővé teszi, hogy a rugalmas alkalmazáskészlet-adatbázisok különböző futtatása a lekérdezésekben és jelentésekben, és távoli adatforrásból elemet egyszerre sok adatbázisokat a készlet tárolt nyithatja meg.

**Futtassa a több adatbázis-tranzakciók.**  
[Rugalmas adatbázis-tranzakciók](sql-database-elastic-transactions-overview.md) , amely számos SQL-adatbázisait-adatbázisok időtartamát, és hajtsa végre a műveleteket, (tehát ha pénzügyi tranzakciók feldolgozása adatbázisok között, vagy ha egy adatbázisban, és a rendelések készlet frissítése) tranzakciók futtatása teszi lehetővé.

**Hajtsa végre a több adatbázis ugyanazokat a műveleteket.**  
[Rugalmas adatbázis feladatok](sql-database-elastic-jobs-overview.md) végrehajtása felügyeleti műveleteket, például az indexek Újraépítés, vagy meglévőket frissíteni a sémák végig a rugalmas készletben adatbázisonként.

Lépjen a kezdőlapra, egyéb SQL-adatbázis nyújtotta előnyöket megjelenítéséhez.
[Kivennie a fájlt](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Következő lépések

Ismerkedés az [Azure előfizetés ingyenes](https://azure.microsoft.com/get-started/) és [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).

## <a name="additional-resources"></a>További források

Ismerkedjen meg az [SQL-adatbázis lehetőségeit](https://azure.microsoft.com/services/sql-database/).
 
Tekintse át az [SQL-adatbázis technikai áttekintés](sql-database-technical-overview.md).  
