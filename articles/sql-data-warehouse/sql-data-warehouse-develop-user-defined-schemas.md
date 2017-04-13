<properties
   pageTitle="SQL-adatraktár a felhasználó által definiált sémák |} Microsoft Azure"
   description="Tippek a Transact-SQL nyelvben sémák az Azure SQL-adatraktár, a megoldások fejlesztésére."
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

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>SQL-adatraktár a felhasználó által definiált sémák

Hagyományos adatraktárakhoz gyakran segítségével külön adatbázisok alkalmazás határai terhelést, a tartomány vagy a biztonsági alapján. Ha például egy SQL Server hagyományos adatraktár tartalmazhatnak olyan átmeneti tárolásra szolgáló adatbázis, adatok raktári adatbázis és bizonyos adatok intelligens adatbázisok. Ebben a topológiában adatbázisonként használatával terhelést, valamint az architektúra biztonsági határérték.

Ezzel ellentétben SQL adatraktár fut, a teljes adatok raktári terhelés belül egy adatbázis. Illesztés adatbázis tartományok nem engedélyezett. Ezért SQL adatraktár vár minden olyan tábla, használja a raktári a egy adatbázis tárolva lehetnek.

> [AZURE.NOTE] Bármilyen típusú közötti az adatbázis-lekérdezések SQL adatraktár nem támogatja. Ezért adatok raktári megvalósítás, kihasználhatja a minta kell jelez.

## <a name="recommendations"></a>Javaslatok

Ezek a javaslatok munkaterhelésekből, a biztonság, a tartomány és a funkcionális határai összesítésére a felhasználó által definiált a sémák segítségével

1. A teljes adatok raktári terhelés futtatásához használt egy adatraktár SQL-adatbázis
2. A meglévő adatok raktári környezet egy adatraktár SQL-adatbázis használata összesítése
3. Adja meg az előzőleg végrehajtott adatbázisokhoz oszlopazonosító a **felhasználó által definiált sémák** kihasználhatja.

A sémák felhasználói nem használták korábban majd esetén tiszta lappal. Egyszerűen használható a régi adatbázisnév alapjául a felhasználó által definiált sémák adatraktár SQL-adatbázisban.

Ha már korábban használt sémák majd néhány lehetőség közül választhat:

1. Távolítsa el a régi séma nevét, és kezdjen friss
2. A régi séma neveket megőrzése előre függőben által a régi séma nevét a táblázat neve
3. A régi séma neveket megőrzése a táblára kattintva hozza létre újból a régi séma felépítésének egy további sémában nézetek alkalmazásával.

> [AZURE.NOTE] Első vizsgálatára beállítás 3 tűnhet a legtöbb vonzó lehetőséget. Az ördögöt viszont a részletesen. Nézetek csak a SQL adatraktár olvasnak. Az alap táblázat címtáron kellene adatok és módosítását. 3-as lehetőséget is található, nézetek réteg a rendszerbe. Érdemes lehet meg néhány további gondolkodási ad, ha Ön használja a nézeteket a architektúra már.


### <a name="examples"></a>Példa:

Adatbázis neve alapján a felhasználó által definiált sémák megvalósítása

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Megtartja régebbi séma nevek által előre függőben őket a tábla neve. A sémák használata a terhelést határérték.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Régi séma nevek nézetek használata

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Bármilyen módosítást séma stratégia az adatbázis van szüksége a biztonsági modell ismerkedést. A legtöbb esetben, előfordulhat, hogy tudja séma szintre engedélyek hozzárendelése a biztonsági modell egyszerűbbé. Ha részletesebb engedélyek szükségesek, majd az adatbázis szerepkörök is használhatja.

## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
