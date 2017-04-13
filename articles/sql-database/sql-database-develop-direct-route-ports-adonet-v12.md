<properties 
    pageTitle="Portok túl az SQL-adatbázis 1433 |} Microsoft Azure"
    description="Azure SQL-adatbázis V12 a ADO.NET-ügyfél kapcsolatot előfordul, hogy a proxy átlépése, és közvetlenül az adatbázis használhatják. Portokon kívül 1433 fontos válnak."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>ADO.NET 4.5 és SQL-adatbázis V12 1433 túl portok


Ez a témakör ismerteti az Azure SQL-adatbázis V12 ADO.NET 4.5 vagy újabb verzióját használó ügyfelek kapcsolat viselkedését magasabbra, hogy a módosításokat.


## <a name="v11-of-sql-database-port-1433"></a>SQL-adatbázis v11: Port 1433


Az ügyfélprogram ADO.NET 4.5 való kapcsolódáshoz és a lekérdezés SQL-adatbázis V11, a belső sorrendjének esetén az alábbi képlettel történik:


1. ADO.NET megpróbálja SQL-adatbázishoz való csatlakozáshoz.

2. ADO.NET portot 1433 használja, ha fel szeretne hívni egy köztes modulra, és a köztes SQL-adatbázishoz csatlakozik.

3. SQL-adatbázis visszaküldi, a választ a köztes, amely továbbítja a porthoz 1433 ADO.NET választ.


**Terminológia:** Az előző sorozat azt ismertetik, által közli, hogy ADO.NET hogyan kommunikáljon a SQL-adatbázis *proxy irányítása*használatával. Nincs köztes lenne szó, ha azt szeretné fel, hogy a *közvetlen átirányításához* használt.


## <a name="v12-of-sql-database-outside-vs-inside"></a>SQL-adatbázis V12: belüli és kívüli


V12-kapcsolatot azt kell kérnie, hogy az ügyfélprogram fut *belüli* vagy *kívüli* az Azure felhő oszlopazonosító. Megtudhatja, hogy a részek két gyakori alkalmazási területek.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Kívül:* Ügyfél az asztali számítógépén futó fut


1433 portja a csak olyan portot, hogy meg kell nyitni az asztali számítógépén futó tárolja az SQL-adatbázis ügyfélalkalmazás.


#### <a name="inside-client-runs-on-azure"></a>*Belül:* Ügyfél Azure fut


Az ügyfelek belül az Azure felhő oszlopazonosító futtatásakor használja, ezeket is hívjuk *közvetlen útvonal* vezérléséhez az SQL-adatbázis kiszolgálója. Miután a kapcsolat létrejött, további kapcsolati az ügyfél és az adatbázis között nincs köztes proxy járnak.


A sorrend értéke az alábbi képlettel történik:


1. ADO.NET 4,5 (vagy újabb verzió) kezdeményez egy rövid interakció Azure a felhőben, és egy dinamikusan azonosított portszámot kap.
 - Dinamikusan azonosított portszámot 11000-11999 vagy 14000-14999 a tartományban van.

2. ADO.NET, majd csatlakozik hozzá az SQL-adatbázis azon kiszolgálóját, közvetlenül a nincs köztes a kettő között.

3. Lekérdezések közvetlenül az adatbázis küldött, és a visszaadott eredmény közvetlenül az ügyfél számára.


Győződjön meg arról, hogy a port 11000-11999 és 14000-14999 az Azure ügyfélgépen cellatartományok rendelkezésre ADO.NET 4.5 ügyfél szolgáltatásainkban folytatott tevékenységét követve SQL-adatbázis V12 maradnak.

- A tartomány portok különösen más kimenő blokkolókat ingyenes kell lennie.

- Kattintson az Azure virtuális a **Fokozott biztonságú Windows tűzfal** szabályozza a portbeállításokat.
 - A [tűzfal felhasználói felület](http://msdn.microsoft.com/library/cc646023.aspx) hozzáadása egy szabályt, amelynek meg egy porttartományt együtt **TCP** protokollt, például **11000-11999**az alábbi is használhatja.


## <a name="version-clarifications"></a>Verzió magyarázata


Ebben a szakaszban a termék verziója hivatkozó kézjegyek értelmezi. A termékek között a verziók néhány párosítása is tartalmazza.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 TDS 7.3 protokollt, de nem 7.4 támogatja.
- ADO.NET 4,5 vagy újabb verzió a TDS 7.4 protokollt támogató.


#### <a name="sql-database-v11-and-v12"></a>SQL-adatbázis V11 és V12


Ez a témakör az ügyfél kapcsolat SQL-adatbázis V11 és különbségeinek V12 megjelenik kiemelve.


*Megjegyzés:* A Transact-SQL-utasítást `SELECT @@version;` értéket adja vissza egy szám, például "11." kezdetű vagy "12.", és ezeket felel meg a V11 és V12 verzió azoknak az SQL-adatbázishoz.


## <a name="related-links"></a>Kapcsolódó hivatkozások


- ADO.NET 4.6 a 2015 július 20 jelent meg. A blog bejelentése .NET csapattag érhető el [az alábbi](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4.5 a 2012 augusztus 15 jelent meg. A blog bejelentése .NET csapattag érhető el [az alábbi](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - ADO.NET 4.5.1 kapcsolatos blogbejegyzésbe érhető el [az alábbi](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [TDS protokoll verzió lista](http://www.freetds.org/userguide/tdshistory.htm)


- [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)


- [Azure SQL-adatbázis tűzfal](sql-database-firewall-configure.md)


- [Útmutató: a SQL-adatbázis tűzfal beállításainak konfigurálása](sql-database-configure-firewall-settings.md)

