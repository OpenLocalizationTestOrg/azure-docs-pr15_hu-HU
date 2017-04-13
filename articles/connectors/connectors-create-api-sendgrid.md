<properties
pageTitle="SendGrid |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. SendGrid kapcsolat szolgáltatója lehetővé teszi küldhet e-mailt, és a címzett listák kezelése."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Első lépések az SendGrid összekötő

SendGrid kapcsolat szolgáltatója lehetővé teszi küldhet e-mailt, és a címzett listák kezelése.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. 

Most egy összefüggés-alkalmazás létrehozásával miként vehetik használatba, [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakör tartalmaz.

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

Az SendGrid összekötő is használható művelet; trigger(s) rendelkezik. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 Az SendGrid összekötő érhető el az alábbi műveletek tartalmaz. Vannak olyan nincs indítók.

### <a name="sendgrid-actions"></a>SendGrid műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|[EmailKüldése](connectors-create-api-sendgrid.md#sendemail)|Egy SendGrid API (legfeljebb 10 000 címzettek) használatával e-mailt küld|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Egy adott címzett hozzáadása a címzettek listájához|


## <a name="create-a-connection-to-sendgrid"></a>SendGrid kapcsolat létrehozása
SendGrid logikájának alkalmazások létrehozásához először kell hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a következő tulajdonságokat: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|ApiKey|igen|Adja meg a SendGrid Api használatával|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] A kapcsolat az egyéb összefüggés-alkalmazásokban is használhatja.

Miután létrehozta a kapcsolatot, vele hajtsa végre a műveleteket, és figyelje a jelen cikkben ismertetett eseményindítók.

## <a name="reference-for-sendgrid"></a>Összefoglalás: a SendGrid
Verziójára vonatkozik: 1.0

## <a name="sendemail"></a>EmailKüldése
E-mail küldése: SendGrid API (legfeljebb 10 000 címzettek) használatával e-mail küldése 

```POST: /api/mail.send.json``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|kérés| |igen|szervezet|nincs lehetőség|E-mail üzenet|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|404|Nem található|
|429|Túl sok kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Címzett hozzáadása listához: egy adott címzett hozzáadása a címzettek listájához 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|listId|karakterlánc|igen|elérési út|nincs lehetőség|Egyedi azonosító a címzettek listája|
|recipientId|karakterlánc|igen|elérési út|nincs lehetőség|A címzett egyedi azonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|400|Hibás kérés|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|404|Nem található|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="object-definitions"></a>Objektum definíciók 

### <a name="emailrequest"></a>EmailRequest


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|a|karakterlánc|igen |
|fromname|karakterlánc|nem |
|a|karakterlánc|igen |
|toname|karakterlánc|nem |
|Tárgy|karakterlánc|igen |
|szervezet|karakterlánc|igen |
|ishtml|logikai érték|nem |
|másolatot kap|karakterlánc|nem |
|ccname|karakterlánc|nem |
|a titkos másolat|karakterlánc|nem |
|bccname|karakterlánc|nem |
|ReplyTo|karakterlánc|nem |
|dátum|karakterlánc|nem |
|Élőfejek|karakterlánc|nem |
|fájlok|tömb|nem |
|fájlnév|tömb|nem |



### <a name="emailresponse"></a>EmailResponse


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|üzenet|karakterlánc|nem |



### <a name="recipientlists"></a>RecipientLists


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|listák|tömb|nem |



### <a name="recipientlist"></a>RecipientList


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|név|karakterlánc|nem |
|recipient_count|egész szám|nem |



### <a name="recipients"></a>A címzettek


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|a címzettek|tömb|nem |



### <a name="recipient"></a>Címzett


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|e-mailben|karakterlánc|nem |
|Vezetéknév|karakterlánc|nem |
|Utónév|karakterlánc|nem |
|azonosító|karakterlánc|nem |


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)