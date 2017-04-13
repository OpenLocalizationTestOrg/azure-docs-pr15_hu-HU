<properties 
    pageTitle="Eszköz kézbesítési házirendek Media Services REST API segítségével beállítása |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan konfigurálható a különböző eszköz kézbesítési házirendek Media Services REST API-t használja." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>A digitális eszköz kiválasztása kézbesítési házirendek beállítása

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Ha dinamikusan titkosított eszközök előadásához, az egyik lépés a Media Services tartalomkézbesítési munkafolyamat van konfigurálása kézbesítési házirendek eszközök. Az eszköz kézbesítési házirend alapján Media Services hogyan szeretné az eszközre, kézbesítése: be kell a digitális eszköz kiválasztása lehet dinamikusan csomagolt (az például MPEG kötőjel, HLS, zökkenőmentes Streaming vagy az összes), attól függetlenül, hogy az eszköz dinamikusan titkosítani kívánt, és hogyan mely streaming jegyzőkönyv (a borítékon vagy a közös titkosítási).

Ez a témakör azt ismerteti, hogy miért és hogyan hozhat létre és eszköz kézbesítési házirendek beállítása.

>[AZURE.NOTE]Dinamikus csomagolást és dinamikus titkosítást tudja gondoskodnia arról, hogy legalább egy időosztás egységet (más néven adatfolyam egység). További információért tájékozódhat [skála Media szolgáltatás](media-services-portal-manage-streaming-endpoints.md).
>
>Az eszköz is adaptív átviteli sebesség MP4s vagy adaptív átviteli sebesség zökkenőmentes Streaming fájlok kell tartalmaznia.

Az azonos eszköz sikerült különböző szabályok vonatkoznak. Például PlayReady titkosítási sikerült alkalmazása zökkenőmentes Streaming és AES boríték titkosítási MPEG szaggatott és HLS. Bármely kézbesítési házirend nem meghatározott protokollok (például felvett egy egyetlen házirendet, amely csak a protokolljaként adja meg HLS) letiltja a folyamatos átvitelű. Ez a kivétel ez alól nincs eszköz kézbesítési házirend egyáltalán definiált esetén. Ezt követően törölje minden protokoll engedélyezett.

Ha szeretne egy tároló titkosított eszköz olvasása, meg kell adnia az eszköz kézbesítési házirend. Az eszköz folyamatosan lejátszott is kell, mielőtt a továbbított kiszolgáló eltávolítja a tárterület-titkosítás, és adatfolyamként továbbítja a tartalom, a megadott kézbesítési házirend használata. Például az eszköz speciális Encryption Standard (AES) boríték titkosítási kulcs titkosított előadásához, állítsa a házirend típusa **DynamicEnvelopeEncryption**. Tárterület titkosításának eltávolítása és törlése a eszköz adatfolyam, állítsa a házirend típusa **NoDynamicEncryption**. Az alábbi példák, amelyek bemutatják, hogyan állítsa be ezeket a házirend-típusokat.

Az eszköz kézbesítési házirend beállításaitól függően az esetben is dinamikusan csomagolása, dinamikusan titkosítása és a következő adatfolyam protokollok adatfolyam: folyamatos zökkenőmentes, HLS, MPEG szaggatott és HDS adatfolyam megjelenítését.

A következő lista mutatja a formátumok sima, HLS, szaggatott és HDS adatfolyamként való használható.

Zökkenőmentes Streaming:

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-KÖTŐJEL

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Tárgyi eszköz közzététele és adatfolyam URL-cím létrehozása című cikkben olvashat [összeállítása adatfolyam URL-címet](media-services-deliver-streaming-content.md).


##<a name="considerations"></a>Megfontolandó szempontok

- Egy tárgyi eszköz társított, miközben egy (adatfolyam) OnDemand megnevezés létezik az adott eszköz AssetDeliveryPolicy nem törölhetők. Az ajánlási a házirendet eltávolítása az eszköz a házirend törlése előtt.
- Egy adatfolyam megnevezés nem hozható létre a tárhely titkosított eszköz nincs eszköz kézbesítési házirend beállítása esetén.  Ha az eszköz nem titkosított tárhely, a rendszer közli, hozzon létre egy megnevezés, valamint a eszköz törlése nélkül az eszköz kézbesítési házirend-adatfolyam.
- Lehet, hogy több eszköz kézbesítési házirendek egyetlen eszköz társított, de csak adhatja meg lehet egy adott AssetDeliveryProtocol kezelni.  Ami azt jelenti, ha megpróbál adja meg a hibát eredményez, mivel a rendszer nem tudja, amely egy szeretne alkalmazni, amikor egy ügyfél zökkenőmentes Streaming kérelmet AssetDeliveryProtocol.SmoothStreaming protokollnak két kézbesítési házirendek hivatkozás.
- Tárgyi eszköz egy meglévő adatfolyam megnevezés, ha, nem egy új házirendet csatolása az eszköz, az eszköz a meglévő házirend leválasztása vagy frissítése az eszköz társított kézbesítési házirend.  Először távolítsa el a továbbított megnevezés, módosítsa a házirendek és hozza létre a továbbított megnevezés van.  Az azonos locatorId is használhatja, amikor a továbbított megnevezés hozza létre ismét, de győződjön meg arról, hogy nem problémákat okoznak ügyfélalkalmazások óta tartalom gyorsítótárba helyezhető az origin vagy a későbbi CDN.

