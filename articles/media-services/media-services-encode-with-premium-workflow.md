<properties 
    pageTitle="Speciális Media Encoder prémium munkafolyamat kódolást |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Media Encoder prémium munkafolyamattal kódolását. Mintakódok írt C#, és használja a Media Services SDK .NET." 
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

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Speciális kódolást Media Encoder prémium munkafolyamattal

>[AZURE.NOTE] Ebben a témakörben tárgyalt Media Encoder prémium munkafolyamat media processzor Kínában nem érhető el.

A prémium encoder kérdésekre e-mailben mepd a Microsoft.com webhelyről.

##<a name="overview"></a>– Áttekintés

Microsoft Azure Media Services van a **Media Encoder prémium munkafolyamat** media processzor bemutatása A processzor ajánlatok előzetes kódolása a prémium verzió igény szerinti munkafolyamatok funkciókhoz. 

A következő témakörök szerkezeti **Media Encoder prémium munkafolyamat**kapcsolatos részleteket: 

- [Prémium munkafolyamat Media Encoder által támogatott formátumok](media-services-premium-workflow-encoder-formats.md) – **Media Encoder prémium munkafolyamat**támogatja a fájlformátumok és kodekek foglalkozik.

- A [kódolók összehasonlítása](media-services-encode-asset.md#compare_encoders) szakasz összehasonlítja **Media Encoder prémium munkafolyamat** és **Media Encoder szabványos**kódolási lehetőségeit.

Ez a témakör bemutatja, hogyan lehet a **Media Encoder prémium munkafolyamat** .NET kódolását.

A **Media Encoder prémium munkafolyamat** kódolási feladatok szükség külön konfigurációs fájl, a munkafolyamat-fájl neve. Ezek a fájlok .workflow kiterjesztése, és a [Munkafolyamat-Tervező](media-services-workflow-designer.md) eszközzel létrehozott.

##<a name="encode"></a>Kódolását

A **Media Encoder prémium munkafolyamat** kódolási feladatok szükség külön konfigurációs fájl, a munkafolyamat-fájl neve. Ezek a fájlok .workflow kiterjesztése, és a [Munkafolyamat-Tervező](media-services-workflow-designer.md) eszközzel létrehozott.


Is megnyithatja az alapértelmezett munkafolyamat fájlok [Itt](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). A mappa is tartalmazza ezeket a fájlokat a leírását.

A munkafolyamat-fájlokat kell fel kell tölteni a Media Services fiókjába eszközként, és ez az eszköz kell átadni a a kódolási tevékenység.

A következő példa bemutatja, hogyan lehet **Media Encoder prémium**munkafolyamattal kódolását. 

Az alábbi lépéseket kell végrehajtani: 
 
1. Hozzon létre egy eszközt, és a munkafolyamat-fájl feltöltése. 
2. Hozzon létre egy eszközt, és töltse fel a forrás médiafájl.
3. Ismerkedés a "Media Encoder prémium munkafolyamat" media processzor.
4. Hozzon létre egy feladatot, és a tevékenység. 

    A legtöbb esetben a konfigurációs a tevékenység karakterlánca üres (például a következő példában). Forgatókönyv bizonyos speciális (igénylő meg a való futtatókörnyezet tulajdonságok dinamikusan) ebben az esetben a kódolási tevékenységhez XML-karakterlánc volna biztosít. Példák olyan esetek: létrehozása az átfedő, párhuzamos vagy egymás utáni adathordozóra, varrással, feliratozás.
5. Adja hozzá a két bemeneti eszközök a tevékenységhez.
    
    egy. a 1st – a munkafolyamat-eszközt.

    b. 2 – a videó eszköz.
    
    **Megjegyzés**: A munkafolyamat eszköz hozzá kell adnia a tevékenységhez a digitális eszköz előtt. Ehhez a feladathoz konfigurációs karakterláncban üres kell lennie. 

6. A kódolás feladat küldése

Egy kész példa a következő: További információt a Media Services .NET fejlesztési beállítása, című témakörben olvashat a [Media Services fejlesztési a .NET](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
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
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
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
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
            }
    
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }


A prémium encoder kérdésekre e-mailben mepd a Microsoft.com webhelyről.

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
