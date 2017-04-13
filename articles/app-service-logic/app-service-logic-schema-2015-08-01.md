<properties 
    pageTitle="Új séma verzió 2015-08-01 – előzetes verzió" 
    description="Megtudhatja, hogy miként írhat a legújabb logika alkalmazások JSON definíciója" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Új séma verzió 2015-08-01 – előzetes verzió

Az új sémafájl és az API verzió összefüggés-alkalmazások javítása, amely a megbízhatóságát számát és a könnyű kezelés alkalmazása logika alkalmazások van. 4 főbb különbségek vannak:

1. A **APIApp** művelettípus egy új **APIConnection** művelettípus frissült.
2. **Ismételje meg** átnevezése a **Foreach**.
3. A **HTTP-figyelő** API-alkalmazás már nem szükséges.
4. Egy új séma alárendelt munkafolyamatok hívásához használja.

## <a name="1-moving-to-api-connections"></a>1. áthelyezése az API-kapcsolatok

A legfontosabb változások az, hogy már nem kell API-alkalmazások terjesztése az Azure-előfizetése API használni. 2 módon API-khoz is használhatja:
* Felügyelt API
* Az egyéni webes API

Az alábbiak mindegyike kezeli kissé másképp, mert a saját irányítási és az adatmodellek szolgáltatója eltérőek. A modell egyik előnye, használja az erőforráscsoport már nem korlátozódik információforrások, amelyek van telepítve. 

### <a name="managed-apis"></a>Felügyelt API-hoz

Számos API, amely a Microsoft kezeli a nevében, például az Office 365-ben, a Salesforce, a Twitteren, a FTP stb... Néhány e felügyelt API-val is alkalmazható-van, például a Bing lefordítása, míg másokat konfigurációs. Ebben a konfigurációban a *kapcsolat*neve.

Ha például az Office 365 használata esetén szüksége, amely tartalmazza az Office 365 bejelentkezési jogkivonat kapcsolat létrehozása. Ez a token biztonságosan tárolja és frissítése, hogy a logika alkalmazás mindig felhívhatja, ha az Office 365 API-t. Másik lehetőségként szeretne csatlakozni az SQL- vagy FTP-kiszolgálóhoz, ha szükséges, amelynek a kapcsolati karakterlánc kapcsolat létrehozása. 

A definíció belül a következő műveletek úgynevezett `APIConnection`. Íme egy példa, hogy a hívások küldjön e-mailt az Office 365-kapcsolat:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

A ráfordítások API kapcsolatok egyedi része a `host` objektumot. Két részből tartalmaz: `api` és `connection`.

A `api` van a futtatókörnyezet URL-CÍMÉT, ahol, felügyelt API üzemelteti. Látható az összes felügyelt API-khoz érhető el, hívja fel `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Egy API-t használ, lehet, hogy vagy valószínűleg nincs **kapcsolat paraméterek** definiált. Ha nem nincs **kapcsolat** szükség. Ha igen, majd be kell hoznia egy olyan kapcsolatot. Amikor adott fognak rendelkezésére állni azt a nevet, kiválaszthatja, hogy kapcsolatot hoz létre, és majd hivatkozik, amely a a `connection` belüli objektumok a `host` objektumot. Kapcsolat létrehozása egy erőforrás csoportban, a hívás:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

A következő szervezethez:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>A felügyelt API-khoz üzembe helyezése egy Azure erőforrás-kezelő sablonban

Egy teljes alkalmazás létrehozhat egy ARM sablon mindaddig, amíg meg nem interaktív bejelentkezést igényelnek. Ha bejelentkezési van szüksége, is a szolgáltatás beállítása a ARM sablonnal, de fog továbbra is fennáll, látogasson el a portálon, a kapcsolatok engedélyezésére. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Ez a példa, hogy a kapcsolatok is, hogy az erőforráscsoport élő csak normál erőforrások megjelenik. A-előfizetése rendelkezésre álló managedAPIs hivatkozás őket.

### <a name="your-custom-web-apis"></a>Az egyéni webes API-hoz

Ha a saját API (illetve különösen azon, nem a Microsoft kezeli egyesre), majd kell használnia a beépített **HTTP** -művelet hívja őket. Annak érdekében, hogy van egy ideális élmény, a API kell egy swagger végpontot nézetéhez. Ezzel engedélyezi a logika alkalmazás Tervező jeleníti meg a ráfordítások és a kimeneti értékeket a API. Anélkül, hogy egy swagger a tervező csak akkor tudja megjeleníteni a ráfordítások és a kimeneti értékeket nem átlátszó JSON objektumként.

Íme egy példa az új `metadata.apiDefinitionUrl` tulajdonság:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Ha Ön üzemelteti **Alkalmazás** szolgáltatása a webes API majd, automatikusan megjelenik a tervezőben elérhető műveletek listájában. Ha nem, illessze be az URL-cím közvetlenül kell. A swagger végpont nem lehet hitelesített, annak érdekében, hogy használható a logika alkalmazások Tervező belül (de magát a API való bármilyen módon támogatja a Swagger a kérhetik).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Az már telepített API-alkalmazások használata a Skype 2015-08-01 – előzetes verzió

Ha korábban már telepítette az API-alkalmazásokban, a **HTTP** -művelet keresztül is felhívhatja.

Például Dropbox lista fájlok használatakor előfordulhat, hogy van valami ehhez hasonló a **2014-es – 12-01 – előzetes** verzió sémadefiníciója a:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Az azonos HTTP művelet például (a Paraméterek csoport alatt változatlanul logika alkalmazás definition marad) állíthat össze:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Útmutató a tulajdonságok egy az egyhez által alapján:

| Művelet tulajdonság |  Leírás |
| --------------- | -----------  |
| `type` | `Http`ahelyett, hogy`APIapp` |
| `metadata.apiDefinitionUrl` | Ha ezzel a művelettel a logika alkalmazások-szerkesztőben, ha meg szeretné jeleníteni a metaadat-alapú végpont érdemes. Ez a helyes összeállítás:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Ez a helyes összeállítás:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Mindig`POST` |
| `inputs.body` | Az api-alkalmazás paramétereket azonos | 
| `inputs.authentication` | Azonos az api-alkalmazás hitelesítés |

Ezt a megközelítést összes API-alkalmazás műveletet kell működniük. Jó helyen jár kérjük, ne feledje, hogy az előző API alkalmazások már nem támogatottak, és helyezze át a két egyéb lehetőségekkel fölött (egy felügyelt API vagy az egyéni webes API szolgáltatója).

## <a name="2-repeat-renamed-to-foreach"></a>2. a ismétlés Foreach átnevezett

Séma előző verziójában azt érkeznek ügyfeleink visszajelzései **ismételje meg** az adott áttekinthetőbb legyen, és nem megfelelően rögzítése, hogy valóban lett-e a az egyes le. Emiatt azt van átnevezi azt **Foreach**. Példa:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Szeretné ekkor kell írni:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

A függvény a korábban `@repeatItem()` használta az aktuális elem alatt többször is fölé hivatkozni szeretne. Ez csak az egyszerűbb lett `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>A kimeneti értékeket, a Foreach hivatkozó
További egyszerűsítése érdekében, a kimeneti értékeket **Foreach** műveletek fog nem kell csomagolni **repeatItems**nevű objektum. Ez azt jelenti, mivel a kimeneti értékeket, a fenti ismétlés volt:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Most már, akkor:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Ezek a kimeneti értékeket hivatkozik, a művelet törzsében eléréséhez szeretné kell elvégeznie:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Most már meg inkább a következőket végezheti el:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

