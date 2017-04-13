<properties 
    pageTitle="Azure Media Services tartalomhoz .NET közzététele" 
    description="Megtudhatja, hogy miként hozhat létre egy megnevezés adatfolyam URL-cím létrehozásához használt. Mintakódok írt C#, és használja a Media Services SDK .NET." 
    authors="juliako" 
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


# <a name="publish-azure-media-services-content-using-net"></a>Azure Media Services tartalomhoz .NET közzététele
 
> [AZURE.SELECTOR]
- [TÖBBI](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Portál](media-services-portal-publish.md)

##<a name="overview"></a>– Áttekintés

Egy adaptív átviteli sebesség MP4 beállítása: egy OnDemand adatfolyam megnevezés, valamint épület adatfolyam URL-címet is adatfolyamként. A [Tárgyi eszköz kódolást](media-services-encode-asset.md) témakörben megtudhatja, hogy miként kódolását egy adaptív átviteli sebesség MP4 állítsa be. 

>[AZURE.NOTE]Ha a tartalom titkosítva van, állítsa be eszköz kézbesítési házirend ( [Ebben](media-services-dotnet-configure-asset-delivery-policy.md) a témakörben leírtak) egy megnevezés létrehozása előtt. 

Egy megnevezés streaming OnDemand fokozatosan letölthető MP4-fájlok mutató URL-össze is használhatja.  

Ez a témakör bemutatja, hogyan hozhat létre egy OnDemand a folyamatos átvitelű megnevezés tegye közzé a eszköz és egy sima MPEG szaggatott és a folyamatos átvitelű URL-címek HLS összeállítása. Azt is láthatók lesznek hideg, létrehozásához a progresszív letöltés URL-ek. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Hozzon létre egy a folyamatos átvitelű megnevezés OnDemand

A folyamatos átvitelű megnevezés OnDemand létrehozása és az URL-címek első kell tegye a következőket:

   1. Ha a tartalom titkosítva van, az access-házirend megadása
   2. Hozzon létre egy a folyamatos átvitelű megnevezés OnDemand.
   3. Adatfolyam-tervezi, ha fájlletöltés az adatfolyam nyilvánvalóan (.ism) az eszközt. 
        
    Ha fokozatosan töltheti le, beszerzése a MP4-fájlok eszköz nevét.  
   4. URL-címek összeállítása a nyilvánvalóan fájl vagy MP4-fájlokat. 
   

###<a name="use-media-services-net-sdk"></a>.NET SDK Media Services használata 

Adatfolyam URL-címek összeállítása 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

A kód kimeneti értékeket:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]A tartalom is lehet adatfolyamként, SSL-kapcsolaton keresztül. Ehhez ellenőrizze, hogy a továbbított URL-címek Kiindulás HTTPS-e. 

Összeállítása a progresszív letöltés URL-címei 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

A kód kimeneti értékeket:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Használja a Media Services .NET SDK-bővítmények

A következő kódot, amely egy megnevezés létrehozása és a folyamatos zökkenőmentes, HLS és MPEG-vonal URL-címek készítése adaptív streaming .NET SDK bővítmények módszerek hívások.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még:

[Töltse le az eszközök](media-services-deliver-asset-download.md)
[eszköz kézbesítési házirend konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md)
