<properties 
    pageTitle="Fájlok feltöltése a Media Services figyelembe .NET segítségével |} Microsoft Azure" 
    description="Megtudhatja, hogy miként médiatartalom foglalkozni Media Services létrehozásával és eszközök feltöltése." 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Fájlok feltöltése a Media Services figyelembe .NET használatával

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [TÖBBI](media-services-rest-upload-files.md)
 - [Portál](media-services-portal-upload-files.md)

Media Services, az Ön feltöltése (vagy ingest) a digitális eszköz fájlokat. Az **eszköz** entitás is tartalmazhat, videó, hang, képek, miniatűrök gyűjtemények, szöveg számok és kódolt felirat fájlok (és ezek a fájlok metaadatait.)  A fájlok feltöltése befejeződik, amikor a tartalom tárolási biztonságosan további feldolgozásra és a folyamatos átvitelű a felhőben.

Az eszköz a fájlok **Eszköz fájlok**nevezik. A **AssetFile** példány és a tényleges médiafájl két különálló objektummal. AssetFile példány, a multimédiás fájl metaadatokat tartalmaz, miközben a multimédiás fájl tartalmaz a tényleges médiatartalom.

>[AZURE.NOTE]Egy tárgyi eszköz fájl nevének megadása érvényesek a következő szempontokat mérlegelve:
>
>- Media Services használja a IAssetFile.Name tulajdonság értékét adatfolyam (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) tartalma URL-címek készítésekor Emiatt az URL-kódolás nem engedélyezett. A **Name** tulajdonság értéke nem lehet a következő [karakterek készültségi kódolást-fenntartott](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Még csak lehet egy "." esetében a fájlnévhez tartozó kiterjesztést.
>
>- Nevének hossza nem lehet nagyobb, mint 260 karakter.

Eszközök létrehozásakor a következő titkosítási beállításokat is megadhat. 

- **Nincs** - titkosítás nem használatos. Az alapértelmezett értéket. Figyelje meg, hogy ez a beállítás használata esetén a tartalom nem védett a hálózaton átvitt vagy tárolóban lévő többi.
Ha szeretne egy MP4 előadása fokozatos letöltése, használja a használja ezt a beállítást. 
- **CommonEncryption** – ezt a lehetőséget, ha Ön már titkosított és látták el közös titkosítást vagy PlayReady DRM (például zökkenőmentes Streaming védett a PlayReady DRM) tartalma feltöltése.
- **EnvelopeEncrypted** – ezt a lehetőséget, ha vannak feltöltése a titkosított AES HLS. Ne feledje, hogy a fájlok kell van már kódolt átalakítása Manager titkosítja.
- **StorageEncrypted** – a világos tartalomhoz helyileg bites AES 256 titkosítást és feltöltések azt Azure tárolóhoz tárolási helyére a titkosított a többi titkosítja. Tárterület-titkosítással védett eszközök automatikusan titkosítatlan és egy titkosított fájlrendszer előtt kódolást helyezett, és tetszés szerint újra titkosított egy új kimeneti eszközként feltöltése előtt. Az elsődleges használatieset az tárterület-titkosítás esetén, a kiváló minőségű beviteli médiafájlokat a többi titkosítású a lemezen védeni kívánt.

    Media Services lemezen tároló titkosítást, az eszközök, nem túl a – vezetékes például a digitális jogkezelési Manager (DRM) biztosít.

    Ha az eszköz titkosított tárhely, meg kell adnia az eszköz kézbesítési házirend. További információt a [konfigurálása eszköz kézbesítési házirend](media-services-dotnet-configure-asset-delivery-policy.md)témakörben található.

Adja meg az eszköz titkosítva, amelyet egy **CommonEncrypted** , vagy egy **EnvelopeEncypted** lehetőséget, ha szüksége lesz az eszköz társítása egy **ContentKey**. További tudnivalókért lásd: [hogyan hozhat létre egy ContentKey](media-services-dotnet-create-contentkey.md). 

Adja meg az eszköz titkosítható egy **StorageEncrypted** beállítást választja, a .NET rendszerhez, a Media Services SDK az eszköz **StorateEncrypted** **ContentKey** hoz létre.


Ez a témakör bemutatja, hogyan Media Services .NET SDK, valamint a Media Services .NET SDK bővítmények használata a fájlok feltöltése a Media Services eszköz be.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>A Media Services .NET SDK egy fájl feltöltése 

Az alábbi példakódot .NET SDK használatával végezze el az alábbi műveleteket: 

- Létrehoz egy üres eszköz.
- Létrehoz egy AssetFile példány azt az eszköz társítani kívánt.
- Létrehoz egy AccessPolicy példány, amely definiálja az engedélyek és hozzáférés időtartam az eszközre.
- Létrehoz egy megnevezés-példányt, amely az eszköz hozzáférést biztosít.
- Egyetlen médiafájl feltölti a Media Services be. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>A Media Services .NET SDK több fájl feltöltése 

A következő kódot megtudhatja, hogy miként hozzon létre egy eszközt, és töltse fel a fájlokat.

A kódot az alábbi műveleteket végzi el:
    
-   Létrehoz egy üres eszköz az előző lépésben megadott CreateEmptyAsset módszerrel.
    
-   Létrehoz egy **AccessPolicy** példány, amely definiálja az engedélyek és hozzáférés időtartam az eszközre.
    
-   Létrehoz egy **Megnevezés** -példányt, amely az eszköz hozzáférést biztosít.
    
-   Létrehoz egy **BlobTransferClient** példány. Típus jelöl, amely az Azure BLOB ügyfél. Ebben a példában a az ügyfél feltöltés nyomon, hogy használjuk. 
    
-   A megadott könyvtár fájlok közötti felsorolja, és létrehoz egy **AssetFile** példány fájlonként.
    
-   A fájlok feltölti a **UploadAsync** módszerrel Media Services be. 
    
>[AZURE.NOTE] Győződjön meg arról, hogy a hívások nem blokkolja a program, és a fájlok feltöltése párhuzamosan befejeződik a UploadAsync módszert.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Nagyszámú eszközök feltöltésekor vegye figyelembe az alábbiakat.

- Hozzon létre egy új **CloudMediaContext** objektum szálon. A **CloudMediaContext** osztály nem biztonságos szál.
 
- Az alapértelmezett érték, a 2 például 5 nagyobb értéket NumberOfConcurrentTransfers növelése Ez a tulajdonság beállításával **CloudMediaContext**minden előfordulását érinti. 
 
- Az alapértelmezett érték 10-es ParallelTransferThreadCount őrizni.
 
##<a id="ingest_in_bulk"></a>Segítségével a Media Services .NET SDK ingesting eszközök 

Nagy eszköz fájlfeltöltéskor lehet egy szűk eszköz létrehozása során. A tömeges vagy a "Tömeges Ingesting" eszközök ingesting, magában foglalja a szétválasztás eszköz létrehozása a feltöltés folyamat. A tömeges megközelítés ingesting használatához, amely leírja a digitális eszköz kiválasztása és a hozzájuk kapcsolódó fájlokat jegyzék (IngestManifest) létrehozása A feltöltési módszer lehetőség használatával a hozzájuk kapcsolódó fájlokat feltölteni a jegyzék blob-tárolóhoz. Microsoft Azure Media Services se nézze, amikor a blob-tárolóhoz a jegyzék társított. Miután feltöltött egy fájlt a blob-tárolóhoz, a Microsoft Azure Media Services befejezte konfigurációja miatt az eszköz a jegyzék (IngestManifestAsset) az eszköz létrehozását.


Hozzon létre egy új IngestManifest hívja a létrehozás módszer a IngestManifests gyűjteményben kattintson a CloudMediaContext által elérhetővé tett. Ezzel a módszerrel hoz létre egy új IngestManifest a nyilvánvalóan nevet ad.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Az eszközök, amelyek a tömeges IngestManifest társítjuk létrehozása. Az eszköz tömeges ingesting a kívánt titkosítási beállítását.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

A tömeges ingesting tömeges IngestManifest tárgyi eszköz egy IngestManifestAsset társít. Azt is az egyes eszközök alkotó AssetFiles társít. Hozzon létre egy IngestManifestAsset, használja a Create metódus az kiszolgálói környezetben.

A következő példa bemutatja, hogyan hozzáadása két új IngestManifestAssets, amely a korábban létrehozott tömeges két eszközök társítása ingest jegyzék. Minden egyes IngestManifestAsset minden eszköz feltöltődnek fájlokat is társít tömeges ingesting során.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Bármelyik alkalmas a digitáliseszköz-fájlok feltöltése a blob-tároló tároló a IngestManifest **IIngestManifest.BlobStorageUriForUpload** tulajdonsága által biztosított URI nagy sebességű ügyfélalkalmazás is használhatja. Egy nagy sebességű főbb feltöltés szolgáltatása [Aspera igény szerinti Azure alkalmazáshoz](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). A kódírás való fájlfeltöltéskor az eszközök kódot a következő példában látható módon.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

A minta, ez a témakör a használt eszköz fájlfeltöltéskor kódját a következő példa látható.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Meghatározhatja, hogy tömeges ingesting által a **IngestManifest**statisztika tulajdonsága lekérdezési egy **IngestManifest** társított összes eszközök előrehaladását. Végrehajtási adatok frissítése, hogy egy új **CloudMediaContext** minden alkalommal, amikor Ön lekérdezik az statisztika tulajdonság kell használnia.

Az alábbi példa bemutatja a **azonosító**szerint egy IngestManifest lekérdezési.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>.NET SDK bővítmények használata a fájlok feltöltése 

Az alábbi példa bemutatja, hogyan .NET SDK Extensions használata egy fájl feltöltése. Ebben az esetben a **CreateFromFile** módszert, de az aszinkron verziója is elérhető (**CreateFromFileAsync**). A **CreateFromFile** módszer adja meg a fájl nevét, a titkosítási beállítás, és a visszahívás, annak érdekében, hogy a fájl feltöltése állapotának jelentése az teszi lehetővé.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Az alábbi példa meghívja UploadFile függvényt, és adja meg a tárterület-titkosítás mint a eszköz létrehozása lehetőséget.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Következő lépés

Most, hogy az eszköz Media Services feltöltött saját, nyissa meg a [beszerzése a médiafájlok processzor][] témakörre.

[A médiafájlok processzor beszerzése]: media-services-get-media-processor.md
 
