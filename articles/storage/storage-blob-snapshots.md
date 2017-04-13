<properties
    pageTitle="Hozzon létre egy blob csak olvasható pillanatképét |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre az adatok biztonsági mentéséhez blob egy adott időpillanatban blob pillanatképét. Ismerje meg, hogyan pillanatképek számlázva vannak és használatuk kapacitás költségek csökkentése érdekében."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="create-a-blob-snapshot"></a>Hozzon létre egy blob-pillanatfelvétel

## <a name="overview"></a>– Áttekintés

Pillanatkép blob, amely egy pontján származik, az idő írásvédett verziója fut. Egy adott időpontban érvényes hasznosak BLOB mentésével. Miután létrehozott egy pillanatképet, olvassa el, másolása vagy törölheti azt, de nem tudja módosítani.

Blob pillanatképét megegyezik az alap blob, azzal a különbséggel, hogy a blob URI egy **DateTime típusú** érték, a blob az időpontot, amelynél a pillanatkép felvételének URI fűzött. Például ha az weblapon blob-URI is `http://storagesample.core.blob.windows.net/mydrives/myvhd`, a URI hasonlít pillanatkép `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Az összes pillanatképek ossza meg az alap blob URI. Az alap blob- és a pillanatkép csak megkülönböztetése az az hozzáfűzött **DateTime típusú** érték.

Tetszőleges számú pillanatképek blob van. Egy adott időpontban érvényes továbbra is fennáll, amíg kifejezetten törlődnek. Pillanatkép nem outlive az alap blob. Egy adott időpontban érvényes, az aktuális pillanatképek nyomon követésére a viszonyítási blob társított is számbavétele.

Amikor létrehoz egy blob pillanatképét, a blob Rendszertulajdonságok másolandó a pillanatkép ugyanazokkal az értékekkel. Az alap blob-metaadatok is másolja a pillanatkép, ha nem ad meg, a pillanatkép külön metaadatait létrehozásakor.

Bármely az alap blob társított bérletek nem vonatkoznak a pillanatkép. Pillanatkép megtekintéséhez kattintson a bérleti nem szerezheti be.

## <a name="create-a-snapshot"></a>Pillanatkép létrehozása

Az alábbi példa a .NET pillanatfelvételt mutatja. Ebben a példában a pillanatkép külön metaadatait adja meg, amikor a létrehozás.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Egy adott időpontban érvényes másolása

Másolja a műveleteket magukban foglaló BLOB és pillanatképek kövesse ezeket a szabályokat.

- Pillanatkép másolhatja át az alap blob. Az alap blob látható helyzetére pillanatkép támogatásával visszaállíthatja blob korábbi verzióiban. A pillanatkép marad, de a következő blob a rendszer felülírja a pillanatkép írható másolatát.

- Pillanatkép másolhatja a cél blob, más néven. Az eredményül kapott cél blob pedig írható blob pillanatkép nem.

- Ha egy adatforrás blob másolt, minden olyan elérhető információk, amelyek a forrás blob nem kerülnek át a cél. Amikor a rendszer felülírja a cél blob másolatát, bármely, társított az eredeti cél blob pillanatképek változatlanok maradnak.

- A továbbfejlesztett fájlblokkolás blob pillanatképét létrehozásakor a blob lekötött Blokkokból álló lista is másolja a pillanatkép. Nem véglegesített téglalapok nem kerülnek.

## <a name="specify-an-access-condition"></a>Az access feltétel megadása

Megadhatja az access feltételt, hogy a pillanatkép hoz létre, csak akkor, ha egy feltétel teljesül. Az access feltételt megadni, használja a **AccessCondition** tulajdonságot. Ha a megadott feltétel nem teljesül, a pillanatkép nem jön létre, és a Blob-szolgáltatás állapotkód HTTPStatusCode.PreconditionFailed adja eredményül.

## <a name="delete-snapshots"></a>Egy adott időpontban érvényes törlése

Pillanatképek blob nem törölhető, kivéve, ha a pillanatképek is törlődnek. Pillanatkép törlése egyenként, vagy megadhatja, hogy az összes pillanatfelvételek a forrás blob törlésekor lehet törölni. Ha megpróbálja törlése blob, amely egy adott időpontban érvényes továbbra is tartalmaz, a egy hibát eredményez.

Az alábbi példa szemlélteti blob- és a .NET rendszerhez, a pillanatképek törlése hol `blockBlob` **CloudBlockBlob**típusú változó:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Azure prémium adathordozós pillanatképek

Használata pillanatképek prémium tároló kövesse ezeket a szabályokat:

- Egy lap blob egy prémium tároló fiókban pillanatképek maximális száma 100. Ez a korlát túllépésekor a a pillanatkép Blob művelet hibakód 409 (**SnapshotCountExceeded**) adja eredményül.

- Készíthet egy lap blob pillanatképét egy prémium tároló fiókban 10 percenként. A ráta túllépésekor a a pillanatkép Blob művelet hibakód 409 (**SnaphotOperationRateExceeded**) adja eredményül.

- Első Blob egy prémium tároló fiókban lap blob pillanatképét olvasható nem hívható meg. 400 (**InvalidOperation**) kódszámú hiba jelenik meg egy prémium tároló fiókban pillanatkép első Blob felhívja adja eredményül. Azonban felhívhatja Blob-tulajdonságok beolvasása és az első Blob-metaadatok egy prémium tároló fiókban pillanatkép szemben.

- Pillanatkép olvasható, a Másolás Blob művelet segítségével pillanatkép másolása másik oldal blob-fiók. A cél blob-a másolás nem rendelkezhet bármelyik meglévő pillanatképek. Ha a célként megadott blob pillanatképek, a Másolás Blob művelet hibakód 409 (**SnapshotsPresent**) adja eredményül.

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Az abszolút URI visszatérhet pillanatkép megtekintéséhez

A C# kód példa létrehoz egy pillanatképet, és írja meg az abszolút URI elsődleges helye.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Hogyan pillanatképek felmerülés a költségek ismertetése

Van egy blob egy írásvédett példányát, pillanatfelvétel létrehozásának eredményezhet további adatokat tároló díjak a fiókjába. Az alkalmazás tervezésekor fontos tudnia, hogyan lehet felmerülés ezeket a díjakat, hogy a felesleges költségeket kis méretűre.

### <a name="important-billing-considerations"></a>Fontos számlázási figyelembe veendő szempontok

Az alábbi lista tartalmazza-pillanatfelvétel létrehozásának szempontjai főbb pontjairól.

- Tárterület fiók egyedi blokkok vagy lapok, költségeket vonz. akár a blob vagy a pillanatkép. A fiók nem merülnek fel további díjai pillanatképek blob társított, amíg nem frissíti a blob, amelyen alapul. Miután módosította az alap blob, a pillanatképek eltér. Amikor ez történik, az előfizetést terhelő a egyedi blokkok vagy -lapokon, az egyes blob vagy pillanatkép.

- Amikor cserél blokkokból álló blob belül egy szövegrészt, a továbbfejlesztett fájlblokkolás később terheli egy egyedi sávként. Ez a igaz, akkor is, ha a tömb az azonos blokk Azonosítóját, illetve ugyanazokat az adatokat, mert a pillanatkép. Tiltás elkötelezett után újra, azt saját megfelelőjük bármely pillanatfelvétel eltér, és, az adatok ráterheljük. Ugyanez igaz, oldal lap blob, amely a statisztikai frissítve a.

- A blob lévő összes blokkok blokk blob cseréje hívja fel a **UploadFile**, **UploadText**, **UploadStream**vagy **UploadByteArray** módszer váltja fel. Ha, hogy a blob társított projektjeibe, összes megakadályozza az alap blob és pillanatkép most tér el, és az összes a mindkét BLOB blokkok fel. Az IGAZ, ha az alap blob- és a pillanatkép adatainak azonos marad.

- Az Azure Blob-szolgáltatás nincs olyan határozza meg, hogy a két blokkok azonos adatokat tartalmazzanak. Minden blokk feltöltött és lekötött kezelik egyedi, még akkor sem ha ugyanazokat az adatokat, és ugyanazt az blokk azonosítót. Költségek felmerülés az egyedi blokkok, mert az vegye figyelembe, amelynek további egyedi blokkok és további díjak pillanatkép eredményez blob frissíteni.

> [AZURE.NOTE] Ajánlott eljárások meghatározzák, hogy Ön kezeli-e pillanatképek gondosan további költségek elkerülésére. Azt javasoljuk, hogy Ön kezeli-e pillanatfelvételek a következő módon:

> - Hozza létre a társított blob amikor frissíti a blob, még akkor is, ha a frissíteni kívánt azonos adatokat, kivéve ha az alkalmazás tervezés igényel, hogy megmaradjanak pillanatképek pillanatképek és törlése. Törlés, majd hozza létre újra a blob pillanatképek, biztosíthatja, hogy a blob és pillanatképek tér el.

> - Blob pillanatfelvételek Ez esetben megtartják, kerülje a **UploadFile**, **UploadText**, **UploadStream**vagy **UploadByteArray** frissítése a blob hívásához. Ezeket a módszereket cserélje ki az összes blokkok a blob, az, hogy az alap blob és pillanatképek tér el mértékben. Ehelyett frissítse a blokkok lehető legkevesebb módszerekkel **PutBlock** és **PutBlockList** .


### <a name="snapshot-billing-scenarios"></a>Pillanatkép számlázási felhasználási területei


A következő helyzetleírás mutatja be, hogyan költségeket a továbbfejlesztett fájlblokkolás blob és a pillanatképek felmerülés.

1 az esetben az alap blob nem frissült után a pillanatkép származik, így csak az egyedi blokkok 1, 2 és 3 felmerülő költségeket.

![Azure tároló-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

2 az esetben az alap blob frissült, de a pillanatkép még nem. Blokkokból álló 3 frissült, és annak ellenére, hogy ugyanazokat az adatokat, és az ugyanazt az Azonosítót tartalmaz, nem ugyanaz, mint letiltani a pillanatkép 3. A fiók terheli eredményt adja, négy blokkok az.

![Azure tároló-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

3 az esetben az alap blob frissült, de a pillanatkép még nem. Blokkokból álló 3 felváltotta az alap blob 4 blokkja, de a pillanatkép továbbra is tükrözi blokk 3. A fiók terheli eredményt adja, négy blokkok az.

![Azure tároló-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

4 esetben az alap blob teljesen frissült, és az eredeti blokkok egyike sem tartalmazza. A fiók eredményt adja, az összes nyolc egyedi blokkok terheli. Ebben az esetben akkor fordulhat elő, ha **UploadFile**, **UploadText**, **UploadFromStream**vagy **UploadByteArray**, például egy frissítés módszert használja, mivel ezeket a módszereket blob-tartalom az összes cseréje.

![Azure tároló-erőforrások](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Következő lépések

További példákat Blob-tárolóhoz használ olvassa el a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob)című témakört. A minta alkalmazások letöltéséhez és indítsa el, vagy keresse meg a GitHub kódot. 
