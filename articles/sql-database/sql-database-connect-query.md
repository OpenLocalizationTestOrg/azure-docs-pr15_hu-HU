<properties
    pageTitle="SQL-adatbázis csatlakoztatása a C# lekérdezéssel |} Microsoft Azure"
    description="Írja be a program lekérdezéséhez és SQL-adatbázis csatlakoztatása a C#. IP-címek, kapcsolati karakterláncot, biztonságos bejelentkezés és ingyenes Visual Studio információt."
    services="sql-database"
    keywords="adatbázis SQL-C csatlakoztatása c# adatbázis-lekérdezés, c# lekérdezés#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Csatlakozás a Visual Studio SQL-adatbázishoz

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Megtudhatja, hogy miként Visual Studio-Azure SQL-adatbázishoz való csatlakozáshoz. 

## <a name="prerequisites"></a>Előfeltételek


Visual Studio segítségével SQL-adatbázis csatlakoztatása, a következőkre lesz szüksége: 


- SQL-adatbázishoz való csatlakozáshoz. Ez a cikk az **AdventureWorks** mintaadatbázis használja. A AdventureWorks adatbázisban című témakörben kaphat [létrehozása a bemutató adatbázist](sql-database-get-started.md).


- Visual Studio 2013 frissítés 4 (vagy újabb verzió). Microsoft Visual Studio közösségi most Itt *ingyenes*.
 - [Töltse le a Visual Studio-Közösség](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [A további lehetőségek ingyenes Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Nyissa meg a Visual Studio az Azure portálról


1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).

2. Kattintson a **További szolgáltatások** > **SQL-adatbázisait**
3. Nyissa meg az **AdventureWorks** adatbázis lap megkeresése és beírása az *AdventureWorks* adatbázisból.

6. Kattintson az **eszközök** gombra az adatbázishoz a lap tetején:

    ![Új lekérdezést. Csatlakozás SQL-adatbázis kiszolgálója: az SQL Server Management Studio eszközben](./media/sql-database-connect-query/tools.png)

7. Kattintson a **Megnyitás a Visual Studióban** (Ha a Visual Studio van szüksége, kattintson a letöltés hivatkozásra):

    ![Új lekérdezést. Csatlakozás SQL-adatbázis kiszolgálója: az SQL Server Management Studio eszközben](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio megnyílik, és a **Csatlakozás a kiszolgálóhoz** ablak már be van állítva a kiszolgáló és a portálon, kijelölt adatbázis csatlakozni.  (Válassza a **Beállítások** ellenőrzéséhez, hogy a kapcsolatot a megfelelő adatbázis értéke.) Írja be a kiszolgáló rendszergazdai jelszavát, és kattintson a **Csatlakozás**gombra.


    ![Új lekérdezést. Csatlakozás SQL-adatbázis kiszolgálója: az SQL Server Management Studio eszközben](./media/sql-database-connect-query/connect.png)


8. Ha nem rendelkezik tűzfal szabálykészletet tagjának számítógépének IP-cím hibaüzenet *nem tud csatlakozni* az alábbi. A tűzfal szabályt szeretne létrehozni, a [konfigurálása az Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabály](sql-database-configure-firewall-settings.md)című témakört.


9. Sikeres csatlakozás után az **SQL Server-objektum Explorer** -ablak megnyílik a kapcsolatot az adatbázishoz.

    ![Új lekérdezést. Csatlakozás SQL-adatbázis kiszolgálója: az SQL Server Management Studio eszközben](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Minta lekérdezések futtatása

Most, hogy kapcsolódik az adatbázist, a következő lépések bemutatják, hogyan tehető függővé egy egyszerű lekérdezés:

2. Kattintson a jobb gombbal az adatbázist, és válassza az **Új lekérdezést**.

    ![Új lekérdezést. Csatlakozás SQL-adatbázis kiszolgálója: az SQL Server Management Studio eszközben](./media/sql-database-connect-query/new-query.png)

3. A lekérdezés ablakban másolja és illessze be a következő kódot.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. A **végrehajtás** gombra kattintva futtassa a lekérdezést:

    ![A siker. Csatlakozás SQL-adatbázis kiszolgálója: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Következő lépések

- Az SQL Server Data Tools SQL-adatbázisait nyissa meg a Visual Studio használja. További részletekért olvassa el [Az SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Csatlakozás kód használatával SQL-adatbázishoz, olvassa el a [Csatlakozás, .NET (C#) használatával SQL-adatbázishoz](sql-database-develop-dotnet-simple.md).



