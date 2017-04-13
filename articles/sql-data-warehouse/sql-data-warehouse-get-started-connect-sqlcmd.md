<properties
   pageTitle="A lekérdezés Azure SQL-adatraktár (sqlcmd) |} Microsoft Azure"
   description="Azure SQL-adatraktár lekérdezése a parancssori segédprogram sqlcmd együtt."
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
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Lekérdezés Azure SQL-adatraktár (sqlcmd)

> [AZURE.SELECTOR]
- [A Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure gépi tanulási](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Az útmutató az Azure SQL-adatraktár lekérdezés használja az [sqlcmd][] parancssori segédprogramot.  

## <a name="1-connect"></a>1. csatlakozás

Első lépések [sqlcmd][], nyissa meg a parancssort, és írja be a **sqlcmd** a kapcsolati karakterláncot az adatraktár SQL-adatbázis követ. A kapcsolati karakterlánc paraméterei a következők:

+ **Server (-S):** Az űrlap kiszolgálójába `<`kiszolgálónév`>`. database.windows.net
+ **Adatbázis (– d):** Az adatbázis neve.
+ **Engedélyezése jegyzett azonosítók (-e):** Idézőjelek közé zárt azonosítókat egy SQL adatraktár példányához engedélyezve kell lennie.

SQL Server-hitelesítés használatához kell a felhasználónevével és jelszavával paraméterek:

+ **Felhasználói (-U):** A képernyőn Server-felhasználók `<`felhasználói`>`
+ **Jelszó (-P):** A felhasználó tartozó jelszót.

Ha például a kapcsolati karakterlánc előfordulhat, hogy fog kinézni:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory integrált hitelesítés segítségével kell az Azure Active Directory-paraméterek:

+ **Azure Active Directory-hitelesítés (-G):** Azure Active Directory authentication használata

Ha például a kapcsolati karakterlánc előfordulhat, hogy fog kinézni:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Meg kell [Azure Active Directory-hitelesítés engedélyezése](sql-data-warehouse-authentication.md) hitelesítést végezni, az Active Directory használatával.

## <a name="2-query"></a>2. lekérdezés

Kapcsolat után bármilyen támogatott Transact-SQL-utasítások szemben a példány bocsát ki.  Ebben a példában lekérdezések elküldésének interaktív módon.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Ezek a következő példák bemutatják, hogyan futtathatja a lekérdezések – kérdések parancs vagy az SQL sqlcmd gázvezetékrendszer köteg módban.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Következő lépések

[Sqlcmd] [sqlcmd dokumentációjában]találhat sqlcmd rendelkezésre álló lehetőségek olvashat bővebben.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[Sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
