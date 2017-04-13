<properties 
    pageTitle="Azure várólista-tároló használata a Lync-Media Services feladat értesítéseinek a .NET |} Microsoft Azure" 
    description="Megtudhatja, hogy miként figyelheti a Media Services feladat értesítéseinek Azure várólista-tároló használatával. A kód minta írott C#, és használja a Media Services SDK a .NET rendszerhez." 
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
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Azure várólista-tároló használata a Lync-Media Services feladat értesítéseinek a .NET

Feladatok futtatásakor gyakran szükség feladat-végrehajtás nyomon követése lehetőséget. Ellenőrizni a haladás Azure várólista tároló használata a Lync-Media Services feladat értesítéseinek (ebben a témakörben leírtak) vagy egy StateChanged eseménykezelő definiálásával, ( [Ebben](media-services-check-job-progress.md) a témakörben leírtak.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Azure várólista-tároló használata a Lync-Media Services feladat értesítések

Microsoft Azure Media Services használhat az előadáshoz értesítését az [Azure várólista-tároló](../storage-dotnet-how-to-use-queues.md#what-is) media feladatok feldolgozása közben lehetősége van. Ez a témakör bemutatja az alábbi értesítését beszerzése várólista tárhelyről.

Várólista tárolóhoz beérkező üzenetek webböngészőn keresztül elérhetők bárhol a világon. Az Azure várólista architektúra üzenetküldés a megbízható és erősen méretezhető. Lekérdezési várólista-tároló ajánlott más módszerekkel fölé. 

Egy Media Services értesítések meghallgatásához leggyakoribb helyzet a tartalomkezelő rendszerben, néhány további tevékenység végrehajtása után egy kódolási feladatot kell, hogy a fejlesztéséért egészíti ki (például a következő lépés a munkafolyamat, és tartalom közzététele eseményindító). 

###<a name="considerations"></a>Megfontolandó szempontok

Azure tároló várólista használó Media Services-alkalmazások fejlesztésével vegye figyelembe az alábbiakat.

- A sorok szolgáltatás nem nyújt garantálja az első be először ki (FIFO) rendelt kézbesítési. További tudnivalókért lásd: [Azure sorban várakozó és Azure Service Bus sorban várakozó képest és Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
- Azure tároló sorok nem leküldéses szolgáltatás; Ha a várólista lekérdezik. 
- A sorok értéke lehet. További tudnivalókért olvassa el a [Várólista szolgáltatást REST API -val](https://msdn.microsoft.com/library/azure/dd179363.aspx)című témakört.
- Azure tároló sorban várakozó van, bizonyos korlátozások, és a használatát a részletek, a következő cikkben ismertetett: [Azure sorban várakozó és Azure Service Bus sorban várakozó képest és Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Példa

Ebben a szakaszban a példa az alábbi műveleteket végzi el:

1. Megadja a **EncodingJobMessage** osztály, amely az értesítő üzenet formátumának rendeli. A kódot a sorból **EncodingJobMessage** típusú objektumok a Beérkezett üzenetek deserializes.
1. A Media Services, a tárterület-fiók adatait betölti az app.config fájlból. Ezt az információt a **CloudMediaContext** és **CloudQueue** objektumok létrehozásához használja.
1. A kódolás feladattal kapcsolatos értesítések fogadására használt várólista létrehoz.
1. Létrehoz az értesítési végpont, amely a sor van rendelve.
1. Az értesítési végpont csatolja a feladatot, és elküldi a kódolási feladatot. Beállíthatja, hogy a feladat csatolva több értesítés végpontjait.
1. Ebben a példában azt érdeklik csak végleges állapotban a feladat feldolgozás, így azt át a **NotificationJobState.FinalStatesOnly** a **AddNew** módszerrel. 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. A sikeres NotificationJobState.All, ha minden állapot módosítása értesítést kap, hogy várt kell: aszinkron ütemezett -> feldolgozás -> -> befejezett. Jó helyen jár amint korábbi, az Azure tároló várólista szolgáltatást nem garantálja sorszámozott kézbesítési. A (az alábbi példában a EncodingJobMessage adja definiált) időbélyeg tulajdonság sorrendben üzenetek is használhatja. Akkor lehet, hogy ismétlődő értesítési üzenetet kap. A tulajdonsággal ETag (EncodingJobMessage típusú definiált) duplikált elemek keresése. Lehetőség arra is, hogy az egyes állapot módosítása értesítések kimarad. 
1. A feladat kész állam eléréséhez, jelölje be a várólista 10 másodpercenként várakozik. Törli az üzeneteket, azok feldolgozása után.
1. A sor és az értesítési végpont törlése.

>[AZURE.NOTE]Az ajánlott módszereket figyelése a feladat állapota szerint ki van hallgat értesítését, az alábbi példában látható módon.
>
>Azt is megteheti ellenőrizheti a feladat állapotát a **IJob.State** tulajdonság segítségével.  A feladat befejezése kapcsolatos értesítő üzenet előfordulhat, hogy mire eljut az üzenetem a **IJob** állapota **Befejezett**beállítása előtt. A **IJob.State** tulajdonság egy kis idő a pontos állapotát tükrözi.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
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

A fenti példa elő a következő eredményt. Értékek, változhatnak.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
