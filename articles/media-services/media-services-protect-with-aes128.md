<properties
    pageTitle="A dinamikus titkosítást AES-128 és a fő kézbesítési szolgáltatás |} Microsoft Azure"
    description="Microsoft Azure Media Services lehetővé teszi a titkosított AES 128 bites titkosítási kulcs tartalmak olvasása. Media Services is tartalmaz a kulcs kézbesítési szolgáltatás, amely biztosítja a titkosítási kulcs jogosult felhasználók. Ez a témakör bemutatja, hogyan dinamikusan titkosítás jelszóval: AES-128 és a fő kézbesítési szolgáltatás használata."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Dinamikus titkosítást AES-128 és a fő kézbesítési szolgáltatás használatával

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>– Áttekintés

>[AZURE.NOTE] Lásd: [ebből](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) a videóból megtudhatja, hogy miként védelme a multimédiás tartalom AES titkosítást.

Microsoft Azure Media Services lehetővé teszi, hogy Http-Live-adatfolyam (HLS) és a speciális Encryption Standard (AES) (128 bites titkosítási kulcs használatával) titkosított zökkenőmentes adatfolyam megjelenítését. Media Services is tartalmaz a kulcs kézbesítési szolgáltatás, amely biztosítja a titkosítási kulcs jogosult felhasználók. Ha azt szeretné, a Media Services tárgyi eszköz titkosítására, kell társítani a titkosítási kulcs az eszköz és is beállíthatja az billentyű engedélyezési házirendeket. Adatfolyam a Windows Media Player kérésekor Media Services használja a megadott kulcs dinamikusan titkosítása a tartalom AES titkosítást használ. Visszafejteni az adatfolyam, a Windows Media player felkéri a kulcsot a fő kézbesítési szolgáltatásból. Döntse el, vagy sem a felhasználó jogosult a kulcs első, a szolgáltatás kiértékeli a kulcs megadott engedélyezési házirendek.

