<properties
pageTitle="A Yammer-összekötő beszúrása az összefüggés-alkalmazások |} Microsoft Azure"
description="A Yammer-összekötő REST API-paraméterekkel áttekintése"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-yammer-connector"></a>Első lépések a Yammer-összekötő

Csatlakozás a vállalati hálózat access beszélgetésekre Yammer.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik.

A Yammer a következőkre van lehetősége:

- Hozza létre az üzleti folyamat, az adatok beolvasása a Yammer alapján. 
- Használata elindítja az, ha van egy új üzenet a csoport vagy a hírcsatorna a következő.
- Üzenet küldése gombra, majd a minden üzenet és az egyéb műveletek segítségével. Az alábbi műveletek kap választ, és végezze el a kimenet rendelkezésre álló további lehetőségeket. Például egy új üzenet jelenik meg, amikor az Office 365 e-mail küldhet.

Logika alkalmazások művelet hozzáadásához lásd: [a összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek
A Yammer tartalmazza a következő eseményindítók és műveleteket. 

Eseményindító | Műveletek
--- | ---
<ul><li>Ha egy csoportban új üzenet</li><li>Ha van, akkor a következő új üzenet hírcsatorna</li></ul>| <ul><li>Az összes üzenet</li><li>Üzenetek kap egy csoportban</li><li>Az üzenetek, a következő hírcsatorna keresése</li><li>Üzenet közzététele</li><li>Ha egy csoportban új üzenet</li><li>Ha van, akkor a következő új üzenet hírcsatorna</li></ul>

Összekötők JSON és az XML formátumú támogatja az adatokat. 

## <a name="create-a-connection-to-yammer"></a>A Yammer-kapcsolat létrehozása
A Yammer-összekötő használatához, először hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a tulajdonságok: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|Jogkivonat|igen|A Yammer hitelesítő adatok megadása|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] A kapcsolat az egyéb összefüggés-alkalmazásokban is használhatja.

## <a name="yammer-rest-api-reference"></a>A Yammer REST API-hivatkozás
Ez a dokumentáció áll verziójához: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Az összes nyilvános üzenetek lekérése a Yammer-hálózaton a bejelentkezett felhasználó
A Yammer webhely felületen a "Minden" beszélgetést felel meg.  
```GET: /messages.json```

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|older_then|egész szám|nem|lekérdezés|nincs lehetőség|Adja eredményül a megadott numerikus karakterláncként Üzenetazonosító régebbi üzeneteket. Az üzenetek paginating esetében hasznos lehet. Ha például, amely éppen látható 20 üzenetek és a legrégebbi szám 2912, amelyek hozzáfűzése "? older_than = 2912″ való kérését a 20 üzenet azok előtt jelenik meg.|
|newer_then|egész szám|nem|lekérdezés|nincs lehetőség|Üzenetek adja vissza az Üzenetazonosító megadott numerikus karakterláncként újabbak. Meg kell használni, amikor az új üzenetek lekérdezési. Üzenetek rögzíthetők, és a legfrissebb üzenetére visszaadott 3516, ha a paraméter egy kérelmet teheti "? newer_than = 3516″ annak érdekében, hogy nem jelenik meg üzenet ismétlődő másolatát már a lapon.|
|határérték|egész szám|nem|lekérdezés|nincs lehetőség|Lépjen vissza az üzenetek csak a megadott számot.|
|lap|egész szám|nem|lekérdezés|nincs lehetőség|A megadott lapon találja. Ha adatok értéke nagyobb, mint a korlátot, a mező használható további lapok eléréséhez|


### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|408|Kérelem időtúllépése|
|429|Túl sok kérések|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|503|A Yammer szolgáltatás nem érhető el|
|504|Átjáró időtúllépése|
|alapértelmezett|A művelet sikertelen volt.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Üzenet küldése egy csoport, vagy az összes vállalati hírcsatorna
Amennyiben Csoportazonosító, üzenet fel az adott csoport egyéb az összes vállalati hírcsatorna fog közzétett.    
```POST: /messages.json``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Adatbevitel| |igen|szervezet|nincs lehetőség|Bejegyzés üzenetkérelmet|


### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|201|Létrehozva|



## <a name="object-definitions"></a>Objektum definíciók

#### <a name="message-yammer-message"></a>Jelenik meg: A Yammer-üzenet

| név | Adattípus | Szükséges |
|---|---| --- | 
|azonosító|egész szám|nem|
|content_excerpt|karakterlánc|nem|
|sender_id|egész szám|nem|
|replied_to_id|egész szám|nem|
|created_at|karakterlánc|nem|
|network_id|egész szám|nem|
|message_type|karakterlánc|nem|
|sender_type|karakterlánc|nem|
|URL-címe|karakterlánc|nem|
|web_url|karakterlánc|nem|
|group_id|egész szám|nem|
|szervezet|Nincs megadva|nem|
|thread_id|egész szám|nem|
|direct_message|logikai érték|nem|
|client_type|karakterlánc|nem|
|client_url|karakterlánc|nem|
|nyelvi|karakterlánc|nem|
|notified_user_ids|tömb|nem|
|Adatvédelem|karakterlánc|nem|
|liked_by|Nincs megadva|nem|
|system_message|logikai érték|nem|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Jelöli bejegyzés kér a Yammer összekötő a közzétételi Yammerre

| név | Adattípus | Szükséges |
|---|---| --- | 
|szervezet|karakterlánc|igen|
|group_id|egész szám|nem|
|replied_to_id|egész szám|nem|
|direct_to_id|egész szám|nem|
|szórás|logikai érték|nem|
|Téma1|karakterlánc|nem|
|téma2|karakterlánc|nem|
|topic3|karakterlánc|nem|
|topic4|karakterlánc|nem|
|topic5|karakterlánc|nem|
|topic6|karakterlánc|nem|
|topic7|karakterlánc|nem|
|topic8|karakterlánc|nem|
|topic9|karakterlánc|nem|
|topic10|karakterlánc|nem|
|topic11|karakterlánc|nem|
|topic12|karakterlánc|nem|
|topic13|karakterlánc|nem|
|topic14|karakterlánc|nem|
|topic15|karakterlánc|nem|
|topic16|karakterlánc|nem|
|topic17|karakterlánc|nem|
|topic18|karakterlánc|nem|
|topic19|karakterlánc|nem|
|topic20|karakterlánc|nem|

#### <a name="messagelist-list-of-messages"></a>MessageList: Üzenetek listája

| név | Adattípus | Szükséges |
|---|---| --- | 
|üzenetek|tömb|nem|


#### <a name="messagebody-message-body"></a>MessageBody tulajdonság: Üzenettörzs

| név | Adattípus | Szükséges |
|---|---| --- | 
|elemzett|karakterlánc|nem|
|egyszerű|karakterlánc|nem|
|gazdag|karakterlánc|nem|

#### <a name="likedby-liked-by"></a>LikedBy: Által tetszett

| név | Adattípus | Szükséges |
|---|---| --- | 
|Darabszám|egész szám|nem|
|nevek|tömb|nem|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Által tetszett

| név | Adattípus | Szükséges |
|---|---| --- | 
|típus|karakterlánc|nem|
|azonosító|egész szám|nem|
|full_name|karakterlánc|nem|


## <a name="next-steps"></a>Következő lépések
[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
