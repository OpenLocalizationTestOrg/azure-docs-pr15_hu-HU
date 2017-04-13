<properties
   pageTitle="Hozzon létre egy SQL adatraktár az Azure-portálon |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre az Azure SQL-adatraktár az Azure-portálon"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Hozzon létre egy Azure SQL-adatraktár

> [AZURE.SELECTOR]
- [Azure portál](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [A PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Ebben az oktatóprogramban az Azure portálon használja a egy SQL-adatraktár egy AdventureWorksDW mintaadatbázis tartalmazó létrehozásához.


## <a name="prerequisites"></a>Előfeltételek

Első lépések a következők szükségesek:

- **Azure-fiók**: látogatás [Azure ingyenes próbaverziót][] vagy az [MSDN Azure Credits][] hozhat létre fiókot.
- **Azure SQL server**: további részleteket lásd: [Hozzon létre egy logikai Azure SQL-adatbázis-kiszolgálót az Azure portálján][] .

> [AZURE.NOTE] Egy SQL adatraktár létrehozása új számlázható szolgáltatás eredményezhet.  Lásd: az [SQL adatraktár árak][] további információt.

## <a name="create-a-sql-data-warehouse"></a>Hozzon létre egy SQL adatraktár

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. Kattintson a **+ Új** > **adatok + tárhely** > **SQL adatraktár**.

    ![Hozzon létre](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. Az **SQL adatraktár** lap töltse ki az információkat szükség, majd nyomja le a "Létrehozása" hozhat létre.

    ![Adatbázis létrehozása](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Kiszolgáló**: azt javasoljuk, először jelölje ki a kiszolgálót.  

    - **Az adatbázis neve**: A nevet, amelyet az SQL adatraktár hivatkozást használják.  A kiszolgáló egyedinek kell lennie.
    
    - **Teljesítmény**: azt javasoljuk, 400 [DWUs]kezdődő[DWU]. Húzza a csúszkát balra vagy jobbra a adatraktár teljesítményét vagy átméretezheti a létrehozását követően felfelé vagy lefelé.  Ha többet szeretne megtudni a DWUs, dokumentációjában olvasható a [Méretezés](./sql-data-warehouse-manage-compute-overview.md) vagy az [ár, oldal][SQL adatraktár árak]. 

    - **Előfizetés**: Jelölje ki az [előfizetés] számlázási ez SQL adatraktár.

    - **Erőforráscsoport**: [az erőforrás csoportok] [ Resource group] segítséget nyújt az Azure erőforrások kezelésében tárolók. További információ [az erőforrás csoportok](../azure-resource-manager/resource-group-overview.md).

    - **Jelölje be a forrás**: kattintson a **Jelölje ki a forrás** > **minta**. Azure automatikusan kitölti a **Válassza ki a minta** beállítás AdventureWorksDW.

> [AZURE.NOTE] Az alapértelmezett rendezési egy SQL adatraktár SQL_Latin1_General_CP1_CI_AS. Másik rendezési van szükség, ha [Az SQL-T][] egy másik rendezési az adatbázis létrehozására használható.

4. Kattintson a **Létrehozás** a SQL adatraktár létrehozásához.

5. Várjon néhány percet. Amikor készen áll a adatraktár, meg kell juttatni az [Azure-portálon](https://portal.azure.com). Az SQL-adatbázisok csoportban, illetve az erőforráscsoport létrehozásához használt szerepel az irányítópulton a SQL adatraktár is megkeresheti. 

    ![portál megtekintése](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Következő lépések

Most, hogy egy SQL adatraktár hozott létre, akkor készen áll a [Csatlakozás](./sql-data-warehouse-connect-overview.md) , és kezdje el a lekérdezés.

Adatok betöltése SQL adatraktár, olvassa el az [Áttekintés betöltése](./sql-data-warehouse-overview-load.md).

Ha egy meglévő adatbázis áttelepítése SQL adatraktár, az [áttelepítési – áttekintés](./sql-data-warehouse-overview-migrate.md) című témakörben találhat, vagy [Áttelepítési](./sql-data-warehouse-migrate-migration-utility.md)eszközzel.

Tűzfalszabályokat is konfigurálhatók Transact-SQL nyelvben. További tudnivalókért olvassa el a [sp_set_firewall_rule][] és [sp_set_database_firewall_rule][]című témakört.

Érdemes is remek is megtekintheti a [Gyakorlati tanácsok][]a.

<!--Article references-->
[Egy logikai Azure SQL-adatbázis-kiszolgáló létrehozása az Azure portálján]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Ajánlott eljárások]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[előfizetés]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[AZ SQL-T]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL-adatraktár árak]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure ingyenes próbaverziója]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

