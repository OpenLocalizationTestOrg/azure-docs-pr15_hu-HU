<properties 
   pageTitle="A diagnosztikai naplók megtekintése Azure adatok tó elemzéséhez |} Microsoft Azure" 
   description="Megtudhatja, hogyan beállítása és a diagnosztikai naplók Azure adatok tó elemzéséhez elérése " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>A diagnosztikai naplók elérése Azure adatok tó elemzéséhez

Tudnivalók az adatok tó Analytics-fiók a diagnosztikai naplózás engedélyezése, és hogy miként tekintheti meg a naplókat, a fiók gyűjtött.

Szervezetek engedélyezheti a diagnosztikai naplózás Azure adatok tó Analytics fiókjukhoz gyűjtendő adatok access könyvvizsgálati ellenőrzéshez. Ezek a naplók például adja meg adatokat:

* Az adatok elérhető felhasználók listája.
* Milyen gyakran az adatok érhetők el.
* Hogy mennyi adatot a fiók vannak tárolva.

## <a name="prerequisites"></a>Előfeltételek

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- **Engedélyezze az Azure előfizetés** tó Analytics nyilvános Adatvillámnézet. [Útmutatást](data-lake-analytics-get-started-portal.md#signup)találhat.
- **Azure adatok tó Analytics-fiókot**. Kövesse az utasításokat az [első lépések az Azure adatok tó Analytics az Azure portálon](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>A naplózás engedélyezése

1. Jelentkezzen be az új [Azure portálon](https://portal.azure.com).

2. Nyissa meg az adatok tó Analytics-fiókját, és az adatok tó Analytics fiók lap, kattintson a **Beállítások**elemre, és válassza a **Diagnosztikai beállítások**lehetőséget.

3. A **diagnosztikai** a lap a következő módosításokat a diagnosztikai naplózás.

    ![Diagnosztikai naplózás engedélyezéséhez] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "A diagnosztikai naplók engedélyezése")

    * **Állapot** beállítása **a** diagnosztikai naplózás engedélyezéséhez.
    * Az adatokat tároló/folyamat kétféle módon választhatja.
        * Válassza az Azure-esemény hubhoz adatfolyam naplóadatok **esemény központi exportálása** lehetőséget. Használata ezt a lehetőséget, ha van egy későbbi folyamat valós idejű bejövő naplók elemzéséhez. Ha ezt a beállítást választja, meg kell adnia a részleteket az Azure-esemény használni kívánt központban.
        * Jelölje ki a **exportálása tároló fiókba** Azure tárterület-fiókhoz naplók tárolásához. Akkor használja ezt a lehetőséget, ha azt szeretné, az adatok archiválása. Ha ezt a beállítást választja, meg kell adnia a naplók mentése Azure tároló fiók.
    * Adja meg, hogy szeretné-e a naplókat vagy kérelem naplók vagy mindkettőt.
    * Adja meg, amelynek az adatokat kell maradnia napok számát.
    * Kattintson a **Mentés**gombra.

Miután engedélyezte a diagnosztikai beállítások, nézze meg a naplókat, a **Diagnosztikai naplók** lapon.

## <a name="view-logs"></a>Naplók megtekintése

Kétféleképpen az adatok tó Analytics-fiók a naplóadatokat megtekintéséhez.

* Az adatok tó Analytics Fiókbeállítások
* Az adatokat tároló Azure tárterület-fiókból

### <a name="using-the-data-lake-analytics-settings-view"></a>Az adatok tó Analytics beállításainak nézet

1. Kattintson az adatok tó Analytics fiók **Beállítások** lap, **A diagnosztikai naplók**.

    ![A nézet a diagnosztikai naplózás] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "A diagnosztikai naplók megtekintése") 

2. A **Diagnosztikai naplók** lap a **Naplókat** és a **Naplókat kérése**szerint kategorizált naplókban láthatók.
    * Kérés naplók rögzítése minden API kérelme az adatok tó Analytics-fiókot.
    * Naplókat hasonlóak naplók kérése, de az adatok tó Analytics-ügyfél hajt végre a műveleteket sokkal több részletes kifejtése biztosít. Például kérelem naplókban egyetlen feltöltés API hívás okozhat a naplókat több "Hozzáfűző" műveletek.

3. Kattintson a **letöltése** hivatkozására kattintva töltse le a naplókat naplóbejegyzést.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Az Azure tárterület-fiókból, amely tartalmaz naplóadatok

1. Nyissa meg az Azure tároló fiók lap Naplózás az adatok tó Analytics társított, és kattintson a BLOB. A **Blob-szolgáltatás** lap két tárolók sorolja fel.

    ![A nézet a diagnosztikai naplózás] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "A diagnosztikai naplók megtekintése")

    * A tároló **háttérismeretek – naplók-ellenőrzési** a naplókat tartalmazza.
    * A tároló **háttérismeretek – naplók-kérelmek** a kérelem naplók tartalmazza.

2. Ezek a tárolók belül a naplókat csoportban az alábbi szerkezet tárolja.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] A `##` az elérési út bejegyzések tartalmaznak, az év, hónap, nap és jött létre a napló óra. Adatok tó Analytics létrehoz egy fájl óránként, így `m=` mindenképp kell tartalmaznia egy értéket a `00`.

    Példaként a teljes elérési útját egy napló lehet:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Hasonlóképpen a teljes elérési útját kérelem naplózási lehet:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Log struktúra

