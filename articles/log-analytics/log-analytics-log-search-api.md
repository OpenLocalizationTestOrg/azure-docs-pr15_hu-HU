<properties
    pageTitle="Log Analytics jelentkezzen be a keresési REST API |} Microsoft Azure"
    description="Ez az útmutató nyújt segítséget egyszerű azt mutatja be, hogyan használhatja a napló Analytics keresési REST API-t a tevékenységek kezelése csomagja (MOBILE), és azt példákat mutat be, amely megmutatja, hogyan használható a parancsokat."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Log Analytics napló keresési REST API-val

Ez az útmutató nyújt segítséget egyszerű azt mutatja be, hogyan használhatja a napló Analytics keresési REST API-t a tevékenységek kezelése csomagja (MOBILE), és azt példákat mutat be, amely megmutatja, hogyan használható a parancsokat. Ez a cikk példáiban részét hivatkozás műveleti hírcsatornájában, melyiket érdemes napló Analytics előző verziójának nevét.

## <a name="overview-of-the-log-search-rest-api"></a>A napló keresési REST API – áttekintés

A napló Analytics keresési REST API RESTful és azok webböngészőn keresztül az Azure erőforrás-kezelő API-t. A dokumentum találja példák hol az API-t a [ARMClient](https://github.com/projectkudu/ARMClient), a Megnyitás parancssori eszköz, amely megkönnyíti az Azure erőforrás-kezelő API meghívása keresztül érhető el. A ARMClient és a PowerShell használata a napló Analytics keresési API eléréséhez sok lehetőségek közül. Egy másik, hogy az Azure PowerShell-modult tartalmazó eléréséhez a keresési parancsmagok OperationalInsights használata. Ezeket az eszközöket is használhatja a RESTful Azure erőforrás-kezelő API segítségével MOBILE munkaterületek kezdeményezhetnek, és végezze el a keresés parancsainak őket. Az API kimeneti fog találatokat, hogy Ön JSON formátumban, lehetővé téve a keresési eredmények programozás útján számos különböző módon használatát.

Az Azure erőforrás-kezelő keresztül a [tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn910477.aspx) , valamint a [REST API -t](https://msdn.microsoft.com/library/azure/mt163658.aspx)is használható. Tekintse át a hivatkozott weblapok további információt.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Egyszerű napló Analytics keresési REST API-oktatóanyag

### <a name="to-use-the-arm-client"></a>Az ARM ügyfél használatához

1. Telepítés [Chocolatey](https://chocolatey.org/), amely egy megnyitott csomag Forráskezelő Windows. Nyisson meg egy parancssorablakot rendszergazdaként, és futtassa az alábbi parancsot:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Telepítse a ARMClient a következő parancs futtatásával:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Egyszerű keresési használata a ARMClient végrehajtásához

1. Bejelentkezés a Microsoft vagy fejlesztés fiókjához:

    ```
    armclient login
    ```

    A sikeres jelentkezzen be az adott fiókhoz tartozik minden előfizetés sorolja fel:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. A tevékenységek kezelése csomagja munkaterületek kap:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Sikeres Get-hívást szeretne kimeneti összes munkaterületek az előfizetéshez tartozik:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. A keresés változó létrehozása:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Keresés az új keresési változó használata:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Jelentkezzen be a Analytics keresési REST API-hivatkozás példák
Az alábbi példák bemutatják, hogy miként használhatja a keresési API.

### <a name="search---actionread"></a>Keresés – művelet/olvasott

**Minta URL-címe:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**A kérelem:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Az alábbi táblázat ismerteti a rendelkezésre álló tulajdonságait.

|**A tulajdonság**|**Leírás**|
|---|---|
|felső|Eredmény maximális száma.|
|Kiemelés|Egyező mezőket kiemelés használt általában előtti és utáni paraméterek tartalmazza.|
|formátumba|A megfeleltetett mezők a megadott karakterláncot előtagok.|
|bejegyzés|Az adott karakterlánc hozzáfűzi az egyező mezőket.|
|lekérdezés|A keresési lekérdezést használva gyűjtötte és a találatokat is visszaadjon.|
|indítása|A kívánt eredmény érhető el időkeret elejére.|
|vége|A kívánt eredmény érhető el időkeret végére.|


**Válasz:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Keresés / {ID} - művelet/olvasott

**A kérelem mentett keresés tartalma:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Ha a Keresés eredménye "Függőben" állapotú, majd a frissített eredményeit lekérdezési történik – ez az API. 6 perc, után a Keresés eredménye a program eltávolítja a gyorsítótárból, és visszatér az HTTP megszűnt. A kezdeti keresési kérelem közvetlenül a "Sikeres" állapot ad vissza, ha ez nem hozzáadódik a gyorsítótár okoz a API-val való visszatéréshez HTTP megszűnt, ha a lekérdezés. A kezdeti keresési kérelem csak frissített értékű megegyező formátumban egy HTTP 200 eredmény tartalmának tekinthetők meg.

### <a name="saved-searches---rest-only"></a>Mentett keresés – csak a többi

**A kérelem mentett keresés listája:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Támogatott módszerek: GET ELHELYEZNI törlése

A webhelycsoport módszerek támogatott: GET

Az alábbi táblázat ismerteti a rendelkezésre álló tulajdonságait.

|A tulajdonság|Leírás|
|---|---|
|Azonosító|Az egyedi azonosító.|
|ETag|A **javítás szükséges**. Minden egyes írás kiszolgáló frissíti. Értékét meg kell egyeznie a jelenlegi tárolt érték vagy "*" frissítéséhez. 409 a régi/érvénytelen értéket adja vissza.|
|Properties.Query|A **szükséges**. A keresési lekérdezést.|
|properties.displayName|A **szükséges**. A lekérdezés felhasználói definiált megjelenítendő neve. A modellezni Azure erőforrásként, ez akkor címke.|
|Properties.Category|A **szükséges**. A felhasználó által definiált a lekérdezés kategóriát. Azure erőforrásként modellezni Ez akkor címke.|

>[AZURE.NOTE] A napló Analytics keresési API jelenleg adja eredményül a felhasználó által létrehozott mentett kereséseket, amikor mentett keresés munkaterületen kérdezte le. Az API nem ad vissza az adott időben megoldások által biztosított mentett keresés. Ezt a funkciót a későbbi időpontban kerül.

### <a name="create-saved-searches"></a>Mentett keresés létrehozása

**A kérelem:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Törlés mentett keresések

**A kérelem:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Frissítés mentett keresések

 **A kérelem:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metaadat - JSON csak

Az alábbiakban látható, a munkaterületen gyűjtött adatok diagramtípusokat napló tartozó mezőket. Például ha azt szeretné tudni, hogy ha az esemény típusa van-e a számítógép nevű mezőt, majd az egyik módja keresheti meg, és erősítse meg.

**Kérés mezők:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Válasz:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Az alábbi táblázat ismerteti a rendelkezésre álló tulajdonságait.

|**A tulajdonság**|**Leírás**|
|---|---|
|név|A mező neve.|
|displayName|A megjelenítendő név mező.|
|típus|A mező értékét a típusát.|
|facetable|Kombinációját aktuális "indexelt", "tárolt" és "dimenzió" tulajdonságait.|
|megjelenítése|Aktuális megjelenítéséhez' tulajdonság. IGAZ, ha látható a Keresés mező.|
|ownerType|Csak a csoportjába tartozó onboarded IP-típusok csökken.|


## <a name="optional-parameters"></a>Választható paraméterek
Az alábbiakból megtudhatja választható lehetőségek állnak rendelkezésre.

### <a name="highlighting"></a>Kiemelés

A "Kiemelés" paraméter választható paraméter kérése a keresési alrendszer használhat egy sor olyan jelölőkkel szerepeltetni a választ.

Ezek a jelölők kezdetének jelzésére, és a Befejezés kiemelt szöveget, amely megfelel a keresési lekérdezés megadott adatokra.
A kezdő és záró jelölők úgy veheti körül a a kijelölt kifejezés keresés által használt is megadhat.

**Példa keresési lekérdezés**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Példa eredménye:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Figyelje meg, hogy a eredménye a fenti hibaüzenet jelenik meg, előtaggal és fűzött tartalmazza.

## <a name="computer-groups"></a>Számítógép-csoportok

Számítógép-csoportok speciális mentett keresés számítógépek visszaadó.  Más lekérdezések számítógép csoport segítségével a számítógépre, csoportjában az eredmények korlátozásához.  Számítógép csoport áll rendelkezésre, mint a számítógép érték csoport címkével ellátott mentett keresés.

Az alábbiakban számítógép csoportjának minta választ.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Számítógép-csoportok beolvasása

A Get-metódus használata a Csoportazonosítót beolvasni a számítógép-csoportot.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Létrehozásakor vagy frissítésekor a számítógép csoportja

Módszert a helyezése egyedi keresési azonosító mentett olyan új számítógép csoportot szeretne létrehozni. Ha egy meglévő számítógép Csoportazonosítót, hogy az egyik lehet módosítani. A MOBILE konzolban számítógép csoport létrehozásakor, akkor az azonosító a csoportot, és a név jön létre.

A csoport definíció használt lekérdezés vissza kell térnie a számítógépek, a csoport csak akkor működik megfelelően.  Azt ajánljuk, hogy a *a lekérdezést befejezése |} Eltérők számítógép* annak érdekében, hogy a megfelelő adatokat ad vissza.

A mentett keresés a definíció címke nevű csoport a keresés számítógép sorolhatók, mint egy számítógép csoport érték kell tartalmaznia.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Számítógép-csoportok törlése

A Törlés mód használata a Csoportazonosítót számítógép csoport törlése.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Következő lépések

- Egyéni mezők feltétel lekérdezések összeállításához megismerheti a [napló keresések](log-analytics-log-searches.md) .
