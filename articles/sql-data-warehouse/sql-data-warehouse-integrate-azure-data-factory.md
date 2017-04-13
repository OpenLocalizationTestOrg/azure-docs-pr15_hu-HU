<properties
   pageTitle="Azure Data Factory használata SQL adatraktár |} Microsoft Azure"
   description="Tippek az Azure adatok gyári (ADF) és az Azure SQL-adatraktár megoldások fejlesztésével."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Azure Data Factory használata SQL adatraktár

Azure Data Factory orchestrating SQL adatraktár a tárolt eljárásokat végrehajtásának és az adatok továbbítása a teljes körű felügyelt módszert kínál.  Ez lehetővé teszi, könnyebben beállítási és ütemezett összetett átalakítása kibontása és terhelés (ETL) eljárásokat az SQL adatraktár. Azure Data Factory teljesebb áttekintéséhez lásd: [Azure Data Factory dokumentációt][].

## <a name="data-movement"></a>Adatok mozgás

Azure Data Factory lehetővé teszi, hogy az adatok mozgás a helyszíni adatforrások és a különböző Azure szolgáltatásai között.  Általános, az aktuális integrálása az Azure Data Factory szeretne, és az alábbi helyekről adatok mozgását támogatja:

+ Azure blob-tárolóhoz
+ Azure SQL-adatbázis
+ A helyszíni SQL Server
+ SQL Server IaaS

Olvashat arról, hogyan állíthatja be az adatok másolása tevékenység című [Azure Data Factory az adatok másolása][]

## <a name="stored-procedures"></a>Tárolt eljárás
 Megegyező módon használható adatátvitel ütemezhet Azure Data Factory is használható téve a tárolt eljárásokat végrehajtását.  Ez a lehetővé teszi, hogy a létrehozandó összetettebb folyamatok, és használja ki számítási az adatraktár SQL Azure Data Factory lehetősége kiterjed.

## <a name="next-steps"></a>Következő lépések
Integráció áttekintést [SQL adatraktár integrációs áttekintése][]című témakörben találhat.
Fejlesztési vonatkozó további tippek [SQL adatraktár fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->

[Azure Data Factory az adatok másolása]: ../data-factory/data-factory-data-movement-activities.md
[SQL-adatraktár fejlesztése – áttekintés]: ./sql-data-warehouse-overview-develop.md
[SQL-adatraktár integrációs – áttekintés]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory dokumentáció]:https://azure.microsoft.com/documentation/services/data-factory/

