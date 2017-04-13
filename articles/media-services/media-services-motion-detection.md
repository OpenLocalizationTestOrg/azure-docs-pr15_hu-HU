<properties
    pageTitle="Az Azure Media Analytics mozgásokkal észleli |} Microsoft Azure"
    description="Az Azure Media Mozgásvonalas jelentő media processzor (MP) lehetővé teszi, hogy hatékony az egyes szakaszokhoz kamat-egyébként hosszú és Eseménytelen videó belül."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Az Azure Media Analytics mozgásokkal feltárása

##<a name="overview"></a>– Áttekintés

Az **Azure Media Mozgásvonalas jelentő** media processzor (MP) lehetővé teszi, hogy hatékony az egyes szakaszokhoz kamat-egyébként hosszú és Eseménytelen videó belül. Mozgásvonalas észlelése statikus kamera képanyag hol fordul elő a mozgásvonalak videó szakaszok azonosítása használható. A metaadat-alapú időbélyegeket és a határoló régió, ahol az esemény bekövetkezett tartalmazó JSON fájl hoz létre.

Célzott biztonsági videó hírcsatornák felé, a technológia el tudja mozgásvonalas sorolni fontos események és téves például árnyékok, és a megvilágítás módosításokat. Ez lehetővé teszi a biztonsági figyelmeztetések készítése a kamera hírcsatornák anélkül, hogy éppen címünkre végtelen lényegtelen eseményekkel, miközben érdeklődésre számot tartó percet kinyerése rendkívül hosszú felügyeleti videókat is.

Az **Azure Media Mozgásvonalas jelentő** MP jelenleg előzetes verzióban.

Ez a témakör az **Azure Media Mozgásvonalas jelentő** tájékoztatást nyújt, és bemutatja, hogyan használhatja a Media Services SDK a .NET rendszerhez


##<a name="motion-detector-input-files"></a>Mozgásvonalas jelentő bemeneti fájlok

Videofájlokat. Jelenleg az alábbi formátumok támogatottak: MP4, MOV és WMV.

##<a name="task-configuration-preset"></a>Tevékenység beállítása (állomás)

Feladat létrehozása az **Azure Media Mozgásvonalas jelentő**, meg kell adnia egy konfigurációs készletet. 

###<a name="parameters"></a>Paraméterek

A következő paraméterek használhatók:

név|Beállítások|Leírás|Alapértelmezett
---|---|---|---
sensitivityLevel|Karakterlánc: "nem", "közepes", "magas"|A magánjellegű szint beállítása, mely mozgásokkal jelenteni. Állítsa be az ez téves mennyiségét beállításához.|"közepes"
frameSamplingValue|Pozitív egész szám|Melyik algoritmus futtatja a gyakoriságát állítja be. 1 minden keret egyenlő, 2 azt jelenti, hogy minden 2nd keretet, és így tovább.|1
detectLightChange|Logikai: "true", "false"|Állítja be, hogy egyszerűsített módosítások jelentik a találatok között|"False"
mergeTimeThreshold|Xs és idő: Óó<br/>Példa: 00:00:03|Adja meg a időkeret mozgásvonalas eseményeket, ahol 2 esemény fog kombinált és 1, jelentése között.|00:00:00
detectionZones|Egy tömb észlelési zónáinak:<br/>-Észlelési zóna egy tömb 3 vagy több pont<br/>-Pont egy x és y koordinálása 0-tól 1.|A használandó sokszög észlelési zónák listáját ismerteti.<br/>Egy ID, az első egy éppen "azonosító" zónában fog jelenteni az eredményeket: 0|Egyetlen zónában, amely a teljes keret foglalkozik.

###<a name="json-example"></a>Példa JSON

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Mozgásvonalas jelentő kimeneti fájlok

A mozgásvonal észlelése JSON fájl ad vissza, a kimeneti eszköz mozgást figyelmeztetések és a kategóriák belül a videó ismerteti, amelyek a. A fájl az idő és a videoklip található mozgásvonalas időtartam információt fog tartalmazni.

A mozgásvonal jelentő API biztosít a jelölőket, ha vannak olyan objektumok mozgást rögzített háttér videó (például a felügyelet video). A mozgásvonal jelentő van képzett téves riasztást, például a megvilágítás elemre, és az árnyék változások csökkentése érdekében. Aktuális algoritmusok néhány korlátozás éjszakai látás videók, félig átlátszó színű objektumok és kisméretű objektumokat.

