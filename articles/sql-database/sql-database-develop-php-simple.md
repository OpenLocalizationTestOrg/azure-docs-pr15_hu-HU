<properties
    pageTitle="SQL-adatbázis csatlakoztatása a Windows PHP használatával |} Microsoft Azure"
    description="Hogyan jeleníti meg a ügyfélről Windows Azure SQL-adatbázishoz csatlakozik, és az ügyfél szükséges szoftvert összetevővel mutató hivatkozások minta PHP programot."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>SQL-adatbázis csatlakoztatása a Windows PHP használatával


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Ez a témakör bemutatja, hogyan csatlakozhat Azure SQL-adatbázis Windows rendszeren futó PHP nyelven íródott ügyfél-alkalmazásból.

## <a name="step-1--configure-development-environment"></a>Lépés: 1: Fejlesztői környezet beállítása

[PHP fejlesztési fejlesztői környezet beállítása](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Lépés: 2: Az SQL-adatbázis létrehozása

Lásd: az [első lépések lap](sql-database-get-started.md) megtudhatja, hogy miként hozhat létre a mintavállalati adatbázis.  Fontos, kövesse az egy **AdventureWorks adatbázis-sablon**létrehozása a útmutató. A minták alább látható módon csak az **AdventureWorks séma**használata.


## <a name="step-3-get-connection-details"></a>Lépés 3: Kapcsolat részleteket

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Lépés: 4: Minta kódokat futtathatnak.

* [Kapcsolódás PHP használata SQL fogalmat igazolása](https://msdn.microsoft.com/library/mt720665.aspx)
* [Csatlakozás SQL PHP a eltolódhasson](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Következő lépések

* Olvassa el az [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)
* További információt a [Microsoft SQL Server PHP-illesztőprogram](https://msdn.microsoft.com/library/dn865013.aspx)
* PHP telepítésével és használatával kapcsolatos további tudnivalókért olvassa el a [Elérése SQL Server adatbázisok PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx)című témakört.

## <a name="additional-resources"></a>További források 

* [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázissal tervezés mintázatok](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ismerje meg az [SQL-adatbázis funkciók](https://azure.microsoft.com/services/sql-database/)
