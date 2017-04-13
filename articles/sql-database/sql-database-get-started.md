<properties
    pageTitle="SQL-adatbázis oktatóprogram: az SQL-adatbázis létrehozása |} Microsoft Azure"
    description="Megtudhatja, hogy miként állíthat be egy logikai SQL-adatbázis azon kiszolgálóját, a kiszolgáló tűzfal szabályt, az SQL-adatbázis és a mintaadatok. Ezenkívül megtudhatja, hogy miként csatlakozhat az ügyféleszközök elől, állítsa be a felhasználók és adatbázis tűzfal szabály beállítása."
    keywords="SQL-adatbázis oktatóanyagban sql-adatbázis létrehozása"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>SQL-adatbázis oktatóprogram: perc egy SQL-adatbázis létrehozása az Azure portál használatával

> [AZURE.SELECTOR]
- [Azure portál](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [A PowerShell](sql-database-get-started-powershell.md)

Ebből az oktatóanyagból megismerheti, hogyan használni szeretné az Azure portált:

- A mintaadatokat tartalmazó Azure SQL-adatbázis létrehozása.
- Egy egyetlen IP-cím vagy az IP-címeket tartalmazó kiszolgálói szintű tűzfal szabály létrehozása.

Ezeket a feladatokat a [C#](sql-database-get-started-csharp.md) vagy a [PowerShell](sql-database-get-started-powershell.md)használatával hajthatják végre.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Az első Azure SQL-adatbázis létrehozása 

1. Ha éppen nem csatlakozik, az [Azure portál](http://portal.azure.com)csatlakozni.
2. Kattintson az **Új**gombra, kattintson az **adatok + tárhely**, és keresse meg az **SQL-adatbázishoz**.

    ![Új 1 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Kattintson az **SQL-adatbázissal** az SQL-adatbázis lap megnyitásához. Ez a lap tartalmának az előfizetések és a meglévő objektumok (például a meglévő-kiszolgálók) számától függően változik.

    ![Új 2 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-2.png)

4. **Az adatbázis neve** mezőbe nevezze el az első adatbázis – például a "saját adatbázis". Egy zöld pipa jelzi, hogy meg van adva egy érvényes nevet.

    ![Új 3 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Ha több előfizetéssel rendelkezik, jelöljön ki egy előfizetést.
6. **Erőforráscsoport**csoportban kattintson a **Létrehozás új** gombra, és nevezze el az első erőforráscsoport – például a "saját-erőforrás-csoport". Egy zöld pipa jelzi, hogy meg van adva egy érvényes nevet.

    ![Új 4 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-4.png)

7. **Jelölje ki a forrást**, csoportban kattintson a **minta** gombra, és kattintson a csoportban **Jelölje be a minta** a **AdventureWorksLT [V12]**gombra.

    ![Új 5 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-5.png)

8. A **kiszolgáló**kattintson a **konfigurálás szükséges beállításai**.

    ![Új 6 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Kattintson a kiszolgáló a lap, **Hozzon létre egy új kiszolgálót**. Azure SQL-adatbázishoz kiszolgáló-objektum, amely lehet egy új vagy meglévő kiszolgáló vagy belül jön létre.

    ![Új 7 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Tekintse át az **Új kiszolgáló** lap meg kell adnia az új kiszolgáló információk megértéséhez.

    ![Új 8 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-8.png)

11. A **kiszolgáló neve** szövegmezőbe nevezze el az első kiszolgáló – például a "saját – új-kiszolgáló-objektum". Egy zöld pipa jelzi, hogy meg van adva egy érvényes nevet.

    ![Új 9 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. A **kiszolgáló felügyeleti bejelentkezési**adja meg a felhasználó nevét a rendszergazdai bejelentkezés kiszolgáló – például a "saját-rendszergazda-fiók". Ez a bejelentkezés egyszerű bejelentkezés kiszolgálóra nevezik. Egy zöld pipa jelzi, hogy meg van adva egy érvényes nevet.

    ![Új 10 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-10.png)

13. A **jelszó** és a **jelszó megerősítése**mezőbe adja meg a jelszót a kiszolgáló fő bejelentkezési fiók – például "p@ssw0rd1". Zöld pipa jelzi, hogy meg van adva a helyes jelszót.

    ![Új 11 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. A **hely**válassza a megfelelő a helyre – például "Ausztrália keleti" adatközpont.

    ![Új 12 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-12.png)

15. A ** létrehozása V12 server (a legújabb frissítésben), megfigyelheti, hogy csak az aktuális verziójának Azure SQL server létrehozása lehetősége van.

    ![Új 13 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Figyelje meg, hogy alapértelmezés szerint a **kiszolgáló elérésére azure engedélyezése** szolgáltatások jelölőnégyzet be van jelölve, és itt nem módosítható. Ez a speciális beállítás. Ezt a beállítást a kiszolgáló objektum kiszolgálóbeállítások tűzfal módosíthatja, bár a találatokkal erre nincs szükség.

    ![Új 14 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Kattintson az új kiszolgáló lap olvassa el a megfelelő beállításokat, és kattintson a **Jelölje ki** a jelölje ki az új adatbázisban a új kiszolgáló.

    ![Új 15 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Az SQL-adatbázis lap **árak réteg**, csoportban a **S2 normál** gombra, és válassza a **egyszerű** választhatja ki az első adatbázis legalább drága árak réteg. A árak réteg később bármikor módosíthatja.

    ![Új 16 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Az SQL-adatbázis lap olvassa el a megfelelő beállításokat, és kattintson a **Create** a első kiszolgáló és az adatbázis létrehozása gombra. A megadott értékek érvényesítése, és üzembe elindítja.

    ![Új 17 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-17.png)

20. A portál eszköztáron kattintson a telepítés állapotának ellenőrzése az **értesítések** elemeket.

    ![Új 18 sql-adatbázis](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Telepítés befejezése után az új Azure SQL-kiszolgáló és az adatbázis létrehozásakor Azure-ban. Nem tud csatlakozni az új kiszolgáló és az adatbázis az SQL Server eszközökkel, amíg nem hoz létre kattintva nyissa meg a kapcsolatok kívüli Azure SQL-adatbázis tűzfal kiszolgáló tűzfal szabályt.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy befejezte az SQL-adatbázis oktatóanyag és létrehozott egy adatbázist tartalmazó, akkor készen is feltárása a kedvenc eszközeit.

- Ha ismeri a Transact-SQL nyelvben és az SQL Server Management Studio (SSMS), megtudhatja, hogyan [Csatlakozás és a lekérdezés SQL-adatbázis SSMS](sql-database-connect-query-ssms.md).

- Ha tudja, hogy az Excel, megtudhatja, hogyan lehet csatlakozni [az Excellel Azure SQL-adatbázishoz](sql-database-connect-excel.md).

- Ha készen áll a kezdésre coding, válassza a [kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md)a programnyelv.

- Lépjen a helyszíni SQL Server-adatbázisok Azure, című témakörben olvashat [SQL-adatbázishoz adatbázis áttelepítése](sql-database-cloud-migrate.md) című témakörben talál további.

- Ha szeretne egy új táblát egy CSV-fájlból az BCP parancssori eszközzel bizonyos adatok betöltése, lásd: [adatok az CSV-fájlba történő BCP SQL-adatbázis betöltés](sql-database-load-from-csv-with-bcp.md).

- Ha szeretné az Azure SQL-adatbázis biztonsági felfedezőút megkezdéséhez, olvassa el a [biztonsági – első lépések](sql-database-get-started-security.md)


## <a name="additional-resources"></a>További források

[Mi az SQL-adatbázishoz?](sql-database-technical-overview.md)
