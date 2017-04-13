<properties 
    pageTitle="Első lépések a tartalom többi használatával igény továbbítása |} Microsoft Azure" 
    description="Ebben az oktatóanyagban végigvezeti a végrehajtási igény szerint a tartalomkézbesítési alkalmazás az Azure Media Services REST API-t használja." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Első lépések a tartalom többi használatával igény továbbítása 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](/pricing/free-trial/?WT.mc_id=A261C142F). 


Ez a quickstart útmutató végigvezeti a végrehajtási egy igény szerinti videó (VoD) tartalomkézbesítési alkalmazás segítségével az Azure Media Services (AMS) REST API-khoz. 

Az oktatóprogram vezet be, az egyszerű Media Services munkafolyamat és a leggyakoribb programozási objektumok és a Media Services fejlesztési szükséges feladatokat. Az oktatóanyag végén fogja tudni adatfolyam vagy fokozatosan feltölteni, kódolt és letöltött minta multimédiás fájl letöltése.  

## <a name="prerequisites"></a>Előfeltételek
Az alábbi előfeltételek fejlesztés a Media Services REST API-khoz indításához szükséges.

- Media Services REST API-val fejlesztésével megértése. További információt a [media-szolgáltatások-többi-áttekintése](http://msdn.microsoft.com/library/azure/hh973616.aspx)című témakörben találhat.
- A választási lehetőségek, elküldheti a HTTP-összehívások és válaszok alkalmazását. Ebben az oktatóanyagban [Fiddler](http://www.telerik.com/download/fiddler)használja. 

Az alábbi műveleteket a quickstart útmutató jelennek meg.

1.  Fiók hozzárendelése Media Services (az Azure portál használatával).
2.  Állítsa be a továbbított végpont (az Azure portál használatával).
1.  Csatlakozás a Media Services fiók REST API-val.
1.  Hozzon létre egy új eszközt, és töltse fel a REST API-val videofájlra.
1.  Állítsa be a továbbított egységek REST API-val.
2.  A forrásfájl kódolását be adaptív átviteli sebesség MP4 fájlokat REST API-val.
1.  Az eszköz és fokozatos letöltési URL REST API-val és a folyamatos átvitelű get közzététele. 
1.  A tartalom lejátszása. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Az Azure portálon Azure Media Services fiók létrehozása

Ez a szakasz lépései bemutatják, hogyan hozhat létre AMS fiókot.

1. Jelentkezzen be a az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+ Új** > **Media + CDN** > **Media Services**.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. A **MEDIA SERVICES-fiók létrehozása** adja meg a szükséges értékeket.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. A **Fiók neve**mezőbe írja be az új AMS fiók nevére. A Media Services fióknév összes kisbetűs számokat vagy betűket, szóközök nélkül, és 3-24 karakter hosszú.
    2. Az előfizetését a különböző Azure-előfizetések listáját van hozzáférése közül választhat.
    
    2. Válassza az új vagy meglévő erőforrás **Erőforráscsoport**.  Erőforráscsoport információforrások, amelyek életciklus, engedélyeket és házirendeket megosztása gyűjteménye. További tudnivalók [az alábbi](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Helyét**jelölje ki a földrajzi területhez tartozik használják a média és metaadatok rekordokat a Media Services fiók tárolásához. Ez a terület folyamat, és a multimédia-adatfolyam szolgál. A rendelkezésre álló Media Services régiók jelennek meg a legördülő listából. 
    
    3. **Tárterület-fiókot**válassza ki a tárhely adja meg a Media Services fiókjából médiatartalom blob-tárolóhoz. Kijelölhet egy meglévő tárterület-fiókot a Media Services fiókként ugyanabban a földrajzi régióban, vagy létrehozhat egy tárterület-fiókkal. Új tároló fiók ugyanabban a régióban jön létre. Tárterület fióknevét szabályairól megegyeznek a Media Services fiókok.

        További tudnivalók a tárhely [Itt](storage-introduction.md).

    4. Válassza a **Irányítópult PIN-kódot** a fiók telepítési állapotának megtekintéséhez.
    
7. Kattintson a **Create** a képernyő alján.

    Miután sikeresen létrejön a fiók, az állapot **futó**változik. 

    ![Multimédia-szolgáltatások beállításai](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS fiókjának kezelését szeretné (például videó feltöltése, kódolását eszközök, figyelheti a projekt előrehaladásának) a **Beállítások** ablakban.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Az Azure portálon adatfolyam végpontok konfigurálása

Amikor az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség videofunkcióinak elkötelezett a leggyakoribb esetek egyike Azure Media Services használata. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak).

Dinamikus csomagolása, amely lehetővé teszi a adaptív átviteli sebesség kódolt MP4 tartalmának nélkül ezeknek a folyamatos átvitelű formátumok előre csomagolt változatának tárolni a Media Services (MPEG kötőjel HLS, zökkenőmentes Streaming, HDS) csak igény, által támogatott streaming előadásához Media Services ismertetése

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- Emeletes (forrás) fájlját kódolását be adaptív átviteli sebesség MP4-fájlokat (a kódolási lépéseket mutatni a később az oktatóprogram vannak).  
- Hozzon létre egy adatfolyam mennyisége, a *folyamatos átvitelű végpontot* , amelyhez használni kíván kézbesítési a tartalom. Az alábbi lépésekkel bemutatják, hogyan módosíthatja az adatfolyam egységek számát.

Dinamikus csomagolása, csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services hoz létre, és a megfelelő választ, egy ügyfél érkező kérések alapján szolgál.

Szeretne létrehozni, és módosítsa a folyamatos átvitelű fenntartott egységek számát, tegye a következőket:


1. Kattintson a **Beállítások** ablak **adatfolyam végpontok**. 

2. Kattintson az alapértelmezett streaming végpontot. 

    A **Folyamatos ÁTVITELŰ VÉGPONT ADATAIT alapértelmezett** ablak.

3. Adja meg az adatfolyam egységek számát, húzza a **adatfolyam egységek** csúszkát.

    ![A folyamatos átvitelű erőforrás-mennyiség](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kattintson a **Mentés** gombra a módosítások mentéséhez.

    >[AZURE.NOTE]A felosztás minden új egységek is legfeljebb 20 percet igénybe vehet.

## <a id="connect"></a>A Media Services-fiókomhoz REST API-val

Két dolgot szükségesek, ha az Azure Media Services elérése: egy jogkivonat által biztosított Azure Access vezérlő szolgáltatások (ACS), és a Media Services URI magát. Azt szeretné, hogy ezek a kérelmek létrehozásakor, mindaddig, amíg a megfelelő élőfejet értéket, és átadásakor a hozzáférési jogkivonat megfelelően hívásakor Media Services módon is használhatja.

Az alábbi lépések leírják a leggyakoribb munkafolyamatot, a Media Services REST API-t a Media Services csatlakozni:

1. Az első egy hozzáférési jogkivonat. 
2. Csatlakozás a Media Services URI.  

    Ne feledje, hogy https://media.windows.net sikeres kapcsolódás után kap a 301 átirányítást másik Media Services URI megadása. Az új URI-ra, további hívások kell végeznie. Az ODATA-API-metaadatok leírását tartalmazó HTTP/1.1 200 választ jelenhet meg.
3. Bejegyzés írása a későbbi API-hívások az új URL-címet. 
    
    Ha például ha az csatlakozni, miután szerezte be az alábbi:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    A későbbi API hívások https://wamsbayclus001rest-hs.cloudapp.net/api/ kell közzé.

###<a name="getting-an-access-token"></a>Az első egy hozzáférési jogkivonat

Közvetlenül a REST API Media Services eléréséhez egy hozzáférési jogkivonat lekérése ACS, és minden HTTP kérés, akkor készítsen a szolgáltatás során használni. Egyéb vonatkozó követelmények nem szükséges, hogy közvetlenül a Media Services való csatlakozás előtt.

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

A kérés; törzsében client_id és client_secret értékeket igazolni kell client_id és client_secret felel meg a fióknév és AccountKey értéket, illetve. Ezeket az értékeket az Ön által nyújtott Media Services a fiók beállításakor. 

URL-címként kódolt a Media Services fiókjának AccountKey kell lennie, amikor használja a hozzáférési jogkivonat kérelem client_secret értékeként.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Egy külső tárolóhoz "access_token" és "expires_in" értékeket gyorsítótárban ajánlott. A token adatok sikerült olvassa be a tár és később újra használni a Media Services REST API-hívások. Az esetek, amikor a token biztonságosan megosztható folyamatok vagy a számítógépek között különösen hasznos.

Ellenőrizze, hogy figyelheti a "expires_in" a hozzáférési jogkivonat értékét, és szükség szerint frissítse az új tokenek a REST API-hívások.

###<a name="connecting-to-the-media-services-uri"></a>Kapcsolódás a Media Services URI

A legfelső szintű Media Services URI https://media.windows.net/. Az eredetileg a URI csatlakozzon, és ha a 301 átirányítást vissza válaszként, gondoskodnia kell az új URI-ra, további hívások. Ezeken kívül ne használjon olyan automatikus-átirányítási/követés logika az igényeinek. HTTP-műveletek és kérelem szervezetek nem továbbításra kerül az új URI-ra.

A legfelső szintű URI tölt fel és eszköz-fájlok letöltése a tárhely fióknév esetén ugyanarra a számítógépre a Media Services fiókbeállítás során használt https://yourstorageaccount.blob.core.windows.net/.

Az alábbi példa bemutatja a HTTP-kérés a Media Services legfelső szintű URI (https://media.windows.net/). A kérés megkapja a 301 átirányítást vissza választ. A későbbi kérés használ az új URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-kérés**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Ezután az új URI használatos ebben az oktatóanyagban.

## <a id="upload"></a>Hozzon létre egy új eszközt, és töltse fel a REST API-val videofájl

A Media Services a digitális fájlok eszköz be feltöltése. Az **eszköz** entitás is tartalmazhat, videó, hang, képek, miniatűrök gyűjtemények, szöveg számok és kódolt felirat fájlok (és ezek a fájlok metaadatait.)  A fájlok feltöltése az eszköz be befejeződik, amikor a tartalom tárolási biztonságosan további feldolgozásra és a folyamatos átvitelű a felhőben. 

Az értékeket, meg kell adnia a tárgyi eszköz létrehozásakor egyik eszköz létrehozási beállítások. A **Beállítások** tulajdonság értéke egy számbavétel, amely a tárgyi eszköz hozható létre a titkosítási beállításokat ismerteti. Érvényes érték a a listában nem kombinációjának a listából az alábbi értékek közül:

 
- **Nincs** = **0** - titkosítás nem használatos. Ha ezzel a beállítással a tartalom nem védett a hálózaton átvitt vagy tárolóban lévő többi.
    Ha szeretne egy MP4 előadása fokozatos letöltése, használja a használja ezt a beállítást. 
- **StorageEncrypted** = **1** - titkosítja a helyi meghajtóra titkosítással AES-256 bit törlése tartalmat, és majd feltölti azt Azure tároló tárolási helyére a többi titkosított. Tárterület-titkosítással védett eszközök automatikusan titkosítatlan és egy titkosított fájlrendszer előtt kódolást helyezett, és tetszés szerint újra titkosított egy új kimeneti eszközként feltöltése előtt. Az elsődleges használatieset az tárterület-titkosítás esetén, a kiváló minőségű beviteli médiafájlokat a többi titkosítású a lemezen védeni kívánt.
- **CommonEncryptionProtected** = **2** - használja ezt a lehetőséget, ha Ön már titkosított és látták el közös titkosítást vagy PlayReady DRM (például zökkenőmentes Streaming védett a PlayReady DRM) tartalma feltöltése.
- **EnvelopeEncryptionProtected** = **4** – ezt a lehetőséget, ha vannak feltöltése a titkosított AES HLS. A fájlok kell kódolt és átalakítás Manager titkosítja.

### <a name="create-an-asset"></a>Egy tárgyi eszköz létrehozása

Egy tárgyi eszköz tároló típusú vagy a Media Services, beleértve a videó a hang, képek, miniatűrök gyűjtemények, szöveg-s zeneszámok fájlok, és kódolt felirat objektumok csoportja. A REST API-eszköz létrehozása és elő kell készítenie POST kérelem küld a Media Services úgy, hogy az eszköz minden tulajdonság információt összehívás törzsébe.

A következő példa bemutatja, hogyan hozhat létre egy eszközt.

**HTTP-kérés**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

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
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Hozzon létre egy AssetFile

A [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) entitás video- vagy blob-tárolóban tárolt fájlt jelöl. Egy tárgyi eszköz fájl mindig társított tárgyi eszköz, és tárgyi eszköz egy vagy több AssetFiles tartalmazhat. A Media Services Encoder tevékenység sikertelen lesz, ha az eszköz fájlt objektumként program nem társítja blob-tároló digitális fájl.

Blob-tároló be a digitális eszközökkel készített tartalom fájl feltöltése, után a http- **EGYESÍTÉSE** kérés használatával kapcsolatban a multimédiás fájl (ahogy című témakör) frissítése a AssetFile adatai. 

**HTTP-kérés**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
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

Fájlok feltöltése az blob-tárolóhoz, előtt házirend jogok tárgyi eszköz írandó az access beállítását. Megtennie, tartalmakat tehet közzé, a AccessPolicies entitás állítsa a HTTP felkérés. Létrehozás után DurationInMinutes értékének meghatározása vagy egy 500 belső kiszolgáló hibaüzenetet kap válaszként vissza. AccessPolicies kapcsolatos további tudnivalókért olvassa el a [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx)című témakört.

A következő példa bemutatja, hogyan hozhat létre egy AccessPolicy:
        
**HTTP-kérés**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-válasz**

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
    X-Powered-By: ASP.NET
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

### <a name="get-the-upload-url"></a>A feltöltés URL-cím beolvasása

Szeretne kapni a tényleges feltöltés URL-CÍMÉT, hozzon létre egy Társítások megnevezés. Locator meghatározása a Kezdés időpontja és a kapcsolat végpontjának típusú fájlok eszköz elérni kívánt ügyfelek számára. Az adott AccessPolicy és eszköz két másik ügyfél-összehívások és igényeinek kezelése több megnevezés entitás hozhat létre. A kezdő időpont értékét, valamint a AccessPolicy DurationInMinutes értékének minden e Locator használja egy URL-cím használható idejének hosszát határozza meg. További tudnivalókért olvassa el a [Megnevezés](http://msdn.microsoft.com/library/azure/hh974308.aspx)című témakört.


Társítások URL-címet a következő formátumban van:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Néhány érvényesek:

- Egy adott eszköz társított egyszerre legfeljebb öt egyedi Locator nem lehet. További tudnivalókért olvassa el a megnevezés című témakört.
- Ha szeretne közvetlenül töltse fel fájljait, beállíthatja a kezdő időpont értékét öt perc, az aktuális idő után. Ennek az oka lehet óra ferdeség az ügyfélszámítógép és a Media Services között. A kezdő időpont értéket kell is a következő DateTime formátumban: YYYY-MM-DDTHH:mm:ssZ (például "2014-es-05-23T17:53:50Z").   
- Egy 30-40 második lehet késleltetése használható esetén egy megnevezés létrehozását követően. Ez a probléma Társítások URL-cím és a Locator Érvényességé vonatkozik.

Az alábbi példa mutatja hozzon létre egy Társítások URL-Megnevezés ("1" egy Társítások megnevezés) és a "2"-On tárolt origin megnevezés összehívás törzsébe írja be tulajdonságban meghatározott. A visszaadott **Path** tulajdonság URL-címet kell használnia a töltse fel a fájlt tartalmazza.
    
**HTTP-kérés**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
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
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-válasz**

Ha sikeres, a következő adja vissza: a HTTP/1.1 204 Nincs tartalom

## <a name="delete-the-locator-and-accesspolicy"></a>A kereső- és AccessPolicy törlése 

**HTTP-kérés**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP-válasz**

Ha sikeres, az alábbi adja vissza:

    HTTP/1.1 204 No Content 
    ...

**HTTP-kérés**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-válasz**

Ha sikeres, az alábbi adja vissza:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Állítsa be a továbbított egységek REST API-val

Amikor az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség elkötelezett a leggyakoribb esetek egyike Azure Media Services használata. A folyamatos átvitelű adaptív átviteli sebesség, az ügyfél válthat nagyobb vagy kisebb átviteli sebesség adatfolyam, a videó jelenik meg az aktuális hálózati sávszélesség, Processzor kihasználtsági és egyéb tényezők alapján. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak). 

Dinamikus csomagolása, amely lehetővé teszi a folyamatos átvitelű Media Services (MPEG kötőjel HLS, zökkenőmentes Streaming, HDS) által támogatott adaptív átviteli sebesség MP4 vagy zökkenőmentes adatfolyam-címként kódolt tartalmának előadásához nélkül az ezek a folyamatos átvitelű formátumok újracsomagolására Media Services ismertetése 

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- legalább egy adatfolyam egység beszerzése a **folyamatos átvitelű végpontot **, amelyhez az (a jelen szakaszban ismertetett) tartalma előadása megtervezése.
- kódolását vagy alakítható át az Emeletes (forrás) fájlt a alkalmazásba adaptív átviteli sebesség MP4 vagy adaptív átviteli sebesség zökkenőmentes adatfolyam-fájlokat (a kódolási lépéseket, később az oktatóprogram vannak igazolni), csoportja  

Dinamikus csomagolása, csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services hoz létre, és a megfelelő választ, egy ügyfél érkező kérések alapján szolgál. 


>[AZURE.NOTE] Árak részletes információt olvassa el a [Media Services árak részletei](http://go.microsoft.com/fwlink/?LinkId=275107)című témakört.

Ha módosítani szeretné a folyamatos átvitelű fenntartott egységek számát, tegye a következőket:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>A frissíteni kívánt adatfolyam végpont beszerzése

Ha például pedig hozzuk működésbe be az első adatfolyam végpontot fiókjában (állapot futó egyszerre két adatfolyam végpontok lehet).

**HTTP-kérés**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-válasz**
    
Ha sikeres, az alábbi adja vissza:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Az adatfolyam végpontot méretezése
 
**HTTP-kérés**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP-válasz**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>A hosszabb ideig futó művelet állapotának ellenőrzése

A felosztás minden új egységek körülbelül 20 perc megnyitja befejezéséhez. A művelet használata a **Műveletek** módszer állapotának ellenőrzése, és adja meg a műveletet azonosítója. A művelet azonosító visszaadott a válasz, a **Méretezés** -összehívásba.

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP-kérés**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP-válasz**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>A forrásfájl kódolását be adaptív átviteli sebesség MP4 fájlokat

Után a Media Services media eszközök is kell kódolni ingesting, transmuxed, vízjellel, és így tovább mielőtt átadása ügyfelek. Ezek a tevékenységek ütemezett és futtassanak több háttér szerepkör példányokat nagy teljesítmény és elérhetősége érdekében. Ezek a tevékenységek úgynevezett feladatokat, és minden [feladat](http://msdn.microsoft.com/library/azure/hh974289.aspx) tevődik össze atomi feladatok, amelyek a tényleges munkát a digitáliseszköz-fájlt. 

Amint volt már említettük, amikor az Azure Media Services, az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség elkötelezett a leggyakoribb esetek egyike használata. Media Services dinamikusan is csomag adaptív átviteli sebesség MP4 fájlokat be a következő formátumokban: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak). 

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- kódolását vagy alakítható át az Emeletes (forrás) fájlt a alkalmazásba adaptív átviteli sebesség MP4 vagy adaptív átviteli sebesség zökkenőmentes Streaming fájlokat, csoportja  
- Szerezze be legalább egy adatfolyam egység az adatfolyam végpontot, ahonnan a tartalmak olvasása megtervezése. 

A következő szakasz hozzon létre egy projektet, amely tartalmaz egy kódolási tevékenység mutatja. A feladat Megadja, hogy nem alakítható át az Emeletes fájl adaptív átviteli sebesség MP4s használatával **Media Encoder szabványos**alakítja. A szakasz azt is megtudhatja, hogy miként figyelheti a projekt előrehaladását feldolgozása. A feladat befejezésekor lenne létrehozása access eléréséhez kattintson az eszközök szükséges Locator. 

### <a name="get-a-media-processor"></a>A médiafájlok processzor beszerzése

A Media Services media feldolgozó összetevő, amely egy adott feldolgozási tevékenység például kódolást, konvertálás, titkosítása és titkosításának multimédiás tartalom kezeli. Ebben az oktatóanyagban látható a kódolási tevékenység fogjuk használni a Media Encoder szabványos.

A következő kódot kéri a kódoló azonosítója. 

**HTTP-kérés**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-válasz**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Feladat létrehozása

Minden feladat beállíthatja, hogy egy vagy több feladat, amely azt szeretné, hogy miként végezhet el feldolgozás típusától függően. REST API-val keresztül létrehozhat projekteken és tevékenységeken a hozzájuk kapcsolódó rendelt két módszer egyikével: tevékenységek definiált beágyazott feladat személyek a tevékenységek navigációs tulajdonságot, vagy az OData-parancsfájl keresztül is lehet. A Media Services SDK parancsfájl használja. Az ebben a témakörben mintakódok olvashatóságát, azonban a tevékenységek olyan definiált beágyazott. Parancsfájl a további tudnivalókért lásd [Adatok protokollhoz (OData-) köteg feldolgozása](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

A következő példa bemutatja, hogyan hozhat létre, és indítsa el a feladat egy tevékenység beállítása adott felbontásban és minőségű videoklip kódolását. A dokumentáció szakaszt tartalmaz a lista összes a [tevékenység készletek](http://msdn.microsoft.com/library/mt269960) Media Encoder szabványos feldolgozó által támogatott.  

**HTTP-kérés**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-válasz**

Ha sikeres, a következő válasz adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Létezik néhány fontos megjegyzés feladat kérésének:

- TaskBody tulajdonságok konstans XML kell használnia beviteli számának meghatározása, vagy a kimeneti a tevékenység által használt eszköz. A tevékenység a témakör az XML-séma definíciója az XML-hez.
- A TaskBody definícióban minden belső értéke <inputAsset> és <outputAsset> JobInputAsset(value) vagy JobOutputAsset(value) kell állítani.
- Feladat több kimeneti eszközök állhat. Egy JobOutputAsset(x) csak egyszer használható ezt az egy projektben tevékenység egy eredményt.
- Megadhatja a bemeneti tárgyi eszköz egy tevékenység JobInputAsset vagy JobOutputAsset.
- Feladatok nem kell verziókövetéssel egy űrlapot.
- Az érték paraméter JobInputAsset vagy JobOutputAsset átadandó index értékét az eszköz képviseli. A tényleges eszközök a feladat entitás meghatározása a InputMediaAssets és OutputMediaAssets navigációs tulajdonságok határozzák meg. 

>[AZURE.NOTE] Media Services OData v3 épül, mert az egyes eszközök InputMediaAssets és OutputMediaAssets navigációs tulajdonság tartozó hivatkozott keresztül egy "__metadata: uri" név – érték pár. 

- Egy vagy több eszközök Media Services létrehozott InputMediaAssets rendeli hozzá. OutputMediaAssets hozza létre a rendszer. Azok a meglévő tárgyi eszköz nem hivatkozik.
- OutputMediaAssets nevet adhat a assetName attribútum használatával. Ha az attribútum nem szerepel a, majd a OutputMediaAsset neve, függetlenül a belső szöveges értéket a <outputAsset> elem utótaggal a projekt-név oszlopban látható értéket vagy a feladat azonosítója értéket (abban az esetben, ha a Name tulajdonság nem definiálja). Például ha assetName "Minta" értéket, majd a OutputMediaAsset Name tulajdonság volna állíthatók be "Minta". Azonban assetName értéket nem adta meg, de adta meg az "NewJob" a feladat nevére, majd a OutputMediaAsset neve akkor "JobOutputAsset (érték-) _NewJob". 

    A következő példa bemutatja, hogyan kell beállítani a assetName attribútum:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Ahhoz, hogy tevékenység láncolás:

    - A feladat legalább két feladatot kell rendelkeznie.
    - Legalább egy tevékenység, amelynek bemeneti érték a feladatot egy másik feladat kimenetét kell lennie.

A további információt [a Media Services REST API-val-kódolás feladat létrehozásához](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Folyamatban feldolgozás képernyő

A feladat állapota lekérheti a State tulajdonság segítségével, az alábbi példában látható módon. 

**HTTP-kérés**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-válasz**

Ha sikeres, a következő válasz adja vissza:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Egy feladat megszakítása

Media Services lehetővé teszi a Mégse futó feladatok CancelJob funkción keresztül. A hívás egy 400 kódszámú hiba jelenik meg, ha a feladat megszakítására állapotába megszakad, amikor megpróbál, visszavonás, hibát adja eredményül, vagy elkészült.

A következő példa bemutatja, hogyan CancelJob hívja.


**HTTP-kérés**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Sikerrel jár, ha egy 204 válasz kód nem üzenettörzs az ad vissza.

>[AZURE.NOTE] Meg kell URL-kódolás a feladat azonosítója (általában nb:jid:UUID: somevalue) Ha beengedné a paraméterként CancelJob.


### <a name="get-the-output-asset"></a>Ismerkedés a kimeneti eszköz 

A következő kódrészlet szemlélteti, hogyan kell a kimeneti eszköz azonosítójával. 


**HTTP-kérés**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-válasz**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Az eszköz és fokozatos letöltési URL REST API-val és a folyamatos átvitelű get közzététele

Adatfolyam-vagy letöltése tárgyi eszköz, először kell "közzététel": hozzon létre egy megnevezés. Locator az eszköz tárolt fájlok hozzáférést biztosít. Media Services támogatja a kétféle Locator: OnDemandOrigin Locator, használva továbbítani a médiafájlokat (például MPEG kötőjel, HLS vagy zökkenőmentes Streaming) és az Access aláírás (Társítások) Locator, médiafájlok letöltéséhez használt.

Miután létrehozta a Locator, az URL-adatfolyam-vagy a fájlok letöltése hozhat létre. 


Adatfolyam URL-zökkenőmentes Streaming formátuma a következő:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Adatfolyam URL-HLS formátuma a következő:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-kötőjel adatfolyam URL-CÍMÉT az alábbi formátumban van:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Fájlok letöltése használt Társítások URL-címet a következő formátumban foglalja magában:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Ebben a szakaszban mutatja az alábbi feladatok elvégzéséhez szükséges "közzététel" eszközeit.  

- Olvasási engedélyt a AccessPolicy létrehozása 
- Társítások URL-cím létrehozása a tartalom letöltése 
- Adatfolyam-tartalmat az origin URL létrehozása 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Olvasási engedélyt a AccessPolicy létrehozása

Le, illetve a folyamatos átvitelű bármely médiatartalom előtt először megadása egy AccessPolicy olvasási engedéllyel rendelkező, és hozza létre az adja meg a ügyfelei számára engedélyezni szeretné a kézbesítési szerkezet megnevezés megfelelő egység. A Tulajdonságok érhető el a további tudnivalókért lásd: [AccessPolicy entitás tulajdonságai](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

A következő példa bemutatja a AccessPolicy az egy adott eszköz olvasási engedélyeinek megadása.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Sikerrel jár, ha egy 201 sikeres kódot ad vissza, az Ön által létrehozott AccessPolicy entitás leíró. Ezt követően az együtt az eszközkód az eszköz, amely tartalmazza a kívánt fájlt, a megnevezés entitás létrehozása (például a kimeneti tárgyi eszköz) előadásához AccessPolicy azonosítója.

>[AZURE.NOTE]
Egyszerű munkafolyamat megegyezik egy fájl feltöltése, amikor ingesting tárgyi eszköz (mint lett című témakörben tárgyalt). Is való fájlfeltöltéskor, például ha Ön (vagy az ügyfelek) közvetlenül a fájlok eléréséhez szükséges, adja meg a kezdő időpont értéket öt perc, az aktuális idő után. Ez a művelet szükség, mert előfordulhatnak óra ferdeség az ügyfél és a Media Services között. A kezdő időpont értéket kell lennie az alábbi DateTime formátumban: YYYY-MM-DDTHH:mm:ssZ (például "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Társítások URL-cím létrehozása a tartalom letöltése 

A következő kód szemlélteti, hogyan hozhatja létre, és a korábban feltöltött médiafájl letöltéséhez használható URL. Engedélyek beállítása a AccessPolicy olvasási, és megnevezés elérési utal Társítások letöltési URL-címet.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Ha sikeres, a következő válasz adja vissza:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


A visszaadott **Path** tulajdonság a Társítások URL-CÍMÉT tartalmazza.

>[AZURE.NOTE]
Ha titkosított tároló tartalomletöltés, kell manuálisan visszafejteni előtt meg, vagy használja a tárhely visszafejtés MediaProcessor feldolgozott fájlok törlése egy OutputAsset a kimeneti, és töltse le ezt az eszközt a feldolgozási tevékenység. Feldolgozási kapcsolatos további tudnivalókért olvassa el a-kódolás feladat létrehozása a Media Services REST API-val című témakört. Társítások URL-cím Locator nem lehet frissíteni, miután hozott létre. A kezdő időpont frissített értékkel azonos megnevezés például nem lehet újra felhasználni. Ez a Társítások URL-címek létrehozott módja miatt. Szeretne hozzáférni a letöltési tárgyi eszköz egy megnevezés lejárta után, ha majd, létre kell hoznia egy új kezdő időpont egy újat.

###<a name="download-files"></a>Fájlok letöltése

Ha befejezte a AccessPolicy és megnevezés beállítása, letöltheti az Azure tároló REST API-k használata a fájlok.  

>[AZURE.NOTE] Hozzá kell adnia a fájlnevet, a fájl, amelyet szeretne tölteni az előző szakaszban kapott megnevezés **elérési út** értékre. Ha például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Tárterület Azure BLOB-használata a további tudnivalókért lásd: [Blob szolgáltatás REST API -t](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Eredményeként korábbi (kódolás be adaptív MP4 beállítása) végrehajtott kódolási feladat Ha több MP4-fájlokká fokozatosan letöltheti. Példa:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Adatfolyam-tartalom adatfolyam URL-cím létrehozása


A következő kódot jeleníti meg, hogyan hozhat létre egy URL-cím adatfolyam megnevezés:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Ha sikeres, a következő válasz adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Adatfolyam-zökkenőmentes Streaming origin URL-cím egy adatfolyam médialejátszót, az elérési út tulajdonság neve folyamatos zökkenőmentes fájlt, majd cikkét "/ cikkét" írjon mögé.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Adatfolyam-HLS, hogy a Hozzáfűzés (formátum m3u8-aapl =) után a "/ cikkét".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Adatfolyam-MPEG kötőjel, a Hozzáfűzés (formátum mpd – idő-csf =) után a "/ cikkét".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>A tartalom lejátszása  

Adatfolyam-, videó, használja az [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Tesztelje a progresszív letöltésére, illessze be egy URL-CÍMÉT (például IE, Chrome, Safari) böngészőben.


##<a name="next-steps-media-services-learning-paths"></a>Következő lépések: A Media Services tanulási javaslatok

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Mást keres?

Ha ez a témakör nem tartalmaznak, mi, várt, valamit, amit hiányzik, és a más módon nem felel meg az igényeinek, és adja meg az alábbi Disqus szál használatával visszajelzését.