A módosítások a függvények `@repeatItem()`, `@repeatBody()` és `@repeatOutputs()` törlődnek.

## <a name="3-native-http-listener"></a>3. a natív HTTP-figyelő 
A HTTP-figyelő funkciók állnak a beépített, így a már nincs szüksége egy HTTP figyelő API-alkalmazások terjesztése. További információ: [hogyan itt teheti a logika alkalmazás végpont hívható teljes részleteit](app-service-logic-http-endpoint.md). 

A módosítások, a függvény a `@accessKeys()` törlődik, és felváltotta az `@listCallbackURL()` céljából (ha szükséges) a végpont első függvény. Ezeken kívül most definiálni kell legalább egy hatására a logika alkalmazásban most. Ha azt szeretné, hogy `/run` a munkafolyamat-kell egyikére van egy `manual`, `apiConnectionWebhook` vagy `httpWebhook` indítók. 

## <a name="4-calling-child-workflows"></a>4. a hívó alárendelt munkafolyamatok

A korábban hívja fel a gyermek munkafolyamatok szükséges látogasson el a munkafolyamat, első a hozzáférési jogkivonat, és majd beillesztése, hogy az logika alkalmazás, amely fel szeretne hívni a gyermek-definíciós verziójának. Az új verzióval séma a logika alkalmazások motor automatikus generálása egy Társítások futásidőben a gyermek munkafolyamat, ami azt jelenti, hogy nem kell minden titkos kulcsok illessze be a definíció.  Lássunk egy példát:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

A második fokozása, azt fogja kell a gyermek munkafolyamatok teljes hozzáférést biztosítva a beérkező kérelmet. Ez azt jelenti, hogy továbbíthatja paraméterek a *lekérdezések* szakaszában és a *fejlécek* objektumban és, hogy a teljes szervezet teljesen adhatja meg.

Végezetül vannak a gyermek munkafolyamathoz a szükséges módosításokat. Mivel a előtt közvetlenül; csak hívhatja alárendelt munkafolyamat most kell hozni egy eseményindító végpontot, a munkafolyamat, ha fel szeretne hívni a szülő. Általánosságban elmondható Ez azt jelenti, hogy kell hozzáadni, írja be a **kézi** eseményindító, majd használja a szülő-definícióban. Megjegyzendő, hogy a `host` konkrétan a tulajdonságnak a `triggerName`, mert, amely az eseményindító hívott mindig meg kell adnia.

## <a name="other-changes"></a>Egyéb módosítások

### <a name="new-queries-property"></a>Új lekérdezések tulajdonság
Az összes művelet típusú **lekérdezések**nevű egy új beviteli támogatják. Ez lehet egy strukturált objektum helyett meg, hogy a karakterlánc kézzel gyűjtenie.

### <a name="parse-function-renamed"></a>átnevezett Parse() függvény
Azt hamarosan hozzáadja további tartalomtípusok, ahogy a `parse()` függvény átnevezett `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Hamarosan: nagyvállalati integrációs API-hoz
Ezen a ponton a egyszerre hogy még nem rendelkezik felügyelt verzióiban, a nagyvállalati integrációs API-k (például AS2). Ezek fog kell hamarosan az [Ütemterv](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/)hatálya alá. Eközben, használja a meglévő telepített BizTalk API-k keresztül a HTTP-művelet fent, a "használata a már telepített API-alkalmazásokat."
