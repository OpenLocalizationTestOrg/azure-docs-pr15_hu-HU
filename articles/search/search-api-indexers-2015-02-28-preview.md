<properties 
pageTitle="Az indexelési műveletek (Azure keresési szolgáltatás REST API: a Skype 2015-02-28 – előzetes verzió) |} Azure keresési előnézete API" 
description="Az indexelési műveletek (Azure keresési szolgáltatás REST API: a Skype 2015-02-28 – előzetes verzió)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Az indexelési műveletek (Azure keresési szolgáltatás REST API: a Skype 2015-02-28 – előzetes verzió)#

> [AZURE.NOTE] Ez a cikk ismerteti az indexelő a [Skype 2015-02-28 – előzetes verzió REST API-t](search-api-2015-02-28-preview.md). API verziójában felveszi a dokumentum kinyerésének Azure Blob-tárolóhoz indexelő és Azure Táblatárolóhoz indexelő, valamint további fejlesztések: előzetes verziója. Az API támogatja a általában elérhető (kiadás) indexelő, beleértve az indexelő az Azure SQL-adatbázissal, SQL Server Azure VMs és Azure DocumentDB is.

## <a name="overview"></a>– Áttekintés ##

Azure keresési is integrálása közvetlenül néhány gyakori adatforrás adatait indexelni kódírás kell eltávolítása. Ezt meg beállítani, felhívhatja az Azure keresési API-t **Indexelő** és az **adatforrás**létrehozása és kezelése. 

Egy **indexelési** , amelyek az adatforrásokhoz kapcsolódnak cél keresési indexeket az erőforrás. Indexkiszolgáló alapul, az alábbi módon: 

- Végezze el az adatok kitöltéséhez az index egyszeri másolatát.
- Index az ütemezés az adatforrásban végrehajtott módosítások szinkronizálása. Az ütemezés az indexelő definition része.
- Szükség szerint a tárgymutató frissíthető igény hivatkozhat. 

