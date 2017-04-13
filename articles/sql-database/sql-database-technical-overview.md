<properties
    pageTitle="Mi az SQL-adatbázishoz? Bevezetés az SQL-adatbázishoz |} Microsoft Azure"
    description="SQL-adatbázis-ismertetés: technikai részletek és a funkciók a Microsoft a relációs adatbázis-kezelő rendszer (RDBMS) a felhőben."
    keywords="sql-ben – bevezetés SQL, mi az sql-adatbázis – bevezetés"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Mi az SQL-adatbázishoz? SQL-adatbázis – bevezetés

SQL-adatbázis olyan relációs adatbázisból szolgáltatás, a felhőben, az egyedülálló Microsoft SQL Server kulcsfontosságú funkciókhoz motor alapján. SQL-adatbázis biztosítja kiszámíthatóan teljesítmény elérése érdekében nincs legrövidebb leállás, üzemkészségének és adatvédelem méretezhetőség – mindezt közelében nulla felügyeleti. Gyors alkalmazások fejlesztéséhez és az idő felgyorsítása piacra helyett virtuális gépeken futó és infrastruktúra kezelése összpontosíthat. Az [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) -motor alapuló, mert SQL-adatbázis támogatja a meglévő SQL Server eszközök, tárak és API-hoz, így könnyebb áthelyezése és bővítése a felhőbe.

Ez a témakör az SQL-adatbázis core fogalmak és a teljesítmény, méretezhetőség és kezelhetőséget, amely hivatkozásokat tartalmaz az részleteinek feltárása funkciói. Ha készen áll a ugrani, akkor [az első SQL-adatbázis létrehozása](sql-database-get-started.md) vagy [egy rugalmas adatbázis készlet létrehozása](sql-database-elastic-pool-create-portal.md) perc alatt. Ha azt szeretné, hogy egy mélyebb merülési, ebből a videóból 30 perc.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>A teljesítmény és a méretezés nélkül legrövidebb leállás módosítása

SQL-adatbázisait Basic, normál és Premium *szolgáltatási rétegek*érhető el. Minden szolgáltatási réteg [Teljesítmény és funkciók különböző mértékű](sql-database-service-tiers.md) felajánlja az adatbázis-munkaterhelésekből nehéz lightweight támogatja. Hozhat létre az első alkalmazást egy kis adatbázisban az néhány hajdú-bihar megye hó, majd [a szolgáltatási réteg módosítása](sql-database-scale-up.md) manuálisan vagy programozás útján bármikor előrehaladtával az alkalmazás vírus világszerte, az alkalmazás és az ügyfelekkel legrövidebb leállás nélkül.

Sok vállalkozások és alkalmazások esetén nem hozza létre az adatbázisokat és igény szerinti egyetlen adatbázis teljesítményét felfelé vagy lefelé is elegendő, különösen ha szokásai viszonylag kiszámíthatóan. De ha járhat szokásai, azt teheti azt nehéz költségek és a vállalati modell kezelése.

