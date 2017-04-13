<properties
    pageTitle="Tartalom kézbesítése ügyfelek |} Microsoft Azure"
    description="Ez a témakör áttekintést nyújt a Mi az Azure Media Services-tartalom előadásához vesz részt."
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


# <a name="deliver-content-to-customers"></a>Tartalom kézbesítése vevők
A folyamatos átvitelű vagy igény szerinti videó tartalom ügyfelek esetén tartja, a cél esetén nagyfelbontású videoklipekkel végzi a különböző hálózati feltételek különféle eszközökön.

Ez a cél elérését a következőkre van lehetősége:

- A többszörös átviteli sebesség (adaptív átviteli sebesség) video-adatfolyam adatfolyam kódolását. Ez lesz gondoskodik a minőség és a hálózati feltételek.
- Microsoft Azure Media Services [dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md) segítségével dinamikusan újra csomag be a adatfolyam különböző protokollok be. Ez lesz gondoskodik a folyamatos átvitelű eszközön. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák kézbesítve: HTTP-Live Streaming (HLS), a folyamatos zökkenőmentes, MPEG-vonal és HDS (az Adobe Primetime és a hozzáférés licenciavevők csak).

Ez a cikk áttekintést nyújt a fontos tartalomkézbesítési fogalmak.

Ismert problémák ellenőrzése, olvassa el a [ismert problémái](media-services-deliver-content-overview.md#known-issues)című témakört.

## <a name="dynamic-packaging"></a>Dinamikus csomagolása

A dinamikus csomagolása, ahol a Media Services, a folyamatos átvitelű Media Services (MPEG-vonal, HLS, zökkenőmentes Streaming, HDS) által támogatott anélkül, hogy újra be ezeket a folyamatos átvitelű formátumok csomag adaptív átviteli sebesség MP4 vagy zökkenőmentes adatfolyam-címként kódolt tartalmának juttathat el. Azt javasoljuk, hogy a tartalom dinamikus összecsomagolása előadásához.

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- Emeletes (forrás) fájlját kódolását adaptív átviteli sebesség MP4 vagy adaptív átviteli sebesség zökkenőmentes Streaming fájlokat alakítja.
- Szerezze be legalább egy igény szerinti adatfolyam egység az adatfolyam végpontot, ahonnan a tartalmak olvasása megtervezése. További információért megtudhatja, [hogy miként igény szerinti fenntartott egységek streaming méretezni](media-services-portal-manage-streaming-endpoints.md).

A dinamikus csomagolása, tárolhatja, és a egyetlen tárolási formátumú fájlok fizetni. Media Services össze, és a szolgálnak a megfelelő választ, a kérések alapján.

Kívül hozzáférhetővé a dinamikus összecsomagolása funkciók, igény szerinti fenntartott egységek streaming adja meg, amely az időosztáson 200 MB vásárolhatja dedikált kilépési kapacitása. Alapértelmezés szerint a igény szerinti streaming más felhasználókkal megosztott erőforrásokat (például a számítási és kilépési kapacitású) mely kiszolgáló megosztott példány adatmodell van beállítva. Az igény szerinti adatfolyam átviteli adatfolyam fenntartott egységek igény szerinti vásároljon javíthatja.

További tudnivalókért lásd: [dinamikus csomagolást](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Szűrők és dinamikus jegyzékfájlok

A Media Services eszközök szűrők határozhatja meg. Ezek a szűrők, amelyek segítségével az ügyfelek, mint egy adott részének videó lejátszása dolgot kell tennie, vagy adja meg, hogy az ügyfél eszköz (helyett az összes a megjelenítések az eszköz társított) képes kezelni, hang- és megjelenítések csak egy részhalmazát kiszolgálóoldali szabályok. Ez a szűrés jönnek létre, ha az ügyfél kéri, hogy egy vagy több megadott szűrőkkel alapján videolejátszás *dinamikus jegyzékfájlok* elérni.

További tudnivalókért olvassa el a [szűrők és dinamikus jegyzékfájlok](media-services-dynamic-manifest-overview.md)című témakört.

## <a name="locators"></a>Locator

Ahhoz, hogy a felhasználó URL-adatfolyam-vagy a tartalom letöltése használható, először kell tegye közzé a tárgyi eszköz egy megnevezés létrehozásával. Egy megnevezés tárgyi eszköz tárolt fájlok eléréséhez belépési pontjához biztosít. Media Services kétféle Locator támogatja:

- OnDemandOrigin Locator. Ezek használják (például MPEG-vonal, HLS vagy zökkenőmentes Streaming) adatfolyam multimédiás vagy fokozatosan letölteni fájlokat.
-  Megosztott access aláírás (Társítások) URL-cím Locator. Ezek a házirendek töltse le a médiafájlokat a helyi számítógépre.

Az *access-házirend* használják (például olvasható, írást és lista) engedélyek meghatározása és időtartama, amelynek az ügyfél rendelkezik hozzáféréssel egy adott eszközre. Ne feledje, hogy az a (AccessPermissions.List) engedéllyel nem használható a egy OrDemandOrigin megnevezés létrehozása.

Locator lejárati dátummal rendelkeznek. Az Azure portál 100 évvel későbbi lejárati dátumot Locator állítja be.

>[AZURE.NOTE] Az Azure portal hozta létre Locator március 2015 előtt, ha adott Locator állított, két év múlva lejár.

Frissíti egy megnevezés a lejárati dátum, használja a [többi](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-khoz. Figyelje meg, hogy ha frissíti a Társítások megnevezés lejárati dátumát, az URL-cím megváltozik.

Felhasználói hozzáférés-vezérlés kezelésére Locator nem készült. Különböző hozzáférési jogosultsággal, és az egyéni felhasználók a digitális a tartalomvédelmi szolgáltatást (DRM) használja megoldások használatával adhat meg. További tudnivalókért olvassa el a [Media biztonságossá tétele](http://msdn.microsoft.com/library/azure/dn282272.aspx)című témakört.

Amikor létrehoz egy megnevezés, miatt szükséges tárhely és a terjesztési folyamatok Azure-tárolóban lévő 30 másodperces késleltetést lehet.


## <a name="adaptive-streaming"></a>A folyamatos átvitelű adaptív

Adaptív átviteli sebesség technológiák lehetővé teszik a videolejátszó alkalmazások hálózati feltételeinek meghatározására, és válassza a több bitrates. Csökkenti a hálózati forgalmat, amikor az ügyfél választhat egy alsó átviteli sebesség, az alsó képminőség a lejátszás továbbra is. Hálózati feltételek javítása, mint az ügyfél egy újabb átviteli sebesség, a továbbfejlesztett képminőség lehet váltani. Azure Media Services támogatja a következő adaptív átviteli sebesség technológiák: HTTP-Live Streaming (HLS), a folyamatos zökkenőmentes, MPEG-vonal és HDS.

A folyamatos átvitelű URL-címeit, hogy a felhasználók, először létre kell hoznia egy OnDemandOrigin megnevezés. A megnevezés létrehozása biztosít az alap elérési útja az eszköz, amely tartalmazza a kívánt adatfolyam-tartalom. Azonban engedélyezni a adatfolyamként, akkor módosítania kell az elérési út további. A teljes URL-címet a továbbított nyilvánvalóan fájl összeállításához, a megnevezés path érték és a jegyzék (filename.ism) kell ÖSSZEFŰZ fájl nevét. Ezután **hozzáfűzése/cikkét** és a megfelelő formátumú (ha szükséges) a megnevezés elérési útját.

>[AZURE.NOTE]A tartalom is lehet adatfolyamként, SSL-kapcsolaton keresztül. Ehhez ellenőrizze, hogy a továbbított URL-címek Kiindulás HTTPS-e.

Akkor is csak adatfolyamként SSL Ha az adatfolyam végpontot, amelyből a tartalmak olvasása után szeptember 10, 2014-es jött létre. Ha a továbbított URL-ek a létrehozása után szeptember 10, 2014-es, adatfolyam végpontok alapuló URL-CÍMÉT tartalmazza-e a "streaming.mediaservices.windows.net." Adatfolyam URL-címeit tartalmazó "origin.mediaservices.windows.net" (a régi formátum) nem támogatják az SSL. Ha szeretné engedélyezni szeretné a SSL-adatfolyam az URL-címet a régi formátumban van, az új adatfolyam végpont létrehozásához. Az új adatfolyam végpontot alapján URL használatával a adatfolyamként SSL.


## <a name="streaming-url-formats"></a>A folyamatos átvitelű URL-formátumok

### <a name="mpeg-dash-format"></a>MPEG-vonal formázása

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=mpd-Time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) V4 formázása

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3 formázása

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS) formátum csak hangot tartalmazó szűrővel

Alapértelmezés szerint csak hangot tartalmazó számok szerepelnek a HLS jegyzék. Az Apple Store áruházból hitelesítő mobil hálózatok szükséges. Ebben az esetben ha egy ügyfél nincs elegendő a sávszélesség, vagy egy 2G-kapcsolaton keresztül csatlakozik, a lejátszás – Váltás csak hang. Ezzel az elemzéssel megtartása adatfolyamként pufferelési nélkül, de nem a videót. Bizonyos esetekben pufferelési player lehet használni kívánt csak hangot tartalmazó fölé. Ha el szeretné távolítani a csak hangot tartalmazó nyomon követéséhez, vegye fel **csak hangot tartalmazó = false** az URL-címet.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3,audio-only=FALSE)

További tudnivalókért olvassa el a [dinamikus cikkét kialakítási támogatási és HLS kimeneti további funkciókkal](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)című témakört.


### <a name="smooth-streaming-format"></a>Zökkenőmentes adatfolyam-formátum

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Példa:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Zökkenőmentes Streaming 2.0-s jegyzék (régi jegyzék)

Alapértelmezés szerint nyilvánvalóan formátum zökkenőmentes Streaming tartalmazó ismétlési (r-címke). Néhány lejátszó azonban nem támogatják a "r" címke. Ezeket az ügyfelek számára használható, amely letiltja a "r" címke formátumot:

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak)

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Progresszív letöltés

