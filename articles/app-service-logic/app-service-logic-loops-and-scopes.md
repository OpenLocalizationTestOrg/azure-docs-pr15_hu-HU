<properties
   pageTitle="Logika alkalmazások hurkok hatókörök és Debatching |} Microsoft Azure"
   description="Logika alkalmazás hurok, hatókörére és debatching fogalmak"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logika alkalmazások hurkok hatókörök és Debatching
  
>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2016 – 04-01 – előzetes verzió séma és később vonatkozik.  Fogalmak sémákat régebbi hasonló, de hatókörök csak rendelkezésre állnak a séma és újabb.
  
## <a name="foreach-loop-and-arrays"></a>ForEach hurok és a tömb
  
Logika alkalmazások lehetővé teszi az adatok adott csoporthalmaz ismétlése és mindegyik elemhez tartozó művelet végrehajtása.  Ez az alkalmazáson keresztül a `foreach` művelet.  A tervezőben azt is megadhatja, hogy hozzáadása egy az egyes le.  Miután kijelölte a kívánt ismétlés tömb, elkezdhet műveletek.  Jelenleg csak egy művelet per foreach hurok korlátozódik, de ezt a korlátozást azonnal fel kell oldani, az elkövetkezendő héten.  Ha le belül is megkezdi megadhatja, hogy mi történjen, a minden érték a tömb.

Kódnézet használata esetén megadhatja a az egyes hurok például alatt.  Ez a példa a az egyes által küldött e-mailben, az egyes e-mail címet, amely tartalmazza a "microsoft.com" hurok:

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` művelet is találta felett tömbök legfeljebb 5 000 sort.  Minden alkalommal ezzel párhuzamosan hajthat végre, így üzenetek hozzáadásához várólista, ha folyamatvezérlés szükség van szükség lehet.
  
## <a name="until-loop"></a>Amíg ciklus
  
  Elvégezhető művelet vagy műveletsor elindítására mindaddig, amíg a megadott feltétel teljesül.  Az ehhez a leggyakoribb helyzet van hívása zárólap, amíg el nem a válasz, a keres.  A tervezőben azt is megadhatja, hogy hozzáadása egy hurok-ig.  Miután megadta a leállításig belül műveletek, beállíthatja, hogy a Kilépés feltétellel, valamint a leállításig korlátai.  1 perc késés van hurok ciklus között.
  
  Kódnézet használata esetén megadhatja egy mindaddig, amíg a loop például alatt.  Ez a példa hívja fel a HTTP zárólap mindaddig, amíg a válasz szervezet értékkel rendelkezik "Kész".  Amikor befejeződik, vagy 
  
  * HTTP-válasz "Kész" állapota
  * Az 1 óra próbált
  * Azt még beállításpanelen 100 időpontok
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn és Debatching

Előfordul, hogy az eseményindító jelenhet meg egy tömb debatch és elemenként munkafolyamat indítása kívánt elemeket.  A lehet elvégezni a `spliton` parancsot.  Ha a kiváltó ok mező swagger adja meg a tömb méreténél, amely hasznos alapértelmezés szerint egy `spliton` hozzáadódik, és indítsa el a Futtatás elemenként.  SplitOn csak az eseményindító lehet hozzáadni.  A kézi beállítható vagy definition Kódnézet felülbírálva.  Jelenleg SplitOn debatch is tömbök legfeljebb 5 000 elemet.  Nem lehet egy `spliton` és a szinkron válasz mintát is alkalmazhat.  A munkafolyamat neve, amely tartalmaz egy `response` kívül művelet `spliton` asyncronously futtatja és azonnali elküldése `202 Accepted` válasz.  

A megadott SplitOn kód nézetben, a következő példa.  Ez az elemek tömb recieves és debatches minden sorban.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Keresési tartományok

Ajánlatos csoport műveletsor elindítására hatókör együttes használatával.  Ez akkor különösen hasznos végrehajtási kezelésének módját.  A Tervező új hatókör hozzáadásához, és azokat a műveleteket, akkor belül helyezne.  Keresési tartományok az alábbihoz hasonló Kódnézet határozhatja meg:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
