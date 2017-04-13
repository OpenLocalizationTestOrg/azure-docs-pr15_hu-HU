<properties
    pageTitle="Csatlakozás SQL-adatbázis Excel |} Microsoft Azure"
    description="Útmutató a Microsoft Excel csatlakozni a felhőben Azure SQL-adatbázishoz. Adatok importálása az Excel-jelentése és az adatok feltárása."
    services="sql-database"
    keywords="Csatlakozás SQL az excel, az excel-adatok importálása"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>SQL-adatbázis oktatóprogram: az Excel csatlakozni Azure SQL-adatbázishoz, és a jelentés létrehozása

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

További információ az Excel csatlakozni a felhőben SQL-adatbázishoz, importálni az adatokat, és hozzon létre a táblákat és diagramokat az adatbázis értékek alapján. Ebben az oktatóanyagban fog az Excel és egy adatbázis tábla közötti kapcsolatot beállítani mentse a fájlt tároló adatok és a kapcsolat adatait az Excel, és kattintson a kimutatás diagram létrehozása az adatbázis-értékeinek.

Megkezdése előtt kell az Azure SQL-adatbázishoz. Ha nincs telepítve egyik, lásd: [az első SQL-adatbázis létrehozása](sql-database-get-started.md) fel mintaadatokat, és néhány perc fut adatbázis eléréséhez. Ebben a cikkben fogja minta adatokat importál az Excel a cikkben, de lépésekkel hasonló meg a saját adatain.

Az Excel másolatának is szüksége lesz. Ez a cikk a [Microsoft Excel 2016-ban](https://products.office.com/en-US/)használja.

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Az Excel SQL-adatbázishoz hozhat létre, és odc-fájl

1.  Az Excel csatlakozni az SQL-adatbázishoz, nyissa meg az Excelt és hozzon létre egy új munkafüzetet vagy megnyithat egy meglévő Excel-munkafüzetet.

2.  A menüsávon kattintson a lap tetején kattintson az **adatok**, kattintson **A más forrásokból**, és válassza az **SQL Server**parancsra.

    ![Adatforrás kijelölése: Excel Csatlakozás SQL-adatbázishoz.](./media/sql-database-connect-excel/excel_data_source.png)

    Ekkor megnyílik az Adatkapcsolat varázsló.

3.  **Adatbázis-kiszolgálóhoz való csatlakozás** párbeszédpanelen írja be a SQL-adatbázis- **kiszolgáló neve** , amelyhez csatlakozni szeretne, a képernyőn <*kiszolgálónév*>**. database.windows.net**. Ha például **adworkserver.database.windows.net**.

4.  **Jelentkezzen be hitelesítő adatait**jelölje be **az alábbi felhasználónév és jelszó használata**csoportban írja be a **Felhasználónevet** és **jelszót** állíthat be az SQL-adatbázis kiszolgálója a létrehozása után, és kattintson a **Tovább gombra**.

    ![Írja be a kiszolgáló neve és a bejelentkezési hitelesítő adatok](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] Attól függően, hogy a hálózati környezetben, előfordulhat, hogy nem tud csatlakozni, vagy elveszhetnek a kapcsolatot, ha az SQL-adatbázis kiszolgálója nem engedélyezi az ügyfél IP-címének érkező forgalmat. Az [Azure portál](https://portal.azure.com/), SQL-kiszolgálók kattintson, kattintson a kiszolgálóra, tűzfal kattintson a beállítások és az ügyfél IP-címének hozzáadása. Megtudhatja, [hogy miként tűzfal beállításainak](sql-database-configure-firewall-settings.md) további információt.

5. Az **adatbázis és tábla kijelölése** párbeszédpanelen jelölje ki az adatbázist a listából a használni kívánt, és kattintson a a táblák vagy nézetek, amellyel dolgozni szeretne (választottunk **vGetAllCategories**), majd kattintson a **Tovább**gombra.

    ![Egy adatbázis és tábla kijelölése.](./media/sql-database-connect-excel/select-database-and-table.png)

    Az **Adatkapcsolatfájl mentése és Befejezés** párbeszédpanel, ahol adja meg az Office adatbázis-kapcsolatot (*.odc) fájlt Excel használó információt. Hagyja az alapértelmezett beállításokat, és testre szabhatja a megfelelő beállításokat.

6. Az alapértelmezett hagyja, de különösen figyelje meg a **Fájl nevét** . Egy **leírást**, egy **Rövid nevet**, és a **Keresési kulcsszavakat** segít, és más felhasználók ne feledje, hogy mit kapcsolódik és keresse meg a kapcsolatot. Kattintson a **mindig próbálja használni a Adatfrissítés a fájl** , ha azt szeretné, hogy a kapcsolat adatait tárolja a ODC-fájl, így azt frissítheti, amikor csatlakozni, és kattintson a **Befejezés gombra**.

    ![Odc-fájl mentése](./media/sql-database-connect-excel/save-odc-file.png)

    Az **adatok importálása** párbeszédpanel jelenik meg.

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Az adatok importálása az Excel és a PowerPivot diagram létrehozása
Most, hogy létrehozta a kapcsolatot, és és a csatlakozási adatok hozta létre a fájlt, az épp olvasott szeretné importálni az adatokat.

1. **Adatok importálása** párbeszédpanelen kattintson a kapcsolatos adatok a munkalapon a kívánt beállítást, és kattintson **az OK**gombra. **Kimutatásdiagram**választottunk. Úgy is dönt, hogy hozzon létre egy **új munkalapot** vagy felvétele **adatmodellbe ezeket az adatokat**. Az adatmodellek kapcsolatos további tudnivalókért lásd: [az Excel programban adatmodell létrehozása](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Kattintson a **Tulajdonságok** feltárása az előző lépésben létrehozott ODC-fájl információt, és válassza az Adatfrissítés a beállításokat.

    ![Az Excel adatok formátumának kiválasztása](./media/sql-database-connect-excel/import-data.png)

    A munkalap ekkor megjelenik egy üres kimutatás és diagram.

8. **A kimutatásmezők**területen jelölje be az összes-jelölőnégyzetet a mezők meg szeretné tekinteni.

    ![Állítsa be az adatbázis-jelentés.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Ha az adatbázis más Excel-munkafüzetek és munkalapok csatlakozni szeretne, kattintson az **adatok**, kattintson a **kapcsolatok**gombra, kattintson a **Hozzáadás**gombra, válassza ki a listában létrehozott kapcsolatát, és kattintson a **Megnyitás**.
> ![Nyisson meg egy kapcsolatot a másik munkafüzetből](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan speciális lekérdezése és elemzéshez [Csatlakozás SQL Server Management Studio az SQL-adatbázishoz](sql-database-connect-query-ssms.md) .
- Tudjon meg többet a [rugalmas készletek](sql-database-elastic-pool.md)előnyeit.
- Megtudhatja, hogy miként hozhat [létre, amely a háttéradatbázist a SQL-adatbázishoz csatlakozik webalkalmazás](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
