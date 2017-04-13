<properties
    pageTitle="Globális adatbázis-replikáció DocumentDB |} Microsoft Azure"
    description="Megtudhatja, hogy miként kezelheti a globális replikáció DocumentDB fiókjának az Azure portálon keresztül."
    services="documentdb"
    keywords="globális adatbázis, a replikáció"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Hogyan végezhetők el DocumentDB globális adatbázis-replikáció: az Azure portál használatával

Megtudhatja, hogy miként az Azure portal segítségével adatokat az Azure DocumentDB adataiban globális állásáról több régióban replikáció.

Hogyan globális adatbázis információt replikációs DocumentDB működik, című témakörben [globálisan DocumentDB elosztás adatokat](documentdb-distribute-data-globally.md). Globális adatbázis-replikáció programozás útján végrehajtásával kapcsolatos további tudnivalókért olvassa el a [több területre DocumentDB fiókoknál fejlődő](documentdb-developing-with-multiple-regions.md)című témakört.

> [AZURE.NOTE] Globális DocumentDB adatbázisok eloszlása általában elérhető, és minden újonnan létrehozott DocumentDB fiókok automatikusan engedélyezett. Ahhoz, hogy minden meglévő fiókok globális terjesztési dolgozunk, de időközben, ha azt szeretné, hogy engedélyezve van a fiók globális terjesztési [ügyfélszolgálatát](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , és azt fogja engedélyezze azt meg most gombra.

## <a id="addregion"></a>Az adatbázis globális területek hozzáadása

A legtöbb [Azure régióban]érhető el DocumentDB [azureregions]. Miután kijelölte az alapértelmezett konzisztencia szint az adatbázis-fiók, egy vagy több régiók társítható (attól függően, hogy a választott alapértelmezett konzisztencia szintű és globális terjesztési igényeinek).

1. Az [Azure-portált](https://portal.azure.com/)a Jumpbar, a **DocumentDB fiókok**parancsára.
2. Válassza a **Fiók DocumentDB** lap módosítása az adatbázis-fiókot.
3. A fiók lap kattintson **globálisan bizonyos adatok** a menüből.
4. A **replikáció globálisan az adatok** lap jelölje ki a hozzáadni vagy eltávolítani a régiók, és kattintson a **Mentés**gombra. A költség vehet fel a régiók van című [árak, oldal](https://azure.microsoft.com/pricing/details/documentdb/) vagy a [globálisan DocumentDB elosztás adatokat](documentdb-distribute-data-globally.md) a cikk további információt.

    ![Kattintson a hozzáadni vagy eltávolítani azokat a térképen a régiók][1]

### <a name="selecting-global-database-regions"></a>Globális adatbázis terület kijelölése

Két vagy több régiók konfigurálásakor javasoljuk, hogy területek vannak jelölve, a régió párban leírtak alapján a [üzleti folytonosságot és katasztrófa helyreállítási (BCDR): Azure párosított régiók]  [ bcdr] cikk.

Konkrétan a tartalomkeresési több területre, ellenőrizze, hogy minden egyes párosított régió oszlopot a régiók (+/-1 páratlan) eltérő számú jelölje ki. Például ha azt szeretné, négy amerikai területek szeretne telepíteni, válasszon két USA-beli térség a bal oldali oszlopban, és két jobbról. Igen, a következő lenne a egy megfelelő szolgáltatáskészletet: nyugati Amerikai Egyesült Államok, kelet-Amerikai Egyesült Államok, Észak központi US és Dél központi US.

Ez az útmutató fontos, hogy hajtsa végre, ha csak két sorterület katasztrófa helyreállítási esetek vannak beállítva. Több mint két régió, a útmutatásának megfelelően érdemes, de nem kritikus mindaddig, amíg a kijelölt terület részét igazodjon a párosításhoz.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Következő lépések

Megtudhatja, hogy miként kezelheti globálisan replikált fiókja következetességét [DocumentDB konzisztencia szintjén](documentdb-consistency-levels.md)olvasási.

Hogyan globális adatbázis információt replikációs DocumentDB működik, című témakörben [globálisan DocumentDB elosztás adatokat](documentdb-distribute-data-globally.md). Programozás útján adatreplikálás több régióban tudni olvassa el a [több területre DocumentDB fiókoknál fejlődő](documentdb-developing-with-multiple-regions.md)című témakört.

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
