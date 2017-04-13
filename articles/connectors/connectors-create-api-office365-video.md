<properties
pageTitle="Az összefüggés-alkalmazások használata az Office 365 videó összekötő |} Microsoft Azure"
description="Első lépések az Office 365 videó összekötő használata a Microsoft Azure szolgáltatás logika alkalmazásindítónalkalmazások"
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

# <a name="get-started-with-the-office365-video-connector"></a>Első lépések az Office 365 videó összekötő
Az Office 365 videó kapcsolatos tájékozódáshoz az Office 365 videó, videók és egyebek listájának szeretne csatlakozni. Az Office 365 videó összekötő is használhatók:

- Logika alkalmazások 

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. Ez az összekötő bármely séma korábbi verzióiban nem támogatott.

Office 365 videó alkalmazással a következőket teheti:

- Hozza létre az üzleti folyamat kap, az Office 365 videó adatok alapján. 
- Használja a műveleteket, amelyeket a videó portál állapotának ellenőrzése gombra, majd a minden videó csatorna és az egyéb listáját. Az alábbi műveletek kap választ, és végezze el a kimenet rendelkezésre álló további lehetőségeket. Ha például a Bing keresési összekötő használata az Office 365 videó kereséséhez, és az Office 365 videó összekötő segítségével videó adatainak. Ha a videó megfelel az igényeinek, akkor ez a videó a Facebookon elküldheti. 

Logika alkalmazások művelet hozzáadásához lásd: [a összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

Az Office 365 videó összekötő érhető el az alábbi műveletek tartalmaz. Vannak olyan nincs indítók.

| Eseményindítók | Műveletek|
| --- | --- |
| Nincs lehetőség | <ul><li>Videó portál állapotának ellenőrzése</li><li>Az összes megtekinthető csatornák beszerzése</li><li>Videoklip lejátszási URL-címét az Azure Media Services jegyzék beszerzése</li><li>Ismerkedés az bearer jogkivonathoz érheti el a videó visszafejtése</li><li>Kap információt egy adott Office 365 videó</li><li>Az Office 365-ben videók kiemelése egy csatornában bemutató listája</li></ul>

Összekötők JSON és az XML formátumú támogatja az adatokat. 

## <a name="create-a-connection-to-office365-video-connector"></a>Office 365 videó connector-kapcsolat létrehozása
Ez az összekötő logika alkalmazás beállításakor kell bejelentkezés az Office 365 videó-fiókjába, és a fiókhoz való csatlakozáshoz összefüggés-alkalmazás letiltása.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Miután létrehozta a kapcsolatot, írja be az Office 365 videó-tulajdonságok hasonló bérlő nevét, vagy csatorna azonosítójával. Ez a témakör a **REST API-hivatkozás** e-tulajdonságokat ismerteti.

>[AZURE.TIP] A többi logika alkalmazás a ugyanazt az Office 365 videó kapcsolat is használhatja.

## <a name="swagger-rest-api-reference"></a>Swagger REST API-hivatkozás
Verziójára vonatkozik: 1.0.

### <a name="checks-video-portal-status"></a>Videó portál állapotának ellenőrzése 
Annak vizsgálata, hogy a videó portál állapotára, és láthatja, ha engedélyezve vannak-e a videó szolgáltatások.  
```GET: /{tenant}/IsEnabled``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Bérlői webhelyen|karakterlánc|igen|elérési út|nincs lehetőség|A könyvtár a felhasználónak a bérlői nevét része|


#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|404|Nem található|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|



### <a name="get-all-viewable-channels"></a>Az összes megtekinthető csatornák beszerzése 
A felhasználó nem rendelkezik megtekintése a hozzáférést az összes csatornák kap.  
```GET: /{tenant}/Channels``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Bérlői webhelyen|karakterlánc|igen|elérési út|nincs lehetőség|A könyvtár a felhasználónak a bérlői nevét része|


#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|404|Nem található|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Az Office 365-ben videók kiemelése egy csatornában bemutató listája 
Az Office 365-ben videók kiemelése egy csatornában bemutató listája.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Bérlői webhelyen|karakterlánc|igen|elérési út|nincs lehetőség|A könyvtár a felhasználónak a bérlői nevét része|
|channelId|karakterlánc|igen|elérési út|nincs lehetőség|A csatorna azonosítója, amelyből videók kell beolvasandó|


#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|404|Nem található|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|




### <a name="gets-information-about-a-particular-office365-video"></a>Kap információt egy adott Office 365 videó 
Videó kap információt egy adott Office 365-ben.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Bérlői webhelyen|karakterlánc|igen|elérési út|nincs lehetőség|A könyvtár a felhasználónak a bérlői nevét része|
|channelId|karakterlánc|igen|elérési út|nincs lehetőség|A csatorna azonosítója|
|videoId|karakterlánc|igen|elérési út|nincs lehetőség|A videó azonosító|


#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|404|Nem található|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Videoklip lejátszási URL-címét az Azure Media Services jegyzék beszerzése 
Videoklip lejátszási URL-címét az Azure Media Services jegyzék beszerzése.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Bérlői webhelyen|karakterlánc|igen|elérési út|nincs lehetőség|A könyvtár a felhasználónak a bérlői nevét része|
|channelId|karakterlánc|igen|elérési út|nincs lehetőség|A csatorna azonosítója|
|videoId|karakterlánc|igen|elérési út|nincs lehetőség|A videó azonosító|
|streamingFormatType|karakterlánc|igen|lekérdezés|nincs lehetőség|Adatfolyam formátum típusa. 1 – a folyamatos átvitelű Sima kapcsolódású vagy MPEG-vonal. 0 - adatfolyam HLS|


#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|404|Nem található|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Ismerkedés az bearer jogkivonathoz érheti el a videó visszafejtése 
Ismerkedés az bearer jogkivonathoz érheti el a videó visszafejtése.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Bérlői webhelyen|karakterlánc|igen|elérési út|nincs lehetőség|A könyvtár a felhasználónak a bérlői nevét része|
|channelId|karakterlánc|igen|elérési út|nincs lehetőség|A csatorna azonosítója|
|videoId|karakterlánc|igen|elérési út|nincs lehetőség|A videó azonosító|


#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|404|Nem található|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="object-definitions"></a>Objektum definíciók

#### <a name="channel-channel-class"></a>Csatorna: Csatorna osztály

| név | Adattípus | Szükséges|
|---|---|---|
|Azonosító|karakterlánc|nem|
|Cím|karakterlánc|nem|
|Leírás|karakterlánc|nem|


#### <a name="video"></a>A videó 

| név | Adattípus |Szükséges|
|---|---|---|
|Azonosító|karakterlánc|nem|
|Cím|karakterlánc|nem|
|Leírás|karakterlánc|nem|
|CreationDate|karakterlánc|nem|
|Tulajdonos|karakterlánc|nem|
|ThumbnailUrl|karakterlánc|nem|
|VideoUrl|karakterlánc|nem|
|VideoDuration|egész szám|nem|
|VideoProcessingStatus|egész szám|nem|
|ViewCount|egész szám|nem|


## <a name="next-steps"></a>Következő lépések
[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).
