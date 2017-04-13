<properties
    pageTitle="A helyszíni blob-tárolóhoz (Java) alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre egy konzolhoz alkalmazás Azure feltölti képet, és kattintson a böngészőben jeleníti meg a képet. Java-ban, mintakódok."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>A helyszíni alkalmazással blob-tárolóhoz

## <a name="overview"></a>– Áttekintés

A következő példa bemutatja, hogyan használhatja Azure tároló képeket tárolja az Azure-ban. Ez a cikk a kód van konzol alkalmazáshoz Azure feltölti képet, és ekkor létrehoz egy HTML-fájl, amely a böngészőben jeleníti meg a képet.

## <a name="prerequisites"></a>Előfeltételek

- Egy Java Developer Kit (JDK), 1,6 vagy újabb verziója van telepítve.
- Az Azure SDK telepítve van.
- Az Azure-tárak Java a üveg, minden esetben függőség kancsó telepített és az összeállítás elérési út a Java fordító által használt. Az Azure-tárak Java telepítésével kapcsolatos további tudnivalókért lásd [a Java Azure SDK letöltése](java-download-azure-sdk.md).
- Azure tárterület-fiók beállítása. A fiók nevét, és a fiókkulcs a tárterület-fiókom a kód, a jelen cikkben lesz. Megtudhatja, [hogy miként tároló fiók létrehozása](storage-create-storage-account.md#create-a-storage-account) a fiókkulcs beolvasása információt a tárterület-fiókot, és [nézet és a Másolás tároló hívóbetűk](storage-create-storage-account.md#view-and-copy-storage-access-keys) létrehozásáról további információt.

- Létrehozott egy helyi kép nevű fájlt tárolja az elérési út c:\\myimages\\image1.jpg. Azt is megteheti módosítsa a   **FileInputStream** konstruktor a példában egy másik kép elérési utat és nevet szeretne.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Fájl feltöltése Azure blob-tárolóhoz használatával

Lépésenkénti Itt jelennek meg. Ha azt szeretné, akkor a következő kimarad, a teljes kód megjelenik ez a cikk későbbi részében.

Többek között az import az Azure core tároló osztályok, az Azure blob-ügyfél osztályok, a Java IO osztályok és a **URISyntaxException** osztály első lépésként a kódot.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

**StorageSample**nevű osztály deklarálhatnak, és a nyitó zárójel, **{**tartalmazza.

    public class StorageSample {

A **StorageSample** osztály belül egy alapértelmezett végpont protokollt, a tárhely fióknév és a tárhely hívóbetű Azure tároló fiókban meghatározott tartalmazó karakterlánc típusú változóban deklarálhatnak. Cserélje ki a helyőrző **a\_fiók\_neve** és **a\_fiók\_kulcs** saját fiók nevét és a fiók billentyűt, illetve.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

A **fő**a Rekorddeklaráció helyezhet, egy **próbálja** szövegrészt tartalmaz, és tartalmazzák a szükséges **{**megnyitott szögletes.

    public static void main(String[] args)
    {
        try
        {

A következő típusú (a leírásokat is használatuk ebben a példában a) változók deklarálhatnak:

-   **CloudStorageAccount**: a fiók objektum Azure tároló fióknév és kulcs inicializálni, illetve a blob ügyfél-objektum létrehozása.
-   **CloudBlobClient**: a blob-szolgáltatás eléréséhez használt.
-   **CloudBlobContainer**: blob-tároló létrehozásához használt, a listában a tárolóban a BLOB, és törölje a tároló.
-   **CloudBlockBlob**: segítségével a helyi kép fájl feltöltése a tároló.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

A **fiók** változó értéket rendeli.

    account = CloudStorageAccount.parse(storageConnectionString);

A **serviceClient** változó értéket rendeli.

    serviceClient = account.createCloudBlobClient();

A **tároló** változó értéket rendeli. Szerezze be azt a tároló **gettingstarted**nevű hivatkozást.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Hozza létre a tárolót. Ez a módszer a tároló hoz létre, ha ez nem léteznek (és a **Igaz értéket**ad vissza). Ha a tároló létezik, **FALSE értéket**ad vissza. **CreateIfNotExists** helyett a lehetőség **létrehozása** (amely hibát ad vissza, ha már létezik a tároló).

    container.createIfNotExists();

Név nélküli hozzáférés beállítása a tároló.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Ismerkedés a továbbfejlesztett fájlblokkolás blob, amely amelye az Azure-tárolóban lévő blob mutató hivatkozás.

    blob = container.getBlockBlobReference("image1.jpg");

A **fájl** konstruktor segítségével olyan, amely akkor feltöltődnek helyileg létrehozott fájlra mutató hivatkozást. Gondoskodjon arról, hogy a kód futtatása előtt létrehozott ezt a fájlt.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Töltse fel a **CloudBlockBlob.upload** módszerrel hívás – a helyi fájl. Az első a **CloudBlockBlob.upload** módszerrel paraméter egy **FileInputStream** -objektum, amely az Azure-tárolóhoz fel kell tölteni a helyi fájl. A második paraméter bájtos, a fájl mérete.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Névvel ellátott **MakeHTMLPage**, hogy egy egyszerű HTML-lapot tartalmazó segítő függvény egy ** &lt;kép&gt; ** a forrás elem az blob, amely már a Azure tárterület-fiókját beállítva. Ez a cikk későbbi részében **MakeHTMLPage** kódját tárgyalja.

    MakeHTMLPage(container);

Nyomtassa ki egy állapotüzenetet és információt a létrehozott HTML-lapra.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Zárja be a **próbálkozzon** blokk illesszen be egy záró zárójel: **}**

Kezelheti a következő kivételekkel:

-   **FileNotFoundException**: a **FileInputStream** vagy **FileOutputStream** konstruktorok által is lehet elő.
-   **StorageException**: az Azure ügyfél tároló Library is lehet elő.
-   **URISyntaxException**: **ListBlobItem.getUri** módszerrel is lehet elő.
-   **Kivétel**: általános kezelésének módját.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Zárja be a **fő** illesszen be egy záró zárójel: **}**

Hozzon létre **MakeHTMLPage** hozhat létre egyszerű HTML lap nevű függvény. Ez a módszer paramétere típusú **CloudBlobContainer**, a feltöltött BLOB listáját Végigléptetés használt. Ez a módszer kivételek típusú **FileNotFoundException**, amely is lehet elő fog throw **FileOutputStream** konstruktor, és **URISyntaxException**, amely is lehet elő **ListBlobItem.getUri** módszer szerint. A nyitó zárójel, **{**tartalmazza.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Hozzon létre egy helyi nevű **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

A helyi fájl hozzáadása a írni a ** &lt;html&gt;**, ** &lt;élőfej&gt;**, és ** &lt;törzsébe&gt; ** elemeket.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Végigléptetés feltöltött BLOB listáját. Az egyes blob, a HTML-lap hozzon létre egy ** &lt;img&gt; ** elemet, amelyet a küldött a URI a blob-tároló Azure-fiókjában található **src** attribútummal rendelkezik.
Bár a felvett csak egy képet a következő példában, ha több fel kód találta szeretné az összeset.

Az egyszerűség kedvéért ez a példa feltételezi minden feltöltött blob képet. Ha a rendszer az eltérő képek BLOB vagy blokk BLOB helyett lap BLOB, igény szerint módosítsa a kódot.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Bezárás a ** &lt;szervezet&gt; ** elem és a ** &lt;html&gt; ** elemet.

    stream.println("</body>");
    stream.println("</html>");

Zárja be a helyi fájl.

    stream.close();

Zárja be a **MakeHTMLPage** illesszen be egy záró zárójel: **}**

Zárja be a **StorageSample** illesszen be egy záró zárójel: **}**

Az alábbiakban ebben a példában a teljes kódját. Ne felejtse el módosítani a helyőrző értékeket **a\_fiók\_neve** és **a\_fiók\_kulcs** a fiók nevét, és a fiókkulcs, illetve használni.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Mellett a helyi kép feltöltést Azure tárhely, a példa kód egy helyi fájl namedindex.html, lásd: a feltöltött képe a böngésző megnyithat hoz létre.

Mivel a kódot a fiók nevét, és a fiókkulcs tartalmaz, győződjön meg arról, hogy a forráskód biztonságos.

## <a name="to-delete-a-container"></a>A tároló törlése

Az-előfizetést terhelő tárhely, mivel előfordulhat, hogy törölni szeretné a **gettingstarted** tároló után végzett ebben a példában kísérletezzen. A tároló törléséhez használja a **CloudBlobContainer.delete** módszert.

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Hívja fel a **CloudBlobContainer.delete** módszer, a folyamat **CloudStorageAccount**, **ClodBlobClient**és **CloudBlobContainer** objektumok inicializálása megegyezik **createIfNotExist** mód látható módon. Egy kész példa **gettingstarted**tároló törlésére a következő:

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Más blob-tároló osztályok és módszerek áttekintése megtudhatja, [hogy miként használhatja a Java Blob-tárolóhoz](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Következő lépések

Kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az összetettebb tároló tevékenységek.

- [Azure tároló Java SDK](https://github.com/azure/azure-storage-java)
- [Azure tároló ügyfél SDK hivatkozás](http://dl.windowsazure.com/storage/javadoc/)
- [Azure tárolása REST API-val](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)
