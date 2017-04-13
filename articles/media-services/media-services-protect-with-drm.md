<properties
    pageTitle="Dinamikus PlayReady és/vagy Widevine közös titkosítással |} Microsoft Azure"
    description="Microsoft Azure Media Services lehetővé teszi, hogy MPEG-vonal, a zökkenőmentes Streaming és a Http-Live-adatfolyam (HLS) adatfolyamok látták el a Microsoft PlayReady DRM kézbesítése. Lehetővé teszi a kézbesítési Widevine DRM titkosított kötőjel. Ez a témakör bemutatja, hogyan dinamikusan titkosítás jelszóval: PlayReady és Widevine DRM."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Dinamikus PlayReady és/vagy Widevine közös titkosítással

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Services lehetővé teszi, hogy MPEG-vonal, a zökkenőmentes Streaming és a HTTP-Live-adatfolyam (HLS) adatfolyamok látták el [A Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)kézbesítése. Lehetővé teszi a titkosított kötőjel adatfolyamok előadása a Widevine DRM licenceket. A közös titkosítási (ISO/IEC 23001-7 CENC) specifikációja egy titkosított PlayReady és Widevine is. [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (a 3.5.1-es verzióját kezdve) vagy a REST API segítségével állítsa be a AssetDeliveryConfiguration Widevine használni.

Media Services szolgáltatást a kézbesítési PlayReady és Widevine DRM licencek nyújt. Media Services is itt adhatja meg a jogokat és korlátozásokat kívánt a PlayReady vagy Widevine DRM Runtime be, amikor a felhasználó lejátssza API-k védett tartalmat. Amikor a felhasználó kér egy DRM védett tartalmat, a lejátszó alkalmazást licenc kér a AMS licenc szolgáltatásból. A AMS licenc szolgáltatás az engedélyezett-licenc hiba a lejátszó. PlayReady vagy Widevine licenc a ügyfél Windows Media Player visszafejtése és a adatfolyamként használható visszafejtés kulcsot tartalmazza.


A következő AMS partnerek segít Widevine licencek előadása is használhatja: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). További tudnivalókért lásd: integráció a [Axinom](media-services-axinom-integration.md) és [castLabs](media-services-castlabs-integration.md).

