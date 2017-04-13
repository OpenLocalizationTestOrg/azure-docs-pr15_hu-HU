<properties 
    pageTitle="Hogyan végezhetők el az Azure Media Services használatával hogyan hozhat létre több-átviteli sebesség adatfolyamok .NET élő folyamatos átvitelű |} Microsoft Azure" 
    description="Ebben az oktatóanyagban végigvezeti a csatorna, amely egyetlen-átviteli sebesség élő adatfolyam fogadása és kódolja a többszörös-átviteli sebesség adatfolyam .NET SDK használatával történő létrehozását." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Hogyan végezhetők el az Azure Media Services használatával hogyan hozhat létre több-átviteli sebesség adatfolyamok .NET élő folyamatos átvitelű

> [AZURE.SELECTOR]
- [Portál](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API-VAL](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban végigvezeti a **csatorna** , amely egyetlen-átviteli sebesség élő adatfolyam fogadása és kódolja a szeretne több-átviteli sebesség adatfolyam létrehozása.

Élő kódolást engedélyezett csatornák kapcsolatos további tájékoztatást talál [Live streaming az Azure Media Services hozhat létre több-átviteli sebesség adatfolyamok használata](media-services-manage-live-encoder-enabled-channels.md)


##<a name="common-live-streaming-scenario"></a>Élő adatfolyam szokták

Az alábbi lépések leírják a közös élő adatfolyam alkalmazások létrehozása feladatairól.

>[AZURE.NOTE] Élő esemény max ajánlott hosszának jelenleg 8 órát. Ha hosszabb ideig csatorna futtatásához van szüksége, forduljon a amslived a Microsoft.com webhelyről.

1. Videokamera kapcsolódni a számítógéphez. Indítsa el, és állítsa be a helyszíni élő kódoló, amely az egyik a következő protokollok egyetlen átviteli sebesség adatfolyam is kimeneti: RTMP, zökkenőmentes Streaming vagy RTP (MPEG-TS). További tudnivalókért lásd: az [Azure Media Services RTMP támogatási és Live kódolók](http://go.microsoft.com/fwlink/?LinkId=532824).

A csatorna létrehozása után is hajtható végre ezt a lépést.

1. Csatorna létrehozása és.

1. A csatorna lekérés ingest URL-CÍMÉT.

Az élő kódoló ingest URL-CÍMÉT az adatfolyam küldése a csatorna használt.

1. A csatorna kép URL-Címének beolvasásához.

Ha ellenőrizni szeretné, hogy a csatorna megfelelően fogadja az élő adatfolyam az URL-cím használata

2. Hozzon létre egy eszközt.
3. Ha azt szeretné, az eszköz dinamikusan titkosítása lejátszása közben, tegye a következőket:
1. Hozzon létre egy tartalom billentyűt.
1. Állítsa be a tartalom kulcs engedélyezési házirend.
1. Állítsa be az eszköz kézbesítési házirend (dinamikus csomagolást és dinamikus titkosítást által használt).
3. Hozzon létre egy programot, és adja meg, az Ön által létrehozott eszköz használatára.
1. Az eszköz a program társított: hozzon létre egy OnDemand megnevezés a Közzététel gombra.

Győződjön meg arról, hogy legalább egy, a folyamatos átvitelű fenntartott egységre, amelyhez képest adatfolyam tartalom adatfolyam végpont.

1. Ha készen áll a kezdésre streaming és az archiválás, indítsa el a program.
2. Másik lehetőségként a élő kódoló is jelezhető egy reklámot indításához. A Közzététel helye kerül a kimeneti adatfolyamban.
1. Ha meg szeretné szüntetni a folyamatos átvitelű, és az archiválás az eseményt, állítsa le a program.
1. Törölje a Program (, és tetszés szerint törölje az eszköz).

## <a name="what-youll-learn"></a>A tananyag Dióhéjban

Ez a témakör bemutatja, hogyan szeretné a csatornák és -alkalmazások használata a Media Services .NET SDK különböző műveletek végrehajtása. Mivel a sok művelet vannak hosszan futó kezelése hosszan futó .NET API-khoz műveletek használják.

A témakör bemutatja, hogyan tegye a következőket:

1. Csatorna létrehozása és. A hosszú ideig futó API-khoz használják.
1. Ismerkedés a csatornák ingest (i) végpontot. A végpont meg kell adni a kódoló, amelyek egyetlen átviteli sebesség élő adatfolyam küldhetnek szeretne.
1. Ismerkedés az előnézeti végpontot. A végpont megtekintése a adatfolyam szolgál.
1. Hozzon létre egy a tartalom tárolására használt eszköz. Az eszköz kézbesítési házirendek úgy kell beállítani, valamint, ebben a példában látható módon.
1. Hozzon létre egy programot, és adja meg a korábban létrehozott eszköz használatára. Indítsa el a program. A hosszú ideig futó API-khoz használják.
1. Hozzon létre egy megnevezés az eszközre,, a tartalom kap közzé, és az ügyfelek részére is lehet.
1. Megjelenítheti és elrejtheti palák. Indítsa el, és állítsa le a hirdetések. A hosszú ideig futó API-khoz használják.
1. A csatorna és a kapcsolódó erőforrásokat karbantartása.


##<a name="prerequisites"></a>Előfeltételek

Az alábbiak szükségesek az oktatóprogram elvégzéséhez.

- Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges.

Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](/pricing/free-trial/?WT.mc_id=A261C142F). Próbálja ki az Azure szolgáltatások fizetett használható jóváírások kap. Után a credits használt, megtarthatja a fiókot, és ingyenes Azure szolgáltatásokat és funkciókat, például a Web Apps alkalmazások funkció Azure App szolgáltatásban.
- A Media Services fiók. Media Services fiók létrehozása, olvassa el a [Fiók létrehozása](media-services-portal-create-account.md)című témakört.
- Visual Studio 2010 SP1 (Professional, prémium, Ultimate vagy Express) vagy újabb verziók.
- Media Services .NET SDK 3.2.0.0 verziót kell használnia vagy újabb verziót.
- Webkamera és -kódoló, amelyek egyetlen átviteli sebesség élő adatfolyam küldhetnek.

##<a name="considerations"></a>Megfontolandó szempontok

- Élő esemény max ajánlott hosszának jelenleg 8 órát. Ha hosszabb ideig csatorna futtatásához van szüksége, forduljon a amslived a Microsoft.com webhelyről.
- Győződjön meg arról, hogy legalább egy, a folyamatos átvitelű fenntartott egységre, amelyhez képest adatfolyam tartalom adatfolyam végpont.

##<a name="download-sample"></a>Töltse le a minta

Szerezze be és lebonyolítása minta [Itt](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>A .NET rendszerhez a Media Services SDK fejlesztési beállítása

1. Visual Studio segítségével console-alkalmazás létrehozása.
1. A Media Services NuGet csomag használata New-.NET hozzáadása a Media Services SDK csomagjában talál.

##<a name="connect-to-media-services"></a>Csatlakozás Media Services
Legjobb módszer egy app.config fájlt tárolja a Media Services nevét és a fiók billentyűt kell használnia.

>[AZURE.NOTE]A név és a kulcs értékek megkeresése az Azure portált, és jelölje ki a fiókját. Beállítások ablakban a jobb oldalon megjelenik. A beállítások ablakban jelölje ki a billentyűk. A minden szövegmező melletti ikonra kattintva másolja át az értéket, a rendszer a vágólapra.

A appSettings szakasz hozzáadása a app.config fájlt, és a Media Services nevét és a fiók fiókkulcs értékének beállítása.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Példa

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Mást keres?

Ha ez a témakör nem tartalmaznak, mi, várt, valamit, amit hiányzik, és a más módon nem felel meg az igényeinek, és adja meg, használja az alábbi Disqus szál visszajelzést.
