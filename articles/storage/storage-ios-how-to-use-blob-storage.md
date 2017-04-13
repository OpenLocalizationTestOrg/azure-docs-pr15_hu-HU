<properties
    pageTitle="Az iOS Azure blobtárolóhoz használata |} Microsoft Azure"
    description="Strukturálatlan adatokat tárolja az Azure Blob-tárolóhoz (objektum tároló) a felhőben."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Az iOS Blob-tárolóhoz használata

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan végrehajtásához használja a Microsoft Azure Blob-tárolóhoz esetei. A minták cél-C írt, és a [Azure tároló ügyfél tár IOS](https://github.com/Azure/azure-storage-ios). Az érintett esetek **feltöltése**, **listaelem**, **letöltése**és **törlése** BLOB. BLOB a további tudnivalókért lásd: a [Következő lépésekkel](#next-steps) szakaszban. Letöltheti a [minta alkalmazás](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) gyorsan megtekintheti az Azure-tárhely használatának az iOS-alkalmazás is.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Az alkalmazás az Azure tároló iOS tár importálása

Lehetősége van importálni az Azure tároló iOS tár az alkalmazás az [Azure tároló CocoaPod](https://cocoapods.org/pods/AZSClient) használatával, vagy a **keret** fájl importálásával.

## <a name="cocoapod"></a>CocoaPod

1. Ha azt még nem tette, [Telepítse CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) Terminálszolgáltatások ablak megnyitása és a következő parancsot futtató számítógépen

        sudo gem install cocoapods

2. Következő, a project címtárban (a címtár-tartalmazó a `.xcodeproj` fájl), hozzon létre egy új fájlt nevű `Podfile`(fájl kiterjesztés nélkül). Az alábbi módon hozzáadása `Podfile` és mentése

        pod 'AZSClient'

3. A Terminálszolgáltatások ablakban nyissa meg azt a projekt könyvtárában, és futtassa a következő parancsot

        pod install

4. Ha a `.xcodeproj` , nyissa meg a Xcode, és zárja be. A projekt címtárban nyissa meg az újonnan létrehozott projektfájl fog rendelkezni, amely a `.xcworkspace` bővítmény. Ez az a fogja dolgozik a most a fájlt.

## <a name="framework"></a>Keret
Az Azure tároló iOS tár használatához, akkor először a keret fájl létrehozásához.

1. Először töltse le vagy az [azure-tárterület-ios repó](https://github.com/azure/azure-storage-ios)klónozhatja.

2. Válassza az *azure-tárterület-ios* -> *tár* -> *Azure tároló ügyfél tár*és Megnyitás `AZSClient.xcodeproj` a Xcode.

3. Módosítania kell a bal felső részén Xcode, a "Keretrendszer" tárból"Azure tároló ügyfél" aktív séma.

4. Építse fel a projektet (⌘ + B). Ez a művelet létrehoz egy `AZSClient.framework` fájl az asztalon.

Ezután importálhatja a keret fájlt az alkalmazás az alábbiak szerint:

1. Új projekt létrehozása, vagy nyissa meg a Xcode meglévő projektjére.

2. Kattintson a bal oldali navigációs sávon a projekt, és kattintson az *Általános* fülre, a project-szerkesztő tetején.

3. A *csatolt keretek és a tárak* csoportban kattintson a Hozzáadás (+) gombra.

4. Kattintson az *egyéb... hozzáadása*. Nyissa meg azt, és adja hozzá a `AZSClient.framework` újonnan létrehozott fájl.

5. A *csatolt keretek és a tárak* csoportban kattintson ismét a Hozzáadás (+) gombra.

6. A tárak, már megadott listáját, keressen rá a `libxml2.2.dylib` , és adja hozzá a projekthez.

7. Kattintson a projekt-szerkesztő tetején a *Beállítások összeállítása* fülre.

8. A *Keresési javaslatok* csoportban kattintson duplán a *Keret keresési javaslatok* , és adja hozzá az elérési útját a `AZSClient.framework` fájlt.

## <a name="import-statement"></a>Importálás utasítás
Szüksége lesz az alábbi importálása utasítás szerepeltetni a fájlt, amelyre fel szeretné elindítani az Azure tároló API-val.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Aszinkron műveletek
> [AZURE.NOTE] Aszinkron műveletek, amelyek minden módszer, hogy a szolgáltatás ellen egy kérés végrehajtása. A mintakódok feladatnál találja meg, ezeket a módszereket rendelkezik-e egy teljesítése kezelő. Kód, a befejezési kezelő belül futnak **után** a kérést, nincs befejezve. A befejezési kezelő fog futni **közben** a kérelem utáni kódot történik.

## <a name="create-a-container"></a>A tároló létrehozása
Azure-tárolóban lévő minden blob tárolóban kell lennie. A következő példa bemutatja, hogyan hozhat létre egy tároló nevű *newcontainer*, ha még nem létezik a tárterület-fiókjában. A tároló nevét kiválasztásakor legyen tudatában annak, hogy az imént említett lekérdezéstípusokról elnevezési szabályokat.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Győződhet meg, hogy ez hogyan működik a [Microsoft Azure tároló Explorer](http://storageexplorer.com) megjeleníti, és ellenőrzi, hogy *newcontainer* megtalálható a listában, a tárolók a tárterület-fiók.

## <a name="set-container-permissions"></a>A tároló engedélyeinek beállítása
A tároló engedélyei konfigurálták **magánjellegű** az access alapértelmezés szerint. Tárolók azonban felkínálja, hogy a container hozzáférési néhány választási lehetősége:

- **Magánjellegű**: adatok tároló és blob használatával fióktulajdonos csak olvasható.

- **BLOB**: névtelen elküldött kérésre Blob-adatok a tárolóban is olvashatók, de tároló adatok nem érhető el. Ügyfelek nem számba veheti a névtelen elküldött kérésre tárolóban BLOB.

- **Tároló**: adatok tároló és blob névtelen elküldött kérésre is olvashatók. Ügyfelek BLOB névtelen elküldött kérésre tárolóban is számbavétele, de nem számba veheti a tárhely számla belüli tárolók.

A következő példa bemutatja, hogyan készíthet tároló **Container** hozzáférési engedélyek, amely nyilvános, csak olvasási hozzáférést az interneten minden felhasználó számára lehetővé:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni
[Blob-szolgáltatási fogalmak](#blob-service-concepts) részben említett Blob-tárolóhoz felajánlja a különféle BLOB három különböző típusú: BLOB blokkolása BLOB hozzáfűző és BLOB lap. Ez jelenleg az Azure tároló iOS tár csak blokk BLOB használatát támogatja. Az esetek többségében a továbbfejlesztett fájlblokkolás blob a használata ajánlott típusa.

A következő példa bemutatja egy NSString a továbbfejlesztett fájlblokkolás blob feltöltése. Ha már létezik egy ilyen nevű blob tároló, ez blob tartalmának felülíródnak.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Győződhet meg, hogy ez hogyan működik a [Microsoft Azure tároló Explorer](http://storageexplorer.com) megjeleníti, és ellenőrzi, hogy a tároló, *containerpublic*, tartalmazza a blob, *sampleblob*. Az ebben a példában használt nyilvános tároló úgy is ellenőrizheti, hogy ez dolgozott nyissa meg a BLOB URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Nemcsak a feltöltésének tiltása blob-NSString származó, hasonló módszerek léteznek NSData, NSInputStream vagy egy helyi fájl.

## <a name="list-the-blobs-in-a-container"></a>A BLOB tárolóban lista
A következő példa bemutatja, hogyan valamennyi BLOB-tárolóban listája. Ez a művelet végrehajtásakor kell szem előtt tartva, az alábbi paramétereknek:     

- **continuationToken** - a folytatását jelző jogkivonat helyén, ahol a nevére a partnerlistában műveletet kell kezdődnie. Amennyiben nem jogkivonat, itt a felsorolásban BLOB az elejéről. Tetszőleges számú BLOB is szerepelnek, nullától felfelé maximális meg. Akkor is, ha ez a módszer visszatérési értéke nulla, ha `results.continuationToken` nulla, nem lehet további BLOB az szolgáltatása, amely nem szerepel.
- **előtag** - megadhatja az előtagot, amelyet blob listaelem. Csak ezt az előtagot kezdődő BLOB lesz látható.
- **useFlatBlobListing** - említett [névhasználati és hivatkozó tárolók és BLOB](#naming-and-referencing-containers-and-blobs) csoportban a Blob-szolgáltatás ugyan a strukturálatlan tároló sablonjának hozhat létre virtuális hierarchia szerint az elérési útja BLOB elnevezési. Azonban nem sima nevére a partnerlistában jelenleg nem támogatott. Ez hamarosan. Most ezt az értéket kell lennie`YES`
- **blobListingDetails** - megadhatja, mely elemek szerepeljen BLOB listázása
    - `AZSBlobListingDetailsNone`: Csak a lekötött BLOB listában, és nem ad vissza blob-metaadatok.
    - `AZSBlobListingDetailsSnapshots`: Lista lekötött BLOB- és blob-pillanatképek.
    - `AZSBlobListingDetailsMetadata`: Egyes blob blob-metaadatait lekérés visszaadva a nevére a partnerlistában.
    - `AZSBlobListingDetailsUncommittedBlobs`: Vállalt és nem véglegesített BLOB sorolja fel.
    - `AZSBlobListingDetailsCopy`: Tulajdonságok másolása bele a nevére a partnerlistában.
    - `AZSBlobListingDetailsAll`: Az összes rendelkezésre álló vállalt BLOB, nem véglegesített BLOB és pillanatképek listában, és visszatérési e BLOB összes metaadatok és a Másolás állapota.
- **maxResults** – ehhez a művelethez eredmény maximális száma. -1 használatával nem korlátozhatja.
- **completionHandler** - kódot a nevére a partnerlistában művelet eredményével hajthat végre lévő időtartomány.

Ebben a példában a segítő módja használt rekurzív hívás a lista BLOB módszer minden alkalommal, amikor egy folytatását jelző token értéket adja vissza.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Blob letöltése

A következő példa bemutatja, hogyan tölthető le blob-objektum NSString.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Blob törlése

A következő példa bemutatja, hogy miként törölhet egy blob.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Törölje a blob-tárolóhoz

A következő példa bemutatja, hogyan kell a tároló törölni.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta Blob-tárolóhoz használatáról az iOS, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az iOS-tár és a tároló szolgáltatás.

- [Az iOS Azure tároló ügyfél tárban](https://github.com/azure/azure-storage-ios)
- [Azure tároló iOS hivatkozási dokumentáció](http://azure.github.io/azure-storage-ios/)
- [Azure tárolása REST API-val](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage)

Ha a tár kapcsolatos kérdések nyugodtan közzéteheti a [fórum az MSDN webhelyen Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) és [Papírhalom túlcsordulás](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Ha szolgáltatás javaslatok Azure tárolására, kérjük, tartalmakat tehet közzé [Azure tároló visszajelzést](https://feedback.azure.com/forums/217298-storage/).
