<properties
pageTitle="Olvassa el az Azure Service Bus összekötő a logika alkalmazás |} Microsoft Azure"
description="Azure alkalmazás szolgáltatás hozzon létre logikájának alkalmazások. Azure Service Bus üzenetek küldése és fogadása szeretne csatlakozni. Például a sorba, küldje el a témakör, várólista kapott, és kapott az előfizetés műveleteket végezheti el."
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
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Első lépések az Azure Service Bus összekötő

Azure Service Bus üzenetek küldése és fogadása szeretne csatlakozni. Például a sorba, küldje el a témakör, várólista kapott, és kapott az előfizetés műveleteket végezheti el.

Szeretne használni [kívánt összekötőre](./apis-list.md), először összefüggés-alkalmazás létrehozása céljából. [Most egy összefüggés-alkalmazás](../app-service-logic/app-service-logic-create-a-logic-app.md)létrehozásával használatának megkezdéséhez.

## <a name="connect-to-service-bus"></a>Csatlakozás szolgáltatás Bus

Mielőtt a logika alkalmazás elérhető valamelyik szolgáltatás először létre kell hoznia egy kapcsolatot a szolgáltatás. [Kapcsolat](./connectors-overview.md) a összefüggés-at, és egy másik szolgáltatás közötti kapcsolatot biztosít.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>A szolgáltatás Bus eseményindító használata

