<properties 
    pageTitle="Hogyan kódolását használatával Media Encoder szabványos tárgyi eszköz |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Media Encoder szokásos segítségével a Media Services médiatartalom kódolását. Mintakódok REST API-t használja." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Egy tárgyi eszköz használatával Media Encoder szabványos kódolását módjáról


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [TÖBBI](media-services-rest-encode-asset.md)
- [Portál](media-services-portal-encode.md)

##<a name="overview"></a>– Áttekintés
Digitális videó előadása az interneten keresztül érdekében tömöríti a médiafájlokat. Digitális videofájlok igen nagy, és lehetséges, hogy túl nagy az interneten keresztül, vagy a vevők eszközök: helytelenül jelennek meg az előadásához. A folyamaton, így ügyfelei megtekintheti a médiafájlok tömörítése a video- és audiofájl kódolást.

Kódolási feladatok a Media Services a leggyakoribb feldolgozási műveletek közül. Médiafájlok konvertálása között egy kódolást kódolási feladatok létrehozása Amikor a kódolását, a Media Services beépített kódoló (Media Encoder Standard) is használhatja. Is használhatja a Media Services partner; által biztosított kódoló harmadik fél kódolók érhetők el a Microsoft Azure piactéren. Megadhatja, hogy előre definiált karakterláncokat a kódoló használatával, illetve előre definiált konfigurációs fájl használatával kódolása a tevékenységek részleteit. A rendelkezésre álló készletek típusú talál további [Media Encoder szabványos tevékenység készletet](http://msdn.microsoft.com/library/mt269960).

Minden feladat beállíthatja, hogy egy vagy több feladat, amely azt szeretné, hogy miként végezhet el feldolgozás típusától függően. Az REST API-k hozhat létre projekteken és tevékenységeken a hozzájuk kapcsolódó rendelt két módszer egyikével:

- Feladatok lehet definiált beágyazott keresztül feladat személyek, a feladatok navigációs tulajdonságot, vagy
- keresztül OData parancsfájl.


Ajánlott mindig kódolását Emeletes fájlok be adaptív átviteli sebesség MP4 beállítása, és alakítsa át a beállítás használata a [Dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md)kívánt formátumra. Kihasználhatja dinamikus csomagolása, először a továbbított végpontot, amelyből tervezi kézbesítési a tartalom előbb be kell szereznie legalább egy igény szerinti adatfolyam egység. További információért tájékozódhat [skála Media Services](media-services-portal-manage-streaming-endpoints.md).

Ha a kimeneti eszköz titkosított tárhely, meg kell adnia eszköz kézbesítési házirend. További információt a [konfigurálása eszköz kézbesítési házirend](media-services-rest-configure-asset-delivery-policy.md)témakörben található.


>[AZURE.NOTE]Médiafájlok processzorok hivatkozó megkezdése előtt ellenőrizze, hogy rendelkezik-e a megfelelő media processzor azonosítójával. További tudnivalókért lásd: az [Első Media processzorok](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>A feladat kódolási egyetlen feladat létrehozása

>[AZURE.NOTE] Amikor a Media Services REST API-val, a következő érvényesek:
>
>A Media Services szervezetek elérésekor a be kell állítani a HTTP-összehívásokban adott fejlécmezők és értékek. További tudnivalókért olvassa el a [Media Services REST API -val fejlesztési beállítása](media-services-rest-how-to-use.md)című témakört.

>Https://media.windows.net sikeres kapcsolódás után jelenik meg a 301 átirányítást másik Media Services URI megadása. A [Csatlakozás a Media Services REST API segítségével](media-services-rest-connect-programmatically.md)leírtak szerint új URI későbbi hívásainak kell végeznie.
>
>Ha JSON használ, és megadja, hogy használja a **__metadata** kulcsszót a kérelem (például a hivatkozások a csatolt objektumot) [JSON részletes formátumot](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)kell állítani az **Elfogadás** élőfej: elfogadás: alkalmazás/json; odata = részletes.

A következő példa bemutatja, hogyan hozhat létre, és indítsa el a feladat egy tevékenység beállítása adott felbontásban és minőségű videoklip kódolását. Kódolást Media Encoder szabványos, ha meghatározott tevékenység konfigurációs készletek is használhatja [az alábbi](http://msdn.microsoft.com/library/mt269960).

A kérelem:

BEJEGYZÉS https://media.windows.net/API/Jobs a HTTP/1.1-tartalomtípust: alkalmazás/json; odata részletes elfogadás =: alkalmazás/json; odata részletes DataServiceVersion =: 3.0-s MaxDataServiceVersion: x-ms-verzió 3.0-s: 2.11 engedélyezési: Bearer <token value> 
 x-ms-ügyfél-kérés-id: 00000000-0000-0000-0000-000000000000 Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Válasz:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>A kimenet eszköz nevének megadása

A következő példa bemutatja, hogyan kell beállítani a assetName attribútum:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Megfontolandó szempontok

- TaskBody tulajdonságok konstans XML kell használnia beviteli számának meghatározása, vagy a kimeneti a tevékenység által használt eszköz. A tevékenység a témakör az XML-séma definíciója az XML-hez.
- A TaskBody definícióban minden belső értéke <inputAsset> és <outputAsset> JobInputAsset(value) vagy JobOutputAsset(value) kell állítani.
- Feladat több kimeneti eszközök állhat. Egy JobOutputAsset(x) csak egyszer használható ezt az egy projektben tevékenység egy eredményt.
- Megadhatja a bemeneti tárgyi eszköz egy tevékenység JobInputAsset vagy JobOutputAsset.
- Feladatok nem kell verziókövetéssel egy űrlapot.
- Az érték paraméter JobInputAsset vagy JobOutputAsset átadandó index értékét az eszköz képviseli. A tényleges eszközök a feladat entitás meghatározása a InputMediaAssets és OutputMediaAssets navigációs tulajdonságok határozzák meg. 
- Media Services OData v3 épül, mert az egyes eszközök InputMediaAssets és OutputMediaAssets navigációs tulajdonság tartozó hivatkozott keresztül egy "__metadata: uri" név – érték pár.
- Egy vagy több eszközök Media Services létrehozott InputMediaAssets rendeli hozzá. OutputMediaAssets hozza létre a rendszer. Azok a meglévő tárgyi eszköz nem hivatkozik.
- OutputMediaAssets nevet adhat a assetName attribútum használatával. Ha az attribútum nem szerepel a, majd a OutputMediaAsset nevét, függetlenül a belső szöveges érték lesz a <outputAsset> elem utótaggal a projekt-név oszlopban látható értéket vagy a feladat azonosítója értéket (abban az esetben, ha a Name tulajdonság nem definiálja). Például ha assetName "Minta" értéket, majd a OutputMediaAsset Name tulajdonság volna állíthatók be "Minta". Azonban assetName értéket nem adta meg, de adta meg az "NewJob" a feladat nevére, majd a OutputMediaAsset neve akkor "JobOutputAsset (érték-) _NewJob". 


##<a name="create-a-job-with-chained-tasks"></a>Hozzon létre egy projektet láncolt feladatok

Sok helyzeteket fejlesztők szeretne feldolgozása tevékenységek sorozatának létrehozása. A Media Services láncolt tevékenységek sorozatának is létrehozhat. Minden tevékenység különböző feldolgozó műveleteket hajtja végre, és használhatja a különböző media processzorok. A láncolt feladatok is leadandó tárgyi eszköz egy tevékenységből között a lineáris sorozatában feladatok elvégzéséhez az eszköz. A projekt feladatait sorrendben kell azonban nem kötelező. Ha láncolt feladatot létrehozni, a láncolt **ITask** objektumok **IJob** egyetlen objektum jönnek létre.

>[AZURE.NOTE] Jelenleg legfeljebb 30 feladatok Feladatonkénti. Lánc 30-nál több feladat van szüksége, ha egynél több feladat, amely tartalmazhat a tevékenységek létrehozása


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Megfontolandó szempontok

Ahhoz, hogy tevékenység láncolás:

- A feladat legalább 2 feladatokat kell rendelkeznie.
- Legalább egy tevékenység, amelynek bemeneti érték a feladatot egy másik feladat kimenetét kell lennie.

## <a name="use-odata-batch-processing"></a>Az OData-parancsfájl használata 

A következő példa bemutatja, hogyan OData parancsfájl segítségével egy feladatot, és a tevékenységek létrehozása. Parancsfájl a további tudnivalókért lásd [Adatok protokollhoz (OData-) köteg feldolgozása](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Hozzon létre egy projektet egy JobTemplate használatával


Gyakori feladatok használatával több eszközök feldolgozása, amikor JobTemplates hasznosak, adja meg az alapértelmezett tevékenység készletek, a feladatok sorrendjének, és így tovább.

A következő példa bemutatja, hogyan lehet létrehozni egy JobTemplate egy definiált TaskTemplate beágyazott. A TaskTemplate használja a Media Encoder szabványos a MediaProcessor kódolása a digitáliseszköz-fájlt. azonban más MediaProcessors felhasználható is. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Eltérően más Media Services szervezetek egy új GUID azonosítója definiálása minden TaskTemplate és helyezze a taskTemplateId és azonosító tulajdonság a összehívás törzsébe. A tartalom azonosítási séma követniük kell a rendszer az Azure Media Services szervezetek azonosítása leírt. JobTemplates is, nem lehet frissíteni. Ehelyett a frissített módosítások egy újat kell létrehoznia.
 

Ha sikeres, a következő válasz adja vissza:
    
    HTTP/1.1 201 Created
    
    . . .


A következő példa bemutatja, hogyan hozhat létre egy arra hivatkozó JobTemplate azonosítóra feladatot:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Ha sikeres, a következő válasz adja vissza:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Következő lépések
Most, hogy tudja, hogy miként hozhat létre egy feladatot egy assset kódolását, nyissa meg a [Hogyan szeretné feladat állapot ellenőrzése a Media Services](media-services-rest-check-job-progress.md) témakörre.


##<a name="see-also"></a>Lásd még:

[Első Media processzorok](media-services-rest-get-media-processor.md)