A naplózási és kérelem naplók JSON formátumban vannak. Ebben a részben hogy tekintse meg kérelme JSON szerkezetének és naplók.

### <a name="request-logs"></a>Naplók kérése

Íme egy példa bejegyzést a JSON-formátumú napló. Minden egyes blob egy legfelső szintű objektumra, más néven napló objektumok tömbképletet tartalmazó **rekordokat** tartalmaz.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Log séma kérése

| név            | Típus   | Leírás                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| idő            | Karakterlánc | A napló (a UTC szerint) az időbélyegző                                              |
| resourceId      | Karakterlánc | Helyezze az erőforrás művelet beállításikon azonosítója                            |
| kategória        | Karakterlánc | A napló kategóriára. Ha például **kéréseket**.                                   |
| operationName   | Karakterlánc | A művelet, hogy be van jelentkezve neve. Ha például GetAggregatedJobHistory.              |
| resultType      | Karakterlánc | A művelet, például 200 állapotát.                                 |
| callerIpAddress | Karakterlánc | A kérés az ügyfél IP-címe                                |
| correlationId   | Karakterlánc | A napló azonosítója. Ez az érték használható csoport kapcsolódó naplóbejegyzések csoportja |
| azonosító        | Objektum | A napló létrehozott azonosítója                                            |
| Tulajdonságok      | JSON   | Lásd: további információt a következő szakaszban (kérés napló tulajdonságok séma) |

#### <a name="request-log-properties-schema"></a>Log tulajdonságok séma kérése

| név                 | Típus   | Leírás                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | Karakterlánc | A művelethez a HTTP-metódus használt. Ha például KÉRJEN. |
| Elérési út                 | Karakterlánc | Az elérési utat a műveletet végre lett                   |
| RequestContentLength | int    | A tartalom hosszát a HTTP-kérés                    |
| ClientRequestId      | Karakterlánc | A kérés egyedileg azonosító azonosítója              |
| Kezdés időpontja            | Karakterlánc | Az idő, amelynél a kiszolgáló kapott a kérelmet         |
| Befejezés időpontja              | Karakterlánc | Az idő, amelynél a kiszolgáló által küldött válasz              |

### <a name="audit-logs"></a>Naplókat

Íme egy példa bejegyzést a JSON-formátumú napló. Minden egyes blob rendelkezik egy olyan legfelső szintű objektumra, más néven napló objektumok tömbképletet tartalmazó **rekordok**

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Naplózási napló séma

| név            | Típus   | Leírás                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| idő            | Karakterlánc | A napló (a UTC szerint) az időbélyegző                                              |
| resourceId      | Karakterlánc | Helyezze az erőforrás művelet beállításikon azonosítója                            |
| kategória        | Karakterlánc | A napló kategóriára. Ha például **naplózási**.                                      |
| operationName   | Karakterlánc | A művelet, hogy be van jelentkezve neve. Ha például JobSubmitted.              |
| resultType | Karakterlánc | A feladat állapota (operationName) az egy részállapot. |
| resultSignature | Karakterlánc | További részletekért kattintson a feladat állapota (operationName). |
| azonosító      | Karakterlánc | A felhasználó által kért a műveletet. Ha például susan@contoso.com.                                 |
| Tulajdonságok      | JSON   | Lásd: további információt a következő szakaszban (naplózási napló tulajdonságok séma) |

> [AZURE.NOTE] __resultType__ __resultSignature__ a művelet eredményét adja meg az adatokat, és csak egy értéket tartalmaz, ha egy művelet befejeződött. Ha például tartalmaznak értéket __operationName__ __JobStarted__ vagy __JobEnded__az értéket tartalmazó.

#### <a name="audit-log-properties-schema"></a>Naplózási napló tulajdonságok séma

| név       | Típus   | Leírás                              |
|------------|--------|------------------------------------------|
| JobId | Karakterlánc | A projekthez hozzárendelt azonosítója  |
| Feladat | Karakterlánc | A nevet, amelyet adta meg a projektre vonatkozóan |
| JobRunTime | Karakterlánc | A feladat végrehajtására használt a futtatókörnyezet |
| SubmitTime | Karakterlánc | A ideje (UTC szerint megadva), amely a feladat elindították |
| Kezdés időpontja | Karakterlánc | A indításakor a feladat futtatása után (UTC) szerint benyújtásának. |
| Befejezés időpontja | Karakterlánc | A feladat befejezésének idő. |
| Párhuzamos | Karakterlánc | Ehhez a feladathoz Beküldési során kért adatokat tó Analytics egységek számát. |

> [AZURE.NOTE] __SubmitTime__, a __Kezdés időpontja__, a __Befejezés időpontja__ és a __párhuzamos__ művelet adja meg az adatokat, és csak egy értéket tartalmaz, ha egy művelet lépések vagy befejezett. Például az __SubmitTime__ értéket tartalmaz, miután __operationName__ __JobSubmitted__jelzi.

## <a name="process-the-log-data"></a>A naplózott adatok folyamatábra

Azure adatok tó Analytics minta biztosít, folyamat és elemzése a naplóadatokat. A minta [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)címen található. 


## <a name="next-steps"></a>Következő lépések

- [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)


