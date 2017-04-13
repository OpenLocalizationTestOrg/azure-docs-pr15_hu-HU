<properties
   pageTitle="Kétmintás T-SQL Azure SQL adatbázis nem támogatott |} Microsoft Azure"
   description="Kisebb, mint támogatott Azure SQL-adatbázisban Transact-SQL-kimutatásokban"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL-adatbázis-Transact-SQL különbségek


A legtöbb a Transact-SQL nyelvben funkciók alkalmazások függő mind a Microsoft SQL Server Azure SQL-adatbázis támogatottak. Részleges alkalmazások támogatott szolgáltatások listáját a következőképpen:

- Adattípusok.
- Operátorok.
- Karakterlánc, a számtani logikai, és a kurzor függvény.

Azure SQL-adatbázis azonban a **fő** adatbázist bármely függés szolgáltatása elkülönítése tervezve. Sok kiszolgálói szintű tevékenység következtében nem a megfelelő SQL-adatbázishoz, és nem támogatottak. Funkciók, amelyek már nem támogatja az SQL Server általában nem támogatott SQL-adatbázisban.

> [AZURE.NOTE]
> Ez a témakör ismerteti, hogy ha frissíti a legújabb verzióra; SQL-adatbázis találhatók a Funkciók SQL-adatbázis V12. V12 kapcsolatos további tudnivalókért lásd: [SQL adatbázis V12 mi meg új](sql-database-v12-whats-new.md).

A következő szakaszokban részben támogatott funkciók és a teljesen nem támogatott funkciókat.


## <a name="features-partially-supported-in-sql-database-v12"></a>Az SQL-adatbázis V12 részben támogatott funkciók

SQL-adatbázis V12 támogatja néhány, de nem az összes az argumentumokat megtalálható a megfelelő SQL Server 2016 Transact-SQL-utasításait. Ha például a CREATE PROCEDURE utasítás érhető el azonban nem érhetők el a CREATE PROCEDURE a beállításokat. Keresse meg a csatolt szintaxis témakörök minden kimutatás a támogatott részeinek kapcsolatban további tájékoztatást.

