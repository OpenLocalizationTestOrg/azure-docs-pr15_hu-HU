<properties
    pageTitle="A mező összekötő hozzáadása az összefüggés-alkalmazások |} Microsoft Azure"
    description="A mező összekötő REST API-paraméterekkel áttekintése"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>A mező összekötő – első lépések
Csatlakozhat jelölőnégyzetet, és hozza létre a fájlokat, fájlok törlése és az egyéb. 

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik.

Mező a következőkre van lehetősége:

- Hozza létre az üzleti folyamat kap mezőbe az adatok alapján. 
- Amikor létrejön vagy frissül egy fájl, használja a indítók.
- Fájl másolása, törlése egy fájlt, és a további műveletek használata Az alábbi műveletek kap választ, és végezze el a kimenet rendelkezésre álló további lehetőségeket. Például fájl módosításakor a jelölőnégyzetet is, hogy a fájl készítése és e-mailben elküldheti az Office 365.

Logika alkalmazások művelet hozzáadásához lásd: [a összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek
Mező tartalmazza a következő eseményindító és műveleteket.

| Eseményindítók | Műveletek|
| --- | --- |
|<ul><li>Ha egy fájl jön létre</li><li>Egy fájl módosításának</li></ul> | <ul><li>Fájl létrehozása</li><li>Ha egy fájl jön létre</li><li>Fájl másolása</li><li>Fájl törlése</li><li>Bontsa ki az archiválási mappába</li><li>Get-fájl tartalmának azonosítójuk segítségével</li><li>Get-fájl tartalmának elérési út segítségével</li><li>Fájl metaadatait azonosítójával beszerzése</li><li>Ismerkedés a metaadatok fájl elérési útja</li><li>Fájl frissítése</li><li>Egy fájl módosításának</li></ul>

Összekötők JSON és az XML formátumú támogatja az adatokat.

## <a name="create-a-connection-to-box"></a>Hozzon létre egy kapcsolatot mezőben
Amikor ilyen összekötőt vesz fel a összefüggés-alkalmazásokat, engedélyeznie kell logika alkalmazások csatlakozhat a jelölőnégyzetet.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Miután létrehozta a kapcsolatot, írja be a tulajdonságok. Ez a témakör a **REST API-hivatkozás** e-tulajdonságokat ismerteti.

>[AZURE.TIP] Az egyéb összefüggés-alkalmazásokban a azonos mezőben kapcsolat is használhatja.

## <a name="swagger-rest-api-reference"></a>Swagger REST API-hivatkozás
Verziójára vonatkozik: 1.0.

### <a name="create-file"></a>Fájl létrehozása
Feltölti fájl mezőbe.  
```POST: /datasets/default/files```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappa_útvonala|karakterlánc|igen|lekérdezés|Nincs lehetőség |Mappa elérési útja mezőben a fájl feltöltése|
|név|karakterlánc|igen|lekérdezés|Nincs lehetőség |A mezőben a fájl neve|
|szervezet|String(Binary) |igen|szervezet|Nincs lehetőség |A fájl feltöltése a mező tartalma|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="when-a-file-is-created"></a>Ha egy fájl jön létre
Eseményindítók egy folyamat, új fájl létrehozásakor mezőben mappában.  
```GET: /datasets/default/triggers/onnewfile```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappaazonosító|karakterlánc|igen|lekérdezés|Nincs lehetőség |Egyedi azonosítója mezőbe, a mappában.|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="copy-file"></a>Fájl másolása
Fájl másolása mezőbe.  
```POST: /datasets/default/copyFile```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|forrás|karakterlánc|igen|lekérdezés|Nincs lehetőség |URL-címét a forrásfájl|
|cél|karakterlánc|igen|lekérdezés| Nincs lehetőség|Cél fájl elérési útja mezőben, beleértve a célfájl neve|
|felülírása|logikai érték|nem|lekérdezés| Nincs lehetőség|Felülírja a célfájl, ha "true" értékre van állítva|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="delete-file"></a>Fájl törlése
Fájl törlése a listából.  
```DELETE: /datasets/default/files/{id}```


| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|Nincs lehetőség |Egyedi azonosítója mezőben törölni szeretné a fájlt.|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="extract-archive-to-folder"></a>Bontsa ki az archiválási mappába
Az archív mappák mappába mezőben az első karaktertől (Példa: .zip).  
```POST: /datasets/default/extractFolderV2```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|forrás|karakterlánc|igen|lekérdezés| |Az archiválás fájl elérési útja|
|cél|karakterlánc|igen|lekérdezés| |Archiválás tartalmát kiolvasó mezőben elérési út|
|felülírása|logikai érték|nem|lekérdezés| |Felülírja a cél fájlokat, ha "true" értékre van állítva|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-file-content-using-id"></a>Get-fájl tartalmának azonosítójuk segítségével
Fájl tartalmának beolvassa az azonosító mezőbe.  
```GET: /datasets/default/files/{id}/content```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|Nincs lehetőség |Egyedi azonosítója mezőben a fájlt.|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-file-content-using-path"></a>Get-fájl tartalmának elérési út segítségével
Fájl tartalmának beolvassa az elérési út mezőbe.  
```GET: /datasets/default/GetFileContentByPath```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|elérési út|karakterlánc|igen|lekérdezés|Nincs lehetőség |Egyedi a fájl elérési útja mezőben|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-file-metadata-using-id"></a>Fájl metaadatait azonosítójával beszerzése
Fájl metaadatait átveszi fájl azonosítója mezőbe.  
```GET: /datasets/default/files/{id}```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út| Nincs lehetőség|Egyedi azonosítója mezőben a fájlt.|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-file-metadata-using-path"></a>Ismerkedés a metaadatok fájl elérési útja
Fájl metaadatait beolvassa az elérési út mezőbe.  
```GET: /datasets/default/GetFileByPath```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|elérési út|karakterlánc|igen|lekérdezés|Nincs lehetőség |Egyedi a fájl elérési útja mezőben|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="update-file"></a>Fájl frissítése
Frissíti egy fájl mezőbe.  
```PUT: /datasets/default/files/{id}```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út| Nincs lehetőség|Egyedi azonosítója mezőben frissíteni a fájlt.|
|szervezet|String(Binary) |igen|szervezet|Nincs lehetőség |Frissítheti a mezőbe a fájl tartalmát|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="when-a-file-is-modified"></a>Egy fájl módosításának
Egy folyamat elindítja a fájl módosításakor mezőben mappában.  
```GET: /datasets/default/triggers/onupdatedfile```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappaazonosító|karakterlánc|igen|lekérdezés|Nincs lehetőség |Egyedi azonosítója mezőbe, a mappában.|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="object-definitions"></a>Objektum definíciók

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Tulajdonság neve | Adattípus | Szükséges|
|---|---|---|
|táblázatos|Nincs megadva|nem|
|BLOB|Nincs megadva|nem|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|forrás|karakterlánc|nem|
|displayName|karakterlánc|nem|
|urlEncoding|karakterlánc|nem|
|tableDisplayName|karakterlánc|nem|
|tablePluralName|karakterlánc|nem|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|forrás|karakterlánc|nem|
|displayName|karakterlánc|nem|
|urlEncoding|karakterlánc|nem|

#### <a name="blobmetadata"></a>BlobMetadata

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|Azonosító|karakterlánc|nem|
|név|karakterlánc|nem|
|DisplayName|karakterlánc|nem|
|Elérési út|karakterlánc|nem|
|Módosítás dátuma|karakterlánc|nem|
|Mérete|egész szám|nem|
|MediaType|karakterlánc|nem|
|IsFolder|logikai érték|nem|
|ETag|karakterlánc|nem|
|FileLocator|karakterlánc|nem|

## <a name="next-steps"></a>Következő lépések

[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).
