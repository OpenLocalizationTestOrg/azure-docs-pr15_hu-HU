<properties
    pageTitle="SQL-adatbázis csatlakoztatása a fonetikus |} Microsoft Azure"
    description="Azure SQL-adatbázis csatlakoztatása futtatva fonetikus kód minta ad."
    services="sql-database"
    documentationCenter=""
    authors="ajlam"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="andrela"/>


# <a name="connect-to-sql-database-by-using-ruby"></a>SQL-adatbázis csatlakoztatása a fonetikus használatával 

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

Ez a témakör bemutatja, hogyan csatlakozhat, és a lekérdezés-Azure SQL-adatbázisokkal fonetikus. Ez a példa a Windows Ubuntu Linux és Mac rendszerek futtathatók.

## <a name="step-1-configure-development-environment"></a>Lépés: 1: Fejlesztői környezet beállítása

[Az SQL Server TinyTDS fonetikus illesztőprogram előfeltételei](https://msdn.microsoft.com/library/mt711041.aspx)

## <a name="step-2-create-a-sql-database"></a>Lépés: 2: Az SQL-adatbázis létrehozása

Lásd: az [első lépések lap](sql-database-get-started.md) megtudhatja, hogy miként hozhat létre a mintavállalati adatbázis.  Fontos, kövesse az egy **AdventureWorks adatbázis-sablon**létrehozása a útmutató. A minták alább látható módon csak az **AdventureWorks séma**használata.

## <a name="step-3-get-connection-details"></a>Lépés 3: Kapcsolat részleteket

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Lépés: 4: Minta kódokat futtathatnak.

[Kapcsolódás fonetikus használata SQL fogalmat igazolása](http://msdn.microsoft.com/library/mt715797.aspx)

## <a name="next-steps"></a>Következő lépések

* Olvassa el az [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)
* További információt a [Microsoft SQL Server-fonetikus illesztőprogram](https://msdn.microsoft.com/library/mt691981.aspx)

## <a name="additional-resources"></a>További források 

* [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázissal tervezés mintázatok](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ismerje meg az [SQL-adatbázis funkciók](https://azure.microsoft.com/services/sql-database/)
