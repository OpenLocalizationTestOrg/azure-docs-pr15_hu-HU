<properties 
    pageTitle="A tartalom és a Apple FairPlay és/vagy Microsoft PlayReady HLS védelme |} Microsoft Azure" 
    description="Ez a témakör áttekintést nyújt, és bemutatja, hogyan dinamikusan titkosítsa a HTTP-Live Streaming (HLS) tartalma az Apple FairPlay az Azure Media Services segítségével. Azt is megtudhatja, hogy hogyan a Media Services licenc kézbesítési szolgáltatás ügyfelek végzi a FairPlay licenceket." 
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
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>A tartalom és a Apple FairPlay és/vagy Microsoft PlayReady HLS védelme

Azure Media Services lehetővé teszi a HTTP-Live Streaming (HLS) a következő formátumokban tartalomhoz dinamikusan titkosítás:  

- **AES-128 boríték törlése billentyűt** 

    A teljes adattömb titkosítva van, a **AES-128 CBC** üzemmód használata. Az adatfolyamban a visszafejtése és által támogatott iOS OSX player natív módon. További tudnivalókért lásd: [Ez a cikk](media-services-protect-with-aes128.md).

- **Apple FairPlay** 

    Az egyes hang- és videofájlok mintákat titkosított **AES-128 CBC** módban. **A folyamatos átvitelű FairPlay** Az eszköz operációs rendszerek, a natív-támogatás az iOS és az Apple TV (Képkocka) integrálva van. OS X rendszeren futó Safari lehetővé teszi, hogy a Képkocka használata a titkosított multimédia-bővítmények (EME) felület támogatása.
- **Microsoft PlayReady**

Az alábbi képen látható, a **HLS + FairPlay és/vagy PlayReady dinamikus titkosítást** munkafolyamatot.

