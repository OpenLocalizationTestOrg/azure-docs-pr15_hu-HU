<properties
    pageTitle="Digitális szöveggé alakíthatja szövegtartalom videofájlok az Azure Media Analytics használatával |} Microsoft Azure"
    description="Azure Media Analytics OCR (optikai karakterfelismeréssel) lehetővé teszi a videofájlok szövegtartalom szerkeszthető, kereshető digitális szöveggé alakíthatja.  Ez lehetővé teszi, hogy értelmes metaadatok kivonása, a videó jel médiafájlt a automatizálása."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Digitális szöveggé alakíthatja szövegtartalom videofájlok az Azure Media Analytics használatával 

##<a name="overview"></a>– Áttekintés

Ha tartalom szöveg kinyerése a videofájlokat és szerkeszthető, kereshető digitális szöveg készítése kell az Azure Media Analytics OCR (optikai karakterfelismeréssel) kell használnia. Ez az Azure Media processzor szövegtartalom megkeresi videofájlokat, és hozza létre a használatra szövegfájlokból. Optikai Karakterfelismerés lehetővé teszi, hogy értelmes metaadatok kivonása, a videó jel médiafájlt a automatizálása.

Együtt használják a keresőmotor, ha egyszerűen médiatartalmak index szöveget, és növeli a tartalom a feltárást. Ez akkor rendkívül hasznos erősen szöveges videóban, például egy videofelvétel vagy diavetítés a bemutató képernyőkép. Optikai Karakterfelismerés az Azure Media processzor digitális szöveg van optimalizálva.

Az **Azure Media OCR** media processzor jelenleg előzetes verzióban.

Ez a témakör **Az Azure Media OCR** tájékoztatást nyújt, és bemutatja, hogyan használhatja a Media Services SDK a .NET rendszerhez. További információk és példákat tekintse meg [a](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>Optikai Karakterfelismerés bemeneti fájlok

Videofájlokat. Jelenleg az alábbi formátumok támogatottak: MP4, MOV és WMV.

##<a name="task-configuration"></a>Tevékenység beállítása 

Tevékenység beállítása (beállítás). Feladat létrehozása az **Azure Media OCR**, meg kell adnia a készletet, használja a JSON vagy XML-konfiguráció. 

###<a name="attribute-descriptions"></a>Az attribútumok leírása

Attribútumnév|Leírás
---|---
Nyelvi|(nem kötelező), amelynek meg a szöveg nyelvének ismerteti. Az alábbiak egyikét: automatikus felismerése (alapértelmezett), arab, ChineseSimplified, ChineseTraditional, Cseh dán, holland, angol, finn, francia, német, görög, magyar, olasz, japán, koreai, norvég, lengyel, portugál, román, orosz, SerbianCyrillic, SerbianLatin, szlovák, spanyol, svéd, török.
TextOrientation|(nem kötelező), amelynek meg szöveg irányának ismerteti.  "Bal", az azt jelenti, hogy a felső részén a minden levél felé, a bal vannak nyíllá.  Az alapértelmezett szöveget (például, amely megtalálható a könyv) hívható "Felfelé" lépések.  Az alábbiak egyikét: automatikus felismerése (alapértelmezett), fel, jobbra, lefelé, balra.
TimeInterval|(nem kötelező) a mintavételnél ráta ismerteti.  Az alapértelmezett érték 1/2 másodpercenként.<br/>JSON formátum – ÓÓ. Biztonságos tár szolgáltatás (alapértelmezett 00:00:00.500)<br/>XML formátum – W3C XSD időtartam egyszerű (alapértelmezett PT0.5)
DetectRegions|(nem kötelező) Tömbképletet tartalmazó terület belül, amelyben szöveget feltárása a videoklip keretére DetectRegion objektumok.<br/>Az alábbi négy egész értékeket egy DetectRegion objektum történik:<br/>Bal – képpontokat, a bal margó<br/>Felső – a felső margó a képpont<br/>Szélesség – képpont-terület szélességének<br/>Magasság – a régió képpont magassága

####<a name="json-preset-example"></a>Előre definiált példa JSON
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>XML-előre definiált példa

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>Optikai Karakterfelismerés kimeneti fájlok

A kimenet a OCR media processzor JSON fájl.

###<a name="elements-of-the-output-json-file"></a>A kimeneti JSON fájl elemei

A videó OCR eredménye a videoklip található karakterek idő szegmentált adatok biztosít.  Telefonhívás modul pontosan azokat a szavakat, hogy érdeklik elemzése a attribútumokat például nyelvet és a tájolás is használhatja. 

A kimenet tartalmazza a következő attribútumok:

Elem|Leírás
---|---
Időskála|a videó másodpercenként "osztások"
Eltolás|az idő eltolása időbélyegeket. Videó API-khoz 1.0 verziójú a program mindig 0.
Képkockasebességhez|A videó másodpercenként vázak
szélesség|a videó képpont szélességét
magasság|a videó képpont magassága
Töredékek|a videó, amelybe a metaadatok darabolásos van időalapú mennyiségű tömb
indítása|Indítsa el a "osztások" egy kódrészletet időpontjának
időtartam|a részlet, a "osztások" hossza
intervallum|az egyes események belül az adott fragment intervallum
események|régiók tartalmazó tömb
régió|objektum, amely észlelt szavak vagy kifejezések
nyelvi|a szöveg egy régión belüli észlelt nyelvi
Tájolás|az észlelt a régión belüli szöveg irányának
sorok|egy régión belüli észlelt szövegsorok tömbje
szöveg|a tényleges szöveg

###<a name="json-output-example"></a>JSON kimeneti példa

Ez a példa kimeneti videó általános információk és a több video töredékek tartalmazza. A minden videó fragment minden terület, amit OCR MP nyelvvel észlelésekor tartalmazza, és az a szöveg irányát. A régió is tartalmaz, a sor szöveget, a sorpozíció és minden word adatok (word tartalmat, helyét és megbízhatóság) ebben a sorban a régióban minden word sort. Az alábbi képen, és néhány megjegyzések beágyazott tehettem.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Minta kódot.

A program jeleníti meg az alábbi útmutató:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl be az eszközt.
1. Létrehoz egy feladatot OCR konfigurációs/beállított fájl.
1. A kimenet JSON fájlokat tölt le. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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
