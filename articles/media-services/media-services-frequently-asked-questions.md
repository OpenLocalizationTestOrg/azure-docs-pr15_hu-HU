<properties 
    pageTitle="Gyakori kérdések |} Microsoft Azure" 
    description="Gyakori kérdések" 
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


#<a name="frequently-asked-questions"></a>Gyakori kérdések

##<a name="general-ams-faqs"></a>Általános AMS gyakori kérdések

Kérdés: hogyan, átméretezheti, indexelés?

A: a fenntartott megadott egységek azonos a kódolás és az indexelő feladatokat. [Hogyan skála kódolást fenntartott egységek](media-services-scale-media-processing-overview.md)megjelenő útmutatást követve. **Megjegyzés:** az indexelő teljesítmény fenntartott mértékegysége nem érinti.

Probléma: feltöltött kódolt és videó közzé. Milyen lenne az OK, a videó nem játssza le azt adatfolyam meg?

A: a leggyakoribb okok egyike kiosztva az adatfolyam végpontot, amelyből lejátszás próbál meg legalább egy fenntartott adatfolyam egység nem rendelkezik.  [Hogyan skála Streaming fenntartott egységek](media-services-portal-scale-streaming-endpoints.md)megjelenő útmutatást követve.

Kérdés: van lehetőség a élő adatfolyam összeállítás tenni?

A: összeállítás élő adatfolyamok a jelenleg nem jelenik meg Azure Media Services, így kell volna előre írhat a számítógépen.

Kérdés: van lehetőség Azure CDN az élő Streaming?

A: Media Services támogatja a integrálása az Azure CDN (További információ megtudhatja, [hogy miként kezelheti a folyamatos átvitelű végpontok pontra a Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)).  Adatfolyam-CDN Live is használhatja. Azure Media Services zökkenőmentes adatfolyam, HLS és MPEG-vonal kimeneti értékeket tartalmaz. Ezek a formátumok HTTP használata az adatok átvitele, és a HTTP-gyorsítótárazás előnyei el. Az élő a folyamatos átvitelű videó és hang töredékek részre van osztva, és ez egyes töredékek CDN-az első gyorsítótáras tényleges. Csak adatokat kell frissíteni kell a nyilvánvalóan adatok. CDN rendszeres időközönként frissítse a nyilvánvalóan adatokat.

Kérdés: jelent az Azure Media services támogatja tárolását képeket?

Megoldás: Ha egyszerűen csak vendégfelhasználókat JPEG vagy PNG képeket tárolja, a Azure Blob-tárolóhoz legyen. Nem nincs számára nyújtott előnyökre: a Media Services-fiókjában elhelyez őket, kivéve, ha meg szeretné őrizni a videó vagy hang eszközök társított őket. Vagy ha lehet, hogy a képek használata a videó a kódoló korrektúraréteg szükséges. Media Encoder szabványos képek használatát támogatja felirataként videók fölött, és mi felsorolja JPEG és PNG támogatott formátumok bemeneti. További tudnivalókért olvassa el a [Átfedi létrehozása](media-services-custom-mes-presets-with-dotnet.md#overlay)című témakört.

Kérdés: hogyan másolhatok eszközök Media Services-fiókból egy másikra.

A: másolandó eszközök Media Services-fiókból egy másik .NET, használja a módszernek az [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) bővítmény érhető el az [Azure Media Services .NET SDK-bővítmények](https://github.com/Azure/azure-sdk-for-media-services-extensions/) tárban tárolnak. További tudnivalókért lásd: [a](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) fórum szál.

Kérdés: melyek a támogatott karaktereit fájlok elnevezése AMS való munkavégzés során?

A: Media Services használja a IAssetFile.Name tulajdonság értékét adatfolyam (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) tartalma URL-címek készítésekor Emiatt az URL-kódolás nem engedélyezett. A **Name** tulajdonság értéke nem lehet a következő [karakterek készültségi kódolást-fenntartott](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Még csak lehet egy "." az a fájlnévhez tartozó kiterjesztést.


Kérdés: hogyan csatlakozhat a többi?

A: https://media.windows.net sikeres kapcsolódás után jelenik meg a 301 átirányítást másik Media Services URI megadása. A [Csatlakozás a Media Services REST API segítségével](media-services-rest-connect-programmatically.md)leírtak szerint új URI későbbi hívásainak kell végeznie. 


Kérdés: hogyan lehet videó elforgatása a kódolási folyamat során.

A: a [Media Encoder szabványos](media-services-dotnet-encode-with-media-encoder-standard.md) Elforgatás 90/180/270 többszöröseire által támogatja. Az alapértelmezett működés "Automatikus", ahol próbál meg a Forgatás metaadatok észleli a bejövő MP4/MOV fájlban, és azt kompenzálja. Többek között a következő **forrásokból** elemet az egyik a json készletek definiált [Itt](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
