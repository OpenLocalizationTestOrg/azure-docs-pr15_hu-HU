<properties
    pageTitle="A videó összefoglaló létrehozásához használja az Azure Media Video miniatűrök |} Microsoft Azure"
    description="Videó összefoglaló segíthetnek a forrás videóban érdekes kódrészletek automatikusan választva hosszú videók összegzéseinek hozhat létre. Ez akkor hasznos, ha meg szeretné, hogy mire számíthat, hosszú a videó gyors áttekintést nyújt."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>

#<a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Hozzon létre egy videó összefoglaló az Azure Media Video miniatűrök használatával
##<a name="overview"></a>– Áttekintés

Az **Azure Media Video miniatűrök** media processzor (MP) hozhat létre, amely hasznos lehet ahhoz, hogy ügyfelek, akik csak meg szeretné tekinteni a hosszú videó összefoglalását videó összefoglalását. Például ügyfelek előfordulhat, hogy szeretné, hogy "összefoglaló video" rövid amikor azok vigye az egérmutatót egy miniatűr fölé viszi. A paraméterek keresztül egy konfigurációs készletet az **Azure Media Video miniatűrök** színcsúszkákkal, használhatja egy leíró subclip algorithmically létrehozásához a MP hatékony fényképezés észlelése és összefűzés technológia.  

Az **Azure Media Video miniatűr** MP jelenleg előzetes verzióban.

Ez a témakör az **Azure Media Video miniatűr** tájékoztatást nyújt, és bemutatja, hogyan használhatja a Media Services SDK a .NET rendszerhez

##<a name="video-summary-example"></a>Videó összefoglaló példa 

Íme néhány példa az Azure Media videó miniatűrjét media processzor teendők:

###<a name="original-video"></a>Eredeti videó

[Eredeti videó](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

###<a name="video-thumbnail-result"></a>Videó miniatűr eredménye

[Videó miniatűr eredménye](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="task-configuration-preset"></a>Tevékenység beállítása (állomás)

Videó miniatűr feladat az **Azure Media Video miniatűrök**létrehozásakor meg kell adnia egy konfigurációs készletet. A fenti miniatűr mintában a következő egyszerű JSON beállításokkal hozták létre:

    {"version":"1.0"}

Jelenleg a következő paraméterek módosíthatja:

Paraméter|Leírás
---|---
outputAudio|Azt adja meg, függetlenül attól a eredő videó tartalmaz-e a hangot. <br/>Engedélyezett értékei a következők: IGAZ vagy hamis. Alapértelmezett értéke igaz.
fadeInFadeOut|Adja meg a külön mozgásvonalas miniatűrök között használt-e az elhalványulás átmenetek.  <br/>Engedélyezett értékei a következők: IGAZ vagy hamis.  Alapértelmezett értéke igaz.
maxMotionThumbnailDurationInSecs|Az egész szám, amely meghatározza, hogy mennyi ideig kell a teljes eredő videót.  Alapértelmezett video időtartam eredeti függ.


Az alábbi táblázat ismerteti az alapértelmezett időtartamot, amikor **maxMotionThumbnailInSecs** nem használatos.

||||
---|---|---|---|---
Videó időtartam|d < 3 perc|3 perc < d < 15 perc|15 perc < d < 30 perc| 30 perc < d
A miniatűrök időtartam|15 másodperc (színfalak 2-3)| 30 másodperc (színfalak 3-5)|60 sec (színfalak 5-10)|90 sec (színfalak 10 – 15)


A következő JSON paramétereinek érhető el.
    
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="sample-code"></a>Minta kódot.

A program jeleníti meg az alábbi útmutató:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl be az eszközt.
1. Videó miniatűr feladat, amely tartalmazza a következő json készletet konfigurációs fájl alapján létrehoz egy feladatot. 
        
        {               
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }

1. A kimenet fájlokat tölt le. 

###<a name="net-code"></a>.NET-kód
     
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace VideoSummarization
    {
        class Program
        {
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
    

                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");
    
                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }
    
            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);
    
                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");
    
                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";
    
                var processor = GetLatestMediaProcessorByName(MediaProcessorName);
    
                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);
    
                // Specify the input asset.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
    
                progressJobTask.Wait();
    
                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);
    
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);
    
                return asset;
            }
    
            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }
    
            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));
    
                return processor;
            }
    
            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
    
                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
        }
    }

###<a name="video-thumbnail-output"></a>Videó miniatűr kimenet

[Videó miniatűr kimenet](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Kapcsolódó hivatkozások

[Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)

[Azure Media Analytics-bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)