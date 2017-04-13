<properties
    pageTitle="Az indexelési médiafájlok az Azure Media indexelő"
    description="Azure Media indexelő teszi lehetővé, hogy a médiafájlokat a tartalom kereshető és a teljes szöveges átirat kódolt feliratok és kulcsszavakra létrehozásához. Ez a témakör bemutatja, hogyan Media indexelő használja."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Az indexelési médiafájlok az Azure Media indexelő


Azure Media indexelő teszi lehetővé, hogy a médiafájlokat a tartalom kereshető és a teljes szöveges átirat kódolt feliratok és kulcsszavakra létrehozásához. Egy médiafájl vagy egy köteg több médiafájlok is dolgozza fel.  

>[AZURE.IMPORTANT] Az indexelés tartalmat, ügyeljen az használni a médiafájlokat, amelyek rendkívül egyszerű Beszédfelismerés (háttér zenét, a zajt, az effektusok és a mikrofon hiss) nélkül. Néhány példa a megfelelő tartalom: a rögzített értekezletek, a előadások vagy a bemutatókat. A következő tartalom nem lehet készítéshez alkalmas: filmek, tévéműsorok, bármilyen fájlok vegyes hang- és hangokat, a rosszul rögzíti a háttérzajt (hiss) tartalma.


Egy indexelési feladatot hozhat létre a következő kimeneti értékeket:

- Zárt felirat fájlokat a következő formátumokban: **SZÁMI**, **TTML**és **WebVTT**.

    Kódolt felirat fájloknak Recognizability, mely értékek egy indexelési feladat alapján hogyan felismerhető a beszédet a forrás videóban nevű címke.  Kimenet képernyőn fájlok használhatósági Recognizability értékének is használhatja. Egy alacsony pontszámhoz miatt hangminőség gyenge indexelési eredmények volna jelenti.
