<properties 
    pageTitle="Tartalom-előállítás logika alkalmazás definíciók |} Microsoft Azure" 
    description="Megtudhatja, hogy miként írhat a JSON definícióját összefüggés-alkalmazások" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>A szerző logika alkalmazás definíciók
Ez a témakör bemutatja, hogyan használhatja a [Azure logika alkalmazások](app-service-logic-what-are-logic-apps.md) definíciókat, amely egy egyszerű, deklaráció JSON nyelvet. Ha eddig még, ellenőrizze, [hogy miként hozhat létre új logika alkalmazás](app-service-logic-create-a-logic-app.md) első. Olvassa el a [teljes anyagminta, a definíció nyelv MSDN](http://aka.ms/logicappsdocs)is.

## <a name="several-steps-that-repeat-over-a-list"></a>Ismételje meg a lista fölé lépésből

Az elemek ismételt legfeljebb 10 k tömb fölé, és hajtsa végre a művelet az egyes [foreach típusa](app-service-logic-loops-and-scopes.md) is élvezheti.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Ha valami hiba kezelésének lépés mentésük.

Gyakran szeretné engedélyezni szeretné írni *remediation lépés* – néhány logikájának hajt végre, ha sikertelen **, és csak ha**, közül egy vagy több a hívásokat. Ebben a példában azt vannak adatok lekérése számos helyről, de ha nem sikerül a hívást, kívánt üzenet valahol közzététele, így e is nyomon követése a hiba, hogy később:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Teheti használata a `runAfter` tulajdonságot, adja meg a `postToErrorMessageQueue` csak működnie kell `readData` **sikertelen**van.  Ez is lehet lehetséges értéklista, így `runAfter` lehet `["Succeeded", "Failed"]`.

Végezetül most kezelése állítható a hibát, mert azt már nem megjelölése a Futtatás **nem sikerült**. Amint itt látható, a Futtatás az **sikerült** akkor is, ha egy szinttel nem sikerült, oka lehet írta-e a lépést a kezeli, ezt a hibát.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Két (vagy több) olyan lépés, amely a párhuzamos végrehajtása

Több műveletek végrehajtási párhuzamosan zajlik, hogy a `runAfter` tulajdonság egyenértékű futásidőben kell lennie. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Ahogy a fenti példában is látható `branch1` és `branch2` futtatására beállított `readData`. Emiatt a következő ágak fog futni párhuzamosan:

![Párhuzamos](./media/app-service-logic-author-definitions/parallel.png)

Láthatja, hogy mindkét ágak időbélyegzőjét megegyezik. 

## <a name="join-two-parallel-branches"></a>Bekapcsolódás a két párhuzamos ágak

Két művelet végrehajtásához párhuzamosan elemek hozzáadásával beállított bekapcsolódhat az `runAfter` tulajdonság hasonló felett.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Párhuzamos](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Lista elemeinek megfeleltetése egyes különböző konfigurációs

A következő, tegyük fel, hogy azt szeretné, hogy a tulajdonság értékét attól függően, hogy teljesen más tartalmat. Értékek helyekre térkép létrehozunk egy paraméterként:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

Ebben az esetben azt először lista beszerzése az cikkek, és kattintson a második lépés keres meg térképen, a kategória paraméterként, melyik URL-CÍMÉT, megkapja a tartalmat a definiált alapján. 

Figyelje meg, az alábbi két elemek: a [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) függvény segítségével ellenőrizze, hogy a kategória megfelel-e egy definiált ismert kategóriát. Második, ha azt sikerült, a kategória, azt átteheti az elemet a térkép szögletes zárójelek használatával: `parameters[...]`. 

## <a name="working-with-strings"></a>Karakterláncok használata

Vannak olyan karakterlánc kezelésére használható funkciók számos. Nézzük végig a példát, ha van olyan karakterlánc, amely a rendszert átadni szeretnénk, de azt nem benne, hogy karakter kódolás kezelje a rendszer megfelelően. Egy azt javasoljuk, hogy base64 kódolása a karakterlánc. Azonban egy URL-címben kijutását elkerülése érdekében fogjuk néhány karakter kicserélendő. 

Egy részét is szeretnénk a a sorrendben, mert az első 5 karakter nem használják.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Belülről kifelé használatáról:

1. Ismerkedés a [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) a megrendelő név visszaad vissza karakterek maximális száma

2. Kivonása 5, (mert szolgáltatásban várjuk rövidebb karaktersorozat)

3. Ténylegesen készítése a [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Index kezdőpont azt `5` , és válassza a karakterlánc maradéka.

4. A karakterlánc szeretne átalakítani egy [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) karakterlánc

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden a `+` a karakterek`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden a `/` a karakterek`_`

## <a name="working-with-date-times"></a>Időpontok használata

Dátum-idő akkor lehet hasznos, különösen akkor, ha a próbál természetesen nem támogatja az **eseményindító**adatforrásból származó adatokat olvashat.  Dátum-idő megállapítása, hogy mennyi ideig különböző lépéseket telik is használhatja. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

Ez a példa a kibontás azt a `startTime` , az előző lépést. Ezután azt a pontos idő első és egy második kivonása:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (például használhat más időegység `minutes` vagy `hours`). Végül azt összehasonlíthatja a következő két értékeket. Ha az első kisebb, mint a második, akkor ez azt jelenti, több, mint egy másodperc telt el a sorrend első óta helyezni. 

Is, hogy azt segítségével karakterlánc formatters formázhatja a dátumokat Megjegyzés: a lekérdezési karakterláncban használható [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) a RFC1123 eléréséhez. Az összes dátum formázási [MSDN ismertetését](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Telepítési időpontokat használata különböző környezetekben

Gyakori, hogy egy telepítési életciklus hol van a fejlesztői környezet, a fejlesztői környezet, majd munkakörnyezetben. Az összes, e, előfordulhat, hogy szeretné ugyanaz a meghatározás, de használjon másik adatbázisok, például. Hasonlóképpen érdemes számos különböző régiók ugyanaz a meghatározás használata az elérhetőség magas, de szeretné logika alkalmazás példányai beszélhet az adott régióban adatbázis. 

Ne feledje, hogy ez különbözik, *futtatókörnyezet*, más paraméterekkel véve a, hogy kell használnia a `trigger()` működnek, a fenti beállításokkal. 

Kezdheti azokat is, az alábbihoz hasonló nagyon simplistic meghatározása:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Ezután, a tényleges `PUT` biztosíthat a paraméter kérése a logika alkalmazáshoz `uri`. Tartsa szem előtt, a már nem tartozik egy alapértelmezett értéket a paraméter megadása kötelező a logika app tartalom:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Minden környezetben majd adja meg a különböző értéket a `connection` paraméter. 

Lásd: a [REST API-dokumentáció](https://msdn.microsoft.com/library/azure/mt643787.aspx) összes létrehozásához és kezeléséhez logika alkalmazások választható lehetőségeket. 
