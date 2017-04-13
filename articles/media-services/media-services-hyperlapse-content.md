<properties
    pageTitle="Médiafájlok Hyperlapse az Azure Media Hyperlapse |} Microsoft Azure"
    description="Azure Media Hyperlapse zökkenőmentes idő elévült videók első-személy vagy művelet-kamera tartalmat hoz létre. Ez a témakör bemutatja, hogyan Media indexelő használja."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Médiafájlok Hyperlapse az Azure Media Hyperlapse

Azure Media Hyperlapse egy Media processzor (MP) által létrehozott zökkenőmentes idő elévült videók első-személy vagy művelet fényképezőgép-tartalom.  A felhőalapú azonos szintű [Microsoft Research asztali Hyperlapse Pro](http://aka.ms/hyperlapse)és a telefon-alapú Hyperlapse Mobile, Microsoft Hyperlapse az Azure Media Services használja az Azure Media Services Media feldolgozása platform vízszintesen méretezése és parallelize tömeges beosztásának tömeges Hyperlapse feldolgozása.

>[AZURE.IMPORTANT]A Microsoft Hyperlapse való együttműködésre terveztük legjobb első személyes tartalom a mozgó kamerával.  Habár továbbra is a fényképezőgép képanyag továbbra is dolgozhat, a teljesítmény és a minőség az Azure Media Hyperlapse Media processzor nem lehet garantálni más típusú tartalmat.  További információt a Microsoft Hyperlapse az Azure Media Services-és néhány példa videók megtekintéséhez kivétele a [bevezető blog közzé](http://aka.ms/azurehyperlapseblog) a nyilvános előzetes verzióban.

Az Azure Media Hyperlapse feladat vesz igénybe, mint a bemeneti egy MP4, MOV vagy WMV eszköz fájlban, amely meghatározza, hogy mely keretek videó kell idő elévült konfigurációs fájl együtt, és milyen sebességét (például az első 10 000 kereteket 2 x).  A kimenet stabil, és időt elévült képmegjelenítés, a videó bevitt.

Az Azure Media Hyperlapse naprakész a [Media Services blogok](https://azure.microsoft.com/blog/topics/media-services/)című témakör tartalmaz.

## <a name="hyperlapse-an-asset"></a>Egy tárgyi eszköz Hyperlapse

Először meg kell kívánt beviteli fájljait feltöltheti az Azure Media Services.  Ha többet szeretne megtudni a fogalmak felügyeli tölt fel és a tartalmak karbantartását, olvassa el a [tartalmak kezeléséhez cikk](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Konfigurációs készletet az Hyperlapse

Miután a tartalom a Media Services-fiókjában, szüksége lesz a konfigurációs készlet létrehozása.  Az alábbi táblázat ismerteti a felhasználó által megadott mezők:

 A mező | Leírás
-------|-------------
StartFrame|A keret, amelyen a Microsoft Hyperlapse feldolgozás kell kezdődnie.
NumFrames|A keretek feldolgozása száma
Sebessége|A varianciaanalízis, amellyel a bemeneti videó gyorsítása.

Az alábbi képen az XML és az JSON alkalmazást tesz konfigurációs fájl:

**XML-készletet:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON beállított:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>A AMS .NET SDK a Microsoft Hyperlapse

A következő módszerrel eszközként médiafájl feltölti, és létrehoz egy feladatot az Azure Media Hyperlapse Media processzor.

> [AZURE.NOTE] Már rendelkeznie kell egy CloudMediaContext hatóköre az neve "környezetben" Ez a kód, a munkát, a.  Kapcsolatos további információért olvassa el a [tartalmak kezeléséhez a cikk](media-services-dotnet-get-started.md).

> [AZURE.NOTE] A karakterlánc-argumentum "hyperConfig" várhatóan a fentebb ismertetett JSON vagy a XML-készletet alkalmazást tesz konfiguráció.

statikus logikai RunHyperlapseJob (karakterlánc beviteli, karakterlánc kimeneti, karakterlánc hyperConfig) {/ / hozzon létre eszközt a bemeneti fájl IAsset eszköz = környezetben. Eszközök. CreateAssetAndUploadSingleFile (bemenet "Az Hyperlapse bemenet" AssetCreationOptions.None);

ragadja meg az Azure Media Hyperlapse MP IMediaProcessor mp előfordulását = környezetben. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Feladat létrehozása Hyperlapse tevékenység IJob feldolgozás = környezetben. Feladatok. Hozzon létre (String.Format (bemenet "Hyperlapse: {0}"));

Ha (String.IsNullOrEmpty(hyperConfig)) {/ / config nem lehet üres visszatérési hamis;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = feladatot. Tasks.AddNew ("Hyperlapse tevékenység", mp, hyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse kimenet", AssetCreationOptions.None);


feladat. Submit();

Folyamatban nyomtatás és a feladatok tevékenység progressPrintTask lekérdezés létrehozása új Task(() = = > {

IJob jobQuery = null; végezze el {var progressContext = helyi; jobQuery = progressContext.Jobs. Hol (j = > j.Id == feladatot. Azonosító). First(); Console.WriteLine("a(z) (karakterlánc. Formátum ("{0} {1} \t \t 2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. [[[Folyamatjelző)); Thread.Sleep(10000); } közben (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Támogatott fájltípusok

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Kapcsolódó hivatkozások

[Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)

[Azure Media Analytics-bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
