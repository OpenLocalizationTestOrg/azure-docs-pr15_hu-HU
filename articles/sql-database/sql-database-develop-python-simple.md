<properties
    pageTitle="SQL-adatbázis csatlakoztatása a Python |} Microsoft Azure"
    description="Azure SQL-adatbázishoz való csatlakozáshoz használhatja Python kód minta jeleníti meg."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Csatlakozás SQL-adatbázis Python használatával


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Ez a témakör bemutatja, hogyan csatlakozhat, és a lekérdezés-Azure SQL-adatbázisokkal Python. Ez a példa a Windows Ubuntu Linux és Mac rendszerek futtathatók.


## <a name="step-1-create-a-sql-database"></a>Lépés: 1: Az SQL-adatbázis létrehozása

Lásd: az [első lépések lap](sql-database-get-started.md) megtudhatja, hogy miként hozhat létre a mintavállalati adatbázis.  Fontos, kövesse az egy **AdventureWorks adatbázis-sablon**létrehozása a útmutató. A minták alább látható módon csak az **AdventureWorks séma**használata. Miután hoz létre az adatbázis győződjön meg arról, hogy engedélyezi a hozzáférést az IP-címére, mivel a tűzfalszabályokat, az [első lépések lap](sql-database-get-started.md) leírtak szerint

## <a name="step-2-configure-development-environment"></a>Lépés: 2: Fejlesztői környezet beállítása

### <a name="mac-os"></a>**Mac OS rendszerben**   
### <a name="install-the-required-modules"></a>A szükséges modulokat telepítése
Nyissa meg a terminált, és telepítése

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Nyissa meg a terminált, és kattintson a hova kívánja történő létrehozását a python parancsfájl könyvtárában. Írja be az alábbi parancsok **FreeTDS** és **pymssql**telepítéséhez. pymssql FreeTDS használja a Csatlakozás SQL-adatbázisait.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**A Windows**

Telepítse pymssql [**Itt**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Ellenőrizze, hogy úgy dönt, hogy a megfelelő whl fájlt. Példa: Ha egy 64 bites számítógépen Python 2.7 esetén válassza a: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Miután letöltötte a .whl fájl helyezze a C:/Python27 mappában.

A parancssorból mezőpont használatával pymssql illesztőprogram telepítése. C:/Python27 és a Futtatás a következő CD-hez
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Képernyőn megjelenő utasításokat követve engedélyezze a használata mezőpont megtalálható [Itt](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Lépés 3: Minta kódokat futtathatnak.

Hozzon létre egy **sql_sample.py** nevű fájlt, és illessze be a következő kódot belül a kijelöléséhez. Ez a parancs használatával indítható:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Az SQL-adatbázis csatlakoztatása

Az [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) függvényt használja az SQL-adatbázishoz való csatlakozáshoz.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Az SQL SELECT utasítás végrehajtása

Lekérdezés SQL-adatbázissal eredményhalmazt beolvasni a [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) függvény használható. Ez a funkció lényegében fogadja el a lekérdezés, és adja eredményül egy eredményhalmazban, amely is többször fölé [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone)használatával.


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Sor beszúrása paraméterekkel és beolvasni a létrehozott elsődleges kulcsot

SQL-adatbázisban az [azonosító](https://msdn.microsoft.com/library/ms186775.aspx) tulajdonság és a [sorozat](https://msdn.microsoft.com/library/ff878058.aspx) objektum használható automatikus generálása [elsődleges kulcs](https://msdn.microsoft.com/library/ms179610.aspx) értékét. 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Tranzakciók


A példa bemutatja, hogyan tranzakciók, amelyben meg:

* Kezdje el a tranzakció
* Az adatok sor beszúrása
* Visszaállítás a tranzakciók a Beszúrás visszavonása 

Illessze be a következő kódot sql_sample.py belül.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Következő lépések

* Olvassa el az [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)
* További információt a [Microsoft SQL Server-Python illesztőprogram](https://msdn.microsoft.com/library/mt652092.aspx)
* Látogasson el a [Python Developer Center](/develop/python/).

## <a name="additional-resources"></a>További források 

* [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázissal tervezés mintázatok](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ismerje meg az [SQL-adatbázis funkciók](https://azure.microsoft.com/services/sql-database/)
