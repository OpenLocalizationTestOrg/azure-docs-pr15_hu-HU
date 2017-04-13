<properties
    pageTitle="SQL-adatbázis V12 újdonságai |} Microsoft Azure"
    description="Ismerteti, hogy miért előnyei a Azure SQL-adatbázis használják a felhőben business rendszerek-verzióra való frissítésről V12 most szerint."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>SQL-adatbázis V12 újdonságai


Ez a témakör ismerteti a számos előnye, az új V12 Azure SQL-adatbázis-verziót tartalmazó V11 verziójára.


Szolgáltatások hozzáadása V12 továbbra is. Így javasoljuk, látogasson el az Azure a szolgáltatásfrissítések weblapra, és a szűrők használata:


- A szűrt az [SQL-adatbázis-szolgáltatás](https://azure.microsoft.com/updates/?service=sql-database).
- A szűrt általános elérhetőség [(kiadás) hirdetmények](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) az SQL-adatbázis-szolgáltatásokra.


SQL-adatbázis erőforrás korlátai kapcsolatos legfrissebb információkat a jelent meg:<br/>[Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>SQL Server-megnövelt kompatibilitási


A fő cél SQL-adatbázis V12 a Microsoft SQL Server 2014-es kompatibilitásának javítása és a kompatibilitás, mint az SQL Server új verzióit kiadott volt. Más különböző területei között V12 éri el a eltérés áll fenn, SQL Server-programozási fontos területén. Példa:

- [Beépített JSON-támogatás](https://msdn.microsoft.com/library/dn921897.aspx)

- [Ablak függvények](http://msdn.microsoft.com/library/ms189798.aspx), [keresztül](http://msdn.microsoft.com/library/ms189461.aspx)

- [XML-indexek](http://msdn.microsoft.com/library/bb934097.aspx) és [szelektív XML indexek](http://msdn.microsoft.com/library/jj670104.aspx)

- [Változások követése](http://msdn.microsoft.com/library/bb933875.aspx)

- [VÁLASSZA A LEHETŐSÉGET. AZ](http://msdn.microsoft.com/library/ms188029.aspx)

- [Teljes szöveges keresés](http://msdn.microsoft.com/library/ms142571.aspx)

- [MEGVÁLTOZTATHATJA az adatbázis LAPSZINTŰ KONFIGURÁLÁSA (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Lásd: [Itt](sql-database-transact-sql-information.md) az kis SQL-adatbázisban még nem támogatott szolgáltatások számára.


### <a name="compatibility-level-130"></a>Kompatibilitási szintet 130


> [AZURE.IMPORTANT] *Újonnan* létrehozott adatbázisok Azure SQL-adatbázis V12 **június 2016**-ban indulva van a kezdőpont 130, amely megfelel a Microsoft SQL Server 2016 GA kompatibilitási szintjük
> 
> Használható `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` Ha inkább.
> 
> Június 2016 előtt létrehozott adatbázisok nem rendelkezik a kompatibilitási szintjük módosította az alapértelmezés módosítása. És sem a V11 áttérni V12 módosította adatbázis szintjét.



Hogyan összevetheti magyarázata között a legújabb előző kompatibilitási szintet, és a legfontosabb lekérdezések talál:

- [Továbbfejlesztett lekérdezési teljesítmény kompatibilitási szinthez 130 Azure SQL-adatbázisban](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>További támogatási teljesítmény elérése érdekében új teljesítményszint


V12, az összes prémium teljesítményszint rendelt külön költség nélkül 25 %-kal adatbázis tranzakció egységek (DTUs) nagyobb azt. Még nagyobb teljesítmény nyereség elérhető-e olyan új funkciókat, például:


- A memóriában [columnstore indexek](http://msdn.microsoft.com/library/gg492153.aspx)támogatása.
- A [táblázat szétválasztás sorok szerint](http://msdn.microsoft.com/library/ms187802.aspx) kapcsolódó többletfunkciókkal [CSONKOLÁSA](http://msdn.microsoft.com/library/ms177570.aspx)táblázatra.
- Dinamikus kezelése az elérhető nézetek [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) figyelésére és teljesítményének finomhangolása.


### <a name="reliable-performance"></a>Megbízható teljesítmény


Ha az ügyfélprogram SQL-adatbázis V12 csatlakozik, az ügyfél-Azure virtuális gépen (virtuális) futása közben, meg kell nyitnia az alábbi port tartományokat a virtuális meg:

- 11000-11999
- 14000-14999


Kattintson [ide](sql-database-develop-direct-route-ports-adonet-v12.md) a portokat SQL-adatbázis V12 kapcsolatban további tájékoztatást. Az SQL-adatbázis V12 teljesítménnyel kapcsolatos fejlesztések a portokat szükséges.


## <a name="better-support-for-cloud-saas-vendors"></a>Hatékonyabb támogatása felhő szoftver szállítók


Csak a V12 adtunk ki az új szabványos teljesítményszint S3 és a nyilvános [Rugalmas adatbázis készletek](sql-database-elastic-pool.md)előnézetét. Rugalmas adatbázis készletek felhő szoftver szállítók készült megoldást.  Rugalmas adatbázis-készletek van lehetősége:


- Megosztás DTUs adatbázisok között az adatbázisok nagyszámú költségek csökkentése érdekében.
- A méretezés-adatbázisok kezelése [Rugalmas adatbázis feladatok](sql-database-elastic-jobs-overview.md) végrehajtása.


## <a name="security-enhancements"></a>Fejlesztések biztonság


Biztonsági elsődleges szempont bárki, aki a vállalati verzió fut a felhőben. A legújabb biztonsági V12 megjelent funkciók:


- [Sor szintű biztonság](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Dinamikus adatok maszkolás](sql-database-dynamic-data-masking-get-started.md)
- [A tárolókban található adatbázisokat](http://msdn.microsoft.com/library/ff929188.aspx)
- Engedélyek biztosítása, a MEGTAGADÁS [alkalmazás szerepkörök](http://msdn.microsoft.com/library/ms190998.aspx) kezelése, visszavonása
- [Áttetsző adatok titkosítása](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [SQL-adatbázis csatlakoztatása az Azure Active Directory-hitelesítés használatával](sql-database-aad-authentication.md)
 - SQL-adatbázis Azure Active Directory authentication, egy SQL-adatbázis csatlakoztatása az azonosítási az Azure Active Directory (Azure Active Directory) használatával mechanizmusa támogatja. Azure Active Directory-hitelesítéssel az adatbázis-felhasználók és az egyéb Microsoft-szolgáltatásokhoz egy központi helyen identitások központi kezelésére.
- [Mindig titkosított](https://msdn.microsoft.com/library/mt163865.aspx) (előzetes verzió) tesz titkosítás átlátszó alkalmazások, és lehetővé teszi az ügyfelek anélkül, hogy a titkosítási kulcs megosztását SQL-adatbázis titkosítása a ügyfélalkalmazásokban belül bizalmas adatokat.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Üzleti folytonosságot nagyobb a helyreállítási szükségessége esetén


V12 kínál továbbfejlesztett helyreállítási pont célkitűzések (RPOs), és a becsült helyreállítási időtúllépés (ERTs):


| Az üzleti folytonosságot funkciói | Korábbi verziójában | V12 |
| :-- | :-- | :-- |
| GEO-visszaállítása | • Készletben < 24 óra.<br/>• Beszúrása < 12 óra. | • Készletben < 1 óra értéket.<br/>• Beszúrása < 12 óra. |
| Aktív Geo replikációs | • Készletben < 5 percig tart.<br/>• Beszúrása < 1 óra értéket. | • Készletben < 5 másodperc.<br/>• Beszúrása < 30 másodperces. |


[SQL-adatbázis üzemkészségének](sql-database-business-continuity.md) talál további információt.


## <a name="more-reasons-to-upgrade-now"></a>További okok frissítés most


Vannak miért ügyfelek kell frissítés most Azure SQL-adatbázis V12 az V11 sok érv:


- Funkciók V11 funkciók túl hosszú SQL-adatbázis V12 tartalmaz.
- Új funkciók felvétele V12 továbbra is, de nem új szolgáltatások V11 lehet hozzáadni.
- Új a legtöbb funkció vannak fejlesztésének SQL-adatbázis V12, mielőtt azok kiadott a Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Használja V12 már?


Egy egyszerű adatbázis vagy logikai, az SQL-adatbázis-szolgáltatás egy korábbi verzióját futtató kiszolgáló esetén tennivalókat tegye az alábbiakat:


1. Nyissa meg az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **Tallózás gombra**.
3. Kattintson az **SQL-kiszolgálók**.
4. A kiszolgáló vagy az adatbázis mellett lévő ikonra a szövegegység alapján:
 - ![Ikon v12 kiszolgáló](./media/sql-database-v12-whats-new/v12_icon.png) **V12 logikai kiszolgáló**
 - ![Korábbi verzió Server ikon](./media/sql-database-v12-whats-new/earlier_icon.png) **korábbi verzió logikai kiszolgáló**


Annak megállapítása, a verziót a másik módszer akkor futtatásához a `SELECT @@version;` utasítás az adatbázisban, és az eredmények hasonló:


- **12**.0.2000.10 &nbsp; *(V12 verzió)*
- **11**.0.9228.18 &nbsp; *(V11 verzió)*


V12 adatbázis csak a kiszolgálón V12 logikai tárolhatók. És V12 kiszolgáló csak V12 adatbázisok üzemeltetheti.


Ha még nem használ V12, frissíthet a logikai kiszolgáló [SQL-adatbázis V12 helyen frissítése](sql-database-v12-plan-prepare-upgrade.md)című témakör lépéseit követve.


## <a name="V12AzureSqlDbPreviewGaTable"></a>Általános elérhetősége területek


- Július 31-én, 2015 az összes terület előléptetett az általános elérhetőségű kiadás volna lett.
- A decemberi 2014-es, de csak az előnézeti állapotának V12 jelent meg.

[Kiegészítő feltételek használata a Microsoft Azure előnézete](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
