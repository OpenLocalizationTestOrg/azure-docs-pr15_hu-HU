<properties
    pageTitle="Azure tároló használata a Windows áruház-alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure Blob, várólista, táblázat vagy fájlt tároló használó Windows áruház-alkalmazás létrehozása céljából."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Azure-tárhely Windows Áruházbeli alkalmazások használatáról

## <a name="overview"></a>– Áttekintés

Ez az útmutató szemlélteti, hogyan veheti használatba elkészítésének alkalmazás a Windows áruházból, használja az Azure-tárterületét.

## <a name="download-required-tools"></a>Töltse le a szükséges eszközök

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) egyszerűen létre, hibakeresési, localize a csomagot, és Windows áruház-alkalmazás telepítéséhez. Visual Studio 2012 vagy újabb verziója szükség.
- Az [Azure tároló ügyfél tárban](https://www.nuget.org/packages/WindowsAzure.Storage) a Windows Runtime osztálytár nyújt a Azure tároló használata.
- [WCF adatok szolgáltatások eszközök a Windows áruházból alkalmazásokat](http://www.microsoft.com/download/details.aspx?id=30714) bővíti a szolgáltatási hivatkozás hozzáadása a yammert az ügyféloldali OData támogatási Visual Studio Windows áruház alkalmazásait tartalmazza.

## <a name="develop-apps"></a>Alkalmazások fejlesztése

### <a name="getting-ready"></a>Felkészülés

Új Windows Áruházbeli alkalmazás projekt létrehozása a Visual Studio 2012 vagy újabb verziójában:

![tár-alkalmazások – tárterület-viewben-projekt][store-apps-storage-vs-project]

Ezután hozzáadása az Azure-tároló ügyfél tárakat a jobb gombbal a **hivatkozásokat**, **Hivatkozás hozzáadása**gombra kattintva, majd tallózással a tárhely ügyfél tárban a Windows Runtime letöltött:

![tár-alkalmazások – tárterület-válassza-tár][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>A tár használata a Blob- és várólista szolgáltatások

Ezen a ponton az alkalmazás készen áll a hívja fel az Azure Blob és várólista szolgáltatást. A következő **használata** kimutatásokban adja meg, hogy az Azure tárolási típusok közvetlenül hivatkozhat:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Ezután gomb felvétele a lapra. A következő kód hozzáadása a **kattintás** eseményhez, és módosítsa a eseménykezelő metódus az [aszinkron kulcsszó](http://msdn.microsoft.com/library/vstudio/hh156513.aspx)megadásával:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Ez a kód feltételezi, hogy rendelkezik-e két karakterlánc változók, a *fióknév* és a *accountKey*. Tárterület-fiókja és a fiókhoz társított fiókkulcs képviselik.

Építse fel, és futtassa az alkalmazást. A gombra kattintva ellenőrizze, hogy van-e *container1* nevű tároló fiókban, és ezután hozzon létre, ha nem.

### <a name="using-the-library-with-the-table-service"></a>A táblázat szolgáltatás a tár használata

Típus, amellyel kommunikálni az Azure-táblából szolgáltatással a Windows áruház-alkalmazás tár WCF Data Services függ. Ezután hozzáadása a szükséges WCF-tárak a csomag kezelője konzol használatával:

![tár-alkalmazások-tároló-csomag-kezelése][store-apps-storage-package-manager]

Pont csomag Manager a következő parancsot használja azt a helyet a számítógépen:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Ez a parancs automatikusan hozzáadja a projekthez minden szükséges hivatkozást. Ha nem szeretné, hogy a csomag Manager konzolról, a helyi számítógépre a WCF-adatok szolgáltatások NuGet mappa hozzáadása a csomag adatforrások listáját, és majd [Kezelése NuGet csomagok az párbeszédpanelen](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)ismertetett módon adja hozzá a felhasználói felület és a hivatkozást.

Ha a WCF-adatok szolgáltatások NuGet csomag van hivatkozik, a kód az gombra **kattintás** eseményhez módosítása

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Ez a kód ellenőrzi, hogy *TÁBLÁZAT1* nevű táblázat szerepel-e a fiók, és ezután hoz létre, ha nem.

Felveheti Microsoft.WindowsAzure.Storage.Table.dll, hivatkozást is, amely olyan letöltött csomagban érhető el. A médiatárban további szolgáltatások, például általános lekérdezések és tükröződés-alapú szerializálási. Figyelje meg, hogy a tárban nem támogatja a JavaScript.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
