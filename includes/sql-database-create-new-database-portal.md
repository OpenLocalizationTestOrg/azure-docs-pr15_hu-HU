
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Hozzon létre egy új Azure SQL-adatbázishoz

Új Azure SQL-adatbázis létrehozása új vagy meglévő Azure SQL-adatbázis logikai kiszolgálón kövesse az alábbi lépéseket az Azure-portálon.

1. Ha éppen nem csatlakozik, az [Azure portál](http://portal.azure.com)csatlakozni.
2. Kattintson az **Új**gombra, írja be az **SQL-adatbázishoz**, és válassza az **SQL-adatbázis (új adatbázis)**.

     ![Új adatbázis esetén](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Kattintson az **SQL-adatbázis (új adatbázis)**.

     ![Új adatbázis esetén](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Kattintson a **Létrehozás** hozzon létre új adatbázist az SQL-adatbázis szolgáltatásban.

     ![Új adatbázis esetén](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. A szükséges értékeket a következő kiszolgáló tulajdonságai:

 - Az adatbázis neve
 - Előfizetés: Ez a érvényes csak akkor, ha több előfizetéssel rendelkezik.
 - Erőforráscsoport: Ha, most kezdett, használja az erőforráscsoport logikai kiszolgáló.
 - Jelölje be a forrás: megadhatja, üres adatbázist, a mintaadatok vagy az Azure-adatbázis biztonsági mentése. Egy helyszíni SQL Server-adatbázis áttelepítése vagy adat betöltése az BCP parancssori eszközzel, akkor olvassa el a hivatkozásokat, ez a cikk végén.
 - Kiszolgáló: Egy új vagy meglévő logikai kiszolgáló.
 - Bejelentkezés a kiszolgálóra rendszergazda
 - Jelszó
 - Árak réteg: Ha, most kezdett, használja az alapértelmezett érték S0.
 - Rendezési sorrend: Ez csak akkor, ha egy üres adatbázis választott vonatkozik.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Kattintson a **létrehozása**gombra. Az értesítési területen megtekintheti, hogy telepítési munkatársa.

     ![Új adatbázis esetén](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Várja meg, a telepítés befejezéséhez, mielőtt a következő lépéssel folytatná.

     ![Új adatbázis esetén](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
