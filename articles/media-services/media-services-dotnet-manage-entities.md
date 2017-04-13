
<properties 
    pageTitle="Eszközök és a Media Services .NET SDK kapcsolódó személyek kezelése" 
    description="Megtudhatja, hogy miként kezelheti a .NET eszközök és a Media Services SDK a kapcsolódó szervezetek." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Eszközök és a Media Services .NET SDK kapcsolódó személyek kezelése


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [TÖBBI](media-services-rest-manage-entities.md)


Ez a témakör bemutatja, hogyan kell a következő Media Services felügyeleti feladatok:

- Egy tárgyi eszköz hivatkozás beszerzése 
- A feladat hivatkozások beszerzése 
- A lista összes eszközök 
- Feladatok lista és eszközök 
- A lista összes access-házirendek 
- A lista összes Locator
- Nagy gyűjtemények szervezetek számbavétele
- Eszköz törlése 
- Feladat törlése 
- Az access-házirend törlése 

##<a name="prerequisites"></a>Előfeltételek 

Lásd [a környezet beállítása](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Egy tárgyi eszköz hivatkozás beszerzése

Gyakori tevékenység: hivatkozás egy meglévő eszköz kihozni a Media Services. A következő példa bemutatja, hogyan érheti el egy digitáliseszköz-hivatkozást az eszközök gyűjteményből kiszolgálói környezetben objektumra, tárgyi eszköz azonosító alapján
A következő példa Linq lekérdezést használó megszerezni a meglévő IAsset objektumhoz hivatkozást.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>A feladat hivatkozások beszerzése

Feladatok a Media Services kód feldolgozása-használatakor gyakran kell beszereznie egy meglévő projektet azonosítóval alapján mutató hivatkozás A következő példa bemutatja, hogyan IJob objektum hivatkozás a feladatok gyűjteményből.
WarningWarning szükség lehet a projekt összefoglaló egy hosszabb ideig futó kódolási feladat indításakor, és a feladat állapota szálon ellenőrizni szeretné. Ilyen esetben, ha a módot ad vissza egy szál kell beolvasni a feladat frissített hivatkozást.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>A lista összes eszközök

A már a tárhely eszközök számát növekedésével célszerű lista eszközeit. A következő példa bemutatja, hogyan Végigléptetés az eszközök gyűjtemény a kiszolgáló helyi objektumon. Az egyes eszköz, a kód példa is ír néhány az tulajdonság értékeit a konzolba. Egyes eszközök például sok médiafájlok is tartalmazhat. A példa ír ki minden eszköz társított összes fájlt.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Feladatok lista és eszközök

Fontos kapcsolódó feladatot a Media Services társított feladat lista eszközöket is. A következő példa bemutatja, hogyan a lista egyes IJob objektumra, és kattintson az egyes feladatokhoz, megjeleníti a feladattal kapcsolatos tulajdonságai, az összes kapcsolódó feladatok, az összes bemeneti eszközök és az összes kimenő eszközök. Ebben a példában a kód számos más tevékenységek hasznos lehet. Például lista egy vagy több kódolási feladatok, amely a korábban futtatta a kimeneti eszközök el szeretné, ha ez a kód szemlélteti a kimeneti eszközök eléréséhez. Ha kimeneti tárgyi eszköz egy hivatkozás, majd juttathat el a tartalom a felhasználóknak és más alkalmazások letölti és kezeléséről az URL-ek. 

További információt a kézbesítési eszközök olvassa el a [Eszközöket használhat az előadáshoz a Media Services SDK a .NET rendszerhez](media-services-deliver-streaming-content.md)című témakört.

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>A lista összes Access-házirendek

A Media Services az access-házirend meg tárgyi eszköz vagy annak fájljait. Az access-házirend határozza meg, hogy a fájl- vagy tárgyi eszköz (milyen típusú hozzáférés, és az időtartam) engedélyeit. A Media Services kódban általában definiálása az access-házirend: IAccessPolicy objektum, valamint társítása egy meglévő eszköz. Ezután hozzon létre egy ILocator objektumra, amely lehetővé teszi a Media Services eszközökhöz közvetlen hozzáférést biztosít. A Visual Studio projekt dokumentáció sorozat olvashatja el, amelyek bemutatják, hogyan hozhat létre, és a hozzáférési és Locator hozzárendelése eszközök kód példák tartalmazza.

Az alábbi példa szemlélteti, hogyan lehet a lista összes hozzáférési házirend a kiszolgálón, és minden társítandó jogosultságok típusú jeleníti meg. Hozzáférési megtekintéséhez hasznos úgy, hogy a lista összes ILocator objektumok a kiszolgálón, és kattintson az egyes megnevezés, jeleníthet meg a társított hozzáférési házirend AccessPolicy tulajdonsága használatával.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>A lista összes Locator

Egy megnevezés egy tárgyi eszköz, az eszköz engedélyekkel együtt eléréséhez a megnevezés társított hozzáférési házirend által meghatározott közvetlen elérési útja URL-CÍMÉT. Egyes eszközök beállíthatja, hogy a Locator tulajdonsága társított ILocator objektumok gyűjteménye. A kiszolgálói környezetben, amely tartalmazza az összes Locator Locator gyűjtemény tartalmaz.

Az alábbi példa a kiszolgálón összes Locator sorolja fel. Az egyes megnevezés akkor a kapcsolódó eszközök és a hozzáférési házirend azonosítóját tartalmazza. Az eszközre az engedélyek, a lejárat dátuma és a teljes elérési útját típusú is megjeleníti.

Ne feledje, hogy egy tárgyi eszköz megnevezés elérési csak egy alap URL-CÍMÉT az eszközt. Közvetlen elérési útját, hogy egy felhasználó vagy alkalmazás sikerült tallózással keresse meg külön fájlokat létrehozni, a kód fel kell vennie az adott fájl elérési útja az megnevezés elérési útját. További információt a művelet című témakört [a .NET rendszerhez, a Media Services SDK eszközöket használhat az előadáshoz](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Nagy gyűjtemények szervezetek számbavétele

Lekérdezés szervezetek, ha van legfeljebb 1000 szervezetek egyszerre vissza, mert a nyilvános többi v2 korlátozza a lekérdezés eredményében 1000 eredményekre. Akkor használja a Kihagyás és meg kell tennie, ha személyek nagy gyűjtemények számbavétele. 
    
Az alábbi függvény a megadott Media Services-fiók az összes feladatot keresztül hurkok. Media Services 1000 feladatok feladatok webhelycsoport adja eredményül. A függvényt alkalmazza a Kihagyás gombra, és győződjön meg arról, hogy az összes feladatok átvitel vannak számbavétele, (abban az esetben a fiókban 1000-nél több feladat van).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Eszköz törlése

A következő példa tárgyi eszköz törli.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Feladat törlése

Ha törölni szeretne egy feladatot, ellenőriznie kell a feladat állapota a State tulajdonság feltüntetett. Befejezett vagy megszakad a feladatok törölheti, mialatt a kell először megszakad, amelyek az egyes állapotok például aszinkron, ütemezett vagy feldolgozása, feladatok, majd törölje őket.
Az alábbi példa mutatja egy módszert a feladat törlésével feladat államok ellenőrzése, és majd törlése, ha az állapot befejeződött vagy megszakad. Ez a kód függ, hogy az előző szakaszban, a jelen témakör ismertető hivatkozni szeretne egy feladatot: feladat hivatkozás.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Az Access-házirend törlése

Az alábbi példa szemlélteti az access-házirend a házirend-azonosító alapján való hivatkozás majd törölje a házirendet.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
