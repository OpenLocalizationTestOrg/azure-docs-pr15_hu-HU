<properties
pageTitle="GitHub |} Microsoft Azure"
description="Logika alkalmazások Azure alkalmazás szolgáltatás hozzon létre. GitHub tárháza webes mely számjegy szolgáltatója. Összes elosztott felülvizsgálata hozzáférés és a forrás kód management (SCM) funkció mely számjegy, valamint a saját szolgáltatások hozzáadására is kínál."
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

# <a name="get-started-with-the-github-connector"></a>Első lépések az GitHub összekötő

GitHub tárháza webes mely számjegy szolgáltatója. Összes elosztott verzió hozzáférés és a forrás kód management (SCM) funkció mely számjegy, valamint a saját szolgáltatások hozzáadására is kínál.

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások Skype 2015-08-01-séma verziót vonatkozik. 

Most egy összefüggés-alkalmazás létrehozásával miként vehetik használatba, [egy összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakör tartalmaz.

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek

Az GitHub összekötő is használható művelet; trigger(s) rendelkezik. Összekötők JSON és az XML formátumú támogatja az adatokat. 

 Az alábbi műveleteket és/vagy a rendelkezésre álló trigger(s) az GitHub összekötő foglalja magában:

### <a name="github-actions"></a>GitHub műveletek
Hajthatók végre az alábbi műveleteket:

|Művelet|Leírás|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Létrehoz egy probléma|
### <a name="github-triggers"></a>Eseményindítók GitHub
Az alábbi esemény hallgathatja meg:

|Eseményindító | Leírás|
|--- | ---|
|Probléma megnyitásakor|Probléma van megnyitva|
|Ha a probléma van nyitva|Probléma van nyitva.|
|Ha a probléma van-e hozzárendelve|Probléma van rendelve.|


## <a name="create-a-connection-to-github"></a>GitHub kapcsolat létrehozása
Logika alkalmazások létrehozását GitHub, először kell hozzon létre egy **kapcsolatot** , majd a szükséges az adatait a következő tulajdonságokat: 

|A tulajdonság| Szükséges|Leírás|
| ---|---|---|
|Jogkivonat|igen|GitHub hitelesítő adatok megadása|
Miután létrehozta a kapcsolatot, vele hajtsa végre a műveleteket, és figyelje a jelen cikkben ismertetett eseményindítók. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] A kapcsolat az egyéb összefüggés-alkalmazásokban is használhatja.

## <a name="reference-for-github"></a>Összefoglalás: a GitHub
Verziójára vonatkozik: 1.0

## <a name="createissue"></a>CreateIssue
Hozzon létre a problémát: probléma hoz létre 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| név| Adattípus|Szükséges|Olvassa el a|Alapérték|Leírás|
| ---|---|---|---|---|---|
|repositoryOwner|karakterlánc|igen|elérési út|nincs lehetőség|Tulajdonos a tárházba|
|repositoryName|karakterlánc|igen|elérési út|nincs lehetőség|Tárházba neve|
|issueBasicDetails| |igen|szervezet|nincs lehetőség|A probléma részletei|

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


## <a name="issueopened"></a>IssueOpened
Amikor nyitja meg a problémát: probléma megnyitásakor 

```GET: /trigger/issueOpened``` 

A hívás nincsenek paraméterei
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


## <a name="issueclosed"></a>IssueClosed
Ha nincs megnyitva a problémát: probléma van nyitva. 

```GET: /trigger/issueClosed``` 

A hívás nincsenek paraméterei
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


## <a name="issueassigned"></a>IssueAssigned
Ha meg van-e hozzárendelve problémát: probléma van rendelve. 

```GET: /trigger/issueAssigned``` 

A hívás nincsenek paraméterei
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|cím|karakterlánc|igen |
|szervezet|karakterlánc|igen |
|megbízottja|karakterlánc|igen |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Tulajdonság neve | Adattípus | Szükséges |
|---|---|---|
|cím|karakterlánc|igen |
|szervezet|karakterlánc|igen |
|megbízottja|karakterlánc|igen |
|szám|karakterlánc|nem |
|állam|karakterlánc|nem |
|created_at|karakterlánc|nem |
|repository_url|karakterlánc|nem |


## <a name="next-steps"></a>Következő lépések
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)