[Rugalmas készletek](sql-database-elastic-pool.md) SQL-adatbázisban megoldhatja a problémát. Az egyszerű fogalma. Lefoglalhat egy készlet teljesítményt, és fizet az egyetlen adatbázis teljesítmény, hanem a készlet csoportos teljesítményét. Nem kell hívnia adatbázis teljesítmény, felfelé vagy lefelé. A készletben nevű *rugalmas adatbázisok*, az adatbázisok automatikus méretezése felfelé és lefelé találkozik igény szerint. Rugalmas adatbázisok felhasználása, de ne lépjék készlet, így a költség továbbra is kiszámítható, még akkor is, ha az adatbázis használatát nem. Sőt azt is megteheti, [hozzáadása és eltávolítása a kvótáját adatbázisok](sql-database-elastic-pool-manage-portal.md)ezreihez szabályozhatja, hogy a költségvetésen belül az összes alkalmazás levétele az adatbázisok néhány méretezését. Többet szeretne tudni a szoftver alkalmazásokhoz rugalmas készletek tervezés mintázatok, lásd: [Tervezési mintázatok az Azure SQL-adatbázis több bérlői szoftver alkalmazásokhoz](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Mindkét módon módba – egyszeri vagy rugalmas – nem zárolt állapotban van,. Rugalmas adatbázis készletek egyetlen adatbázis keverése, és módosítsa a szolgáltatási rétegek egyetlen adatbázisok és készleteket az innovatív tervezheti. Ezenkívül a power és Azure gyermekektől, azt is megteheti mix és a hol.van Azure SQL-adatbázis egyedi modern alkalmazás tervezés igényekhez, költség- és erőforrás-hatékonyságot meghajtó és zárolásának feloldása az új üzleti lehetőségek-szolgáltatások.

De hogyan is összehasonlíthatja az adatbázisok és az adatbázis-készletek relatív teljesítménye? Honnan lehet tudni a jobb gombbal kattintson a Leállítás, ha Betárcsázás felfelé és lefelé? A válasz pedig az adatbázis tranzakció egység (DTU) egyetlen adatbázisok a rugalmas és az adatbázis-készletek rugalmas DTU (eDTU). Lásd: [SQL-adatbázis beállítások és a teljesítmény: megértéséhez, hogy mi érhető el az egyes szolgáltatási réteg](sql-database-service-tiers.md) további információt.

## <a name="keep-your-app-and-business-running"></a>Az alkalmazás és az üzletmenet

Azure-féle iparágban vezető 99.99 % elérhetősége szolgáltatásiszint-szerződés [(SLA)](http://azure.microsoft.com/support/legal/sla/), a Microsoft által kezelt adatközpontokkal globális hálózaton hajtott megkönnyíti az alkalmazás 24/7 rendszert futtató. Minden SQL-adatbázissal akkor kihasználhatja beépített adatvédelem hibatűrést és adatvédelem, egyéb esetben kellene tervezése, vásárol, létrehozása és kezelése. Attól függően, hogy az üzleti igények, még ha így annak érdekében, hogy az alkalmazás és a vállalkozás is helyreállítása gyorsan megelőzve katasztrófa, hiba történt vagy mást védelem további rétegek igény is. SQL-adatbázissal minden egyes szolgáltatási réteg menüjéből különböző elsajátításával akár használhatja funkciók és futtatása és maradhat ily módon. Pont és az idő visszaállítása való visszatéréshez adatbázis arra az állapotra, 35 nap as far back as is használhatja. Ezeken kívül az Adatközpont az adatbázisok szolgáltatója egy üzemszünetek találkozik, is egy másik régióbeli adatbázis-kópiák áttérni. Másik lehetőségként használhatja kópiák frissítések vagy áthelyezése különböző régiók.

![Az SQL-adatbázis Geo-replikáció](./media/sql-database-technical-overview/azure_sqldb_map.png)


Lásd: [Üzleti folytonosságot](sql-database-business-continuity.md) a különböző üzleti folytonosságot funkciókat különböző szolgáltatási rétegek kapcsolatban további tájékoztatást.

## <a name="secure-your-data"></a>Az adatok védelme
Az SQL Server, hogy az SQL-adatbázis korlátozhatja a hozzáférést, adatok védelme, amely segít figyelése funkcióival jogállamiságot folytonos adatok biztonsági hagyományokkal tartalmaz. Lásd: [az SQL-adatbázis biztonságossá tétele](sql-database-security.md) a biztonsági beállítások SQL-adatbázisban van egy gyors lefutása. A [Biztonság otthon az SQL Server adatbázismotort és SQL-adatbázis](https://msdn.microsoft.com/library/bb510589) biztonsági funkciók átfogóbb nézetének megtekintése És az [Azure az Adatvédelmi központ](https://azure.microsoft.com/support/trust-center/security/) Azure's platform biztonsággal kapcsolatos információkat.

## <a name="next-steps"></a>Következő lépések
Most, hogy olvassa el a bevezető SQL-adatbázishoz, és választ "Mi SQL-adatbázishoz?", készen áll:

- Lásd: az [ár, oldal](https://azure.microsoft.com/pricing/details/sql-database/) egy adatbázist és rugalmas adatbázis költség összehasonlításokat és kalkulátorok.
- Tudjon meg többet [rugalmas készletek](sql-database-elastic-pool.md).
- Első lépések [az első adatbázis](sql-database-get-started.md)létrehozásával.
- [Csatlakozás és a lekérdezés SSMS](sql-database-connect-query-ssms.md)
- Az első, a C#, Java, Node.js, PHP, Python vagy fonetikus alkalmazás összeállítása: [kapcsolat tárak SQL-adatbázis és az SQL Server](sql-database-libraries.md)
- Lásd: a címek és az [összes témakörök Azure sql-adatbázis-szolgáltatás](sql-database-index-all-articles.md)leírása: index.
