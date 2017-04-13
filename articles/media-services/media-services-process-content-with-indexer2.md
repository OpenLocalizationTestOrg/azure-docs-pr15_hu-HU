<properties
    pageTitle="Az Azure Media indexelő 2 előzetes médiafájlok indexelés |} Microsoft Azure"
    description="Azure Media indexelő teszi lehetővé, hogy a médiafájlokat a tartalom kereshető és a teljes szöveges átirat kódolt feliratok és kulcsszavakra létrehozásához. Ez a témakör bemutatja, hogyan Media indexelő 2 Preview használata."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Az indexelési médiafájlok Azure Media indexelő 2 előzetes verzióban

##<a name="overview"></a>– Áttekintés

Az **Azure Media indexelő 2 Preview** media processzor (MP) lehetővé teszi, hogy a médiafájlok és a tartalom kereshető tétele, valamint a kódolt feliratok számok készítése. Az [Azure Media indexelő](media-services-index-content.md)az előző verzióhoz képest, **Azure Media indexelő 2 Preview** elvégzi a gyorsabb az indexelés és kínálja-e több nyelv támogatása. A támogatott nyelvek közé tartozik, spanyol, német, angol, francia, olasz, kínai, portugál és arab.

Az **Azure Media indexelő 2 Preview** MP jelenleg előzetes verzióban.

Ez a témakör bemutatja, hogyan hozhat létre az indexelő feladatokat az **Azure Media indexelő 2 Preview**.

>[AZURE.NOTE]A következő érvényesek:
>
>Az Azure Kína és Azure kormányzati indexelő 2 nem támogatott.
>
>Az előnézet korlátozott feldolgozási ~ 10 perc, de minden vevőknek ingyenes.
>
>Az indexelés tartalmat, ügyeljen az használni a médiafájlokat, amelyek rendkívül egyszerű Beszédfelismerés (háttér zenét, a zajt, az effektusok és a mikrofon hiss) nélkül. Néhány példa a megfelelő tartalom: a rögzített értekezletek, a előadások vagy a bemutatókat. A következő tartalom nem lehet készítéshez alkalmas: filmek, tévéműsorok, bármilyen fájlok vegyes hang- és hangokat, a rosszul rögzíti a háttérzajt (hiss) tartalma.


Ez a témakör az **Azure Media indexelő 2 Preview** tájékoztatást nyújt, és bemutatja, hogyan használhatja a Media Services SDK a .NET rendszerhez

##<a name="input-and-output-files"></a>Bemeneti és kimeneti fájlok

###<a name="input-files"></a>A bemeneti fájlok

Hang- vagy videofájlok tárolásához

###<a name="output-files"></a>Kimeneti fájlok

Egy indexelési feladat kódolt felirat fájlokat hozhat létre, a következő formátumokban:  

- **SZÁMI**
- **TTML**
- **WebVTT**

A következő formátumú fájlok is használható, hogy a hang-és videofájlok hallja a fogyatékkal élők CC felirat lezárva.

##<a name="task-configuration-preset"></a>Tevékenység beállítása (állomás)

Az indexelési feladat létrehozása az **Azure Media indexelő 2 Preview**, meg kell adnia egy konfigurációs készletet.

A következő JSON paramétereinek érhető el.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>A támogatott nyelvek  

Azure Media indexelő 2 Preview beszéd-szöveg (ha az alább látható módon megadása a tevékenység konfiguráció használata 4-karakterkód szögletes zárójelek között a nyelv neve) a következő nyelvet támogatja:

- Angol [EnUs]
- Spanyol [EsEs]
- Kínai [ZhCn]
- Francia [FrFr]
- Német [DeDe]
- Olasz [ItIt]
- Portugál [PtBr]
- Arab (egyiptomi) [ArEg]


## <a name="sample-code"></a>Minta kódot.

A program jeleníti meg az alábbi útmutató:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl be az eszközt.
1. Az indexelési feladat, amely tartalmazza a következő json készletet konfigurációs fájl alapján létrehoz egy feladatot.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. A kimenet fájlokat tölt le. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
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
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
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


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Kapcsolódó hivatkozások

[Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)

[Azure Media Analytics-bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)