<properties
    pageTitle="Teljesítmény számláló shard térkép Manager"
    description="ShardMapManager osztály és az adatok függő útválasztási teljesítmény számláló"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Teljesítmény számláló shard térkép Manager

Rögzítheti a [shard térkép manager](sql-database-elastic-scale-shard-map-management.md)teljesítményének, főleg, ha az [adatok függő útválasztás](sql-database-elastic-scale-data-dependent-routing.md)használatával. Módszerek a Microsoft.Azure.SqlDatabase.ElasticScale.Client osztály számláló létre.  

Számláló használatával [függő útválasztási adatok](sql-database-elastic-scale-data-dependent-routing.md) műveletek teljesítményét. Ezek a számláló a "Rugalmas adatbázis: Shard kezelő" kategória teljesítményét Monitor érhetők el.

**a legújabb verzió:** Nyissa meg a [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Lásd még: [a legújabb rugalmas adatbázis ügyfél könyvtár használata-alkalmazás frissítése](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Előfeltételek

* A kategória teljesítményét és a számláló létrehozásához a felhasználónak kell lennie a számítógépen, az alkalmazás szolgáltatója a helyi **rendszergazdák** csoport része.  

* Hozza létre a teljesítmény számláló, és a számláló frissítése, a felhasználónak a **rendszergazdák** és a **Teljesítmény Monitor felhasználók** csoport tagjának kell lennie. 

## <a name="create-performance-category-and-counters"></a>Kategória teljesítményét és számláló létrehozása 

A számláló létrehozásához, hívja fel a [ShardMapManagmentFactory osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx)CreatePeformanceCategoryAndCounters metódusát. Csak a rendszergazda a módszerrel hajthat végre: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Is használhatja [a](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-parancsprogramot, hajtsa végre a következő módszerrel. A módszerrel hoz létre a következő teljesítmény számláló:  

* **Gyorsítótár használata hozzárendeléseket**: a hozzárendelések a shard térkép gyorsítótárazott száma.
*  **DDR műveletek/sec**: függő útválasztási adatműveleteket a shard térkép mértéke. A számláló frissítésekor hívást kezdeményez, a cél shard sikeres kapcsolat [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) eredményez. 
*  **Keresési gyorsítótárat a találatok/sec leképezése**: a keresés műveletek sikeres gyorsítótár-hozzárendelések shard térképen aránya. 
*  **Keresési gyorsítótárat mért sikertelen találatok/sec leképezése**: keresési műveletek aránya sikertelen gyorsítótár-hozzárendelések shard térképen.
*  **Hozzárendelések hozzáadott, vagy a gyorsítótár/sec frissített**: mely a megfeleltetések felvesznek vagy a shard térkép frissített a gyorsítótár sebességét. 
*  **Hozzárendelések eltávolítja a gyorsítótár/sec**: ráta, amelynél hozzárendelések vannak eltávolítása a csoportból a shard térkép gyorsítótár. 

Egy folyamat minden gyorsítótárazott shard leképezés teljesítmény számláló jön létre.  


## <a name="notes"></a>Jegyzetek
Az alábbi események elindítani a teljesítmény számláló létrehozása:  

* [Belevágna betöltése](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), ha a ShardMapManager bármely shard térképek tartalmaz, a [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) inicializálni. Ide tartoznak a [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) és a [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) módszereket.
* A keresés sikeres shard leképezés ( [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) vagy [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)segítségével). 

* CreateShardMap() használatával shard térkép sikeres létrehozását.

A teljesítmény számláló által elvégzett a shard térképen és a hozzárendelések a gyorsítótár végrehajtott műveletek frissülnek. A sikeres eltávolítása a shard térkép DeleteShardMap () reults használata a teljesítmény számláló példány törlése.  

## <a name="best-practices"></a>Ajánlott eljárások

* A kategória teljesítményét és a számláló létrehozási ShardMapManager-objektum létrehozása előtt csak egyszer kell elvégezni. Minden, a parancs végrehajtása CreatePerformanceCategoryAndCounters() törli az előző számláló (adatvesztés összes előfordulását által jelzett), és újabbakat hoz létre.  

* Teljesítmény számláló példányai egy folyamat jönnek létre. A teljesítmény számláló példányok törlésének bármely alkalmazás összeomlik vagy eltávolítása a gyorsítótárból shard térkép eredményezi.  

### <a name="see-also"></a>Lásd még:

[Rugalmas adatbázis szolgáltatások áttekintése](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

