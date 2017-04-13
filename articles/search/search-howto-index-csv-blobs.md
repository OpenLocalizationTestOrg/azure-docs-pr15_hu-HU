<properties
pageTitle="A keresés Azure blob-indexelő CSV BLOB indexelés |} Microsoft Azure"
description="Megtudhatja, hogy miként index CSV BLOB az Azure-keresés"
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
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>A keresés Azure blob-indexelő CSV BLOB indexelés 

Alapértelmezés szerint [keresési Azure blob-indexelő](search-howto-indexing-azure-blob-storage.md) elemzi tagolt szöveg BLOB mint a szöveg egyetlen adattömb. Azonban CSV-adatokat tartalmazó okkal gyakran szeretné kezelni a blob különálló dokumentum minden sor. Ha például adott, a következő tagolt szöveget: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

érdemes lehet elemezni a 2 dokumentumokba minden tartalmazó "azonosító", "datePublished" és "kódok" mezőket.

Ebben a cikkben megtanulhatja, hogyan CSV BLOB a keresési Azure blob-indexkiszolgáló elemzésére. 

> [AZURE.IMPORTANT] Ez a funkció jelenleg előzetes verzióban. Akkor érhető el, csak a **Skype 2015-02-28 – előzetes**verzió használatával REST API-t. Kérjük, ne feledje, előnézete API-k tesztelése és az kiértékelés szánt, és nem használható éles környezetben. 

## <a name="setting-up-csv-indexing"></a>Állítsa be a CSV-indexelés

Tárgymutató-CSV BLOB, hozzon létre vagy frissítse az indexelő definícióját, a a `delimitedText` elemzése módban:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Az indexelő létrehozása API további információra kíváncsi tanulmányozza [Az indexelő létrehozása](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`azt jelzi, hogy minden blob (nem üres) az első sort tartalmaz-e a fejléceket.
Ha BLOB-kezdeti fejléc sor nem tartalmaznak, a fejlécek az indexelő beállítás kell megadni: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Jelenleg csak a UTF-8 kódolást használata támogatott. Is csak a vessző `','` támogatott, az elválasztó karaktert. Ha segítségre egyéb kódolások és határoló, tudassa velünk [UserVoice](https://feedback.azure.com/forums/263029-azure-search)webhelyünkön.

> [AZURE.IMPORTANT] A tagolt szöveges mód elemzése használatakor Azure keresési feltételezi, hogy, hogy az adatforrásban valamennyi BLOB lesz CSV. Ha kombinálja a CSV- és nem CSV-BLOB támogatja az azonos adatforrás van szüksége, tudassa velünk [UserVoice](https://feedback.azure.com/forums/263029-azure-search)webhelyünkön.

## <a name="request-examples"></a>Példák kérése

Ez az összes elhelyez együtt, Íme a teljes tartalom példa. 

Adatforrás: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexelő:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Segítse a szolgáltatás Azure keresési továbbfejlesztésében

Ha kérhetek új funkciókat vagy javítása ötleteket, kérjük, keresse meg a kapcsolatfelvételi a [UserVoice webhelyen](https://feedback.azure.com/forums/263029-azure-search/).