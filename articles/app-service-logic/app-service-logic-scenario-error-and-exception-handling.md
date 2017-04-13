<properties
    pageTitle="Naplózás és hibakezelésre vonatkozó az összefüggés-alkalmazások |} Microsoft Azure"
    description="A valós életben használatieset speciális hiba kezelésének és naplózás logika alkalmazással megtekintése"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Naplózás és hibakezelésre vonatkozó összefüggés-alkalmazásokban

Ez a cikk azt ismerteti, hogyan bővítheti a logika alkalmazás hatékonyabb támogatása kezelésének módját. A valós életben használatieset-és a válasz a kérdésre, a "Logika alkalmazásokat támogatja a kivétel és hibakezelésre?"

>[AZURE.NOTE]A logikai alkalmazások funkció a Microsoft Azure alkalmazás szolgáltatás aktuális verziójának művelet válaszokkal nyújt a Normál sablon.
>Ide tartoznak a belső érvényességi és az API-alkalmazás – által eredményül adott hibaérték válaszokkal is.

## <a name="overview-of-the-use-case-and-scenario"></a>A használati eset és forgatókönyv – áttekintés

A következő szövegegység a helyzet használata Ez a cikk.
A jól ismert egészségügyi szervezeti végző számunkra, hogy egy Azure megoldást, amely a betegek portál hozna létre Microsoft Dynamics CRM Online használatával. Találkozó rekordok között a Dynamics CRM Online betegek portált, és a Salesforce küldése esetén szükséges.  Azt felkérték a [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) szabvány használata betegek minden rekordhoz.

A projekt volna két fő követelmények:  

 -  A Dynamics CRM Online portálról küldött rekordok bejelentkezési módszer
 -  Úgy, hogy mikor történt a munkafolyamaton belül a hibák megtekintése


## <a name="how-we-solved-the-problem"></a>Hogyan azt megoldódott a probléma

