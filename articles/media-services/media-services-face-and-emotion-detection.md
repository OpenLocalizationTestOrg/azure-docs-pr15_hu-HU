<properties
    pageTitle="Észleli arc és az Azure Media Analytics Emotion |} Microsoft Azure"
    description="Ez a témakör bemutatja, hogyan arc és az Azure Media Analytics érzelmek feltárása."
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
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Arc és az Azure Media Analytics Emotion feltárása

##<a name="overview"></a>– Áttekintés

Az **Azure Media arc jelentő** media processzor (MP) lehetővé teszi, hogy a száma, a mozgásának nyomon, és a még figyelheti a hallgatóság részvételét és reakció arc kifejezések keresztül. Ez a szolgáltatás két funkciókat tartalmazza: 

- **Arc kimutatására**

    Arc észlelési keres, és nyomon követi a videó emberi lapjaira. Több arcot is az általuk észlelt, és ezt követően kell nyomon követi azokat Navigálás, az időt és a hely metaadatokkal JSON fájlban adja vissza. A nyomon követés során akkor próbálja konzisztens Azonosítóra adni az azonos arc közben a személy található beszúrási pont áthelyezése a képernyőn, ha vannak a probléma egyik oka, vagy röviden elhagyja a keret.

    >[AZURE.NOTE]Ez a szolgáltatás nem hajt végre arc felismerés. Olyan személy, aki elhagyja a keret vagy válik a probléma egyik oka az túl sokáig kap egy új Azonosítót amikor vissza őket.

- **Emotion kimutatására**
    
    Emotion észlelési része egy nem kötelező a arc észlelési Media processzor, amely analysis több érzelmi attribútumain köztük Boldogsága, sadness, félelem, utasítás és más észlel, arc vissza. 

Az **Azure Media arc jelentő** MP jelenleg előzetes verzióban.

Ez a témakör részletezi az **Azure Media arc jelentő** kapcsolatos, és bemutatja, hogyan használhatja a Media Services SDK a .NET rendszerhez.

##<a name="face-detector-input-files"></a>Arcra jelentő bemeneti fájlok

Videofájlokat. Jelenleg az alábbi formátumok támogatottak: MP4, MOV és WMV.

##<a name="face-detector-output-files"></a>Arcra jelentő kimeneti fájlok

A arc észlelése és nyomon követés API biztosít a nagy pontosságú arc hely észlelési és a nyomon követés legfeljebb 64 emberi arc képes észlelni a videoklipet. Elülső arc adja meg a legjobb eredményt, oldal arc közben és a kis ARC (legfeljebb 24 x 24 képpontra) előfordulhat, hogy nem teljesen pontosak a.

Az észlelt és követett arc koordináták (balra, top, szélesség és magasság) szerepeljenek eredményként a képet képpont, valamint a nyomon követés, hogy egyes jelölő ARC azonosító számot arc helyét jelző. Arc Tevékenységazonosító számait jobban alaphelyzetbe állítása körülmények között, amikor a elülső arc elveszett vagy a keretben, átfedésben eredményezi az egyes személyek első rendelt több azonosítók.

###<a id="output_elements"></a>A kimeneti JSON fájl elemei

Arc felderítése és nyomon követése a műveletet a kimeneti eredmény tartalmazza az adott fájlhoz JSON formátumban lapjaira a metaadatok.

A arc észlelése és a követés JSON tartalmazza a következő attribútumok:

Elem|Leírás
---|---
Verzió|Ez a videó API verziójának vonatkozik.
Időskála|A videó másodpercenként "osztások".
Eltolás|Ez a eltolódás az időbélyegeket. Videó API-khoz 1.0 verziójú a program mindig 0. Jövőbeli támogatjuk forgatókönyvek, ez az érték változhat.
Képkockasebességhez|A videó másodpercenként keretek.
Töredékek|A metaadatok töredékek nevű különböző részekre fel van darabolásos. Minden részlet egy Kezdés, időtartam, intervallum száma és esemény tartalmazza.
Indítása|Az első "osztások" esemény kezdetének.
Időtartam|A részlet, a "osztások" hosszát.
Intervallum|A részlet, a "osztások" minden esemény tétel időtartományt.
Események|Egyes események észleli és nyomon követni, hogy időtartamot belül a arc tartalmazza. A teljes tömböt tömbje. A külső tömböt egy időtartományt jelöli. A belső tömb, ahol a 0 és abban az időpontban történt további eseményeket. Egy üres zárójel [] azt jelenti, hogy nincs arc talált.
AZONOSÍTÓ| A arc, amely a követett azonosítója. Ennyi véletlenül megváltozhat, ha a rendszer nem észleli a arc. Egy adott személy videó teljes egészében ugyanazon Azonosítóval kell rendelkeznie, de ez az észlelési algoritmus (hangelnyelés stb.) a korlátai miatt nem lehet garantálni
X, Y|Az X bal felső sarokban és Y koordinátáit a normalizált skálával 0,0-1,0 mezőjében határoló arc. <br/>-X és Y koordináták törzsben mindig, fekvő, ha egy álló videó (vagy fejjel lefelé, az iOS esetében), a koordináták megfelelően Transzponálás is.
Szélesség, magasság|Szélességét és magasságát a normalizált skálával 0,0-1,0 mezőjében határoló arc.
facesDetected|Ez a JSON eredmények végén található, és foglalja össze, amely során a videó észlelt a algoritmus számát. Mivel az azonosítók vissza tudja állítani véletlenül Ha a rendszer nem észleli a ARC (pl. arc kerül képernyőn nem vagyok a gépnél így néz ki), a szám nem előfordulhat, hogy mindig egyenlő a videóban arc igaz számát.