Az **Indexelő** akkor hasznos, ha azt szeretné, hogy index rendszeres frissítéseit. Egy beágyazott ütemterv beállítása az indexelő definíció részeként, vagy indítsa el a [Futtatás indexelő](#RunIndexer)használatával igény. 

Egy **adatforráshoz** Megadja, hogy milyen adatokat kell indexelni, hitelesítő adatokat, és ahhoz, hogy hatékony azonosítja a módosításokat, az adatok (például módosított vagy törölt adatbázis tábla sorainak) Azure keresési házirendek eléréséhez. Egy független erőforrás definíciója, így több indexelő használható.

Az alábbi adatforrások jelenleg támogatott:

- **Azure SQL-adatbázis** és az **SQL Server Azure VMs**. A célzott segédlet [ismertető](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)témakört. 
- **Azure DocumentDB**. A célzott segédlet [ismertető](../documentdb/documentdb-search-indexer.md)témakört. 
- **Azure Blob-tárolóhoz**, beleértve a dokumentum következő formátumokat: PDF-fájlt, a Microsoft Office (DOCX/DOKUMENTUMOT, XSLX/XLS, PPTX/PPT, üzenet), HTML, XML, ZIP és egyszerű szöveges fájlok (beleértve a JSON). A célzott segédlet [ismertető](search-howto-indexing-azure-blob-storage.md)témakört.
- **Azure Táblatárolója**. A célzott segédlet [ismertető](search-howto-indexing-azure-tables.md)témakört.
     
Azt is megfontolni, hogy további adatforrások támogatása a jövőben. Segítse a következő döntések fontossági, adjon a visszajelzését az [Azure keresési Visszajelzési fórum](http://feedback.azure.com/forums/263029-azure-search).

Maximális számával kapcsolatos indexelő és az adatok forrása forrásokat is találhat [Szolgáltatás korlátai](search-limits-quotas-capacity.md) .

## <a name="typical-usage-flow"></a>Tipikus használatát továbbításához ##

Létrehozhat és kezeléséhez indexelő és az adatok egyszerű HTTP-kérések (BEJEGYZÉST, GET, helyezése, Törlés) keresztül ellen egy adott `data source` vagy `indexer` erőforrás.

Állítsa be az Automatikus indexelés általában egy négy lépésben:

1. Az indexelendő kell, hogy az adatokat tartalmazó adatforrás azonosítsa. Ne feledje, hogy Azure keresés nem támogatja az összes adatforrás bemutató adattípusok. [Támogatott adattípusok](https://msdn.microsoft.com/library/azure/dn798938.aspx) talál a listában.

2. Akinek séma az adatforrásban kompatibilis Azure keresési index létrehozása.
  
3. Hozzon létre egy Azure keresési adatforrásból, [Adatforrás létrehozása](#CreateDataSource)leírt módon.
  
4. Azure keresési indexkiszolgáló létrehozása leírt [Indexelő létrehozása](#CreateIndexer).

Meg kell terveznie egy indexelő minden cél index és az adatok forrása kombináció létrehozásával kapcsolatban. Lehet, hogy több indexelő ír be ugyanazt az indexet, és, így újból felhasználhatja több indexelő ugyanazon adatforráshoz. Indexkiszolgáló azonban igénybe vehet, csak egy adatforrást egyszerre, és csak egyetlen index létrehozása tud írni. 

Miután létrehozott egy indexelő, használja az [Indexelési állapot beolvasása](#GetIndexerStatus) művelet végrehajtása állapota meghallgathatja. Indexkiszolgáló (mellett vagy helyett rendszert futtató rendszeres időközönként) bármikor is futtathatja a [Futtatás indexelési](#RunIndexer) művelet használatával.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Adatforrás létrehozása ##

Azure keresés adatforrás használja az indexelő, index létrehozása a célhely az alkalmi vagy ütemezett Adatfrissítés a kapcsolat adatainak a megadásával. Létrehozhat egy új adatforrás az Azure keresési szolgáltatást a HTTP-bejegyzés felkérés belül.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Azt is megteheti használja a helyezése, és az adatforrás nevét adja meg a URI. Ha az adatforrás nem létezik, létrejön.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Megjegyzés**: az adatforrások maximális számát engedélyezett réteg árak változik. Az ingyenes szolgáltatás lehetővé teszi, hogy legfeljebb 3 adatforrásokhoz. Szokásos szolgáltatás lehetővé teszi, hogy az 50 adatforrásokhoz. Című cikkben találhat [Szolgáltatás](search-limits-quotas-capacity.md) további információt.

**Kérés**

HTTPS szükség az összes szolgáltatási kérelmet. Az **Adatforrás létrehozása** kérelem bejegyzés vagy helyezése módszerrel is kialakítani. BEJEGYZÉS használatakor, meg kell adnia egy adatforrás neve és az adatforrás-definíciót összehívás törzsébe. A helyezése a név része URL-CÍMÉT. Ha az adatforrás nem létezik, akkor jön létre. Ha már létezik, az új definíció frissül. 

Az adatforrás neve kell kell kisbetű, kezdje egy betűt vagy számot, nincs perjellel vagy pontot és lehet kisebb, mint a 128 karaktert. Után az adatforrás neve kezdve egy betűt vagy számot, a nevet a többi is elhelyezhet bármely betű, a szám és a szaggatott vonal, mindaddig, amíg a szaggatott vonal nem egymást követő állnak. A részletekért olvassa el a [névhasználati szabályok](https://msdn.microsoft.com/library/azure/dn857353.aspx) .

A `api-version` szükség. Az aktuális verzió `2015-02-28`.

**Fejlécek kérése**

Az alábbi lista bemutatja a kötelező és választható kérelem fejlécében. 

- `Content-Type`: Kötelező. Beállítani`application/json`
- `api-key`: Kötelező. A `api-key` hitelesítést végezni az értekezlet-összehívást a keresési szolgáltatás használják. Érték karakterlánc, egyedi a szolgáltatásban. Az **Adatforrás létrehozása** kérelem tartalmaznia kell egy `api-key` fejlécben a rendszergazda kulcs (nem pedig a lekérdezés billentyűt). 
 
A szolgáltatás neve a kérelem URL-címe összeállításához is szüksége lesz. A szolgáltatás neve elérheti és `api-key` az [Azure-portált](https://portal.azure.com/)a szolgáltatás irányítópultjáról. Lásd: [a portálon keresési szolgáltatásának létrehozása](search-create-service-portal.md) az Oldalválasztás segítségével.

<a name="CreateDataSourceRequestSyntax"></a>
**Összehívás törzsébe szintaxisa**

A kérés törzsében egy adatforrás-definíciót, amelyek tartalmazzák még a írja be az adatforrás hitelesítő adatait, és olvassa el az adatok, valamint egy nem kötelező adatok módosítása észlelési és az adatok törlését észlelési házirendek hatékony azonosítására használt módosultak vagy törli az adatforrással, a rendszeres ütemezett indexelő együtt használva. 

A kérés tartalom tagolásának szintaxisa az alábbi képlettel történik. Egy minta kérelmet a ebben a témakörben további megadva.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Kérés tartalmazza a következő tulajdonságokat: 

- `name`: Kötelező. Az adatforrás neve. Adatforrás nevét kell csak kisbetűket, számok vagy szaggatott vonalat tartalmaz, nem kezdődhet vagy végződhet gondolatjelek és a korlátozott 128 karaktert.
- `description`: Szerint egy leírást. 
- `type`: Kötelező. A támogatott típusú adatforrásokra egyike lehet:
    - `azuresql`-Az SQL azure adatbázis vagy SQL Server Azure VMs
    - `documentdb`-Azure DocumentDB
    - `azureblob`-Azure Blob-tárolóhoz
    - `azuretable`-Azure Táblatárolóhoz
- `credentials`:
    - A szükséges `connectionString` tulajdonság adja meg a kapcsolati karakterláncot az adatforráshoz. A kapcsolati karakterlánc formátumának attól függ, hogy az adatforrás típusa: 
        - Az SQL Azure Ez az a szokásos SQL Server-kapcsolati karakterláncot. Ha az Azure-portálra a kapcsolati karakterlánc történő használata esetén a `ADO.NET connection string` lehetőséget.
        - DocumentDB, a kapcsolati karakterlánc kell lennie a következő formátumban: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Az értékek mind szükség. Megtalálhatja őket az [Azure-portálon](https://portal.azure.com/).  
        - Az Azure Blob- és Táblatároló Ez az a tárterület-fiók kapcsolati karakterláncot. A formátum leírása [Itt](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). HTTPS-végpont protokollt szükség.  
- `container`, szükséges: Itt adhatja meg az indexelendő használatával az adatokat a `name` és `query` tulajdonságok: 
    - `name`, szükséges:
        - Az SQL Azure: Itt adhatja meg, táblákra és a. Séma minősített neve, például: használható `[dbo].[mytable]`.
        - DocumentDB: Itt adhatja meg a webhelycsoport. 
        - Azure Blob-tárolóhoz: Itt adhatja meg a tárhely tároló.
        - Azure Táblatárolóhoz: Adja meg annak a táblának a nevére. 
    - `query`, nem kötelező:
        - DocumentDB: lehetővé teszi, hogy egy lekérdezést, amely egy tetszőleges JSON dokumentumelrendezés összeolvasztja be egy egyszerű sémát, amely Azure keresési index is megadhatja.  
        - Azure Blob-tárolóhoz: lehetővé teszi a blob-tárolóban virtuális mappát ad meg. Például blob elérési út `mycontainer/documents/blob.pdf`, `documents` virtuális mappa is alkalmazható.
        - Azure Táblatárolóhoz: egy lekérdezést, amely az importálni kívánt sorok szűrőkészlettel megadását teszi lehetővé.
        - Az SQL Azure: a lekérdezés nem támogatott. Ha ezt a funkciót, kérjük, szavazzon [a javaslatok](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- A választható `dataChangeDetectionPolicy` és `dataDeletionDetectionPolicy` tulajdonságai az alábbiakban ismertetjük.

<a name="DataChangeDetectionPolicies"></a>
**Adatok módosítása észlelési házirendek**

Az adatok módosítása észlelési házirend célja módosult adatelemeket hatékony azonosítása. Támogatott házirendek adatforrástípus függ. Az alábbi szakaszok bemutatják a minden házirend. 

***Magas vízjel módosítása észlelési házirend*** 

A házirend használata, ha az adatforrás tartalmaz, egy oszlop vagy a következő feltételeknek megfelelő tulajdonság:
 
- Az összes szúr be az oszlop értékének meghatározása. 
- Egy elem összes frissítését is módosíthatja az oszlop értékét. 
- Az e oszlop értéke az egyes módosítások megnöveli.
- A szűrő záradék a következőhöz hasonló használó lekérdezések `WHERE [High Water Mark Column] > [Current High Water Mark Value]` hatékony futtatható.

Ha például Azure SQL adatforrásokból az indexelt használata esetén `rowversion` oszlop a ideális jelölt magas víz megjelölés házirend való használatra. 

Ezzel a házirenddel módon kell megadni:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Adatforrások DocumentDB használatakor kell használnia a `_ts` DocumentDB által biztosított tulajdonság. 

> [AZURE.NOTE] Azure Blob adatforrásokhoz, Azure keresési automatikusan használata esetén használja a sürgős vízjelet észlelési házirendjének módosítása egy blob utoljára módosította időbélyeg; alapján nem kell saját maga ilyen házirend megadásához.   

***SQL integrált észlelési házirend***

Ha az SQL-adatbázis támogatja a [változások követése](https://msdn.microsoft.com/library/bb933875.aspx), SQL integrált módosítása követés házirend használata ajánlott. Ezzel a házirenddel lehetővé teszi, hogy a lehető leghatékonyabb a változások követésének, és lehetővé teszi, hogy a Azure keresési azonosítása törölt sorok nélkül szeretné, hogy egy oszlop explicit "finom törlése" a sémában.

Integrált a változások követésének kezdve az alábbi SQL Server-adatbázis verziók támogatott műveletek: 
- SQL Server 2008 R2, ha az SQL Server Azure VMs használ.
- Azure SQL adatbázis V12, Azure SQL-adatbázis használata.  

Ha SQL integrált változások követése házirend használata, nem ad meg egy külön adatok törlési észlelési házirend - e házirendje rendelkezik beépített támogatása azonosító törölt sorok. 

Ezzel a házirenddel csak akkor használható táblázatokkal; a nézetek nem használhatók. Szükséges ahhoz, hogy a változások követésének a táblázatot, mielőtt használatba vehetné a házirend használ. [Engedélyezése és letiltása a változások követésének](https://msdn.microsoft.com/library/bb964713.aspx) talál utasításokat.    
 
Ha az **Adatforrás létrehozása** kérést, integrált SQL módosítása a nyomon követés házirend tagolásának adható meg az alábbi képlettel történik:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Adatok törlését észlelési házirendek**

Az adatok törlését észlelési házirend célja hatékony az adatokat a törölt elemek azonosítása. Az egyetlen támogatott házirend jelenleg a `Soft Delete` szabályt, amely lehetővé teszi, hogy a törölt elemek értékének alapján azonosítása egy `soft delete` oszlop- vagy az adatforrás tulajdonság. Ezzel a házirenddel módon kell megadni:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**Megjegyzés:** Csak a karakterlánc, egész vagy logikai értékeket tartalmazó oszlopok támogatottak. A használt érték `softDeleteMarkerValue` karakterláncot, kell megadni, akkor is, ha a megfelelő oszlopban tartalmazza az egész számok vagy logikai értékek. Ha például az adatforrásban megjelenő értéke 1, ha az `"1"` , a `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Példák a szervezet**

Ha azt szeretné, az adatforrás használata időközönként futtató indexkiszolgáló, ebben a példában megadása, módosítása és törlése észlelési házirendek: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Ha csak kíván használni az adatforrás egyszeri másolatot az adatokról, nem hagyható a házirendek:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Válasz**

A sikeres kérésekben: 201 létrehozva. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Adatforrás módosítása ##

Létező adatforrás HTTP ELHELYEZNI felkérés használatával frissítheti. Az adatforrás frissítése URI kérésre nevének megadása:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

A `api-version` szükség. Az aktuális verzió `2015-02-28`. [Azure keresési verziószámozás](https://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és helyettesítő verziók további információt tartalmaz.

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Kérés**

A kérés szervezet szintaxisa ugyanaz, mint a [Adatforrás létrehozása](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] Létező adatforrás néhány tulajdonság nem módosíthatók. Létező adatforrás típusú például nem módosítható.  

> [AZURE.NOTE] Ha nem szeretné a kapcsolati karakterlánc, egy már létező adatforrás módosítása, megadhatja a literál `<unchanged>` a kapcsolati karakterlánc. Ezek a funkciók azokról a helyzetekről, ahol frissíteni kell a adatok forrás, de nincs kényelmes hozzáférése a kapcsolati karakterláncot, mivel ez éppen biztonsággal adatokat.

**Válasz**

A sikeres kérésekben: 201 létrehozva, ha egy új adatforrás volt létrehozott és 204 Nincs tartalom beállítási Ha létező adatforrás frissült.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Adatforrások felsorolása ##

Az **Adatforrások felsorolása** művelet az Azure keresési szolgáltatás az adatforrások listáját adja eredményül. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

A `api-version` szükség. Az aktuális verzió `2015-02-28`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

A sikeres kérésekben: 200 az OK gombra.

Íme egy példa válasz szervezet:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Figyelje meg, hogy a választ, amely érdekli tulajdonságait le is szűrheti. Ha például ha azt szeretné, hogy csak adatforrás-nevében listáját, használja az OData `$select` lekérdezés lehetőséget:

    GET /datasources?api-version=205-02-28&$select=name

Ebben az esetben a fenti példa válasza tűnik, az alábbi képlettel történik: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Ez a sávszélesség mentheti, ha rendelkezik az adatforrások sok a keresési szolgáltatás egy hasznos technika.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Adatforrás beszerzése ##

Az **Adatforrás első** művelet megszerzi Azure keresési az adatforrás-definíciót.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

A `api-version` szükség. Az aktuális verzió `2015-02-28`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

[Adatforrás létrehozása példa kérések](#CreateDataSourceRequestExamples)szereplő példák a kérdésre adott választ hasonlít: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Nincs beállítva a `Accept` kérelem fejléc `application/json;odata.metadata=none` mikor elérje a API-t, így okoz `@odata.type` attribútum szerepelni a válaszban, és nem tudja adatainak módosítása és adatok törlési észlelési házirendek különböző adattípusokra közötti különbséget. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Adatforrás törlése ##

Az Azure keresési szolgáltatás adatforrás az **Adatforrás törlése** művelet eltávolítja.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Ha bármely indexelő töröl adatforrásra hivatkozik, a törlési művelet továbbra is folytatódik. Azonban ezeket az indexelő fog lehet a következő futtatása után egy hiba állapotba.  

A `api-version` szükség. Az aktuális verzió `2015-02-28`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

Állapotkód: 204 Nincs tartalom beállítási jelez sikeres válaszra várni.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Indexelő létrehozása ##

Létrehozhat egy új indexelő az Azure keresési szolgáltatást a HTTP-bejegyzés felkérés belül.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Azt is megteheti használja a helyezése, és az adatforrás nevét adja meg a URI. Ha az adatforrás nem létezik, létrejön.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Réteg árak változik az indexelő engedélyezett maximális számát. Az ingyenes szolgáltatás lehetővé teszi, hogy az indexelő legfeljebb 3. Szokásos szolgáltatás lehetővé teszi, hogy az 50 indexelő. Című cikkben találhat [Szolgáltatás](search-limits-quotas-capacity.md) további információt.

A `api-version` szükség. Az aktuális verzió `2015-02-28`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.


<a name="CreateIndexerRequestSyntax"></a>
**Összehívás törzsébe szintaxisa**

A kérés törzsében az indexelő-definíciót, amely megadja az adatforrás és az indexelő, valamint a választható indexelés ütemezése, és a paraméterek cél index tartalmazza. 


A kérés tartalom tagolásának szintaxisa az alábbi képlettel történik. Egy minta kérelmet a ebben a témakörben további megadva.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Indexelő ütemezése**

Másik lehetőségként indexkiszolgáló ütemezést is megadhat. Ütemezett esetén az indexelő ütemezve rendszeres fog futni. A következő attribútumok ütemezés foglalja magában:

- `interval`: Kötelező. Időtartam érték, amely megadja egy időköz vagy az indexelő időszakot fut. Megengedett legkisebb intervallum 5 perccel; egy nappal leghosszabb. Akkor XSD "dayTimeDuration" értelmezi (korlátozott részhalmazát az [ISO 8601 időtartam](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) érték) lesz formázva. Ezt a minta: `"P[nD][T[nH][nM]]"`. Példa: `PT15M` a 15 percenként `PT2H` minden 2 órán keresztül. 

- `startTime`: Kötelező. Egy UTC datetime indításakor az indexelő kell operációs rendszert futtató. 

**Indexelő paraméterei**

Indexkiszolgáló tetszés szerint megadhatja a működése befolyásoló több paramétert. A paraméterek mindegyikében nem kötelező.  

- `maxFailedItems`: Indexelendő előtt az indexelő Futtatás számít hiba történhet elemek száma. Alapértelmezett érték 0. Az [Indexelési állapot beolvasása](#GetIndexerStatus) művelet sikertelen elemek információt adja vissza. 

- `maxFailedItemsPerBatch`: Az elemek száma, amelyek minden tételt, mielőtt az indexelő Futtatás indexelendő történhet tekintendő hibát. Alapértelmezett érték 0.

- `base64EncodeKeys`: Itt adhatja meg, függetlenül attól dokumentum billentyűk lesz-e kódolt alap 64. Azure keresési megszorításokat karakterekkel, amely szerepel egy dokumentum billentyűt. A forrásadatok értékeket azonban érvénytelen karaktereket is tartalmazhat. Szükség esetén a tárgymutató-dokumentum kulcsként ezeket az értékeket, a jelző beállítható, hogy igaz. Az alapértelmezett érték `false`.

- `batchSize`: Itt adhatja meg, amelyek az adatforrás olvassák és indexelt elemek száma egy kötegben teljesítmény javítása érdekében. Az adatforrás típusa függ, hogy az alapértelmezett: az Azure SQL- és DocumentDB 1000 és Azure Blob-tárolóhoz 10.

**A mező megfeleltetésének**

A mező nevét az adatforrás hozzárendelése egy másik mező nevét a cél index mező megfeleltetésének is használhatja. Például érdemes megvizsgálni a forrástáblához mezővel `_id`. Azure keresés nem engedélyezi az aláhúzásjel kezdve, így a mezőt kell átnevezni mező nevét. Ehhez használja a `fieldMappings` az alábbiak szerint az indexelő tulajdonságlapján: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Több mező megfeleltetésének adhatja meg: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Forrás- és célwebhelyek mezőnevek és nagybetűk között.

<a name="FieldMappingFunctions"></a>
***Mező a hozzárendelés függvények***

A mező megfeleltetésének is használható *megfeleltetés*függvényeivel mező forrásértékeket átalakítása.

Csak egy ilyen funkció jelenleg támogatott: `jsonArrayToStringCollection`. A cél tárgymutató Collection(Edm.String) mezőkbe JSON tömbként formázott karakterláncot tartalmazó mező értelmezi azt. Célja Azure SQL-indexelő való használatra különösen, mivel SQL nem tartozik egy natív webhelycsoport adattípust. Szolgál az alábbi képlettel történik: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Ha például a forrás mező tartalmazza a karakterlánc `["red", "white", "blue"]`, majd a célmező típusa `Collection(Edm.String)` tölti fel a három értékekkel `"red"`, `"white"` és `"blue"`.

Megjegyzendő, hogy a `targetFieldName` tulajdonság nem kötelező; balra, ha a `sourceFieldName` értékét használja. 

<a name="CreateIndexerRequestExamples"></a>
**Példák a szervezet**

Az alábbi példa létrehoz egy indexelő, amely a táblázat által hivatkozott adatokat másolja a `ordersds` az adatforrás a `orders` index időközönként a január 1, a Skype 2015 UTC kezdődő és óránként lefut. Minden egyes indexelő meghívási sikeres, ha indexelni az egyes köteg legfeljebb 5 elem nem lesz, és legfeljebb 10 elem nem indexelhetők teljes. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Válasz**

A sikeres kérésekben 201 létrehozva.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Indexelő frissítése ##

Egy meglévő indexelő HTTP ELHELYEZNI felkérés használatával módosíthatja. Az indexelő URI kérésre frissítése nevének megadása:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

A `api-version` szükség. Az aktuális verzió `2015-02-28`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Kérés**

A kérés szervezet szintaxisa ugyanaz, mint a [Indexelő létrehozása](#CreateIndexerRequestSyntax).

**Válasz**

A sikeres kérésekben: 201 létrehozva, ha egy új indexelő volt létrehozott és 204 Nincs tartalom beállítási Ha egy meglévő indexelő frissült.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Lista indexelő ##

A **Lista indexelő** művelet indexelő listája az Azure keresési szolgáltatás adja eredményül. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


A `api-version` szükség. Az előzetes verzió `2015-02-28-Preview`. [Azure keresési verziószámozás](https://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és helyettesítő verziók további információt tartalmaz.

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

A sikeres kérésekben: 200 az OK gombra.

Íme egy példa válasz szervezet:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Figyelje meg, hogy a választ, amely érdekli tulajdonságait le is szűrheti. Ha például ha azt szeretné, hogy csak az indexelő nevek listáját, használja az OData `$select` lekérdezés lehetőséget:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

Ebben az esetben a fenti példa válasza tűnik, az alábbi képlettel történik: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Ez a egy hasznos technika mentenie a sávszélesség, ha a keresési szolgáltatás az indexelő sok van.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Indexelő beszerzése ##

Az **Első indexelési** művelet az indexelő definition megszerzi Azure keresés.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

A `api-version` szükség. Az előzetes verzió `2015-02-28-Preview`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

Állapotkód: 200 OK jelez sikeres válaszra várni.

Példák az [Indexelő létrehozása példa kérelmek](#CreateIndexerRequestExamples)a kérdésre adott választ hasonlít: 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Indexelő törlése ##

A **Törlés indexelési** művelet eltávolítja az Azure keresési szolgáltatás indexkiszolgáló.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Indexkiszolgáló törlésekor az indexelő végrehajtások folyamatban lévő adott időben futnak száma, de nincs további végrehajtások ütemezése fog. Egy nem létező indexelő használni próbál eredményezi HTTP állapotkód 404 nem található. 
 
A `api-version` szükség. Az előzetes verzió `2015-02-28-Preview`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

Állapotkód: 204 Nincs tartalom beállítási jelez sikeres válaszra várni.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Indexelő futtatása ##

Nemcsak a operációs rendszert futtató rendszeres időközönként, indexkiszolgáló is is hivatkozhat a **Futtatás indexelési** művelet keresztül igény: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

A `api-version` szükség. Az előzetes verzió `2015-02-28-Preview`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

Állapotkód: 202 elfogadott jelez sikeres válaszra várni.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Ismerkedés az indexelési állapot ##

Az **Indexelési állapot beolvasása** művelet olvassa be az aktuális állapot és a végrehajtás előzményeihez indexkiszolgáló: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


A `api-version` szükség. Az előzetes verzió `2015-02-28-Preview`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

Állapotkód: az OK gombra a sikeres válaszra 200.

A válasz szervezet általános indexelési állapot állapot, utolsó indexelő meghívását, valamint a legutóbbi indexelő meghívásához előzményeihez információkat tartalmaz, (ha van ilyen). 

A minta válasz szervezet néz ki: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Az indexelő állapotának**

Az indexelő állapotának a következő értékek egyike lehet:

- `running`azt jelzi, hogy az indexelő általában fut-e. Figyelje meg, hogy az indexelő végrehajtások részét előfordulhat, hogy továbbra is meghiúsul, így célszerű ellenőrizni a `lastResult` , valamint a tulajdonságot. 

- `error`azt jelzi, hogy az indexelő hibát, amely nem kell javítani az emberi beavatkozás nélkül. Például az adatforrások hitelesítő adatainak valószínűleg lejárt, vagy a séma az adatforrás vagy a Céllista index változott a legújabb módját. 

**Indexelő végrehajtás eredménye**

Egy indexelési végrehajtás eredménye egy egyetlen indexelő végrehajtás információkat tartalmazza. A legújabb eredményt akkor jelenik meg a `lastResult` az indexelési állapot tulajdonsága. Más legutóbbi, ha jelen van, a visszaadott eredmény a `executionHistory` az indexelési állapot tulajdonsága. 

Indexelő végrehajtás eredménye a következő tulajdonságokat tartalmazza: 

- `status`: az adatvégrehajtás állapotát. Lásd: [Az indexelő végrehajtás állapotának](#IndexerExecutionStatus) alatti további információt. 

- `errorMessage`: hibaüzenet a hibás végrehajtásához. 

- `startTime`: az idő UTC szerint megadva, ez az adatvégrehajtás indításakor.

- `endTime`: az idő UTC szerint megadva, amikor a végrehajtás befejeződött. Ez az érték, ha még folyamatban van a végrehajtás nincs beállítva.

- `errors`: elemszintű hibák, ha vannak ilyenek tömbje. Mindegyik listaelem dokumentum kulcsot tartalmaz (`key` tulajdonság) és a megjelenő hibaüzenet (`errorMessage` tulajdonság). 

- `itemsProcessed`: az adatforrás-elemek (például a táblázatsorok) az indexelő megpróbált tárgymutató-e a végrehajtás során az száma. 

- `itemsFailed`: Ez a végrehajtás során nem sikerült elemszámát.  
 
- `initialTrackingState`: mindig `null` első indexelő végrehajtása, vagy ha az adatok módosítása a nyomon követés házirend használt adatforrás nincs engedélyezve. Ha ilyen házirend engedélyezve van, a későbbi végrehajtások Ez az érték azt jelzi, az első (legalacsonyabb) a változások követésének a végrehajtás által feldolgozott érték. 

- `finalTrackingState`: mindig `null` használt adatforrás nincs engedélyezve, ha az adatokat a nyomon követés házirendjének módosítása. Egyéb esetben azt jelzi, hogy a legújabb (legnagyobb érték közül) a változások követésének sikeresen dolgozza fel a végrehajtás értéket. 

<a name="IndexerExecutionStatus"></a>
**Indexelő végrehajtási állapota**

Az indexelő végrehajtás állapotának rögzíti egy egyetlen indexelő végrehajtási állapotát. Lehet, hogy a következő értékeket:

- `success`azt jelzi, hogy az indexelő végrehajtás sikeresen befejeződött.

- `inProgress`azt jelzi, hogy az indexelő végrehajtás folyamatban van. 

- `transientFailure`azt jelzi, hogy az indexelő végrehajtás sikertelen volt. Lásd: `errorMessage` tulajdonság további információt. Előfordulhat, hogy a hibát, vagy előfordulhat, hogy nincs szükség az emberi beavatkozás javítás – például elhárításához a séma nem kompatibilis az adatforrás és a cél index között kell felhasználói művelet, egy ideiglenes adatok forrása legrövidebb leállás azonban nem. Ha ilyen indexelő meghívásához egy ütemtervet, továbbra is. 

- `persistentFailure`azt jelzi, hogy az indexelő oly módon, hogy csak az emberi beavatkozás nem sikerült. Ütemezett indexelő végrehajtások leállítása parancsra. Kijavítja a hibát, miután alaphelyzetbe állítása az indexelő API segítségével indítsa újra az ütemezett végrehajtások. 

- `reset`azt jelzi, hogy az indexelő alaphelyzetbe hívásával alaphelyzetbe állítása az indexelő API-hoz (lásd alább). 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Indexelő alaphelyzetbe állítása ##

Az **Új indexelési** művelet a változáskövetési állapot az indexelő társított alaphelyzetbe állítása. Lehetővé teszi, hogy az elindítani a teljesen új újraindexelés (például az adatok forrása séma megváltozott), vagy az indexelő társított adatforrás adatainak módosítása észlelési házirend módosítása.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

A `api-version` szükség. Az előzetes verzió `2015-02-28-Preview`. 

A `api-key` (nem pedig a lekérdezés kulcs)-rendszergazdai kulcsnak kell lennie. Olvassa el a [Keresési szolgáltatás REST API -val](https://msdn.microsoft.com/library/azure/dn798935.aspx) kapcsolatos további hitelesítési című részére. [A portál keresési szolgáltatásának létrehozása](search-create-service-portal.md) szolgáltatás URL-CÍMÉT és a kérésben használt kulcsfontosságú tulajdonságokat ismerteti.

**Válasz**

Állapotkód: 204 Nincs tartalom sikeres válaszra várni.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>SQL-adattípusok és Azure keresési adattípusok közötti hozzárendelés ##

<table style="font-size:12">
<tr>
<td>SQL-adattípus</td>  
<td>Engedélyezett cél index mezőtípusok</td>
<td>Jegyzetek</td>
</tr>
<tr>
<td>bit</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>int és smallint, tinyint</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>valós, lebegtetés</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>megjelenítésekor, a rendszer pénzt<br>decimális<br>numerikus
</td>
<td>Edm.String</td>
<td>Azure keresés nem támogatja a decimális típusú átalakítása Edm.Double, mert ez elveszít pontosság
</td>
</tr>
<tr>
<td>karakter, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>Lásd: a [Mező a hozzárendelés függvények](#FieldMappingFunctions) karakterlánc oszlop alakíthat ki egy Collection(Edm.String) kattintva olvashat a dokumentumban</td>
</tr>
<tr>
<td>smalldatetime, a dátum és idő, a datetime2, a dátum, a datetimeoffset</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>földrajzi hely</td>
<td>Edm.GeographyPoint</td>
<td>Támogatott típusú SRID 4326 (Ez az alapértelmezett), csak földrajzi példányok</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>A #HIÁNYZIK</td>
<td>Sor verziójú oszlopok nem tárolhatók a keresési index, de is használható a változások követése</td>
</tr>
<tr>
<td>idő, időszak<br>bináris, varbinary, kép,<br>XML, mértani, CLR típusai</td>
<td>A #HIÁNYZIK</td>
<td>Nem támogatott</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Megfeleltetés JSON adattípusok és Azure keresési adattípusok között ##

<table style="font-size:12">
<tr>
<td>JSON adattípus</td> 
<td>Engedélyezett cél index mezőtípusok</td>
<td>Jegyzetek</td>
</tr>
<tr>
<td>logikai</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Az integrál téma számok</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Lebegőpontos szám</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>karakterlánc</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>egyszerű tömbök típusok, például ["a", "b"; "c"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Karakterláncok, amelyek így néznek dátumok</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON pont objektumok</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON pontok JSON objektumok a következő formátumban: {"típus": "Pont", "koordináták": [hosszú, lat]} </td>
</tr>
<tr>
<td>Más JSON-objektumok</td>
<td>A #HIÁNYZIK</td>
<td>Nem támogatott. Azure keresési jelenleg csak a egyszerű típusokhoz és a karakterlánc-webhelycsoportok támogatja</td>
</tr>
</table>