###<a id="output_elements"></a>A kimeneti JSON fájl elemei

>[AZURE.NOTE]A legújabb verzióban a kimeneti JSON formátum módosítását, és az egyes felhasználók a legfrissebb módosítások jelöl.

Az alábbi táblázat ismerteti a kimeneti JSON fájl elemei.

Elem|Leírás
---|---
Verzió|Ez a videó API verziójának vonatkozik. A jelenlegi verzióval értéke 2.
Időskála|A videó másodpercenként "osztások".
Eltolás|A "osztások" időbélyegeket idő eltolás. Videó API-khoz 1.0 verziójú a program mindig 0. Jövőbeli támogatjuk forgatókönyvek, ez az érték változhat.
Képkockasebességhez|A videó másodpercenként keretek.
Szélesség, magasság|Szélessége és magassága a videó képpont hivatkozik.
Indítása|A kezdő időbélyegző, a "osztások".
Időtartam|Az esemény "osztások" hosszát.
Intervallum|Mindegyik tétele a rendezvény részleteivel, a "osztások" időtartományt.
Események|Minden esemény fragment tartalmazza az adott időtartamot belül észlelhető mozgást.
Típus|A jelenlegi verzióval az mindig "2" általános mozgást. A címke ad videó API-k rugalmasan kategorizálásához mozgás jövőbeli verzióiban.
RegionID|Fent bemutatottak a program mindig az ebben a verzióban 0. Ez a címke videó API mozgásvonalas keresheti meg a jövőbeli verzióiban különböző régiók rugalmasan adja vissza.
Területek|A videó hol Önt érdeklő mozgásvonalak területen hivatkozik. <br/><br/>-"azonosító", amely a régió terület – verziójában van csak egyet, 0 azonosítója. <br/>-"típus" az alakzat a régió, a mozgásvonalak érdeklő jelöli. Jelenleg "téglalap" és "sokszög" támogatott.<br/> Ha megadott "téglalap", van-e a régió méretek az X, Y, szélességét és magasságát. Az X és Y koordináták a 0,0-1,0 normalizált skálája területről felső bal oldali XY koordinátáit jelenítik meg. A szélesség és magasság jelenítik meg a 0,0-1,0 normalizált skálája területről méretét. A jelenlegi verzióval X, Y, szélességét és magasságát mindig rögzített 0, a 0 és 1, 1. <br/>Ha megadott "sokszög", a régió méretek pontok tartalmaz. <br/>
Töredékek|A metaadatok töredékek nevű különböző részekre fel van darabolásos. Minden részlet egy Kezdés, időtartam, intervallum száma és esemény tartalmazza. Nincs eseményekkel egy részlet, az azt jelenti, hogy nincs mozgásvonalas észlelt a kezdő időpont és a időtartam során.
Szögletes zárójelek]|Minden egyes zárójel abban az esetben egy intervallum jelöli. Üres szögletes az intervallum azt jelenti, hogy nincs mozgásvonalas talált.
helyek|Az új bejegyzés események csoportban a helyet, ahol a mozgásvonalak történt sorolja fel. Az észlelési zónában mint szűkebb.

Az alábbiakban a JSON kimeneti példa

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Korlátozások

- A beviteli támogatott videoformátumok MP4, MOV és WMV tartalmazza.
- Mozgásvonalas észlelése álló háttér videók van optimalizálva. A algoritmus koncentrál téves riasztást, például a megvilágítás módosításait, és a árnyékok csökkentése.
- Néhány mozgásvonalas nem észlelhető miatt technikai problémáit; például az éjszakai látás videók, félig átlátszó színű objektumok és kisméretű objektumokat.


## <a name="sample-code"></a>Minta kódot.

A program jeleníti meg az alábbi útmutató:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl be az eszközt.
1. Videó mozgásvonalas észlelési feladat, amely tartalmazza a következő json készletet konfigurációs fájl alapján létrehoz egy feladatot. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. A kimenet JSON fájlokat tölt le. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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
[Azure Media Services Mozgásvonalas jelentő-blog](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)

[Azure Media Analytics-bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
