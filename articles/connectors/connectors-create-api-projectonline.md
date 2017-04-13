<properties
pageTitle="Project Online |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. Project Online a project portfólió kezeléshez és a Microsoft mindennapos munka rugalmas online megoldást. Kézbesítve az Office 365 segítségével, a Project Online lehetővé teszi, hogy gyorsan használatba hatékony projektkezelési rendszer megtervezése, a projektekkel és projektek és a project portfolió-befektetések kezelése a szervezet – szinte bárhonnan szinte bármilyen eszközről."
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

# <a name="get-started-with-the-projectonline-connector"></a>Első lépések a Project Online-összekötő

Project Online a project portfólió kezeléshez és a Microsoft mindennapos munka rugalmas online megoldást. Kézbesítve az Office 365 segítségével, a Project Online lehetővé teszi, hogy gyorsan használatba hatékony projektkezelési rendszer megtervezése, a projektekkel és projektek és a project portfolió-befektetések kezelése a szervezet – szinte bárhonnan szinte bármilyen eszközről.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. 

Most egy összefüggés-alkalmazás létrehozásával miként vehetik használatba, [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakör tartalmaz.

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

A Project Online-összekötő is használható művelet; trigger(s) rendelkezik. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 A Project Online-összekötő a következő műveleteket és/vagy a rendelkezésre álló trigger(s) foglalja magában:

### <a name="projectonline-actions"></a>Project online-műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Megjeleníti a projekteket a project online-webhely|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Létrehoz egy új projektet a project online-webhely|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Új feladatot hoz létre, a projektben|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|A project online webhely hoz létre egy vállalati erőforrások|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|A projekt közzétett tevékenységeinek listája|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|A webhely projekt kiveszi|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Jelölje be meglévő a webhelyen a project és közzététele|
### <a name="projectonline-triggers"></a>Project Online indítók
Az alábbi esemény hallgathatja meg:

|Eseményindító | Leírás|
|--- | ---|
|Új projekt létrehozása esetén|Egy folyamat eseményindítók, valahányszor új projekt létrehozása|
|Új erőforrás létrehozásakor|Eseményindítók egy új folyamat, új erőforrás létrehozásakor|
|Új tevékenység létrehozása esetén|Egy folyamat eseményindítók, amikor az új feladat jön létre.|


## <a name="create-a-connection-to-projectonline"></a>Project Online-kapcsolat létrehozása
Logika alkalmazások létrehozását a Project Online, először kell hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a következő tulajdonságokat: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|Jogkivonat|igen|Adja meg a Project online hitelesítő adatok|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] A kapcsolat az egyéb összefüggés-alkalmazásokban is használhatja.

## <a name="reference-for-projectonline"></a>Összefoglalás: a Project Online
Verziójára vonatkozik: 1.0

## <a name="onnewproject"></a>OnNewProject
Új projekt létrehozása esetén: a folyamat eseményindítók, valahányszor új projekt létrehozása 

```GET: /trigger/_api/ProjectData/Projects``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
Új erőforrás létrehozásakor: kezdeményez egy új folyamat, új erőforrás létrehozásakor 

```GET: /trigger/_api/ProjectData/Resources``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
Új tevékenység létrehozása esetén: a munkafolyamat eseményindítók, amikor az új feladat jön létre 

```GET: /trigger/_api/ProjectData/Tasks``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
Projektek listája: Megjeleníti a projekteket a project online-webhely 

```GET: /_api/ProjectServer/Projects``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>CreateProject
Új projekt létrehozása: létrehoz egy új projektet a project online-webhely 

