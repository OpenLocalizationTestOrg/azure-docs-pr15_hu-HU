<properties
    pageTitle="Hozzon létre, és egy Társítások használata Blob-tárolóhoz |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, tartalmazó Blob-tárolóhoz átengedése aláírások használata létrehozását, és hogyan felhasználása őket az ügyfél-alkalmazásokból."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Access aláírások, rész 2 megosztott: Hozzon létre, és egy Társítások használata Blob-tárolóhoz

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban megismerkedett [rész 1](storage-dotnet-shared-access-signature-part-1.md) megosztott access aláírások (Társítások), és a gyakorlati tanácsok a használatuk magyarázata. 2 rész bemutatja, hogyan hozhat létre, és kattintson a Blob-tárolóhoz átengedése aláírások használata. Példák írt C#, és használja az Azure tároló ügyfél tár .NET. Az érintett esetek fenti elemeinek átengedése aláírások használata:

- A tároló átengedése aláírás létrehozása
- A blob átengedése aláírás létrehozása
- Aláírások egy tároló erőforrások kezelése tárolt hozzáférési házirend létrehozása
- A megosztott access aláírásokat az ügyfélalkalmazás tesztelése

## <a name="about-this-tutorial"></a>Ebben az oktatóanyagban kapcsolatban

Ebben az oktatóanyagban azt fogja koncentráljon a tárolók és BLOB átengedése aláírás létrehozása két console-alkalmazás létrehozásával. Az első konzol alkalmazást átengedése aláírások tároló és blob hoz létre. Ez az alkalmazás a tárhely fiók billentyűk ismerete tartalmaz. Ügyfélalkalmazás jár, második konzol alkalmazás segítségével érheti el, tároló és a az első alkalmazással létrehozott átengedése aláírások használata blob-erőforrások. Ez az alkalmazás által a megosztott access aláírások csak a hozzáférést a tároló hitelesítő és blob-erőforrások – nem rendelkezik a fiók billentyűk ismerete.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>1 rész: Átengedése aláírások konzolhoz-alkalmazás létrehozása

