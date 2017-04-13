<properties
    pageTitle="Kiszolgálók között, előfizetés között, és Azure-és kijelentkezés adatbázisok áthelyezése."
    description="A rövid lépéseket követve másolása, áthelyezése és adatok és az adatbázisokkal az Azure SQL-adatbázis áttelepítése."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Kiszolgálók között, előfizetés között, és Azure-és kijelentkezés adatbázisok áthelyezése

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Adatbázis áthelyezése egy másik, azonos előfizetésben kiszolgálóra
- Az [Azure-portálon](https://portal.azure.com)kattintson az **SQL-adatbázisait**, válasszon az adatbázist a listából, és válassza a **Másolás parancsot**. [Másolja az Azure SQL-adatbázishoz](sql-database-copy.md) talál részletesebb.

## <a name="to-move-a-database-between-subscriptions"></a>Adatbázis áthelyezése előfizetés között
- Az [Azure-portálon](https://portal.azure.com)kattintson a **SQL-kiszolgálók** , és válassza a a tartalmazó kiszolgálón történik; az adatbázist a listából. Kattintson az **Áthelyezés**parancsra, és válassza ki az erőforrások áthelyezése és az előfizetés áthelyezése.

## <a name="to-migrate-a-sql-database-into-azure"></a>Az Azure SQL-adatbázis áttelepítése
- Határozza meg az adatbázis-kompatibilis, és válassza ki a megfelelő áttelepítési mód szükségletek. Kövesse a útmutatások és az [SQL Server-adatbázis áttelepítése](sql-database-cloud-migrate.md)beállítások.

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Azure-Ön kívüli adatbázis másolatának létrehozása
- [BACPAC fájlba exportálhatja.](sql-database-export.md)
