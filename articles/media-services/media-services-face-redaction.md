<properties
    pageTitle="Az Azure media analytics kivonási arcra |} Microsoft Azure"
    description="Ez a témakör bemutatja, hogyan lehet az Azure media analytics arc kivonás."
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
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Az Azure media analytics arc kivonási

##<a name="overview"></a>– Áttekintés

**Azure Media Redactor** az [Azure Media Analytics](media-services-analytics-overview.md) -media processzor (MP), amely felajánlja a felhőben méretezhető arc kivonási. Arc kivonási lehetővé teszi, hogy a videoklip módosíthatja a kijelölt személyek arc életlenítés érdekében. Előfordulhat, hogy használni kívánt a arc kivonási szolgáltatás nyilvános biztonsági és hírek media helyzetekben. Kivonás manuálisan az órát is igénybe vehet a Képanyag több ARC tartalmazó néhány percet, de ez a szolgáltatás a arc kivonási folyamat esetén kell néhány egyszerű lépéssel. További tudnivalókért lásd: a [blogban](https://azure.microsoft.com/blog/azure-media-redactor/) .

Ez a témakör részletezi az **Azure Media Redactor** kapcsolatos, és bemutatja, hogyan használhatja a Media Services SDK a .NET rendszerhez.

Az **Azure Media Redactor** MP jelenleg előzetes verzióban.

## <a name="face-redaction-modes"></a>Arc kivonási módok

Arc kivonási működik, arc kimutatására minden videó keretét, és a továbbküldéseket és a korábbi verziókkal időben, az ARC objektum mindkét nyomon követése, hogy az azonos személy is homályos, a más szögben is. Az automatikus kivonási folyamat nagyon összetett bejegyzései, és nem nem mindig termék 100 %-os kívánt kimeneti, ezért Media Analytics, a végleges eredménye módosítására pár.

Mellett a teljesen automatikus módban van, amely lehetővé teszi, hogy a kijelölés/inaktiválása-selection a via azonosítók listája megtalálható arc két-fázis munkafolyamat. Is, hogy a MP keret korrekciók egy tetszőleges JSON formátumban metaadatok fájlt használ. Ez a munkafolyamat oszlik **elemzés** és **Redact** módban. Kombinálhatja egymással a két üzemmód egy menetben, amely egy feladatot; mindkét feladatok fut Ebben az üzemmódban **kombinált**neve.

###<a name="combined-mode"></a>Kombinált mód

Ez hoznak létre egy kivont mp4 automatikusan bármely bevitel kézi nélkül.

Szakasz|Fájlnév|Jegyzetek
---|---|---
Beviteli eszköz|foo.bar|Videó WMV, MOV vagy MP4 formátumban
Beviteli config|Feladat konfigurációs előre definiált|{"verziója": "1.0',"beállítások": {"mód":"kombinált"}}
Kimeneti eszköz|foo_redacted.mp4|Az alkalmazott életlenítés videó

####<a name="input-example"></a>Beviteli példa:

[Ez a videó megtekintése](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Példa a kimenet:

[Ez a videó megtekintése](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Mód elemzése

A két-fázis munkafolyamat **elemzése** menetében egy videó beviteli tart, és arc helyeken JSON fájl, és az egyes észlelt arc jpg képek hoz létre.

Szakasz|Fájlnév|Jegyzetek
---|---|----
Beviteli eszköz|foo.bar|Videó WMV, MPV vagy MP4 formátumban
Beviteli config|Feladat konfigurációs előre definiált|{"verziója": "1.0',"beállítások": {"mód":"elemzés"}}
Kimeneti eszköz|foo_annotations.JSON|Jegyzet adatokat arc helyeken JSON formátumban. Ez a mezőkben határoló életlenítés módosításához a felhasználó által szerkeszthető. Az alábbi példa látható.
Kimeneti eszköz|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Az egyes levágott jpg észlelt arc, ahol azt jelzi, a számot a labelId, a arc

####<a name="output-example"></a>Példa a kimenet:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… a program csak


###<a name="redact-mode"></a>Kivonás mód

A második a munkafolyamat-fázis, amely egyetlen eszköz egyesíthetők ráfordítások nagyobb számot vesz igénybe.

Ide tartoznak a azonosítók lehetőséget, az eredeti videó és a széljegyzeteket JSON listáját. Ebben a módban használja a széljegyzeteket alkalmazni, kattintson a videó bemeneti életlenítés.

Az elemzés fázis kimenete nem része az eredeti videó. A videó be a bemeneti eszköz a Redact mód tevékenység feltöltés és az elsődleges fájlként kijelölt kell.

Szakasz|Fájlnév|Jegyzetek
---|---|---
Beviteli eszköz|foo.bar|Videó WMV, MPV vagy MP4 formátumban. Ugyanaz, ahogy lépés: 1 videó.
Beviteli eszköz|foo_annotations.JSON|Metaadat-alapú fájlt széljegyzetek fázis: egy választható módosításokkal.
Beviteli eszköz|foo_IDList.txt (nem kötelező)|Nem kötelező új sor tagolt arc kivonás azonosítók listája. Ha üresen hagyja a mezőt, ez az összes arc életleníti.
Beviteli config|Feladat konfigurációs előre definiált|{"verziója": "1.0',"beállítások": {"mód":"kivonás"}}
Kimeneti eszköz|foo_redacted.mp4|A videó az alkalmazott életlenítés széljegyzetek alapján

####<a name="example-output"></a>Példa eredménye

Az egy kijelölt egy azonosítójú IDList kimenetét.

[Ez a videó megtekintése](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Az attribútumok leírása

A kivonás MP nagy pontosságú arc hely észleli és nyomon követése, amely legfeljebb 64 emberi arc észleli videó körül. Elülső arc a legjobb eredményt adja, oldal arc közben és a kis ARC (legfeljebb 24 x 24 képpontra) nehéz.

Az észlelt és követett arc jelző a nyomon követés, hogy egyedi azonosító visszaadott arc, valamint egy oldallal helyét jelző adatokkal. Arc Tevékenységazonosító számait jobban alaphelyzetbe állítása körülmények között, amikor a elülső arc elveszett vagy a keretben, átfedésben eredményezi az egyes személyek első rendelt több azonosítók.

Az attribútumok részletes magyarázatot témakört [arc észleli és az Azure Media Analytics Emotion](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Minta kódot.

A program jeleníti meg az alábbi útmutató:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl be az eszközt.
1. Hozzon létre egy feladatot egy ARC kivonási feladat, amely tartalmazza a következő json készletet konfigurációs fájl alapján. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. A kimenet JSON fájlok letöltése elemére. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Kapcsolódó hivatkozások

[Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)

[Azure Media Analytics-bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
