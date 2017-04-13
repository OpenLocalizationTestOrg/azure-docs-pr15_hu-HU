<properties 
   pageTitle="Első lépések az Azure adatok tó tárolja Azure SDK használatának Node.js |} Microsoft Azure"
   description="Megtudhatja, hogy miként Node.js használata tó adattár fiókok és a fájlrendszerben." 
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
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Azure tó adattár Node.js Azure SDK használata – első lépések

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [A PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-VAL](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [NODE.js](data-lake-store-manage-use-nodejs.md)


További információ a Azure SDK Node.js segítségével tó adattár Azure-fiók létrehozása, és olyan mappában, létrehozásakor fájlok feltöltése és letöltése adatok alapvető műveleteket, törölje a fiókot, stb. Tó adattár kapcsolatos további tudnivalókért olvassa el a [Áttekintése a tó adattár](data-lake-store-overview.md)című témakört. Jelenleg a SDK támogatja

  *  **NODE.js verzió: 0.10.0 vagy újabb**
  *  **Fiók REST API-verzió: a Skype 2015-10-01 – előzetes verzió**
  *  **Formázáshoz REST API-verzió: a Skype 2015-10-01 – előzetes verzió**

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Active Directory-alkalmazás létrehozása**. Az Azure Active Directory-alkalmazás segítségével az Azure Active Directory tó adattár alkalmazást hitelesítést végezni. Vannak más módszerekkel hitelesítést végezni az Azure Active Directory, amelyek **végfelhasználói** vagy **szolgáltatás hitelesítés használatával**. Utasításokat és hitelesítést végezni kapcsolatos tudnivalók című témakörben talál [tó áruházzal hitelesítés használatával az Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Telepítése

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Azure Active Directory azonosítania

Az alábbi kódrészletek megjelenítése tó áruházzal hitelesítése különálló kétféle módon Azure Active Directory használatával. A különböző módszereket tó áruházzal hitelesítéshez használt részletes vitafórum olvassa el a [tó áruházzal hitelesítés használatával az Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)című témakört.

Az alábbi kódtöredékének is szükség van a bemeneti adatok alapján, például Azure Active Directory tartományi nevét, ügyfél-azonosító az Azure Active Directory-alkalmazást, más néven stb. Ezeket az adatokat egy alkalmazásból Azure AD, hogy be kell létrehozni, a részleteit is szerepel a fenti hivatkozást tudja visszaszerezni.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>A tó adattár ügyfelek létrehozása

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Adatok tó Store-fiók létrehozása

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
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

## <a name="create-a-file-with-content"></a>Fájl létrehozása a tartalom
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Fájlok és mappák listájának

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Lásd még:

- [Microsoft Azure SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK Node.js - adatok tó Analytics kezelése](https://www.npmjs.com/package/azure-arm-datalake-analytics)