Media Services támogatja a több módon is, hogy a felhasználók, akik fő kéréseket hitelesítése. A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: Nyissa meg vagy token korlátozást. A token korlátozott házirend által egy biztonságos jogkivonat szolgáltatás (STS) kiadott token mellékelni kell. Media Services tokenek támogatja az [Egyszerű webes tokenek](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) és [JSON webes jogkivonat](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátum. További tudnivalókért olvassa el a [a tartalom kulcs engedélyezési házirend beállítása](media-services-protect-with-aes128.md#configure_key_auth_policy)című témakört.

Dinamikus titkosítást előnyeit, amelyben a többszörös-átviteli sebesség MP4 vagy többszörös-átviteli sebesség zökkenőmentes Streaming forrás fájlokat tárgyi eszköz van szükség. Meg kell állítja be a kézbesítési házirendet, az eszköz (című cikkben ismertetett). Ezután alapján adatfolyam URL-CÍMÉT a megadott formátumban, az adatfolyam-On tárolt kiszolgáló biztosítja, hogy az adatfolyam kézbesítve-e a kiválasztott protokoll. Eredményt adja csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services szolgáltatás fog összeállítása és a megfelelő választ, egy ügyfél érkező kérések alapján szolgálnak.

Ez a témakör olvasása védett médiafájlok-alkalmazások használata a fejlesztők számára hasznos lehet. A témakör bemutatja, hogyan állítható be a fő kézbesítési szolgáltatás engedélyezési házirendek az, hogy csak hitelesített ügyfelek kapnak a titkosítási kulcs. Azt is megtudhatja, hogy miként dinamikus-titkosítás használatára.

>[AZURE.NOTE]Dinamikus titkosítást használatának megkezdéséhez, akkor be kell szereznie egy időosztás egységet (más néven adatfolyam egység). További információért tájékozódhat [skála Media szolgáltatás](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Dinamikus titkosítást AES-128 és a fő kézbesítési szolgáltatás munkafolyamat

Az alábbi lépések általános szeretné kell végrehajtani, ha a eszközök AES, a fő kézbesítési Media Services szolgáltatást használ, és a dinamikus titkosítással is titkosítja.

1. [Hozzon létre egy eszközt, és töltse fel a fájlokat az eszköz](media-services-protect-with-aes128.md#create_asset).
1. [A digitális eszköz kiválasztása az adaptív átviteli MP4 meg a fájlt tartalmazó kódolása](media-services-protect-with-aes128.md#encode_asset).
1. A [tartalom kulcs létrehozása és a kódolt eszköz társítandó](media-services-protect-with-aes128.md#create_contentkey). A Media Services a tartalom billentyűt az eszköz a titkosítási kulcs tartalmazza.
1. [A tartalom kulcs engedélyezési házirend beállítása](media-services-protect-with-aes128.md#configure_key_auth_policy). A tartalom főbb engedélyezési házirend Ön által beállított legyen, és az ügyfél, kézbesítése ahhoz, hogy a tartalom billentyűt az ügyfél által teljesül.
1. [A kézbesítési házirendet, az eszköz beállítása](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Tartalmaz a kézbesítési házirendek beállítása: főbb WIA URL-CÍMÉT, illetve inicializálni vektoros IV (AES 128 van szüksége, az azonos IV titkosítása és visszafejtése szállítandó), kézbesítési protocol (például MPEG kötőjel, HLS, HDS, zökkenőmentes Streaming vagy az összes), a típusú dinamikus titkosítást (például a borítékon vagy a dinamikus titkosítás nélkül).

Az azonos eszköz egyes jegyzőkönyv sikerült különböző házirend vonatkozik. Például Sima kapcsolódású vagy szaggatott és HLS AES borítékra PlayReady titkosítási is alkalmazhat. Bármely kézbesítési házirend nem meghatározott protokollok (például felvett egy egyetlen házirendet, amely csak a protokolljaként adja meg HLS) letiltja a folyamatos átvitelű. Ez a kivétel ez alól nincs eszköz kézbesítési házirend egyáltalán definiált esetén. Ezt követően törölje minden protokoll engedélyezett.

1. [Létrehozás egy OnDemand megnevezés](media-services-protect-with-aes128.md#create_locator) ahhoz, hogy egy adatfolyam URL-CÍMÉT.

A témakör is láthatók lesznek [ügyfélalkalmazás hogyan kérhet egy kulcsot a fő kézbesítési szolgáltatásból](media-services-protect-with-aes128.md#client_request).

Egy teljes .NET [Példa](media-services-protect-with-aes128.md#example) a témakör végén található.

Az alábbi képen a fentebb ismertetett munkafolyamat mutatja be. Itt a token hitelesítéshez használt.

![A AES-128 védelme](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Ez a témakör a többi részletes magyarázatot, kód példákat és bemutatja, hogyan tudja elérni a fentebb ismertetett feladatokat tartalmazó témakörökre mutató hivatkozások biztosít.

##<a name="current-limitations"></a>Aktuális korlátozások

Ha szeretne hozzáadni, vagy a digitáliseszköz-kézbesítési házirend módosítására, törölje a egy meglévő Megnevezés (ha van ilyen), és hozzon létre egy új megnevezés.

##<a id="create_asset"></a>Hozzon létre egy eszközt, és a fájlokat feltölteni a digitális eszköz kiválasztása

Kezelése, kódolását, és a videók adatfolyam-, a tartalom először töltse fel a Microsoft Azure Media Services. Miután feltöltött, a tartalom tárolási biztonságosan feldolgozásra és a folyamatos átvitelű a felhőben. 

Részletes információt olvassa el a [Fájl feltöltése a Media Services-fiókba](media-services-dotnet-upload-files.md)című témakört.

##<a id="encode_asset"></a>A digitális eszköz kiválasztása az adaptív átviteli MP4 meg a fájlt tartalmazó kódolását

Dinamikus titkosítással szükség, amelyben a többszörös-átviteli sebesség MP4 vagy többszörös-átviteli sebesség zökkenőmentes Streaming forrás fájlokat tárgyi eszköz létrehozása. Ezután a megadott formátumban, az On igény folyamatos átvitelű jegyzék vagy fragment összehívásban alapján kiszolgáló biztosítja, hogy a választott protokoll kap az adatfolyam. Eredményt adja csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services szolgáltatás fog összeállítása és a megfelelő választ, egy ügyfél érkező kérések alapján szolgálnak. További információ a [Dinamikus összecsomagolása áttekintése](media-services-dynamic-packaging-overview.md) témakörben talál.

Megtudhatja, [hogy miként tárgyi eszköz használatával Media Encoder szabványos kódolását](media-services-dotnet-encode-with-media-encoder-standard.md)kódolását útmutatást.

##<a id="create_contentkey"></a>Tartalom kulcs létrehozása és társíthatja az kódolt eszköz

A Media Services a tartalom billentyűt, hogy az eszköz titkosítani kívánt kulcsot tartalmazza.

Részletes információkért lásd: [létrehozása tartalom billentyűt](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>A tartalom kulcs engedélyezési házirend beállítása

Media Services támogatja a több módon is, hogy a felhasználók, akik fő kéréseket hitelesítése. A tartalom főbb engedélyezési házirend kell állítható be, és az ügyfél (player) teljesülnie kell ahhoz, hogy az ügyfél, kézbesítése a billentyűt. A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: Nyissa meg a, a korlátozás, és IP-korlátozás token.

Részletes információt olvassa el a [Tartalom kulcs engedélyezési házirendek beállítása](media-services-dotnet-configure-content-key-auth-policy.md)című témakört.

##<a id="configure_asset_delivery_policy"></a>A digitális eszköz kiválasztása kézbesítési házirend beállítása 

Állítsa be a kézbesítési házirendet, az eszköz. Néhány dolog, amit, amely tartalmazza az eszköz kézbesítési házirendek beállítása:

- A kulcs WIA URL-címe. 
- A inicializálni vektoros (IV) használata a boríték titkosításhoz. AES 128 az azonos IV titkosítása és visszafejtése szállítandó szükséges. 
- A tárgyi eszköz kézbesítési Protocol (protokoll) (például MPEG kötőjel, HLS, HDS, zökkenőmentes Streaming vagy az összes).
- A dinamikus titkosítást (például AES boríték) típusú vagy dinamikus titkosítás nélkül. 

Részletes információkért lásd: az [eszköz kézbesítési házirend konfigurálása ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Hozzon létre egy OnDemand megnevezés streaming ahhoz, hogy egy adatfolyam URL-címe

Meg kell a szükséges felhasználói az adatfolyam URL-címmel sima, kötőjel vagy HLS.

>[AZURE.NOTE]Ha szeretne hozzáadni, vagy a digitáliseszköz-kézbesítési házirend módosítására, törölje a egy meglévő Megnevezés (ha van ilyen), és hozzon létre egy új megnevezés.

Tárgyi eszköz közzététele és adatfolyam URL-cím létrehozása című cikkben olvashat [összeállítása adatfolyam URL-címet](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>A próba jogkivonat beszerzése

Első teszten jogkivonat a token korlátozást, a fő engedélyezési házirend használt alapján.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

A [AMS lejátszó](http://amsplayer.azurewebsites.net/azuremediaplayer.html) is használhatja az adatfolyam tesztelése.

##<a id="client_request"></a>Hogyan lehet az ügyfélnek kérése egy kulcsot a fő kézbesítési szolgáltatás?

Az előző lépésben kialakítani a nyilvánvalóan fájlra mutató URL-CÍMÉT. Az ügyfélnek kell a szükséges információkat a fő kézbesítési szolgáltatás kérelmének kezdeményezéséhez kinyerése adatfolyam jegyzékfájlok.

###<a name="manifest-files"></a>Jegyzékfájlok

Az ügyfél kell bontsa ki az URL-cím (is tartalom kulcsot tartalmazó (kid) azonosító) mezőbe írja be a nyilvánvalóan fájlt. Az ügyfél majd megpróbálja a titkosítási kulcs beolvasása a fő kézbesítési szolgáltatás. Az ügyfél kell az IV értéket, és azt visszafejteni az adatfolyam kibontásához. Az alábbi kódtöredékének látható a <Protection> a folyamatos zökkenőmentes jegyzék elemet.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

HLS, amíg a legfelső szintű jegyzék osztani szakasz fájlokat. 

Ha például a legfelső szintű jegyzék az: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl), és a szakasz fájlnevek listáját tartalmazza.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Ha nyitja meg a szakasz fájlok egy egyszerű szövegszerkesztő programban (például http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), tartalmaznia kell #EXT X-kulcs, amely azt jelzi, hogy a fájl titkosított.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>A kulcs kérése a fő kézbesítési szolgáltatás

A következő kódrészlet szemlélteti, hogyan lehet Media Services fő kézbesítési szolgáltatás fő kézbesítési (vagyis a jegyzék kiolvasott volt) Uri-összehívás küldése és a jogkivonat (Ez a témakör nem beszélhet az arról, hogy miként juthat az egyszerű webes tokenek biztonságos jogkivonat szolgáltatást).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Példa

1. Konzol új projekt létrehozása.
1. NuGet segítségével telepítése, és adja hozzá az Azure Media Services .NET SDK-bővítmények. A csomag telepítésekor is Media Services .NET SDK telepíti, és hozzáadja szükséges függőségek.
2. Adja hozzá a konfigurációs fájl, amely tartalmazza a fiók nevét, és a főbb tudnivalók:

    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. A kódot a Program.cs fájl felülírása az ebben a szakaszban a kódot.
    
    Győződjön meg arról, hogy hol találhatók a bemeneti fájlok mappák mutassanak változók frissítése.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    // In this case "H264 Multiple Bitrate 720p" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        TaskOptions.None);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "HLS Open Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null // no requirements needed for HLS
                                };
        
                    restrictions.Add(restriction);
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                        "policy",
                        ContentKeyDeliveryType.BaselineHttp,
                        restrictions,
                        "");
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                            new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                            new ContentKeyAuthorizationPolicyRestriction
                            {
                                Name = "Token Authorization Policy",
                                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                Requirements = tokenTemplateString
                            };
        
                    restrictions.Add(restriction);
        
                    //You could have multiple options 
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                            "Token option for HLS",
                            ContentKeyDeliveryType.BaselineHttp,
                            restrictions,
                            null  // no key delivery data is needed for HLS
                            );
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        
                    return tokenTemplateString;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
        
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
