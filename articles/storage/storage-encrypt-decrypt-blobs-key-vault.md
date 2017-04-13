<properties
    pageTitle="Oktatóprogram: Titkosítása és visszafejteni az Azure kulcs tárolóra használata a Microsoft Azure-tárolóban lévő BLOB |} Microsoft Azure"
    description="Ebben az oktatóanyagban végigvezeti titkosítása és visszafejteni az Azure kulcs tárolóból elemre a Microsoft Azure tárolására ügyféloldali titkosítással blob."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Oktatóprogram: Titkosítása és Azure kulcs tárolóra használata a Microsoft Azure-tárolóban lévő BLOB visszafejtése

## <a name="introduction"></a>– Bevezetés

Ebben az oktatóanyagban bemutatja, hogyan készíthet használata ügyféloldali tároló titkosítási az Azure kulcs tárolóból elemre. Végigvezeti titkosítása és visszafejtése blob egy e-technológiákkal konzol alkalmazásra.

**Becsült időt vesz igénybe:** 20 perc

Azure kulcs tárolóra kapcsolatos információ áttekintése [Mi az Azure kulcs tárolóra?](../key-vault/key-vault-whatis.md).

Azure tárolására ügyféloldali titkosítással kapcsolatos, témakör [ügyféloldali titkosítást és a Microsoft Azure-tárterületre Azure kulcs tárolóból elemre](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez az alábbiakat kell rendelkeznie:

- Azure tárterület-fiókkal
- Visual Studio 2013-at vagy újabb verzió
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Ügyféloldali titkosítás áttekintése

Azure tároló ügyféloldali titkosítás áttekintése lásd: az [ügyféloldali titkosítást és Azure kulcs tárolóból elemre a Microsoft Azure-tárterületre](storage-client-side-encryption.md)

Íme egy rövid leírást működése az ügyféloldali titkosítást:

1. Az Azure tároló ügyfél SDK a tartalom titkosítási kulcs (CEK), amely szimmetrikus egyszer használata egy kulcsot hoz létre.
2. Felhasználói adatok titkosított e CEK.
3. A CEK ezt követően (titkosított) a fő titkosítási kulcs (KEK) használatával. A KEK egy kulcs azonosítója jelöl, és egy aszimmetrikus kulcs pár vagy szimmetrikus kulcsot és is lehet helyileg kezeli vagy tárolt Azure kulcs tárolóból elemre. A tároló ügyfél magát soha nem fér hozzá a KEK. Csak a fő körbefuttatási algoritmus kulcs tárolóra által biztosított indítja el. Ügyfelek lehetősége egyéni szolgáltatók billentyű körbefuttatási/kicsomagolása Ha szeretnék.
4. A titkosított adatok majd feltöltött az Azure tárhelyszolgáltatáshoz.


## <a name="set-up-your-azure-key-vault"></a>Állítsa be az Azure kulcs tárolóból elemre
Ebben az oktatóanyagban folytatásához kell tennie az alábbi lépéseket, amely mutatja az [első lépések Azure kulcs tárolóból elemre az](../key-vault/key-vault-get-started.md)oktatóprogram:

- Hozzon létre egy fő tárolóból elemre.
- Billentyű vagy titkos hozzáadása a fő tárolóból elemre.
- Regisztráljon az alkalmazások és az Azure Active Directory.
- Engedélyezheti az alkalmazást a kulcs vagy titkos.

Jegyezze fel a ClientID és az alkalmazások és az Azure Active Directory regisztrálásakor előállított ClientSecret.

Hozzon létre mindkét kulcsot a fő tárolóból elemre. Feltételezzük az oktatóprogram hátralevő használta az alábbi nevek: ContosoKeyVault és TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Csomagok és AppSettings console-alkalmazás létrehozása

A Visual Studióban hozzon létre egy új konzolhoz alkalmazást.

A csomag Manager konzolhoz szükséges nuget csomagok hozzáadása

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Adja hozzá a App.Config AppSettings.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Adja hozzá a következő `using` kimutatások, és ellenőrizze, hogy egy System.Configuration mutató hivatkozás hozzáadása a projekthez.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>A New jogkivonat megszerezni módszer hozzáadása

A következő módszerrel használják, amelyek a fő tárolóra eléréséhez azonosítania osztályok kulcs tárolóból elemre.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Access-tárhely és a kulcs tárolóból elemre az alkalmazásban

A fő függvény adja hozzá a következő kódot.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Fő tárolóból elemre az objektum adatmodellek
>
>Ha meg szeretné érteni, hogy nincsenek-e valójában két kulcs tárolóból elemre objektum modellek lényeges fontos: egy alapuló REST API-t (KeyVault névtér), és a másik pedig egy ügyféloldali titkosításhoz mellék.

> A kulcs tárolóra ügyfél hogyan kommunikáljon a REST API-val, és JSON webes kulcsok és titkos kulcsok dolgot, amely a kulcs tárolóra szereplő két típusú értelmezésére képes.

> A kulcs tárolóra bővítmények olyan osztályok, úgy tűnik az Azure-tárolóban lévő ügyféloldali titkosítás kifejezetten létre. Tartalmazzák felületet kulcsok (IKey) és az osztályok, amely a egy kulcsot feloldó alapján. Vannak olyan IKey kell tudnia, hogy két példányait: RSAKey és SymmetricKey. Most már amikor megtörténnek egybe az dolgot, amely tartalmazza a kulcs tárolóból elemre, de az ezen a ponton független osztályok (ahol az a billentyűt, és a titkos a kulcs tárolóra ügyfél által beolvasott valósítja IKey).


## <a name="encrypt-blob-and-upload"></a>Blob titkosítása és feltöltése
Adja hozzá a következő kódot blob titkosítása, és töltse fel a tárhely Azure-fiókjába. A **ResolveKeyAsync** módszer, amelyet egy IKey adja eredményül.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Az alábbiakban az [Azure klasszikus portál](https://manage.windowsazure.com) ügyféloldali titkosítási kulcs tárolóra tárolt használatával használatával titkosított blob-képernyőképet. A **KeyId** tulajdonság értéke a URI a kulcsot, amely végpontként a KEK kulcs tárolóból elemre. A **EncryptedKey** tulajdonság tartalmazza a CEK titkosított verzióját.

![Képernyőkép a területről, titkosítási metaadatokat tartalmazó Blob-metaadat][1]

> [AZURE.NOTE] Ha megnézi az BlobEncryptionPolicy konstruktort, látni fogja, hogy képes fogadni egy kulcsot, illetve egy feloldó. Ne feledje, hogy jobbra, most nem használható a feloldó titkosítási mert jelenleg nem támogatja az alapértelmezett kulcs.



## <a name="decrypt-blob-and-download"></a>Blob visszafejtése és letöltése
Ha használja a feloldó osztályok értelme visszafejtése valójában. A használható a titkosítási kulcs azonosítója a blob a metaadat-alapú a társítva, így nem lehet beolvasni a kulcsot, és ne feledje, hogy a kulcs és blob közötti társítás indokolt. Csak akkor győződjön meg arról, hogy a kulcs marad, a kulcs tárolóból elemre.   

A titkos kulcs egy RSA kulcs megőrződik a kulcs tárolóból elemre, így a visszafejtés fordul elő, az a titkosított billentyű a blob-metaadatokat, amely tartalmazza a CEK a rendszer elküldi kulcs tárolóból elemre az visszafejtés.

Adja hozzá a következő visszafejteni az imént feltöltött blob.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Néhány névfeloldók könnyebb kulcskezelő, beleértve a más típusú: AggregateKeyResolver és CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Kulcs tárolóra titkos kulcsok használata
Mivel a titkos lényegében szimmetrikus kulcs a SymmetricKey osztály található a egy titkos használata ügyféloldali titkosítási módszer. De fent leírt módon egy titkos kulcs tárolóból elemre a nem feleltesse meg pontosan egy SymmetricKey. Néhány dolgot ismertetése:


- A kulcs egy SymmetricKey működnie kell az rögzített hossza: 128, 192, 256, 384 vagy 512 bit.
- A kulcs egy SymmetricKey kódolt Base64 kell lennie.
- Egy kulcsot tárolóból elemre titkos, amely egy SymmetricKey kell van-e egy webhelytartalom-típus "alkalmazás/nyolcas-adatfolyam" kulcs tárolóból elemre.

Íme egy példa PowerShell létrehozhatók a titkos kulcs tárolóból elemre, mint egy SymmetricKey használható.
Megjegyzés: A gyárilag kódolt, $key, értéke csak a bemutató célra. A saját kód célszerű létrehozni a következő kulcsot.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

A konzolhoz alkalmazásban a azonos hívást, mielőtt egy SymmetricKey, a titkos beolvasásához is használhatja.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

Ez azt. Fogják túlságosan nagyra értékelni!

## <a name="next-steps"></a>Következő lépések

Használata a Microsoft Azure tároló C# kapcsolatos további tudnivalókért olvassa el a [Microsoft Azure tároló ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx)című témakört.

A Blob-REST API-val kapcsolatos további tudnivalókért olvassa el a [Blob-szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)című témakört.

A Microsoft Azure tároló legújabb információkért tanulmányozza a [Microsoft Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
