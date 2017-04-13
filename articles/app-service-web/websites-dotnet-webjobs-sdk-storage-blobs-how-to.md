<properties 
    pageTitle="A WebJobs SDK Azure blob-tárolóhoz használata" 
    description="Megtudhatja, hogy miként Azure blob-tárolóhoz használata a WebJobs SDK csomagjában talál. Egy folyamat elindítása, mikor jelenjen meg egy új blob tároló, és kezelheti a "poison BLOB"." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>A WebJobs SDK Azure blob-tárolóhoz használata

## <a name="overview"></a>– Áttekintés

Ez az útmutató C# kód példák, amelyek bemutatják, hogyan indíthatja el egy folyamat, amikor létrejön vagy frissül egy Azure blob. A mintakódok [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzióját használja 1.x.

BLOB, mintakódok, amelyek bemutatják, hogyan hozhat létre [a WebJobs SDK csomagjában talál az Azure várólista tároló használata](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)témakörben talál. 
        
Az útmutató feltételezi, hogy tudja, [hogyan kell a kapcsolati karakterláncot, mutasson a tárterület-fiók a Visual Studióban WebJob projekt létrehozása](websites-dotnet-webjobs-sdk-get-started.md) vagy [több tárterületet](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs)fiókhoz.

## <a id="trigger"></a>Hogy miként indíthatja el egy függvényt, amikor létrejön vagy frissül egy blob

Ebből a szakaszból megtudhatja, hogyan kell használni a `BlobTrigger` attribútum. 

