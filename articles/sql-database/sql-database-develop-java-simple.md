<properties
    pageTitle="SQL-adatbázis csatlakoztatása a Windows JDBC Java használatával |} Microsoft Azure"
    description="Azure SQL-adatbázishoz való csatlakozáshoz használhatja Java kód minta jeleníti meg. A minta használ JDBC, és a Windows ügyfélszámítógépen fusson."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>SQL-adatbázis csatlakoztatása a Windows JDBC Java használatával


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Ez a témakör bemutatja az Azure SQL-adatbázishoz való csatlakozáshoz használható Java kód minta. A Java minta meg a Java fejlesztési Kit (JDK) verzió 1.8 támaszkodik. A minta Azure SQL-adatbázissal az JDBC illesztőprogram használatával kapcsolódnak.

## <a name="step-1--configure-development-environment"></a>Lépés: 1: Fejlesztői környezet beállítása

[Java fejlesztési fejlesztői környezet beállítása](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Lépés: 2: Az SQL-adatbázis létrehozása

Lásd: az [első lépések lap](sql-database-get-started.md) megtudhatja, hogy miként hozhat létre egy adatbázist.  

## <a name="step-3-get-connection-string"></a>3 lépés: A kapcsolati karakterlánc első

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Ha a JTDS JDBC illesztőprogramot használ, majd meg kell adni "ssl = megkövetelése" a csatlakozási URL-címében a karakterláncot, és meg kell beállítania a következő lehetőséget a JVM "-Djsse.enableCBCProtection=false". JVM beállítás letiltja a biztonsági kockázatot egy javításon, így tudja, hogy milyen kockázatot van szó, a beállítás használata előtt.

## <a name="step-4-run-sample-code"></a>Lépés: 4: Minta kódokat futtathatnak.

* [Kapcsolódás Java használata SQL fogalmat igazolása](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Következő lépések

* Látogasson el a [Java Developer Center](/develop/java/).
* Olvassa el az [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)
* További információt a [Microsoft SQL Server-JDBC illesztőprogram](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>További források 

* [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázissal tervezés mintázatok](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ismerje meg az [SQL-adatbázis funkciók](https://azure.microsoft.com/services/sql-database/)