```POST: /_api/ProjectServer/Projects``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|
|Proj| |igen|szervezet|nincs lehetőség|Új projekt létrehozása|

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


## <a name="createtask"></a>CreateTask
Új feladat létrehozása: új feladatot hoz létre, a projektben 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|karakterlánc|igen|elérési út|nincs lehetőség|A tevékenység hozzáadása a projekt egyedi azonosító|
|tevékenység| |igen|szervezet|nincs lehetőség|Új tevékenység hozzáadása a projekthez|

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


## <a name="createresource"></a>CreateResource
Hozzon létre új erőforrás: a project online webhely hoz létre egy vállalati erőforrások 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|
|erőforrás| |igen|szervezet|nincs lehetőség|Új vállalati erőforrás hozzáadása a projekthez|

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


## <a name="listtasks"></a>ListTasks
Feladatokat sorolja fel: a projekt közzétett tevékenységeinek listája 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|karakterlánc|igen|elérési út|nincs lehetőség|A projekt-ból tevékenységek egyedi azonosító|

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


## <a name="checkoutproject"></a>CheckoutProject
A projekt kivételét: kivesz egy projekt, a webhelyen 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|karakterlánc|igen|elérési út|nincs lehetőség|A tevékenység hozzáadása a projekt egyedi azonosító|

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


## <a name="publishproject"></a>PublishProject
A beadás és a projekt közzététele: beadása és közzététele a webhelyen a project meglévő 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|siteUrl|karakterlánc|igen|lekérdezés|nincs lehetőség|Legfelső szintű a projektwebhely webhely URL-címét (például: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|karakterlánc|igen|elérési út|nincs lehetőség|A projekt a beadás egyedi azonosító|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="triggerproject"></a>TriggerProject


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|ProjectStartDate|karakterlánc|nem |
|ProjectFinishDate|karakterlánc|nem |
|ProjectCreatedDate|karakterlánc|nem |
|ProjectId|karakterlánc|nem |
|ProjectModifiedDate|karakterlánc|nem |
|ProjectType|egész szám|nem |
|Projektnév|karakterlánc|nem |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="triggerresource"></a>TriggerResource


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|ResourceId|karakterlánc|nem |
|ResourceBaseCalendar|karakterlánc|nem |
|ResourceBookingType|egész szám|nem |
|ResourceCanLevel|logikai érték|nem |
|ResourceCostPerUse|szám|nem |
|ResourceCreatedDate|karakterlánc|nem |
|ResourceEarliestAvailableFrom|karakterlánc|nem |
|ResourceEmail|karakterlánc|nem |
|ResourceInitials|karakterlánc|nem |
|ResourceIsActive|logikai érték|nem |
|ResourceIsGeneric|logikai érték|nem |
|ResourceLatestAvailableTo|karakterlánc|nem |
|ResourceModifiedDate|karakterlánc|nem |
|Erőforrásnév|karakterlánc|nem |
|ResourceStatsuName|karakterlánc|nem |
|Erőforrástípus|egész szám|nem |
|TypeDescription|karakterlánc|nem |
|TypeName|karakterlánc|nem |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="triggertask"></a>TriggerTask


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|ProjectId|karakterlánc|nem |
|TaskId|karakterlánc|nem |
|Projektnév|karakterlánc|nem |
|Feladatnév|karakterlánc|nem |
|TaskCreatedDate|karakterlánc|nem |
|TaskModifieddate|karakterlánc|nem |
|TaskStartDate|karakterlánc|nem |
|TaskFinishDate|karakterlánc|nem |
|TaskPriority|egész szám|nem |
|TaskIsActive|logikai érték|nem |



### <a name="newproject"></a>NewProject


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|név|karakterlánc|igen |
|Leírás|karakterlánc|nem |
|Indítása|karakterlánc|nem |



### <a name="newreource"></a>NewReource


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|név|karakterlánc|igen |
|IsBudget|logikai érték|nem |
|IsGeneric|logikai érték|nem |
|IsInactive|logikai érték|nem |



### <a name="project"></a>Projekt


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|ApprovedStart|karakterlánc|nem |
|ApprovedEnd|karakterlánc|nem |
|CheckedOutDate|karakterlánc|nem |
|CheckOutDescription|karakterlánc|nem |
|CheckOutId|karakterlánc|nem |
|CreatedDate|karakterlánc|nem |
|Azonosító|karakterlánc|nem |
|IsCheckedOut|logikai érték|nem |
|LastPublishedDate|karakterlánc|nem |
|LastSavedDate|karakterlánc|nem |
|OptimizerDecision|egész szám|nem |
|PlannerDecision|egész szám|nem |
|ProjectType|egész szám|nem |
|név|karakterlánc|nem |
|WinprojVersion|karakterlánc|nem |



### <a name="projectswrapper"></a>ProjectsWrapper


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="newtask"></a>Új feladat


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Paraméterek|Nincs megadva|igen |



### <a name="taskparameters"></a>TaskParameters


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|név|karakterlánc|igen |
|Jegyzetek|karakterlánc|nem |
|Indítása|karakterlánc|nem |
|Időtartam|karakterlánc|nem |



### <a name="enterpriseresource"></a>EnterpriseResource


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|CanLevel|logikai érték|nem |
|Kód|karakterlánc|nem |
|CostAccrual|egész szám|nem |
|CostCenter|karakterlánc|nem |
|Létrehozva|karakterlánc|nem |
|DefaultBookingType|egész szám|nem |
|E-mailben|karakterlánc|nem |
|ExternalId|karakterlánc|nem |
|Csoport|karakterlánc|nem |
|HireDate|karakterlánc|nem |
|Azonosító|karakterlánc|nem |
|Monogram|karakterlánc|nem |
|IsActive|logikai érték|nem |
|IsBudget|logikai érték|nem |
|IsCheckedOut|logikai érték|nem |
|IsGeneric|logikai érték|nem |
|IsTeam|logikai érték|nem |
|MaterialLabel|karakterlánc|nem |
|Módosítva|karakterlánc|nem |
|név|karakterlánc|nem |
|Fonetika|karakterlánc|nem |
|Erőforrástípus|egész szám|nem |
|TerminationDate|karakterlánc|nem |



### <a name="taskswrapper"></a>TasksWrapper


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|érték|tömb|nem |



### <a name="task"></a>Tevékenység


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|Létrehozva|karakterlánc|nem |
|Módosítva|karakterlánc|nem |
|Indítása|karakterlánc|nem |
|Befejezés|karakterlánc|nem |
|név|karakterlánc|nem |
|Azonosító|karakterlánc|nem |
|Prioritás|egész szám|nem |
|KészültségiSzint|egész szám|nem |
|Jegyzetek|karakterlánc|nem |
|Névjegy|karakterlánc|nem |


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)