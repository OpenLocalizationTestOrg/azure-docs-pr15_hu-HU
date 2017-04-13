<properties 
    pageTitle="Azure SQL-adatbázissal az Azure portálján Geo replikációs beállítása |} Microsoft Azure" 
    description="Az Azure portálon Azure SQL-adatbázis Geo-replikáció konfigurálása" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Azure SQL-adatbázissal az Azure portálján Geo replikációs beállítása


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-geo-replication-overview.md)
- [Azure portál](sql-database-geo-replication-portal.md)
- [A PowerShell](sql-database-geo-replication-powershell.md)
- [AZ SQL-T](sql-database-geo-replication-transact-sql.md)

Ez a cikk bemutatja, hogyan aktív Geo-replikáció konfigurálása [portál Azure](http://portal.azure.com)SQL-adatbázis.

Kezdeményezhet feladatátvevő az Azure portálján, olvassa el [az Azure SQL-adatbázissal az Azure portál tervezett vagy nem tervezett feladatátvevő kezdeményezése](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Aktív Geo replikációs (olvasható formátumú másodlagos zónák) az összes szolgáltatási rétegek adatbázisokra most érhető el. Április 2017 nem olvasható másodlagos típusa visszavonásának, és nem olvasható létező adatbázisokat olvasható formátumú másodlagos zónák automatikusan frissülnek.

Az Azure portálon Geo replikációs konfigurálásához az alábbi erőforrás van szükség:

- Az Azure SQL-adatbázis – az elsődleges adatbázis, amelyet egy másik földrajzi területhez tartozik való replikáció.

## <a name="add-secondary-database"></a>Másodlagos adatbázis hozzáadása

Az alábbi lépéseket a replikáció Geo partnerség hozzon létre egy új másodlagos adatbázist.  

Másodlagos hozzáadásához az előfizetés tulajdonosa, vagy társtulajdonos kell lennie. 

A másodlagos adatbázis lesz az elsődleges adatbázis nevét, és alapértelmezés szerint lesz azonos szintű szolgáltatás. A másodlagos adatbázis lehet egy adatbázist, vagy rugalmas adatbázist. További tudnivalókért olvassa el a [Szolgáltatási rétegek](sql-database-service-tiers.md)című témakört.
Miután a másodlagos létrejön, és rendezés, adatok elindul esetében az elsődleges adatbázisból, az új másodlagos adatbázis amelyek. 

> [AZURE.NOTE] Ha a partner adatbázis már létezik (például - eredményeként előző Geo replikációs kapcsolat megszüntetése) a parancs sikertelen lesz.

### <a name="add-secondary"></a>Másodlagos hozzáadása

1. Keresse meg az adatbázist, amelynek a Geo replikációs beállítása az [Azure-portálon](http://portal.azure.com).
2. SQL-adatbázis lapon jelölje be **A replikáció Geo**, és válassza ki a régió, a másodlagos adatbázis létrehozása. Bármely régiót kívül a régió, az elsődleges adatbázist tartalmazó, de a [páronkénti régió](../best-practices-availability-paired-regions.md) ajánljuk.

    ![a replikáció Geo konfigurálása](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Jelölje be, vagy állítsa be a kiszolgáló, és a másodlagos adatbázis árak réteg.

    ![másodlagos konfigurálása](./media/sql-database-geo-replication-portal/create-secondary.png)

5. Tetszés szerint adhat meg másodlagos adatbázis rugalmas adatbázis-készletbe:

 Erőforráskészlethez tartozik a másodlagos adatbázis létrehozásához kattintson a **Rugalmas adatbázis készlet** , és válassza a cél kiszolgálón erőforráskészlethez tartozik. Egy készlet már léteznie kell a tároló kiszolgáló, ez a munkafolyamat nem készlet létrehozása.

6. Kattintson a **Create** a másodlagos hozzáadása.
 
6. A másodlagos adatbázis létrejött, és a rendezési folyamat kezdődik. 
 
    ![másodlagos konfigurálása](./media/sql-database-geo-replication-portal/seeding0.png)

7. A rendezési folyamat befejeződik, a másodlagos adatbázis állapota jeleníti meg.

    ![rendezi elvégzettként](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Másodlagos adatbázis törlése

A művelet véglegesen megszakítja a másodlagos adatbázis a replikáció és módosítja a szerepkör a másodlagos normál írási és olvasási adatbázis. Ha megszakad a kapcsolat, a másodlagos adatbázishoz, a parancsot sikerült, de a másodlagos fog nem válik az írási és olvasási kapcsolódási után vissza.  

1. Keresse meg az elsődleges adatbázis a Geo replikációs partnerség az [Azure-portálon](http://portal.azure.com).
2. SQL-adatbázis lapján jelölje be **A replikáció Geo**.
3. A **formátumú másodlagos zónák** listában jelölje ki az adatbázist a Geo replikációs kapcsolatos partneri megállapodás az eltávolítani kívánt.
4. Kattintson a **Replikáció leállítása**gombra.

    ![másodlagos eltávolítása](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Kattintás a **Replikáció leállítása** megnyitása egy megerősítő ablak kattintson az **Igen gombra** az adatbázis eltávolítása a Geo replikációs partnerség (beállítása csak olvasásra adatbázis bármelyik replikációs nem része).


## <a name="next-steps"></a>Következő lépések

- Ha többet szeretne tudni aktív Geo replikációs, lásd: - [Aktív Geo-replikáció](sql-database-geo-replication-overview.md)
- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot

