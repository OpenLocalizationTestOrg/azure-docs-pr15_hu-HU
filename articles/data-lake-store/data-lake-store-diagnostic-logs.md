<properties 
   pageTitle="Az Azure tó adattár diagnosztikai naplók megtekintése |} Microsoft Azure" 
   description="Megtudhatja, hogyan beállítása és a diagnosztikai naplók az Azure tó adattár elérése " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>A diagnosztikai naplók az Azure tó adattár elérése

Tudjon meg többet a tó adattár fiókjának a diagnosztikai naplózás engedélyezése, és hogy hogyan tekintheti meg a naplókat, a fiók gyűjtött.

Szervezetek engedélyezheti a diagnosztikai naplózás Azure tó adattár fiókjukhoz gyűjtendő adatok access könyvvizsgálati ellenőrzéshez, ahol a felhasználók szeretne hozzáférni az adatokat, hogy milyen gyakran érhető el az adatokat, például lista információk mennyi adatokat tárolja a fiók stb.

## <a name="prerequisites"></a>Előfeltételek

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure tó adattár fiókot**. Kövesse az utasításokat az [első lépések az Azure tó adattár az Azure-portálon](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Diagnosztikai naplózás tó adattár fiókjának engedélyezése

1. Jelentkezzen be az új [Azure-portálon](https://portal.azure.com).

2. Nyissa meg a tó adattár fiókot a tó adattár fiók lap, kattintson a **Beállítások**és válassza a **Diagnosztikai beállítások**.

3. A **diagnosztikai** a lap a következő módosításokat a diagnosztikai naplózás.

    ![Diagnosztikai naplózás engedélyezéséhez] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "A diagnosztikai naplók engedélyezése")

    * **Állapot** beállítása **a** diagnosztikai naplózás engedélyezéséhez.
    * Az adatokat tároló/folyamat kétféle módon választhatja.
        * Beállítással az **esemény-hubon keresztül csatlakozott** az exportálandó adatfolyam naplóadatok Azure esemény hubhoz. Ez a beállítás valószínűleg egy későbbi folyamat valós időben bejövő naplók elemzéséhez esetén fogja használni. Ha ezt a beállítást választja, meg kell adnia a részleteket az Azure-esemény használni kívánt központban.
        * Beállítással a **tárterület-fiók exportálása** Azure tárterület-fiókhoz naplók tárolásához. Használhat ezt a lehetőséget, ha az adatokat, amelyek lesznek köteg feldolgozott későbbi időpontban archiválni kívánt. Ha bejelöli ezt a beállítást meg kell adnia a naplók mentése Azure tároló fiók.
    * Adja meg, hogy szeretné-e a naplókat vagy kérelem naplók vagy mindkettőt.
    * Adja meg, amelynek az adatokat kell maradnia napok számát.
    * Kattintson a **Mentés**gombra.

Miután engedélyezte a diagnosztikai beállítások, nézze meg a naplókat, a **Diagnosztikai naplók** lapon.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>A tó adattár fiók diagnosztikai naplók megtekintése

Kétféleképpen a naplóadatokat tó adattár fiókjának megtekintéséhez.

* Az adatok tó áruházból fiókból beállításainak megtekintése
* Az adatokat tároló Azure tárterület-fiókból

### <a name="using-the-data-lake-store-settings-view"></a>Az adatok tó áruház beállításainak nézet

1. Kattintson a tó adattár fiók a **Beállítások** lap, **A diagnosztikai naplók**.

    ![A nézet a diagnosztikai naplózás] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "A diagnosztikai naplók megtekintése") 

2. A **Diagnosztikai naplók** lap a **Naplókat** és a **Naplókat kérése**szerint kategorizált naplókban láthatók.
    * Kérés naplók minden API a tó adattár fiók kérelme rögzítése lehetőséget.
    * Naplókat hasonlóak naplók kérése, de a folyamatban lévő művelet tó adattár fiók sokkal több részletes kifejtése biztosít. Például kérelem naplókban egyetlen feltöltés API hívás okozhat a naplókat több "Hozzáfűző" műveletek.

3. Kattintson az egyes naplóbejegyzések letöltése a naplókat ellen **letöltése** hivatkozásra.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Az Azure tárterület-fiókból, amely tartalmaz naplóadatok

1. Nyissa meg a naplózáshoz tó adattár társított Azure tároló fiók lap, és kattintson a BLOB. A **Blob-szolgáltatás** lap két tárolók sorolja fel.

    ![A nézet a diagnosztikai naplózás] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "A diagnosztikai naplók megtekintése")

    * A tároló **háttérismeretek – naplók-ellenőrzési** a naplókat tartalmazza.
    * A tároló **háttérismeretek – naplók-kérelmek** a kérelem naplók tartalmazza.

2. Ezek a tárolók belül a naplókat csoportban az alábbi szerkezet tárolja.

    ![A nézet a diagnosztikai naplózás] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "A diagnosztikai naplók megtekintése")

    Példaként a teljes elérési útját egy napló lehet`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary, a teljes elérési útját kérelem naplózási lehet`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>A naplózott adatok szerkezetének ismertetése

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
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
| kategória        | Karakterlánc | A napló kategóriára. Ha például **kérések**.                                   |
| operationName   | Karakterlánc | A művelet, hogy be van jelentkezve neve. Ha például getfilestatus.              |
| resultType      | Karakterlánc | A művelet, például 200 állapotát.                                 |
| callerIpAddress | Karakterlánc | A kérés az ügyfél IP-címe                                |
| correlationId   | Karakterlánc | A napló, amelyek azonosítója használt csoportba szeretne foglalni kapcsolódó naplóbejegyzések csoportja |
| azonosító        | Objektum | A napló létrehozott azonosítója                                            |
| Tulajdonságok      | JSON   | A részletekért lásd az alábbi                                                          |

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
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
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
| operationName   | Karakterlánc | A művelet, hogy be van jelentkezve neve. Ha például getfilestatus.              |
| resultType      | Karakterlánc | A művelet, például 200 állapotát.                                 |
| correlationId   | Karakterlánc | A napló, amelyek azonosítója használt csoportba szeretne foglalni kapcsolódó naplóbejegyzések csoportja |
| azonosító        | Objektum | A napló létrehozott azonosítója                                            |
| Tulajdonságok      | JSON   | A részletekért lásd az alábbi                                                          |

#### <a name="audit-log-properties-schema"></a>Naplózási napló tulajdonságok séma

| név       | Típus   | Leírás                              |
|------------|--------|------------------------------------------|
| StreamName | Karakterlánc | Az elérési utat a műveletet végre lett  |


## <a name="samples-to-process-the-log-data"></a>A minták a naplóadatokat feldolgozása

Azure tó adattár minta biztosít, folyamat és elemzése a naplóadatokat. A minta [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)címen található. 


## <a name="see-also"></a>Lásd még:

- [Azure tó adattár áttekintése](data-lake-store-overview.md)
- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)

