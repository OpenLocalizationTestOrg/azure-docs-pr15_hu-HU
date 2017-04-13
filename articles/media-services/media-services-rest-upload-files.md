<properties 
    pageTitle="Fájlok feltöltése a Media Services használata a többi figyelembe |} Microsoft Azure" 
    description="Megtudhatja, hogy miként médiatartalom foglalkozni Media Services létrehozásával és eszközök feltöltése." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Fájlok feltöltése a többi használatával Media Services-fiókba

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [TÖBBI](media-services-rest-upload-files.md)
 - [Portál](media-services-portal-upload-files.md)

A Media Services a digitális fájlok eszköz be feltöltése. Az [eszköz](https://msdn.microsoft.com/library/azure/hh974277.aspx) entitás is tartalmazhat, videó, hang, képek, miniatűrök gyűjtemények, szöveg számok és kódolt felirat fájlok (és ezek a fájlok metaadatait.)  A fájlok feltöltése az eszköz be befejeződik, amikor a tartalom tárolási biztonságosan további feldolgozásra és a folyamatos átvitelű a felhőben. 

>[AZURE.NOTE]Egy tárgyi eszköz fájl nevének megadása érvényesek a következő szempontokat mérlegelve:
>
>- Media Services használja a IAssetFile.Name tulajdonság értékét adatfolyam (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) tartalma URL-címek készítésekor Emiatt az URL-kódolás nem engedélyezett. A **Name** tulajdonság értéke nem lehet a következő [karakterek készültségi kódolást-fenntartott](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Még csak lehet egy "." esetében a fájlnévhez tartozó kiterjesztést.
>
>- Nevének hossza nem lehet nagyobb, mint 260 karakter.

Az egyszerű munkafolyamat eszközök feltöltése az alábbi szakaszok van osztva:

- Egy tárgyi eszköz létrehozása
- (Nem kötelező) egy eszköz titkosítása
- Fájl feltöltése blob-tárolóhoz

AMS azt is lehetővé teszi, hogy töltse fel a tömeges eszközök. További tudnivalókért lásd az [Ebben](media-services-rest-upload-files.md#upload_in_bulk) a szakaszban.

##<a name="upload-assets"></a>Töltse fel a eszközök

###<a name="create-an-asset"></a>Egy tárgyi eszköz létrehozása

>[AZURE.NOTE] Amikor a Media Services REST API-val, a következő érvényesek:
>
>A Media Services szervezetek elérésekor a be kell állítani a HTTP-összehívásokban adott fejlécmezők és értékek. További tudnivalókért olvassa el a [Media Services REST API -val fejlesztési beállítása](media-services-rest-how-to-use.md)című témakört.

>Https://media.windows.net sikeres kapcsolódás után jelenik meg a 301 átirányítást másik Media Services URI megadása. A [Csatlakozás a Media Services REST API segítségével](media-services-rest-connect-programmatically.md)leírtak szerint új URI későbbi hívásainak kell végeznie. 
 
Egy tárgyi eszköz tároló típusú vagy a Media Services, beleértve a videó a hang, képek, miniatűrök gyűjtemények, szöveg-s zeneszámok fájlok, és kódolt felirat objektumok csoportja. A REST API-eszköz létrehozása és elő kell készítenie POST kérelem küld a Media Services úgy, hogy az eszköz minden tulajdonság információt összehívás törzsébe.

A tulajdonságok, adhatja meg az eszköz létrehozásakor egyik **Beállítások**. **Beállítások** érték egy számbavétel, amely leírja a titkosítási beállításokat, hogy az eszköz hozhatja létre. Érvényes érték az értékek közül nem kombinációjának az alábbi listából. 

- **Nincs** = **0**: titkosítás nem lesz. Az alapértelmezett értéket. Figyelje meg, hogy ez a beállítás használata esetén a tartalom nem védett a hálózaton átvitt vagy tárolóban lévő többi.
    Ha szeretne egy MP4 előadása fokozatos letöltése, használja a használja ezt a beállítást. 

- **StorageEncrypted** = **1**: megadhatja, hogy a fájlok feltöltése és tárolására szolgáló AES-256 bit titkosítással titkosítása számára.

    Ha az eszköz titkosított tárhely, meg kell adnia az eszköz kézbesítési házirend. További információt a [konfigurálása eszköz kézbesítési házirend](media-services-rest-configure-asset-delivery-policy.md)témakörben található.

- **CommonEncryptionProtected** = **2**: Adja meg, ha az egy közös titkosítási módszer (például PlayReady) védett fájlok feltöltése. 

- **EnvelopeEncryptionProtected** = **4**: Adja meg, ha a titkosított fájlok AES HLS tölt fel. Ne feledje, hogy a fájlok kell van már kódolt átalakítása Manager titkosítja.

>[AZURE.NOTE]Ha az eszköz fog használni a titkosítást, létre kell hoznia egy **ContentKey** és hivatkozás a digitális eszköz kiválasztása a következő témakör útmutatását:[hogyan hozhat létre egy ContentKey](media-services-rest-create-contentkey.md). Figyelje meg, hogy után a fájlok feltöltése a be az eszközt, módosítania kell a titkosítási tulajdonságok a **AssetFile** entitás szerezte be a **digitáliseszköz** -titkosítás során értékekkel. Végezze el ezt a http- **EGYESÍTÉSE** kérés használatával. 


A következő példa bemutatja, hogyan hozhat létre egy eszközt.

**HTTP-kérés**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

**HTTP-válasz**

Ha sikeres, az alábbi adja vissza:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Hozzon létre egy AssetFile

A [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) entitás video- vagy blob-tárolóban tárolt fájlt jelöl. Egy tárgyi eszköz fájl mindig tárgyi eszköz társított, és tárgyi eszköz egy vagy több eszköz fájlokat tartalmazhatnak. A Media Services Encoder tevékenység sikertelen lesz, ha az eszköz fájlt objektumként program nem társítja blob-tároló digitális fájl.

Ne feledje, hogy az **AssetFile** példány és a tényleges médiafájl két különálló objektummal. AssetFile példány, a multimédiás fájl metaadatokat tartalmaz, miközben a multimédiás fájl tartalmaz a tényleges médiatartalom.

Blob-tároló be a digitális eszközökkel készített tartalom fájl feltöltése, után a http- **EGYESÍTÉSE** kérés fogja használni a frissítése a AssetFile adatai kapcsolatban a multimédiás fájl (mint a témakörnek a későbbi szakaszában látható). 

**HTTP-kérés**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-válasz**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>A AccessPolicy létrehozása írási engedélye. 

Fájlok feltöltése az blob-tárolóhoz, előtt házirend jogok tárgyi eszköz írandó az access beállítását. Megtennie, tartalmakat tehet közzé, a AccessPolicies entitás állítsa a HTTP felkérés. Létrehozás után DurationInMinutes értékének meghatározása vagy vissza válaszként kap egy 500 belső kiszolgáló hibaüzenet jelenik meg. AccessPolicies kapcsolatos további tudnivalókért olvassa el a [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx)című témakört.

A következő példa bemutatja, hogyan hozhat létre egy AccessPolicy:
        
**HTTP-kérés**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-kérés**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

###<a name="get-the-upload-url"></a>A feltöltés URL-cím beolvasása

Szeretne kapni a tényleges feltöltés URL-CÍMÉT, hozzon létre egy Társítások megnevezés. Locator meghatározása a Kezdés időpontja és a kapcsolat végpontjának típusú fájlok eszköz elérni kívánt ügyfelek számára. Az adott AccessPolicy és eszköz két másik ügyfél-összehívások és igényeinek kezelése több megnevezés entitás hozhat létre. A kezdő időpont értékét, valamint a AccessPolicy DurationInMinutes értékét minden e Locator használata egy URL-cím használható idejének hosszát határozza meg. További tudnivalókért olvassa el a [Megnevezés](http://msdn.microsoft.com/library/azure/hh974308.aspx)című témakört.


Társítások URL-címet a következő formátumban van:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Néhány érvényesek:

- Egy adott eszköz társított egyszerre legfeljebb öt egyedi Locator nem lehet. További tudnivalókért olvassa el a megnevezés című témakört.
- Ha szeretne közvetlenül töltse fel fájljait, beállíthatja a kezdő időpont értékét öt perc, az aktuális idő után. Ennek az oka lehet óra ferdeség az ügyfélszámítógép és a Media Services között. A kezdő időpont értéket kell is a következő DateTime formátumban: YYYY-MM-DDTHH:mm:ssZ (például "2014-es-05-23T17:53:50Z").   
- Egy 30-40 második lehet késleltetése használható esetén egy megnevezés létrehozását követően. Ez a probléma Társítások URL-cím és a Locator Érvényességé vonatkozik.

Az alábbi példa mutatja hozzon létre egy Társítások URL-Megnevezés ("1" egy Társítások megnevezés) és a "2"-On tárolt origin megnevezés összehívás törzsébe írja be tulajdonságban meghatározott. A visszaadott **Path** tulajdonság URL-címet kell használnia a töltse fel a fájlt tartalmazza.
    
**HTTP-kérés**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-válasz**

Ha sikeres, a következő válasz adja vissza:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Fájl feltöltése egy blob-tároló tárolóba
    
Ha befejezte a AccessPolicy és megnevezés beállítása, az Azure Blob-tárolóhoz tárolóhoz az Azure tároló REST API-k használata a tényleges fájlt töltenek fel. Töltse fel a lapra, vagy BLOB blokkolni. 

>[AZURE.NOTE] Hozzá kell adnia a fájlnevet a fájlnév fel szeretné tölteni az előző szakaszban kapott megnevezés **elérési út** értékre. Ha például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Tárterület Azure BLOB-használata a további tudnivalókért lásd: [Blob szolgáltatás REST API -t](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>A AssetFile frissítése 

Most, hogy feltöltötte a fájljait, frissítése a FileAsset méret (és másik). Példa:
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-válasz**

Ha sikeres, a következő adja vissza: a HTTP/1.1 204 Nincs tartalom

### <a name="delete-the-locator-and-accesspolicy"></a>A kereső- és AccessPolicy törlése 

**HTTP-kérés**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP-válasz**

Ha sikeres, az alábbi adja vissza:

    HTTP/1.1 204 No Content 
    ...

**HTTP-kérés**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP-válasz**

Ha sikeres, az alábbi adja vissza:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Töltse fel a tömeges eszközök

###<a name="create-the-ingestmanifest"></a>A IngestManifest létrehozása

A IngestManifest az eszközök, fájlok eszköz és határozza meg a készlet ingesting tömeges végrehajtásának használható statisztikai adatokat tároló.


**HTTP-kérés**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Eszközök létrehozása

Mielőtt hoz létre a IngestManifestAsset, az eszköz, amely megy végbe, használja a tömeges ingesting létrehozásához szükséges. Egy tárgyi eszköz tároló típusú vagy a Media Services, beleértve a videó a hang, képek, miniatűrök gyűjtemények, szöveg-s zeneszámok fájlok, és kódolt felirat objektumok csoportja. A REST API-eszköz létrehozása és elő kell készítenie HTTP POST kérelem küld a Microsoft Azure Media Services úgy, hogy az eszköz minden tulajdonság információt összehívás törzsébe. Ebben a példában az eszköz alapján hoz létre a összehívás törzsébe részét képező StorageEncrption(1) lehetőséget.

**HTTP-válasz**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>A IngestManifestAssets létrehozása

A tömeges ingesting használt eszközök belül egy IngestManifest IngestManifestAssets jelenítik meg. A hivatkozás alapvetően az eszköz a jegyzék. Azure Media Services figyeli belső a többi fájlfeltöltéshez az IngestManifestAsset társított IngestManifestFiles webhelycsoport alapján. Ha ezek a fájlok feltöltése befejeződik, az eszköz, nincs befejezve. HTTP POST kérelem hozhat létre egy új IngestManifestAsset. A kérelem törzsébe írja be a IngestManifest és a digitáliseszköz-azonosító, amely a IngestManifestAsset kell összekapcsolása a tömeges ingesting.

**HTTP-válasz**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>A IngestManifestFiles létrehozása az egyes eszközök

Egy IngestManifestFile tényleges video- vagy blob-objektum, hogy az eszköz ingesting tömeges részeként feltöltődnek jelöli. Titkosítási kapcsolódó tulajdonságok használatuk nem kötelező, kivéve ha az eszköz használ egy titkosító lehetőséget. Az ebben a részben használt példa bemutatja, hogy a korábban létrehozott eszköz használó StorageEncryption egy IngestManifestFile létrehozása.


**HTTP-válasz**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>A fájlok feltöltése a Blob-tárolóhoz

Bármelyik alkalmas a digitáliseszköz-fájlok feltöltése a Uri a IngestManifest BlobStorageUriForUpload tulajdonsága által biztosított blob-tároló tároló nagy sebességű ügyfélalkalmazás is használhatja. Egy nagy sebességű főbb feltöltés szolgáltatása [Aspera igény szerinti Azure alkalmazáshoz](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Monitor tömeges Ingest folyamatban

Figyelheti a tömeges műveleteket egy IngestManifest ingesting által a IngestManifest statisztika tulajdonsága lekérdezési elért haladás. Ez a tulajdonság összetett típusú [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). A statisztikai tulajdonság lekérdezik, kérelem HTTP GET átadása a IngestManifest azonosítóját.
 

##<a name="create-contentkeys-used-for-encryption"></a>Titkosítási használt ContentKeys létrehozása

Ha az eszköz fog használni a titkosítást, a titkosítási a digitáliseszköz-fájlok létrehozása előtt használandó ContentKey kell létrehoznia. Tárterület titkosításhoz a következő tulajdonságokat összehívás törzsében szerepelnie kell.
 
Összehívás törzsébe tulajdonság   | Leírás
---|---
Azonosító | A ContentKey beszerezni, amely azt ragozott formáival készítése a következő formátumban "nb:kid:UUID:<NEW GUID>".
ContentKeyType | Ez a webhelytartalom-típus, a tartalom kulcs egész szám. Azt adja át a tárhely titkosításhoz 1 értéket.
EncryptedContentKey | Hozzunk létre egy új tartalom kulcs értéket, amely 256 bites (32 bájt) érték. A kulcs titkosított tároló titkosítási x.509-es tanúsítvány, amely azt beolvasása a Microsoft Azure Media Services hajtja végre a HTTP GET kérés GetProtectionKeyId és GetProtectionKey módszerek használatával.
ProtectionKeyId | Ez a védelem kulcs azonosítóját a titkosítási tárterület-X.509 a tartalom kulcs titkosítására használt tanúsítvány.
ProtectionKeyType | Ez az a védelem billentyűt a tartalom kulcs titkosítására használt titkosításának típusát. Ez az érték nem StorageEncryption(1) példáinkban.
Ellenőrző |A MD5 számított ellenőrző a tartalom billentyűt. Hogy titkosítja a tartalom billentyűt a tartalom azonosító számítja ki. A példa kód bemutatja, hogyan számítsa ki az ellenőrző.


**HTTP-válasz**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Az eszköz a ContentKey csatolása

HTTP POST kérelem küld a ContentKey egy vagy több eszközök társítva. A következő kérelem képen a példa ContentKey hivatkozni szeretne a példa az eszköznek azonosítójával.

**HTTP-válasz**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP-válasz**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