> [AZURE.NOTE] A WebJobs SDK nézze meg új vagy módosított BLOB-naplófájlok ellenőrzése. Ez a folyamat nem valós idejű; a függvény előfordulhat, hogy nem kérjen elindítja néhány perccel vagy már a blob létrehozása után. Emellett az alap [tárterület-naplófájlokat hoz létre, a "legjobb erőfeszítéseket"](https://msdn.microsoft.com/library/azure/hh343262.aspx) ; nem sem garantálja, hogy az összes eseményének fog kell irányítania. Bizonyos körülmények között a naplók kihagyott kell. Blob indítók sebességétől és megbízhatóságára vonatkozó korlátozások az alkalmazás nem elfogadhatók, ha a javasolt módszer-e várólista üzenet létrehozása a blob létrehozása, és a [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribútum helyett használja a `BlobTrigger` attribútum a blob a függvény.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Blob kiterjesztésű egyetlen helyőrzője  

A következő kódot minta másolja be a *bemeneti* tároló a *kimeneti* tárolóhoz megjelenő szöveg BLOB:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Az attribútum konstruktora karakterlánc paramétert tároló neve és a blob nevének helyőrzője. Ebben a példában ha a *bemeneti* tároló *Blob1.txt* nevű blob jön létre a függvény hoz létre *Blob1.txt* a *kimeneti* tárolóban nevű blob. 

A blob neve helyőrző neve mintát ahogy a következő kódot minta adhatja meg:

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

## <a id="types"></a>BLOB kell kötni adattípusa

Használhatja a `BlobTrigger` attribútum az alábbiak:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Más típusú által [ICloudBlobStreamBinder](#icbsb) 

Ha közvetlenül dolgozhat az Azure tárterület-fiókot szeretne, akkor is hozzáadhat egy `CloudStorageAccount` a módszer aláírás paramétert.

Példák című témakörben talál [az azure-webjobs-sdk tárházából GitHub.com blob kötés kódot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Az első karakterlánc kötés blob szövegtartalom

Ha a szöveg BLOB várják, `BlobTrigger` is alkalmazható egy `string` paraméter. A következő kódot minta köti a szöveg blob egy `string` nevű paraméter `logMessage`. A függvény a paraméter használja a blob tartalmának írni az WebJobs SDK irányítópulton. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Az első szerializált blob-tartalom ICloudBlobStreamBinder használatával

A következő kódot minta használ, amely osztály `ICloudBlobStreamBinder` ahhoz, hogy a `BlobTrigger` attribútum blob szeretné kötni a `WebImage` típusát.

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

A `WebImage` kód kötelező megadni egy `WebImageBinder` származik osztály `ICloudBlobStreamBinder`.

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

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>A kiváltó blob blob elérési beszerzése

A tároló és blob nevét az blob, amely elindítja a függvény az első, tartalmaz egy `blobTrigger` karakterlánc-függvény aláírás paramétert.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Poison BLOB kezelése

Ha egy `BlobTrigger` függvény sikertelen, a SDK meghívja visszatérve abban az esetben, ha a hiba miatt nem tranziens hiba. A blob tartalmát okozza a hibát, ha a parancs nem működik, minden alkalommal, amikor megkísérli a blob feldolgozása. Alapértelmezés szerint a SDK szólít fel egy függvény legfeljebb 5 megszorzása egy adott blob. Ha nem sikerül az ötödik próbálja meg, a SDK üzenet ad *webjobs-blobtrigger-mérgezett*nevű várólista.

Újrapróbálkozások legfeljebb hány állítható be. BackStyle [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) poison blob-kezelési és poison várólista üzenetkezelés szolgál. 

A várakozási üzenet poison BLOB-egy JSON-objektum, amely tartalmazza a következő tulajdonságokat:

* FunctionId (a *{WebJob neve}*formátumban. Funkciók. *{Függvénynevet}*, például: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" vagy "PageBlob")
* ContainerName
* BlobName
* ETag (egy blob verziójának azonosítója, például: "0x8D1DC6E70A277EF")

A következő kódot mintában a `CopyBlob` függvénynek kódot, amely megjeleníti, hogy minden olyan alkalommal, amikor ennek neve sikertelen lesz. Miután a SDK hívások engedélyezése a újrapróbálkozások maximális száma egy üzenetet a poison blob várólista jön létre és az üzenet által feldolgozott a `LogPoisonBlob` függvény. 

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

A SDK automatikusan deserializes a JSON üzenetet. Az alábbiakban a `PoisonBlobMessage` osztály: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>BLOB lekérdezési algoritmus

A WebJobs SDK megvizsgálja által meghatározott összes tárolók `BlobTrigger` attribútumok alkalmazás indítása elemre. Egy nagy tároló fiókban ellenőrzés időt vehet igénybe néhány, előtt új BLOB talál egy ideig előfordulhat, hogy és `BlobTrigger` függvények végrehajtása.

Új vagy módosított BLOB feltárása alkalmazás indítása után, a SDK rendszeres beolvassa a blob-tároló naplókat. A blob naplók pufferelt vannak, és csak első fizikailag írt 10 percenként vagy, így lehet jelentős késleltetés után blob létrehozásakor vagy frissítésekor, mielőtt a megfelelő `BlobTrigger` végrehajtja a függvény. 

Kivétel használatával létrehozott BLOB-a `Blob` attribútum. Amikor a WebJobs SDK létrehoz egy új blob, továbbítja az új blob azonnal bármely egyező `BlobTrigger` függvények. Ezért ha egy lánc blob ráfordítások és a kimeneti értékeket, a SDK csomagjában talál is feldolgozni azokat hatékonyan. Ha azt szeretné, hogy fut a blob feldolgozás függvények létrehozott vagy más módon frissülnek BLOB-kis késés, használata ajánlott, de `QueueTrigger` helyett `BlobTrigger`.

### <a id="receipts"></a>BLOB visszaigazolások

A WebJobs SDK azt ellenőrzi, hogy nincs `BlobTrigger` függvény többször nevű kap egy új vagy frissített blob. Mindezt *blob visszaigazolások* fenntartása érdekében határozza meg, ha egy adott blob-verzió megtörtént feldolgozása.

*Azure-webjobs-hosts* a AzureWebJobsStorage kapcsolati karakterláncban megadott Azure tároló fiók nevű tároló BLOB visszaigazolások tárolja. Blob visszaigazolását rendelkezik az alábbi adatokat:

* A függvény, amely hívták meg az a blob ("*{WebJob neve}*. Funkciók. *{Függvénynevet}*", például:"WebJob1.Functions.CopyBlob")
* A tároló neve
* A blob típusa ("BlockBlob" vagy "PageBlob")
* A blob neve
* A ETag (egy blob verziójának azonosítója, például: "0x8D1DC6E70A277EF")

Ha szeretne blob újrafeldolgozása kötelező, kézzel is törölheti az, hogy a blob-blob-visszaigazolás az *azure-webjobs-hosts* tárolóból.

## <a id="queues"></a>A sorok cikkben foglalt Kapcsolódó témakörök

Információ arról, hogy miként kezelje a várólista üzenet által indított blob-feldolgozási, illetve WebJobs SDK blob-nem jellemző felhasználási területei feldolgozása, megtudhatja, [hogy miként használja a WebJobs SDK csomagjában talál az Azure várólista tároló](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Ebben a cikkben szereplő kapcsolódó témakörök a következők:

* Aszinkron függvények
* Több példányban
* Biztonságos leállítása
* WebJobs SDK attribútumok törzsében, a függvény használata
* Beállíthatja a SDK a csatlakozási_karakterlánc kódot.
* Értékek az WebJobs SDK konstruktor paramétereket állíthat a kódot.
* Állítsa be `MaxDequeueCount` poison blob-kezeléshez.
* Kézzel indítja el függvény
* Naplók írása

## <a id="nextsteps"></a>Következő lépések

Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezelheti a esetei Azure BLOB-használata. Azure WebJobs és a WebJobs SDK használatával kapcsolatos további tudnivalókért lásd: [Azure WebJobs ajánlott erőforrásokat](http://go.microsoft.com/fwlink/?linkid=390226).
 
