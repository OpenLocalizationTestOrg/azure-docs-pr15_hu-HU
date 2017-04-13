<properties
   pageTitle="Az SQL adatraktár változók hozzárendelése |} Microsoft Azure"
   description="Tippek a Transact-SQL Azure SQL-adatraktár változók kiosztása megoldások fejlesztésére."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="assign-variables-in-sql-data-warehouse"></a>Az SQL adatraktár változók hozzárendelése
Az SQL adatraktár változók használata vannak beállítva a `DECLARE` utasítás vagy a `SET` utasítás.

Az alábbi módon tökéletesen érvényes állíthat be egy változó érték:

## <a name="setting-variables-with-declare"></a>A DECLARE változók beállítása

A DECLARE változók inicializálása az egyik változó érték megadása a SQL adatraktár leginkább rugalmas módjai.

```sql
DECLARE @v  int = 0
;
```

A DECLARE egynél több változót beállítása egyszerre is használhatja. Nem használhatja `SELECT` vagy `UPDATE` művelet:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Nem módon, és használja a ugyanazt a DECLARE utasítást változó. A pont, az alábbi példa szemlélteti, hogy **nem** lehet @p1 van inicializált és a ugyanazt a DECLARE utasítást használja. Ezt a hibát eredményez.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Értékek való beállításával
Nagyon közös módszert egy változó beállítás be kapcsolva.

Az összes az alábbi példákban szemléltetett módon érvényes beállítására változó való:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Csak beállíthatja, hogy egyváltozós egyszerre való. Azonban látható legyen a fent, összetett operátorok megengedett is.

## <a name="limitations"></a>Korlátozások
Változó hozzárendelés kijelölése vagy a frissítés nem használható.


## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
