<properties
    pageTitle="Tárolók és BLOB névtelen olvasási hozzáférést kezelése |} Microsoft Azure"
    description="Megtudhatja, hogyan tárolók és elérhetővé BLOB-név nélküli hozzáférés, és hogyan érheti el őket programozás útján."
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

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Tárolók és BLOB névtelen olvasási hozzáférést kezelése

## <a name="overview"></a>– Áttekintés

Alapértelmezés szerint a tárterület-fiókot csak a tulajdonos belül, a fiók tároló-erőforrások is hozzáférhet. A csak Blob-tárolóhoz beállíthatja, hogy egy tároló engedélyei lehetővé teszi a tároló és annak a BLOB névtelen olvasási hozzáférést, hogy a fiókkulcs megosztása nélkül adhat hozzá az erőforrásokhoz.

Név nélküli hozzáférés ideális alkalmazási helyzetek, amelyhez bármikor elérhetővé szeretné tenni a névtelen olvasási hozzáférést bizonyos BLOB. Eltérés finomabb figyelembevételével vezérlő átengedése aláírás, amely lehetővé teszi, korlátozott meghatalmazás használatával különböző engedélyeket és megadott időközönként keresztül is létrehozhat. Megosztott access aláírások létrehozásával kapcsolatos további tudnivalókért lásd: [Segítségével a megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Tárolók és BLOB névtelen felhasználók engedélyek megadása

Alapértelmezés szerint tároló és a benne minden BLOB is elérhető csak a tulajdonos, a tárterület-fiók. Névtelen felhasználók számára, olvassa el a BLOB-tároló engedélyei adhat, állíthat be nyilvános hozzáférés engedélyezése a tároló engedélyeket. Névtelen felhasználók olvashatják a nyilvánosan hozzáférhető tárolóban lévő BLOB hitelesítése a kérelem nélkül.

Tárolók adja meg a tároló hozzáférés kezelése a következő beállításokat:

- **Teljes nyilvános olvasási hozzáférést:** Tárolók és blob-adatok névtelen elküldött kérésre is olvashatók. Ügyfelek BLOB névtelen elküldött kérésre tárolóban is számbavétele, de nem számba veheti a tárhely számla belüli tárolók.

- **Nyilvános, olvassa el az access csak BLOB:** Névtelen elküldött kérésre BLOB-adatok a tárolóban is olvashatók, de tároló adatok nem érhető el. Ügyfelek nem számba veheti a névtelen elküldött kérésre tárolóban BLOB.

- **Nincs nyilvános, olvassa el az access:** Csak a fiók tulajdonos tároló és blob adatokat is olvashatók.

Beállíthatja, hogy a tároló engedélyei az alábbi módon:

- Az [Azure-portálon](https://portal.azure.com).
- Programozás útján, a tárhely ügyfél tárat vagy használatával a REST API-t.
- Megismerkedhet a PowerShell használatával. Tároló engedélyeinek beállítása az Azure PowerShell kapcsolatos további tudnivalókért olvassa el az [Azure PowerShell használatá Azure adathordozós](storage-powershell-guide-full.md#how-to-manage-azure-blobs)című témakört.

### <a name="setting-container-permissions-from-the-azure-portal"></a>A tároló engedélyeinek beállítása az Azure portálról

Az [Azure-portálon](https://portal.azure.com)tároló engedélyeinek beállításához kövesse az alábbi lépéseket:

1. Nyissa meg az irányítópulton a tárterület-fiókjához.
2. Válassza ki a tárolóhoz nevét a listában. A nevére kattintva teszi lehetővé a kiválasztott tárolóban a BLOB
3. Jelölje ki a **hozzáférési házirendet** az eszköztáron.
4. A **hozzáférési típusa** mezőben válassza ki a kívánt az engedélyek az alábbi képernyőképen látható módon.

    ![A tároló metaadatok párbeszédpanel szerkesztése](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>.NET programozás útján használatával tároló engedélyeinek beállítása

A .NET-ügyfél tár használatával tároló engedélyeinek beállításához először beolvashatja a tároló meglévő engedélyeket hívja fel a **GetPermissions** módszerrel. Kattintson a tulajdonság **PublicAccess** az **BlobContainerPermissions** objektum, a függvény által visszaadott **GetPermissions** módszer szerint. Végül hívja fel a frissített engedélyekkel rendelkező **SetPermissions** módszer.

Az alábbi példa a teljes körű nyilvános olvasási hozzáférést szeretne a tároló engedélyeket állít be. Engedélyek beállítása nyilvános olvasási hozzáférést BLOB-csak a tulajdonsága **PublicAccess** **BlobContainerPublicAccessType.Blob**. A névtelen felhasználók minden engedélye eltávolításához a tulajdonsága **BlobContainerPublicAccessType.Off**.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Tárolók és BLOB névtelenül elérése

Egy ügyfél hozzáférő tárolók és BLOB névtelenül használható hitelesítő adatokat nem igénylő konstruktorok. Az alábbi példák bemutatják többféleképpen Blob-szolgáltatás erőforrások névtelenül hivatkozni szeretne.

### <a name="create-an-anonymous-client-object"></a>Név nélküli ügyfél-objektum létrehozása

Név nélküli hozzáférés az új szolgáltatás ügyfél objektum hozhat létre, mert a Blob-szolgáltatás végpontjának a fiókhoz. Azonban is ismernie kell a tároló az adott fiókban elérhető a névtelen hozzáférés nevét.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>A tároló névtelenül hivatkozás

Ha az URL-cím névtelenül elérhető tárolóhoz, használhatja a tároló közvetlenül hivatkozni szeretne.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Referencia a blob névtelenül

Ha elérhető a névtelen hozzáférés blob URL-CÍMÉT, hivatkozhat a blob közvetlenül az, hogy az URL-cím használatával:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Névtelen felhasználók számára elérhető szolgáltatások

Az alábbi táblázat bemutatja, hogy milyen műveleteket meghívhatja névtelen felhasználók nyilvános elérésének beállítása egy tároló vezérlés esetén.

| TÖBBI művelet                                         | Teljes körű nyilvános olvasási hozzáférést engedély | Nyilvános olvasási hozzáférést csak BLOB-engedélyek |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Tárolók lista                                        | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| A tároló létrehozása                                       | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| A tároló tulajdonságainak lekérése                               | Az összes                                     | Csak a tulajdonos                                        |
| A tároló metaadatok beszerzése                                 | Az összes                                     | Csak a tulajdonos                                        |
| A tároló metaadatok beállítása                                 | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Tároló vezérlés beszerzése                                      | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Tároló vezérlés beállítása                                      | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Tároló törlése                                       | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Lista BLOB                                             | Az összes                                     | Csak a tulajdonos                                        |
| Helyezze a Blob                                               | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Blob beszerzése                                               | Az összes                                     | Az összes                                               |
| Első Blob-tulajdonságok                                    | Az összes                                     | Az összes                                               |
| Blob-tulajdonságainak beállítása                                    | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Első Blob-metaadat                                      | Az összes                                     | Az összes                                               |
| Blob-metaadatok beállítása                                      | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Továbbfejlesztett fájlblokkolás helyezése                                              | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Blokkokból álló lista (csak lekötött blokkolja) beszerzése                 | Az összes                                     | Az összes                                               |
| (Csak a véglegesített blokkok vagy az összes adatblokkok) blokkokból álló lista beolvasása | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Blokkokból álló lista helyezése                                         | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Blob törlése                                            | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Blob másolása                                              | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Pillanatkép Blob                                          | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Bérleti Blob                                             | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Helyezze a lapon                                               | Csak a tulajdonos                              | Csak a tulajdonos                                        |
| Első oldal tartományok                                        | Az összes                                     | Az összes                                                  |
| Hozzáfűző Blob                                            | Csak a tulajdonos                              | Csak a tulajdonos                                                  |


## <a name="see-also"></a>Lásd még:

- [Az Azure tároló szolgáltatások hitelesítés](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Megosztott Access aláírások (Társítások) használata](storage-dotnet-shared-access-signature-part-1.md)
- [Hozzáférés egy megosztott Access-aláírással delegálása](https://msdn.microsoft.com/library/azure/ee395415.aspx)
