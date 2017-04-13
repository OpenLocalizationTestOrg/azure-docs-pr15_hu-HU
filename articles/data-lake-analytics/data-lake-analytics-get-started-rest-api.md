<properties 
   pageTitle="Első lépések az adatok tó Analytics REST API segítségével |} Microsoft Azure" 
   description="Adatok tó Analytics műveletek hajthatók végre WebHDFS REST API-k használata" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Első lépések az Azure adatok tó Analytics REST API-k használata

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Megtudhatja, hogy miként WebHDFS REST API-k és az adatok tó Analytics REST API-k kezelése a adatok tó Analytics partnerek, a feladatok és a katalógus használatával. 

## <a name="prerequisites"></a>Előfeltételek

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Active Directory-alkalmazás létrehozása**. Az adatok tó Analytics alkalmazás az Azure Active Directory hitelesítő az Azure Active Directory-alkalmazás segítségével. Vannak más módszerekkel hitelesítést végezni az Azure Active Directory, amelyek **végfelhasználói** vagy **szolgáltatás hitelesítés használatával**. Utasításokat és hitelesítést végezni kapcsolatos tudnivalók című témakörben talál [az adatok tó Analytics használatával az Azure Active Directory hitelesítő](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Ez a cikk cURL használja a bemutatják, hogyan készíthet egy adatok tó Analytics-fiók REST API-hívások.

## <a name="authenticate-with-azure-active-directory"></a>Az Azure Active Directory hitelesítő

Két módon az Azure Active Directory hitelesítő.

### <a name="end-user-authentication-interactive"></a>Végfelhasználói hitelesítés (interaktív)

Ezzel a módszerrel alkalmazás kéri, jelentkezzen be a felhasználó, és az összes művelet a felhasználó környezetében történik. 

Kövesse ezeket a lépéseket az interaktív hitelesítés:

1. Az alábbi URL-címet a felhasználó átirányítása a alkalmazáson keresztül:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Az ÁTIRÁNYÍTÁSI-URI > kell kell kódolva használata az URL-címet. Igen, https://localhost, használja a `https%3A%2F%2Flocalhost`)

    Céljából ebben az oktatóanyagban cserélje ki a helyőrző feletti URL-címét, és illessze be a böngésző címsorában. Az Azure login azonosítania irányítja. Miután sikeresen, jelentkezzen be, a válasz a böngésző címsorában jelenik meg. A válasz lesz a következő formátumban:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Rögzítés a kérdésre adott választ engedélyezési kódot. Ebben az oktatóprogramban az engedélyezési kód másolása a böngésző címsorába, és adja át a bejegyzés kérésben a token végpont alább látható módon:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Ebben az esetben a \<ÁTIRÁNYÍTÁS-URI > nem szükséges kódolt.

3. A válasz egy JSON-objektum, amely egy hozzáférési jogkivonat tartalmazza (például `"access_token": "<ACCESS_TOKEN>"`) és a frissítési jogkivonat (például `"refresh_token": "<REFRESH_TOKEN>"`). Az alkalmazás segítségével az jogkivonat Azure tó adattár és a frissítési jogkivonat elérésekor jelenik meg egy másik hozzáférési jogkivonat, ha egy hozzáférési jogkivonat lejár.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Amikor lejár az jogkivonat, kérheti egy új jogkivonat, használja a frissítés jogkivonat, alább látható módon:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Interaktív felhasználóhitelesítés kapcsolatos további tudnivalókért olvassa el a [Adja meg az adatfolyam engedélyezési kód](https://msdn.microsoft.com/library/azure/dn645542.aspx)című témakört.

### <a name="service-to-service-authentication-non-interactive"></a>Szolgáltatás hitelesítés (nem interaktív)

Ezzel a módszerrel alkalmazás biztosít a saját hitelesítő adatok a műveletek elvégzéséhez. Ahhoz, hogy ez el kell küldenie a bejegyzés kérelmének olyanra, mint az alább látható módon: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

A kimenet a kérelem fogja tartalmazni egy jóváhagyási jogkivonat (-lel `access-token` az alábbi kimeneti), hogy később továbbítja a REST API-hívások. Mentse a hitelesítési token szövegfájlban; Ez a cikk későbbi részében lesz szüksége.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Ez a cikk használja a **nem interaktív** módon. Nem interaktív (szolgáltatás hívások) kapcsolatos további tudnivalókért olvassa el a [szolgáltatást, hogy a hívások hitelesítő adataival](https://msdn.microsoft.com/library/azure/dn645543.aspx)című témakört.
## <a name="create-a-data-lake-analytics-account"></a>Adatok tó Analytics-fiók létrehozása

Az Azure erőforráscsoport, és egy tó adattár fiók egy adatok tó Analytics-fiók létrehozása előtt kell létrehoznia.  Lásd: [tó adattár fiók létrehozása](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

A következő Curl parancsot szemlélteti, hogyan lehet fiókot létrehozni:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Csere \< `REDACTED` \> a jóváhagyási jogkivonat, a \< `AzureSubscriptionID` \> az előfizetés azonosítójú \< `AzureResourceGroupName` \> egy meglévő Azure erőforráscsoport nevét, és \< `NewAzureDataLakeAnalyticsAccountName` \> adatok tó Analytics-fiók új néven. A parancs a kérelem tartalom megadva **CreateDatalakeAnalyticsAccountRequest.json** fájlban található a `-d` fenti paraméter. A input.json fájl tartalmát a következőhöz hasonló:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Az előfizetés lista adatok tó-elemző fiókjai

A következő Curl parancs előfizetés a partnerek lista mutatja be:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Csere \< `REDACTED` \> , a jóváhagyási jogkivonat \< `AzureSubscriptionID` \> az előfizetés azonosítójával. A kimenet hasonlít:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Egy adatok tó Analytics-fiók adatainak megtekintése

A következő Curl parancs beszerzése-fiók adatait jeleníti meg:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Csere \< `REDACTED` \> a jóváhagyási jogkivonat, a \< `AzureSubscriptionID` \> az előfizetés azonosítójú \< `AzureResourceGroupName` \> egy meglévő Azure erőforráscsoport nevét, és \< `DataLakeAnalyticsAccountName` \> egy meglévő adatok tó Analytics-fiókot a nevet. A kimenet hasonlít:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Lista adatainak tó tárolja adatok tó Analytics-fiók

A következő Curl parancs lista az adatok tó tárolja-fiók mutatja be:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Csere \< `REDACTED` \> a jóváhagyási jogkivonat, a \< `AzureSubscriptionID` \> az előfizetés azonosítójú \< `AzureResourceGroupName` \> egy meglévő Azure erőforráscsoport nevét, és \< `DataLakeAnalyticsAccountName` \> egy meglévő adatok tó Analytics-fiókot a nevet. A kimenet hasonlít:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Az SQL-U feladatok elküldése

A következő Curl parancs szemlélteti, hogyan lehet az SQL-U feladat elküldése:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Csere \< `REDACTED` \> , a jóváhagyási jogkivonat \< `DataLakeAnalyticsAccountName` \> egy meglévő adatok tó Analytics-fiókot a nevet. A parancs a kérelem tartalom megadva **SubmitADLAJob.json** fájlban található a `-d` fenti paraméter. A input.json fájl tartalmát a következőhöz hasonló:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

A kimenet hasonlít:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Lista U-SQL nyelvben feladatok

A következő Curl parancs szemlélteti, hogyan lehet az SQL-U feladatok lista:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Csere \< `REDACTED` \> a jóváhagyási jogkivonat, és \< `DataLakeAnalyticsAccountName` \> egy meglévő adatok tó Analytics-fiókot a nevet. 


A kimenet hasonlít:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Katalógus-elemek

A következő Curl parancs az adatbázisok beszerzése a katalógusról jeleníti meg:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

A kimenet hasonlít:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Lásd még:

- Összetett lekérdezés talál további [elemzés webhely naplók Azure adatok tó Analytics használatával](data-lake-analytics-analyze-weblogs.md).
- Első lépésként U – SQL-alkalmazások fejlesztésével lásd: [adatok tó Tools for Visual Studio segítségével kidolgozása U – SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).
- Az SQL-U című témakörben talál [Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md).
- Teendőkről olvassa el az [Azure adatok kezelése tó Analytics Azure portal segítségével](data-lake-analytics-manage-use-portal.md)című témakört.
- Adatok tó Analytics áttekinti a [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)című témakörben találhat.
- Az egyéb eszközök segítségével azonos oktatóprogram megtekintéséhez kattintson a lap választók lap tetején.
- Diagnosztikai információk naplózása súgójának [elérése diagnosztikai naplók Azure adatok tó elemzéséhez](data-lake-analytics-diagnostic-logs.md)
