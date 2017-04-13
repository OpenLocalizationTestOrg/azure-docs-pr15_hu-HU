### <a name="prerequisites"></a>Előfeltételek
- Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre
- Microsoft [Azure SQL-adatbázis](../articles/sql-database/sql-database-get-started.md) együtt a kapcsolat adatait, beleértve a kiszolgáló nevét, az adatbázis neve és felhasználónevével és jelszavával. Ez az információ található az SQL-adatbázis-kapcsolati karakterláncot:
  
    Kiszolgáló = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalógus =*yourqldbname*; Védelmi adatok megtartása = False; Felhasználói azonosító = {your_username}; Jelszó = {your_password}; MultipleActiveResultSets eredménykészleteket = False; Titkosítás = igaz; TrustServerCertificate = False; A kapcsolat időtúllépési = 30.

    További információ az [Azure SQL-adatbázisait](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Amikor létrehoz egy SQL Azure-adatbázis, hozzon létre a minta adatbázisok SQL részét képező. 



Mielőtt az Azure SQL-adatbázis egy logikai alkalmazást használ, az SQL-adatbázis csatlakoztatása. Ezt megteheti egyszerűen a logika alkalmazásból, az Azure portálon.  

Az Azure SQL-adatbázissal, az alábbi lépésekkel csatlakozhat:  

1. Hozzon létre egy logikai alkalmazást. A logikai alkalmazások tervezőben hozzáadása az eseményindító, és adja hozzá az művelet. A legördülő listában válassza a **Microsoft megjelenítése felügyelt API-hoz** , és írja be a keresőmezőbe a "sql". A műveletek közül választhat:  

    ![SQL Azure-kapcsolat létrehozása lépés](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Ha még nem korábban létrehozott minden SQL-adatbázis-kapcsolatot, megjelenik egy kérdés a kapcsolat adatai:  

    ![SQL Azure-kapcsolat létrehozása lépés](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Adja meg az SQL-adatbázis adatait. A tulajdonságok egy csillag szükség.

    | A tulajdonság | Részletek |
|---|---|
| Csatlakozás átjáró keresztül | Hagyja üresen ezt. Ez használatos, amikor csatlakozik egy helyszíni SQL Server. |
| A kapcsolat neve * | Írja be a kapcsolatot egy tetszőleges nevet. | 
| SQL-kiszolgáló neve * | Írja be a kiszolgáló neve; Ez az alábbihoz hasonló *servername.database.windows.net*. A kiszolgáló nevét az Azure-portálon SQL-adatbázis tulajdonságainak jelenik meg, és is megjelennek a kapcsolati karakterlánc. | 
| SQL-adatbázis neve * | Írja be az SQL-adatbázis megadott névnek. Ez a kapcsolati karakterláncot az SQL-adatbázis tulajdonságainak szerepel: kezdeti katalógus =*yoursqldbname*. | 
| Username * | Írja be az SQL-adatbázis létrehozásakor létrehozott felhasználónév. Az Azure-portálon SQL-adatbázis tulajdonságainak szerepel. | 
| Jelszó * | Írja be az SQL-adatbázis létrehozásakor létrehozott jelszót. | 

    Ezek a hitelesítő adatok használatával csatlakozhat, és az SQL-adatok eléréséhez az összefüggés-alkalmazás engedélyezése. Miután elkészült, a kapcsolat adatai az alábbiakhoz hasonló jelennek meg:  

    ![SQL Azure-kapcsolat létrehozása lépés](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Jelölje ki a **létrehozása**. 

5. Értesítés a kapcsolat létrejött. Most folytassa a lépéseket a logika alkalmazásban: 

    ![SQL Azure-kapcsolat létrehozása lépés](./media/connectors-create-api-sqlazure/table.png)