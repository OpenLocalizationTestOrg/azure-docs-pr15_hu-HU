<properties 
    pageTitle="Kapcsolódás a Media Services-fiók használatával REST API |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan csatlakozhat a Media Services uisng REST API-t." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Keresztüli Media Services Media Services REST API segítségével

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [TÖBBI](media-services-rest-connect-programmatically.md)

Ez a témakör a Microsoft Azure Media Services programozott kapcsolat beszerzése, ha meg van programozási a Media Services REST API-val.

Két dolgot szükségesek, ha a Microsoft Azure Media Services megnyitása:-jogkivonat által biztosított Azure Access vezérlő szolgáltatások (ACS), és a Media Services URI magát. Azt szeretné, hogy ezek a kérelmek létrehozásakor, mindaddig, amíg a megfelelő élőfejet értéket, és átadásakor a hozzáférési jogkivonat megfelelően hívásakor Media Services módon is használhatja.

Az alábbi lépések leírják a leggyakoribb munkafolyamatot, a Media Services REST API-t a Media Services csatlakozni:

1. Az első egy hozzáférési jogkivonat 
2. Kapcsolódás a Media Services URI 

    >[AZURE.NOTE] Https://media.windows.net sikeres kapcsolódás után jelenik meg a 301 átirányítást másik Media Services URI megadása. Az új URI-ra, további hívások kell végeznie.
Az ODATA-API-metaadatok leírását tartalmazó HTTP/1.1 200 választ jelenhet meg.

3. Tartalmakat tehet közzé a későbbi API-hívások az új URL-címet. 

    Ha például ha az csatlakozni, miután szerezte be az alábbi:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    A későbbi API hívások https://wamsbayclus001rest-hs.cloudapp.net/api/ kell közzé.

##<a name="access-control-address"></a>MAC-címe

Media Services MAC-címe https://wamsprodglobal001acs.accesscontrol.windows.net, kivéve a Észak-kínai régió, amennyiben https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Az első egy hozzáférési jogkivonat

Közvetlenül a REST API Media Services eléréséhez egy hozzáférési jogkivonat lekérése ACS, és minden HTTP kérés, akkor készítsen a szolgáltatás során használni. Ez a token más által biztosított az élőfejben vagy a HTTP-kérelem és az OAuth v2 protokoll használata az access jogcímeken alapuló ACS tokenek hasonlít. Egyéb vonatkozó követelmények nem szükséges, hogy közvetlenül a Media Services való csatlakozás előtt.

A következő példa bemutatja a HTTP-kérés fejléce és a szövegtörzs jogkivonat beolvasásához használt.

**Élőfej**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Szervezet**:

Biztos szeretne lenni abban client_id és client_secret értékeket ez kérelem; törzsében client_id és client_secret felel meg a fióknév és AccountKey értéket, illetve. Ezeket az értékeket az Ön által nyújtott Media Services a fiók beállításakor. 

Ne feledje, hogy a Media Services fiókjának AccountKey kell URL-címként kódolt (lásd: az [URL-kódolás](http://tools.ietf.org/html/rfc3986#section-2.1) használatakor a hozzáférési jogkivonat kérelem client_secret értékeként.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Példa: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


A következő példa bemutatja a HTTP-válasz, amely tartalmazza a hozzáférési jogkivonat, a válasz törzsébe.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Egy külső tárolóhoz "access_token" és "expires_in" értékeket gyorsítótárban ajánlott. A token adatok sikerült olvassa be a tár és később újra használni a Media Services REST API-hívások. Az esetek, amikor a token biztonságosan megosztható folyamatok vagy a számítógépek között különösen hasznos.

Ellenőrizze, hogy figyelheti a "expires_in" a hozzáférési jogkivonat értékét, és szükség szerint frissítse az új tokenek a REST API-hívások.

###<a name="connecting-to-the-media-services-uri"></a>Kapcsolódás a Media Services URI

A legfelső szintű Media Services URI https://media.windows.net/. Az eredetileg a URI csatlakozzon, és ha a 301 átirányítást vissza válaszként, gondoskodnia kell az új URI-ra, további hívások. Ezeken kívül ne használjon olyan automatikus-átirányítási/követés logika az igényeinek. HTTP-műveletek és kérelem szervezetek nem továbbításra kerül az új URI-ra.

Ne feledje, hogy a legfelső szintű tölt fel és eszköz-fájlok letöltése URI https://yourstorageaccount.blob.core.windows.net/ a tárhely fióknév esetén ugyanarra a számítógépre a Media Services fiókbeállítás során használt.

Az alábbi példa bemutatja a HTTP-kérés a Media Services legfelső szintű URI (https://media.windows.net/). A kérés megkapja a 301 átirányítást vissza választ. A későbbi kérés használ az új URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-kérés**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-válasz**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-kérés** (az új URI használatával):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-válasz**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Ha már sikerült az új URI, ez az URI, amellyel kommunikálni a Media Services. 


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
