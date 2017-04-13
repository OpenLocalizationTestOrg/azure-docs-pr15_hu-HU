<properties
    pageTitle="Másolja az Azure SQL-adatbázishoz |} Microsoft Azure"
    description="Azure SQL-adatbázishoz másolatának létrehozása"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Másolja az Azure SQL-adatbázis

> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-copy.md)
- [Azure portál](sql-database-copy-portal.md)
- [A PowerShell](sql-database-copy-powershell.md)
- [AZ SQL-T](sql-database-copy-transact-sql.md)

Használhatja az Azure [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md) az SQL-adatbázis másolatot szeretne készíteni. Az adatbázis-példány ugyanazt a technológiát használja, mint a geo replikációs funkciót. De eltérően geo replikációs szünteti meg a replikáció hivatkozás, mint a rendezési fázis befejezése után. A Másolás adatbázis ezért a forrásadatbázis kezdve az idő a Másolás kérelem pillanatképét.  
Az adatbázis másolatot készíthet ugyanazon a kiszolgálón vagy egy másik kiszolgálóra. A szolgáltatás réteg és a teljesítmény szint (árak réteg) az adatbázis-példány ugyanazok, mint a forrásadatbázis alapértelmezés szerint. Amikor az API-t, választhat egy másik teljesítményszint belül az azonos szolgáltatási réteg (kiadás). A másolás befejeződött, a másolatot teljes funkcionalitású, független adatbázis lesz. Ezen a ponton frissítése vagy bármely Edition vissza léptetheti azt. A bejelentkezések, a felhasználók és engedélyek kezelheti önállóan.  

Adatbázis ugyanarra a logikai kiszolgálóra másolásakor a azonos bejelentkezések mindkét adatbázisra használható. A rendszerbiztonsági tag segítségével másolja a vágólapra az adatbázis lesz az adatbázis tulajdonosa (DBO) az új adatbázis. Adatbázis-felhasználók, az engedélyek és a biztonsági azonosítók az adatbázis-példányt másolja.  

Adatbázis egy másik logikai kiszolgáló másolásakor a a rendszerbiztonsági tag az új kiszolgálón lesz az adatbázis tulajdonosának meg az új adatbázist. Ha a [felhasználó az adatbázisban tárolt](sql-database-manage-logins.md) az adatokhoz való hozzáférés gondoskodhat arról elsődleges és másodlagos adatbázisok mindig a felhasználó hitelesítő adatait, másolás befejeződése után azonnal hozzáférhet a hitelesítő adatait a. [Azure Active Directory](../active-directory/active-directory-whatis.md)használata esetén a hova másolja a hitelesítő adatok kezelése kell teljesen kizárható. Azonban másolásakor az adatbázis egy új kiszolgáló a bejelentkezési-alapú hozzáférés általában nem működik, mert a bejelentkezések nem jönnek létre az új kiszolgálón. [Azure SQL-adatbázis biztonsági után vészhelyreállítás kezelése](sql-database-geo-replication-security-config.md) című témakörben talál tudnivalókat bejelentkezések adatbázis egy másik logikai kiszolgálóra másolásakor témakörben találhat. 

Másolja az SQL-adatbázissal, a következőkre lesz szüksége:

- Egy Azure-előfizetést. Ha van szüksége az Azure előfizetéssel egyszerűen **Ingyenes PRÓBAVERZIÓT** , ez a lap tetején kattintson, és ezután térjen vissza az Ez a cikk Befejezés.
- SQL-adatbázis másolásához. Ha nem rendelkezik egy SQL-adatbázissal, hozzon létre egy az ebben a cikkben leírt lépéseket követve: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).

## <a name="next-steps"></a>Következő lépések

- Lásd: [az Azure portálon Azure SQL-adatbázisból másolása](sql-database-copy-portal.md) az Azure portálon adatbázis másolása.
- Lásd: a PowerShell használatá adatbázis másolása [Azure SQL-adatbázishoz másolása a PowerShell használatával](sql-database-copy-powershell.md) .
- Lásd: a [Másolás egy Azure SQL-adatbázis használata az SQL-T](sql-database-copy-transact-sql.md) a Transact-SQL-adatbázisokkal másolásához.
- [Azure SQL-adatbázis biztonsági után vészhelyreállítás kezelése](sql-database-geo-replication-security-config.md) című témakörben talál tudnivalókat a felhasználók és a bejelentkezés egy másik logikai-kiszolgáló adatbázis másolásakor témakörben találhat.



## <a name="additional-resources"></a>További források

- [Bejelentkezés kezelése](sql-database-manage-logins.md)
- [Az SQL Server Management Studio SQL-adatbázis csatlakoztatása, és hajtsa végre a minta T az SQL lekérdezés](sql-database-connect-query-ssms.md)
- [Az adatbázis exportálása egy BACPAC](sql-database-export.md)
- [Üzleti folytonosságot – áttekintés](sql-database-business-continuity.md)
- [SQL-adatbázis dokumentáció](https://azure.microsoft.com/documentation/services/sql-database/)