Először győződjön meg arról, hogy a .NET rendszerhez telepítve van az Azure tároló ügyfél tár. Telepítheti a ügyfél tár; a legfrissebb szerelvények tartalmazó [NuGet csomag](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet csomag") Ez a javasolt módszer annak biztosítására, hogy rendelkezik-e a legutóbbi hibaelhárítási lépéseket. Is letöltheti az ügyfél-tárba az [Azure SDK a .NET rendszerhez](https://azure.microsoft.com/downloads/)legújabb verziójának részeként.

A Visual Studióban hozzon létre egy új Windows konzol alkalmazást, és **GenerateSharedAccessSignatures**nevet. Adja hozzá hivatkozást **Microsoft.WindowsAzure.Configuration.dll** és **Microsoft.WindowsAzure.Storage.dll**, az alábbi eljárások valamelyikével:

-   Ha szeretne a NuGet csomag telepítéséhez, először [NuGet ügyfél](https://docs.nuget.org/consume/installing-nuget)telepítése. A Visual Studióban, jelölje be a projekt **|} NuGet csomagok kezelése**keresése online **Azure**tárolására és az utasításokat követve telepítse.
-   Azt is megteheti keresse meg a szerelvények az Azure SDK telepítésének, és adja hozzá ezeket a hivatkozásokat.

A Program.cs fájl tetején adja hozzá a következő kimutatások **használatával** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Módosítsa az app.config fájl tartalmaz, a beállítások, a kapcsolati karakterlánc, amely a tárterület-fiók mutat. A app.config fájl láthatóhoz hasonlóan kell kinéznie:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>A tároló átengedése aláírás URI készítése

Először adunk meg egy új tároló átengedése aláírás létrehozásához a módszer. Ebben az esetben az aláírás program nem társítja egy tárolt hozzáférési házirendet, az általa URI-n az információkat, jelezve a lejárati idő és ad engedélyeket.

Első lépésként vegyen fel kód hitelesíteni a tárterület-fiók elérésének, és hozzon létre egy új tároló **Main()** módszer:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Következő lépésként vegyen fel egy új módszer a megosztott access aláírást a tároló hoz létre, és az aláírás URI visszaadó:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Adja hozzá a következő sort a **Main()** módszer előtt a hívást **Console.ReadLine()** **GetContainerSasUri()** felhívását, és írja be az aláírás URI a konzolhoz ablak alján:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Állíthat össze, és futtassa az új tároló átengedése aláírás URI kimeneti. A URI az alábbi URI hasonló lesz:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

A kód futtatása után a megosztott access aláírás, amely a tároló hozta létre a lesz érvényes a következő 24 óra. Az aláírás engedélyt egy ügyfél a tárolóban és írni egy új blob a tároló lista BLOB.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Megosztott Access aláírás URI Blob készítése

Ezután azt átengedése aláírás létrehozása, és hozzon létre egy új blob a tárolóban hasonló kódot kell írni. A megosztott access aláírás nem kapcsolódik egy tárolt hozzáférési házirendet, a kezdő időpont lejárati idő és a URI jogosultsági információkat tartalmazza.

Új módszer, hogy létrehoz egy új blob, és írja be a szöveget, majd hozza létre a megosztott access aláírás, és adja vissza az aláírás URI:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

A képernyő alján az **Main()** módszer **Console.ReadLine()**, a hívás előtt **GetBlobSasUri()**, hívja a következő vonalakkal, és a megosztott access aláírás URI írni a konzol ablakban:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Állíthat össze, és futtassa az új blob-átengedése aláírás URI kimeneti. A URI az alábbi URI hasonló lesz:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>A tároló a tárolt hozzáférési házirend létrehozása

Most egy tárolt hozzáférési házirend létrehozása a határozza meg a vele társított megosztott access aláírásokkal korlátozásokkal tárolóra.

Az előző példákban azt megadott a kezdő időpont (implicit és explicit), és a lejárati idő, az engedélyek aláírás a megosztott access URI magát. Az alábbi példákban azt határozza meg ezeket a tárolt hozzáférési házirend nem pedig a megosztott access aláírást. Ha így lehetővé teszi, hogy us korlátozó módosítása nélkül a megosztott access aláírás újrakiadásának.

Akkor lehet, hogy az érvényességi tartományán aláírás a megosztott access és a tárolt hozzáférési házirendet a többi közül. Jó helyen jár csak adhatja meg a kezdési időpontot, a lejárati idő és az engedélyek egy helyre vagy más; például nem aláírás a megosztott hozzáférési engedélyek megadása és is megadhatja őket a tárolt hozzáférési házirend.

Figyelje meg, hogy amikor felvesz egy access-házirend tároló, meg kell beszerzése a tároló meglévő engedélyeket, adja hozzá az új hozzáférési házirendet, és állítsa a tároló engedélyei.

Adja hozzá az új módszer tároló hoz létre egy új tárolt hozzáférési házirendet, és a szabály nevét adja vissza:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

A képernyő alján az **Main()** módszer **Console.ReadLine()**, a hívás előtt a következő sorokat vehet fel első törlése bármelyik meglévő access házirendek, és akkor hívja meg a **CreateSharedAccessPolicy()** módszer:    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Figyelje meg, hogy az access házirendek tároló törlésekor kell először beszerzése a tároló meglévő engedélyeket, majd törölje a jelet az engedélyek, majd állítsa be újra a engedélyeket.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>A tároló, az Access-házirend használó URI átengedése aláírás létrehozása

Ezután azt létre fogja hozni egy másik megosztott access-aláírás a korábbi, de ez esetben azt fogja az aláírást társítani a hozzáférési házirendet, amely az előző példában létrehozott létrehozott tárolóra.

Adja hozzá az új módszer a tároló egy másik megosztott access aláírás létrehozásához:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

A **Main()** módszer **Console.ReadLine()**, a hívás előtt az add a következő sorokat, hívja fel a **GetContainerSasUriWithPolicy** módszer:

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>A Blob-hozzáférési házirend használó URI átengedése aláírás létrehozása

Végezetül adunk meg hasonló módon hozhat létre egy másik blob, és készítése átengedése aláírás, amely az access-házirend van társítva.

Egy új módszert, hozzon létre egy blob és készítése átengedése aláírás hozzáadása:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

A **Main()** módszer **Console.ReadLine()**, a hívás előtt az add a következő sorokat, hívja fel a **GetBlobSasUriWithPolicy** módszer:    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

A **Main()** módszert kell a következőhöz hasonlóan egészében. Aláírás létrehozása a megosztott access URL-címe a konzol ablak, majd másolja és illessze be a második részben álló ebben az oktatóanyagban használatra szövegfájl futtatásával.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

A GenerateSharedAccessSignatures New futtatásakor látni fogja a konzol ablakban az alábbihoz hasonló eredményt ad. Ezek a megosztott access aláírás, amely az oktatóprogram az 2 kell megadnia.

![társítások-konzol-eredménye-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>2 rész: Tesztelje a megosztott Access aláírások Console-alkalmazás létrehozása

A megosztott access aláírásokat az előző példában a létrehozott teszteléséhez azt létre fogja hozni egy második konzol alkalmazás az aláírások a tároló és blob műveletek elvégzéséhez.

> [AZURE.NOTE] Ha több mint 24 óráig letelik, mivel elvégezte a oktatóanyag első része, az Ön által létrehozott aláírások már nem érvényesek. Ebben az esetben az első konzol alkalmazást, a második részben álló az oktatóprogram az aláírások friss átengedése a használatra szeretne a kódot kell futtatnia.

A Visual Studióban hozzon létre egy új Windows konzol alkalmazást, és **ConsumeSharedAccessSignatures**nevet. Hivatkozások **Microsoft.WindowsAzure.Configuration.dll** és **Microsoft.WindowsAzure.Storage.dll**, adja meg a korábban elvégzett.

A Program.cs fájl tetején adja hozzá a következő kimutatások **használatával** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

A **Main()** módszer törzsében az alábbi állandók hozzáadása és frissítése az oktatóprogram 1 részében létrehozott megosztott access aláírásokat az értékeket.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Próbálja ki egy megosztott Access aláírás használatának tároló műveletek módszer hozzáadása

Ezután adunk meg egy módszer, amely azt vizsgálja, bizonyos átengedése aláírás használata a tároló képviselő tároló műveleteket. Figyelje meg, hogy a megosztott access aláírás használja a tároló hitelesítése a tároló az aláírás egyedül alapján való hozzáférés hivatkozását adja eredményül.

A következő módszerrel Program.cs felvétele:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Frissíteni felhívhatja **UseContainerSAS()** mind az átengedése aláírások a tároló létrehozott **Main()** módszer:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>A módszer próbálja meg egy megosztott Access aláírás használatának Blob-műveletek hozzáadása

Végül egy módszer, amely azt vizsgálja, néhány jellemző blob művelet átengedése aláírás használata a blob adunk meg. Ebben az esetben használjuk **CloudBlockBlob(String)**, a megosztott access aláírásban áthaladó konstruktorának a blob hivatkozását adja eredményül. Más hitelesítés nem szükséges; az aláírás egyedül alapul.

A következő módszerrel Program.cs felvétele:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Frissíteni felhívhatja **UseBlobSAS()** mind az átengedése aláírások a blob létrehozott **Main()** módszer:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Futtassa az konzol alkalmazást, és tekintse meg az a kimenet kattintva megtekintheti, hogy melyik aláírásokat kijelölésének milyen műveleteket. A kimenet konzol ablakban jelenik meg, az alábbihoz hasonló:

![társítások-konzolhoz-eredménye 2][sas-console-output-2]

## <a name="next-steps"></a>Következő lépések

[Megosztott Access aláírások, 1 rész: A Társítások modell ismertetése](storage-dotnet-shared-access-signature-part-1.md)

[Tárolók és BLOB névtelen olvasási hozzáférést kezelése](storage-manage-access-to-resources.md)

[Hozzáférés egy megosztott Access-aláírással (REST API-val) delegálása](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Táblázat és a várólista Társítások bemutatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
