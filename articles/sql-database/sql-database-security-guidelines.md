<properties
   pageTitle="Azure SQL-adatbázis biztonsági irányelveket és érvényes korlátozások |} Microsoft Azure"
   description="Tudjon meg többet a Microsoft Azure SQL-adatbázis irányelvei és biztonsági kapcsolatos korlátai."
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
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Azure SQL-adatbázis biztonsági irányelvei és korlátai

Ez a témakör a Microsoft Azure SQL-adatbázis útmutatások és a kapcsolódó biztonsági korlátozások. Az Azure SQL-adatbázisok biztonsága kezelésekor, vegye figyelembe az alábbiakat.

## <a name="access-to-the-virtual-master-database"></a>A virtuális fő adatbázis elérése

Csak a rendszergazdák általában a fő adatbázist hozzáférésre van szükségük. Minden felhasználó adatbázishoz való rutinszerű hozzáférés létrehozott minden adatbázisban tárolt adatbázis rendszergazdai jogokkal nem rendelkező felhasználók keresztül kell lennie. A tárolókban található adatbázis-felhasználók használatakor nem szükséges bejelentkezések létrehozása a fő adatbázisban. További tudnivalókért lásd: [Tárolt adatbázis felhasználók - tétele az adatbázis hordozható](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Tűzfal

Az SQL Server tűzfalat, amelyek a teljes Azure SQL Server megfelelően változik általában van konfigurálva a portálon keresztül, és a rendszergazdák által használt IP-címek csak kell beengedése. Adatbázisonként szükséges IP-címek megnyíló adatbázis – munkafüzetszintű tűzfal szabály létrehozása, csatlakozás felhasználó adatbázishoz, és majd használja a [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) utasítást a Transact-SQL nyelvben.

Az SQL Azure-adatbázis szolgáltatás csak az portot 1433 keresztül érhető el. SQL-adatbázis elérése a számítógépről, győződjön meg arról, hogy az ügyfél számítógép tűzfal lehetővé teszi, hogy a TCP-port 1433 kimenő TCP kommunikációt. Ha más alkalmazások nem szükséges, blokkolja a TCP-port 1433 bejövő kapcsolatokat. 

A csatlakozási folyamat részeként Azure virtuális gépeken futó kapcsolatokat megnyílik a egy másik IP-címet és a port egyedi minden dolgozó szerepkörhöz. A portszámot 11999 11000 terjedő tartományban található. TCP-portok kapcsolatos további tudnivalókért lásd: a [portok ADO.NET 4.5 és SQL-adatbázis V12 1433 túl](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Hitelesítés

Használja az Active Directory-hitelesítés (integrált adatvédelem) minden lehetséges esetben. Active Directory authentication beállításáról lásd: [Csatlakozás SQL adatbázis által használatával Azure Active Directory-hitelesítés](sql-database-aad-authentication.md), és [egy hitelesítési mód kiválasztása](https://msdn.microsoft.com/library/ms144284.aspx) az SQL Server könyvek Online. 

A tárolókban található adatbázis-felhasználók segítségével méretezhetőség erősítése. További tudnivalókért lásd: [Tárolt adatbázis-felhasználók - tétele az adatbázis hordozható](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL nyelvben)](https://technet.microsoft.com/library/ms173463.aspx)és [Található adatbázisokat](https://technet.microsoft.com/library/ff929071.aspx).

Az adatbázis-vezérlő, amely legfeljebb 30 percig marad üresjárati kapcsolatok bezárása. A kapcsolat bejelentkezési újra kell, mielőtt is használható.

Folyamatosan SQL-adatbázis aktív kapcsolat megkövetelése a reauthorization (végzi az adatbázis-vezérlő) legalább 10 óránként. A adatbázismotor megpróbálja reauthorization az eredeti elküldött jelszót használja, és nincs adatbevitel szükség. Teljesítmény okokból jelszó alaphelyzetbe SQL-adatbázisban, amikor a nincs kapcsolat hitelesíteni, még akkor is, ha a kapcsolat alaphelyzetbe állítása kapcsolatkészletezés miatt. Ez különbözik a helyszíni SQL Server viselkedését. Ha a jelszó módosult, mivel az eredetileg engedélyezték a kapcsolatot, a kapcsolat be kell fejezni, és új kapcsolatot létrehozni az új jelszót használja. Az adatbázis-KAPCSOLATOT törlése jogosult felhasználó explicit módon is megszünteti a kapcsolat SQL-adatbázis [törlése](https://msdn.microsoft.com/library/ms173730.aspx) paranccsal.

## <a name="logins-and-users"></a>Bejelentkezés és a felhasználók

Bejelentkezés, és a felhasználók az SQL-adatbázis kezelése esetén nincsenek korlátozások.


- Csatlakozva kell lennie a **fő** adatbázissal végrehajtásakor az ``CREATE/ALTER/DROP DATABASE`` kimutatások. – A kiszolgálói szintű egyszerű felhasználónév megfelelő fő adatbázisban az adatbázis-felhasználó nem módosítható vagy eltávolítja. 
- Amerikai Egyesült Államok-angol az alapértelmezett nyelv a kiszolgálói szintű fő bejelentkezési.
- Csak a rendszergazdák (kiszolgálói szintű fő bejelentkezési vagy Azure AD-rendszergazdaként) és a **fő** adatbázisban **dbmanager** adatbázis szerepkör tagjai jogosultak hajtsa végre a ``CREATE DATABASE`` és ``DROP DATABASE`` kimutatások.
- Csatlakozva kell lennie a fő adatbázissal végrehajtásakor az ``CREATE/ALTER/DROP LOGIN`` kimutatások. Azonban az bejelentkezések használata javasolt. A tárolókban található adatbázis-felhasználók használni.
- Adatbázishoz szeretne kapcsolódni egy felhasználó, meg kell adnia a kapcsolati karakterláncot az adatbázis nevét.
- Csak a kiszolgálói szintű egyszerű felhasználónév és a **fő** adatbázisban **loginmanager** adatbázis szerepkör tagjai jogosultak hajtsa végre a ``CREATE LOGIN``, ``ALTER LOGIN``, és ``DROP LOGIN`` kimutatások.
- Végrehajtásakor az ``CREATE/ALTER/DROP LOGIN`` és ``CREATE/ALTER/DROP DATABASE`` ADO.NET alkalmazás kimutatások paraméteres parancsaival visszacsatolás nem engedélyezett. További tudnivalókért olvassa el a [Parancsok és a paraméterek](https://msdn.microsoft.com/library/ms254953.aspx)című témakört.
- Végrehajtásakor az ``CREATE/ALTER/DROP DATABASE`` és ``CREATE/ALTER/DROP LOGIN`` kimutatások, minden egyes ezeket az utasításokat a csak utasítást a Transact-SQL nyelvben köteg kell lennie. Egyéb esetben a hiba történik. Például a Transact-SQL ellenőrzi, hogy létezik-e az adatbázist. Ha létezik, egy ``DROP DATABASE`` utasítás nevezik, ha el szeretné távolítani az adatbázist. Mivel a ``DROP DATABASE`` utasítás nem a köteg csak utasításában, ezért a következő végrehajtása Transact-SQL-utasítást hibát eredményez.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Végrehajtásakor az ``CREATE USER`` utasítást a ``FOR/FROM LOGIN`` beállítást választja, a csak utasítást a Transact-SQL nyelvben köteg kell lennie.
- Végrehajtásakor az ``ALTER USER`` utasítást a ``WITH LOGIN`` beállítást választja, a csak utasítást a Transact-SQL nyelvben köteg kell lennie.
- A ``CREATE/ALTER/DROP`` igényel egy felhasználó a ``ALTER ANY USER`` engedéllyel-adatbázisban.
- Ha egy adatbázis-szerepkörnek tulajdonosát próbál létrehozni egy másik adatbázis-felhasználó, illetve a adott adatbázis szerepkör hozzáadása vagy eltávolítása, a következő hiba akkor fordulhat: **felhasználó vagy a "Név" szerepkör nem szerepel ebben az adatbázisban.** Ez a hiba oka az, hogy a felhasználó nem láthatók, a tulajdonos. Ez a probléma megoldásához adja meg a szerepkör tulajdonosa a ``VIEW DEFINITION`` a felhasználó engedélyeinek. 

További információt a bejelentkezési adatok és a felhasználók lásd: az [adatbázisok kezelése és bejelentkezések Azure SQL-adatbázisban](sql-database-manage-logins.md).


## <a name="see-also"></a>Lásd még:

[Azure SQL-adatbázis tűzfal](sql-database-firewall-configure.md)

[Útmutató: tűzfal beállításainak (Azure SQL-adatbázis)](sql-database-configure-firewall-settings.md)

[Adatbázisok és bejelentkezések az Azure SQL-adatbázis kezelése](sql-database-manage-logins.md)

[Biztonság otthon az SQL Server adatbázismotort és Azure SQL-adatbázis](https://msdn.microsoft.com/library/bb510589)
