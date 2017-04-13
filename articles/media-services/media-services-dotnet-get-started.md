<properties
    pageTitle="Első lépések a tartalom továbbítása .NET használatával igény |} Azure"
    description="Ebben az oktatóanyagban végigvezeti a végrehajtási igény szerint a tartalomkézbesítési alkalmazás az Azure Media Services .NET használatával."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Első lépések a tartalom .NET SDK használatával igény továbbítása

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>– Áttekintés 

Ebben az oktatóanyagban végigvezeti a végrehajtási egy igény szerinti videó (VoD) tartalomkézbesítési alkalmazás segítségével az Azure Media Services (AMS) SDK a .NET rendszerhez.


Az oktatóprogram vezet be, az egyszerű Media Services munkafolyamat és a leggyakoribb programozási objektumok és a Media Services fejlesztési szükséges feladatokat. Az oktatóanyag végén fogja tudni adatfolyam vagy fokozatosan feltölteni, kódolt és letöltött minta multimédiás fájl letöltése.

## <a name="what-youll-learn"></a>A tananyag Dióhéjban

Az oktatóprogram szemlélteti, hogyan lehet az alábbi feladatok elvégzéséhez:

1.  Fiók hozzárendelése Media Services (az Azure portál használatával).
2.  Állítsa be a továbbított végpont (az Azure portál használatával).
3.  Hozzon létre, és állítsa be a Visual Studio projekt.
5.  Csatlakozás a Media Services-fiókjába.
6.  Hozzon létre egy új eszközt, és töltse fel a videofájlokat.
7.  A forrásfájl kódolását be adaptív átviteli sebesség MP4 fájlokat.
8.  Az eszköz közzétegye és URL-címek adatfolyam és fokozatos letöltésre.
9.  Tesztelje a tartalom lejátszása.

## <a name="prerequisites"></a>Előfeltételek

Az alábbiak szükségesek az oktatóprogram elvégzéséhez.

- Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. 
    
    Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](/pricing/free-trial/?WT.mc_id=A261C142F). Próbálja ki az Azure szolgáltatások fizetett használható jóváírások kap. Után a credits használt, megtarthatja a fiókot, és ingyenes Azure szolgáltatásokat és funkciókat, például a Web Apps alkalmazások funkció Azure App szolgáltatásban.
- Operációs rendszer: A Windows 8-as vagy újabb verzió Windows 2008 R2, Windows 7.
- .NET-keretrendszer 4.0-s vagy újabb verzió
- Visual Studio 2010 SP1 (Professional, prémium, Ultimate vagy Express) vagy újabb verziók.


##<a name="download-sample"></a>Töltse le a minta