>[AZURE.NOTE] Amikor a Media Services REST API-val, a következő érvényesek:
>
>A Media Services szervezetek elérésekor a be kell állítani a HTTP-összehívásokban adott fejlécmezők és értékek. További tudnivalókért olvassa el a [Media Services REST API -val fejlesztési beállítása](media-services-rest-how-to-use.md)című témakört.

>Https://media.windows.net sikeres kapcsolódás után jelenik meg a 301 átirányítást másik Media Services URI megadása. A [Csatlakozás a Media Services REST API segítségével](media-services-rest-connect-programmatically.md)leírtak szerint új URI későbbi hívásainak kell végeznie.


##<a name="clear-asset-delivery-policy"></a>Eszköz törlése kézbesítési házirend

###<a id="create_asset_delivery_policy"></a>A digitális eszköz kiválasztása kézbesítési házirend létrehozása
A következő HTTP-kérés házirendet hoz létre a digitális eszköz kiválasztása kézbesítési arról, hogy nem vonatkoznak a dinamikus titkosítást és előadásához az adatfolyam bármely, az alábbi protokollokat: MPEG kötőjel HLS és zavartalan Streaming protokollok. 

Milyen értékeket szeretne megadhat egy AssetDeliveryPolicy létrehozásakor című [típusok AssetDeliveryPolicy megadásakor használható](#types) .   


A kérelem:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Válasz:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Az eszköz kézbesítési házirend hivatkozás eszköz

A következő HTTP-kérés csatol eszköz kézbesítési házirendet, amely a megadott eszköz.

A kérelem:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Válasz:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption eszköz kézbesítési házirend 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Hozza létre a EnvelopeEncryption típusú tartalom kulcsot, és csatolja az eszköz

DynamicEnvelopeEncryption kézbesítési házirend megadása esetén ügyeljen arra, hogy az eszköz csatolása EnvelopeEncryption típusú tartalom kulcs van szükség. További tudnivalókért lásd: [létrehozása a tartalom billentyűt](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Kézbesítési URL-cím beolvasása

Szerezze be a tartalom billentyűt az előző lépésben létrehozott megadott kézbesítési módszer kézbesítési URL-CÍMÉT. Ügyfél-AES kulcs igénylése használja a visszaadott URL-CÍMÉT, vagy egy PlayReady licenc sorrendben, a lejátszás a védett tartalmat.

Adja meg az URL-címe beszerzése a HTTP-összehívás törzsében. Ha védelmére PlayReady, a tartalom kérése a Media Services PlayReady licenc WIA URL-címet, 1 használata a keyDeliveryType: {"keyDeliveryType": 1}. Ha a tartalom, a boríték titkosítással védelmén kérése kulcs WIA URL-cím 2 keyDeliveryType megadásával: {"keyDeliveryType": 2}.

A kérelem:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Válasz:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>A digitális eszköz kiválasztása kézbesítési házirend létrehozása

A következő HTTP-kérés hoz létre a **AssetDeliveryPolicy** van konfigurálva, hogy dinamikus boríték titkosítási (**DynamicEnvelopeEncryption**) alkalmazása a **HLS** protocol (ebben a példában más protokollok, azzal blokkolhatja a folyamatos átvitelű). 


Milyen értékeket szeretne megadhat egy AssetDeliveryPolicy létrehozásakor című [típusok AssetDeliveryPolicy megadásakor használható](#types) .   

A kérelem:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Válasz:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Az eszköz kézbesítési házirend hivatkozás eszköz

Lásd: [az eszköz kézbesítési házirend hivatkozás eszköz](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption eszköz kézbesítési házirend 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Hozza létre a CommonEncryption típusú tartalom kulcsot, és csatolja az eszköz

DynamicCommonEncryption kézbesítési házirend megadása esetén ügyeljen arra, hogy az eszköz csatolása CommonEncryption típusú tartalom kulcs van szükség. További tudnivalókért lásd: [létrehozása a tartalom billentyűt](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Kézbesítési URL-cím beolvasása

Szerezze be a tartalom billentyűt az előző lépésben létrehozott PlayReady kézbesítési módszer kézbesítési URL-CÍMÉT. Egy ügyfél sorrendben, a lejátszás a védett tartalmat PlayReady licenc kérése a visszaadott URL-címet használja. További tudnivalókért lásd: az [Első kézbesítési URL-CÍMÉT](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>A digitális eszköz kiválasztása kézbesítési házirend létrehozása

A következő HTTP-kérés hoz létre a **AssetDeliveryPolicy** van konfigurálva, hogy dinamikus közös titkosítást (**DynamicCommonEncryption**) alkalmazása a **Zökkenőmentes Streaming** protocol (ebben a példában más protokollok, azzal blokkolhatja a folyamatos átvitelű). 

Milyen értékeket szeretne megadhat egy AssetDeliveryPolicy létrehozásakor című [típusok AssetDeliveryPolicy megadásakor használható](#types) .   


A kérelem:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


A tartalomhoz Widevine DRM védelemmel ellátni kívánt, frissítse AssetDeliveryConfiguration értékekkel kell beállítani a WidevineLicenseAcquisitionUrl (amely a 7 érték van), majd adja meg a licenc kézbesítési szolgáltatás URL-CÍMÉT. Használhatja a következő AMS partnerek segít előadása Widevine licencek: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Példa: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Widevine titkosításához volna csak is használhat az előadáshoz kötőjel használatával. Ellenőrizze, hogy az eszköz kézbesítési protokoll kötőjel (2) adhat meg.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Az eszköz kézbesítési házirend hivatkozás eszköz

Lásd: [az eszköz kézbesítési házirend hivatkozás eszköz](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Adattípusainak AssetDeliveryPolicy megadásakor

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