>[AZURE.TIP] Megtekintheti a projekt áttekintő képet videó [Integrációs felhasználócsoport](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integrációs felhasználói csoportot").

A naplókban és a hiba rekordokat (rekordok dokumentumként hivatkozik DocumentDB) összegyűjti [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") azt választotta. Mivel logika alkalmazások Normál sablon az összes választ, azt nem kellene hozzon létre egy egyéni sémát. Akkor hozható létre hiba és a naplófájlok megtekintése rekordok **beszúrása** és a **lekérdezés** API alkalmazás. Azt is is meghatározhat a séma az egyes az API-alkalmazásból.  

Rekord törlése egy adott dátum után egy másik követelmény volt. DocumentDB [élettartam](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "élettartam") (TTL), hogy mi állítsuk **élettartam** értéket minden rekordhoz vagy a webhelycsoport engedett nevű tulajdonságot tartalmaz. Ez alverzió DocumentDB rekordjaihoz manuálisan törölni kell.

### <a name="creation-of-the-logic-app"></a>Az összefüggés-alkalmazás létrehozása

Az első lépésként a összefüggés-alkalmazás létrehozása, és töltse be a tervezőben. Ebben a példában a szülő-gyermek logika alkalmazások használják azt. Tegyük fel, hogy már korábban elkészült a szülő, és lépjen egy Gyermekszint összefüggés-alkalmazás létrehozása.

Mivel a Dynamics CRM Online ki lesz rekordot kell naplózás fogjuk, első tetején. Használja a kérelem az eseményindító, mert a szülő logika alkalmazás elindítja a gyermek szükség.

> [AZURE.IMPORTANT] Ebben az oktatóanyagban befejezéséhez szüksége lesz DocumentDB adatbázis és a (naplózás és hibák) két webhelycsoportok létrehozása.

### <a name="logic-app-trigger"></a>Logika alkalmazás eseményindító

A kérelem eseményindító azt használják, az alábbi példában látható módon.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Lépések

Jelentkezzen be a forrás (kérelem) a betegek rekord a Dynamics CRM Online portálról szükség.

1. Új találkozó rekord beolvasása a Dynamics CRM Online szükség.
    Az eseményindító CRM érkező a **CRM PatentId**, **rekordtípust**, **Új vagy frissített rekordot** ad (új és frissítheti a logikai érték), és **SalesforceId**. A **SalesforceId** null lehet, mert csak egy frissítési használja.
    A CRM **PatientID** és a **Record Type**azt fog kapni a CRM rekordot.
1. Következő lépésként el kell hozzá a DocumentDB API **InsertLogEntry** művelet a következő szám látható módon.


#### <a name="insert-log-entry-designer-view"></a>Helyezze be a napló bejegyzés Tervező nézete

![Naplóbejegyzés beszúrása](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Helyezze be a hiba bejegyzés Tervező nézetben
![Naplóbejegyzés beszúrása](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>A ellenőrzése hiba rekord létrehozása

![Feltétel](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Logika alkalmazás forráskód

>[AZURE.NOTE]  Az alábbiakban csak minták. Ebben az oktatóprogramban egy végrehajtás jelenleg gyártási alapul, mert az érték egy **Adatforrás csomópont** nem tulajdonságait, amelyek való találkozó ütemezése jelenhet meg.

### <a name="logging"></a>Naplózás
A következő összefüggés alkalmazás-minta megtudhatja, hogy miként kezelje a naplózás.

#### <a name="log-entry"></a>Naplóbejegyzés
Ez az alkalmazás logika forráskód naplóbejegyzést beszúrására.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Log kérés

Ez a közzétett az API-alkalmazásba, a naplófájl kérő üzenet.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Válasz naplózása

Ez az a napló válaszüzenetet a API-alkalmazásból.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Most Vegyük szemügyre az hibakezelésre vonatkozó lépéseket.


### <a name="error-handling"></a>Hibakezelő

A következő összefüggés alkalmazások kód példa bemutatja, hogyan alkalmazhat hibakezelés.

#### <a name="create-error-record"></a>Hiba rekord létrehozása

Ez a egy hiba rekord logika alkalmazások forráskódot.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Hiba beszúrása be DocumentDB – kérése

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Hiba beillesztése DocumentDB – válasz


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Salesforce hiba válasz

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Visszatérés a választ a szülő logika alkalmazás

Után a válasz, a szülő logika alkalmazásban való átviheti.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Vissza a sikeres válasz a szülő logika alkalmazásba

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Vissza a szülő logika alkalmazás hibaüzenetet küld

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>DocumentDB tárházba és portál

A megoldás hozzáadott [DocumentDB](https://azure.microsoft.com/services/documentdb)a további lehetőségeket.

### <a name="error-management-portal"></a>Hiba kezelőportálja segítségével

Kattintva megtekintheti a hibákat, létrehozhat egy MVC webalkalmazás DocumentDB hiba rekordját megjelenítéséhez. **Lista**, **Részletek**, **szerkesztése**és **törlése** műveletek szerepelnek a korábbit.

> [AZURE.NOTE]Szerkesztés művelet: DocumentDB jelent, a csere a teljes dokumentum.
> Rekordok láthatók a **listában** , és a **Részletek** nézetek csak a minták. Még nem tényleges betegek találkozókat.

A MVC app adatait az előzőekben megközelítés készült alábbi példák.

#### <a name="error-management-list"></a>Adatkezelési Hibalista

![Hibalista](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Hiba management adatait megjelenítő nézet

![Részletek](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Log kezelőportálja segítségével

A naplók megtekintéséhez az MVC web app is létrehozott.  A MVC app adatait az előzőekben megközelítés készült alábbi példák.

#### <a name="sample-log-detail-view"></a>Minta napló adatait megjelenítő nézet

![Log adatait megjelenítő nézet](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API-app adatait

#### <a name="logic-apps-exception-management-api"></a>Logika alkalmazások kivétel felügyeleti API

A Megnyitás-forrás logika alkalmazások kivétel management API alkalmazás a következő funkciókat tartalmaz.

Van két vezérlők:

- **ErrorController** DocumentDB gyűjtemény szúr be egy hiba rekord (dokumentum).
- **LogController** Telefonnapló-rekordok (dokumentum) DocumentDB gyűjtemény szúrja be.

> [AZURE.TIP] Mindkét vezérlők használata `async Task<dynamic>` műveletek. Ez a műveletek, el kell hárítani futásidőben, így a DocumentDB séma létrehozunk a művelet a szervezet lehetővé teszi.

Minden dokumentum DocumentDB rendelkeznie kell egy egyedi azonosítót. Használja azt `PatientId` és Unix időbélyegző értéket (dupla) konvertált időbélyeg hozzáadása. Ha el szeretné távolítani a tört érték azt azt csonkolja.

A hiba vezérlő API [a GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)forráskódot tekinthet meg.

A következő szintaxissal logika alkalmazásból hívása az API-t.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Az előző kód mintában a kifejezés **nem sikerült** *Create_NewPatientRecord* állapotának keresi.

## <a name="summary"></a>Összefoglalás

- Naplózás és hibakezelésre vonatkozó egy logikai alkalmazásban egyszerűen hajtja végre.
- Is használhatja DocumentDB a tárházba naplókban és a hiba-rekordok (dokumentumok).
- A portál naplókban és a hiba rekordok megjelenítéséhez a létrehozásához MVC is használhatja.

### <a name="source-code"></a>Forráskód
A forráskód logika alkalmazások kivétel kezelésére API-alkalmazás ez [GitHub tárházba](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logika alkalmazás kivétel Management API")érhető el.


## <a name="next-steps"></a>Következő lépések
- [További példák összefüggés-alkalmazások és -esetek megtekintése](app-service-logic-examples-and-scenarios.md)
- [Tudnivalók a logika alkalmazások figyelése eszközök](app-service-logic-monitor-your-logic-apps.md)
- [Logika alkalmazás automatikus telepítési sablon létrehozása](app-service-logic-create-deploy-template.md)