Szerezze be és lebonyolítása minta [Itt](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Az Azure portálon Azure Media Services fiók létrehozása

Ez a szakasz lépései bemutatják, hogyan hozhat létre AMS fiókot.

1. Jelentkezzen be a az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+ Új** > **Media + CDN** > **Media Services**.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. A **MEDIA SERVICES-fiók létrehozása** adja meg a szükséges értékeket.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. A **Fiók neve**mezőbe írja be az új AMS fiók nevére. A Media Services fióknév összes kisbetűs számokat vagy betűket, szóközök nélkül, és 3-24 karakter hosszú.
    2. Az előfizetését a különböző Azure-előfizetések listáját van hozzáférése közül választhat.
    
    2. Válassza az új vagy meglévő erőforrás **Erőforráscsoport**.  Erőforráscsoport információforrások, amelyek életciklus, engedélyeket és házirendeket megosztása gyűjteménye. További tudnivalók [az alábbi](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Helyét**jelölje ki a földrajzi területhez tartozik használják a média és metaadatok rekordokat a Media Services fiók tárolásához. Ez a terület folyamat, és a multimédia-adatfolyam szolgál. A rendelkezésre álló Media Services régiók jelennek meg a legördülő listából. 
    
    3. **Tárterület-fiókot**válassza ki a tárhely adja meg a Media Services fiókjából médiatartalom blob-tárolóhoz. Kijelölhet egy meglévő tárterület-fiókot a Media Services fiókként ugyanabban a földrajzi régióban, vagy létrehozhat egy tárterület-fiókkal. Új tároló fiók ugyanabban a régióban jön létre. Tárterület fióknevét szabályairól megegyeznek a Media Services fiókok.

        További tudnivalók a tárhely [Itt](storage-introduction.md).

    4. Válassza a **Irányítópult PIN-kódot** a fiók telepítési állapotának megtekintéséhez.
    
7. Kattintson a **Create** a képernyő alján.

    Miután sikeresen létrejön a fiók, az állapot **futó**változik. 

    ![Multimédia-szolgáltatások beállításai](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS fiókjának kezelését szeretné (például videó feltöltése, kódolását eszközök, figyelheti a projekt előrehaladásának) a **Beállítások** ablakban.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Az Azure portálon adatfolyam végpontok konfigurálása

Amikor az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség videofunkcióinak elkötelezett a leggyakoribb esetek egyike Azure Media Services használata. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak).

Dinamikus csomagolása, amely lehetővé teszi a adaptív átviteli sebesség kódolt MP4 tartalmának nélkül ezeknek a folyamatos átvitelű formátumok előre csomagolt változatának tárolni a Media Services (MPEG kötőjel HLS, zökkenőmentes Streaming, HDS) csak igény, által támogatott streaming előadásához Media Services ismertetése

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- Emeletes (forrás) fájlját kódolását be adaptív átviteli sebesség MP4-fájlokat (a kódolási lépéseket mutatni a később az oktatóprogram vannak).  
- Hozzon létre egy adatfolyam mennyisége, a *folyamatos átvitelű végpontot* , amelyhez használni kíván kézbesítési a tartalom. Az alábbi lépésekkel bemutatják, hogyan módosíthatja az adatfolyam egységek számát.

Dinamikus csomagolása, csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services hoz létre, és a megfelelő választ, egy ügyfél érkező kérések alapján szolgál.

Szeretne létrehozni, és módosítsa a folyamatos átvitelű fenntartott egységek számát, tegye a következőket:


1. Kattintson a **Beállítások** ablak **adatfolyam végpontok**. 

2. Kattintson az alapértelmezett streaming végpontot. 

    A **Folyamatos ÁTVITELŰ VÉGPONT ADATAIT alapértelmezett** ablak.

3. Adja meg az adatfolyam egységek számát, húzza a **adatfolyam egységek** csúszkát.

    ![A folyamatos átvitelű erőforrás-mennyiség](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kattintson a **Mentés** gombra a módosítások mentéséhez.

    >[AZURE.NOTE]A felosztás minden új egységek is legfeljebb 20 percet igénybe vehet.

##<a name="create-and-configure-a-visual-studio-project"></a>Létrehozása és konfigurálása a Visual Studio projekt

1. Hozzon létre egy új C# konzol alkalmazást a Visual Studio 2013, a Visual Studio 2012 vagy a Visual Studio 2010 SP1. **Megoldás neve**, **helyét**és **nevét**, és kattintson **az OK**gombra.

2. A [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) NuGet csomag használatával telepítse az **Azure Media Services .NET SDK-bővítmények**.  A Media Services .NET SDK-bővítmények bővítmény módszerek és a segítő funkciók, amelyek a kód egyszerűbbé és egyszerűbben a Media Services kidolgozása. A csomag telepítésekor is **Media Services.NET SDK** telepíti, és hozzáadja szükséges függőségek.

3. System.Configuration összeállítás mutató hivatkozás hozzáadása. Ez az összeállítás konfigurációs fájlokat, például App.config eléréséhez használt **System.Configuration.ConfigurationManager** osztály tartalmazza.

4. Nyissa meg a App.config fájlt (a fájl hozzáadása a projekthez Ha alapértelmezés szerint nem került), és egy *appSettings* szakasz hozzáadása a fájlhoz. Az Azure Media Services nevét és a fiók fiókkulcs, értékeinek beállítása, az alábbi példában látható módon. Szerezze be a fiók nevét, és a főbb információk, az [Azure-portálra](https://portal.azure.com/) , és jelölje ki a AMS fiókját. Válassza a **Beállítások** > **kulcsok**. A Manage billentyűk windows jeleníti meg a fiók nevét, és az elsődleges és másodlagos kulcsok jelenik meg.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. A meglévő **használata** kimutatásokban elején Program.cs fájl felülírja a következő kódot.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Hozzon létre egy új mappát, a projektek címtár csoportban, és másolja a-.mp4 vagy .wmv fájlt szeretne kódolni és adatfolyam vagy fokozatosan letöltése. Ebben a példában a "C:\VideoFiles" elérési út használható.

##<a name="connect-to-the-media-services-account"></a>A Media Services fiók csatlakoztatása

A .NET Media Services használatakor kell használnia a **CloudMediaContext** osztály a legtöbb Media Services feladatok programozási: keresztüli Media Services; létrehozása, módosítása, elérése és törlése a következő objektumoknak: eszközök, fájlok eszköz, feladatok, hozzáférési, Locator stb.

Az alapértelmezett Program osztály felülírja a következő kódot. A kód bemutatja, hogyan App.config-fájlból olvassa el a kapcsolat értékeket és hogyan hozhat létre a **CloudMediaContext** objektum Media Services való csatlakozáshoz. További információt a Media Services való csatlakozással kapcsolatban olvassa el a [Csatlakozás a .NET rendszerhez, a Media Services SDK a Media Services](http://msdn.microsoft.com/library/azure/jj129571.aspx).

A **fő** függvény felhívja módszereket határozza meg, ebben a szakaszban további.

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
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Hozzon létre egy új eszközt, és töltse fel a videofájl

Media Services, az Ön feltöltése (vagy ingest) a digitális eszköz fájlokat. Az **eszköz** entitás is tartalmazhat, videó, hang, képek, miniatűrök gyűjtemények, szöveg számok és kódolt felirat fájlok (és ezek a fájlok metaadatait.)  A fájlok feltöltése befejeződik, amikor a tartalom tárolási biztonságosan további feldolgozásra és a folyamatos átvitelű a felhőben. Az eszköz a fájlok **Eszköz fájlok**nevezik.

A **UploadFile** módszer definiált alatti hívások **CreateFromFile** (.NET SDK bővítmények megadva). **CreateFromFile** létrehoz egy új eszközt, amelybe a megadott forrás fájl feltöltésekor.

A **CreateFromFile** módszer veszi **AssetCreationOptions** , amely lehetővé teszi, hogy jelenjen meg az eszköz létrehozása az alábbi lehetőségek közül:

- **Nincs** - titkosítás nem használatos. Az alapértelmezett értéket. Figyelje meg, hogy amikor ezt a beállítást, a tartalom nem védett a hálózaton átvitt vagy tárolóban lévő többi.
Ha szeretne egy MP4 előadása fokozatos letöltése, használja a használja ezt a beállítást.
- **StorageEncrypted** – ezzel a beállítással a világos tartalom helyileg Speciális titkosítási szabványos (AES használ)-256 bit, majd feltölti azt tárolására szolgáló Azure tárhely, amely titkosítani titkosított a többi használatát. Tárterület-titkosítással védett eszközök automatikusan titkosítatlan és egy titkosított fájlrendszer előtt kódolást helyezett, és tetszés szerint újra titkosított egy új kimeneti eszközként feltöltése előtt. Az elsődleges használatieset az tárterület-titkosítás esetén, a kiváló minőségű beviteli médiafájlokat a többi titkosítású a lemezen védeni kívánt.
- **CommonEncryptionProtected** – ezt a lehetőséget, ha Ön már titkosított és látták el közös titkosítást vagy PlayReady DRM (például zökkenőmentes Streaming védett a PlayReady DRM) tartalma feltöltése.
- **EnvelopeEncryptionProtected** – ezt a lehetőséget, ha vannak feltöltése a titkosított AES HLS. Ne feledje, hogy a fájlok kell van már kódolt átalakítása Manager titkosítja.

A **CreateFromFile** módszerrel is lehetővé teszi annak érdekében, hogy a fájl feltöltése állapotának jelentése az adja meg a visszahívás.

A következő példa azt adja meg, **nincs** az eszköz lehetőségekről.

Az alábbi módon adja hozzá a Program osztály.

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


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>A forrásfájl kódolását be adaptív átviteli sebesség MP4 fájlokat

Eszközök ingesting Media Services be, miután media lehet kódolt transmuxed, vízjellel, és így tovább, ügyfeleknek átadása előtt. Ezek a tevékenységek ütemezett és futtassanak több háttér szerepkör példányokat nagy teljesítmény és elérhetősége érdekében. Ezek a tevékenységek úgynevezett feladatokat, és minden feladat atomi feladatok, amelyek a tényleges munkát a digitáliseszköz-fájl állhatnak.

Volt már említettük, amikor az Azure Media Services használata, mint a leggyakoribb esetek egyike elkötelezett adaptív átviteli sebesség, a folyamatos átvitelű az ügyfelek számára. Media Services dinamikusan is csomag adaptív átviteli sebesség MP4 fájlokat be a következő formátumokban: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak).

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- Kódolását vagy alakítható át az Emeletes (forrás) fájl adaptív átviteli sebesség MP4 vagy adaptív átviteli sebesség zökkenőmentes Streaming fájlokat alakítja.  
- Szerezze be legalább egy adatfolyam egység az adatfolyam végpontot, amelyből tervezi kézbesítési a tartalmakat.

A következő kódrészlet egy kódolási feladat elküldése mutatja. A feladat, amely meghatározza nem alakítható át az Emeletes fájl adaptív átviteli sebesség MP4s használatával **Media Encoder szabványos**alakítja egy tevékenység tartalmazza. A kód elküldi a feladatot, és megvárja, amíg befejeződik.

A feladat befejezése után lenne tudni a digitáliseszköz-adatfolyam vagy fokozatosan a eredményeként transcoding készített MP4-fájlokat letölteni.
Látható, hogy nem kell több, mint 0 adatfolyam egységből áll a fokozatosan a MP4 fájlok letöltése érdekében.

Az alábbi módon adja hozzá a Program osztály.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Az eszköz közzétegye és URL-címek adatfolyam és fokozatos letöltésre

Adatfolyam-vagy letöltése tárgyi eszköz, először kell "közzététel": hozzon létre egy megnevezés. Locator az eszköz tárolt fájlok hozzáférést biztosít. Media Services támogatja a kétféle Locator: OnDemandOrigin Locator, használva továbbítani a médiafájlokat (például MPEG kötőjel, HLS vagy zökkenőmentes Streaming) és az Access aláírás (Társítások) Locator, médiafájlok letöltéséhez használt.

Miután létrehozta a Locator, az URL-adatfolyam-vagy a fájlok letöltése hozhat létre.


Adatfolyam URL-zökkenőmentes Streaming formátuma a következő:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Adatfolyam URL-HLS formátuma a következő:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-kötőjel adatfolyam URL-CÍMÉT az alábbi formátumban van:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Fájlok letöltése használt Társítások URL-címet a következő formátumban foglalja magában:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK bővítmények kényelmes segítő módszerek formázott URL-ek a közzétett eszköz visszaadó biztosítanak.

A következő kódot a .NET SDK bővítmények Locator létrehozása és a folyamatos átvitelű beszerzése és a progresszív letöltés URL-címeket használja. A kódot is mutatja egy helyi mappába a fájlok letöltése.

Az alábbi módon adja hozzá a Program osztály.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>A tartalom lejátszásával tesztelése  

Miután futtatja a program az előző szakaszban definiált, az alábbihoz hasonló URL-címek konzol ablakban fog megjelenni.

Adaptív adatfolyam URL-címek:

Zökkenőmentes Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG-KÖTŐJEL

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Progresszív letöltés URL-ek (hang és videó).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Adatfolyam-, videó, használja az [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Tesztelje a progresszív letöltésére, illessze be egy URL-CÍMÉT (például az Internet Explorer, a Chrome vagy a Safari) böngészőben.


##<a name="next-steps-media-services-learning-paths"></a>Következő lépések: A Media Services tanulási javaslatok

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Mást keres?

Ha ez a témakör nem tartalmaznak, mi, várt, valamit, amit hiányzik, és a más módon nem felel meg az igényeinek, és adja meg az alábbi Disqus szál használatával visszajelzését.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
