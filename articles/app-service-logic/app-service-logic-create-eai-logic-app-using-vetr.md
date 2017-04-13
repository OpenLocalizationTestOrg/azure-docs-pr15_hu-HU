<properties
   pageTitle="Azure App szolgáltatásban logika alkalmazások VETR használata EAI összefüggés-alkalmazás létrehozása |} Microsoft Azure"
   description="Érvényesítése, kódolását és átalakítás BizTalk XML services lehetőségei"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>VETR használatával EAI összefüggés-alkalmazás létrehozása

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Vállalati alkalmazás integrációs (EAI) találatokkal résidőkiosztással adatokat a forrás- és egy között. Az ilyen esetek gyakran közös rendelkeznek követelmények:

- Győződjön meg arról, hogy más rendszerekből származó adatok hibásan vannak formázva.
- Bejövő adatok döntések "keresési" végezni.
- Adatok konvertálása az egyik formátumról a másikra. Például konvertálása adatok egy CRM rendszerrel adatformátum egy ERP rendszer adatformátum.
- A kívánt alkalmazásba vagy a rendszer átirányítja az adatokat.

Ebből a cikkből megtudhatja, hogy közös integrációs mintát: "egyirányú üzenet közvetítés" vagy a VETR (érvényesítése Enrich, átalakítás, útvonal). A VETR minta közvetítő adatok a forrás személy és a cél entitás között. A forrás- és általában adatforrásokhoz.

Fontolja meg olyan webhely esetében, megrendelések. Rendelések listát a rendszer HTTP használatával a felhasználók tartalmakat. Bepillantás a színfalak mögé a rendszer ellenőrzi a bejövő helyességét, normalizálja és továbbra is fennáll a szolgáltatás Bus várakozási sorban található további feldolgozásra. A rendszer megnyitja a rendelések kikapcsolása a várólista meglepő, hogy egy bizonyos formátumban. Így a végpontok közötti folyamat a következő:

**HTTP** → **érvényesítése** → **átalakítása** → **szolgáltatás Bus**

![Egyszerű VETR továbbításához][1]

A következő BizTalk API súgó összeállítása a minta:

* **Eseményindító HTTP** - forrás eseményindító üzenet eseményre
* **Érvényesítés** - ellenőrzi a bejövő adatok helyességét
* **Átalakítás** - átalakítások adatainak bejövő formátumúnak kell által feldolgozott rendszer szükséges
* **Szolgáltatás Bus Connector** - cél entitás, ahol adatokat küldi


## <a name="constructing-the-basic-vetr-pattern"></a>Az egyszerű VETR mintát megépítése.
### <a name="the-basics"></a>Az alapvető műveletei

Az Azure-portálon válassza az **+ Új**, jelölje be a **webes + Mobile**, és válassza az **Alkalmazás összefüggés**. Válassza a neve, hely, előfizetés, erőforráscsoport és helyét, hogy. Erőforrás-csoportokat használni kívánt tárolók az alkalmazásai; az azonos erőforráscsoport az erőforrásokat, az alkalmazás megnyitásához.

Ezután eseményindítók és a műveletek hozzáadása.


## <a name="add-http-trigger"></a>HTTP eseményindító hozzáadása
1. **Logika alkalmazássablonok**válassza a **nulláról létrehozása**lehetőséget.
1. Válassza ki a **HTTP-figyelő** hozhat létre egy új figyelő a gyűjteményből. **HTTP1**hívja meg.
2. Állítsa a **Automatikus válaszok küldése?** hamis értékűre állítja. Állítsa be a _HTTP-metódus_ _bejegyzésekbe_ és beállításoknál _Relatív URL-cím_ _/OneWayPipeline_az eseményindító beavatkozását:  
    ![HTTP eseményindító][2]
3. Jelölje ki a zöld pipa jelzi az eseményindító befejezéséhez.

## <a name="add-validate-action"></a>Adja hozzá az érvényesítés művelet

Most vegyük adja meg a műveleteket, amelyeket elindul, akkor indul el, az eseményindító – Ez azt jelenti, hogy jelenik meg, ha a HTTP-végpont hívás fogadásakor.

