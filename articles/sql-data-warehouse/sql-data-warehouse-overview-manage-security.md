<properties
   pageTitle="Az SQL adatraktár az adatbázis védelmét |} Microsoft Azure"
   description="Tippek az Azure SQL-adatraktár adatbázisok biztonságossá megoldások fejlesztésére."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Az SQL adatraktár az adatbázis védelmét

> [AZURE.SELECTOR]
- [Biztonsági – áttekintés](sql-data-warehouse-overview-manage-security.md)
- [Hitelesítés](sql-data-warehouse-authentication.md)
- [Titkosítás (Portal)](sql-data-warehouse-encryption-tde.md)
- [Titkosítás (T – SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Az alábbiakban ismertetjük az Azure SQL-adatraktár adatbázis biztonságossá tétele alapjait. Különösen ez a cikk segítséget nyújtanak az forrásokból, a hozzáférés korlátozása a adatok védelme és a tevékenység egy adatbázisban ellenőrzése.

## <a name="connection-security"></a>Kapcsolat biztonsága

Kapcsolat biztonsági hogyan korlátozása és a biztonságos kapcsolatok az adatbázishoz tűzfalszabályokat és a kapcsolat titkosítása hivatkozik.

Tűzfalszabályokat a kiszolgáló és az adatbázis által használt IP-címeket, amelyek nem lettek kifejezetten whitelisted a kapcsolatfelvételi elutasításához. Engedélyezi a kapcsolatokat a alkalmazásból vagy a ügyfél gépi nyilvános IP-címet, először létre kell hoznia egy kiszolgálói szintű tűzfal szabályt az Azure Portal, a REST API-t vagy a PowerShell használatával. Legjobb módszer mely szerint a kiszolgáló tűzfalon keresztül engedélyezett IP-címtartományokat korlátozni.  Azure SQL-adatraktár eléréséhez a helyi számítógépről, győződjön meg róla, a tűzfal a hálózati és a helyi számítógépen lehetővé teszi, hogy a kimenő kommunikáció 1433 portot.  További tudnivalókért lásd: [Azure SQL-adatbázis tűzfal][], [sp_set_firewall_rule][]és [sp_set_database_firewall_rule][].

Az SQL adatraktár kapcsolatok alapértelmezés szerint titkosított.  A függvény figyelmen kívül hagyja a titkosítási letiltása módosítása kapcsolatbeállításokat.

## <a name="authentication"></a>Hitelesítés

Hitelesítés hogyan, a személyazonosság az adatbázishoz való csatlakozáskor hivatkozik. SQL-adatraktár jelenleg támogatott SQL Server-hitelesítés a felhasználónevet és jelszót, valamint egy Azure Active Directory. 

Az adatbázis létrehozásakor a logikai kiszolgáló meghatározott "kiszolgáló felügyeleti" bejelentkezési felhasználónév és jelszó. Ezek a hitelesítő adatok használata esetén hitelesítheti az adatbázis tulajdonosa, vagy a "dbo" SQL Server-hitelesítés keresztül kiszolgálón tárolt bármely adatbázishoz.

Jó helyen jár a legjobb módszer a szervezet felhasználói kell használni egy másik fiókba hitelesítést végezni. Ezzel a módszerrel korlátozhatja az engedélyeket az alkalmazás és a kártékony tevékenység kockázatok csökkentése abban az esetben, ha az alkalmazás kódja téve egy SQL-utasítások beszúrását szándékú a. 

Az SQL Server hitelesített felhasználó létrehozása, a **Diaminta** a kiszolgáló felügyeleti jelentkezzen be a kiszolgáló-adatbázis csatlakoztatása, és hozzon létre egy új kiszolgáló bejelentkezést.  Emellett célszerű az felhasználó létrehozása az Azure SQL-adatraktár felhasználók fő adatbázis. A felhasználó létrehozásának minta lehetővé teszi a felhasználóknak bejelentkezési eszközeivel például SSMS nélkülire az adatbázis nevét.  Lehetővé teszi az objektum Intézővel adatbázisokra meg az SQL server őket.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Ezután a kiszolgáló felügyeleti jelentkezzen be az **adatraktár SQL-adatbázis** csatlakoztatása, és hozzon létre egy adatbázis felhasználót az imént létrehozott server bejelentkezés alapján.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Ha egy felhasználó a további műveletek, például bejelentkezések létrehozása vagy létrehozása új adatbázis mindaddig végre, azok is kell legyenek hozzárendelve a `Loginmanager` és `dbmanager` szerepkörök a fő adatbázist. Amit szerepkörökről és az SQL-adatbázishoz hitelesíteni a további tudnivalókért lásd: az [adatbázisok kezelése és bejelentkezések Azure SQL-adatbázisban][].  Az adatraktár SQL Azure AD a további információra kíváncsi olvassa el a [Csatlakozás az SQL adatok raktári által használatával Azure Active Directory-hitelesítés][].


## <a name="authorization"></a>Engedély

Engedély utal, mire az Azure SQL-adatraktár adatbázis belül, és ez szabályozza a felhasználói fiók szerepköreit és engedélyeit. Legjobb módszer akkor érdemes engedélyezése a felhasználóknak a legalacsonyabb szükséges jogosultságokkal. Azure SQL-adatraktár megkönnyíti ezt az SQL-T a szerepkör kezelése:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

A kiszolgáló felügyeleti fiókja csatlakozik a tagja db_owner, amelynek hitelesítésszolgáltató valami az adatbázisból. Ehhez a fiókhoz, az üzembe helyezése a séma frissítések és más adatkezelési műveletek menteni. Korlátozott engedélyű "ApplicationUser"-fiókom segítségével kapcsolatba az adatbázis az alkalmazásból a legalacsonyabb jogosultság szükség szerint az alkalmazás.

További korlátozhatja a felhasználó mire alkalmas az Azure SQL-adatbázis módja van:

- Részletes [engedélyek][] vezérelhet milyen műveleteket végezheti el a egyes oszlopok, táblák, nézetek, eljárások és az adatbázis más objektumaihoz. Részletes engedélyek használata a legtöbb hozzáféréssel rendelkezik, és adja meg a minimálisan szükséges engedélyekkel. A részletes jogosultsági rendszer némileg bonyolult, és néhány tanulmányi hatékony használatára van szükség.
- [Adatbázis szerepkörök][] db_datareader és db_datawriter nem nagyobb teljesítményű alkalmazás felhasználói fiókokat vagy kisebb hatékony kimutatásokat létrehozására használható. A beépített rögzített adatbázis szerepkörök ugyanígy engedélyeket szeretne adni a szükséges, de eredményezhet engedélyek több, mint amennyi szükséges.
- [A tárolt eljárások][] használható korlátozhatja a műveleteket, amelyeket az adatbázis meg lehessen hozni.

A portál felhasználói fiók szerepkör-hozzárendelés adatbázisok és logikai kiszolgálók kezelése az Azure klasszikus portálról, vagy a Azure erőforrás-kezelő API szabályozza. Ez a témakör a további tudnivalókért lásd: [Azure portál szerepköralapú hozzáférés-vezérlés][].

## <a name="encryption"></a>Titkosítás:

Azure SQL adatok raktári átlátszó adatok titkosítási (TDE) segítségével rosszindulatú tevékenység veszélyével vírusvédelmet valós idejű titkosítás és a többi adat visszafejtés hajt végre.  Ha az adatbázis titkosítása esetén tartozó másolatok és tranzakció naplófájlok anélkül, hogy az alkalmazások módosításainak titkosított. TDE titkosítja a teljes adatbázisra tárolására nevű adatbázist titkosítókulcs szimmetrikus kulcs használatával. SQL-adatbázisban az adatbázis titkosítókulcs védi beépített kiszolgálói tanúsítvány. A beépített kiszolgálói tanúsítvány az egyes SQL-adatbázis kiszolgálója egyedi. A Microsoft hozzávetőlegesen minden 90 napon automatikusan elforgatása tanúsítványok. Az SQL adatraktár által használt titkosítási algoritmust AES-256. TDE általános leírása olvassa el a [Átlátszó adatok titkosítása][]című témakört.

Az adatbázis az [Azure-portálon] titkosíthatja[ Encryption with Portal] vagy [Az SQL-T][Encryption with TSQL].

## <a name="next-steps"></a>Következő lépések

Részletek és példát tartalmaz az SQL adatraktár különböző protokollokat a való csatlakozással kapcsolatban olvassa el a [Csatlakozás az SQL adatraktár][].

<!--Image references-->

<!--Article references-->
[Csatlakozás SQL adatraktár]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Kapcsolódás adatraktár SQL Azure Active Directory-hitelesítés használatával]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL-adatbázis tűzfal]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Adatbázis-szerepkörök]: https://msdn.microsoft.com/library/ms189121.aspx
[Adatbázisok és bejelentkezések az Azure SQL-adatbázis kezelése]: https://msdn.microsoft.com/library/ee336235.aspx
[Engedélyek]: https://msdn.microsoft.com/library/ms191291.aspx
[Tárolt eljárás]: https://msdn.microsoft.com/library/ms190782.aspx
[Áttetsző adatok titkosítása]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Azure portál szerepköralapú hozzáférés-vezérlés]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