Fokozatos letöltés indíthat media lejátszása, mielőtt a teljes fájl letöltődött. Fokozatosan .ism * (ismv, isma, ismt vagy ismc) fájlok nem lehet letölteni.

Fokozatosan a tartalom letöltése, használja a megnevezés OnDemandOrigin típusát. A következő példa bemutatja az URL-cím megnevezés OnDemandOrigin típusú alapján:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Tárterület titkosított eszközök, a progresszív letöltésére, a származási szolgáltatásból adatfolyam kívánt kell visszafejtése.

## <a name="download"></a>Letöltés

Egy ügyfél eszközön töltse le a tartalmakat, létre kell hoznia egy Társítások megnevezés. A Társítások megnevezés férhet hozzá a Azure tároló tároló ahol a fájl található. Össze a letöltés URL-CÍMÉT, hogy beágyazása a host és Társítások aláírás között a fájl nevét.

A következő példa bemutatja az URL-címet, amely a Társítások megnevezés alapul:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

A következő érvényesek:

- Tárterület titkosított eszközök, a progresszív letöltésére, a származási szolgáltatásból adatfolyam kívánt kell visszafejtése.
- A letöltés, amely még nem fejeződött be 12 órán belül sikertelen lesz.

## <a name="streaming-endpoints"></a>Adatfolyam-végpontok

