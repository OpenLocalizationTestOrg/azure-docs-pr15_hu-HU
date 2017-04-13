<properties
pageTitle="Megtudhatja, hogy miként szeretné használni az FTP-összekötő összefüggés-alkalmazások |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. Csatlakozás FTP-kiszolgálót, kezelheti a fájlokat. Végezze el a különféle műveletek, például a feltöltés, frissítés, beszerzése és FTP-kiszolgáló fájlok törlése."
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
ms.date="07/22/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-ftp-connector"></a>Első lépések az FTP-összekötő

Az FTP-összekötő segítségével figyelése, kezelése és az FTP-kiszolgálón fájlok létrehozása. 

Szeretne használni [kívánt összekötőre](./apis-list.md), először összefüggés-alkalmazás létrehozása céljából. [Most egy összefüggés-alkalmazás](../app-service-logic/app-service-logic-create-a-logic-app.md)létrehozásával használatának megkezdéséhez.

## <a name="connect-to-ftp"></a>Csatlakozás FTP

Mielőtt a logika alkalmazás elérhető valamelyik szolgáltatás először létre kell hoznia egy *kapcsolatot* a szolgáltatás. [Kapcsolat](./connectors-overview.md) a összefüggés-at, és egy másik szolgáltatás közötti kapcsolatot biztosít.  

### <a name="create-a-connection-to-ftp"></a>FTP-kapcsolat létrehozása

>[AZURE.INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]

## <a name="use-a-ftp-trigger"></a>Az eseményindító FTP használata

Az eseményindító az eseményre kattintva elindíthatja a munkafolyamatot egy logikai alkalmazásban definiált használható. [További tudnivalók a indítók](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.IMPORTANT]Az FTP-összekötő szükséges FTP-kiszolgáló, amely az internetről elérhető, és van konfigurálva, hogy passzív üzemmódban működnek. Az FTP-összekötő is **nem kompatibilis a implicit FTPS (FTP SSL)**. Az FTP-összekötő csak a közvetlen FTPS (FTP SSL) támogatja.  

Ebben a példában fog megjelenítése használatáról az **FTP - fájl hozzáadása vagy módosítása során** eseményindító logika alkalmazás munkafolyamat indítására történő hozzáadásakor egy fájlt, és módosítani, az FTP-kiszolgálón. A vállalati példát Ez az eseményindító segítségével figyelheti az FTP-mappa új fájlokat megjelenítő rendelések felhasználóktól.  Például a **fájl tartalmát** FTP összekötő művelet segítségével majd feldolgozásra és tárolására szolgáló sorrendjének tartalmának kaphat a rendelések adatbázist.

