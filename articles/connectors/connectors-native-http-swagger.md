
<properties
    pageTitle="Adja hozzá a HTTP + Swagger művelet az összefüggés-alkalmazások |} Microsoft Azure"
    description="A HTTP + Swagger áttekintése művelet és műveletek"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http--swagger-action"></a>Első lépések a HTTP + Swagger művelet

A HTTP + Swagger művelet hozhat létre bármely többi végpontra keresztül egy [dokumentum Swagger](https://swagger.io)osztályú összekötő. Is bővítheti a logika alkalmazás bármely többi végpont olyan osztályú logika alkalmazás Designer megoldást hívni.

Az első lépések a HTTP + Swagger művelet logika alkalmazásban, látni [logika új alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Használja a HTTP + Swagger eseményindító vagy művelet

A HTTP + Swagger eseményindító és a művelet ugyanúgy működnek, mint a [HTTP-művelet](connectors-native-http.md) , de megjelenítésével az API-val és a kimeneti értékeket az alakzatot a tervezőben a [metaadat-alapú Swagger](https://swagger.io)adja meg a egy jobb tervezési hatékonyságát. Emellett használhatja a HTTP + kiindulópontként Swagger. A lekérdezési eseményindító végrehajtásához, meg kell partnerlistáját ismertetett [létrehozása egy egyéni API összefüggés-alkalmazások használata](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers)a lekérdezési mintát.

[További tudnivalók a logika alkalmazás eseményindítók és a műveletek.](connectors-overview.md)

Az alábbiakban használata a HTTP + Swagger műveletben egy munkafolyamatot egy logikai alkalmazásban művelet példa.

1. Az **Új lépést** gombra.
2. Válassza a **Hozzáadás művelet**.
3. A művelet Keresés mezőbe írja be a listában a HTTP + Swagger művelet **swagger** .

    ![Jelölje ki a HTTP + Swagger művelet](./media/connectors-native-http-swagger/using-action-1.png)

4. Írja be a Swagger dokumentum URL-címe:
    - A logikai alkalmazás tervező használatához az URL-cím HTTPS-végpont kell lennie, és CORS engedélyezve van.
    - Ha a Swagger dokumentum nem felel meg az e követelmény, [CORS engedélyezve van az Azure tároló](#hosting-swagger-from-storage) tárolja a dokumentumot is használhatja.
5. Kattintson a **következő** olvashatók, és a Swagger dokumentumból jeleníti meg.
6. Vegye fel a HTTP-híváshoz szükséges paramétereket.

    ![HTTP-művelet befejezése](./media/connectors-native-http-swagger/using-action-2.png)

1. Az eszköztáron, és a logika alkalmazás menti mindkét bal felső sarkában kattintson a **Mentés** és közzététel (aktiválása).

### <a name="host-swagger-from-azure-storage"></a>A Host Swagger Azure tárhelyről

Érdemes lehet Swagger dokumentum, amely nem üzemelteti, illetve, amely nem felel meg a biztonság és a Tervező határokon származási követelményei hivatkozni szeretne. A probléma megoldásához a Swagger dokumentumot tárolja az Azure-tárolóban lévő és engedélyezése CORS a dokumentum hivatkozni szeretne.  

Hozzon létre, beállítása és Azure-tárolóban lévő Swagger a dokumentumok tárolása a lépések a következők:

1. [Az Azure Blob-tárolóhoz Azure tároló fiók létrehozása](../storage/storage-create-storage-account.md). (Ehhez engedélyeket **nyilvános**elérés.)
2. A blob CORS engedélyezése [A PowerShell-parancsprogramot](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) is használhatja, hogy a beállítás automatikusan konfigurálja.
3. Töltse fel a Swagger fájlt a blob-be. Ehhez az [Azure portál](https://portal.azure.com) vagy az [Azure tároló Explorer](http://storageexplorer.com/)például egy eszközt.
1. Egy HTTPS-hivatkozás a Azure Blob-tárolóban lévő dokumentumra mutató hivatkozást. (A hivatkozás követi formátuma `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Műszaki információk

Az alábbiakban a eseményindítók és a műveletek részleteit, amely a HTTP + Swagger összekötő támogatja.

## <a name="http--swagger-triggers"></a>HTTP + Swagger indítók

Az eseményindító az eseményre kattintva elindíthatja a munkafolyamatot egy logikai alkalmazásban definiált használható. [További tudnivalók a indítók.](connectors-overview.md) A HTTP + Swagger összekötő van egy eseményindító.

|Eseményindító|Leírás|
|---|---|
|HTTP + Swagger|HTTP kezdeményezése és visszatérés a válasz tartalom|

## <a name="http--swagger-actions"></a>HTTP + Swagger műveletek

Művelet egy műveletet, amely a munkafolyamat egy logikai alkalmazásban definiált végzi el. [További tudnivalók a műveletek.](connectors-overview.md) A HTTP + Swagger összekötő van egy lehetséges műveletet.

|Művelet|Leírás|
|---|---|
|HTTP + Swagger|HTTP kezdeményezése és visszatérés a válasz tartalom|

### <a name="action-details"></a>Művelet részletei

A HTTP + Swagger összekötő megtalálható egy lehetséges műveletet. Az alábbiakban a műveleteket, a kötelező és választható beviteli mezők és a megfelelő kimeneti részletek, a használatuk társított információkra.

#### <a name="http--swagger"></a>HTTP + Swagger

Ellenőrizze a HTTP-kimenő kérelem Swagger metaadatok segítséget.
Csillag (*) azt jelzi, hogy a kötelező megadni.

|Megjelenítendő név|Tulajdonság neve|Leírás|
|---|---|---|
|A módszer *|a módszer|HTTP-utasítás használata.|
|URI *|URI|A HTTP-kérés URI.|
|Élőfejek|Élőfejek|A HTTP-fejlécek, amelyet fel szeretne venni egy JSON objektumot.|
|Szervezet|szervezet|HTTP összehívás törzsébe.|
|Hitelesítés|hitelesítés|Hitelesítés kérelem használható. [További részletekért olvassa el a HTTP](./connectors-native-http.md#authentication)-e.|

**Kimeneti részletei**

HTTP-válasz

|Tulajdonság neve|Adattípus|Leírás|
|---|---|---|
|Élőfejek|objektum|Válasz fejlécek|
|Szervezet|objektum|Válasz objektum|
|Állapotkód|int|HTTP-állapotkód|

### <a name="http-responses"></a>HTTP-válaszok

Hívás a különféle műveletek, ha adott válaszok jelenhet meg. Az alábbiakban egy olyan táblát, ismertet a megfelelő választ, és leírását.

|név|Leírás|
|---|---|
|200|oké|
|202|Elfogadott|
|400|Hibás kérés|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|404|Nem található|
|500|Belső kiszolgálóhiba. Ismeretlen hiba történt.|

---

## <a name="next-steps"></a>Következő lépések

Próbálja ki az platform- és [összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md) most. Megismerheti az egyéb elérhető összekötők logika alkalmazások [API-khoz listáját](apis-list.md)megjeleníti.
