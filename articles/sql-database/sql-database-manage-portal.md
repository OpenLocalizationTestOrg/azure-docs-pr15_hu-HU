<properties
    pageTitle="Az Azure-portálon Azure SQL-adatbázis kezelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként az Azure-portálon használja a felhőben, az Azure-portálon relációs adatbázisok kezelését."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Az Azure portálon Azure SQL-adatbázisok kezelése


> [AZURE.SELECTOR]
- [Azure portál](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [A PowerShell](sql-database-manage-powershell.md)

Az [Azure portál](https://portal.azure.com/) hozhat létre, figyelésére és Azure SQL-adatbázisok és kiszolgálók kezelése teszi lehetővé. Ebben a cikkben rövid leírását és a gyakori feladatok részleteit mutató hivatkozásokat.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Az Azure SQL-adatbázisok, kiszolgálók és készletek megtekintése

Az SQL-adatbázis elérhető szolgáltatások megtekintéséhez kattintson a **További szolgáltatások**, és a Keresés mezőbe írja be **az SQL** :

![SQL-adatbázis](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Hogyan létrehozása, illetve megtekintheti az Azure SQL-adatbázisait?

Az **SQL-adatbázisait** lap megnyitásához kattintson a **SQL-adatbázisait**, majd kattintson az adatbázist, amellyel dolgozni szeretne lehetőséget, vagy válassza a **+ Hozzáadás** SQL-adatbázis létrehozása. A részletekért olvassa [létrehozása az Azure portál használatával percben SQL-adatbázishoz](sql-database-get-started.md).


![SQL-adatbázisait](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Hogyan létrehozása vagy Azure SQL-kiszolgálók megtekintése?

Az **SQL-kiszolgálók** lap megnyitásához kattintson a **SQL-kiszolgálók**, majd kattintson a kiszolgálóra, amellyel dolgozni szeretne lehetőséget, vagy válassza a **+ Hozzáadás** hozhat létre SQL server. A részletekért olvassa [létrehozása az Azure portál használatával percben SQL-adatbázishoz](sql-database-get-started.md).

![SQL-kiszolgálók](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Hogyan létrehozása vagy SQL rugalmas készletek megtekintése?

Az **SQL rugalmas készletek** lap megnyitásához kattintson **rugalmas készletek SQL**-és majd kattintson a készlet szeretne dolgozni, vagy válassza a **+ Hozzáadás** készlet létrehozása. Részletekért olvassa el [az Azure portálján egy rugalmas adatbázis készlet létrehozása](sql-database-elastic-pool-create-portal.md).

![Rugalmas SQL-készletek](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Hogyan frissítheti és SQL-adatbázis-beállítások megtekintése?

Kattintson a kívánt beállítást a SQL-adatbázis lap megtekintéséhez, illetve az adatbázis-beállítások frissítése:


![SQL-adatbázis beállítása](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Hogyan találhatom meg egy SQL-adatbázisok teljesen minősített kiszolgáló neve?

Az adatbázis-kiszolgáló nevének megtekintéséhez az **SQL-adatbázis** lap az **Áttekintés** hivatkozásra, és figyelje meg a kiszolgáló neve:


![SQL-adatbázis beállítása](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Hogyan kezelhetem az SQL server és a adatbázis a hozzáférés vezérléséhez tűzfalszabályokat?

Megtekintése, létrehozása és frissítése tűzfalszabályokat, **állítsa a kiszolgáló tűzfal** gombjára kattintva az **SQL-adatbázis** lap. További információ a [konfigurálása egy Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályt, az Azure portálon](sql-database-configure-firewall-settings.md)című témakört.


![tűzfalszabályokat](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Hogyan módosíthatom a SQL adatbázis szolgáltatás réteg vagy a teljesítmény szintjét?


A szolgáltatás réteg vagy a teljesítmény szintet SQL-adatbázis frissítéséhez kattintson az **SQL-adatbázis** lap a **árak réteg (méretarány DTUs)** . A részletekért olvassa [szolgáltatás réteg és a teljesítmény szintjének módosítása (árak réteg) SQL-adatbázishoz](sql-database-scale-up.md).


![réteg árak](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Hogyan állítható be a naplózás és veszélyt észlelési SQL-adatbázishoz?

Naplózás és veszélyt észlelési SQL-adatbázishoz konfigurálásához az **SQL-adatbázis** lap **naplózás és veszélyt észlelési** parancsára. A részletekért olvassa [– első lépések az SQL-adatbázis naplózását](sql-database-auditing-get-started.md), és [első lépések az SQL-adatbázis veszélyt észlelési](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Hogyan állítható be a dinamikus adatok maszkolás SQL-adatbázishoz?

Dinamikus adatok SQL-adatbázishoz maszkolás konfigurálásához az **SQL-adatbázis** lap **maszkolás dinamikus adatok** parancsára. A részletekért olvassa [– első lépések SQL adatbázis dinamikus adatok maszkolás](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Hogyan állítható be egy SQL-adatbázis átlátszó titkosítás (TDE)?

Áttetsző adatok titkosítás SQL-adatbázis konfigurálása, **átlátszó adatok titkosítása** gombjára kattintva az **SQL-adatbázis** lap. A részletekért olvassa [TDE engedélyezése egy adatbázisban a portálon](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Hogyan megtekintése vagy módosítása a maximális méret SQL-adatbázis?

Tekintse meg vagy módosítsa az SQL-adatbázis méretét, kattintson az **SQL-adatbázis** lap **az adatbázis mérete** . A maximális méret adatbázis frissítése a szolgáltatási réteg vagy a teljesítmény szintet módosításával. A részletekért olvassa [szolgáltatás réteg és a teljesítmény szintjének módosítása (árak réteg) SQL-adatbázishoz](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Hogyan lehet figyelni és SQL-adatbázis teljesítményének javítása?

Figyelésére és javítható teljesítményének jellemzőit SQL-adatbázishoz, kattintson az **SQL-adatbázis** lap **Teljesítmény áttekintése** . Részletekért olvassa el [Az SQL-adatbázis teljesítmény betekintést](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Hogyan állítható be a replikáció Geo?

Állítsa be a replikáció Geo SQL-adatbázishoz, **Geo replikációs** gombjára kattintva az **SQL-adatbázis** lap. A részletekért olvassa [Geo-replikáció konfigurálása az Azure SQL-adatbázissal az Azure-portálra](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Hogyan tudom geo replikált SQL-adatbázishoz való áttérés?

Geo replikált másodlagos áttérni **Geo replikációs** kattintson az **SQL-adatbázis** lap a, majd **feladatátvevő**. Részletekért olvassa el [az Azure SQL-adatbázissal az Azure portál tervezett vagy nem tervezett feladatátvevő kezdeményezhet](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Hogyan másolása SQL-adatbázishoz

Másolja az SQL-adatbázishoz, kattintson a **Másolás** gombra az **SQL-adatbázis** lap. A részletekért olvassa el, [másolja a vágólapra az Azure portálon Azure SQL-adatbázisból](sql-database-copy-portal.md).


![SQL-adatbázis beállítása](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Hogyan archiválás BACPAC fájlba az Azure SQL-adatbázishoz?

Egy BACPAC SQL-adatbázis létrehozása, az **SQL-adatbázis** lap **Exportálás** parancsára. A részletekért olvassa [archívumba az Azure portálon BACPAC fájlba Azure SQL-adatbázisból](sql-database-export.md).


![SQL-adatbázis exportálása](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Hogyan állíthatók vissza egy SQL-adatbázis előző pontjához időben?

SQL-adatbázis visszaállításához kattintson a az **SQL-adatbázis** lap **visszaállítása** gombra. Részletekért olvassa el [az Azure-portálra az időt az előző pontjához az Azure SQL-adatbázis visszaállítása](sql-database-point-in-time-restore-portal.md).


![SQL-adatbázis beállítása](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Hogyan hozhatok létre Azure SQL-adatbázishoz BACPAC fájlból?

SQL-adatbázis létrehozása BACPAC fájlból, az **SQL server** lap **adatbázis importálása** parancsára. A részletekért olvassa [-Azure SQL-adatbázis létrehozása BACPAC fájl importálása](sql-database-import.md).


![Az SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Hogyan állíthatók vissza a törölt SQL-adatbázishoz?

Szeretné visszaállítani a törölt SQL-adatbázishoz, kattintson a **Törölt adatbázisokat** a az **SQL server** (az SQL server, amely tartalmazza az adatbázist, amely a törölt) lap. Részletekért olvassa el [az Azure portálon törölt Azure SQL-adatbázis visszaállítása](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Hogyan törölhetek SQL-adatbázishoz?

Ha törölni szeretne egy SQL-adatbázisban, az **SQL-adatbázis** lap **törlése** parancsára. 

![SQL-adatbázis beállítása](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>További források

- [SQL-adatbázis](sql-database-technical-overview.md)
- [Figyelése és kezelése egy rugalmas adatbázis készlet az Azure portálján](sql-database-elastic-pool-manage-portal.md)
