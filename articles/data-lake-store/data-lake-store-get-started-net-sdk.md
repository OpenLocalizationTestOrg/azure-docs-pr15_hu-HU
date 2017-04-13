<properties
   pageTitle="Adatok tó áruházból .NET SDK segítségével alkalmazások fejlesztéséhez |} Microsoft Azure"
   description="Alkalmazások fejlesztéséhez Azure adatok tó áruházból .NET SDK használatával"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Első lépések az Azure tó adattár .NET SDK használatával

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [A PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-VAL](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [NODE.js](data-lake-store-manage-use-nodejs.md)

Megtudhatja, hogy miként használhatja az [Azure adatok tó áruházból .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) alapvető műveleteket, például létrehozásakor mappák, töltse fel, és töltse le a adatfájlok stb. Az adatok tó kapcsolatos további tudnivalókért lásd: [Azure tó adattár](data-lake-store-overview.md).

## <a name="prerequisites"></a>Előfeltételek

* **Visual Studio 2013 vagy a Skype 2015**. Az alábbi útmutatást a Visual Studio 2015 használja.

* **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

* **Azure tó adattár fiókot**. Hogyan hozhat létre egy fiókot, tanulmányozza [Azure tó adattár – első lépések](data-lake-store-get-started-portal.md)

* **Azure Active Directory-alkalmazás létrehozása**. Az Azure Active Directory-alkalmazás segítségével az Azure Active Directory tó adattár alkalmazást hitelesítést végezni. Vannak más módszerekkel hitelesítést végezni az Azure Active Directory, amelyek **végfelhasználói** vagy **szolgáltatás hitelesítés használatával**. Utasításokat és hitelesítést végezni kapcsolatos tudnivalók című témakörben talál [tó áruházzal hitelesítés használatával az Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>.NET-alkalmazás létrehozása

1. Nyissa meg a Visual Studióban, és console-alkalmazás létrehozása.

2. A **fájl** menüben kattintson az **Új**gombra, és kattintson a **Projekt**.

3. **Új projekt**írja be vagy válassza ki a következő értékeket:

  	| A tulajdonság | Érték                       |
  	|----------|-----------------------------|
  	| Kategória | Sablonok és Visual C# / Windows |
  	| Sablon | Konzol alkalmazás         |
  	| név     | CreateADLApplication        |

4. Kattintson az **OK gombra** a projekt létrehozása.

5. A Nuget csomagok hozzáadása a projekthez.

    1. Kattintson a jobb gombbal az megoldás Explorer a projekt nevét, majd kattintson a **NuGet csomagok kezelése**.
    2. A **Nuget csomag Manager** fülre győződjön meg arról, hogy **csomag forrás** **nuget.org** van állítva, és, hogy **olyan előzetes** jelölőnégyzet be van jelölve.
    3. Keresse meg és telepítse a következő NuGet csomagok:

        * `Microsoft.Azure.Management.DataLake.Store`– Ez az oktatóanyag v0.12.5 – előzetes verzió használja.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`– Ez az oktatóanyag v0.10.6 – előzetes verzió használja.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`– Ez az oktatóanyag v2.2.8 – előzetes verzió használja.

        ![Nuget forrás hozzáadása] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Azure adatok tó új fiók létrehozása")

    4. Zárja be a **Nuget csomag Manager**.

6. Nyissa meg a **Program.cs**, a meglévő kódot törlése és akkor tartalmazzák a névtér mutató hivatkozásokat adhat a következő utasításokat.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. A változók deklarálhatnak az alább látható módon, és küldje el az értékeket tó adattár nevét és az erőforrás-csoport nevét, amely már létezik. Győződjön meg arról, hogy a számítógép léteznie kell, itt helyi elérési útját és nevét. Adja hozzá a következő kódrészletet névtér nyilatkozatokat után.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

A cikk hátralévő szakaszban megjelenik az elérhető .NET módszerek használatáról a műveleteket, például a hitelesítés, a többi fájlfeltöltéshez stb.

## <a name="authentication"></a>Hitelesítés

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Ha hitelesítéssel végfelhasználói (ebben az oktatóanyagban ajánlott)

Ezzel az alkalmazással egy meglévő Azure Active Directory "Natív ügyfele"; egy megadva, az alábbi. Segít ebben az oktatóanyagban gyorsabb befejezéséhez, azt javasoljuk ezt a módszert használja.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Tudnivalók a fenti kódtöredékének pár.

* Segít az oktatóprogram gyorsabb befejezése, a kódtöredék használja az Azure AD egy tartományt, és az ügyfél-azonosító elérhető az összes Azure előfizetések alapértelmezés szerint. Ebben az esetben is **használja, a kódtöredék – az alkalmazás a**.
* Azonban használni kívánt saját Azure Active Directory tartományi és az alkalmazás ügyfél-azonosító, ha meg kell natív Azure AD-alkalmazás létrehozása az Azure Active Directory tartománynév, az ügyfél-azonosító és URI átirányítása az alkalmazáshoz létrehozott korábbi. [Az Active Directory-alkalmazások létrehozása](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) talál utasításokat.

>[AZURE.NOTE] A fenti hivatkozások utasításait vannak az Azure Active Directory webes alkalmazáshoz. Jó helyen jár a lépések megegyeznek pontosan még akkor is, ha azt választotta, hogy inkább natív ügyfele-alkalmazás létrehozása. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Ha a ügyfél titkos használja, a szolgáltatás hitelesítés 

A következő kódtöredékének használható az alkalmazás nem interaktívan, hitelesíteni a ügyfélprogramban titkos / kulcs az-alkalmazások és szolgáltatások egyszerű. Ezzel az [Azure Active Directory "webalkalmazás" alkalmazás](../resource-group-create-service-principal-portal.md)meglévő.

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Szolgáltatás hitelesítési tanúsítvány használatakor

Beállítás egy harmadik, mint az alábbi kódtöredékének használható az alkalmazás nem interaktívan, hitelesítő-alkalmazások és szolgáltatások egyszerű a tanúsítvány használatával. Ezzel az [Azure Active Directory "webalkalmazás" alkalmazás](../resource-group-create-service-principal-portal.md)meglévő.

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Ügyfél-objektumok létrehozása

A következő kódtöredékének a tó adattár fiók és a fájlrendszer ügyfél objektumok kibocsátása kérelmek a szolgáltatás használt hoz létre.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>A lista minden tó adattár fiók előfizetés belül

A következő kódtöredékének adott Azure előfizetés belül minden tó adattár fiók sorolja fel.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Könyvtár létrehozása

Az alábbi kódtöredékének látható egy `CreateDirectory` módszer címtár belül egy tó adattár fiók létrehozásához használható.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Fájl feltöltése

Az alábbi kódtöredékének látható egy `UploadFile` módszer, amely a fájlok feltöltése egy tó adattár fiókot is használhatja.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`képes rekurzív feltöltése és letöltése helyi fájl elérési utat és egy tó adattár fájl elérési útja között.    

## <a name="get-file-or-directory-info"></a>Fájl vagy könyvtár Infó

Az alábbi kódtöredékének látható egy `GetItemInfo` módszer, fájlt vagy adatok tó áruházból elérhető könyvtárat adatainak beolvasásához használható. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Lista-fájl vagy könyvtárak

Az alábbi kódtöredékének látható egy `ListItem` módszert, amelyekkel a fájl- és egy tó adattár fiókban könyvtárak listája.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>ÖSSZEFŰZ fájlok

Az alábbi kódtöredékének látható egy `ConcatenateFiles` való összefűzésére fájlok használt módszert. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Lehetőség van a fájl

Az alábbi kódtöredékének látható egy `AppendToFile` használt módszer adatok hozzáfűzése egy tó adattár fiókkal már tárolt fájlt.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Egy fájl letöltése

Az alábbi kódtöredékének látható egy `DownloadFile` tó adattár fiókból egy fájl letöltésére használt módszert.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Következő lépések

- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Tó adattár .NET SDK hivatkozás](https://msdn.microsoft.com/library/mt581387.aspx)
- [Tó adattár többi hivatkozás](https://msdn.microsoft.com/library/mt693424.aspx)
