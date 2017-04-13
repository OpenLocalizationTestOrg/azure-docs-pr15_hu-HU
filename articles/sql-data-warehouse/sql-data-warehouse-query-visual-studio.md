<properties
   pageTitle="A lekérdezés Azure SQL-adatraktár (Visual Studio) |} Microsoft Azure"
   description="SQL-adatraktár lekérdezés a Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Lekérdezés Azure SQL-adatraktár (Visual Studio)

> [AZURE.SELECTOR]
- [A Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure gépi tanulási](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Visual Studio segítségével lekérdezés Azure SQL-adatraktár mindössze néhány perc alatt. Ez a módszer az SQL Server Data Tools (SSDT) kiterjesztése a Visual Studióban. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban használatához szükséges:

+ Egy meglévő SQL adatraktár. Hozhat létre egyet, olvassa el a [egy SQL adatraktár létrehozása][]című témakört.
+ Visual Studio SSDT. Ha a Visual Studio, valószínűleg már rendelkezik ezzel. Telepítési utasításokat és beállításai című témakörben talál [a Visual Studio telepítése és SSDT][].
+ A teljesen minősített SQL-kiszolgáló neve. A [Csatlakozás az SQL adatraktár][], olvassa el.

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. a SQL adatraktár csatlakoztatása

1. Nyissa meg a Visual Studio 2013 vagy a Skype 2015.
2. Nyissa meg az SQL Server-objektum Explorer. Ehhez jelölje be a **Nézet** > **SQL Server-objektum Explorer**.

    ![Az SQL Server-objektum Explorer][1]

3. Kattintson az **SQL Server hozzáadása** ikonra.

    ![Az SQL Server hozzáadása][2]

4. Töltse ki a mezőket a kapcsolódás ablakba.

    ![Csatlakozás a kiszolgálóhoz][3]

    - A **kiszolgáló nevét**. Írja be a **kiszolgáló nevét** , korábban azonosította.
    - **Hitelesítés**. Jelölje ki **az SQL Server-hitelesítés** vagy **integrált az Active Directory-hitelesítést**.
    - **Felhasználónév** és **jelszó**. Adja meg a felhasználónevét és jelszavát, ha az SQL Server-hitelesítés a fent kiválasztott volt.
    - Kattintson a **Csatlakozás**gombra.

5. Ismerje meg, hogy az Azure SQL server kibontása A kiszolgáló társított adatbázisok tekintheti meg. Bontsa ki a mintavállalati adatbázis megtekintéséhez AdventureWorksDW.

    ![AdventureWorksDW feltárása][4]

## <a name="2-run-a-sample-query"></a>2. a minta lekérdezések futtatása

Most, hogy a kapcsolat létrejött, az adatbázishoz, nézzük írni a lekérdezést.

1. Kattintson a jobb gombbal az SQL Server-objektum Explorer az adatbázist.

2. Jelölje ki az **új lekérdezést**. Megnyílik egy új lekérdezést ablak.

    ![Új lekérdezés][5]

3. Ez a TSQL lekérdezés másolja be a lekérdezés ablak:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Futtassa a lekérdezést. Ehhez a zöld nyílra, és kövesse az alábbi lépéseket: `CTRL` + `SHIFT` + `E`.

    ![Lekérdezés futtatása][6]

5. Nézze meg a lekérdezés eredményét. Ebben a példában a FactInternetSales táblának 60398 sorokat.

    ![Lekérdezés eredménye][7]

## <a name="next-steps"></a>Következő lépések

Most, hogy kapcsolódni, és a lekérdezés, próbálkozzon [a PowerBI adatok megjelenítése][].

Állítsa be a környezetet az Azure Active Directory authentication, olvassa el [az SQL adatraktár hitelesítés][].

<!--Arcticles-->
[Csatlakozás SQL adatraktár]: sql-data-warehouse-connect-overview.md
[Hozzon létre egy SQL adatraktár]: sql-data-warehouse-get-started-provision.md
[Visual Studio és SSDT telepítése]: sql-data-warehouse-install-visual-studio.md
[SQL-adatraktár hitelesítéshez]: sql-data-warehouse-authentication.md
[a PowerBI adatok megjelenítése]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
