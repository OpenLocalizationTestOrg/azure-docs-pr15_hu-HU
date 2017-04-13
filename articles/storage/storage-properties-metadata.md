<properties
    pageTitle="Tulajdonságok és objektumok Azure-tárolóban lévő metaadatok beolvasása és beállítása |} Microsoft Azure"
    description="Egyéni metaadatokat tárolhatnak az Azure-tárolóban lévő objektumok beállítása és a Rendszertulajdonságok beolvasásához."
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

# <a name="set-and-retrieve-properties-and-metadata"></a>Tulajdonságok és a metaadatokat beolvasása és beállítása #

## <a name="overview"></a>– Áttekintés

Objektumok támogatási Rendszertulajdonságok Azure tárhely és a felhasználó által definiált metaadatokat mellett az adatokat tartalmaznak:

*   **A Rendszertulajdonságok.** A Rendszertulajdonságok minden tárolási erőforrás található. Egy részüket olvasási vagy beállítani, míg mások csak olvasható. A magában foglalja a néhány Rendszertulajdonságok bizonyos szabványos HTTP-fejlécek felelnek meg. Az Azure tároló ügyfél tár ezek az Ön kezeli.  

*   **Felhasználó által definiált metaadatok.** Felhasználó által definiált metaadatai név – érték pár formájában egy adott erőforráshoz megadott metaadatokat. További értékeket tároló erőforráshoz; tároló használhatja a metaadat-alapú Ezek az értékek csak a saját célok szempontjából, és nem befolyásolja az erőforrás viselkedését.  

Tulajdonság és a metaadatokat értékeket tároló erőforrás beolvasása két lépésből áll. Mielőtt el tudja olvasni ezeket az értékeket, nem kell kifejezetten lehívni őket hívja fel a **FetchAttributes** módszerrel.

> [AZURE.IMPORTANT] Tulajdonság és a metaadatokat értékeket tároló erőforrás a program nem tölti fel, kivéve ha az **FetchAttributes** módszerekkel hívja. 

## <a name="setting-and-retrieving-properties"></a>Beállításával és a tulajdonságok lekérdezése

Tulajdonságértékeit adja vissza, hívja fel a **FetchAttributes** módszer blob vagy tároló kitöltéséhez a tulajdonságok, majd olvassa el az értékeket.

Tulajdonságainak beállítása a kívánt objektumra, adja meg a tulajdonság értékét, majd a **SetProperties metódus** módszer hívja.

A következő példa tároló hoz létre, és konzol ablak ír bizonyos az tulajdonság értékeit:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Beállításával és metaadatainak beolvasása

Megadhatja a metaadatok szerint egy vagy több név – érték párokká blob vagy tároló erőforrás. Metaadat-alapú beállításához a **metaadat-alapú** gyűjteményben kattintson az erőforrás neve – érték párokká hozzáadása, majd hívja fel a **SetMetadata** módszer mentése az értékeket a szolgáltatás.

> [AZURE.NOTE] A C# azonosítók elnevezési szabályai meg kell felelnie a metaadat-alapú nevét.
 
Az alábbi példa a metaadat-alapú a tároló állítja be. Egy érték van beállítva, a webhelycsoport **Add** metódussal. A többi értéke implicit kulcs/érték szintaxis használatára. Mindkét érvényesek.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Metaadat-alapú beolvasásához, hívja fel a **FetchAttributes** módszer blob vagy a **metaadatok** gyűjtemény kitöltéséhez tároló olvassa el az értékeket, majd az alábbi példában látható módon.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Lásd még:  

- [Azure tároló ügyfél dokumentumtár a .NET-hivatkozás](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [.NET-csomag Azure tároló ügyfél tárban](https://www.nuget.org/packages/WindowsAzure.Storage/) 
