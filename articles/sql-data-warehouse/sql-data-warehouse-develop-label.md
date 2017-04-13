<properties
   pageTitle="Címkék instrument lekérdezések használata SQL adatraktár |} Microsoft Azure"
   description="Tippek az Azure SQL-adatraktár címkék instrument lekérdezések használatának megoldások fejlesztésére."
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

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>SQL-adatraktár instrument lekérdezések címkék használata
SQL-adatraktár lekérdezés címkék nevű fogalma támogatja. Mielőtt bármilyen mélységű vegyük érkeznek megtekintése egy példát:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

A lekérdezésre a "Saját lekérdezés felirat" karakterlánc utolsó sor címkéket tartalmaznak. Ez a kifejezetten hasznosak lehetnek, mint a címke lekérdezés-tudja a DMVs keresztül. Ez ad egy mechanizmusa probléma-lekérdezések nyomon követése és is azonosításához folyamatban, egy ETL Futtatás keresztül.

Jó elnevezni valójában itt segítséget nyújt. Például úgy, mint ahogy "PROJECT: eljárás: utasítás: Megjegyzés" segíthet azonosítja a lekérdezést az egyik ablaktábláról a másikra az adatforrás-vezérlő összes kódot.

Keresés címke alapján a következő lekérdezés a dinamikus kezelése nézetek használó is használhatja:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Fontos, hogy Ön tördelése szögletes zárójelek vagy dupla idézőjelek körül a word címke lekérdezése esetén. Címke foglalt szó, és hibát jelenít meg, ha nem tagolt lesz.


## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
