<properties
   pageTitle="Logika alkalmazások kezelésének módját |} Microsoft Azure"
   description="További tudnivalók a mintázatok, a hiba és a kivétel az Azure összefüggés-alkalmazások kezelése"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Logikai alkalmazások hiba és a kivétel kezelése

Logika alkalmazások biztosít gazdag eszközök és mintázatok segítheti elő a integrációs robusztus és rugalmassá hibák szemben.  A problémáit bármely integrációs architektúra az egyik annak biztosítása, hogy legrövidebb leállás és a problémák függő rendszerek megfelelő kezelését.  Logika alkalmazások teszi hibák kezelése első osztályú változat, biztosítva a kivételek és a munkafolyamatok előforduló hibák postaládáról az eszközt.

## <a name="retry-policies"></a>Ismételje meg a házirendek

A kivétel és hibakezelésre legalapvetőbb típusú egy újrapróbálkozási házirend.  A házirend azt határozza meg, ha a művelet meg kell ismételni, ha a kezdeti időtúllépési vagy sikertelen (egy 429 eredményező kérésének vagy 5xx választ).  Alapértelmezés szerint minden műveletek 4 további alkalommal fölé intervallumokat 20 másodperces próbálkozzon újra.  Igen, ha az első kérelem érkezik egy `500 Internal Server Error` válasz, a munkafolyamat-motor szüneteltetheti 20 másodperc és kísérlete a kérést újra.  Ha minden próbálkozás után a válasz is kivétel, illetve hiba, a munkafolyamat folytatódik, és a művelet állapotát, mint `Failed`.

