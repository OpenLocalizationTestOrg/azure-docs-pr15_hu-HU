<properties
pageTitle="Az Azure keresés indexelési Azure Táblatárolóhoz"
description="Megtudhatja, hogy miként indexelni az Azure keresés Azure táblákban tárolt adatok"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Az Azure keresés indexelési Azure Táblatárolóhoz

Ez a cikk bemutatja, hogyan Azure kereséssel Azure Táblatárolóhoz tárolt adatait indexelni. Az új Azure keresési tábla indexelő ellenőrzi ezt a folyamatot, gyors és zökkenőmentesen elvégezhető. 

> [AZURE.IMPORTANT] Ez a funkció jelenleg előzetes verzióban. Akkor érhető el, csak **2015-02-28 – előzetes** verzió használatával REST API-val és a .NET SDK verzió 2.0-előnézetét. Kérjük, ne feledje, előnézete API-k tesztelése és az kiértékelés szánt, és nem használható éles környezetben.

## <a name="setting-up-azure-table-indexing"></a>Azure-táblából az indexelés beállítása

Állíthatja be, és állítsa be az Azure-táblából az indexelő, az Azure keresési REST API segítségével létrehozása és kezelése az **Indexelő** és **adatforrások** [az indexelési műveletek](https://msdn.microsoft.com/library/azure/dn946891.aspx)leírt módon. A .NET SDK [2.0 – előzetes verzió](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) is használhatja. A jövőben támogatása, a táblázat indexelés, hozzáadódik az Azure-portálra.

Adatforrás indexelése, az adatok és a házirendek az Azure-keresés (új, módosítás vagy törölt sorok) az adatok változásai hatékony azonosítása engedélyezése eléréséhez szükséges hitelesítő adatok megadása

Indexkiszolgáló adatforrásból származó adatokat olvas be, és betölti azt be a cél keresési index.

Táblázat indexelés beállítása:

1. Adatforrás létrehozása
    - Állítsa a `type` paramétere`azuretable`
    - A tároló fiók kapcsolati karakterláncot, mint a sikeres a `credentials.connectionString` paraméter
    - Adja meg, hogy a táblázat neve használata a `container.name` paraméter
    - Tetszés szerint adja meg, a lekérdezés használatával a `container.query` paraméter. Amikor csak lehetséges, használjon szűrőt PartitionKey a legjobb teljesítmény elérése érdekében; bármilyen más lekérdezést egy teljes táblában képolvasóról, amely a nagy táblázatok gyenge teljesítményt eredményezhet eredményezi.
2. A keresési index létrehozása, amely megfelel a tárgymutató-kívánt tábla oszlopainak a sémában. 
3. Kapcsolódás az adatforráshoz a keresési index létrehozása az indexelő.

### <a name="create-data-source"></a>Adatforrás létrehozása

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Az adatforrás létrehozása API bővebben lásd: [Adatforrás létrehozása](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Index létrehozása 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Az Index létrehozása API bővebben lásd: [Index létrehozása](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Indexelő létrehozása 

Végül hozza létre az indexelő, amely az adatforrás és a cél index hivatkozik. Példa:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Az indexelő létrehozása API további információra kíváncsi tanulmányozza [Az indexelő létrehozása](search-api-indexers-2015-02-28-preview.md#create-indexer).

Ez az összes van, és-boldog indexelés!

## <a name="dealing-with-different-field-names"></a>Különböző mező nevek kezelése

A mezőnevek a már meglévő indexet gyakran, a táblázatban a tulajdonságok neve eltér lesz. A tulajdonságok neve az értékeket a mező nevére a keresési index megfeleltetéséhez **mező megfeleltetésének** is használhatja. Mező megfeleltetésének kapcsolatos további információért olvassa el a [Azure keresési indexelő mező megfeleltetésének egymással, áthidalhatják a adatforrások és a keresési index közötti különbségek](search-indexer-field-mappings.md)című témakört.

## <a name="handling-document-keys"></a>Dokumentum kulcsok kezelése

Azure keresés a dokumentum kulcs egyedileg kell azonosítania a dokumentum. Minden keresési index rendelkeznie kell típusú pontosan egy kulcsmező `Edm.String`. A kulcs mezőjét szükség az indexhez felvett minden dokumentum (valójában akkor a kizárólag a szükséges mező).

Táblázatsorok telepítve van az összetett kulcs, mivel az Azure keresési hoz létre egy szintetikus nevű mező `Key` , hogy ez egy összefűzése partíciót billentyűt, és a sor kulcs értékeit. Például ha egy sor PartitionKey is `PK1` és RowKey `RK1`, majd `Key` mezőérték lesz `PK1RK1`. 

> [AZURE.NOTE] A `Key` érték tartalmazhatnak, amelyek a dokumentum kulcsok, például a szaggatott vonal érvénytelen karakterek. Akkor is foglalkozik érvénytelen karakterek használatával a `base64Encode` [mező a hozzárendelés függvény](search-indexer-field-mappings.md#base64EncodeFunction). Ha így tesz, akkor ne felejtse el is használhatja az URL-cím vannak Base64 kódolás, ha például a keresési dokumentum billentyűk áthaladó API felhívja.

## <a name="incremental-indexing-and-deletion-detection"></a>Növekményes indexelés és a törlési kimutatására
 
Egy táblázat indexelő időközönként futtatásához beállításakor azt reindexes csak az új vagy frissített sorok egy sor által meghatározott `Timestamp` értéket. Nincs módosítás észlelési házirend megadásához – Növekményes indexelés engedélyezett-e meg automatikusan. 

Jelzi, hogy bizonyos dokumentumokat el kell távolítani az index, akkor – finom törlés stratégia törlés sor helyett, azt jelzik, hogy törli, és kattintson az adatforrás finom törlés észlelési házirend beállítása tulajdonság felvétele. Például az alább látható módon házirend meg fogjuk vizsgálni, hogy egy sor a program törli, ha egy tulajdonság `IsDeleted` értékkel `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Segítse a szolgáltatás Azure keresési továbbfejlesztésében

Ha kérhetek új funkciókat vagy javítása ötleteket, kérjük, keresse meg a kapcsolatfelvételi a [UserVoice webhelyen](https://feedback.azure.com/forums/263029-azure-search/).