1. **XML-érvényesítő BizTalk** hozzáadása a gyűjteményből, és a nevet _(Validate1)_ egy példányának létrehozására.
2. Állítsa be az XML-bejövő üzenetek érvényesítéséhez XSD-séma. Jelölje ki az _érvényesítés_ műveletet, és válassza a _triggers('httplistener').outputs. Tartalom_ _inputXml_ paraméter értéke.

Most az érvényesítés művelet válik az első művelet után a HTTP-figyelő: 

![BizTalk XML érvényesítő][3]

Hasonlóképpen hozzáadása a műveleteket a többi. 

## <a name="add-transform-action"></a>Átalakítás művelet hozzáadása
Vegyük állítsa be a bejövő adatok normalizálandó átalakítások.

1. **BizTalk átalakítása szolgáltatás** hozzáadása a gyűjteményből.
2. Az XML-bejövő üzenetek átalakításához átalakító konfigurálásához jelölje ki a **átalakítása** műveletet, a művelet elvégzéséhez, ez az API hívásakor. Válassza a ```triggers(‘httplistener’).outputs.Content``` _inputXml_dátumaként. *Térkép* nem kötelező paraméter, mivel a bejövő adatok megegyezik az összes konfigurált átalakítások, és csak azokat, amelyek megegyeznek a séma alkalmazza.
3. Végül pedig az átalakítás fut, csak akkor, ha sikerül ellenőrzése. Ez a feltétel beállításához jelölje be a jobb felső sarokban lévő fogaskerék ikonra, és válassza a _Hozzáadás a feltételnek kell teljesülnie_. Állítsa a feltétel ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalk átalakítások][4]


## <a name="add-service-bus-connector"></a>Szolgáltatás Bus összekötő hozzáadása
Ezután a cél hozzáadása – szolgáltatás Bus várólista – az adatok írásához.

1. **Szolgáltatás Bus összekötő** hozzáadása a gyűjteményből. _Servicebus1_meg a **nevét** , **Kapcsolati karakterlánc** állítsuk be a szolgáltatás bus példány a kapcsolati karakterláncot, _várólista_meg a **Személy nevét** , és ugorja át az **Előfizetés neve**.
2. Jelölje ki az **Üzenet küldése** műveletet, és jelölje be a **tartalom** mezőben annak a műveletnek _actions('transformservice').outputs. OutputXml_.
3. A **Webhelytartalom-típus** mező beállítása az XML- *alkalmazás /*:  

![Szolgáltatás Bus][5]


## <a name="send-http-response"></a>HTTP-válasz küldése
Miután befejeződött a folyamat feldolgozása, visszajelzés vissza egy HTTP sikeres és a hiba az alábbi lépéseket:

1. Adja hozzá a **HTTP-figyelő** a gyűjteményből, és válassza ki a **HTTP-válasz küldése** .
2. Állítsa be az **azonosító válasz** *küldéséhez*.
2. Állítsa **Válasz tartalom** *folyamat feldolgozása, Befejezve*.
3. **Válasz állapotkódja** *200* , hogy HTTP 200 az OK gombra.
4. Jelölje ki a legördülő lista jobb felső sarokban lévő, és válassza a **Hozzáadás egy feltételnek teljesülnie kell**.  A feltétel megadása a következő kifejezést:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Ismételje meg ezeket a lépéseket, hogy HTTP választ hiba esetén is. **Feltétel** módosítása a következő kifejezést:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Válassza **az OK gombra** , majd **Hozzon létre**.



## <a name="completion"></a>Befejezés
Minden alkalommal, amikor valaki üzenetet küld a HTTP-végpontot, elindítja az alkalmazást, és az imént létrehozott műveletek végrehajtása. Összes ilyen logika alkalmazás létrehozása, az Azure-portálon lépjen a **Tallózás gombra** , és kattintson a **Logika alkalmazások**kezeléséhez. További információ az alkalmazás kijelölése

Hasznos témakörökben:

[Kezelése és az API-alkalmazások és összekötők figyelése](app-service-logic-monitor-your-connectors.md)  <br/>
[A logikai alkalmazások figyelése](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
