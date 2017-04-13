<properties
   pageTitle="Azure adatok tó Analytics Azure SDK használatának Node.js kezelése |} Azure"
   description="Útmutató: adatok tó Analytics-fiókokat, adatforrásokhoz, feladatok és Azure SDK használatának Node.js felhasználók kezelése"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Azure adatok tó Analytics Azure SDK használatának Node.js kezelése


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Az Azure SDK Node.js az Azure adatok tó Analytics fiókok, feladatok és katalógusok kezelésére használható. Adatkezelési témakör egyéb eszközök segítségével megtekintéséhez kattintson a fenti lapon válasszon.

Jobbra, most támogatja:

  *  **NODE.js verzió: 0.10.0 vagy újabb**
  *  **Fiók REST API-verzió: a Skype 2015-10-01 – előzetes verzió**
  *  **Katalógus REST API-verzió: a Skype 2015-10-01 – előzetes verzió**
  *  **Feladat REST API-verzió: 2016 – 03 – 20 – előzetes verzió**

## <a name="features"></a>Szolgáltatások

- A fiók kezelése: hozzon létre, kérjen, lista, frissíthet és törölhet.
- Feladat kezelése: elküldése, juthat, lista, Mégse gombra.
- Katalógusegyesítési kezelése: get, lista, hozzon létre (titkos kulcsok), (titkos kulcsok) frissítése és törlése a (titkos kulcsok).

## <a name="how-to-install"></a>Telepítése

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Azure Active Directory azonosítania

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Az adatok tó Analytics-ügyfél létrehozása

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Adatok tó Analytics-fiók létrehozása

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>A feladatok lista beszerzése

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Lista beszerzése az adatbázisok tó Analytics adatkatalógusában
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Lásd még:

- [Microsoft Azure SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK Node.js - adatok tó kezelése](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