Az eseményindító az eseményre kattintva elindíthatja a munkafolyamatot egy logikai alkalmazásban definiált használható. [További tudnivalók a indítók](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>A szolgáltatás Bus művelettel

Művelet egy olyan művelet, a munkafolyamat egy logikai alkalmazásban definiált által végzett. [További tudnivalók a műveletek](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Műszaki információk

Az alábbiakban a eseményindítók, műveletek és válaszokat, amely támogatja a kapcsolat adatait.

### <a name="service-bus-triggers"></a>Eseményindítók szolgáltatás Bus

A következő eseményindítók szolgáltatás Bus foglalja magában:  

|Eseményindító | Leírás|
|--- | ---|
|[Amikor üzenet érkezik egy sorban](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Ezt a műveletet egy folyamat eseményindítók, amikor egy üzenet érkezik, egy sorban.|
|[Amikor üzenet érkezik a témakör előfizetés](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Ezt a műveletet egy folyamat eseményindítók, amikor egy üzenet érkezik, a témakör előfizetés.|


### <a name="service-bus-actions"></a>Szolgáltatás Bus műveletek

Szolgáltatás Bus rendelkezik az alábbi műveleteket:


|Művelet|Leírás|
|--- | ---|
|[Üzenet küldése](connectors-create-api-servicebus.md#send-message)|Ez a művelet üzenetet küld, a várakozási sora és a tematikus.|
### <a name="action-and-trigger-details"></a>Művelet, és a kiváltó ok mező részletei

Az alábbiakban a részletek, a tevékenységek és a összekötő együtt a válaszaikhoz indítók.



#### <a name="send-message"></a>Üzenet küldése

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|ContentData *|Tartalom|Az üzenet tartalmát.|
|ContentType|Webhelytartalom-típus|Az üzenet tartalmának tartalomtípus.|
|Tulajdonságok|Tulajdonságok|Az egyes kulcs – érték párokká brokered tulajdonság.|
|egyed neve *|A várakozási/témakör neve|A várakozási vagy tematikus nevét.|

A speciális paraméterekkel közül is választhat:

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|MessageId|Üzenet azonosítója|A felhasználó által definiált érték, amely szolgáltatás Bus segítségével azonosíthatja ismétlődő üzeneteket, ha engedélyezve van.|
|A|A|Cím szeretne küldeni.|
|ReplyTo|Válasz küldése|Cím a várólista válaszolni szeretne.|
|ReplyToSessionId|Válasz a munkamenet-azonosító|Ha válaszolni szeretne a munkamenet azonosítója.|
|Címke|Címke|Az alkalmazás-specifikus címke.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Dátum és idő UTC szerint megadva, amikor az üzenet hozzáadódik a várakozási sorban található.|
|Munkamenet-azonosító|Munkamenet-azonosító|A munkamenet azonosítója.|
|CorrelationId|Korrelációs azonosító|A korrelációs azonosítója.|
|Élettartam|Élettartam|Az időtartam, osztások, hogy az üzenet nem érvényes. Az időtartam elindul, amikor az üzenet szolgáltatás Bus a rendszer elküldi a.|



Egy * jelzi, hogy szükség-e egy tulajdonságot.


#### <a name="when-a-message-is-received-in-a-queue"></a>Amikor üzenet érkezik egy sorban

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|Várólistanév *|Várólista neve|A sor nevét.|


Egy * jelzi, hogy szükség-e egy tulajdonságot.


##### <a name="output-details"></a>Kimeneti részletei

ServiceBusMessage: Ez az objektumnak a tartalom és a Tulajdonságok szolgáltatás Bus üzenet.


| Tulajdonság neve | Adattípus | Leírás |
|---|---|---|
|ContentData|karakterlánc|Az üzenet tartalmát.|
|ContentType|karakterlánc|Az üzenet tartalmának tartalomtípus.|
|Tulajdonságok|objektum|Az egyes kulcs – érték párokká brokered tulajdonság.|
|MessageId|karakterlánc|A felhasználó által definiált érték, amely szolgáltatás Bus segítségével azonosíthatja ismétlődő üzeneteket, ha engedélyezve van.|
|A|karakterlánc|Küldje el a címet.|
|ReplyTo|karakterlánc|Cím a várólista válaszolni szeretne.|
|ReplyToSessionId|karakterlánc|Ha válaszolni szeretne a munkamenet azonosítója.|
|Címke|karakterlánc|Az alkalmazás-specifikus címke.|
|ScheduledEnqueueTimeUtc|karakterlánc|Dátum és idő UTC szerint megadva, amikor az üzenet hozzáadódik a várakozási sorban található.|
|Munkamenet-azonosító|karakterlánc|A munkamenet azonosítója.|
|CorrelationId|karakterlánc|A korrelációs azonosítója.|
|Élettartam|karakterlánc|Az időtartam, osztások, hogy az üzenet nem érvényes. Az időtartam elindul, amikor az üzenet szolgáltatás Bus a rendszer elküldi a.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Amikor üzenet érkezik a témakör előfizetés

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|topicName *|Témakör neve|A témakör neve.|
|subscriptionName *|A témakör előfizetés neve|A témakör előfizetés nevére.|


Egy * jelzi, hogy szükség-e egy tulajdonságot.


##### <a name="output-details"></a>Kimeneti részletei

ServiceBusMessage: Ez az objektumnak a tartalom és a Tulajdonságok szolgáltatás Bus üzenet.


| Tulajdonság neve | Adattípus | Leírás |
|---|---|---|
|ContentData|karakterlánc|Az üzenet tartalmát.|
|ContentType|karakterlánc|Az üzenet tartalmának tartalomtípus.|
|Tulajdonságok|objektum|Az egyes kulcs – érték párokká brokered tulajdonság.|
|MessageId|karakterlánc|A felhasználó által definiált érték, amely szolgáltatás Bus segítségével azonosíthatja ismétlődő üzeneteket, ha engedélyezve van.|
|A|karakterlánc|Küldje el a címet.|
|ReplyTo|karakterlánc|Cím a várólista válaszolni szeretne.|
|ReplyToSessionId|karakterlánc|Ha válaszolni szeretne a munkamenet azonosítója.|
|Címke|karakterlánc|Az alkalmazás-specifikus címke.|
|ScheduledEnqueueTimeUtc|karakterlánc|Dátum és idő UTC szerint megadva, amikor az üzenet hozzáadódik a várakozási sorban található.|
|Munkamenet-azonosító|karakterlánc|A munkamenet azonosítója.|
|CorrelationId|karakterlánc|A korrelációs azonosítója.|
|Élettartam|karakterlánc|Az időtartam, osztások, hogy az üzenet nem érvényes. Az időtartam elindul, amikor az üzenet szolgáltatás Bus a rendszer elküldi a.|



### <a name="http-responses"></a>HTTP-válaszok

A fenti műveletek és indítók végre az alábbi HTTP állapot kódokat a térhet vissza:

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
[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).
