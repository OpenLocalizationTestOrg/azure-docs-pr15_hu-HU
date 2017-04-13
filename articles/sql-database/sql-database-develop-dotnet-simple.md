<properties
    pageTitle="SQL-adatbázis csatlakoztatása a .NET (C#) használatával |} Microsoft Azure"
    description="Használata ezt a rövid a minta kódot a és C# modern alkalmazás összeállítása és hatékony relációs adatbázis a felhőben Azure SQL-adatbázis biztonsági."
    services="sql-database"
    documentationCenter=""
    authors="tobbox"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="tobiast"/>

# <a name="connect-to-sql-database-by-using-net-c"></a>SQL-adatbázis csatlakoztatása a .NET (C#) használatával

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

## <a name="step-1--configure-development-environment"></a>Lépés: 1: Fejlesztői környezet beállítása

[ADO.NET fejlesztési fejlesztői környezet beállítása](https://msdn.microsoft.com/library/mt718321.aspx)

## <a name="step-2-create-a-sql-database"></a>Lépés: 2: Az SQL-adatbázis létrehozása

Lásd: az [első lépések lap](sql-database-get-started.md) megtudhatja, hogy miként hozhat létre a mintavállalati adatbázis.  Fontos, kövesse az egy **AdventureWorks adatbázis-sablon**létrehozása a útmutató. A minták alább látható módon csak az **AdventureWorks séma**használata.  

## <a name="step-3--get-connection-string"></a>3 lépés: A kapcsolati karakterlánc első

[AZURE.INCLUDE [sql-database-include-connection-string-dotnet-20-portalshots](../../includes/sql-database-include-connection-string-dotnet-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Lépés: 4: Minta kódokat futtathatnak.

* [Csatlakozás SQL ADO.NET használatával fogalma igazolása](https://msdn.microsoft.com/library/mt718320.aspx)
* [Csatlakozás SQL az ADO.NET eltolódhasson](https://msdn.microsoft.com/library/mt703195.aspx)

## <a name="next-steps"></a>Következő lépések

* [ASP.NET MVC alkalmazás auth és SQL-adatbázis létrehozása, és telepítse az Azure alkalmazás szolgáltatás]( ../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)
* Olvassa el az [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)
* További információt a [Microsoft SQL Server ADO.Net-illesztőprogram](https://msdn.microsoft.com/library/mt657768.aspx)

## <a name="additional-resources"></a>További források 

* [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázissal tervezés mintázatok](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ismerje meg az [SQL-adatbázis funkciók](https://azure.microsoft.com/services/sql-database/)