- Kulcsszó-fájl (XML).
- A hang indexelés blob-fájl (AIB) a SQL Serverrel való használatra.

    További tudnivalókért lásd: [Az Azure Media indexelő és az SQL Server használatával AIB fájlokat](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


Ez a témakör bemutatja, hogyan hozhat létre **egy eszköz Index** és a **Tárgymutató-fájlokat**az indexelő feladatokat.

Az Azure Media indexelő naprakész a [Media Services blogok](#preset)című témakör tartalmaz.

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Feladatok az indexelés konfigurálása és jegyzék fájlok használata

Az indexelési feladatokhoz további részleteket a tevékenység konfiguráció használatával is megadhat. Ha például megadhatja, melyik metaadatokat használni a multimédiás fájl. A metaadatok használja, a nyelv motor bontsa ki a szószedet, és nagymértékben javítja a beszédfelismerés pontosságát.  Azt is megadhatja a kívánt kimeneti fájlok.

Több médiafájlok egyszerre is feldolgozni nyilvánvalóan-fájl használatával.

További tudnivalókért olvassa el a [Tevékenység készletet az Azure Media indexelő](https://msdn.microsoft.com/library/dn783454.aspx)című témakört.

## <a name="index-an-asset"></a>Tárgymutató-eszköz

A következő módszerrel eszközként médiafájl feltölti, és létrehoz egy feladatot indexelni az eszköz.

Figyelje meg, hogy nincs konfigurációs fájl meg van adva, ha a médiafájl fog indexelni az összes alapértelmezett beállításokkal.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a id="output_files"></a>Kimeneti fájlok

Alapértelmezés szerint egy indexelési feladatot a következő kimeneti fájlokat hoz létre. Az első kimeneti eszköz a is menti a fájlokat.

Ha egynél több beviteli médiafájl, indexelő készítése a nyilvánvalóan kimeneti értékeket feladat "JobResult.txt" nevű fájlt. Az egyes bemeneti médiafájl, az eredményül kapott AIB SZÁMI, TTML, WebVTT fájlok, és kulcsszó, egymás után számozott és nevű "Alias" használatával.

Fájlnév | Leírás
----------|------------
__InputFileName.aib__ | Az indexelési blob hangfájlt. <br /><br /> Hang indexelés Blob (AIB) fájl lehet keresni a teljes szöveget a keresés használatáról a Microsoft SQL server bináris fájl.  AIB fájl hatékonyabb, mint a egyszerű felirat fájlok, mert az egyes word lehetővé teszi a jóval magasabb szintű keresőmoduljának alternatívák tartalmaz. <br/> <br/>A telepítése a indexelő SQL-bővítmény a számítógépen futó Microsoft SQL server 2008 vagy újabb szükséges hozzá. Keresés a AIB használata a Microsoft SQL server teljes szöveges keresési pontosabb találatok keresés WAMI által létrehozott kódolt felirat fájlokhoz, mint biztosít. Ennek oka az, a AIB tartalmaz word alternatívák mely hasonló hang, mivel a kódolt felirat fájlok tartalmaznak a legmagasabb megbízhatóság szó minden szakasz a hangot. Ha a elhangzott szavak keresése upmost jelentőségű, majd a Microsoft SQL Server AIB a együttes használata ajánlott.<br/><br/> A bővítmény letöltéséhez kattintson az <a href="http://aka.ms/indexersql">Azure Media indexelő SQL bővítményre</a>. <br/><br/>Más keresőmotorok, például Apache Lucene/Solr egyszerűen indexelni a videó kódolt felirat és kulcsszó XML-fájlok alapján a csatlakozást a lehetőség arra is, de ennek eredményeképpen a kevésbé pontos találatok.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |SZÁMI, TTML és WebVTT formátumok kódolt felirat (CC) fájlokat.<br/><br/>Hogy a hang-és videofájlok hallja a fogyatékkal élők használható.<br/><br/>Kódolt felirat fájlok <b>Recognizability</b> , mely értékek hogyan felismerhető a beszédet a forrásban videó alapján egy indexelési feladat van nevű címke tartalmazza.  Kimenet képernyőn fájlok használhatósági <b>Recognizability</b> értékének is használhatja. Egy alacsony pontszámhoz miatt hangminőség gyenge indexelési eredmények volna jelenti.
__InputFileName.kw.xml<br />InputFileName.info__ |Kulcsszó és információ fájlokat. <br/><br/>A fájl kulcsszó tartalmazza e kulcsszavakat a beszéd tartalom kiolvasott gyakoriság és eltolás adatokat tartalmazó XML-fájlt. <br/><br/>Információ egy egyszerű szöveges formátumú fájl minden felismerte kifejezés részletes információt tartalmazó fájl. Az első sor speciális és a Recognizability pontszám tartalmazza. Minden egyes további sor tabulátorral tagolt listáját a következő adatokat: idő, a befejezési időpontot, a szót vagy kifejezést, a megbízhatóság indítása. A időtartamok másodpercben, és a megbízhatóság számként megadott 0 – 1. <br/><br/>Példa sor: "1,20 1.45 word 0.67" <br/><br/>Ezeket a fájlokat lehet használt számú céljából, például: beszédfelismerés elemzések, vagy csak akkor érhető el keresőprogramok, például Bing, Google vagy a Microsoft SharePoint, hogy a médiafájlokat, további felfedezhetőségét, vagy akár több megfelelő hirdetések előadásához használt.
__JobResult.txt__ |Jelenik meg, csak akkor, ha több fájlt, az alábbi adatokat tartalmazó indexelés kimeneti jegyzék:<br/><br/><table border="1"><tr><th>Bemeneti_fájl</th><th>Alias</th><th>MediaLength</th><th>Hibaüzenet</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Ha beviteli médiafájlok sikeresen indexelve vannak, az indexelési feladat 4000 hibakóddal meghiúsul. További tudnivalókért olvassa el a [hibakódok](#error_codes)című témakört.

## <a name="index-multiple-files"></a>Több fájl indexelése

A következő módszerrel feltöltések megjelenítése több médiafájlok eszközként, és létrehoz egy feladatot egy köteg e fájlok indexelendő.

Egy nyilvánvalóan .lst kiterjesztésű fájl létrehozott és feltöltése be az eszközt. A nyilvánvalóan fájl tartalmaz a digitáliseszköz-fájlok listája. További tudnivalókért olvassa el a [Tevékenység készletet az Azure Media indexelő](https://msdn.microsoft.com/library/dn783454.aspx)című témakört.

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Részben sikeres feladat

Ha beviteli médiafájlok sikeresen indexelve vannak, az indexelési feladat 4000 hibakóddal meghiúsul. További tudnivalókért olvassa el a [hibakódok](#error_codes)című témakört.


Az azonos kimeneti értékeket (sikeres feladatként) jönnek létre. Megtudhatja, hogy milyen bemeneti fájlok sikertelen, akkor a hiba oszlopértékek megfelelően a kimeneti nyilvánvalóan fájl is hivatkozhat. A beviteli fájlok, amelyeket nem sikerült, az eredményül kapott AIB, SZÁMI, TTML, WebVTT és kulcsszó fájlok nem jön létre.

### <a id="preset"></a>Tevékenység készletet az Azure Media indexelő

Az Azure Media indexelő feldolgozása testre szabható azáltal, hogy a feladat mellett előre definiált választható feladatról.  A következő formátumban kell a konfigurációs xml ismerteti.

név | Megkövetelése | Leírás
----|----|---
__Adatbevitel__ | hamis | Tárgymutató-kívánt eszköz fájlokat.</p><p>Azure Media indexelő támogatja a következő médiafájl-formátumok: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>A fájl nevét (s) megadhatja a **nevét** vagy a **lista** attribútum a **bemeneti** elem (mint alább). Ha nem adja meg az indexelendő eszköz fájlt, az elsődleges fájl kivétele történik. Ha nem az elsődleges eszköz fájl van beállítva, az első fájlra a bemeneti eszköz indexelt.</p><p>A digitáliseszköz-fájl neve kifejezetten megadásához végezze el:<br />`<input name="TestFile.wmv">`<br /><br />Akkor is elvégezheti az indexelést több eszköz fájlok egyszerre (legfeljebb 10). Ennek módja:<br /><br /><ol class="ordered"><li><p>Hozzon létre egy szövegfájlt (nyilvánvalóan fájl), és tegyen .lst bővítménye. </p></li><li><p>A beviteli eszköz összes digitáliseszköz-nevek listája hozzáadása a nyilvánvalóan fájlt. </p></li><li><p>Adja hozzá az eszköz thanifest fájl (feltöltése).  </p></li><li><p>A beviteli lista attribútum adja meg a nyilvánvalóan fájl nevét.<br />`<input list="input.lst">`</li></ol><br /><br />Megjegyzés: Ha 10-nél több fájlok hozzáadása a nyilvánvalóan fájlt, az indexelési feladat sikertelen lesz a 2006 hibakóddal.
__metaadatok__ | hamis | A megadott eszköz fájlokat szószedet kiigazítás használt metaadatait.  Hasznos lehet ahhoz, hogy nem szabványos szószedet szavakat, mint például a megfelelő nouns felismerje indexelő előkészítése.<br />`<metadata key="..." value="..."/>` <br /><br />__Értékek__ előre definiált __kulcsok__adhat meg. Jelenleg a következő kulcsok támogatottak:<br /><br />"cím" és "leírása" – szószedet kiigazítás használt nyelvet működését a feladat modell, és javíthatja a beszédfelismerés pontosságát.  Az értékek rendező internetes keresések kétszámjegyes megfelelő szöveget dokumentumokat, a belső szótár révén az indexelő tevékenység időtartamát a tartalmát.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__szolgáltatások__ <br /><br /> Az 1.2-es verzió hozzá. Az egyetlen támogatott funkció jelenleg beszédfelismerés automatikus: ("rendszer-Helyreállítás").| hamis | A beszédfelismerés funkció az alábbi beállítások kulcsok foglalja magában:<table><tr><th><p>Kulcs</p></th>     <th><p>Leírás</p></th><th><p>Példa érték</p></th></tr><tr><td><p>Nyelvi</p></td><td><p>A természetes nyelvű ismeri fel a multimédiás fájl.</p></td><td><p>Angol, spanyol</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>a kívánt kimeneti feliratos formátumok (ha van ilyen) pontosvesszővel elválasztott listája</p></td><td><p>ttml; Számi; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Egy logikai jelzővel megadva vagy nem AIB fájlként állapota szükséges (SQL Server-és az ügyfél indexelő IFilter).  További tudnivalókért lásd: <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Az Azure Media indexelő és az SQL Server használatával AIB fájlokat</a>.</p></td><td><p>Igaz; Hamis</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Egy logikai jelzővel megadása vagy nincs szükség-e egy kulcsszót XML-fájlba.</p></td><td><p>Igaz; Hamis. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Egy logikai jelzővel megadása kötelező teljes feliratok (függetlenül attól, konfidenciaszint) kell-e.  </p><p>Alapértelmezés szerint hamis, ebben az esetben szavak és kifejezések, amelyek egy kisebb, mint 50 %-os konfidenciaszint kimarad a végleges felirat kimeneti értékeket és lecseréli az alkalmazáshoz tartozó három pont ("…").  A három hasznosak felirat minőség-ellenőrzéséhez és naplózás.</p></td><td><p>Igaz; Hamis. </p></td></tr></table>

### <a id="error_codes"></a>Hibakódok esetén

Azure Media indexelő jelentse a hibát, ha ismét az alábbi hibakódok egyike:

Kód | név | Lehetséges okai
-----|------|------------------
2000 | Érvénytelen konfiguráció | Érvénytelen konfiguráció
2001. | Érvénytelen beviteli eszközök | Hiányzik a bemeneti eszközök vagy üres eszköz.
2002. | Érvénytelen jegyzék | Jegyzék üres vagy jegyzék érvénytelen elemeket tartalmaz.
2003 | Nem sikerült letölteni a multimédiás fájl | Érvénytelen URL-címe a nyilvánvalóan fájlban.
2004. | Nem támogatott Protocol (protokoll) | Protocol (protokoll) media URL-cím nem támogatott.
2005 | Nem támogatott formátumba | Beviteli médiatípus fájl nem támogatott.
2006 | Túl sok bemeneti fájlok | A beviteli jegyzék a 10-nél több fájlok vannak.
3000 | Médiafájl dekódolását sikertelen volt. | Nem támogatott media kodek <br/>vagy<br/> Sérült médiafájl <br/>vagy<br/> Nincs beviteli media audio adatfolyam.
4000 | Részlegesen sikerült köteg indexelés | A beviteli médiafájlok részét vannak nem sikerült indexelni. További tudnivalókért olvassa el a <a href="#output_files">kimeneti fájlok</a>című témakört.
más | Több belső hiba | Kérjük, forduljon a támogatási csoporthoz. indexer@microsoft.com


## <a id="supported_languages"></a>A támogatott nyelvek

Jelenleg angol és spanyol nyelven támogatottak. További tudnivalókért lásd: [az 1.2-es verzió megjelenési blogbejegyzés](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Kapcsolódó hivatkozások

[Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)

[AIB fájlok használata az Azure Media indexelő és az SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Az indexelési médiafájlok Azure Media indexelő 2 előzetes verzióban](media-services-process-content-with-indexer2.md)
