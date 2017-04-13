<properties
    pageTitle="Első lépések a blob tárhely és a Visual Studio a csatlakoztatott szolgáltatások (WebJob projektek) |} Microsoft Azure"
    description="Hogyan kezdjek hozzá az Blob-tárolóhoz WebJob projekt-Azure tároló Visual Studio segítségével történő csatlakozás után szolgáltatások össze."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Első lépések az Azure Blob tárhely és a Visual Studio a csatlakoztatott szolgáltatások (WebJob projektek)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Ebben a cikkben C# kód példák, amelyek bemutatják, hogyan indíthatja el egy folyamat, amikor létrejön vagy frissül egy Azure blob. A mintakódok [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verziójának használatához 1.x. Amikor a tárterület-fiók hozzáadása WebJob projekthez a **Csatlakoztatott szolgáltatások hozzáadása** a Visual Studio párbeszédpanel segítségével, a megfelelő Azure tároló NuGet csomag telepítve van, a megfelelő .NET hivatkozások vannak a projekthez való hozzáadásának, és a tárterület-fiók csatlakozási karakterláncok frissülnek a App.config fájlban.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Hogy miként indíthatja el egy függvényt, amikor létrejön vagy frissül egy blob

Ebben a szakaszban a **BlobTrigger** attribútum használatát mutatja.

 **Megjegyzés:** A WebJobs SDK nézze meg új vagy módosított BLOB-naplófájlok ellenőrzése. Ez a folyamat eleve lassú; a függvény előfordulhat, hogy nem első elindítja néhány perccel vagy már a blob létrehozását követően.  Ha az alkalmazás azonnal feldolgozása BLOB, az ajánlott módszer várólista üzenet létrehozása a blob létrehozása, és a [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribútum használata helyett a **BlobTrigger** attribútum a blob a függvény.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Blob kiterjesztésű egyetlen helyőrzője  

A következő kódot minta másolja be a *bemeneti* tároló a *kimeneti* tárolóhoz megjelenő szöveg BLOB:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Az attribútum konstruktora karakterlánc paramétert tároló neve és a blob nevének helyőrzője. Ebben a példában ha a *bemeneti* tároló *Blob1.txt* nevű blob jön létre a függvény hoz létre *Blob1.txt* a *kimeneti* tárolóban nevű blob.

A blob neve helyőrző neve mintát adhatja meg a következő kódot minta látható módon:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Ez a kód csak BLOB kezdetű "eredeti-" nevű másolja. *Eredeti-Blob1.txt* a *bemeneti* tárolóban például *Másolása-Blob1.txt* a *kimeneti* tárolóban másolja.

Kell neve minta megadása blob neveket a kapcsos zárójelek között a nevét, ha duplán a kapcsos zárójeleket. Ha meg szeretné keresni BLOB a *képek* tárolóban ilyen nevű például:

        {20140101}-soundfile.mp3

Ezt a minta használja:

        images/{{20140101}}-{name}

A példában a *név* helyőrző érték *soundfile.mp3*lesz.

### <a name="separate-blob-name-and-extension-placeholders"></a>Külön blob neve és kiterjesztése helyőrzők

A következő kódot minta másolja át a *bemeneti* tároló a *kimeneti* tárolóhoz megjelenő BLOB kiterjesztéssel változik. A kód naplózza a *bemeneti* blob kiterjesztését, és a *kimeneti* blob kiterjesztését *.txt*állítja be.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a name="types-that-you-can-bind-to-blobs"></a>BLOB kell kötni adattípusa

A következő típusú felhasználhatja a **BlobTrigger** attribútum:

* **karakterlánc**
* **TextReader**
* **Adatfolyam**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Más típusú által [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Ha szeretne közvetlenül dolgozhat az Azure tárterület-fiókot, felveheti is **CloudStorageAccount** paraméter az módszer aláírást.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Az első karakterlánc kötés blob szövegtartalom

Ha a szöveg BLOB várják, **BlobTrigger** **karakterlánc** paraméter alkalmazhatók. A következő kódot minta szöveg blob kötődik **logMessage**nevű **karakterlánc** paraméter. A függvény a paraméter használja a blob tartalmának írni az WebJobs SDK irányítópulton.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Az első szerializált blob-tartalom ICloudBlobStreamBinder használatával

A következő kódot minta használ, amely **ICloudBlobStreamBinder** ahhoz, hogy a **BlobTrigger** attribútum blob kötést létrehozni a **WebImage** típus osztály.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK",
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

A **WebImage** kötés kódot megadva olyan **WebImageBinder** osztályban **ICloudBlobStreamBinder**származik.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input,
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="how-to-handle-poison-blobs"></a>Poison BLOB kezelése

Ha nem sikerül **BlobTrigger** függvény, a SDK csomagjában talál hívások rá újra, abban az esetben, ha a hiba miatt nem tranziens hiba. A blob tartalmát okozza a hibát, ha a parancs nem működik, minden alkalommal, amikor megkísérli a blob feldolgozása. Alapértelmezés szerint a SDK szólít fel egy függvény legfeljebb 5 megszorzása egy adott blob. Ha nem sikerül az ötödik próbálja meg, a SDK üzenet ad *webjobs-blobtrigger-mérgezett*nevű várólista.

Újrapróbálkozások legfeljebb hány állítható be. BackStyle [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) poison blob-kezelési és poison várólista üzenetkezelés szolgál.

A várakozási üzenet poison BLOB-egy JSON-objektum, amely tartalmazza a következő tulajdonságokat:

* FunctionId (a *{WebJob neve}*formátumban. Funkciók. *{Függvénynevet}*, például: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" vagy "PageBlob")
* ContainerName
* BlobName
* ETag (egy blob verziójának azonosítója, például: "0x8D1DC6E70A277EF")

A **CopyBlob** függvény a következő kódot minta kódot, amely megjeleníti, hogy minden olyan alkalommal, amikor ennek neve nem sikerül tartalmaz. A SDK felhívja újrapróbálkozások maximális számát, miután egy üzenetet a poison blob várólista jön létre, és ezt az üzenetet a **LogPoisonBlob** függvény által feldolgozott.

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

A SDK automatikusan deserializes a JSON üzenetet. Az alábbiakban a **PoisonBlobMessage** osztály:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB lekérdezési algoritmus

A WebJobs SDK ellenőrzi az alkalmazás indítása **BlobTrigger** attribútumok által meghatározott összes tárolók. Egy nagy tároló fiókban a beolvasott képet készíthet egy kis időt, új BLOB találhatók, és **BlobTrigger** függvények végrehajtása előtt egy ideig előfordulhat, hogy.

Új vagy módosított BLOB feltárása alkalmazás indítása után, a SDK rendszeres beolvassa a blob-tároló naplókat. A blob naplók pufferelt vannak, és csak első fizikailag írt 10 percenként vagy, így lehet jelentős késleltetés után blob létrehozásakor vagy frissítésekor, mielőtt a megfelelő **BlobTrigger** függvény hajt végre.

A **Blob** -attribútum használatával létrehozott BLOB kivételt van. Ha a WebJobs SDK hoz létre egy új blob, továbbítja az új blob azonnal bármely egyező **BlobTrigger** funkciót. Ezért ha egy lánc blob ráfordítások és a kimeneti értékeket, a SDK csomagjában talál is feldolgozni azokat hatékonyan. De ha azt szeretné, hogy a létrehozott vagy más módon frissülnek BLOB-függvények feldolgozása blob futó kis késés, használata ajánlott **BlobTrigger**, hanem **QueueTrigger** .

### <a name="blob-receipts"></a>BLOB visszaigazolások

A WebJobs SDK meggyőződik arról, hogy nincs **BlobTrigger** függvény kap meghívott többször az azonos új vagy frissített blob. Mindezt *blob visszaigazolások* fenntartása érdekében határozza meg, ha egy adott blob-verzió megtörtént feldolgozása.

*Azure-webjobs-hosts* a AzureWebJobsStorage kapcsolati karakterláncban megadott Azure tároló fiók nevű tároló BLOB visszaigazolások tárolja. Blob visszaigazolását rendelkezik az alábbi adatokat:

* A függvény, amely hívták meg az a blob ("*{WebJob neve}*. Funkciók. *{Függvénynevet}*", például:"WebJob1.Functions.CopyBlob")
* A tároló neve
* A blob típusa ("BlockBlob" vagy "PageBlob")
* A blob neve
* A ETag (egy blob verziójának azonosítója, például: "0x8D1DC6E70A277EF")

Ha szeretne blob újrafeldolgozása kötelező, kézzel is törölheti az, hogy a blob-blob-visszaigazolás az *azure-webjobs-hosts* tárolóból.

## <a name="related-topics-covered-by-the-queues-article"></a>A sorok cikkben foglalt Kapcsolódó témakörök

Információk arról, hogy miként kezelje a várólista üzenet által indított blob-feldolgozási, illetve WebJobs SDK nem jellemző blob-esetek feldolgozása, megtudhatja, [hogy miként használja a WebJobs SDK csomagjában talál az Azure várólista tároló](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Ebben a cikkben szereplő kapcsolódó témakörök a következők:

* Aszinkron függvények
* Több példányban
* Biztonságos leállítása
* WebJobs SDK attribútumok törzsében, a függvény használata
* Beállíthatja a SDK a csatlakozási_karakterlánc kódot.
* Értékek az WebJobs SDK konstruktor paramétereket állíthat a kódot.
* Állítsa be a **MaxDequeueCount** poison blob-kezeléshez.
* Kézzel indítja el függvény
* Naplók írása

## <a name="next-steps"></a>Következő lépések

Ez a cikk nyújtott mintakódok, amelyek bemutatják, hogyan kezelheti a esetei Azure BLOB-használata. Azure WebJobs és a WebJobs SDK használatáról további információt az [Azure WebJobs dokumentáció forrásokban](http://go.microsoft.com/fwlink/?linkid=390226)talál.
