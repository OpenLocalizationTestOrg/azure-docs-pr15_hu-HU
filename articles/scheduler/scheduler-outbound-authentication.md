<properties
 pageTitle="Kimenő hitelesítést ütemező"
 description="Kimenő hitelesítést ütemező"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Kimenő hitelesítést ütemező

Kimenő hitelesítést igénylő szolgáltatások hívás kezdeményezése a Feladatütemező szolgáltatás szükséges. Ezzel a módszerrel a hívott szolgáltatás megállapítható az ütemezési feladat érhető el az erőforrásait. Az alábbi szolgáltatások néhány egyéb Azure szolgáltatások, a Salesforce.com-hoz, a Facebook és a biztonságos egyéni webhelyek.

## <a name="adding-and-removing-authentication"></a>Hozzáadása és eltávolítása a hitelesítés

Egyszerű egy ütemezési feladat-hitelesítés – hozzáadása egy JSON gyermekelem hozzáadása `authentication` szeretne a `request` elem létrehozásakor vagy frissítésekor egy feladatot. Titkos kulcsok átadni a Feladatütemező helyezése, javítása vagy bejegyzés összehívásban – részeként a `authentication` objektum – soha ne szerepeljenek eredményként a válaszokat. Válasz, a titkos információk értéke null értékű, vagy egy nyilvános jogkivonat, amely a hitelesített entitás lehetnek.

Hitelesítés eltávolításához ELHELYEZNI, vagy a feladat kifejezetten, javítás beállítása a `authentication` objektum null. Bármely hitelesítési tulajdonságai válaszként vissza nem jelenik meg.

Az egyetlen támogatott hitelesítéstípusok jelenleg a `ClientCertificate` modell (a használja az SSL/TLS-ügyfél tanúsítványok), a `Basic` modell (a legbiztonságosabb), és a `ActiveDirectoryOAuth` modell (az Active Directory OAuth hitelesítés.)

## <a name="request-body-for-clientcertificate-authentication"></a>Szervezet kérése ClientCertificate hitelesítéshez

Hitelesítés használatával hozzáadásakor a `ClientCertificate` modell, akkor adja meg a következő további elemek összehívás törzsébe.  

|Elem|Leírás|
|:---|:---|
|_hitelesítés (szülőelem)_|Hitelesítés objektum ügyfél SSL-tanúsítvány használatával.|
|_típus_|Szükséges. Hitelesítés típusa. Ügyfél SSL-tanúsítványok, az értéket kell `ClientCertificate`.|
|_PFX_|Szükséges. A PFX fájl Base64 kódolású tartalmát.|
|_jelszó_|Szükséges. A jelszó a PFX fájl elérésére.|


## <a name="response-body-for-clientcertificate-authentication"></a>Válasz szervezet ClientCertificate hitelesítéshez

Kérés hitelesítési információival küldött, a válasz a az alábbi hitelesítési kapcsolódó elemeket tartalmaz.

|Elem |Leírás |
|:--|:--|
|_hitelesítés (szülőelem)_ |Hitelesítés objektum ügyfél SSL-tanúsítvány használatával.|
|_típus_ |Hitelesítés típusa. Az ügyfél SSL-tanúsítványok, az érték `ClientCertificate`.|
|_certificateThumbprint_ |A tanúsítvány ujjlenyomat.|
|_certificateSubjectName_ |A tárgy megkülönböztető név a tanúsítvány.|
|_certificateExpiration_ |A tanúsítvány lejárati dátumát.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Minta többi kérelem ClientCertificate hitelesítéshez

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Minta többi válasz ClientCertificate hitelesítéshez

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Szervezet kérje az alapszintű hitelesítés

Hitelesítés használatával hozzáadásakor a `Basic` modell, akkor adja meg a következő további elemek összehívás törzsébe.