Ismét házirendek beállíthatja az adott művelet a **bemeneti adatok alapján** .  Egy újrapróbálkozási házirend beállítható úgy, hogy időközönként próbálja meg a állhat 4 alkalommal több mint 1 óra értéket.  Minden részlet, a bemeneti tulajdonságok lehet az [MSDN webhelyen található][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Ha a HTTP-művelet 4 alkalommal újra, és várja meg az egyes kísérletek közötti 10 perc azt szeretné, hogy a következő meghatározása:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

További információt a támogatott szintaktikai, tekintse meg a [ismét a házirenddel szakaszában találhat az MSDN][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>Hibák tájékozódást segíti, hogy RunAfter tulajdonság

Minden egyes logikai alkalmazás művelet deklarálása milyen műveleteket kell elvégeznie, mielőtt megkezdi a műveletet.  Érdemes elképzelnie ez munkafolyamatát lépéseiért sorrendje megegyezik.  A sorrend nevezik a `runAfter` tulajdonság a művelet meghatározása.  Olyan objektum, amely ismerteti, hogy mely tevékenységek és a művelet állapotok volna művelet végrehajtása.  Alapértelmezés szerint minden műveletet alkalmaz a tervező segítségével hozzáadhat vannak beállítva `runAfter` az előző lépést, ha az előző lépésben volt `Succeeded`.  Jó helyen jár, testre szabhatja ezt az értéket műveletek esetben, amikor a korábbi műveletek vannak `Failed`, `Skipped`, vagy lehetséges ezeket az értékeket.  Ha a keresett elem hozzáadása a kijelölt szolgáltatás Bus tematikus egy adott művelet után `Insert_Row` nem sikerül, célszerű használni a következő `runAfter` konfigurációs:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Értesítés a `runAfter` tulajdonsága minden olyan esetben, ha a `Insert_Row` művelet `Failed`.  A művelet futtatásához, ha a művelet állapota `Succeeded`, `Failed`, vagy `Skipped` szintaxisa a következő lesz:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Műveletek futtatása után egy előző művelet sikertelen, és a teljes sikerrel, megjelölve `Succeeded`.  Ez a jelenség, az azt jelenti, ha sikeres összes tényleges sikertelen a munkafolyamat a Futtatás magát van megjelölve `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Keresési tartományok és az eredmények műveletek ki szeretné számítani

Hogyan futtatva hasonló egyéni műveletek, után is csoport műveleteket, amelyeket használni kívánt műveletek logikai csoportja közös belül [hatókör](app-service-logic-loops-and-scopes.md) .  Keresési tartományok hasznosak, a logika alkalmazás műveletek rendszerezésében, mind az összesítő értékelést elvégzéséhez a hatókör állapotát.  A hatókör magát kap egy állapotot, hatókör összes művelet befejezése után.  A hatókör határozza meg, a Futtatás – azonos feltételekkel, ha az utolsó művelet egy adatvégrehajtás ág `Failed` vagy `Aborted` az Állapot oszlop értéke `Failed`.

Akkor `runAfter` hatókör van megjelölve `Failed` műveletkifejezést hatálya alá tartozó előfordult hibák esetében a meghatározott műveletekhez.  Futtatása után a hatókör nem sikerül lehetővé teszi a hibák tájékozódást segíti, ha nem sikerül a hatálya alá tartozó művelet *végrehajtása* , hogy egyetlen művelet létrehozása.

### <a name="getting-the-context-of-failures-with-results"></a>Az első hibák és az eredmények környezetében

A hatókör hibák kifogására rendkívül hasznos, de érdemes megértéséhez pontosan milyen műveletek nem sikerült, a helyi és az esetleges hibákat vagy a állapot kódokat küldte-e vissza.  A `@result()` munkafolyamat függvény helyi itt az összes művelet hatókör eredményét.

`@result()`Megnyitja a egyetlen paramétert, a hatókör nevét, és az összes művelet eredményeit, hogy hatálya alá tömbjét adja eredményül.  E művelet objektumok tartalmazza a azonos tulajdonságait, mint a `@actions()` objektum, például művelet kezdetének, a művelet befejezési időpontot, a művelet állapota, a művelet bemeneti adatok alapján, a művelet korrelációs azonosítót és a művelet exportálja.  Is egyszerűen párosítás egy `@result()` működik, ha egy `runAfter` esetleges műveleteket, amelyeket nem sikerült környezetében küldhet egy hatálya alá.

Ha azt szeretné, hogy *az egyes* művelet művelet végrehajtása a hatókör, amely `Failed`, akkor is párosítás `@result()` **[ForEach](app-service-logic-loops-and-scopes.md)** ciklust és egy **[Szűrő tömb](../connectors/connectors-native-query.md)** művelet.  Ez lehetővé teszi, hogy a műveleteket, amelyeket nem sikerült eredményre tömb szűrése.  A szűrt eredménye tömb készíthet, és minden hiba **ForEach** le használata egy művelet végrehajtása.  Íme egy példa az alábbi, részletes magyarázatot követ.  Ez a példa küld az esetleges műveleteket, amelyeket nem sikerült a válasz szervezethez HTTP POST felkérés hatálya alá tartozó `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Az alábbiakban a részletes ismertetését megtalálja a Mi történik:

1. **Szűrő tömb** művelet szeretné szűrni a `@result('My_Scope')` belül az összes művelet eredményét`My_Scope`
1. A **Szűrő tömb** feltétel érték a bármelyik `@result()` egyenlő állapotú cikket `Failed`.  Ez lesz szűrése az összes művelet eredménye tömbje `My_Scope` a művelet sikertelen eredményeinek tömbje csak szeretne.
1. A **Szűrt tömb** kimeneti értékeket **az egyes** művelet végrehajtása  Ez egy művelet *minden egyes* nem sikerült művelet eredménye azt a fenti szűrt végez.
    - Ha egyetlen művelet sikertelen, a műveletek a hatókör volt a `foreach` csak egyszer szeretné futtatni.  Sok sikertelen műveletek több művelet egy ütközést okozhatja.
1. HTTP POST küldeni a `foreach` válasz törzsébe elem vagy `@item()['outputs']['body']`.  A `@result()` elem alakzat megegyezik a `@actions()` alakzat- és ugyanúgy elemezhető.
1. A művelet sikertelen nevű két egyéni fejlécek is tartalmazza `@item()['name']` és a sikertelen futtatása ügyfél-azonosító követés `@item()['clientTrackingId']`.

A hivatkozás, Íme egy példa egy egyetlen `@result()` elemet.  Megjelenik a `name`, `body`, és `clientTrackingId` tulajdonságok elemzi a fenti példában.  Megjegyzendő, amely kívül esik, a `foreach`, `@result()` ezeknek az objektumoknak tömbjét adja eredményül.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

A fenti kifejezések használatával végezze el a különböző kivétel mintázatok kezelése.  Egy egyetlen kivétel kezelése hatálya alá, fogadja el a hibák és eltávolítása a teljes szűrt tömb művelet végrehajtása választhatja a `foreach`.  Egyéb hasznos tulajdonságait is belefoglalható a `@result()` válasz a fent látható.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnosztikai és telemetriai

A fenti minta remek módja kezelendő hibák és a Futtatás belül kivételek, de is azonosítása és megválaszolása hibát független a Futtatás magát.  [Azure diagnosztika](app-service-logic-monitor-your-logic-apps.md) egyszerű biztosítja az összes munkafolyamat-események (beleértve az összes Futtatás és a művelet állapotok) kell küldenie Azure tárterület-fiókkal vagy egy Azure esemény hubhoz.  Figyelje a naplók és -mértékek, vagy közzéteheti őket bármilyen felügyeleti eszközbe inkább, futtatása állapotok ki szeretné számítani.  Több lehetséges, hogy Azure esemény hubon keresztül az események adatfolyam [Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/)be.  Értékáram-elemzés írhat a diagnosztikai naplók az élő lekérdezések bármely rendellenességeinek, átlagokat és sikertelen elhagyja.  Értékáram-elemzés is egyszerűen kimeneti más adatforrásokhoz, például a sorok, témák, SQL, DocumentDB és a Power BI.

## <a name="next-steps"></a>Következő lépések
- [Lásd: hogyan egy customer beépített robusztus hibakezelési logika alkalmazással](app-service-logic-scenario-error-and-exception-handling.md)
- [Keresse meg a további példák összefüggés-alkalmazások és -esetek](app-service-logic-examples-and-scenarios.md)
- [Megtudhatja, hogy miként hozhat létre logika alkalmazások automatikus telepítések](app-service-logic-create-deploy-template.md)
- [Tervezze meg és a Visual Studio logika alkalmazások terjesztése](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9