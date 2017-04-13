<properties
   pageTitle="Azure erőforrás-kezelő kérelem korlátai |} Microsoft Azure"
   description="Erőforrás-kezelő Azure kéréseivel szabályozásának előfizetési korlátok elérésekor használatát ismerteti."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Erőforrás-kezelő kérések szabályozása

Az egyes előfizetésekre és bérlői erőforrás-kezelő korlátai óránként 15 000 kérések és olvasási kérelmek óránként 1200. Ha az alkalmazás vagy parancsprogramot eléri ezek a korlátok, a kérések szabályozása szüksége. Ebben a témakörben megtudhatja, miként állapítható meg a többi kérések korlát elérése előtt, és amikor elérte a maximális olvashat.

Amikor elér a korlátot, kapja a HTTP-állapotkód **429-es jelű túl sok kérelem**.

Kérések számát az előfizetés vagy a bérlő megfelelően változik. Ha több, egyidejű kérelmek kérések tétele az előfizetését, adott alkalmazásokra kéréseit kerülnek együtt, annak megállapításához, hogy a hátralévő kérések száma.

Az átadás az előfizetés azonosítója, például az erőforrás beolvasása involve csoportok, az előfizetése hatóköre előfizetés kérések azok. Munkafüzetszintű bérlői kérések ne kerüljön bele az előfizetés azonosítója, például az érvényes Azure helyek lekérése.

## <a name="remaining-requests"></a>Hátralévő kérések

A hátralévő kérések száma megállapítható válasz fejlécek vizsgálata alapján. Minden egyes kérelem hátralévő olvasási és írási kérelem száma értékeket tartalmaz. Az alábbi táblázat ismerteti a válasz fejlécek, ezek az értékek ellenőrizheti:

| Válasz élőfej | Leírás |
| --------------- | ----------- |
| x-MS-ratelimit-remaining-Subscription-reads | Előfizetés hatóköre beolvassa a hátralévő |
| x-MS-ratelimit-remaining-Subscription-writes | Előfizetés hatóköre ír hátralévő |
| x-MS-ratelimit-remaining-tenant-reads | Munkafüzetszintű bérlői beolvassa a hátralévő |
| x-MS-ratelimit-remaining-tenant-writes | Munkafüzetszintű bérlői ír hátralévő |
| x-MS-ratelimit-remaining-Subscription-Resource-Requests | Előfizetés az erőforrás típusa kérések hátralévő változik.<br /><br />Ez az élőfej csak a visszaadott érték, ha a szolgáltatás még felül az alapértelmezett korlát. Erőforrás-kezelő hozzáadja ezt az értéket, az előfizetés olvasása és írása helyett. |
| x-MS-ratelimit-remaining-Subscription-Resource-entities-Read | Előfizetés az erőforrás típusa webhelycsoport kérések hátralévő változik.<br /><br />Ez az élőfej csak a visszaadott érték, ha a szolgáltatás még felül az alapértelmezett korlát. Ezt az értéket számos hátralévő webhelycsoport kérések (lista erőforrások). |
| x-MS-ratelimit-remaining-tenant-Resource-Requests | Bérlői hatóköre hátralévő erőforrás típusa kérelmekhez.<br /><br />Ezt a fejlécet csak létre iránti kérelmek bérlői szinten, és csak akkor, ha a szolgáltatás még felül az alapértelmezett korlát. Erőforrás-kezelő hozzáadja ezt az értéket az bérlői olvasása és írása helyett. |
| x-MS-ratelimit-remaining-tenant-Resource-entities-Read | Bérlői erőforrás típusa webhelycsoport kérések hátralévő változik.<br /><br />Ezt a fejlécet csak létre iránti kérelmek bérlői szinten, és csak akkor, ha a szolgáltatás még felül az alapértelmezett korlát. |

## <a name="retrieving-the-header-values"></a>Az élőfej értékek lekérdezése

Ezek a kódot vagy parancsprogramot élőfej az értékek lekérdezése jelentkezés nem különbözik minden élőfej érték beolvasása. 

Például a **C#**, beolvassa élőfej **HttpWebResponse** objektum nevű **Válasz** a következő kódot:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

A **PowerShell**a fejléc értéke Invoke-WebRequest műveletből beolvasásához.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Vagy, ha szeretné, hogy a hátralévő kérelmeket, a hibakereséshez, akkor megadhatja a **-hibakeresési** a **PowerShell** -parancsmag a paramétert.

    Get-AzureRmResourceGroup -Debug
    
Amely a sok olyan fontos információt, többek között az alábbi válasz értéket adja eredményül:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

Az **Azure CLI**a fejléc értéke részletesebb szolgáltatással beolvasásához.

    azure group list -vv --json

Amely a sok olyan fontos információt, például a következő objektum adja eredményül:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Várakozás a következő kérelem elküldése előtt

Amikor elér a kérelem korlátot, az erőforrás-kezelő a **429** HTTP állapotkódot adja vissza., és egy **Újrapróbálkozási után** érték a fejlécen. Az **Ismét után** érték másodperc az alkalmazás kell várnia (vagy meddig alhatnak) számát adja meg a következő kérelem elküldése előtt. Ha ismét értékét letelte előtt kérelem küld, a kérelem feldolgozása nem, és egy új újrapróbálkozási értéket ad vissza.
