<properties
   pageTitle="Azure erőforrás Explorer |} Microsoft Azure"
   description="Azure erőforrás Explorer és hogyan használható megtekintése és frissítése az Azure erőforrás-kezelővel telepítések ismertetése"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Azure erőforrás Intézővel megtekintése és módosítása az erőforrások
Az [Azure erőforrás Explorer](https://resources.azure.com) kiváló eszköz a rögzíthetők információforrások, amelyek már létrehozott az előfizetése. Ez az eszköz segítségével megismerheti, hogyan a szerkezete erőforrásokat megjeleníteni a tulajdonságokat az erőforráshoz rendelt. Tudnivalók a REST API-műveletek és erőforrástípus elérhető PowerShell-parancsmagok, és elküldheti a felületen parancsokat. Erőforrás Explorer különösen akkor hasznos lehet, ha a létrehozandó erőforrás-kezelő sablonokat, mivel lehetővé teszi a meglévő erőforrás tulajdonságainak megtekintése.

A forrás, az erőforrás Explorer tool [github](https://github.com/projectkudu/ARMExplorer), amely biztosítja értékes hivatkozást, ha módosítani szeretné a hasonló működés megvalósítását a saját alkalmazások érhető el.

## <a name="view-resources"></a>Erőforrások megtekintése
Nyissa meg azt a [https://resources.azure.com](https://resources.azure.com) , és jelentkezzen be a a hitelesítő adatait a [Azure-portálon](https://portal.azure.com)használja.

Miután betöltött, a bal oldalon a treeview lehetővé teszi, hogy részletesen megjelenítsen az előfizetések és erőforrás-csoportok:

![TreeView](./media/resource-manager-resource-explorer/are-01-treeview.png)

Erőforráscsoport lehatolhatnak, mint a szolgáltatók, amelynek nincsenek erőforrások az adott csoport jelennek meg:

![szolgáltatók](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Innen elindíthatja a kimutatáshierarchiában való az erőforrás-példányok. Az alábbi képernyőképen tekintheti meg a `sltest` a treeview az SQL Server-példányt. Kattintson a jobb oldali megtekintheti a REST API-kérelmek is használhatja az adott erőforrás információt. Az erőforrás csomópontra navigálással erőforrás Explorer automatikusan a GET kérésekben az erőforrás adatainak beolvasásához hoz létre. A nagyméretű terület alatti URL-CÍMÉT látni fogja az API válasza. 

Ismerkedjen meg az erőforrás-kezelő sablonokat, mint a Laptörzs tartalmának kezdődik meg a már jól ismert! A **Tulajdonságok** részén, a válasz felel meg az értékeket, akkor megadhatja a sablon **Tulajdonságok** szakaszában.

![az SQL server](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Erőforrás Intéző lehetővé teszi az alárendelt források, amíg az SQL-adatbázis kiszolgálója megtartása részletezés, például adatbázisok és tűzfalszabályokat elhárításukhoz gyermek erőforrások.

Ismerkedés a adatbázis us tulajdonságainak megjelenítése arra az adatbázisra. Az alábbi képernyőképen azt láthatja, hogy az adatbázis `edition` van `Standard` és a `serviceLevelObjective` (vagy az adatbázis réteg) van `S1`.

![SQL-adatbázis](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Erőforrások módosítása

Amikor megnyit egy erőforrást, választhat a JSON-tartalom szerkeszthetővé teheti a Szerkesztés gombra. Erőforrás Intéző majd segítségével a JSON szerkesztése és módosítása az erőforrás helyezése-összehívás küldése. Például az alábbi képen látható, a másik adatbázis réteg `S0`:

![adatbázis - helyezése kérés](./media/resource-manager-resource-explorer/are-05-database-put.png)

Kiválasztásával **HELYEZI el** a kérelem elküldése. 

A kérelem elküldése után az erőforrás Explorer újra hibák a GET kérésekben az állapot frissítéséhez. Ebben az esetben is láthatja, hogy a `requestedServiceObjectiveId` frissítette, és eltér a `currentServiceObjectiveId` jelzi, hogy egy méretezési művelet folyamatban van. Az állapot manuális frissítéséhez a beolvasása gombra kattinthat.

![adatbázis - GET request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Erőforrások végrehajtott műveletekhez

A **Műveletek** lap lehetővé teszi a megjelenítéséhez, és további többi hajtanak végre. Például webhely erőforrás kijelölésekor a Műveletek lap jeleníti meg az elérhető műveletek, amelyek az alábbi ábrán egy hosszú listában.

![webhely - bejegyzés kérés](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>A PowerShell API meghívása
A PowerShell lapon az erőforrás Explorer megtekintheti a parancsmagok futtatásával segítségével kapcsolatba léphet az erőforrás, amely jelenleg meg vannak felfedezése. Attól függően, hogy milyen típusú erőforrás kiválasztása után a megjelenített PowerShell-parancsprogramot terjedhet egyszerű parancsmagok (például: `Get-AzureRmResource` és `Set-AzureRmResource`) való bonyolultabb parancsmagok (például egy webhelyen helyek lecserélése). 

![A PowerShell](./media/resource-manager-resource-explorer/are-07-powershell.png)

Az Azure PowerShell-parancsmagok a további tudnivalókért lásd: [Azure PowerShell használatá a Azure-kezelő eszközzel](powershell-azure-resource-manager.md)

## <a name="summary"></a>Összefoglalás
Erőforrás-kezelő használatakor az erőforrás Explorer lehet egy rendkívül hasznos eszközt. Keresse meg a lekérdezés, és végezze el a módosításokat a PowerShell használatával módjai remekül. A REST API-val dolgozik remekül a kezdéshez, és gyorsan tesztelje az API-hívások kódírás megkezdése előtt érdemes. És ha amit éppen ír sablonok lehet egyszerűen, az erőforrás-hierarchia és hol szeretné elhelyezni a konfiguráció – keresés lehet módosítani a portálon, és keresse meg a megfelelő bejegyzések erőforrás Explorer!

További információ a [Videó Scott Hanselman és David Ebbo csatorna 9](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo) megtekintés


