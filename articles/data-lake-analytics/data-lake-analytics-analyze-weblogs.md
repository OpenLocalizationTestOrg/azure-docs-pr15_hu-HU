<properties
   pageTitle="Webhely naplók Azure adatok tó Analytics segítségével elemezheti |} Azure"
   description="Megtudhatja, hogy miként webhely naplók adatok tó Analytics segítségével elemezheti. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Oktatóprogram: Azure adatok tó Analytics használatával webhely naplók elemzése

Megtudhatja, hogy miként elemzése webhely naplók használata adatok tó Analytics, különösen találja meg, hogy melyik ajánlók adódott a hibák, amikor megkíséreltek látogasson el a webhelyére.

>[AZURE.NOTE] Ha csak szeretne lásd: az alkalmazás dolgozik, akkor [használata Azure adatok tó Analytics interaktív oktatóanyagok](data-lake-analytics-use-interactive-tutorials.md)folyamatát időt takaríthat meg. Ebben az oktatóprogramban az ugyanolyan eseteket és ugyanazt kódot alapul. Ebben az oktatóanyagban célja, hogy a fejlesztők létrehozása és adatok tó Analytics alkalmazást futtató végpontok közötti tapasztalatait.

## <a name="prerequisites"></a>Előfeltételek:

- A **visual Studio 2015, a Visual Studio 2013 frissítése, 4, vagy vizuális C++ telepítve van a Visual Studio 2012**.
- **Microsoft Azure SDK a .NET rendszerhez 2.5-ös verziójának, vagy felett**.  Telepítse újra a [Webes platform telepítő](http://www.microsoft.com/web/downloads/platform.aspx)használatával.
- Az **[adatok tó Tools for Visual Studio](http://aka.ms/adltoolsvs)**.

    Miután telepítette az adatok tó Tools for Visual Studio, **Adatok tó** menüt a Visual Studióban jelennek meg:

    ![Az SQL-U Visual Studio menü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Adatok tó analitikai és a Visual Studio tó Adateszközök alapismeretei**. Első lépésként olvassa el:

    - [Első lépések az Azure adatok tó Analytics Azure-portálon](data-lake-analytics-get-started-portal.md).
    - [Adatok tó tools for Visual Studio segítségével kidolgozása U-SQL nyelvben parancsfájl](data-lake-analytics-data-lake-tools-get-started.md).

- **Adatok tó Analytics-fiók.**  Lásd: [Azure adatok tó Analytics-fiók létrehozása](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Az adatok tó eszközök létrehozása adatok tó Analytics-fiókokat nem támogatja.  Úgy van létrehozása az Azure-portálra, Azure PowerShell, .NET SDK vagy Azure CLI használatával.
- **Töltse fel a mintaadatokat az adatok tó Analytics-fiókjába.** Lásd: [SearchLog.tsv feltölteni az alapértelmezett tó adattárolás fiókba](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Adatok tó Analytics feladat futtatása, szüksége lesz néhány adatot. Annak ellenére, hogy a tó Adateszközök támogatja a feltöltése adatokat, de a portálon feltöltése a mintaadatokat, így könnyebben kövesse az ebben az oktatóanyagban kell használni.

## <a name="connect-to-azure"></a>Azure csatlakoztatása

Mielőtt össze, és bármelyik U – SQL-parancsfájlok tesztelése, először csatlakoznia kell Azure.

**Adatok tó Analytics csatlakozni**

1. Nyissa meg a Visual Studio.
2. Az **Adatok tó** menüben kattintson a **Beállítások**elemre.
4. Kattintson a **Sign In**vagy **Felhasználói módosítása** , ha valaki bejelentkezve, és kövesse az utasításokat.
5. Kattintson az **OK gombra** a beállítások párbeszédpanel bezárásához.

**A adatok tó Analytics-fiókokat böngészése**

1. A Visual Studióban **Server Explorer** megnyitásához nyomja le a **CTRL + ALT + S billentyűkombinációt**.
2. A **Kiszolgáló Explorer**bontsa ki az **Azure**, és bontsa ki a **Adatok tó Analytics**. Az adatok tó Analytics partnerlista kell látni, ha vannak. A studio adatok tó Analytics-fiókokat nem hozható létre. Hozzon létre egy fiókot, lásd: [Első lépések az Azure adatok tó Analytics Azure portálon](data-lake-analytics-get-started-portal.md) vagy [Azure adatok tó Analytics Azure PowerShell használata – első lépések](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Az SQL-U alkalmazás kidolgozása

A U – SQL-alkalmazások főleg U-SQL nyelvben parancsfájl. U-SQL nyelvben kapcsolatos további információért olvassa el a [U-SQL nyelvben – első lépések](data-lake-analytics-u-sql-get-started.md)című témakört.

Az alkalmazás hozzáadása a felhasználó által definiált operátorok vehet.  További tudnivalókért olvassa el a [kidolgozása U-SQL nyelvben felhasználó által definiált operátorok az adatok tó Analytics feladatok](data-lake-analytics-u-sql-develop-user-defined-operators.md)című témakört.

**Létrehozása és adatok tó Analytics feladat elküldése**

1. A **fájl** menüben kattintson az **Új**gombra, és kattintson a **Projekt**.
2. Válassza ki a Project U-SQL nyelvben.

    ![új U-SQL nyelvben Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Kattintson az **OK gombra**. A Visual studio megoldást hoz létre Script.usql-fájl segítségével.
4. Írja be a következőt a Script.usql fájlba:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Az SQL-U, című cikkben talál részletes [adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md).    

5. Egy új U-SQL nyelvben parancsprogram hozzáadása a projekthez, és adja meg az alábbiakat:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Váltson vissza az első U – SQL-parancsfájl, és kattintson a **Küldés** gomb melletti, adja meg a Analytics-fiókot.
7. A **Megoldás Intézőben**kattintson a jobb gombbal az **Script.usql**, és kattintson a **Szerkesztés parancsfájl**. Ellenőrizze a kimeneti ablakban az eredményeket.
8. A **Megoldás Explorer**kattintson a jobb gombbal az **Script.usql**, és válassza a **Parancsprogram elküldése**.
9. Bizonyosodjon meg arról a **Analytics-fiók** a kívánt helyre kattintva futtassa a feladatot, és kattintson a **Küldés**gombra. Beküldési eredmények és a feladat hivatkozás érhetők el a Visual Studióban ablak tó Adateszközök a beküldött befejeztével.
10. Várja meg, amíg a feladat sikeresen befejeződött.  Nem sikerült a feladatot, valószínűleg hiányoznak a forrásfájl nélkül.  Kérjük, olvassa el a ebben az oktatóanyagban kapcsolatban előzetesen szükséges szakaszát. További hibaelhárítási tudnivalókért lásd [Monitor és Azure adatok tó Analytics feladatok hibaelhárítása](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    A feladat befejezése után az alábbi képernyőképen kell jelenik meg:

    ![adatok tó analytics webnaplókra webhely naplók elemzése](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Most már ismételje 7 – 10 **Script1.usql**.

>[AZURE.NOTE]Olvassa el a nem, vagy létrehozott vagy belül az azonos parancsfájl U-SQL nyelvben táblázat írni.  Ezért használata két parancsfájlok ebben a példában.

**A feladat kimenet megjelenítéséhez**

1. A **Kiszolgáló Intéző**bontsa ki az **Azure**, bontsa ki az **Adatok tó Analytics**, bontsa ki az adatok tó Analytics-fiókját, bontsa ki a **Tárterület-fiókok**, kattintson a jobb gombbal az alapértelmezett tó adattárolás fiók és **Explorer**parancsra.
2.  Kattintson duplán a **minták** nyissa meg azt a mappát, és kattintson duplán a **kimeneti értékeket**.
3.  Kattintson duplán a **UnsuccessfulResponsees.log**.
4.  Is annak érdekében, hogy közvetlenül a kimenet navigáljon kattintson duplán a kimeneti fájl a diagram nézetben a feladat belül.

## <a name="see-also"></a>Lásd még:

Első lépések az adatok tó Analytics különböző eszközök használatával, olvassa el:

- [Első lépések az adatok tó Analytics Azure portál használatával](data-lake-analytics-get-started-portal.md)
- [Első lépések az adatok tó Analytics Azure PowerShell használatával](data-lake-analytics-get-started-powershell.md)
- [Első lépések az adatok tó Analytics .NET SDK használatával](data-lake-analytics-get-started-net-sdk.md)

A további fejlesztés témakörökben talál:

- [Az SQL-U parancsfájlok tó Adateszközök for Visual Studio kidolgozása](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md)
- [A felhasználó által definiált operátorok U-SQL nyelvben adatok tó Analytics feladatokhoz kidolgozása](data-lake-analytics-u-sql-develop-user-defined-operators.md)
