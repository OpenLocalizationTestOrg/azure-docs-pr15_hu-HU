<properties
pageTitle=" Az tartalékidő összekötő használata az összefüggés-alkalmazások |} Microsoft Azure"
description="A tartalékidő összekötő használata a Microsoft Azure alkalmazás szolgáltatás összefüggés-alkalmazásokat az első lépések"
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

# <a name="get-started-with-the-slack-connector"></a>Első lépések a tartalékidő összekötő

Tartalékidő pedig a csapat kommunikációs eszközben, hogy összegyűjti az összes csoport kommunikáció egyik helyezni, azonnali kereshető és a rendelkezésre álló viheti.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik.

A tartalékidő összekötő van lehetősége:

* Logika alkalmazások készítéséhez használatával

Logika alkalmazások művelet hozzáadásához lásd: [a összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Vegyük beszélni eseményindítók és műveletek

A tartalékidő összekötő is használható művelet; vannak olyan nincs indítók. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 A tartalékidő összekötő a következő műveleteket és/vagy a rendelkezésre álló trigger(s) foglalja magában:

### <a name="slack-actions"></a>Tartalékidő műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|PostMessage|Tartalmakat tehet közzé egy adott csatorna üzenetet.|
## <a name="create-a-connection-to-slack"></a>Tartalékidő kapcsolat létrehozása
A tartalékidő összekötő használatához, először hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a tulajdonságok: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|Jogkivonat|igen|Tartalékidő hitelesítő adatok megadása|

Kövesse ezeket a lépéseket követve jelentkezzen be az tartalékidő és a teljes tartalékidő **kapcsolatot** a logika alkalmazásban konfigurálása:

1. Jelölje ki az **Ismétlődés**
2. Válassza ki a **gyakoriság** , és írjon be egy **intervallum**
3. Jelölje be a **művelet hozzáadása**  
![Tartalékidő konfigurálása][1]  
4. A Keresés mezőbe írja be a tartalékidő, és várja meg a találatok tartalékidő az az összes Naplóbejegyzéshez a nevében
5. Válassza a **tartalékidő - üzenet közzététele**
6. Jelölje be, **Jelentkezzen be a tartalékidőt**:  
![Tartalékidő konfigurálása][2]
7. Adja meg a tartalékidő hitelesítő adatait, és jelentkezzen be az alkalmazás engedélyezése    
![Tartalékidő konfigurálása][3]  
8. A szervezet napló lapon átirányítjuk. **Engedélyezése** A logika alkalmazás vezérléséhez tartalékidő:      
![Tartalékidő konfigurálása][5] 
9. Az engedély befejeződése után a logika alkalmazásba a **tartalékidőt – az összes üzenetre lekérdezése** szakasz konfigurálásával befejezéséhez átirányítjuk. Adja hozzá a többi eseményindítók és a szükséges műveleteket.  
![Tartalékidő konfigurálása][6]
10. A fenti menüsávján **mentése** gombra kattintva mentse a munkáját.


>[AZURE.TIP] A kapcsolat az egyéb összefüggés-alkalmazásokban is használhatja.

## <a name="slack-rest-api-reference"></a>Tartalékidő REST API-hivatkozás
#### <a name="this-documentation-is-for-version-10"></a>Ez a dokumentáció áll verziójához: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Tartalmakat tehet közzé egy adott csatorna üzenetet.
**```POST: /chat.postMessage```** 



| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|csatorna|karakterlánc|igen|lekérdezés|nincs lehetőség|Csatorna, személyes csoport vagy Csevegés csatorna üzenetet küldeni szeretné. A név lehet (ex: #general) vagy kódolt azonosítóval.|
|szöveg|karakterlánc|igen|lekérdezés|nincs lehetőség|Küldése az üzenet szövegét. A formázási beállításokat, olvassa el a https://api.slack.com/docs/formatting című témakört.|
|felhasználónév|karakterlánc|nem|lekérdezés|nincs lehetőség|A bot neve|
|as_user|logikai érték|nem|lekérdezés|nincs lehetőség|A sikeres true helyett, mint egy bot jegyezni az üzenetet, a hitelesített felhasználó|
|elemzése|karakterlánc|nem|lekérdezés|nincs lehetőség|Módosítsa, hogyan kell kezelni az üzeneteket. A részletekért olvassa el a https://api.slack.com/docs/formatting című témakört.|
|link_names|egész szám|nem|lekérdezés|nincs lehetőség|Keresés és kapcsolat csatorna nevét és felhasználónevét.|
|unfurl_links|logikai érték|nem|lekérdezés|nincs lehetőség|A sikeres engedélyezése a tartalom elsősorban szöveges unfurling igaz.|
|unfurl_media|logikai érték|nem|lekérdezés|nincs lehetőség|A sikeres letiltása a médiatartalom unfurling hamis.|
|icon_url|karakterlánc|nem|lekérdezés|nincs lehetőség|Kép URL-cím ikonként használható ez az üzenet|
|icon_emoji|karakterlánc|nem|lekérdezés|nincs lehetőség|Ez az üzenet ikonként használandó Emoji|


### <a name="here-are-the-possible-responses"></a>Az alábbiakban a lehetséges válaszokat:

|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|408|Kérelem időtúllépése|
|429|Túl sok kérések|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|503|Tartalékidő szolgáltatás nem érhető el|
|504|Átjáró időtúllépése|
|alapértelmezett|A művelet sikertelen volt.|
------



## <a name="object-definitions"></a>Objektum meghatározás(ok): 

 **Üzenet**: Yammer-üzenet

Üzenet szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|azonosító|egész szám|
|content_excerpt|karakterlánc|
|sender_id|egész szám|
|replied_to_id|egész szám|
|created_at|karakterlánc|
|network_id|egész szám|
|message_type|karakterlánc|
|sender_type|karakterlánc|
|URL-címe|karakterlánc|
|web_url|karakterlánc|
|group_id|egész szám|
|szervezet|Nincs megadva|
|thread_id|egész szám|
|direct_message|logikai érték|
|client_type|karakterlánc|
|client_url|karakterlánc|
|nyelvi|karakterlánc|
|notified_user_ids|tömb|
|Adatvédelem|karakterlánc|
|liked_by|Nincs megadva|
|system_message|logikai érték|



 **PostOperationRequest**: jelöli bejegyzés kér a Yammer összekötő a közzétételi Yammerre

PostOperationRequest szükséges tulajdonságokat:

szervezet

**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|szervezet|karakterlánc|
|group_id|egész szám|
|replied_to_id|egész szám|
|direct_to_id|egész szám|
|szórás|logikai érték|
|Téma1|karakterlánc|
|téma2|karakterlánc|
|topic3|karakterlánc|
|topic4|karakterlánc|
|topic5|karakterlánc|
|topic6|karakterlánc|
|topic7|karakterlánc|
|topic8|karakterlánc|
|topic9|karakterlánc|
|topic10|karakterlánc|
|topic11|karakterlánc|
|topic12|karakterlánc|
|topic13|karakterlánc|
|topic14|karakterlánc|
|topic15|karakterlánc|
|topic16|karakterlánc|
|topic17|karakterlánc|
|topic18|karakterlánc|
|topic19|karakterlánc|
|topic20|karakterlánc|



 **MessageList**: az üzenetek találhatók

MessageList szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|üzenetek|tömb|



 **A MessageBody tulajdonság**: üzenettörzs

A MessageBody tulajdonság szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|elemzett|karakterlánc|
|egyszerű|karakterlánc|
|gazdag|karakterlánc|



 **LikedBy**: tetszett szerint

LikedBy szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|Darabszám|egész szám|
|nevek|tömb|



 **YammmerEntity**: tetszett szerint

YammmerEntity szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|típus|karakterlánc|
|azonosító|egész szám|
|full_name|karakterlánc|


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Objektum meghatározás(ok): 

 **WebResultModel**: a Bing webes keresési eredmények

WebResultModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|Cím|karakterlánc|
|Leírás|karakterlánc|
|DisplayUrl|karakterlánc|
|Azonosító|karakterlánc|
|FullUrl|karakterlánc|



 **VideoResultModel**: a Bing videó keresési találatok

VideoResultModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|Cím|karakterlánc|
|DisplayUrl|karakterlánc|
|Azonosító|karakterlánc|
|MediaUrl|karakterlánc|
|Futtatókörnyezet|egész szám|
|Miniatűrje|Nincs megadva|



 **ThumbnailModel**: a multimédiás elem tulajdonságainak miniatűrje

ThumbnailModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|MediaUrl|karakterlánc|
|ContentType|karakterlánc|
|Szélesség|egész szám|
|Magasság|egész szám|
|FileSize|egész szám|



 **ImageResultModel**: a Bing kép keresési találatok

ImageResultModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|Cím|karakterlánc|
|DisplayUrl|karakterlánc|
|Azonosító|karakterlánc|
|MediaUrl|karakterlánc|
|SourceUrl|karakterlánc|
|Miniatűrje|Nincs megadva|



 **NewsResultModel**: a Bing hírek keresési találatok

NewsResultModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|Cím|karakterlánc|
|Leírás|karakterlánc|
|DisplayUrl|karakterlánc|
|Azonosító|karakterlánc|
|Forrás|karakterlánc|
|Dátum|karakterlánc|



 **SpellResultModel**: a Bing helyesírási javaslatok eredménye

SpellResultModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|Azonosító|karakterlánc|
|Érték|karakterlánc|



 **RelatedSearchResultModel**: a Bing kapcsolódó keresési találatok

RelatedSearchResultModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|Cím|karakterlánc|
|Azonosító|karakterlánc|
|BingUrl|karakterlánc|



 **CompositeSearchResultModel**: a Bing összetett keresési találatok

CompositeSearchResultModel szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|WebResultsTotal|egész szám|
|ImageResultsTotal|egész szám|
|VideoResultsTotal|egész szám|
|NewsResultsTotal|egész szám|
|SpellSuggestionsTotal|egész szám|
|WebResults|tömb|
|ImageResults|tömb|
|VideoResults|tömb|
|NewsResults|tömb|
|SpellSuggestionResults|tömb|
|RelatedSearchResults|tömb|


## <a name="object-definitions"></a>Objektum meghatározás(ok): 

 **PostOperationResponse**: válasz tartalékidő összekötő bejegyzés működésének jelöli tartalékidő történő közzététel

PostOperationResponse szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|oké|logikai érték|
|csatorna|karakterlánc|
|TS|karakterlánc|
|üzenet|Nincs megadva|
|hibaüzenet|karakterlánc|



 **MessageItem**: csatorna üzenet.

MessageItem szükséges tulajdonságokat:


A tulajdonságok egyike sem szükség. 


**Az összes tulajdonságok**: 


| név | Adattípus |
|---|---|
|szöveg|karakterlánc|
|azonosító|karakterlánc|
|felhasználói|karakterlánc|
|létrehozva|egész szám|
|a törölt is_user|logikai érték|


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
