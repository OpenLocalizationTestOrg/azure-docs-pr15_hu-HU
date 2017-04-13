<properties
pageTitle="SMTP |} Microsoft Azure"
description="Azure alkalmazás szolgáltatás hozzon létre logikájának alkalmazások. Csatlakozás SMTP küldhet e-mailt."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Első lépések az SMTP-összekötő

Csatlakozás SMTP küldhet e-mailt.

Szeretne használni [kívánt összekötőre](./apis-list.md), először összefüggés-alkalmazás létrehozása céljából. [Most egy összefüggés-alkalmazás](../app-service-logic/app-service-logic-create-a-logic-app.md)létrehozásával használatának megkezdéséhez.

## <a name="connect-to-smtp"></a>SMTP csatlakoztatása

Mielőtt a logika alkalmazás elérhető valamelyik szolgáltatás először létre kell hoznia egy *kapcsolatot* a szolgáltatás. [Kapcsolat](./connectors-overview.md) a összefüggés-at, és egy másik szolgáltatás közötti kapcsolatot biztosít. Például hogy csatlakoztatni SMTP, először az SMTP- *kapcsolatot*. Kapcsolat létrehozása kellene általában a szolgáltatást, amelyhez csatlakozni szeretne eléréséhez használt hitelesítő adatait. Igen az SMTP példában kellene a hitelesítő adatait, és a kapcsolat neve, az SMTP-kiszolgáló címét, és a felhasználói bejelentkezési adatok annak érdekében, hogy az SMTP-kapcsolat létrehozása. [Többet szeretne tudni a kapcsolatokkal]()  

### <a name="create-a-connection-to-smtp"></a>SMTP-kapcsolat létrehozása

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>SMTP eseménykód használata

Az eseményindító az eseményre kattintva elindíthatja a munkafolyamatot egy logikai alkalmazásban definiált használható. [További tudnivalók a indítók](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Ebben a példában mivel SMTP nem saját, az eseményindító használjuk a **Salesforce - objektum létrehozásakor** eseményindító. Ez az eseményindító aktiválja a Salesforce egy új objektum létrehozásakor. Példa azt kell állítania azt, hogy az minden alkalommal, amikor egy új érdeklődő Salesforce jön létre, egy *e-mailek küldése* művelet beállítása, az SMTP-összekötő az értesítés az éppen létrehozott új érdeklődő keresztül.

1. *Salesforce* a logikájának alkalmazások Tervező a Keresés mezőbe írja be, majd válassza ki a **Salesforce - objektum létrehozásakor** eseményindító.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. Az **objektum létrehozásakor** vezérlőelem jelenik meg.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Válassza ki az **Objektum típusa** , és válassza a *levezetni* objektumok listáját. Ebben a lépésben vannak jelezve, hogy hoz létre, amely a logika alkalmazás értesítést, valahányszor új érdeklődő Salesforce jön létre az eseményindító.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. Az eseményindító létrehozva.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>SMTP művelet használata

Művelet egy olyan művelet, a munkafolyamat egy logikai alkalmazásban definiált által végzett. [További tudnivalók a műveletek](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Most, hogy az eseményindító kerül, akkor fordul elő, amikor egy új érdeklődő jön létre a Salesforce SMTP művelet hozzáadása az alábbi lépésekkel.

1. Válassza az **+ új lépést** hozzáadni a műveletet szeretne venni egy új érdeklődő létrehozásakor.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Válassza a **Hozzáadás művelet**. A megnyílik a Keresés mezőbe, ahol kereshet minden olyan művelet, szeretné venni.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Adja meg az *smtp* -szeretne keresni, SMTP kapcsolatos műveletek.  

4. Jelölje be a végrehajtandó műveletet, amikor létrejön az új érdeklődő **SMTP - Küldés e-mailben** . A művelet vezérlő letiltása nyílik meg. A Lekérdezéstervező blokk az smtp-kapcsolatot létesíteni, ha, még nem tette korábban akkor.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Adja meg a kívánt levelezési adatok, az **SMTP - Küldés e-mailek** számát.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Mentse a munkáját a munkafolyamat aktiválásához.  

## <a name="technical-details"></a>Műszaki információk

Az alábbiakban a eseményindítók, műveletek és válaszokat, amely támogatja a kapcsolat adatait:

## <a name="smtp-triggers"></a>SMTP indítók

SMTP nem tartozik eseményindító. 

## <a name="smtp-actions"></a>SMTP-műveletek

SMTP rendelkezik az alábbi műveletet:


|Művelet|Leírás|
|--- | ---|
|[E-mail küldése](connectors-create-api-smtp.md#send-email)|Ez a művelet e-mailt küld egy vagy több címzettnek.|

### <a name="action-details"></a>Művelet részletei

Az alábbiakban a részletek, a művelet a összekötő annak válaszokkal együtt:


### <a name="send-email"></a>E-mail küldése
Ez a művelet e-mailt küld egy vagy több címzettnek. 


|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|A|A|Adja meg az e-mail címét, például egymástól pontosvesszővel elválasztvarecipient1@domain.com;recipient2@domain.com|
|MÁSOLATOT KAP|másolatot kap|Adja meg az e-mail címét, például egymástól pontosvesszővel elválasztvarecipient1@domain.com;recipient2@domain.com|
|Tárgy|Tárgy|E-mailek tárgy|
|Szervezet|Szervezet|E-mail törzsében|
|A|A|E-mail cím, például a feladósender@domain.com|
|IsHtml|A Html|Küldje el az e (IGAZ vagy hamis) HTML formátumban|
|A titkos másolat|a titkos másolat|Adja meg az e-mail címét, például egymástól pontosvesszővel elválasztvarecipient1@domain.com;recipient2@domain.com|
|Sürgős|Sürgős|Az e-mailt (a magas, a normál vagy a alacsony) fontosságát|
|ContentData|Mellékletek tartalom adatok|Tartalomtípus-adatok (base64 kódolva, mint adatfolyamok-karakterlánc van)|
|ContentType|Mellékletek webhelytartalom-típus|Webhelytartalom-típus|
|ContentTransferEncoding|Mellékletek tartalom átviteli kódolás|Tartalom átadása kódolást (base64 vagy a Nincs értéket)|
|Fájlnév|Mellékletek fájl neve|Fájlnév|
|ContentId|Mellékletek tartalom azonosítója|Tartalom azonosítója|

Egy * jelzi, hogy szükség-e a tulajdonság


## <a name="http-responses"></a>HTTP-válaszok

A műveletek és a fenti indítók végre az alábbi HTTP állapot kódokat a térhet vissza: 

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
[Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)