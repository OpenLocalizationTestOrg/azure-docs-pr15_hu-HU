<properties 
    pageTitle="Tárgyi eszköz kódolását Media Encoder standard .NET segítségével |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan tárgyi eszköz használatával Media Encoder Strandard kódolását .NET segítségével." 
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
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Egy tárgyi eszköz kódolását Media Encoder standard .NET használatával

Kódolási feladatok a Media Services a leggyakoribb feldolgozási műveletek közül. Médiafájlok konvertálása között egy kódolást kódolási feladatok létrehozása Amikor a kódolását, a Media Services beépített Media Encoder is használhatja. Is használhatja a Media Services partner; által biztosított kódoló harmadik fél kódolók érhetők el a Microsoft Azure piactéren. 

Ez a témakör bemutatja, hogyan .NET segítségével kódolása a Media Encoder szabványos (MES) eszközeit. Media Encoder szabványos van beállítva az egyik a kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Ajánlott mindig kódolását Emeletes fájlok be adaptív átviteli sebesség MP4 beállítása, és alakítsa át a beállítás használata a [Dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md)kívánt formátumra. Kihasználhatja dinamikus csomagolása, először a továbbított végpontot, amelyből tervezi kézbesítési a tartalom előbb be kell szereznie legalább egy igény szerinti adatfolyam egység. További információért tájékozódhat [skála Media Services](media-services-portal-manage-streaming-endpoints.md).

Ha a kimeneti eszköz titkosított tárhely, meg kell adnia eszköz kézbesítési házirend. További információt a [konfigurálása eszköz kézbesítési házirend](media-services-dotnet-configure-asset-delivery-policy.md)témakörben található.

>[AZURE.NOTE]MES hoz létre, a bemeneti fájl nevét az első 32 karaktereket tartalmaz, amelyek neve a kimeneti fájl. A nevet a az előre beállított fájlban megadottak alapján. Ha például a "fájlnév": "{Basename} _ {Index} {bővítmény}". {Basename} a bemeneti fájl nevét az első 32 karakterét váltja fel.

###<a name="mes-formats"></a>MES formátumok

[Formátumok és a kodekekről](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>MES készletek

Media Encoder szabványos van beállítva az egyik a kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Bemeneti és kimeneti metaadatok

Ha Ön kódolását MES használatával beviteli tárgyi eszköz (vagy eszközök), kimeneti tárgyi eszköz kattint, a sikeres befejezésétől, amely kódolását tevékenység. A kimenet eszköz tartalmazza a videó, a hang, a miniatűrök, jegyzék, használja a kódolási készlet alapján stb.

A kimenet eszköz metaadatokkal kapcsolatos a bemeneti eszköz fájlt is tartalmaz. A metaadat-alapú XML-fájl nevét a következő formátumban van: < asset_id > _metadata.xml (például 41114ad3-eb5e - 4-es c 57-8d 92-5354e2b7d4a4_metadata.xml), a bemeneti eszköz eszközkód értékének < asset_id > esetén. A beviteli metaadatok XML sémájának leírása [Itt](http://msdn.microsoft.com/library/azure/dn783120.aspx).

A kimenet eszköz metaadatokkal kapcsolatos a kimeneti eszköz fájlt is tartalmaz. A metaadat-alapú XML-fájl nevét a következő formátumban van: < source_file_name > _manifest.xml (például BigBuckBunny_manifest.xml). A kimenet metaadatok XML leírása sémájának [Itt](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Ha meg szeretné vizsgálni vagy a két metaadat-fájlokat, létrehozhat egy Társítások megnevezés, és töltse le a fájlt a helyi számítógépre. Hozzon létre egy Társítások megnevezés, és töltse le a fájlt a Media Services .NET SDK-bővítmények talál példát.

##<a name="download-sample"></a>Töltse le a minta

Szerezze be és lebonyolítása minta [Itt](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Példa

Az alábbi példa a Media Services .NET SDK használatával végezze el az alábbi műveleteket:

- Hozzon létre egy kódolási feladatot.
- Ismerkedés a Media Encoder szabványos kódoló mutató hivatkozás.
- Adja meg a használni a "H264 több átviteli sebesség 720p" előre definiált. A típus csoportban megjelenik [az alábbi](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Is vizsgálja meg a séma, amelyhez a készleteket meg kell felelniük [Itt](https://msdn.microsoft.com/library/mt269962.aspx) témakört.
- Egyetlen kódolási tevékenység hozzáadása a feladatot. 
- Adja meg a kívánt beviteli eszköz.
- Hozzon létre egy kimeneti eszköz, amely tartalmazni fogja a kódolt eszköz.
- Adja hozzá a feladat állapotának ellenőrzése a eseménykezelő.
- A feladat küldése
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
                TaskOptions.None);
        
            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);
        
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
            return job.OutputMediaAssets[0];
        }
        
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                    break;
                default:
                    break;
            }
        }
        
        
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
            return processor;
        }


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még: 

[.NET Media Encoder szabványos használata miniatűr létrehozásának módját](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services kódolást – áttekintés](media-services-encode-asset.md)