![A FairPlay védelme](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Ez a témakör bemutatja, hogyan lehet dinamikusan titkosítsa az Apple FairPlay HLS tartalom az Azure Media Services használatával. Azt is megtudhatja, hogy hogyan a Media Services licenc kézbesítési szolgáltatás ügyfelek végzi a FairPlay licenceket.

>[AZURE.NOTE] Ha is szeretne a HLS tartalom PlayReady titkosítása, kell gyakori tartalom kulcs létrehozása és társíthatja az eszköz. Is kell a tartalom kulcs engedélyezési házirendet, állítsa be, [dinamikus közös titkosítást használ PlayReady](media-services-protect-with-drm.md) ismertetett módon.

    
## <a name="requirements-and-considerations"></a>Követelmények és kapcsolatos szempontok

- Az alábbiak szükségesek AMS FairPlay titkosított HLS előadásához és használatakor előadásához FairPlay licenceket.

    - Az Azure-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](/pricing/free-trial/?WT.mc_id=A261C142F).
    - A Media Services fiók. Media Services fiók létrehozása, olvassa el a [Fiók létrehozása](media-services-portal-create-account.md)című témakört.
    - Jelentkezzen [Apple fejlesztési](https://developer.apple.com/)programmal.
    - Apple szükséges a tartalom tulajdonosát, hogy a [telepítőcsomag](https://developer.apple.com/contact/fps/)beszerzése. Adja meg a kérést, már végrehajtott KSM (kulcs biztonsági modul) az Azure Media Services és az utolsó Képkocka csomag kérelmet. A végleges Képkocka csomag hitelesítő készítése és szerezze be a kérdés, amely FairPlay konfigurálása használatával című témakör útmutatását lesz. 

    - Azure Media Services .NET SDK verzió **3.6.0** vagy újabb verziójában.

- Az alábbi dolgot be kell állítania a AMS fő kézbesítési oldalon:
    - **Alkalmazás-tanúsítvány (TK)** - titkos kulcs tartalmazó .pfx fájl. A fájl ügyfél által létrehozott és ugyanazon ügyfél által a jelszóval titkosított. 
        
        Az ügyfél beállítja a fő kézbesítési házirendet, amikor azok kell adnia, hogy a jelszó és a .pfx base64 formátumban.

        Az alábbi lépéseket ismertetik, hogyan lehet pfx tanúsítvány létrehozása való FairPlay.
        
        1. Telepítse az OpenSSL https://slproweb.com/products/Win32OpenSSL.html
        
            Nyissa meg a mappát, hogy hol a FairPlay tanúsítvány és más fájlok Apple hozta.
        
        2. A cer konvertálása pem parancssori:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-tájékoztatja der-a fairplay.cer-fairplay-out.pem meg
        
        3. A parancssor pem átalakítása pfx a titkos kulccsal (a pfx fájl jelszavának majd felkéri OpenSSL).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-exportálása - fairplay-out.pfx ki-inkey privatekey.pem – a fairplay-out.pem - passin file:privatekey-pem-pass.txt 
        
    - **Alkalmazás tanúsítvány jelszó** - ügyfél jelszavát a .pfx fájl létrehozásához.
    - **Alkalmazás tanúsítvány jelszó azonosító** – az ügyfélnek kell feltölteni a hasonló hogyan azok feltöltése AMS szolgáló egyéb billentyűk és **ContentKeyType.FairPlayPfxPassword** felsorolás érték használata a jelszót. Ennek eredményeképpen kapnak AMS azonosító Ez az általuk szükséges a fő kézbesítési házirend-beállítást belül is használhatja.
    - **iv** - 16 bájt véletlenszerűen kiválasztott értéket, meg kell egyeznie a iv az eszköz kézbesítési házirend. Ügyfél a IV hoz létre, és elhelyezi a két helyen: tárgyi eszköz kézbesítési házirend- és kézbesítési házirend lehetőséget. 
    - **ASK** - ASK (alkalmazás titkos kulcs) érkezik, a minősítési Apple Developer portal segítségével létrehozásakor. Minden egyes fejlesztőcsapatához kap egy egyedi ASK. A kérdés másolatának mentése, és a biztonságos helyen tárolja azt. Szüksége lesz az ASK konfigurálja az Azure Media Services később FairPlayAsk. 
    -  **Kérje meg azonosító** – az ügyfél feltöltések megjelenítése az ASk be AMS kapott. Az ügyfél kell töltse fel az ASk **ContentKeyType.FairPlayASk** felsorolás érték használata. Ennek eredményeképpen AMS azonosítóját adja vissza, és most, hogy mit kell használni, amikor a fő kézbesítési beállítást.

- Képkocka ügyféloldali kell beállítania a következő műveleteket:
    - **Alkalmazás-tanúsítvány (TK)** - OS néhány hasznos titkosítása használó nyilvános kulcs.cer/.der-fájlt. AMS kell tudnom még azt, mivel a Windows Media Player szükség van. A fő kézbesítési szolgáltatás visszafejti a megfelelő titkos kulccsal.

- Lejátszás FairPlay titkosított adatfolyam és szeretné jobban valós ASK első valós tanúsítványt, majd generálni szüksége. Ezt a folyamatot minden 3 kijelzők hozza létre:

    -  .der, 
    -  .pfx és 
    -  a .pfx jelszavát.
 
- **AES-128 CBC** titkosítással HLS támogató ügyfelek: Safari OS X, Apple TV iOS operációs rendszeren.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Lépései FairPlay dinamikus titkosítást és a licencek kézbesítési szolgáltatások konfigurálása

Az alábbi lépések általános volna kell végrehajtani, ha a FairPlay, a Media Services licenc kézbesítési szolgáltatást használ, és a dinamikus titkosítással is eszközök védelme.

1. Hozzon létre egy eszközt, és fájlokat feltölteni az eszközt. 
1. A digitális eszköz kiválasztása az adaptív átviteli MP4 meg a fájlt tartalmazó kódolását.
1. Hozzon létre egy tartalom kulcsot, és a kódolt eszköz társítása.  
1. Állítsa be a tartalom kulcs engedélyezési házirend. A tartalom főbb engedélyezési házirend létrehozásakor meg kell adnia a következőket: 
    
    - Kézbesítési módszer (ebben az esetben az FairPlay), 
    - Házirend-beállítások konfigurálása FairPlay. FairPlay konfigurálásával kapcsolatos részletekért olvassa el az alábbi minta ConfigureFairPlayPolicyOptions() módszert.
    
        >[AZURE.NOTE] Általában volna beállítandó FairPlay házirend-beállítások csak egyszer óta csak akkor hitelesítő, és kérdezze meg az egyik készlete.
-(nyitott vagy jogkivonat), - korlátozás és a fő kézbesítési típusa, amely definiálja, hogyan az a billentyűt eljuttatni az ügyfél-specifikus adatait. 
    
2. Állítsa be a digitális eszköz kiválasztása kézbesítési házirend. Tartalmaz a kézbesítési házirendek beállítása: 

    - kézbesítési protocol (HLS), 
    - milyen típusú dinamikus titkosítást (közös CBC titkosítás) 
    - licenc WIA URL-címe. 
    
    >[AZURE.NOTE]Ha FairPlay + másik DRM titkosított adatfolyam olvasása, van-e külön kézbesítési házirendek beállítása 
    >
    >- Egy IAssetDeliveryPolicy kötőjel konfigurálása CENC (PlayReady + WideVine) és a PlayReady sima. 
    >- Egy másik IAssetDeliveryPolicy HLS FairPlay konfigurálása

1. Hozzon létre egy OnDemand megnevezés megszerezni a továbbított URL-címet.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>Fő kézbesítési FairPlay használatával player/ügyfélalkalmazások

Ügyfelek fejleszteni player alkalmazások iOS SDK használatával. Annak érdekében, hogy tudja lejátszani FairPlay tartalmat az ügyfelek kell licencet exchange jegyzőkönyv végrehajtása. A licenc exchange protokoll szerint Apple nincs megadva. Esetén a minden alkalmazás fő kézbesítési kérések üzenetküldés. A fő kézbesítési AMS FairPlay szolgáltatások vár a SPC hamarosan www-form-url kódolt küldés üzenetként a következő formátumban: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player FairPlay lejátszás beépített nem támogatja. Ügyfelek kell kérnie a minta Windows Media player elvégezve FairPlay lejátszás MAC OSX Apple Fejlesztőeszközök fiókból. 
 
##<a name="streaming-urls"></a>Adatfolyam URL-címei

A digitális eszköz kiválasztása a egynél több DRM titkosítva van, ha egy titkosító címke kell használni az adatfolyam URL-cím: (formátum = "m3u8-aapl" titkosítási = "xxx").

A következő érvényesek:

- Csak nulla vagy egy típusú titkosítást adható meg.
- Titkosítási típus nem található meg az URL-címet kell megadni, ha csak egy titkosító alkalmaztak az eszközre.
- Titkosítási típus a kis-és nagybetűk.
- A következő típusú titkosítást adható meg:  
    - **cenc**: közös titkosítási (Playready vagy Widevine)
    - **cbcs-aapl**: Fairplay
    - **CBC**: AES boríték titkosítást.


##<a name="net-example"></a>.NET példa


A következő példa bemutatja a funkciókat, amelyek a .NET rendszerhez az Azure Media Services SDK jelent meg-verzió 3.6.0 (az azt jelenti, hogy a tartalom FairPlay titkosított előadása Azure Media Services segítségével). A csomag telepítéséhez használt Nuget csomag a következő parancsot:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Konzol projektet létrehozni.
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
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Következő lépések: A Media Services tanulási javaslatok

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
