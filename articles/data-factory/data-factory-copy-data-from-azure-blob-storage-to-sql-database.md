<properties
    pageTitle="Adatok másolása Blob-tárolóhoz SQL-adatbázis |} Microsoft Azure"
    description="Ebben az oktatóanyagban bemutatja, hogyan az Azure Data Factory folyamat másolás tevékenység használatával adatok másolása Blob-tárolóhoz SQL-adatbázishoz."
    Keywords="BLOB sql, blob-tárolóhoz adatok másolása"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Adatok másolása SQL-adatbázisokkal Data Factory Blob-tárolóhoz 
> [AZURE.SELECTOR]
- [Áttekintés és a vonatkozó követelmények](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Másolja a varázsló](data-factory-copy-data-wizard-tutorial.md)
- [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [A PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Erőforrás-kezelő Azure-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-VAL](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Ebben az oktatóanyagban hoz létre egy adatok gyári egy folyamat adatok másolása Blob-tárolóhoz SQL-adatbázishoz.

A Másolás tevékenység az adatok mozgását Azure Data Factory hajt végre. Világszerte elérhető szolgáltatás, amely másolhatja az adatokat a különböző adatokat tárolja, biztonságos, megbízható és méretezhető útján között épül. [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk lásd: a Másolás tevékenység kapcsolatban további tájékoztatást.  

> [AZURE.NOTE] Az adatok gyári szolgáltatás részletes áttekintéséhez az [Azure Data Factory – bevezetés](data-factory-introduction.md) témakört is.

##<a name="prerequisites-for-the-tutorial"></a>Az oktatóprogram előfeltételei
Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Azure előfizetés**.  Ha nem előfizetés, létrehozhat egy ingyenes próba-fiókkal, mindössze néhány perc az. [Ingyenes próbaverzió](http://azure.microsoft.com/pricing/free-trial/) témakörben további információt.
- **Azure tárterület-fiókot**. A **forrás** -tárolóban blob-tárolóhoz használnak ebben az oktatóanyagban. Ha nincs Azure tárterület-fiókkal, ismertető [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) megtudhatja, hogy hozzon létre egy újat.
- **Azure SQL-adatbázis**. **Adatok céltárat** az Azure SQL-adatbázis használata ebben az oktatóanyagban. Ha nem rendelkezik, amelyek segítségével használhatja az oktatóprogram Azure SQL-adatbázishoz, megtudhatja, [hogyan hozhat létre és Azure SQL-adatbázis konfigurálása](../sql-database/sql-database-get-started.md) hozhat létre egyet.
- **Az SQL Server 2012 és 2014-es vagy Visual Studio 2013**. A mintavállalati adatbázis létrehozása és az eredményül kapott adatokat az adatbázis megtekintéséhez SQL Server Management Studio és a Visual Studio használhatja.  

## <a name="collect-blob-storage-account-name-and-key"></a>Blob-tároló fiók nevét, és a billentyűk összegyűjtése 
A fiók nevét, és a fiókkulcs oktatóprogram elvégzéséhez a Azure tárterület-fiók szükséges. Megjegyzés: le **fiók nevét** , és a **fiókkulcs** Azure tárterület-fiókját.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. A bal oldali menüben kattintson a **További szolgáltatások** elemre, és válassza a **Tárterület-fiókok**.

    ![Tallózás – tárterület-fiókok](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. Jelölje ki az **Azure tárterület-fiókot** , ebben az oktatóanyagban használni kívánt a **Tárterület-fiókok** lap.
4. Jelölje ki **a beállítások**csoportban a **hívóbetűk** hivatkozást.
5.  **Tárterület fióknév** szövegmező melletti (kép) **másolása** gombra, és a Mentés és illessze be valahol (például: szövegfájlban).
6. Ismételje meg az előző lépésben másolás vagy lefelé a **azonosító1**megjegyzés.
    
    ![Tárterület hívóbetű](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. **X**gombra kattintva zárja be az összes rögzítéséhez.

## <a name="collect-sql-server-database-user-names"></a>Az SQL server, az adatbázis, a felhasználóneveket összegyűjtése
Azure SQL server, az adatbázis és a felhasználót, hogy az oktatóprogram elvégzéséhez azoknak a szükséges. Megjegyzés: lefelé az Azure SQL-adatbázis **server**, az **adatbázis**és a **felhasználó** nevét.

1. Az **Azure-portálon**kattintson a bal oldalon a **További szolgáltatások** , és jelölje ki az **SQL-adatbázisait**.
2. A **SQL-adatbázisok lap**jelölje be az ebben az oktatóanyagban használni kívánt **adatbázist** . Megjegyzés: lefelé az **adatbázis nevét**.  
3. Az **SQL-adatbázis** lap a **Beállítások**csoportban kattintson a **Tulajdonságok** parancsra.
4. Megjegyzés: az értékek lefelé **Kiszolgáló neve** és a **Kiszolgáló felügyeleti jelentkezzen be**.
5. **X**gombra kattintva zárja be az összes rögzítéséhez.

## <a name="allow-azure-services-to-access-sql-server"></a>Szeretne elérni SQL server Azure szolgáltatások engedélyezése 
Győződjön meg arról **, hogy **Azure szolgáltatásokhoz való hozzáférés engedélyezése** beállítást bekapcsolva az Azure SQL Server, hogy az adatok gyári szolgáltatás hozzáférhessen az Azure SQL server** . Ellenőrizze, és kapcsolja be ezt a beállítást, végezze el az alábbi lépéseket:

1. Kattintson a **További szolgáltatások** központi a bal oldalon, majd az **SQL-kiszolgálók**.
2. Jelölje ki a kiszolgálót, majd kattintson a **tűzfalat** a **beállításai**. 
4. Kattintson a **tűzfal beállításai** lap **Tovább** az **Azure szolgáltatások legyenek elérhetők**.
5. **X**gombra kattintva zárja be az összes rögzítéséhez.

## <a name="prepare-blob-storage-and-sql-database"></a>Készítsen Blob-tárolóhoz és SQL-adatbázis 
Most előkészítése az Azure blob-tárolóhoz és Azure SQL-adatbázissal az oktatóprogram hajt végre az alábbi lépéseket:  

1. Indítsa el a Jegyzettömb, illessze be az alábbi szöveget és mentése másként **emp.txt** **C:\ADFGetStarted** mappát a merevlemezen.

        John, Doe
        Jane, Doe

2. Eszközeivel [Azure tároló Explorer](https://azurestorageexplorer.codeplex.com/) például a **adftutorial** tároló létrehozása és feltöltése a **emp.txt** a tároló.

    ![Azure tároló Explorer. Adatok másolása Blob-tárolóhoz SQL-adatbázishoz](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. A következő SQL-parancsfájl használatával a **Vállalati projektirányítási** tábla létrehozása az Azure SQL-adatbázisban.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Esetén az SQL Server 2012 és 2014-es telepítve van a számítógépén:** kövesse az utasításokat [lépés 2: kezelése Azure SQL-adatbázis használata SQL Server Management Studio SQL-adatbázis csatlakoztatása](../sql-database/sql-database-manage-azure-ssms.md#Step2) csatlakozni az Azure SQL-kiszolgálóhoz, és futtassa a SQL cikk. Ez a cikk a [Klasszikus Azure portálon](http://manage.windowsazure.com)nem az [Új Azure portálon](https://portal.azure.com)használja a tűzfal Azure SQL-kiszolgáló konfigurálása.

    Ha az ügyfélnek nem engedélyezi az Azure SQL-kiszolgáló elérésére, szeretné beállítani a tűzfalat a számítógépről (IP-cím) hozzáférés engedélyezése a Azure SQL Server szükséges. [Ez a cikk](../sql-database/sql-database-configure-firewall-settings.md) lépései konfigurálja a tűzfalat a Azure SQL Server tartalmaz.

A Előfeltételek befejezése. Adatok gyár használja az alábbi módokon hozhat létre. Kattintson a lapok vagy tetején az oktatóprogram elvégzéséhez az alábbi hivatkozások egyikére.     

- [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [A PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API-VAL](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Másolja a varázsló](data-factory-copy-data-wizard-tutorial.md)
