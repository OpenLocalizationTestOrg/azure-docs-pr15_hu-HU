<properties
 pageTitle="Azure tároló blob-tartalom az Azure CDN elévülési kezelése |} Microsoft Azure"
 description="Tudjon meg többet a time-to-live biztonságkezelési az Azure CDN-gyorsítótárazás BLOB-beállításokat."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Tárterület Azure blob tartalom az Azure CDN lejárati kezelése

> [AZURE.SELECTOR]
- [Azure Web Apps alkalmazások/Cloud Services, az ASP.NET vagy az IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure blob tárhelyszolgáltatáshoz](cdn-manage-expiration-of-blob-content.md)

[Tároló Azure](../storage/storage-introduction.md) [blob-szolgáltatás](../storage/storage-introduction.md#blob-storage) az egyik Azure CDN integrálódik több Azure-alapú forrásokból.  Nyilvánosan hozzáférhető blob-tartalmakat gyorsítótárba helyezhető az Azure CDN mindaddig, amíg a program a time-to-live (TTL).  A TTL (élettartam) a [ *Gyorsítótár-vezérlők* élőfej](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) a HTTP-válasz Azure tárhelyről határozza meg.

>[AZURE.TIP] Előfordulhat, hogy nincs TTL (élettartam) beállítása a blob választhatja.  Ebben az esetben a Azure CDN automatikusan elhelyezi a TTL (élettartam), a hét napig alapértelmezett.
>
>Azure CDN BLOB-és más fájlok access gyorsítására működésével kapcsolatos további tudnivalókért olvassa el a az [Azure CDN – áttekintés](./cdn-overview.md)című témakört.
>
>Kattintson az Azure blob tárhelyszolgáltatáshoz, lásd: [Blob szolgáltatás – fogalmak](https://msdn.microsoft.com/library/dd179376.aspx). 

Ebben az oktatóanyagban jelenítheti meg a TTL (élettartam) Azure-tárolóban lévő blob többféleképpen mutatja be.  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md) egyike, a Azure szolgáltatások felügyelete leggyorsabb és leghatékonyabb módjai.  Használja a `Get-AzureStorageBlob` parancsmag hivatkozás a blob, majd állítsa a `.ICloudBlob.Properties.CacheControl` tulajdonság. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] A PowerShell használatával [a CDN profilok kezelése és végpontok pontra](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Azure tároló ügyfél tár a .NET rendszerhez

.NET használata egy blob TTL beállításához használja a [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) tulajdonság beállítása az [Azure tároló ügyfél tár a .NET rendszerhez](../storage/storage-dotnet-how-to-use-blobs.md) .

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Számos további .NET mintakódok áll rendelkezésre az [Azure Blob-tároló minták a .NET rendszerhez](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).

## <a name="other-methods"></a>Más módszerek

- [Azure parancssor](../xplat-cli-install.md)

    A blob feltöltésekor használatával *cacheControl* tulajdonság beállítása a `-p` válthat.  Ez a példa egy óra (3600 másodpercet) állítja be a TTL (élettartam).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure tárolása REST API-val](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Az *x-ms-blob-gyorsítótár-vezérlők* tulajdonság explicit módon beállított a [Blob elhelyezni](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Blokkokból álló lista helyezi el](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)vagy [Blob tulajdonságainak beállítása](https://msdn.microsoft.com/library/azure/ee691966.aspx) kérésre.

- Harmadik fél tárterület-kezelő eszközök

    Egyes külső Azure tárterület-kezelő eszközök a *CacheControl* tulajdonság a BLOB teszi lehetővé. 

## <a name="testing-the-cache-control-header"></a>A *Gyorsítótár-vezérlők* fejléc tesztelése

A TTL (élettartam): a BLOB egyszerűen ellenőrizheti.  [Fejlesztőeszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)a böngésző használata esetén ellenőrizze, hogy a blob van-e a *Gyorsítótár-vezérlők* válasz fejlécekkel együtt.  **Wget**, [Postman](https://www.getpostman.com/)és [Fiddler](http://www.telerik.com/fiddler) eszköz segítségével vizsgálja meg a választ fejlécek.

## <a name="next-steps"></a>Következő lépések

- [További információ: a *Gyorsítótár-vezérlők* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Megtudhatja, hogy miként lejárati Azure CDN Felhőszolgáltatásba tartalmának kezelése](./cdn-manage-expiration-of-cloud-service-content.md)

