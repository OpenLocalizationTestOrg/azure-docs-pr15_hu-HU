<properties
pageTitle="Outlook.com |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. Outlook.com connector lehetővé teszi a levelezés, naptárak és névjegyek kezelése. Végezze el a különféle műveletek, például küldése e-mailt, -értekezleteket ütemezni, adja hozzá a névjegyek stb."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Első lépések az Outlook.com-összekötő

Outlook.com connector lehetővé teszi a levelezés, naptárak és névjegyek kezelése. Végezze el a különféle műveletek, például küldése e-mailt, -értekezleteket ütemezni, adja hozzá a névjegyek stb.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. 

Most egy összefüggés-alkalmazás létrehozásával miként vehetik használatba, [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakör tartalmaz.

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

Az Outlook.com-összekötő is alkalmazható művelet; trigger(s) rendelkezik. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 Az Outlook.com-összekötő a következő műveleteket és/vagy a rendelkezésre álló trigger(s) foglalja magában:

### <a name="outlookcom-actions"></a>Outlook.com-műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|E-mailek átveszi mappa|
|[EmailKüldése](connectors-create-api-outlook.md#SendEmail)|E-mail küldése|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Törli az e-mailben azonosító szerint|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|E-mailben, mint az olvasás utáni jelek|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Válasz írása az e-mailben|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Olvassa be e-mailben mellékletként azonosító szerint|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Küldjön e-mailt több beállításokkal, és várja meg a címzettet a vissza a lehetőségek közül a válaszadásra|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Jóváhagyási e-mail küldése, és várja meg a címzett visszajelzés|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Olvassa be a naptárakban|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Elemek beolvassa a naptárból|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Új esemény létrehozása|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Olvassa be az egy adott elemet a naptárból|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|A naptár elem törlése|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Részben frissíti a naptárelem|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Olvassa be a Névjegyalbum mappában|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Névjegyek beolvassa a Névjegyalbum mappa|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Új névjegy létrehozása|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Adott partnerrel beolvassa a Névjegyalbum mappa|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Névjegy törlése|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Névjegy részben frissítése|
### <a name="outlookcom-triggers"></a>Outlook.com indítók
Az alábbi esemény hallgathatja meg:

|Eseményindító | Leírás|
|--- | ---|
|A közelgő eseményt|Egy folyamat eseményindítók, egy közelgő naptáresemény indításakor|
|Új e-mailek|Eseményindítók egy folyamat, új e-mail érkezésekor|
|Kattintson az új elemek|Indul el, ha egy új naptárelem létrehozása|
|A frissített elemek|Naptárelem módosításakor indított|


## <a name="create-a-connection-to-outlookcom"></a>Outlook.com-kapcsolat létrehozása
Logika alkalmazások létrehozása az Outlook.com, először kell hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a következő tulajdonságokat: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|Jogkivonat|igen|Outlook.com hitelesítő adatok megadása|
Miután létrehozta a kapcsolatot, vele hajtsa végre a műveleteket, és figyelje a jelen cikkben ismertetett eseményindítók.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] A kapcsolat a többi logikájának alkalmazás is használhatja.  

## <a name="reference-for-outlookcom"></a>Outlook.com részletes ismertetése
Verziójára vonatkozik: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
A közelgő eseményt: elindítja a folyamat egy közelgő naptáresemény indításakor 

```GET: /Events/OnUpcomingEvents``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|lekérdezés|nincs lehetőség|A naptár egyedi azonosító|
|lookAheadTimeInMinutes|egész szám|nem|lekérdezés|15|A következő keres a közelgő eseményekről (percben) idő|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|202|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="getemails"></a>GetEmails
E-mailek első: e-mailek átveszi mappa 

```GET: /Mail``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappa_útvonala|karakterlánc|nem|lekérdezés|Beérkezett üzenetek mappa|Az e-mailek beolvasásához mappa elérési útját (alapértelmezett: "Beérkezett üzenetek")|
|felső|egész szám|nem|lekérdezés|10|E-mailek beolvasásához száma (alapértelmezés: 10)|
|fetchOnlyUnread|logikai érték|nem|lekérdezés|Igaz|A lekérni csak olvasatlan e-mailek?|
|includeAttachments|logikai érték|nem|lekérdezés|hamis|Ha az értéke igaz, mellékletek is lesz visszakeresve, amely az e-mailt együtt|
|searchQuery|karakterlánc|nem|lekérdezés|nincs lehetőség|Keresési lekérdezés az e-mailek szűrése|
|kihagyása|egész szám|nem|lekérdezés|0|E-mailek kihagyása száma (alapértelmezett: 0)|
|skipToken|karakterlánc|nem|lekérdezés|nincs lehetőség|Jogkivonat ugorja át a fájllehívási új lapra|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="sendemail"></a>EmailKüldése
E-mail küldése: e-mailt küld. 

```POST: /Mail``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|üzenethez| |igen|szervezet|nincs lehetőség|E-mailben|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="deleteemail"></a>DeleteEmail
E-mail törlése: törli az e-mailben azonosító szerint 

```DELETE: /Mail/{messageId}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|messageId|karakterlánc|igen|elérési út|nincs lehetőség|Az e-mailben törlése azonosítója|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="markasread"></a>MarkAsRead
Elem megjelölése olvasottként: jelöli meg, hogy elolvasta e-mailben 

```POST: /Mail/MarkAsRead/{messageId}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|messageId|karakterlánc|igen|elérési út|nincs lehetőség|Az e-mailben lesznek olvasottként megjelölve, azonosítója olvasása|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="replyto"></a>ReplyTo
E-mail megválaszolása: válaszok egy e-mailhez 

```POST: /Mail/ReplyTo/{messageId}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|messageId|karakterlánc|igen|elérési út|nincs lehetőség|Az e-mailt szeretne válaszolni azonosítója|
|Megjegyzés|karakterlánc|igen|lekérdezés|nincs lehetőség|Válasz a megjegyzésre|
|replyAll|logikai érték|nem|lekérdezés|hamis|Válasz küldése minden címzett|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="getattachment"></a>GetAttachment
Melléklet első: kérdezi le e-mailben mellékletként azonosító szerint 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|messageId|karakterlánc|igen|elérési út|nincs lehetőség|Az e-mailt azonosítója|
|mellékletazonosító|karakterlánc|igen|elérési út|nincs lehetőség|A mellékletre és letöltés azonosítója|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="onnewemail"></a>OnNewEmail
Új e-mailek: elindítja a folyamat, új e-mail érkezésekor 

```GET: /Mail/OnNewEmail``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|Mappa_útvonala|karakterlánc|nem|lekérdezés|Beérkezett üzenetek mappa|Mappa beolvasásához (alapértelmezett: Beérkezett üzenetek mappa)|
|a|karakterlánc|nem|lekérdezés|nincs lehetőség|Címzettek e-mail címét|
|a|karakterlánc|nem|lekérdezés|nincs lehetőség|Címről:|
|sürgős|karakterlánc|nem|lekérdezés|Normál|Az e-mailt (magas, normál, alacsony) fontosságát (alapértelmezett: normál)|
|fetchOnlyWithAttachment|logikai érték|nem|lekérdezés|hamis|Lekérés csak az e-maileket melléklettel együtt|
|includeAttachments|logikai érték|nem|lekérdezés|hamis|Mellékletet tartalmaznak|
|subjectFilter|karakterlánc|nem|lekérdezés|nincs lehetőség|A Tárgy mezőben keres karakterlánc|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A művelet sikeresen befejeződött|
|202|Elfogadott|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Beállítások az e-mail küldése: küldjön e-mailt több beállításokkal, és várja meg a címzettet a vissza a lehetőségek közül a válaszadásra 

```POST: /mailwithoptions/$subscriptions``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |igen|szervezet|nincs lehetőség|Előfizetés kérése e-mail beállítások|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|201|Az előfizetés létrehozása|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Jóváhagyás e-mail küldése: jóváhagyási e-mail küldése, és várja meg a címzett visszajelzés 

```POST: /approvalmail/$subscriptions``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |igen|szervezet|nincs lehetőség|Előfizetés kérelem jóváhagyása a levelezéshez|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|201|Az előfizetés létrehozása|
|400|BadRequest|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|500|Belső kiszolgálóhiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendargettables"></a>CalendarGetTables
Ismerkedés a naptárak: naptárak olvassa be 

```GET: /datasets/calendars/tables``` 

A hívás nincsenek paraméterei
#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendargetitems"></a>CalendarGetItems
Események kérjen: elemek beolvassa a naptárból 

```GET: /datasets/calendars/tables/{table}/items``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|A naptár beolvasásához egyedi azonosító|
|$filter|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA szűrő lekérdezés korlátozása bejegyzéseinek száma|
|$orderby|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA orderBy lekérdezés bejegyzések sorrendjének megadása|
|$skip|egész szám|nem|lekérdezés|nincs lehetőség|Kihagyása bejegyzéseinek száma (alapértelmezett = 0)|
|$top|egész szám|nem|lekérdezés|nincs lehetőség|Beolvasásához bejegyzések maximális száma (alapértelmezett = 256)|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendarpostitem"></a>CalendarPostItem
Esemény létrehozása: egy új esemény létrehozása 

```POST: /datasets/calendars/tables/{table}/items``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Naptár egyedi azonosítója|
|elem| |igen|szervezet|nincs lehetőség|Naptárelem létrehozása|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendargetitem"></a>CalendarGetItem
Esemény első: keres vissza egy adott elemet a naptárból 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Naptár egyedi azonosítója|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Naptárelem beolvasásához egyedi azonosítója|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Esemény törlése: törli naptárelem 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Naptár egyedi azonosítója|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Egyedi azonosító naptár elem törlése|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Frissítés esemény: részben frissíti a naptárelem 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Naptár egyedi azonosítója|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Egyedi azonosítója frissítése elemre.|
|elem| |igen|szervezet|nincs lehetőség|Naptárelem frissítése|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Kattintson az új elemek: indul el, ha egy új naptárelem létrehozása 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Naptár egyedi azonosítója|
|$filter|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA szűrő lekérdezés korlátozása bejegyzéseinek száma|
|$orderby|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA orderBy lekérdezés bejegyzések sorrendjének megadása|
|$skip|egész szám|nem|lekérdezés|nincs lehetőség|Kihagyása bejegyzéseinek száma (alapértelmezett = 0)|
|$top|egész szám|nem|lekérdezés|nincs lehetőség|Beolvasásához bejegyzések maximális száma (alapértelmezett = 256)|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
A frissített cikkek: indul el, ha naptárelem lett módosítva 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Naptár egyedi azonosítója|
|$filter|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA szűrő lekérdezés korlátozása bejegyzéseinek száma|
|$orderby|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA orderBy lekérdezés bejegyzések sorrendjének megadása|
|$skip|egész szám|nem|lekérdezés|nincs lehetőség|Kihagyása bejegyzéseinek száma (alapértelmezett = 0)|
|$top|egész szám|nem|lekérdezés|nincs lehetőség|Beolvasásához bejegyzések maximális száma (alapértelmezett = 256)|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="contactgettables"></a>ContactGetTables
A névjegymappák első: olvassa be a Névjegyek mappa 

```GET: /datasets/contacts/tables``` 

A hívás nincsenek paraméterei
#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="contactgetitems"></a>ContactGetItems
Névjegyek: névjegyek beolvassa a Névjegyalbum mappa 

```GET: /datasets/contacts/tables/{table}/items``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|A Névjegyek mappa beolvasásához egyedi azonosító|
|$filter|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA szűrő lekérdezés korlátozása bejegyzéseinek száma|
|$orderby|karakterlánc|nem|lekérdezés|nincs lehetőség|Az ODATA orderBy lekérdezés bejegyzések sorrendjének megadása|
|$skip|egész szám|nem|lekérdezés|nincs lehetőség|Kihagyása bejegyzéseinek száma (alapértelmezett = 0)|
|$top|egész szám|nem|lekérdezés|nincs lehetőség|Beolvasásához bejegyzések maximális száma (alapértelmezett = 256)|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="contactpostitem"></a>ContactPostItem
Névjegy létrehozása: új névjegy létrehozása 

```POST: /datasets/contacts/tables/{table}/items``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Névjegymappa egyedi azonosítója|
|elem| |igen|szervezet|nincs lehetőség|Névjegy létrehozása|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="contactgetitem"></a>ContactGetItem
Partner első: egy adott partnerrel beolvassa a Névjegyalbum mappa 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Névjegyalbummappa egyedi azonosítója|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|A partnerek beolvasásához egyedi azonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Partner törlése: törli a névjegy 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Névjegymappa egyedi azonosítója|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Egyedi azonosító partner törlése|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="contactpatchitem"></a>ContactPatchItem
Partner frissítése: részben frissíti a névjegy 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|táblázat|karakterlánc|igen|elérési út|nincs lehetőség|Névjegyalbummappa egyedi azonosítója|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Kapcsolattartó frissítése egyedi azonosítója|
|elem| |igen|szervezet|nincs lehetőség|Névjegykártya frissítése|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="object-definitions"></a>Objektum definíciók 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [karakterlánc, objektum]]


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="object"></a>Objektum


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Mellékletek|tömb|nem |
|A|karakterlánc|nem |
|Másolatot kap|karakterlánc|nem |
|A titkos másolat|karakterlánc|nem |
|Tárgy|karakterlánc|igen |
|Szervezet|karakterlánc|igen |
|Sürgős|karakterlánc|nem |
|IsHtml|logikai érték|nem |
|A|karakterlánc|igen |



### <a name="sendattachment"></a>SendAttachment


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|@odata.type|karakterlánc|nem |
|név|karakterlánc|igen |
|ContentBytes|karakterlánc|igen |



### <a name="receivemessage"></a>ReceiveMessage


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Azonosító|karakterlánc|nem |
|IsRead|logikai érték|nem |
|HasAttachment|logikai érték|nem |
|DateTimeReceived|karakterlánc|nem |
|Mellékletek|tömb|nem |
|A|karakterlánc|nem |
|Másolatot kap|karakterlánc|nem |
|A titkos másolat|karakterlánc|nem |
|Tárgy|karakterlánc|igen |
|Szervezet|karakterlánc|igen |
|Sürgős|karakterlánc|nem |
|IsHtml|logikai érték|nem |
|A|karakterlánc|igen |



### <a name="receiveattachment"></a>ReceiveAttachment


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Azonosító|karakterlánc|igen |
|ContentType|karakterlánc|igen |
|@odata.type|karakterlánc|nem |
|név|karakterlánc|igen |
|ContentBytes|karakterlánc|igen |



### <a name="digestmessage"></a>DigestMessage


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Tárgy|karakterlánc|igen |
|Szervezet|karakterlánc|nem |
|Sürgős|karakterlánc|nem |
|Kivonat|tömb|igen |
|Mellékletek|tömb|nem |
|A|karakterlánc|igen |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



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



### <a name="tablemetadata"></a>TableMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|név|karakterlánc|nem |
|cím|karakterlánc|nem |
|x – az ms-engedélyek|karakterlánc|nem |
|x – az ms-funkciók|Nincs megadva|nem |
|séma|Nincs megadva|nem |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|sortRestrictions|Nincs megadva|nem |
|filterRestrictions|Nincs megadva|nem |
|filterFunctions|tömb|nem |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|rendezhető|logikai érték|nem |
|unsortableProperties|tömb|nem |
|ascendingOnlyProperties|tömb|nem |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|szűrhető|logikai érték|nem |
|nonFilterableProperties|tömb|nem |
|requiredProperties|tömb|nem |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|NotificationUrl|karakterlánc|nem |
|Üzenet|Nincs megadva|nem |



### <a name="messagewithoptions"></a>MessageWithOptions


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Tárgy|karakterlánc|igen |
|Beállítások|karakterlánc|igen |
|Szervezet|karakterlánc|nem |
|Sürgős|karakterlánc|nem |
|Mellékletek|tömb|nem |
|A|karakterlánc|igen |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|karakterlánc|nem |
|erőforrás|karakterlánc|nem |
|NotificationTípus|karakterlánc|nem |
|notificationUrl|karakterlánc|nem |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|NotificationUrl|karakterlánc|nem |
|Üzenet|Nincs megadva|nem |



### <a name="approvalmessage"></a>ApprovalMessage


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Tárgy|karakterlánc|igen |
|Beállítások|karakterlánc|igen |
|Szervezet|karakterlánc|nem |
|Sürgős|karakterlánc|nem |
|Mellékletek|tömb|nem |
|A|karakterlánc|igen |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|SelectedOption|karakterlánc|nem |



### <a name="tableslist"></a>TablesList


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="table"></a>Táblázat


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|név|karakterlánc|nem |
|DisplayName|karakterlánc|nem |



### <a name="item"></a>Elem


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|ItemInternalId|karakterlánc|nem |



### <a name="calendaritemslist"></a>CalendarItemsList


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="calendaritem"></a>CalendarItem


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|ItemInternalId|karakterlánc|nem |



### <a name="contactitemslist"></a>ContactItemsList


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="contactitem"></a>ContactItem


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|ItemInternalId|karakterlánc|nem |



### <a name="datasetslist"></a>DataSetsList


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="dataset"></a>Adatkészlet


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|név|karakterlánc|nem |
|DisplayName|karakterlánc|nem |


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)