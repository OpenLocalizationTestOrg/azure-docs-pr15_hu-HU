<properties
    pageTitle="SQL-adatbázis csatlakoztatása a Node.js |} Microsoft Azure"
    description="Azure SQL-adatbázishoz való csatlakozáshoz használhatja Node.js kód minta jeleníti meg."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>

# <a name="connect-to-sql-database-by-using-nodejs"></a>Csatlakozás SQL-adatbázis Node.js használatával

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

Ez a témakör bemutatja, hogyan csatlakozhat, és a lekérdezés Azure SQL-adatbázisokkal Node.js. Ez a példa a Windows Ubuntu Linux és Mac rendszerek futtathatók.

## <a name="step-1-configure-development-environment"></a>Lépés: 1: Fejlesztői környezet beállítása

[Az SQL Server fárasztó Node.js illesztőprogram előfeltételei](https://msdn.microsoft.com/library/mt652094.aspx)

## <a name="step-2-create-a-sql-database"></a>Lépés: 2: Az SQL-adatbázis létrehozása

Lásd: az [első lépések lap](sql-database-get-started.md) megtudhatja, hogy miként hozhat létre a mintavállalati adatbázis.  Fontos, kövesse az egy **AdventureWorks adatbázis-sablon**létrehozása a útmutató. A minták alább látható módon csak az **AdventureWorks séma**használata.

## <a name="step-3-get-connection-details"></a>Lépés 3: Kapcsolat részleteket

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Lépés: 4: Minta kódokat futtathatnak.

[Kapcsolódás Node.js használata SQL fogalma igazolása](https://msdn.microsoft.com/library/mt715784.aspx)

## <a name="next-steps"></a>Következő lépések

* Olvassa el az [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)
* További információt a [Microsoft SQL Server-Node.js illesztőprogram](https://msdn.microsoft.com/library/mt652093.aspx)

## <a name="additional-resources"></a>További források 

* [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázissal tervezés mintázatok](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ismerje meg az [SQL-adatbázis funkciók](https://azure.microsoft.com/services/sql-database/)
