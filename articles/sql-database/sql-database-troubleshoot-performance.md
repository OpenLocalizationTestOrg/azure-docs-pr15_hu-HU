<properties
    pageTitle="SQL-adatbázis teljesítményének, tippek beállítása |} Microsoft Azure"
    description="Tippek a teljesítmény elérése érdekében keresztül értékelése és javítása az Azure SQL-adatbázis beállítása."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="SQL-teljesítményének beállítása, adatbázis teljesítményének javítása, sql teljesítmény javítása tippeket sql-adatbázis teljesítményének javítása"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>SQL-adatbázis teljesítmény eszközbeállítási tippek
Megváltoztathatja a [szolgáltatási réteg](sql-database-service-tiers.md) egy adatbázist, vagy a eDTUs rugalmas adatbázis-készlet növelje a teljesítmény javítása érdekében bármikor, de érdemes először optimalizálni a lekérdezési teljesítmény javítása és a lehetőségek azonosítása. Hiányzó indexek és gyengén optimalizált lekérdezések adatbázis gyenge teljesítményt gyakori oka. Ez a cikk ismerteti a teljesítmény javítása SQL-adatbázisban.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Adatbázis-teljesítmény lépéseket követve felmérése és finomhangolása
1.  Az [Azure-portálon](https://portal.azure.com)kattintson a **SQL-adatbázisait**, jelölje ki az adatbázist, és a figyelés diagram segítségével keresse meg a maximális közelít erőforrások parancsra. DTU felhasználási alapértelmezés szerint látható. Kattintson a **Szerkesztés** időtartomány és megjelenített értékek módosítása gombra.
2.  A lekérdezések DTUs ki szeretné számítani használata a [Lekérdezési teljesítmény betekintést](sql-database-query-performance.md) , és [Az SQL-adatbázis Advisor](sql-database-advisor.md) segítségével létrehozása és az indexek eltávolítása, a lekérdezések paraméterezése és a problémák orvoslására séma problémák javaslatok megtekintése.
3.  Dinamikus kezelése nézetek (DMVs), bővített események (Xevents) és a lekérdezés-tárolójának SSMS segítségével teljesítmény paraméterek első valós időben. Olvassa el a [Teljesítmény útmutatást a témakör](sql-database-performance-guidance.md) részletes figyelemmel kísérésére és tippek beállítása.


    > [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>További források az adatbázist a teljesítmény javítása érdekében lépések
1.  Egyetlen adatbázisok esetén lehetőség van a [szolgáltatási rétegek módosítsa](sql-database-scale-up.md) igény szerinti adatbázis a teljesítmény javítása érdekében.
2.  A több adatbázisok fontolja meg [Rugalmas adatbázis készletek](sql-database-elastic-pool-guidance.md) erőforrások automatikusan méretezni.
