<properties
    pageTitle="Dinamikus összecsomagolása áttekintése |} Microsoft Azure"
    description="A témakör ad és dinamikus összecsomagolása áttekintése."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dinamikus csomagolása

##<a name="overview"></a>– Áttekintés

Microsoft Azure Media Services is használható számos forrást fájl médiaformátumok, media adatfolyam-formátumokat, nyújtásához, és tartalomvédelem formátumok ügyfél technológiák számos (például iOS, XBOX, a Silverlight, a Windows 8). Ezek az ügyfelek megérteni a különböző protokollok, például az iOS igényel-4-es HTTP-Live Streaming (HLS) formátum, és a Silverlight és Xbox konzolra megkövetelése zökkenőmentes Streaming. Ha vannak olyan egy adaptív átviteli sebesség (többszörös-átviteli sebesség) MP4 (ISO alap 14496 – 12) médiafájlok vagy adaptív átviteli sebesség zökkenőmentes Streaming fájlokat szolgálnak az ügyfelek számára MPEG kötőjel, a HLS vagy a folyamatos zökkenőmentes megértéséhez kívánt, meg kell kihasználhatja Media Services dinamikus csomagolást.

Dinamikus összecsomagolása szükség, amelyben az adaptív átviteli sebesség MP4 fájlokat vagy adaptív átviteli sebesség zökkenőmentes adatfolyam-fájlok eszköz létrehozása. Ezután a megadott formátumban, az On igény folyamatos átvitelű jegyzék vagy fragment összehívásban alapján kiszolgáló biztosítja, hogy a választott protokoll kap az adatfolyam. Eredményt adja csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services szolgáltatás fog összeállítása és a megfelelő választ, egy ügyfél érkező kérések alapján szolgálnak.

Az alábbi ábra mutatja, a hagyományos kódolási és a munkafolyamat statikus csomagolást.

![Statikus kódolást](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Az alábbi ábra mutatja a dinamikus összecsomagolása munkafolyamatot.

![Dinamikus kódolást](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Kihasználhatja dinamikus csomagolása, először a továbbított végpontot, amelyből tervezi kézbesítési a tartalom előbb be kell szereznie legalább egy igény szerinti adatfolyam egység. További információért tájékozódhat [skála Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="common-scenario"></a>Leggyakoribb helyzet

1. Töltse fel a bemeneti fájl (Emeletes fájl neve). Ha például H.264, MP4 vagy WMV (- [Formátumokat támogatja a Media Encoder szabványos által](media-services-media-encoder-standard-formats.md)támogatott formátumok lásd: listája.

1. Emeletes fájljait H.264 MP4 adaptív átviteli sebesség beállítása kódolását.

1. Az eszköz, amely tartalmazza az adaptív átviteli MP4 beállítása a On igény megnevezés létrehozásával a Közzététel gombra.

1. Szerkesztés és a adatfolyamként adatfolyam URL-címét.


##<a name="preparing-assets-for-dynamic-streaming"></a>Dinamikus streaming eszközök előkészítése

Az eszköz Felkészülés dinamikus streaming lehetőségei vannak:

1. A [fő fájl feltöltése](media-services-dotnet-upload-files.md).
2. [Használja a Media Encoder szabványos kódoló kapcsol H.264 MP4 adaptív átviteli sebesség beállítása](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [A tartalom adatfolyam](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Dinamikus összecsomagolása által nem támogatott

A következő forrás fájlformátumok dinamikus összecsomagolása által nem támogatott.

- Dolby digital mp4-fájlok.
- Dolby digital zökkenőmentes fájlokat.

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