Media Services engedélyezése a felhasználók, akik fő kéréseket többféleképpen támogatja. A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: Nyissa meg vagy token korlátozást. A token korlátozott házirend által egy biztonságos jogkivonat szolgáltatás (STS) kiadott token mellékelni kell. Media Services tokenek támogatja az [Egyszerű webes tokenek](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) és [JSON webes jogkivonat](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátum. További tudnivalókért lásd: a tartalom kulcs engedélyezési házirend beállítása.

Dinamikus titkosítást előnyeit, amelyben a többszörös-átviteli sebesség MP4 vagy többszörös-átviteli sebesség zökkenőmentes Streaming forrás fájlokat tárgyi eszköz van szükség. Meg kell az eszköz (című cikkben ismertetett) a kézbesítési házirendek beállítása. Ezután alapján adatfolyam URL-CÍMÉT a megadott formátumban, az adatfolyam-On tárolt kiszolgáló biztosítja, hogy az adatfolyam kézbesítve-e a kiválasztott protokoll. Eredményt adja csak akkor van szükség tárolására és a fájlok egyetlen tárolási formátuma kifizetéséhez és Media Services fog összeállítása és szolgál az egyes kérésének megfelelően ügyfélről megfelelő HTTP választ.

Ez a témakör olvasása látták el több DRMs, például PlayReady és Widevine media-alkalmazások használata a fejlesztők számára hasznos lehet. A témakör bemutatja, hogyan állítható be a PlayReady licenc kézbesítési szolgáltatás engedélyezési házirendek az, hogy csak az engedélyezett ügyfelei PlayReady vagy Widevine licencet kap. Azt is megtudhatja, hogy miként használata a dinamikus titkosítás titkosítás PlayReady és Widevine DRM vonal fölé.

>[AZURE.NOTE]Dinamikus titkosítást használatának megkezdéséhez, akkor be kell szereznie egy időosztás egységet (más néven adatfolyam egység). További információért tájékozódhat [skála Media szolgáltatás](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Töltse le a minta

Letöltheti a minta, ez a cikk az [Itt](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm)leírtak.

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Dinamikus közös titkosítást és DRM licenc kézbesítési szolgáltatások konfigurálása

Az alábbi lépések általános volna kell végrehajtani, ha a PlayReady, a Media Services licenc kézbesítési szolgáltatást használ, és a dinamikus titkosítással is eszközök védelme.

1. Hozzon létre egy eszközt, és fájlokat feltölteni az eszközt.
1. A digitális eszköz kiválasztása az adaptív átviteli MP4 meg a fájlt tartalmazó kódolását.
1. Hozzon létre egy tartalom kulcsot, és a kódolt eszköz társítása. A Media Services a tartalom billentyűt az eszköz a titkosítási kulcs tartalmazza.
1. Állítsa be a tartalom kulcs engedélyezési házirend. A tartalom főbb engedélyezési házirend Ön által beállított legyen, és az ügyfél, kézbesítése ahhoz, hogy a tartalom billentyűt az ügyfél által teljesül.

A tartalom főbb engedélyezési házirend létrehozásakor meg kell adnia az alábbi: kézbesítési módszer (PlayReady vagy Widevine), korlátozások (nyitott vagy jogkivonat), és a fő kézbesítési típusa, amely definiálja, hogyan az a billentyűt eljuttatni az ügyfél ([PlayReady](media-services-playready-license-template-overview.md) vagy [Widevine](media-services-widevine-license-template-overview.md) licenc-sablon)-specifikus adatait.
1. Állítsa be a kézbesítési házirendet, az eszköz. A kézbesítési házirendjének beállítása tartalmazza: kézbesítési protocol (például MPEG kötőjel, HLS, HDS, zökkenőmentes Streaming vagy az összes), a típusú dinamikus titkosítást (például gyakori titkosítás), PlayReady vagy Widevine licenc WIA URL-CÍMÉT.

Az azonos eszköz egyes jegyzőkönyv sikerült különböző házirend vonatkozik. Például Sima kapcsolódású vagy szaggatott és HLS AES borítékra PlayReady titkosítási is alkalmazhat. Bármely kézbesítési házirend nem meghatározott protokollok (például felvett egy egyetlen házirendet, amely csak a protokolljaként adja meg HLS) letiltja a folyamatos átvitelű. Ez a kivétel ez alól nincs eszköz kézbesítési házirend egyáltalán definiált esetén. Ezt követően törölje minden protokoll engedélyezett.
1. Hozzon létre egy OnDemand megnevezés ahhoz, hogy egy adatfolyam URL-CÍMÉT.

A teljes .NET vett példa a témakör végén található.

Az alábbi képen a fentebb ismertetett munkafolyamat mutatja be. Itt a token hitelesítéshez használt.

![A PlayReady védelme](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Ez a témakör a többi részletes magyarázatot, kód példákat és bemutatja, hogyan tudja elérni a fentebb ismertetett feladatokat tartalmazó témakörökre mutató hivatkozások biztosít.

##<a name="current-limitations"></a>Aktuális korlátozások

Ha szeretne hozzáadni, vagy egy digitáliseszköz-kézbesítési házirend módosítására, törölje a társított Megnevezés (ha van ilyen), és hozzon létre egy új megnevezés.

Korlátozás az Azure Media Services Widevine titkosításához: jelenleg nem támogatottak több tartalom billentyűk.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Hozzon létre egy eszközt, és a fájlokat feltölteni a digitális eszköz kiválasztása

Kezelése, kódolását, és a videók adatfolyam-, a tartalom először töltse fel a Microsoft Azure Media Services. Miután feltöltött, a tartalom tárolási biztonságosan feldolgozásra és a folyamatos átvitelű a felhőben.

Részletes információt olvassa el a [Fájl feltöltése a Media Services-fiókba](media-services-dotnet-upload-files.md)című témakört.

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>A digitális eszköz kiválasztása az adaptív átviteli MP4 meg a fájlt tartalmazó kódolását

Dinamikus titkosítással szükség, amelyben a többszörös-átviteli sebesség MP4 vagy többszörös-átviteli sebesség zökkenőmentes Streaming forrás fájlokat tárgyi eszköz létrehozása. Ezután a megadott formátumban a jegyzék és fragment kérelem alapján, az adatfolyam-On tárolt kiszolgáló biztosítja, hogy a választott protokoll kap az adatfolyam. Eredményt adja csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services szolgáltatás fog összeállítása és a megfelelő választ, egy ügyfél érkező kérések alapján szolgálnak. További információ a [Dinamikus összecsomagolása áttekintése](media-services-dynamic-packaging-overview.md) témakörben talál.

Megtudhatja, [hogy miként tárgyi eszköz használatával Media Encoder szabványos kódolását](media-services-dotnet-encode-with-media-encoder-standard.md)kódolását útmutatást.


##<a id="create_contentkey"></a>Tartalom kulcs létrehozása és társíthatja az kódolt eszköz

A Media Services a tartalom billentyűt, hogy az eszköz titkosítani kívánt kulcsot tartalmazza.

Részletes információkért lásd: [létrehozása tartalom billentyűt](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>A tartalom kulcs engedélyezési házirend beállítása

Media Services támogatja a több módon is, hogy a felhasználók, akik fő kéréseket hitelesítése. A tartalom főbb engedélyezési házirend kell állítható be, és az ügyfél (player) teljesülnie kell ahhoz, hogy az ügyfél, kézbesítése a billentyűt. A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: Nyissa meg vagy token korlátozást.

Részletes információt olvassa el a [Tartalom kulcs engedélyezési házirendek beállítása](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption)című témakört.

##<a id="configure_asset_delivery_policy"></a>A digitális eszköz kiválasztása kézbesítési házirend beállítása 

Állítsa be a kézbesítési házirendet, az eszköz. Néhány dolog, amit, amely tartalmazza az eszköz kézbesítési házirendek beállítása:

- DRM licenc WIA URL-CÍMÉT. 
- A tárgyi eszköz kézbesítési Protocol (protokoll) (például MPEG kötőjel, HLS, HDS, zökkenőmentes Streaming vagy az összes). 
- A dinamikus titkosítást (ebben az esetben az közös titkosítási) típusát. 

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

##<a id="example"></a>Példa


A következő példa bemutatja a funkciókat, amelyek a .NET rendszerhez az Azure Media Services SDK jelent meg-verzió 3.5.2 (illetve különösen azon, az azt jelenti, hogy Widevine licenc sablon létrehozása és Widevine licenc kérése az Azure Media Services). A csomag telepítéséhez használt Nuget csomag a következő parancsot:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Konzol új projekt létrehozása.
1. NuGet segítségével telepítése, és adja hozzá az Azure Media Services .NET SDK.
2. További hivatkozások hozzáadása: System.Configuration.
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

1. Szerezze be legalább egy adatfolyam egység az adatfolyam végpontot, amelyből tervezi kézbesítési a tartalmakat. További tudnivalókért lásd: [állítsa be a továbbított végpontok](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. A kódot a Program.cs fájl felülírása az ebben a szakaszban a kódot.
    
    Győződjön meg arról, hogy hol találhatók a bemeneti fájlok mappák mutassanak változók frissítése.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
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
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
        
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


## <a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Lásd még:

[CENC többszörös-DRM és a hozzáférés-vezérlés](media-services-cenc-with-multidrm-access-control.md)

[AMS Widevine összecsomagolása konfigurálása](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[A Google Widevine licenc kézbesítési szolgáltatások az Azure Media Services kihirdetése](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
