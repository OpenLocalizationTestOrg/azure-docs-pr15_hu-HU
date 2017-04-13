<properties
pageTitle="A OneDrive vállalati verzió |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. Csatlakozás a OneDrive vállalati verzió kezelheti a fájlokat. Különböző műveleteket végezhet feltöltése, például frissítése, beszerzése és a fájlok törlése."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-onedrive-for-business-connector"></a>A OneDrive for Business connector – első lépések

Csatlakozás a OneDrive vállalati verzió kezelheti a fájlokat. Különböző műveleteket végezhet feltöltése, például frissítése, beszerzése és a fájlok törlése.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. 

Most egy összefüggés-alkalmazás létrehozásával miként vehetik használatba, [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakör tartalmaz.

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

A OneDrive for Business connector is használható művelet; trigger(s) rendelkezik. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 A OneDrive for Business-összekötő a következő műveleteket és/vagy a rendelkezésre álló trigger(s) foglalja magában:

### <a name="onedrive-for-business-actions"></a>A OneDrive vállalati verzió műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|[GetFileMetadata](connectors-create-api-onedriveforbusiness.md#getfilemetadata)|Olvassa be a metaadat-alapú OneDrive vállalati verzió azonosítójával fájl|
|[UpdateFile](connectors-create-api-onedriveforbusiness.md#updatefile)|Frissíti egy fájlt a OneDrive vállalati verzióban|
|[DeleteFile](connectors-create-api-onedriveforbusiness.md#deletefile)|Töröl egy fájlt a OneDrive vállalati verzióban|
|[GetFileMetadataByPath](connectors-create-api-onedriveforbusiness.md#getfilemetadatabypath)|Olvassa be a metaadat-alapú OneDrive vállalati verzió elérési út segítségével fájl|
|[GetFileContentByPath](connectors-create-api-onedriveforbusiness.md#getfilecontentbypath)|A OneDrive vállalati verzió elérési út segítségével fájl tartalmát veszi|
|[GetFileContent](connectors-create-api-onedriveforbusiness.md#getfilecontent)|A OneDrive vállalati verzió azonosítójával fájl tartalmát veszi|
|[CreateFile](connectors-create-api-onedriveforbusiness.md#createfile)|Feltölt egy fájlt a onedrive vállalati verzióba|
|[CopyFile](connectors-create-api-onedriveforbusiness.md#copyfile)|Másolja át egy fájlt a OneDrive vállalati verzió|
|[ListFolder](connectors-create-api-onedriveforbusiness.md#listfolder)|Listák fájlok a OneDrive vállalati verzióbeli mappa|
|[ListRootFolder](connectors-create-api-onedriveforbusiness.md#listrootfolder)|Fájlok a OneDrive vállalati verzió legfelső szintű mappa listája|
|[ExtractFolderV2](connectors-create-api-onedriveforbusiness.md#extractfolderv2)|A OneDrive vállalati verzió mappa kibontása|
### <a name="onedrive-for-business-triggers"></a>A OneDrive vállalati verzió eseményindítók
Az alábbi esemény hallgathatja meg:

|Eseményindító | Leírás|
|--- | ---|
|Ha egy fájl jön létre|Egy folyamat eseményindítók, amikor egy új fájlt a OneDrive vállalati verzióbeli mappában jön létre|
|Egy fájl módosításának|Egy folyamat eseményindítók, a OneDrive vállalati verzióbeli fájl módosításakor|


## <a name="create-a-connection-to-onedrive-for-business"></a>A OneDrive vállalati verzió-kapcsolat létrehozása
Logika alkalmazások létrehozása a OneDrive vállalati verzióval, először kell hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a következő tulajdonságokat: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|Jogkivonat|igen|Adja meg a OneDrive vállalati hitelesítő adatok|
Miután létrehozta a kapcsolatot, vele hajtsa végre a műveleteket, és figyelje a jelen cikkben ismertetett eseményindítók. 

>[AZURE.INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]

>[AZURE.TIP] A kapcsolat az egyéb összefüggés-alkalmazásokban is használhatja.

## <a name="reference-for-onedrive-for-business"></a>Összefoglalás: a OneDrive vállalati verzióban
Verziójára vonatkozik: 1.0

## <a name="getfilemetadata"></a>GetFileMetadata
Első fájl metaadatait azonosítójával: olvassa be a metaadat-alapú OneDrive vállalati verzió azonosítójával fájl 

```GET: /datasets/default/files/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Adja meg a fájlt|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="updatefile"></a>UpdateFile
Frissítési fájl: frissíti egy fájlt a OneDrive vállalati verzióban 

```PUT: /datasets/default/files/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Adja meg a fájlt frissítése|
|szervezet| |igen|szervezet|nincs lehetőség|A OneDrive vállalati verzió frissítése a fájl tartalmát|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="deletefile"></a>DeleteFile
Fájl törlése: törli a fájl a OneDrive vállalati verzióban 

```DELETE: /datasets/default/files/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Adja meg a törölni kívánt fájlt|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="getfilemetadatabypath"></a>GetFileMetadataByPath
Metaadatok fájl elérési útja kérjen: olvassa be a metaadat-alapú OneDrive vállalati verzió elérési út segítségével fájl 

```GET: /datasets/default/GetFileByPath``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|elérési út|karakterlánc|igen|lekérdezés|nincs lehetőség|Egyedi a fájl elérési útját a OneDrive vállalati verzió|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="getfilecontentbypath"></a>GetFileContentByPath
Get-fájl tartalmának elérési út segítségével: a OneDrive vállalati verzió elérési út segítségével fájl tartalmát veszi 

```GET: /datasets/default/GetFileContentByPath``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|elérési út|karakterlánc|igen|lekérdezés|nincs lehetőség|Egyedi a fájl elérési útját a OneDrive vállalati verzió|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="getfilecontent"></a>GetFileContent
Get-fájl tartalmának azonosítójával: a OneDrive vállalati verzió azonosítójával fájl tartalmát veszi 

```GET: /datasets/default/files/{id}/content``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Adja meg a fájlt|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="createfile"></a>CreateFile
Fájl létrehozása: feltölt egy fájlt a onedrive vállalati verzióba 

```POST: /datasets/default/files``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappa_útvonala|karakterlánc|igen|lekérdezés|nincs lehetőség|Mappa elérési útja feltölteni a fájlt a onedrive vállalati verzióba|
|név|karakterlánc|igen|lekérdezés|nincs lehetőség|A fájl létrehozása a OneDrive vállalati verzió neve|
|szervezet| |igen|szervezet|nincs lehetőség|A fájl feltöltése a OneDrive vállalati verzió tartalma|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="copyfile"></a>CopyFile
Fájl másolása: másolja át egy fájlt a OneDrive vállalati verzió 

```POST: /datasets/default/copyFile``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|forrás|karakterlánc|igen|lekérdezés|nincs lehetőség|URL-címét a forrásfájl|
|cél|karakterlánc|igen|lekérdezés|nincs lehetőség|A OneDrive vállalati verzióban, beleértve a célfájl neve cél fájl elérési útja|
|felülírása|logikai érték|nem|lekérdezés|hamis|Felülírja a célfájl, ha "true" értékre van állítva|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="onnewfile"></a>OnNewFile
Fájl létrehozása esetén: a munkafolyamat eseményindítók, amikor egy új fájlt a OneDrive vállalati verzióbeli mappában jön létre 

```GET: /datasets/default/triggers/onnewfile``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappaazonosító|karakterlánc|igen|lekérdezés|nincs lehetőség|Adja meg azt a mappát|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="onupdatedfile"></a>OnUpdatedFile
Egy fájl módosításának: elindítja a folyamat egy fájlt a OneDrive vállalati verzióbeli módosításakor 

```GET: /datasets/default/triggers/onupdatedfile``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappaazonosító|karakterlánc|igen|lekérdezés|nincs lehetőség|Adja meg azt a mappát|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="listfolder"></a>ListFolder
A lista mappában lévő fájlok: listák fájlok a OneDrive vállalati verzióbeli mappa 

```GET: /datasets/default/folders/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Adja meg azt a mappát|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="listrootfolder"></a>ListRootFolder
Lista legfelső szintű mappa: fájlok a OneDrive vállalati verzió legfelső szintű mappa listája 

```GET: /datasets/default/folders``` 

A hívás nincsenek paraméterei
#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="extractfolderv2"></a>ExtractFolderV2
Bontsa ki a mappát: a OneDrive vállalati verzió mappa kibontása 

```POST: /datasets/default/extractFolderV2``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|forrás|karakterlánc|igen|lekérdezés|nincs lehetőség|Az archiválás fájl elérési útja|
|cél|karakterlánc|igen|lekérdezés|nincs lehetőség|A OneDrive vállalati verzió archív tartalmát kiolvasó elérési út|
|felülírása|logikai érték|nem|lekérdezés|hamis|Felülírja a cél fájlokat, ha "true" értékre van állítva|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="object-definitions"></a>Objektum definíciók 

### <a name="datasetsmetadata"></a>DataSetsMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|táblázatos|Nincs megadva|nem |
|BLOB|Nincs megadva|nem |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|forrás|karakterlánc|nem |
|displayName|karakterlánc|nem |
|urlEncoding|karakterlánc|nem |
|tableDisplayName|karakterlánc|nem |
|tablePluralName|karakterlánc|nem |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|forrás|karakterlánc|nem |
|displayName|karakterlánc|nem |
|urlEncoding|karakterlánc|nem |



### <a name="blobmetadata"></a>BlobMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Azonosító|karakterlánc|nem |
|név|karakterlánc|nem |
|DisplayName|karakterlánc|nem |
|Elérési út|karakterlánc|nem |
|Módosítás dátuma|karakterlánc|nem |
|Mérete|egész szám|nem |
|MediaType|karakterlánc|nem |
|IsFolder|logikai érték|nem |
|ETag|karakterlánc|nem |
|FileLocator|karakterlánc|nem |



### <a name="object"></a>Objektum


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)