Egy adatfolyam végpont adatfolyam szolgáltatás, amely közvetlenül a lejátszó ügyfélalkalmazás vagy további eloszlás tartalomkézbesítési hálózatai (CDN) tartalma tartana képviseli. A kimenő adatfolyam adatfolyam végpont szolgáltatásból lehet élő adatfolyam vagy egy igény szerinti videó eszköz a Media Services-fiókjában. Azt is szabályozhatja az adatfolyam végpont szolgáltatás kezelése növekvő sávszélesség igényeinek módosításával és a folyamatos átvitelű fenntartott egységek kapacitása. Alkalmazások munkakörnyezetben lefoglalandó kell legalább egy fenntartott egység. További információért tájékozódhat [skála media szolgáltatás](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Ismert problémák

### <a name="changes-to-smooth-streaming-manifest-version"></a>Zökkenőmentes Streaming módosításai cikkét verziója

Július 2016 szolgáltatás kiadása – előtt eszközök készített a Media Encoder normál, amikor Media Encoder prémium munkafolyamat vagy a korábbi Azure Media Encoder voltak folyamatosan lejátszott dinamikus csomagolást – zökkenőmentes Streaming használatával szeretné megfelelnek a visszaadott jegyzék 2.0-s verzióját. A 2.0-s verziója a fragment időtartamok ne használjon az úgynevezett Ismétlés (r) címkéket. Példa:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

A július 2016 szolgáltatás verzióban a létrehozott zökkenőmentes Streaming jegyzék megfelel-e verzió 2.2 fragment időtartamok ismétlési-címkék használata. Példa:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

A régi zökkenőmentes Streaming ügyfelek nem támogatja az ismétlés címkéket és a jegyzék betöltése sikertelen lesz. Probléma csökkentésében, használhatja a régi nyilvánvalóan formátum paraméter **(formátum = fmp4-v20)** meg és frissítheti az ügyfelek a legújabb verzióra, amely támogatja az ismétlés címkék. További tudnivalókért lásd: [zökkenőmentes Streaming 2.0-s](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Kapcsolódó témakörök

[Media Services Locator frissítése után közbeni tároló kulcsok](media-services-roll-storage-access-keys.md)
