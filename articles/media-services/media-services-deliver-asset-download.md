<properties 
    pageTitle="Tartalom letöltése" 
    description="Ismerkedjen meg az információ eszközök letöltése a számítógépre. Mintakódok írt C#, és használja a Media Services SDK .NET." 
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
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Útmutató: tárgyi eszköz letölthető olvasása

Ez a témakör azt ismerteti, hogy a Media Services feltöltött multimédiás elemeket a Kézbesítési beállítások. Számos alkalmazás helyzetekben tartana Media Services tartalmat. Töltse le a multimédiás elemeket, vagy egy megnevezés segítségével elérheti őket. Médiatartalom elküldheti egy másik alkalmazás, vagy egy másik tartalomszolgáltató. Nagyobb teljesítmény és méretezhetőség esetén is tartana tartalmat a tartalom kézbesítési hálózati (CDN) használatával.

Ez a példa bemutatja, hogyan multimédiás elemeket a helyi számítógépre töltse le a Media Services. A kód lekérdezi a feladat azonosító szerint a Media Services-fiókhoz kapcsolódó feladatok és fér hozzá a **OutputMediaAssets** webhelycsoport (amely a kimeneti multimédiás elemeket egy vagy több készletét, amely a feladat futását). Ez a példa szemlélteti töltse le a kimeneti multimédiás elemeket egy feladatot, de más eszközökre töltheti le ugyanazon módszert alkalmazhat.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Lásd még: 

[Adatfolyam-tartalmak olvasása](media-services-deliver-streaming-content.md)