- Adatbázisok: [létrehozása](https://msdn.microsoft.com/library/dn268335.aspx )/[módosítása](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs mindig elérhető szolgáltatások is elérhetők.
- Függvények: [létrehozása](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER FÜGGVÉNY](https://msdn.microsoft.com/library/ms186967.aspx)
- [TÖRLÉSE](https://msdn.microsoft.com/library/ms173730.aspx) 
- Bejelentkezés: [létrehozása](https://msdn.microsoft.com/library/ms189751.aspx)/[bejelentkezési módosítása](https://msdn.microsoft.com/library/ms189828.aspx)
- Tárolt eljárás: [létrehozása](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Táblák: [létrehozása](https://msdn.microsoft.com/library/dn305849.aspx)/[módosítása](https://msdn.microsoft.com/library/ms190273.aspx)
- Típus (egyéni): [Létrehozása](https://msdn.microsoft.com/library/ms175007.aspx)
- Felhasználók: [létrehozása](https://msdn.microsoft.com/library/ms173463.aspx)/[MÓDOSÍTJA a felhasználói](https://msdn.microsoft.com/library/ms176060.aspx)
- Nézetek: [létrehozása](https://msdn.microsoft.com/library/ms187956.aspx)/[NÉZET módosítása](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>SQL-adatbázisban nem támogatott szolgáltatások

- Objektumok rendezésének módosítása
- Kapcsolódó kapcsolat: végpont kimutatások ORIGINAL_DB_NAME. SQL-adatbázis nem támogatja a Windows-hitelesítés, de a hasonló Azure Active Directory-hitelesítés támogatására. Néhány hitelesítéstípusok szükség SSMS legújabb verzióját. További tudnivalókért olvassa el a [Csatlakozás SQL-adatbázis vagy SQL adatok raktári által használatával Azure Active Directory-hitelesítés](sql-database-aad-authentication.md).
- Az adatbázis-lekérdezések három vagy négy rész nevekkel Cross. (Csak olvasható keresztté adatbázis-lekérdezések támogatott [Rugalmas adatbázis](sql-database-elastic-query-overview.md)-lekérdezéssel.)
- Több adatbázis tulajdonosának láncolás, a megbízható beállítás
- Adatgyűjtő
- Adatbázis-diagramok
- Adatbázis-Mail
- DATABASEPROPERTY (DATABASEPROPERTYEX használja helyette)
- VÉGREHAJTÁS AS bejelentkezések
- Titkosítás: extensible kulcskezelő
- Eseménykezelő: események, a esemény értesítések, a lekérdezés értesítések
- Adatbázis-fájl helyét, méretét és adatbázisfájlok, amely a Microsoft Azure automatikusan kezeli kapcsolódó funkciók.
- Funkciók, amelyek a magas elérhetőségét, a Microsoft Azure-fiókon keresztül kezelik vonatkoznak: készítsen biztonsági másolatot, AlwaysOn adatbázis tükrözés, napló szállítási, helyreállítási módok visszaállításához. További információk, lásd: Azure SQL-adatbázis biztonsági mentése és visszaállítása
- Funkciók, amelyek a napló olvasó futó SQL-adatbázis támaszkodik: leküldéses replikáció, módosítás rögzítése.
- Funkciók, amelyek az SQL Server-Agent vagy az MSDB adatbázis támaszkodik: feladatok, értesítések, operátorok, adatkezelési házirend-alapú, az adatbázis mail, a központi felügyelet alól kiszolgálók.
- FILESTREAM
- Funkciók: fn_get_sql, fn_virtualfilestats, fn_virtualservernodes
- Globális ideiglenes táblák
- Hardveres kapcsolatos kiszolgálójának beállításai: memóriát, dolgozó szálak, Processzor affinitás, követési jelölők stb. Szolgáltatási szintek használja.
- HAS_DBACCESS
- STAT FELADAT TÖRLÉSE
- Csatolt kiszolgálók, LEKÉRDEZÉSMEGNYITÁSA, OPENROWSET, OPENDATASOURCE, TÖMEGES BESZÚRÁSA és a négy részből álló nevek
- Főadat/célhely kiszolgálók
- .NET-keretrendszer [SQL Server CLR integrációja](http://msdn.microsoft.com/library/ms254963.aspx)
- Erőforrás-szabályozó
- Szemantikai keresés
- A kiszolgáló hitelesítő adatai. Adatbázis használata helyett hatóköre a hitelesítő adatait.
- Server alkalmazás szintű elemek: kiszolgálói szerepkörök, IS_SRVROLEMEMBER, sys.login_token. Kiszolgáló webhelycsoportszintű engedélyek néhány helyébe az adatbázis-engedélyek beállítása, ha nem érhetők el. Egyes kiszolgálói szintű DMVs néhány helyébe adatbázis szintű DMVs, ha nem érhetők el.
- Kiszolgáló nélküli express: localdb, a felhasználói példányok
- Szolgáltatás-ügynök
- REMOTE_PROC_TRANSACTIONS BEÁLLÍTÁSA
- LEÁLLÍTÁS
- sp_addmessage
- sp_configure beállítások és KONFIGURÁLJA. Néhány lehetőség áll rendelkezésre [adatbázis hatóköre](https://msdn.microsoft.com/library/mt629158.aspx)konfigurációval VÁLTOZTATJA meg.
- sp_helpuser
- sp_migrate_user_to_contained
- Az SQL Server naplózási. Használja az SQL-adatbázis Képletvizsgálat helyette.
- SQL Server Profilert
- Az SQL Server-nyomkövetés
- Követési jelölők. Egyes nyomkövetési jelző elemeket át lett helyezve a kompatibilis üzemmódot.
- A Transact-SQL nyelvben hibakeresése során
- Eseményindítók: Server – munkafüzetszintű vagy bejelentkezési eseményindítók
- HASZNÁLATI utasítás: másik adatbázist az adatbázis-környezet módosításához az új adatbázist szeretne új kapcsolatot kell végeznie.


## <a name="full-transact-sql-reference"></a>Teljes Transact-SQL referencia

Többet szeretne tudni a Transact-SQL nyelvben nyelvhelyesség-ellenőrzés, használatát és példát tartalmaz olvassa el a [Transact-SQL referencia (adatbázismotor)](https://msdn.microsoft.com/library/bb510741.aspx) az SQL Server könyvek Online című témakört. 

### <a name="about-the-applies-to-tags"></a>A "Vonatkozik" címkék

Az aktuális verzióira SQL Server 2008 kapcsolódó témakörök a Transact-SQL referencia. A cím alatti területre témakör van ikon sáv-, amelyben a négy SQL Server-platform, és jelzi az alkalmazási terület. Rendelkezésre állási csoportok például SQL Server 2012 bevezetett. A [AVAILABILTY csoport létrehozása](https://msdn.microsoft.com/library/ff878399.aspx) a témakör azt jelzi, hogy a kimutatás vonatkozik ** SQL Server (2012 kezdve). A kimutatás nem vonatkozik az SQL Server 2008, SQL Server 2008 R2, SQL Azure-adatbázis, Azure SQL-adatraktár vagy párhuzamos adatraktár.

Egyes esetekben az általános témakör tárgyát programjaiban használható, de a termékek között a kis különbségek vannak. A különbségeket szerepelnek, a középpont felett a témakörben szükség szerint.