1. *Ftp* a logika alkalmazások Tervező a Keresés mezőbe írja be, majd válassza az **FTP - fájl hozzáadása vagy módosítása során** eseményindító   
![FTP eseményindító kép 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
Megnyitja a **fájl hozzáadása vagy módosítása során** vezérlő  
![FTP eseményindító kép 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
- Jelölje ki a **…** a vezérlő jobb oldalán található. Ekkor megnyílik a mappa Dátumválasztó gomb  
![FTP eseményindító kép 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
- Jelölje ki a **>** (jobbra), és tallózással keresse meg az új vagy módosított fájlok figyelni kívánt mappát. Jelölje ki a mappát, és figyelje meg a mappát, megjelennek a **mappa** vezérlő.  
![FTP eseményindító kép 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   


Ezen a ponton a logika alkalmazás van beállítva, hogy a fájl módosított vagy az adott FTP mappában létrehozott eseményindítók és a munkafolyamat műveletei a Futtatás elindul az eseményindító. 

>[AZURE.NOTE]Egy logikai alkalmazását fog működni legalább egy eseményindító és egy műveletet kell tartalmaznia. Kövesse a művelet hozzáadása a következő szakaszban.  



## <a name="use-a-ftp-action"></a>Egy FTP művelettel

Művelet egy olyan művelet, a munkafolyamat egy logikai alkalmazásban definiált által végzett. [További tudnivalók a műveletek](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

Most, hogy hozzáadott egy eseményindító, az alábbi lépésekkel hozzáadása egy műveletet, amely az eseményindító által talált az új vagy módosított fájl tartalmát kapja.    

1. Válassza az **+ új lépést** hozzáadni az első FTP-kiszolgálón a fájl tartalmát, a művelet  
- Jelölje ki az **művelet hozzáadása** hivatkozásra.  
![FTP művelet kép 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
- Adja meg az *FTP* szeretne keresni, FTP kapcsolatos összes műveletet.
- Jelölje be a végrehajtandó műveletet, ha egy új vagy módosított fájl megtalálható a FTP-mappában **FTP - fájl tartalmának első** .      
![FTP művelet kép 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
A **Get-fájl tartalmának** vezérlő nyílik meg. **Megjegyzés**: az FTP-kiszolgáló fiókja elérésére, ha, még nem tette korábban összefüggés-alkalmazás engedélyezése kéri.  
![FTP művelet kép 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
- Jelölje ki **a vezérlőt** (az üres terület **fájl**alatt található *). Ebben az esetben is használhatja a bármely tulajdonsága különböző fájlból az új vagy módosított FTP-kiszolgálón található.  
- Válassza a **fájl tartalma** lehetőséget.  
![FTP művelet kép 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
-  A vezérlő frissül, jelezve, hogy az **FTP - fájl tartalmának első** művelet jelenik meg a *fájlt a tartalom* az új vagy módosított fájl FTP-kiszolgálón.      
![FTP művelet kép 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
- Mentse a munkáját, majd a fájl hozzáadása a FTP-mappát, a munkafolyamat tesztelése céljából.    

Ezen a ponton a logika alkalmazásban konfigurált egy eseményindító figyelheti az FTP-kiszolgáló egyik mappájára mutat, és a munkafolyamat indítása, ha úgy találja, egy új fájlt vagy a módosított fájl FTP-kiszolgálón. 

A logikai alkalmazás is van beállítva az új vagy módosított fájl tartalmának megszerezni művelet.

Egyéb művelet végrehajtása, például az [SQL Server - sor beszúrása](./connectors-create-api-sqlazure.md#insert-row) műveletet az új vagy módosított fájl tartalmának beillesztése egy SQL-adatbázis táblázat most felvehet.  

## <a name="technical-details"></a>Műszaki információk

Az alábbiakban a eseményindítók, műveletek és válaszokat, amely támogatja a kapcsolat adatait:

## <a name="ftp-triggers"></a>Eseményindítók FTP

A következő trigger(s) FTP foglalja magában:  

|Eseményindító | Leírás|
|--- | ---|
|[Ha egy fájl hozzáadása vagy módosítása](connectors-create-api-ftp.md#when-a-file-is-added-or-modified)|Ezt a műveletet egy folyamat eseményindítók, ha egy fájl hozzáadása vagy módosítása egy mappában.|


## <a name="ftp-actions"></a>FTP-műveletek

FTP rendelkezik az alábbi műveleteket:


|Művelet|Leírás|
|--- | ---|
|[Fájl metaadatait beszerzése](connectors-create-api-ftp.md#get-file-metadata)|Ezt a műveletet a fájl metaadatait kap.|
|[Fájl frissítése](connectors-create-api-ftp.md#update-file)|Ez a művelet frissíti egy fájlt.|
|[Fájl törlése](connectors-create-api-ftp.md#delete-file)|Ez a művelet töröl egy fájlt.|
|[Ismerkedés a metaadatok fájl elérési útja](connectors-create-api-ftp.md#get-file-metadata-using-path)|Ez a művelet kap egy fájlt, az elérési út metaadatait.|
|[Get-fájl tartalmának elérési út segítségével](connectors-create-api-ftp.md#get-file-content-using-path)|Ez a művelet az elérési út segítségével fájl tartalmát kapja.|
|[Fájl tartalom beszerzése](connectors-create-api-ftp.md#get-file-content)|Ezt a műveletet a fájl tartalmát kapja.|
|[Fájl létrehozása](connectors-create-api-ftp.md#create-file)|Ez a művelet létrehoz egy fájlt.|
|[Fájl másolása](connectors-create-api-ftp.md#copy-file)|Ez a művelet FTP-kiszolgálón másolja át egy fájlt.|
|[Mappában lévő fájlok listája](connectors-create-api-ftp.md#list-files-in-folder)|Ezt a műveletet a fájl és mappa almappáinak lista beolvasása|
|[A fájllista gyökérmappa](connectors-create-api-ftp.md#list-files-in-root-folder)|Ez a művelet kapja meg a fájlokat és a legfelső szintű mappa almappáinak listáját.|
|[Mappa kibontása](connectors-create-api-ftp.md#extract-folder)|Ez a művelet kibontja az archív mappák mappába (Példa: .zip).|
### <a name="action-details"></a>Művelet részletei

Az alábbiakban a részletek, a tevékenységek és a összekötő együtt a válaszaikhoz indítók:



### <a name="get-file-metadata"></a>Fájl metaadatait beszerzése
Ezt a műveletet a fájl metaadatait kap. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|azonosító *|Fájl|Jelölje ki a fájlt|

Egy * jelzi, hogy szükség-e a tulajdonság

#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|




### <a name="update-file"></a>Fájl frissítése
Ez a művelet frissíti egy fájlt. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|azonosító *|Fájl|Jelölje ki a fájlt|
|szervezet *|Fájl tartalma|A fájl tartalmát|

Egy * jelzi, hogy szükség-e a tulajdonság

#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|




### <a name="delete-file"></a>Fájl törlése
Ez a művelet töröl egy fájlt. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|azonosító *|Fájl|Jelölje ki a fájlt|

Egy * jelzi, hogy szükség-e a tulajdonság




### <a name="get-file-metadata-using-path"></a>Ismerkedés a metaadatok fájl elérési útja
Ez a művelet kap egy fájlt, az elérési út metaadatait. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|elérési út *|Fájl elérési útja|Jelölje ki a fájlt|

Egy * jelzi, hogy szükség-e a tulajdonság

#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|




### <a name="get-file-content-using-path"></a>Get-fájl tartalmának elérési út segítségével
Ez a művelet az elérési út segítségével fájl tartalmát kapja. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|elérési út *|Fájl elérési útja|Jelölje ki a fájlt|

Egy * jelzi, hogy szükség-e a tulajdonság




### <a name="get-file-content"></a>Fájl tartalom beszerzése
Ezt a műveletet a fájl tartalmát kapja. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|azonosító *|Fájl|Jelölje ki a fájlt|

Egy * jelzi, hogy szükség-e a tulajdonság




### <a name="create-file"></a>Fájl létrehozása
Ez a művelet létrehoz egy fájlt. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|Mappa_útvonala *|Mappa elérési útja|Jelölje ki azt a mappát|
|név *|Fájlnév|A fájl neve|
|szervezet *|Fájl tartalma|A fájl tartalmát|

Egy * jelzi, hogy szükség-e a tulajdonság

#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|




### <a name="copy-file"></a>Fájl másolása
Ez a művelet FTP-kiszolgálón másolja át egy fájlt. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|forrás *|Forrás URL-címe|URL-címét a forrásfájl|
|cél *|Cél fájl elérési útja|Cél fájl elérési útját, beleértve a célfájl neve|
|felülírása|Felülírja?|Felülírja a célfájl, ha "true" értékre van állítva|

Egy * jelzi, hogy szükség-e a tulajdonság

#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|




### <a name="when-a-file-is-added-or-modified"></a>Ha egy fájl hozzáadása vagy módosítása
Ezt a műveletet egy folyamat eseményindítók, ha egy fájl hozzáadása vagy módosítása egy mappában. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|Mappaazonosító *|Mappa|Jelölje ki azt a mappát|

Egy * jelzi, hogy szükség-e a tulajdonság




### <a name="list-files-in-folder"></a>Mappában lévő fájlok listája
Ezt a műveletet a fájl és mappa almappáinak lista beolvasása 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|azonosító *|Mappa|Jelölje ki azt a mappát|

Egy * jelzi, hogy szükség-e a tulajdonság



#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|




### <a name="list-files-in-root-folder"></a>A fájllista gyökérmappa
Ez a művelet kapja meg a fájlokat és a legfelső szintű mappa almappáinak listáját. 


A hívás nincsenek paraméterei

#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|




### <a name="extract-folder"></a>Mappa kibontása
Ez a művelet kibontja az archív mappák mappába (Példa: .zip). 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|forrás *|Forrás archív fájl elérési útja|Az archiválás fájl elérési útja|
|cél *|Cél mappa elérési útja|A cél mappa elérési útja|
|felülírása|Felülírja?|Felülírja a cél fájlokat, ha "true" értékre van állítva|

Egy * jelzi, hogy szükség-e a tulajdonság



#### <a name="output-details"></a>Kimeneti részletei

BlobMetadata


| Tulajdonság neve | Adattípus |
|---|---|---|
|Azonosító|karakterlánc|
|név|karakterlánc|
|DisplayName|karakterlánc|
|Elérési út|karakterlánc|
|Módosítás dátuma|karakterlánc|
|Mérete|egész szám|
|MediaType|karakterlánc|
|IsFolder|logikai érték|
|ETag|karakterlánc|
|FileLocator|karakterlánc|



## <a name="http-responses"></a>HTTP-válaszok

A műveletek és a fenti indítók végre az alábbi HTTP állapot kódokat a térhet vissza: 

|név|Leírás|
|---|---|
|200|oké|
|202|Elfogadott|
|400|Hibás kérés|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|404|Nem található|
|500|Belső kiszolgálóhiba. Ismeretlen hiba történt.|
|alapértelmezett|A művelet sikertelen volt.|







## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)