<properties
    pageTitle="A Facebook-összekötő beszúrása az összefüggés-alkalmazások |} Microsoft Azure"
    description="A Facebook-összekötő a REST API-paraméterekkel áttekintése"
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

# <a name="get-started-with-the-facebook-connector"></a>Első lépések a Facebook-összekötő
Kapcsolódás a Facebookhoz és ütemterv bejegyzéseket tehet közzé, majd a hírcsatorna lapon, és így tovább. 

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik.


A Facebook a következőkre van lehetősége:

- Hozza létre az üzleti folyamat az adatok beolvasása a Facebook alapján. 
- Az eseményindító használatával új bejegyzés érkezik.
- Az ütemterv, a Yammerben műveletek használata beszerzése a hírcsatorna lapon, és így tovább. Az alábbi műveletek kap választ, és végezze el a kimenet rendelkezésre álló további lehetőségeket. Például ha az ütemterven új bejegyzés, is készíthet, hogy a bejegyzés és a Twitteren hírcsatornára leküldéses azt. 



Logika alkalmazások művelet hozzáadásához lásd: [a összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek
A Facebook-összekötő tartalmazza a következő eseményindító és műveleteket. 

| Eseményindítók | Műveletek|
| --- | --- |
| <ul><li>Ha az ütemterven új bejegyzés</li></ul> |<ul><li>Az idősor első hírcsatorna</li><li>Tartalmakat tehet közzé saját ütemterv</li><li>Ha az ütemterven új bejegyzés</li><li>Ismerkedés a hírcsatorna lapon</li><li>Felhasználói idősor első</li><li>Tartalmakat tehet közzé lap</li></ul>

Összekötők JSON és az XML formátumú támogatja az adatokat.

## <a name="create-a-connection-to-facebook"></a>Facebook-kapcsolat létrehozása
Ez az összekötő logika alkalmazás beállításakor engedélyeznie kell a csatlakozás a facebookhoz logika alkalmazások.

1. Jelentkezzen be a Facebook-fiókját
2. Jelölje be az **Engedélyezés**, illetve engedélyezheti a csatlakozhat, és használja a Facebook-logika alkalmazások. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Egyéb összefüggés-alkalmazások is használhatja a ugyanazt a Facebook-kapcsolatot.

## <a name="swagger-rest-api-reference"></a>Swagger REST API-hivatkozás
Verziójára vonatkozik: 1.0.

### <a name="get-feed-from-my-timeline"></a>Az idősor első hírcsatorna
Adatcsatornák megszerzi a bejelentkezett felhasználó ütemterv.  
```GET: /me/feed```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|mezők|karakterlánc|nem|lekérdezés|nincs lehetőség |Adja meg, a visszaadott kívánt mezőket. Példa (azonosítóját, név, kép).|
|határérték|egész szám|nem|lekérdezés| nincs lehetőség|Visszaszerezni bejegyzések maximális száma|
|a|karakterlánc|nem|lekérdezés| nincs lehetőség|Csak azokat az csatolt hellyel korlátozza a bejegyzések listáját.|
|szűrő|karakterlánc|nem|lekérdezés| nincs lehetőség|Csak egy adott adatfolyam szűrőnek megfelelő bejegyzések beolvasásához.|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="post-to-my-timeline"></a>Tartalmakat tehet közzé saját ütemterv
Tartalmakat tehet közzé a bejelentkezett felhasználó ütemtervbe állapotüzenet.  
```POST: /me/feed```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|bejegyzés|karakterlánc |igen|szervezet|nincs lehetőség |Új üzenetet a közzéteendő|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Ha az ütemterven új bejegyzés
Eseményindítók egy új folyamat, új bejegyzés a bejelentkezett felhasználó idősor esetén.  
```GET: /trigger/me/feed```

Nincsenek paraméterek. 

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-page-feed"></a>Ismerkedés a hírcsatorna lapon
Bejegyzések megnyithatja a hírcsatorna megadott lap.  
```GET: /{pageId}/feed```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|pageId|karakterlánc|igen|elérési út| nincs lehetőség|A lapot, amelyből bejegyzések lesz visszakeresve, hogy azonosítója.|
|határérték|egész szám|nem|lekérdezés| nincs lehetőség|Visszaszerezni bejegyzések maximális száma|
|include_hidden|logikai érték|nem|lekérdezés|nincs lehetőség |Hogy bármilyen bejegyzések a lap voltak rejtett tartalmazza-e|
|mezők|karakterlánc|nem|lekérdezés|nincs lehetőség |Adja meg, a visszaadott kívánt mezőket. Példa (azonosítóját, név, kép).|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="get-user-timeline"></a>Felhasználói idősor első
Bejegyzések megnyithatja a felhasználó idősor.  
```GET: /{userId}/feed```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Felhasználóazonosító|karakterlánc|igen|elérési út|nincs lehetőség |A felhasználót, akinek ütemterv kell visszaszerezni azonosítója.|
|határérték|egész szám|nem|lekérdezés|nincs lehetőség |Visszaszerezni bejegyzések maximális száma|
|a|karakterlánc|nem|lekérdezés|nincs lehetőség |Csak azokat az csatolt hellyel korlátozza a bejegyzések listáját.|
|szűrő|karakterlánc|nem|lekérdezés| nincs lehetőség|Csak egy adott adatfolyam szűrőnek megfelelő bejegyzések beolvasásához.|
|mezők|karakterlánc|nem|lekérdezés| nincs lehetőség|Adja meg, a visszaadott kívánt mezőket. Példa (azonosítóját, név, kép).|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="post-to-page"></a>Tartalmakat tehet közzé lap
Üzenet küldése egy Facebook-lapra, a bejelentkezett felhasználó.  
```POST: /{pageId}/feed```

| név|Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|pageId|karakterlánc|igen|elérési út|nincs lehetőség |A lapon a közzétételi azonosítója.|
|bejegyzés|sok |igen|szervezet|nincs lehetőség |Új üzenetet a közzéteendő.|

#### <a name="response"></a>Válasz
|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="object-definitions"></a>Objektum definíciók

#### <a name="getfeedresponse"></a>GetFeedResponse

|Tulajdonság neve | Adattípus | Szükséges|
|---|---|---|
|adatok|tömb|nem|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|adatok|tömb|nem|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: A profil egy bejegyzés adatait a hírcsatorna
A profil lehet egy felhasználó, oldal, alkalmazás vagy csoport. 

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|nem|
|admin_creator|tömb|nem|
|a felirat|karakterlánc|nem|
|created_time|karakterlánc|nem|
|Leírás|karakterlánc|nem|
|feed_targeting|Nincs megadva|nem|
|a|Nincs megadva|nem|
|ikon|karakterlánc|nem|
|is_hidden|logikai érték|nem|
|is_published|logikai érték|nem|
|hivatkozás|karakterlánc|nem|
|üzenet|karakterlánc|nem|
|név|karakterlánc|nem|
|object_id|karakterlánc|nem|
|kép|karakterlánc|nem|
|hely|Nincs megadva|nem|
|Adatvédelem|Nincs megadva|nem|
|Tulajdonságok|tömb|nem|
|forrás|karakterlánc|nem|
|status_type|karakterlánc|nem|
|a szövegegység|karakterlánc|nem|
|célba juttatása|Nincs megadva|nem|
|a|tömb|nem|
|típus|karakterlánc|nem|
|updated_time|karakterlánc|nem|
|with_tags|Nincs megadva|nem|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: A profil egy bejegyzés adatait a hírcsatorna
A profil lehet egy felhasználó, oldal, alkalmazás vagy csoport.

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|nem|
|created_time|karakterlánc|nem|
|a|Nincs megadva|nem|
|üzenet|karakterlánc|nem|
|típus|karakterlánc|nem|

#### <a name="adminitem"></a>AdminItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|nem|
|hivatkozás|karakterlánc|nem|

#### <a name="propertyitem"></a>PropertyItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|név|karakterlánc|nem|
|szöveg|karakterlánc|nem|
|href|karakterlánc|nem|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|üzenet|karakterlánc|igen|
|hivatkozás|karakterlánc|nem|
|kép|karakterlánc|nem|
|név|karakterlánc|nem|
|a felirat|karakterlánc|nem|
|Leírás|karakterlánc|nem|
|hely|karakterlánc|nem|
|címkék|karakterlánc|nem|
|Adatvédelem|Nincs megadva|nem|
|object_attachment|karakterlánc|nem|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|üzenet|karakterlánc|igen|
|hivatkozás|karakterlánc|nem|
|kép|karakterlánc|nem|
|név|karakterlánc|nem|
|a felirat|karakterlánc|nem|
|Leírás|karakterlánc|nem|
|műveletek|tömb|nem|
|hely|karakterlánc|nem|
|címkék|karakterlánc|nem|
|object_attachment|karakterlánc|nem|
|célba juttatása|Nincs megadva|nem|
|feed_targeting|Nincs megadva|nem|
|a közzétett|logikai érték|nem|
|scheduled_publish_time|karakterlánc|nem|
|backdated_time|karakterlánc|nem|
|backdated_time_granularity|karakterlánc|nem|
|child_attachments|tömb|nem|
|multi_share_end_card|logikai érték|nem|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|nem|

#### <a name="profilecollection"></a>ProfileCollection

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|adatok|tömb|nem|

#### <a name="useritem"></a>UserItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|nem|
|Utónév|karakterlánc|nem|
|Vezetéknév|karakterlánc|nem|
|név|karakterlánc|nem|
|Gender|karakterlánc|nem|
|tudnivalók a|karakterlánc|nem|

#### <a name="actionitem"></a>ActionItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|név|karakterlánc|nem|
|hivatkozás|karakterlánc|nem|

#### <a name="targetitem"></a>TargetItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|ország|tömb|nem|
|területi beállítások|tömb|nem|
|régiók|tömb|nem|
|Város|tömb|nem|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Hírek szabályozó objektum hírcsatorna-bejegyzéssel célba juttatása
Bárki ezekhez a csoportokhoz szívesen látja a bejegyzését, mások kisebb valószínűleg. Lapok csak vonatkozik.

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|ország|tömb|nem|
|régiók|tömb|nem|
|Város|tömb|nem|
|age_min|egész szám|nem|
|age_max|egész szám|nem|
|genders|tömb|nem|
|relationship_statuses|tömb|nem|
|interested_in|tömb|nem|
|college_years|tömb|nem|
|érdeklődési|tömb|nem|
|relevant_until|egész szám|nem|
|education_statuses|tömb|nem|
|területi beállítások|tömb|nem|

#### <a name="placeitem"></a>PlaceItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|nem|
|név|karakterlánc|nem|
|overall_rating|szám|nem|
|hely|Nincs megadva|nem|

#### <a name="locationitem"></a>LocationItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|a város|karakterlánc|nem|
|ország|karakterlánc|nem|
|szélesség|szám|nem|
|located_in|karakterlánc|nem|
|hosszúság|szám|nem|
|név|karakterlánc|nem|
|régió|karakterlánc|nem|
|állam|karakterlánc|nem|
|utca.|karakterlánc|nem|
|Irányítószám|karakterlánc|nem|

#### <a name="privacyitem"></a>PrivacyItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|Leírás|karakterlánc|nem|
|érték|karakterlánc|igen|
|engedélyezése|karakterlánc|nem|
|elutasítása|karakterlánc|nem|
|barátok|karakterlánc|nem|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|hivatkozás|karakterlánc|nem|
|kép|karakterlánc|nem|
|image_hash|karakterlánc|nem|
|név|karakterlánc|nem|
|Leírás|karakterlánc|nem|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|URL-címe|karakterlánc|igen|
|a felirat|karakterlánc|nem|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|igen|
|post_id|karakterlánc|igen|

#### <a name="postvideorequest"></a>PostVideoRequest

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|videoData|karakterlánc|igen|
|Leírás|karakterlánc|igen|
|cím|karakterlánc|igen|
|uploadedVideoName|karakterlánc|nem|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|adatok|Nincs megadva|igen|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|URL-címe|karakterlánc|igen|
|is_silhouette|logikai érték|igen|
|magasság|karakterlánc|nem|
|szélesség|karakterlánc|nem|

#### <a name="geteventresponse"></a>GetEventResponse

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|adatok|tömb|igen|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Tulajdonság neve | Adattípus |Szükséges|
|---|---|---|
|azonosító|karakterlánc|igen|
|név|karakterlánc|igen|
|start_time|karakterlánc|nem|
|end_time|karakterlánc|nem|
|Időzóna|karakterlánc|nem|
|hely|karakterlánc|nem|
|Leírás|karakterlánc|nem|
|ticket_uri|karakterlánc|nem|
|rsvp_status|karakterlánc|igen|


## <a name="next-steps"></a>Következő lépések

[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).