|Elem|Leírás|
|:--|:--|
|_hitelesítés (szülőelem)_ |Hitelesítés objektum alapszintű hitelesítés használatával.|
|_típus_ |Szükséges. Hitelesítés típusa. Alapszintű hitelesítés, az értéket kell `Basic`.|
|_felhasználónév_ |Szükséges. Felhasználónév hitelesítést végezni.|
|_jelszó_ |Szükséges. Jelszó hitelesítést végezni.|

## <a name="response-body-for-basic-authentication"></a>Válasz szervezet alapszintű hitelesítés

Kérés hitelesítési információival küldött, a válasz a az alábbi hitelesítési kapcsolódó elemeket tartalmaz.

|Elem|Leírás|
|:--|:--|
|_hitelesítés (szülőelem)_ |Hitelesítés objektum alapszintű hitelesítés használatával.|
|_típus_ |Hitelesítés típusa. Alapszintű hitelesítés, az érték `Basic`.|
|_felhasználónév_ |A hitelesített felhasználónév.|

## <a name="sample-rest-request-for-basic-authentication"></a>Alapszintű hitelesítés minta többi kérése

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Minta többi válasz alapszintű hitelesítés

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Szervezet Request for ActiveDirectoryOAuth hitelesítés

Hitelesítés használatával hozzáadásakor a `ActiveDirectoryOAuth` modell, akkor adja meg a következő további elemek összehívás törzsébe.

|Elem |Leírás |
|:--|:--|
|_hitelesítés (szülőelem)_ |Hitelesítés objektum ActiveDirectoryOAuth hitelesítés használatával.|
|_típus_ |Szükséges. Hitelesítés típusa. Az értéket kell ActiveDirectoryOAuth hitelesítéshez `ActiveDirectoryOAuth`.|
|_Bérlői webhelyen_ |Szükséges. Az Azure AD-bérlő bérlői azonosítója.|
|_a célközönség_ |Szükséges. Ez a https://management.core.windows.net/ értékre van állítva.|
|_clientId_ |Szükséges. Adja meg az ügyfél-azonosító az Azure Active Directory-alkalmazáshoz.|
|_titkos kulcs_ |Szükséges. Az ügyfél, a token igénylő titkos.|

### <a name="determining-your-tenant-identifier"></a>Annak megállapítása, a bérlői azonosító

Megtalálhatja a bérlői azonosító az Azure AD-bérlő futtatásával `Get-AzureAccount` az Azure PowerShell.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Válasz szervezet ActiveDirectoryOAuth hitelesítéshez

Kérés hitelesítési információival küldött, a válasz a az alábbi hitelesítési kapcsolódó elemeket tartalmaz.

|Elem |Leírás |
|:--|:--|
|_hitelesítés (szülőelem)_ |Hitelesítés objektum ActiveDirectoryOAuth hitelesítés használatával.|
|_típus_ |Hitelesítés típusa. ActiveDirectoryOAuth hitelesítéshez értéke `ActiveDirectoryOAuth`.|
|_Bérlői webhelyen_ |Az Azure AD-bérlő bérlői azonosítója. |
|_a célközönség_ |Ez a https://management.core.windows.net/ értékre van állítva.|
|_clientId_ |Az Azure Active Directory-alkalmazás ügyfél azonosítója.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Minta többi kérelem ActiveDirectoryOAuth hitelesítéshez

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Minta többi válasz ActiveDirectoryOAuth hitelesítéshez

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Lásd még:


 [Mi az a Feladatütemező?](scheduler-intro.md)

 [Azure ütemező fogalmak, kifejezések és entitás hierarchia](scheduler-concepts-terms.md)

 [Az Azure-portálon a Feladatütemező használatának első lépései](scheduler-get-started-portal.md)

 [Tervek és a számlázásra az Azure ütemező](scheduler-plans-billing.md)

 [Azure ütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Azure ütemező PowerShell-parancsmagok hivatkozás](scheduler-powershell-reference.md)

 [Azure ütemező magas rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure ütemező korlátozások, alapértelmezett és hibakódok esetén](scheduler-limits-defaults-errors.md)
