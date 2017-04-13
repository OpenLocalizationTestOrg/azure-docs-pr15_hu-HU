<properties
pageTitle="Wunderlist |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. Wunderlist adja meg a Teendők lista és a tevékenység manager céljából beszerzése a elvégezhetik feladataikat.  Egy bevásárlólista loved egy oszt meg, hogy a projekten dolgozó vagy egy szabadság, tervezés Wunderlist egyszerűen rögzítheti, megosztása és a to¬dos befejezéséhez. Wunderlist azonnali szinkronizálja között a telefonon, táblagépen és számítógépen, hogy bárhonnan elérhetők minden tevékenység."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Első lépések az Wunderlist összekötő

Wunderlist adja meg a Teendők lista és a tevékenység manager céljából beszerzése a elvégezhetik feladataikat.  Egy bevásárlólista loved egy oszt meg, hogy a projekten dolgozó vagy egy szabadság, tervezés Wunderlist egyszerűen rögzítheti, megosztása és a to¬dos befejezéséhez. Wunderlist azonnali szinkronizálja között a telefonon, táblagépen és számítógépen, hogy bárhonnan elérhetők minden tevékenység.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. 

Most egy összefüggés-alkalmazás létrehozásával miként vehetik használatba, [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakör tartalmaz.

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

Az Wunderlist összekötő is használható művelet; trigger(s) rendelkezik. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 Az alábbi műveleteket és/vagy a rendelkezésre álló trigger(s) az Wunderlist összekötő foglalja magában:

### <a name="wunderlist-actions"></a>Wunderlist műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|A listák, a fiókkal társított beolvasásához.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Lista létrehozása.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Feladatok beolvashatja egy adott lista.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Feladat létrehozása|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Egy adott lista vagy egy adott tevékenységhez beolvashatja az altevékenységek.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Hozzon létre egy altevékenység belül egy adott tevékenységhez|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Jegyzetek egy adott lista vagy egy adott tevékenységhez beolvasásához.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Megjegyzés hozzáadása egy adott tevékenységhez|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Lekérdezi egy adott lista vagy egy adott tevékenységhez tartozó tevékenység megjegyzések.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Megjegyzés hozzáadása egy konkrét tevékenységhez|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Lekérdezi egy adott lista vagy egy adott tevékenységhez emlékeztetőinek.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Emlékeztetők beállítása.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Lekérdezi egy adott lista vagy egy adott tevékenységhez fájlok.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Olvassa be az egy adott lista|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Lista törlése|
|[Listafrissítési](connectors-create-api-wunderlist.md#updatelist)|Egy adott lista módosítása|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Olvassa be az egy adott tevékenységhez|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Frissíti egy adott tevékenységhez|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Adott feladat törlése|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Egy adott altevékenység olvassa be|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Frissíti egy adott altevékenység|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Egy adott altevékenység törlése|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Egy adott jegyzet megjelenítéséhez|
|[LetöltéseMegjegyzés:](connectors-create-api-wunderlist.md#updatenote)|Megjegyzés frissítése|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Megjegyzés törlése|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Adott feladat Megjegyzés beolvasása|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Egy adott emlékeztető frissítése|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Egy adott emlékeztető törlése|
### <a name="wunderlist-triggers"></a>Eseményindítók Wunderlist
Az alábbi esemény hallgathatja meg:

|Eseményindító | Leírás|
|--- | ---|
|Amikor egy tevékenység esedékes|Eseményindítók egy új munkafolyamat, ha a listában szereplő feladat esedékes|
|Új tevékenység létrehozása esetén|Eseményindítók egy új munkafolyamat, ha az új feladat jön létre a lista|
|Emlékeztető esetén|Egy új munkafolyamat elindítja az emlékeztető esetén|


## <a name="create-a-connection-to-wunderlist"></a>Wunderlist kapcsolat létrehozása
Wunderlist logika alkalmazások létrehozásához először kell hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a következő tulajdonságokat: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|Jogkivonat|igen|Wunderlist hitelesítő adatok megadása|
Miután létrehozta a kapcsolatot, vele hajtsa végre a műveleteket, és figyelje a jelen cikkben ismertetett eseményindítók. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] A kapcsolat a többi logikájának alkalmazás is használhatja.

## <a name="reference-for-wunderlist"></a>Összefoglalás: a Wunderlist
Verziójára vonatkozik: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Amikor egy tevékenység esedékes: egy új munkafolyamat eseményindítók, amikor a listában szereplő feladat esedékes. 

```GET: /trigger/tasksdue``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|


## <a name="triggertasknew"></a>TriggerTaskNew
Új tevékenység létrehozása esetén: eseményindítók egy új munkafolyamat, ha az új feladat jön létre a lista 

```GET: /trigger/tasksnew``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|


## <a name="triggerreminder"></a>TriggerReminder
Emlékeztető bekövetkezésekor: egy új munkafolyamat elindítja az emlékeztető esetén 

```GET: /trigger/reminders``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|a TASK_ID|egész szám|nem|lekérdezés|nincs lehetőség|Tevékenységazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|


## <a name="retrievelists"></a>RetrieveLists
Listák kérjen: a listák, a fiókkal társított beolvasásához. 

```GET: /lists``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="createlist"></a>CreateList
Lista létrehozása: lista létrehozása. 

```POST: /lists``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|bejegyzés| |igen|szervezet|nincs lehetőség|Létrejön az új lista|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="listtasks"></a>ListTasks
Tevékenységek első: tevékenységek lekérhetők egy adott listához. 

```GET: /tasks``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|Befejezett|logikai érték|nem|lekérdezés|nincs lehetőség|Befejezett|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="createtask"></a>CreateTask
Feladat létrehozása: feladat létrehozása 

```POST: /tasks``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|bejegyzés| |igen|szervezet|nincs lehetőség|Új feladat létrehozására|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|201|Létrehozva|


## <a name="listsubtasks"></a>ListSubTasks
Altevékenységek első: altevékenységek beolvasásához, egy adott lista vagy egy adott tevékenységhez. 

```GET: /subtasks``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|a TASK_ID|egész szám|nem|lekérdezés|nincs lehetőség|Tevékenységazonosító|
|Befejezett|logikai érték|nem|lekérdezés|nincs lehetőség|Befejezett|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="createsubtask"></a>CreateSubTask
Hozzon létre egy altevékenység: hozzon létre egy altevékenység belül egy adott tevékenységhez 

```POST: /subtasks``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|bejegyzés| |igen|szervezet|nincs lehetőség|A létrehozandó új altevékenység|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|201|Létrehozva|


## <a name="listnotes"></a>ListNotes
Ismerkedés a jegyzetek: beolvasni a jegyzetek egy adott lista vagy egy adott tevékenységhez. 

```GET: /notes``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|a TASK_ID|egész szám|nem|lekérdezés|nincs lehetőség|Tevékenységazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="createnote"></a>CreateNote
Feljegyzés létrehozása: Megjegyzés hozzáadása egy adott tevékenységhez 

```POST: /notes``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|bejegyzés| |igen|szervezet|nincs lehetőség|Új megjegyzés: a hozható létre|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|201|Létrehozva|


## <a name="listcomments"></a>ListComments
Tevékenység megjegyzések első: beolvasni a tevékenység megjegyzések egy adott lista vagy egy adott tevékenységhez. 

```GET: /task_comments``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|a TASK_ID|egész szám|nem|lekérdezés|nincs lehetőség|Tevékenységazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="createcomment"></a>CreateComment
Megjegyzés hozzáadása tevékenységhez: Megjegyzés hozzáadása egy konkrét tevékenységhez 

```POST: /task_comments``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|bejegyzés| |igen|szervezet|nincs lehetőség|Új tevékenység megjegyzést hozható létre|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|201|Létrehozva|


## <a name="retrievereminders"></a>RetrieveReminders
Emlékeztetők kérjen: egy adott lista vagy egy adott tevékenységhez emlékeztetőinek beolvasásához. 

```GET: /reminders``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|a TASK_ID|egész szám|nem|lekérdezés|nincs lehetőség|Tevékenységazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="createreminder"></a>CreateReminder
Emlékeztetők beállítása: emlékeztetők beállítása. 

```POST: /reminders``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|bejegyzés| |igen|szervezet|nincs lehetőség|A létrehozandó új emlékeztető|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="retrievefiles"></a>RetrieveFiles
Fájlok: egy adott lista vagy egy adott tevékenységhez fájlok beolvasásához. 

```GET: /files``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|a TASK_ID|egész szám|nem|lekérdezés|nincs lehetőség|Tevékenységazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|A sikeres művelet|
|400|Hibás kérés|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="getlist"></a>GetList
Lista beszerzése: keres vissza egy adott lista 

```GET: /lists/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Lista azonosítója|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="deletelist"></a>DeleteList
Lista törlése: törli a lista 

```DELETE: /lists/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Lista azonosítója|
|verzió|egész szám|igen|lekérdezés|nincs lehetőség|Verzió|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|204.|Nincs tartalom|


## <a name="updatelist"></a>Listafrissítési
Frissítse a listát: egy adott lista módosítása 

```PATCH: /lists/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Lista azonosítója|
|bejegyzés| |igen|szervezet|nincs lehetőség|A lista, részletek|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="gettask"></a>GetTask
Ismerkedés a tevékenység: keres vissza egy adott tevékenységhez 

```GET: /tasks/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Tevékenységazonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="updatetask"></a>UpdateTask
Tevékenység frissítése: frissíti egy adott tevékenységhez 

```PATCH: /tasks/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Tevékenységazonosító|
|bejegyzés| |igen|szervezet|nincs lehetőség|Feladat részletei|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="deletetask"></a>DeleteTask
Tevékenység törlése: törli az adott tevékenység 

```DELETE: /tasks/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|list_id|egész szám|igen|lekérdezés|nincs lehetőség|Lista azonosítója|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Tevékenységazonosító|
|verzió|egész szám|igen|lekérdezés|nincs lehetőség|Verzió|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|204.|Nincs tartalom|


## <a name="getsubtask"></a>GetSubTask
Első altevékenység: keres vissza egy adott altevékenység 

```GET: /subtasks/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Altevékenység azonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="updatesubtask"></a>UpdateSubTask
Frissítse a altevékenység: frissíti egy adott altevékenység 

```PATCH: /subtasks/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Altevékenység azonosító|
|bejegyzés| |igen|szervezet|nincs lehetőség|Altevékenység részletei|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="deletesubtask"></a>DeleteSubTask
Egy altevékenység törlése: törli az egy adott altevékenység 

```DELETE: /subtasks/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Altevékenység azonosító|
|verzió|egész szám|igen|lekérdezés|nincs lehetőség|Verzió|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|204.|Nincs tartalom|


## <a name="getnote"></a>GetNote
Jegyzet első: Megjegyzés beolvasásához. 

```GET: /notes/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Megjegyzés: azonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="updatenote"></a>LetöltéseMegjegyzés:
Jegyzet frissítése: Megjegyzés frissítése 

```PATCH: /notes/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Megjegyzés: azonosító|
|bejegyzés| |igen|szervezet|nincs lehetőség|Megjegyzés: részletei|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="deletenote"></a>DeleteNote
Megjegyzés törléséhez: Megjegyzés törlése 

```DELETE: /notes/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Megjegyzés: azonosító|
|verzió|egész szám|igen|lekérdezés|nincs lehetőség|Verzió|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|204.|Nincs tartalom|


## <a name="getcomment"></a>GetComment
Tevékenység Megjegyzés kérjen: egy adott tevékenységhez megjegyzés beolvasása 

```GET: /task_comments/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|karakterlánc|igen|elérési út|nincs lehetőség|Megjegyzés azonosító|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="updatereminder"></a>UpdateReminder
Emlékeztető frissítése: egy adott emlékeztető frissítése 

```PATCH: /reminders/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Emlékeztető azonosító|
|bejegyzés| |igen|szervezet|nincs lehetőség|Emlékeztető részletei|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|200|oké|


## <a name="deletereminder"></a>DeleteReminder
Emlékeztető törlése: törli az adott emlékeztető 

```DELETE: /reminders/{id}``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|azonosító|egész szám|igen|elérési út|nincs lehetőség|Az emlékeztető azonosítója.|
|verzió|egész szám|igen|lekérdezés|nincs lehetőség|Verzió|

#### <a name="response"></a>Válasz

|név|Leírás|
|---|---|
|204.|Nincs tartalom|


## <a name="object-definitions"></a>Objektum definíciók 

### <a name="list"></a>Lista


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|created_at|karakterlánc|nem |
|cím|karakterlánc|nem |
|list_type|karakterlánc|nem |
|típus|karakterlánc|nem |
|verzió|egész szám|nem |



### <a name="createdlist"></a>CreatedList


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|created_at|karakterlánc|nem |
|cím|karakterlánc|nem |
|verzió|egész szám|nem |
|típus|karakterlánc|nem |



### <a name="task"></a>Tevékenység


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|assignee_id|egész szám|nem |
|assigner_id|egész szám|nem |
|created_at|karakterlánc|nem |
|created_by_id|egész szám|nem |
|due_date|karakterlánc|nem |
|list_id|egész szám|nem |
|verzió|egész szám|nem |
|starred|logikai érték|nem |
|cím|karakterlánc|nem |



### <a name="subtask"></a>Altevékenység


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|a TASK_ID|egész szám|nem |
|created_at|karakterlánc|nem |
|created_by_id|egész szám|nem |
|verzió|karakterlánc|nem |
|cím|karakterlánc|nem |



### <a name="note"></a>Megjegyzés:


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|a TASK_ID|egész szám|nem |
|tartalom|karakterlánc|nem |
|created_at|karakterlánc|nem |
|updated_at|karakterlánc|nem |
|verzió|egész szám|nem |



### <a name="comment"></a>Megjegyzés


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|a TASK_ID|egész szám|nem |
|verzió|egész szám|nem |
|szöveg|karakterlánc|nem |
|típus|karakterlánc|nem |
|created_at|karakterlánc|nem |



### <a name="reminder"></a>Emlékeztető


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|dátum|karakterlánc|nem |
|a TASK_ID|egész szám|nem |
|verzió|egész szám|nem |
|típus|karakterlánc|nem |
|created_at|karakterlánc|nem |
|updated_at|karakterlánc|nem |



### <a name="createdreminder"></a>CreatedReminder


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|dátum|karakterlánc|nem |
|a TASK_ID|egész szám|nem |
|verzió|egész szám|nem |
|created_at|karakterlánc|nem |
|updated_at|karakterlánc|nem |



### <a name="file"></a>Fájl


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|azonosító|egész szám|nem |
|URL-címe|karakterlánc|nem |
|a TASK_ID|egész szám|nem |
|list_id|egész szám|nem |
|USER_ID|egész szám|nem |
|fájlnév|karakterlánc|nem |
|CONTENT_TYPE|karakterlánc|nem |
|file_size|egész szám|nem |
|local_created_at|karakterlánc|nem |
|created_at|karakterlánc|nem |
|updated_at|karakterlánc|nem |
|típus|karakterlánc|nem |
|verzió|egész szám|nem |



### <a name="newtask"></a>Új feladat


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|list_id|egész szám|igen |
|cím|karakterlánc|igen |
|assignee_id|egész szám|nem |
|Befejezett|logikai érték|nem |
|recurrence_type|karakterlánc|nem |
|recurrence_count|egész szám|nem |
|due_date|karakterlánc|nem |
|starred|logikai érték|nem |



### <a name="newlist"></a>NewList


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|cím|karakterlánc|igen |



### <a name="newsubtask"></a>NewSubtask


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|list_id|egész szám|igen |
|a TASK_ID|egész szám|igen |
|cím|karakterlánc|igen |
|Befejezett|logikai érték|nem |



### <a name="newnote"></a>NewNote


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|list_id|egész szám|igen |
|a TASK_ID|egész szám|igen |
|tartalom|karakterlánc|igen |



### <a name="newcomment"></a>Új_megjegyzés


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|list_id|egész szám|igen |
|a TASK_ID|egész szám|igen |
|szöveg|karakterlánc|igen |



### <a name="newreminder"></a>NewReminder


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|list_id|egész szám|igen |
|a TASK_ID|egész szám|igen |
|dátum|karakterlánc|igen |



### <a name="updatetask"></a>UpdateTask


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|verzió|egész szám|nem |
|cím|karakterlánc|nem |
|assignee_id|egész szám|nem |
|Befejezett|logikai érték|nem |
|recurrence_type|karakterlánc|nem |
|recurrence_count|egész szám|nem |
|due_date|karakterlánc|nem |
|starred|logikai érték|nem |



### <a name="updatelist"></a>Listafrissítési


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|verzió|egész szám|nem |
|cím|karakterlánc|nem |



### <a name="updatesubtask"></a>UpdateSubtask


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|verzió|egész szám|nem |
|cím|karakterlánc|nem |
|Befejezett|logikai érték|nem |



### <a name="updatenote"></a>LetöltéseMegjegyzés:


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|verzió|egész szám|nem |
|tartalom|karakterlánc|nem |



### <a name="updatereminder"></a>UpdateReminder


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|dátum|karakterlánc|nem |
|verzió|egész szám|nem |


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)