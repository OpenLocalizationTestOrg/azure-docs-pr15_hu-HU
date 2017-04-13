<properties
   pageTitle="Hozzon létre egy SQL adatraktár TSQL |} Microsoft Azure"
   description="Megtudhatja, hogy miként készíthet egy Azure SQL-adatraktár TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Adatraktár SQL-adatbázis létrehozása a Transact-SQL nyelvben (TSQL) használatával

> [AZURE.SELECTOR]
- [Azure portál](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [A PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Ez a cikk bemutatja, hogyan hozhat létre egy SQL-adatraktár az SQL-T használja.

## <a name="prerequisites"></a>Előfeltételek

Első lépések a következők szükségesek: 

- **Azure-fiók**: látogatás [Azure ingyenes próbaverziót][] vagy az [MSDN Azure Credits][] hozhat létre fiókot.
- **Azure SQL server**: [Hozzon létre egy logikai Azure SQL-adatbázis-kiszolgálót, az Azure portálján][] , vagy [Hozzon létre egy logikai Azure SQL-adatbázis-kiszolgálót, a PowerShell][] talál további információt.
- **Erőforráscsoport**: az azonos erőforráscsoport használni az Azure SQL server vagy [erőforráscsoport létrehozásának][]módját.
- **Környezet kialakítása, T – SQL-utasítás végrehajtása**: használhatja a [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]vagy [SSMS][] végrehajtása az SQL-T.

> [AZURE.NOTE] Új számlázható szolgáltatás vonhat egy SQL adatraktár létrehozása.  Lásd: az [SQL adatraktár árak][] árak rendszeren.

## <a name="create-a-database-with-visual-studio"></a>Visual Studio adatbázis létrehozása

Ha először a Visual Studióban, ismertető [Lekérdezés Azure SQL-adatraktár (Visual Studio)][].  Szeretné kezdeni, nyissa meg az SQL Server-objektum Explorer a Visual Studióban, és csatlakozzon a kiszolgálóhoz, ugyanis az adatraktár SQL-adatbázis.  Miután létrejött, létrehozhat egy SQL adatraktár az alábbi SQL-parancs futtatásával a **fő** adatbázissal.  Ez a parancs az adatbázis MySqlDwDb létrehoz egy DW400 szolgáltatás cél, és lehetővé teszi az adatbázis mérete legfeljebb 10 TB nő.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Sqlcmd adatbázis létrehozása

Azt is megteheti futtathatja a parancs ugyanaz sqlcmd a parancssorból a következő futtatásával.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Ha nincs megadva, az alapértelmezett illesztés SZÉTVÁLOGATÁS SQL_Latin1_General_CP1_CI_AS.  A `MAXSIZE` 250 GB és 240 TB között lehetnek.  A `SERVICE_OBJECTIVE` DW100 és DW2000 közötti lehet [DWU][].  Az összes érvényes értékek listáját dokumentációjában találhat az MSDN [-Adatbázis létrehozása][].  A MAXSIZE és a SERVICE_OBJECTIVE egy [Adatbázis módosítása][] az SQL-T parancs használatával módosítható.  Az illesztés adatbázis létrehozása után nem módosíthatók.   Legyen óvatos amikor újraindításra szolgáltatást, amely megszakít nézetbeli összes lekérdezések módosítása a SERVICE_OBJECTIVE DWU módosításával okozza.  MAXSIZE módosítása nem indítsa újra a szolgáltatások, mert csak egy egyszerű metaadatok műveletet.

## <a name="next-steps"></a>Következő lépések

Az SQL adatraktár a kiépítési, befejeződése után is a [minta adat betöltése][] , vagy olvassa el a [fejlesztése][], [betöltése][]és [áttelepítése][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Lekérdezés Azure SQL-adatraktár (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[áttelepítése]: sql-data-warehouse-overview-migrate.md
[kidolgozása]: sql-data-warehouse-overview-develop.md
[betöltése]: sql-data-warehouse-overview-load.md
[Példaadatok betöltése]: sql-data-warehouse-load-sample-databases.md
[Egy logikai Azure SQL-adatbázis-kiszolgáló létrehozása az Azure-portálra]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Hozzon létre egy logikai Azure SQL-adatbázis-kiszolgáló szolgáltatást a PowerShell használatával]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Erőforráscsoport létrehozása]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[ADATBÁZIS LÉTREHOZÁSA]: https://msdn.microsoft.com/library/mt204021.aspx
[ADATBÁZIS MÓDOSÍTÁSÁHOZ]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL-adatraktár árak]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure ingyenes próbaverziója]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