Arc jelentő használja a töredezettség (ahol a metaadatok is tört az időalapú szövegadattömb és letöltheti a csak a keresett) és a szegmens technikákat (ahol az események megszakadnak abban az esetben, ha túl nagy egyre). Néhány egyszerű számítások segítséget nyújtanak az adatokat. Ha például egy esemény lépések 6300 (osztások), a 2997 (osztások/sec) az időskála és 29,97 (keretek/sec), majd a képkockasebességhez:

- Kezdés/időskála = 2.1-es másodperc
- Másodperc (képkockasebességhez/időskála) x = 63 vázak

Az alábbi példája egy egyszerű kiolvasó a JSON be egy / keret formátum arc észlelési és nyomon követése:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Arcra észlelési bemeneti és kimeneti példa

###<a name="input-video"></a>Beviteli videó

[Beviteli videó](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Tevékenység beállítása (állomás)

Feladat létrehozása az **Azure Media arc jelentő**, meg kell adnia egy konfigurációs készletet. A következő konfigurációs beállítások csak arc észlelése nem.

    {"version":"1.0"}

###<a name="json-output"></a>JSON kimeneti

Az alábbi példa a JSON kimenet csonkolt.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Bemeneti és kimeneti Emotion észlelési példa


###<a name="input-video"></a>Beviteli videó

[Beviteli videó](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Tevékenység beállítása (állomás)

Feladat létrehozása az **Azure Media arc jelentő**, meg kell adnia egy konfigurációs készletet. A következő konfigurációs készletet emotion kimutatását alapján JSON létrehozásához adja meg.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Az attribútumok leírása

Attribútumnév|Leírás
---|---
Mód|Arc: Csak a arcra észlelési <br/>AggregateEmotion: Minden lap keretben visszatérési átlagos emotion értékei.
AggregateEmotionWindowMs|Ha AggregateEmotion módban kiválasztott használja. Minden egyesített eredményét, ezredmásodpercben összegyűjtéséhez használt videó hosszát adja meg.
AggregateEmotionIntervalMs|Ha AggregateEmotion módban kiválasztott használja. Itt adhatja meg, milyen gyakorisággal összesítő találatok összegyűjtéséhez.

####<a name="aggregate-defaults"></a>Összesítő alapértékek

Alatti ajánlott összesítő ablakot, és intervallum beállításainak. AggregateEmotionWindowMs AggregateEmotionIntervalMs hosszabb kell lennie.

   |Alapértelmezett (vasárnap)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON kimeneti

A JSON (a program csak) összesítő emotion kimenete:
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Korlátozások

- A beviteli támogatott videoformátumok MP4, MOV és WMV tartalmazza.
- Észlelhető arc méret tartománya 24 x 24 való 2048 x 2048 képpont. A lapok a tartományon kívül észlelése nem történik.
- Minden egyes videók esetén a visszaadott arc maximális száma 64.
- Néhány arc technikai problémáit; miatt nem észlelhető nagyméretű pl. arc többszöröseire (címsor-jelentő), és a nagy hangelnyelés. Elülső és közelében elülső arc van a legjobb eredményt.


## <a name="sample-code"></a>Minta kódot.

A program jeleníti meg az alábbi útmutató:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl be az eszközt.
1. Létrehoz egy feladatot arc észlelési feladat, amely tartalmazza a következő json készletet konfigurációs fájl alapján. 
                    
        {
            "version": "1.0"
        }

1. A kimenet JSON fájlokat tölt le. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Kapcsolódó hivatkozások

[Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)

[Azure Media Analytics-bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)