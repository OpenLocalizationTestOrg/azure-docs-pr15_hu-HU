<properties 
    pageTitle="A tartalmat tároló titkosítással AMS REST API segítségével titkosítása" 
    description="Megtudhatja, hogy miként titkosíthatja a tartalom tároló titkosítással AMS REST API-k használata." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>A tartalmat tároló titkosítással AMS REST API segítségével titkosítása

A tartalom helyileg titkosítással AES-256 bit titkosítása, és töltse fel a Azure tárolóhoz hol kíván tárolni a többi titkosított erősen ajánlott.

Ez a cikk áttekintést AMS tároló titkosítási, és megtudhatja, hogy miként tölthet fel a titkosított tároló tartalom:

- Hozzon létre egy tartalom billentyűt.
- Hozzon létre egy eszközt. Az eszköz létrehozásakor, állítsa a AssetCreationOption StorageEncryption.

     Titkosított eszközök kell társítani tartalom billentyűk.
- Hivatkozás az eszköz a tartalom billentyűt.  
- Állítsa a titkosítási kapcsolódó AssetFile szervezetek a paraméterek.
 
>[AZURE.NOTE]Ha szeretne egy tároló titkosított eszköz olvasása, meg kell adnia az eszköz kézbesítési házirend. Az eszköz folyamatosan lejátszott is kell, mielőtt a továbbított kiszolgáló eltávolítja a tárterület-titkosítás, és adatfolyamként továbbítja a tartalom, a megadott kézbesítési házirend használata. További tudnivalókért lásd: az [Eszköz kézbesítési házirendek beállítása](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Amikor a Media Services REST API-val, a következő érvényesek:
>
>A Media Services szervezetek elérésekor a be kell állítani a HTTP-összehívásokban adott fejlécmezők és értékek. További tudnivalókért olvassa el a [Media Services REST API -val fejlesztési beállítása](media-services-rest-how-to-use.md)című témakört.

>Https://media.windows.net sikeres kapcsolódás után jelenik meg a 301 átirányítást másik Media Services URI megadása. A [Csatlakozás a Media Services REST API segítségével](media-services-rest-connect-programmatically.md)leírtak szerint új URI későbbi hívásainak kell végeznie. 

##<a name="storage-encryption-overview"></a>Tároló titkosítás – áttekintés 

A AMS tároló titkosítás **AES-Parancsra** mód titkosítási vonatkozik a teljes fájlt.  AES-Parancsra módja egy blokk titkosítás, amely titkosíthatja tetszőleges hosszúságú adatok belső margóinak nélkül. Hogy titkosítja a számláló blokk AES algoritmus, majd a kimenet AES az adatokat tartalmazó XVAGY-ing titkosítása és visszafejtése működik.  A InitializationVector értékének másolása a számláló értéke 0 és 7 bájt által használt számláló Tiltás összeállítás, és a számláló értéket 8-15 bájt értéke nulla. A 16 bájtos számláló szövegrészt 8 – 15 (azaz a legkevésbé fontos bájt) bájt használják egy egyszerű 64 bites alá nem írt egész számra, amely az eggyel az egyes későbbi adatblokk feldolgozása, és a hálózati bájt sorrendben legyen. Ne feledje, hogy a beállítást, ha ezt az egész számot elér a legnagyobb érték (0xFFFFFFFFFFFFFFFF) alaphelyzetbe állítása növekvő a továbbfejlesztett fájlblokkolás számláló nulla (bájt 8-15) 64 bit megtartásával a számláló (azaz a 0 és 7 bájt).   Biztonsága érdekében a AES-Parancsra mód titkosítást, egy adott kulcs azonosító minden tartalom billentyű InitializationVector érték kell lennie az egyes fájlok egyedi, és a fájlok kell kevesebb mint 2 ^ 64 blokkok hossza.  Ez a annak érdekében, hogy egy számláló értéke soha nem újrafelhasználható adott használatával. A Parancsra mód kapcsolatos további tudnivalókért olvassa el a [a wikilap](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (a wikicikk használja a kifejezés "Nonce" helyett "InitializationVector") című témakört.

A helyi meghajtóra titkosítással AES-256 bit törlése tartalom titkosítása **Tárterület-titkosítás** használatára, és töltse fel a Azure tárolóhoz tárolási helyére a többi titkosítva. Tárterület-titkosítással védett eszközök automatikusan titkosítatlan és egy titkosított fájlrendszer előtt kódolást helyezett, és tetszés szerint újra titkosított egy új kimeneti eszközként feltöltése előtt. Az elsődleges használatieset az tárterület-titkosítás esetén, a kiváló minőségű beviteli médiafájlokat a többi titkosítású a lemezen védeni kívánt.

Annak érdekében, hogy a tárhely titkosított eszköz olvasása, meg kell adnia az eszköz kézbesítési házirendet, így Media Services megtudhatja, hogyan kívánja-e a tartalmak olvasása. Az eszköz folyamatosan lejátszott is kell, mielőtt a továbbított kiszolgáló eltávolítja a tárterület-titkosítás, és adatfolyamként továbbítja a tartalom a házirenddel megadott kézbesítési (például AES, gyakori titkosítási vagy titkosítás nélkül).

##<a name="create-contentkeys-used-for-encryption"></a>Titkosítási használt ContentKeys létrehozása

Titkosított eszközök kell társítható tároló titkosítási kulcs. A digitáliseszköz-fájlok létrehozása előtt titkosításhoz használandó tartalom kulcs létre kell hoznia. Ez a szakasz ismerteti, hogyan tartalom kulcs létrehozásához.

Az alábbi lépések általános társíthatja az eszközök, amely titkosítani kívánt tartalom kulcsokat létrehozásához. 

1. Tárterület titkosításhoz véletlenszerűen 32 bájtos AES kulcs generálása. 

    Ez az eszköz, ami azt jelenti, hogy eszköz társított összes fájlt a billentyűvel azonos tartalom visszafejtés során kell a tartalom billentyűjét lesz. 
2.  Hívja fel a megfelelő x.509-es tanúsítvány, amellyel a tartalom kulcs titkosításához első [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) és [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) módszereket.
3.  Titkosítsa a tartalom használatával a nyilvános kulccsal x.509-es tanúsítvány. 

    Media Services .NET SDK RSA során a titkosítási OAEP használja.  A .NET vett példa a [EncryptSymmetricKeyData függvény](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs)tekintheti meg.
4.  Hozzon létre egy ellenőrző értékét számítja ki a fontosabb azonosító és a tartalom kulcsot. Ez a példa .NET az ellenőrző használata a fő azonosító és a tartalom törlése kulcs globálisan egyedi azonosítója részét számítja ki.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. A **EncryptedContentKey** (base64-címként kódolt karakterláncot konvertálva), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**és az előző lépéseket kapott **ellenőrző** értékek létrehozása a tartalom billentyűt.

    
    Tárterület titkosításhoz a következő tulajdonságokat összehívás törzsében szerepelnie kell.
     
    Összehívás törzsébe tulajdonság   | Leírás
    ---|---
    Azonosító | A ContentKey beszerezni, amely azt ragozott formáival készítése a következő formátumban "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Ez a webhelytartalom-típus, a tartalom kulcs egész szám. Azt adja át a tárhely titkosításhoz 1 értéket.
    EncryptedContentKey | Hozzunk létre egy új tartalom kulcs értéket, amely 256 bites (32 bájt) érték. A kulcs titkosított tároló titkosítási x.509-es tanúsítvány, amely azt beolvasása a Microsoft Azure Media Services hajtja végre a HTTP GET kérés GetProtectionKeyId és GetProtectionKey módszerek használatával. Példaként lásd a következő .NET-kód: a megadott **EncryptSymmetricKeyData** módszer [Itt](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Ez a védelem kulcs azonosítóját a titkosítási tárterület-X.509 a tartalom kulcs titkosítására használt tanúsítvány.
    ProtectionKeyType | Ez az a védelem billentyűt a tartalom kulcs titkosítására használt titkosításának típusát. Ez az érték nem StorageEncryption(1) példáinkban.
    Ellenőrző |A MD5 számított ellenőrző a tartalom billentyűt. Hogy titkosítja a tartalom billentyűt a tartalom azonosító számítja ki. A példa kód bemutatja, hogyan számítsa ki az ellenőrző.
    

###<a name="retrieve-the-protectionkeyid"></a>A ProtectionKeyId beolvasása 
 

A következő példa bemutatja, hogyan ProtectionKeyId, egy tanúsítványt ujjlenyomat a tanúsítvány kell használnia, amikor a tartalom kulcs titkosítása beolvasásához. Végezze el ezt a lépést, győződjön meg arról, hogy még rendelkezik a megfelelő tanúsítványt a számítógépre.


A kérelem:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Válasz:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>A ProtectionKeyId a ProtectionKey lekérése

A következő példa bemutatja, hogyan letöltheti a X.509 tanúsítványt, használja a ProtectionKeyId az előző lépésben kapott.

A kérelem:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Válasz:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>A tartalom kulcs létrehozása 

A x.509-es tanúsítvány beolvasott és a tartalom használatával titkosítására használt saját nyilvános kulcs, **ContentKey** entitás létrehozása, és ennek megfelelően beállítani az tulajdonság értékeit.

Az értékek közül, hogy kell-e beállítva mikor létrehozása a tartalom kulcsa típusát. A tároló titkosítás esetén értéke "1". 

A következő példa bemutatja, hogyan hozhat létre egy **ContentKey** **ContentKeyType** van beállítva az tárterület-titkosítás ("1"), és jelzi, hogy a védelem kulcs azonosító a x.509-es tanúsítvány ujjlenyomat "0" állítsa a **ProtectionKeyType** .  


Kérés

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Válasz:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Egy tárgyi eszköz létrehozása

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>A ContentKey társítása tárgyi eszköz

Miután létrehozta a ContentKey, társíthatja az eszköz használatával a $links művelet a következő példában látható módon:
    
A kérelem:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Válasz:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Hozzon létre egy AssetFile

A [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) entitás video- vagy blob-tárolóban tárolt fájlt jelöl. Egy tárgyi eszköz fájl mindig tárgyi eszköz társított, és tárgyi eszköz egy vagy több eszköz fájlokat tartalmazhatnak. A Media Services Encoder tevékenység sikertelen lesz, ha az eszköz fájlt objektumként program nem társítja blob-tároló digitális fájl.

Ne feledje, hogy az **AssetFile** példány és a tényleges médiafájl két különálló objektummal. AssetFile példány, a multimédiás fájl metaadatokat tartalmaz, miközben a multimédiás fájl tartalmaz a tényleges médiatartalom.

Blob-tároló be a digitális eszközökkel készített tartalom fájl feltöltése, után a http- **EGYESÍTÉSE** kérés fogja használni a frissítése a AssetFile adatai kapcsolatban a multimédiás fájl (Ez a témakör nem látható). 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
