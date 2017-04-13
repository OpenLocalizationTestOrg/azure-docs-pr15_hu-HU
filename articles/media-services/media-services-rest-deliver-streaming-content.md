<properties 
    pageTitle="Azure Media Services-tartalom használata a többi közzététele" 
    description="Megtudhatja, hogy miként hozhat létre egy megnevezés adatfolyam URL-cím létrehozásához használt. A kód REST API-t használja." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-rest"></a>Azure Media Services-tartalom használata a többi közzététele

> [AZURE.SELECTOR]
- [.NET](media-services-deliver-streaming-content.md)
- [TÖBBI](media-services-rest-deliver-streaming-content.md)
- [Portál](media-services-portal-publish.md)

##<a name="overview"></a>– Áttekintés


Egy adaptív átviteli sebesség MP4 beállítása: egy OnDemand adatfolyam megnevezés, valamint épület adatfolyam URL-címet is adatfolyamként. A [Tárgyi eszköz kódolást](media-services-rest-encode-asset.md) témakörben megtudhatja, hogy miként kódolását egy adaptív átviteli sebesség MP4 állítsa be. Ha a tartalom titkosítva van, állítsa be eszköz kézbesítési házirend ( [Ebben](media-services-rest-configure-asset-delivery-policy.md) a témakörben leírtak) egy megnevezés létrehozása előtt. 

Egy megnevezés streaming OnDemand fokozatosan letölthető MP4-fájlok mutató URL-össze is használhatja.  

Ez a témakör bemutatja, hogyan hozhat létre egy OnDemand a folyamatos átvitelű megnevezés tegye közzé a eszköz és egy sima MPEG szaggatott és a folyamatos átvitelű URL-címek HLS összeállítása. Azt is láthatók lesznek hideg, létrehozásához a progresszív letöltés URL-ek.

A [következő](#types) szakaszban a felsorolás típusú, amelynek értékeket fogja használni a többi hívások jeleníti meg.   
  
##<a name="create-an-ondemand-streaming-locator"></a>Hozzon létre egy a folyamatos átvitelű megnevezés OnDemand

A folyamatos átvitelű megnevezés OnDemand létrehozása és az URL-címek első kell tegye a következőket:


   1. Ha a tartalom titkosítva van, az access-házirend megadása
   2. Hozzon létre egy a folyamatos átvitelű megnevezés OnDemand.
   3. Adatfolyam-tervezi, ha fájlletöltés az adatfolyam nyilvánvalóan (.ism) az eszközt. 
        
    Ha fokozatosan töltheti le, beszerzése a MP4-fájlok eszköz nevét. 
   4. URL-címek összeállítása a nyilvánvalóan fájl vagy MP4-fájlokat. 
   5. Megjegyzendő, hogy nem hozhatók létre, amely tartalmaz egy AccessPolicy használata egy adatfolyam megnevezés írásos vagy engedélyek törlése.


###<a name="create-an-access-policy"></a>Az access-házirend létrehozása

A kérelem:
        
    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    Host: media.windows.net
    Content-Length: 68
    
    {"Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}
    
Válasz:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 311
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https:/media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3A69c80d98-7830-407f-a9af-e25f4b0d3e5f')
    Server: Microsoft-IIS/8.5
    request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:52:09 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AccessPolicies/@Element","Id":"nb:pid:UUID:69c80d98-7830-407f-a9af-e25f4b0d3e5f","Created":"2015-02-18T06:52:09.8862191Z","LastModified":"2015-02-18T06:52:09.8862191Z","Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

###<a name="create-an-ondemand-streaming-locator"></a>Hozzon létre egy a folyamatos átvitelű megnevezés OnDemand

A megnevezés a megadott eszköz és eszköz-házirend létrehozása.

A kérelem:
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    Host: media.windows.net
    Content-Length: 181
    
    {"AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872Z","Type":2}

Válasz:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 637
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Abe245661-2bbd-4fc6-b14f-9cf9a1492e5e')
    Server: Microsoft-IIS/8.5
    request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:58:37 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#Locators/@Element","Id":"nb:lid:UUID:be245661-2bbd-4fc6-b14f-9cf9a1492e5e","ExpirationDateTime":"2015-03-20T06:34:47.267872+00:00","Type":2,"Path":"http://amstest1.streaming.mediaservices.windows.net/be245661-2bbd-4fc6-b14f-9cf9a1492e5e/","BaseUri":"http://amstest1.streaming.mediaservices.windows.net","ContentAccessComponent":"be245661-2bbd-4fc6-b14f-9cf9a1492e5e","AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872+00:00","Name":null}

###<a name="build-streaming-urls"></a>A folyamatos átvitelű URL-címek összeállítása

Az **elérési út** érték a megnevezés létrehozása után vissza az MPEG kötőjel URL sima és HLS használatával. 

Zökkenőmentes adatfolyam: **Elérési út** + nyilvánvalóan fájlnév + "/ cikkét"

Példa:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest

HLS: **Elérési út** + jegyzék fájl neve + "/ manifest(format=m3u8-aapl)"

Példa:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)


SZAGGATOTT: **Elérési út** + nyilvánvalóan fájlnév + "/ manifest(format=mpd-time-csf)"


Példa:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


###<a name="build-progressive-download-urls"></a>Összeállítása a progresszív letöltés URL-címei

Az **elérési út** érték a megnevezés létrehozása után vissza az progresszív letöltés URL-cím használatával.   

URL-címe: **Elérési út** + digitáliseszköz-fájl mp4 neve

Példa:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

##<a id="types"></a>Felsorolás típusai

    [Flags]
    public enum AccessPermissions
    {
        None = 0,
        Read = 1,
        Write = 2,
        Delete = 4,
        List = 8,
    }

    public enum LocatorType
    {
        None = 0,
        Sas = 1,
        OnDemandOrigin = 2,
    }

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még:

[A digitális eszköz kiválasztása kézbesítési házirend beállítása](media-services-rest-configure-asset-delivery-policy.md)
