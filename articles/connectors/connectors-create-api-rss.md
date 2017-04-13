<properties
pageTitle="AZ RSS |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. Az RSS-összekötő lehetővé teszi, hogy a felhasználók közzététele, valamint irány elemek. Azt is lehetővé teszi a felhasználóknak műveleteket az új elem a hírcsatorna közzétételekor elindítani."
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

# <a name="get-started-with-the-rss-connector"></a>Első lépések az RSS-összekötő
RSS az gyakran frissített tartalom – például a blogbejegyzések és hírek közzétételére népszerű webhely-tartalomtípus-gyűjtési formátum.  Sok tartalom közzétevők adja meg, hogy a felhasználók hozzá Előfizetés RSS-hírcsatornára.  Az RSS-összekötő segítségével irány információk és a kiváltó ok mező flow beolvasásának új elemek közzétett az RSS-hírcsatornára.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. 

Most egy összefüggés-alkalmazás létrehozásával miként vehetik használatba, [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakör tartalmaz.

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

Az RSS-összekötő is használható művelet; trigger(s) rendelkezik. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 Az RSS-összekötő a következő műveleteket és/vagy a rendelkezésre álló trigger(s) foglalja magában:

### <a name="rss-actions"></a>Az RSS-műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Ismerkedés a összes RSS-hírcsatorna elemet.|
### <a name="rss-triggers"></a>Az RSS-eseményindítók
Az alábbi esemény hallgathatja meg:

|Eseményindító | Leírás|
|--- | ---|
|Új hírcsatorna elem közzétételének idejét|Elindítja a munkafolyamatot, új hírcsatorna közzétételekor|


## <a name="create-a-connection-to-rss"></a>Az RSS-kapcsolat létrehozása

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] A kapcsolat a többi logikájának alkalmazás is használhatja.

## <a name="reference-for-rss"></a>Az RSS hivatkozás
Verziójára vonatkozik: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Új hírcsatorna elem közzétételének idejét: elindítja a munkafolyamatot, új hírcsatorna közzétételekor 

```GET: /OnNewFeed``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|feedUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Hírcsatorna URL-címe|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|202|Elfogadott|
|400|Hibás kérés|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|404|Nem található|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="listfeeditems"></a>ListFeedItems
A lista összes RSS-hírcsatorna elem.: beszerzése minden RSS-hírcsatorna elemet. 

```GET: /ListFeedItems``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|feedUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Hírcsatorna URL-címe|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|202|Elfogadott|
|400|Hibás kérés|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|404|Nem található|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="object-definitions"></a>Objektum definíciók 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="feeditem"></a>FeedItem


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|karakterlánc|igen |
|cím|karakterlánc|igen |
|tartalom|karakterlánc|igen |
|hivatkozások|tömb|nem |
|updatedOn|karakterlánc|nem |


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)