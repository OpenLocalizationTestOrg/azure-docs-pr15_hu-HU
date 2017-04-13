<properties
    pageTitle="Hogyan végezze el a rendszergazdai feladatok, például a rendszergazdai jelszó alaphelyzetbe |} Microsoft Azure"
    description="Megtudhatja, hogy miként elvégezhető leggyakoribb felügyeleti SQL-adatbázisban. Ha például rendszergazdai jelszó alaphelyzetbe állítása, megadásának és eltávolítás access."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="rendszergazdai jelszó alaphelyzetbe állítása"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Hogyan végezhetők el a leggyakoribb felügyeleti feladatokat, például az Azure SQL-adatbázis rendszergazdai jelszó alaphelyzetbe állítása
Ez a témakör a Gyorsműveletek használatával adja meg, és távolítsa el az Azure SQL-adatbázis eléréséhez. Részletesebb tájékoztatásért lásd:

- [Adatbázisok és bejelentkezések az Azure SQL-adatbázis kezelése](sql-database-manage-logins.md)
- [Az SQL-adatbázis biztonságossá tétele](sql-database-security.md)
- [Biztonság otthon az SQL Server adatbázismotort és Azure SQL-adatbázis](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Olyan logikai kiszolgáló rendszergazdai jelszó alaphelyzetbe állítása

- Az [Azure-portálon](https://portal.azure.com) kattintson a **SQL-kiszolgálók**, jelölje ki a kiszolgáló a listában, és válassza a **Jelszó alaphelyzetbe állítása**parancsra.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>A kiszolgáló elérésére engedélyezett címek érdekében győződjön meg arról, hogy csak hitelesített IP
- Lásd: [hogyan: SQL-adatbázis tűzfal beállításainak](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>A felhasználói adatbázisban tárolt adatbázis-felhasználók létrehozása
- A [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) utasítás és a [Felhasználók](https://msdn.microsoft.com/library/ff929188.aspx)című cikkben található adatbázis - tétele az adatbázis hordozható.

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>A tárolókban található adatbázis-felhasználók hitelesíteni az Azure Active Directory használatával
- Lásd: az [Azure Active Directory-hitelesítés használatával SQL-adatbázishoz való csatlakozásnál](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>A virtuális fő adatbázist magas szintű jogosultsággal rendelkező felhasználók számára a további bejelentkezések létrehozása
- A [Bejelentkezési létrehozása](https://msdn.microsoft.com/library/ms189751.aspx) utasítással, és az [adatbázisok kezelése](sql-database-manage-logins.md) és Azure SQL-adatbázisban bejelentkezések bejelentkezések kezelése szakaszban részletesebb.
