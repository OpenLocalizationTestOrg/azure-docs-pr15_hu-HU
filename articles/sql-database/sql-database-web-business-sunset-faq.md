<properties
   pageTitle="Azure SQL-adatbázis webes és üzleti Edition sunset – gyakori kérdések |} Microsoft Azure"
   description="Megtudhatja, hogy az Azure SQL-webes és az üzleti adatbázisok visszavonásának, és ismereteket szerezhet a és az új szolgáltatás a meghatározási funkciók."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Webes és üzleti Edition sunset gyakori kérdések

Azure SQL-webhely és a vállalati adatbázis most már vannak inaktív. A Basic, normál, prémium és elasztikus rétegek cserélje le a leköszönő webes és az üzleti adatbázis.

Az SQL-adatbázis-szolgáltatás, amely segít a webes és üzleti adatbázisok frissítésével kapcsolatban, javasolja, egy megfelelő szolgáltatás réteg és a teljesítmény szintet (árak réteg)-adatbázisonként a korábbi terhelést alapján.

**Az első árak réteg javaslatok:**

- [SQL-adatbázis V12 frissítése az Azure portál használatával](sql-database-upgrade-server-portal.md)
- [SQL-adatbázis V12 frissítés PowerShell használatával](sql-database-upgrade-server-powershell.md)
- [A webhely vagy a vállalati adatbázis árak réteg módosítása](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Miért jeleníti meg az Azure-portálra a webes és üzleti edition adatbázisok már inaktív?

Webes és üzleti edition adatbázisok nem lesz elérhető szeptember 2015 után, mert a a portálon címkék már inaktív webes és üzleti adatbázist. A visszavont is szolgál, hogy bármely webes és az üzleti adatbázisok frissíteni fogják normál, egyszerű vagy prémium emlékeztetőt. Részletes információt a meglévő webhelyen vagy a vállalati adatbázis frissítése az új szolgáltatási rétegek című témakörben talál [frissítése a Microsoft Azure SQL-adatbázis V12](sql-database-upgrade-server-portal.md).

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Milyen új szolgáltatási réteg frissíteni a meglévő webhelyen vagy a vállalati adatbázisra – a legjobb választás az?

Jelölje ki a megfelelő új szolgáltatás réteg és a teljesítmény szintű a meglévő webhelyen vagy a vállalati adatbázis attól függ, hogy az adott funkció és a teljesítmény követelményeket, az alkalmazás.

A árak réteg javaslatok fölött vagy információ leírt segítségével segít a megfelelő új szolgáltatási réteg kijelölése, [hogy Azure SQL-adatbázis V12 frissítése](sql-database-upgrade-server-portal.md)című cikk tartalmaz.

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Miért van Microsoft bemutatása új szolgáltatási rétegek?

Ügyfeleink visszajelzései alapján Azure SQL-adatbázis új szolgáltatási rétegek munkában további egyszerűen támogatja a relációs adatbázisok munkaterhelésekből van bemutatása. Az új rétegek kiszámíthatóan teljesítményt nyújtsa végig az aktív tranzakció alapú alkalmazás igények egyszerűsített hét szinteket egy dokumentumhasználat készültek. Emellett az új rétegek kínálnak üzleti folytonosságot szolgáltatások erősebb üzemidőt SLA, nagyobb adatbázis mérete kisebb pénz, és a számlázási továbbfejlesztett lehetőségek tartomány.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Hol találok további tudnivalók az új szolgáltatási rétegek?

Az új szolgáltatási rétegek és a teljesítmény modell kapcsolatos részletes tudnivalókért olvassa el a [szolgáltatási rétegek](sql-database-service-tiers.md)című témakört. A részletes adatait az új szolgáltatási rétegek árak, olvassa el a [SQL-adatbázis árak](https://azure.microsoft.com/pricing/details/sql-database/)című témakört.

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Milyen szolgáltatások és funkciók nem lesznek elérhetők, a Basic, normál és prémium verzióban?

A szövetségeire fog kell funkciója a webes és üzleti kiadásában. Azon ügyfelek, akik az adatbázisát skála meg kell olyan javasoljuk, hogy inkább használja, vagy áthelyezése [rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md) az [Azure SQL-adatbázissal](sql-database-elastic-scale-get-started.md), amely összeállítását, és kezelni a sharding használó alkalmazást egyszerűbbé teszi. Ügyfél .NET tár lehetővé teszi, hogy az alkalmazások határozza meg, hogyan adatok megfelelő adatbázisok shards és útvonalak OLTP kérések van rendelve. A támogatási adatkezelési műveletek dolgozóját, hogy hogyan adatok shards között van meghatározva, az Azure felhőalapú szolgáltatás sablon megtalálható a saját Azure előfizetés is megbízhat. [Rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md)kívül Microsoft továbbra is létrehozása és közzététele a mély ügyfél Előjegyzések learnings alapuló [egyéni sharding mintázatok és eljárásokat útmutatást](https://msdn.microsoft.com/library/azure/dn764977.aspx) . Új azon ügyfelek, akik skála funkciók ki kell kell [rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md) kivétele és/vagy lépjen kapcsolatba a Microsoft Support azok beállítások ki szeretné számítani.

A Microsoft is az adatbázis másolása élmény a prémium adatbázisok megváltoztatása. ADATBÁZIS létrehozása prémium adatbázis kvóta korlátozott, volt korábban... A másolat az SQL-T létrehozott felfüggesztett prémium adatbázis fenntartott erőforrásokat, nem lett-e alapdíjával díjazott az üzleti adatbázisként azonos. Prémium kvóta érhető el további szabadon, mint a már nem támogatott felfüggesztett prémium verzióban. Adatbázis másolatok ekkor létrejön a azonos edition és teljesítményszint forrásaként, és ennek megfelelően számlázható. Ügyfelek választhat egy másik szolgáltatáscsaládba réteg vagy a teljesítmény szintre csökkentheti a költség, ha azt szeretné, a másolt adatbázisok vissza léptetheti. Meglévő felfüggesztett prémium adatbázisok Business edition részeként ebben a kiadásban átalakításra kerül. Várható, hogy [Pont és az idő visszaállítása](sql-database-recovery-using-backups.md#point-in-time-restore) bevezetésével csökkenti az adatbázisok biztonsági másolatot készíthet kell.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Hogyan nem Basic, normál és Premium növelheti a számlázási hatékonyságát?

Egyszerű, szabványos, és prémium Azure SQL-adatbázisait számláját az órát, és az azt jelenti, hogy minden adatbázisban méretezni, felfelé vagy lefelé kell. Van a szolgáltatás réteg és a teljesítmény legmagasabb úgy dönt, hogy minden olyan órában alapján rögzített sebessége számlát kapni. Ezenkívül teljesítményszint (Példa: egyszerű, S1 és P2), hogy könnyebben adatbázis nap/óra teljesítmény szintenként egyetlen hónap kapcsolatban felmerülő száma látható a számla megszakadnak. Üzleti és a webes adatbázis továbbra is az adatbázis mérete alapján adatbázis egységek használatával számlát kapni. Kérjük, keresse fel az [SQL-adatbázis árak, oldal](https://azure.microsoft.com/pricing/details/sql-database/) további információt a árak és az új szolgáltatási rétegek közötti különbségeket.


## <a name="see-also"></a>Lásd még:

[Azure SQL-adatbázis](https://azure.microsoft.com/documentation/services/sql-database/)

[Webes és az üzleti árak](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Szolgáltatási rétegek](sql-database-service-tiers.md)

[Frissítsen az Azure SQL-adatbázis V12](sql-database-upgrade-server-portal.md)
