1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2.  A Jumpbar, a kattintson az **Új** **adatok + tárhely**kattintson, és válassza a **DocumentDB (NoSQL)**.

    ![Képernyőkép: az Azure portálon, kiemelés további szolgáltatások és DocumentDB (NoSQL)](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  

3. Az **Új fiók** lap adja meg a kívánt konfiguráció DocumentDB fiók.

    ![Az új DocumentDB lap képe](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    - Az **azonosító** mezőbe írja be egy nevet, amely azonosítja a DocumentDB fiókot.  Amikor **azonosító** érvényesítése, zöld pipa jelzi megjelenik az **azonosító** mezőbe. Az **azonosító** értéke az állomásnév belül a URI lesz. Az **azonosító** tartalmazhat csak a kisbetűket, számok, és a "-" karakter, és a 3 és 50 karakter közé kell lennie. Figyelje meg, hogy *documents.azure.com* van ellátva, amelynek eredménye változik a DocumentDB fiók végpont lehetőséget választja, végpontjának neve.

    - A **NoSQL API** mezőben jelölje ki a **DocumentDB**.  

    - **Előfizetés**jelölje be az Azure előfizetés DocumentDB fióknak használni kívánt. Ha a fiók csak egy előfizetéssel rendelkezik, a fiók alapértelmezés szerint van jelölve.

    - **Erőforráscsoport**jelölje be, vagy hozzon létre egy erőforrás csoportot a DocumentDB fiók.  Alapértelmezés szerint új erőforráscsoport jön létre. További tudnivalókért lásd: [az Azure portálon kezelheti az Azure erőforrások](../articles/azure-portal/resource-group-portal.md).

    - **Hely** segítségével adja meg, amelyben tárolni a DocumentDB fiók földrajzi helyét. 

4.  Miután az új DocumentDB a Fiókbeállítások van beállítva, kattintson a **Létrehozás**gombra. A telepítés állapotának ellenőrzése, jelölje be az értesítések-központban.  

    ![Hozza létre az adatbázisokat gyorsan - Képernyőkép az értesítések megjelenítése a DocumentDB fiók létrejön-központban:](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  

    ![Képernyőkép: az értesítések, hogy a DocumentDB fiókot sikeresen létre, és üzembe erőforráscsoport – Online adatbázis készítő értesítés-központban](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)

5.  A DocumentDB fiók létrehozását követően az alapértelmezett beállításokkal használatra kész. Az alapértelmezett konzisztencia DocumentDB fiók **munkamenet**értékre van állítva.  Beállíthatja, hogy az alapértelmezett konzisztencia az erőforrás menü **Alapértelmezett konzisztencia** gombra kattintva. További DocumentDB által kínált kapcsolatos összhangot, olvassa el a [DocumentDB konzisztencia szintjén](../articles/documentdb/documentdb-consistency-levels.md)című témakört.

    ![Képernyőkép: az erőforráscsoport lap - alkalmazások fejlesztése lépések](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  

    ![Képernyőkép: a konzisztencia szint lap - munkamenet konzisztencia](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md
