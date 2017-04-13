<properties 
    pageTitle="Szűrők és dinamikus jegyzékfájlok |} Microsoft Azure" 
    description="Ez a témakör ismerteti, hogyan hozhat létre a szűrők, így az ügyfélnek lehet a segítségükkel adatfolyam meghatározott szakaszait megjelenítő Értékáram. Media Services létrehoz dinamikus jegyzékfájlok a szelektív streaming archiválását." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Szűrők és dinamikus jegyzékfájlok

2.11 megjelenés kezdve, a Media Services lehetővé teszi a eszközök szűrők megadása. Ezek a szűrők kiszolgáló egymás szabályok, amelyek lehetővé teszik az ügyfelek dönt, hogy a többek között: csak egy részét (helyett a teljes videó lejátszása) a videó lejátszás, vagy adjon meg, hogy az ügyfél eszköz (helyett az összes a megjelenítések az eszköz társított) képes kezelni, hang- és megjelenítések csak egy részét. Ez a szűrés a eszközök archivált **Dinamikus cikkét**s, az ügyfél kérésre adatfolyam-videó megadott szűrő alapján létrehozott keresztül.

A témakörök esetei, amelyben ismerteti, hogy nagyon hasznos a vevők és, amelyek bemutatják, hogy miként hozhat létre a szűrők programozás útján témakörökre mutató hivatkozások szűrőkkel lenne (jelenleg hozhat létre szűrők REST API-khoz csak).

##<a name="overview"></a>– Áttekintés

A tartalom ügyfelek (a folyamatos átvitelű élő eseményeket vagy igény szerinti videó) során a célja egy jó minőségű videó végzi a különböző hálózati feltételek különféle eszközökön. Eléréséhez ezzel a cél, tegye a következőket:

- a többszörös átviteli sebesség ([Adaptív átviteli sebesség](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) video-adatfolyam (Ez lesz gondoskodik a minőség és a hálózati feltételek) adatfolyam kódolni és 
- [Dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md) Media Services segítségével dinamikusan újra csomag be a adatfolyam (Ez lesz gondoskodik a folyamatos átvitelű eszközön) különböző protokollok be. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák kézbesítve: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak). 

###<a name="manifest-files"></a>Jegyzékfájlok 

Ha, kódolását adaptív átviteli sebesség folyamatos tárgyi eszköz, létrejön egy **cikkét** (lista) fájl (a fájlt a szöveges alapú vagy XML-alapú). A **cikkét** fájl tartalmaz, például a metaadat-alapú streaming: nyomon követheti a típusa (hang, videó vagy szöveg), nyomon követheti a nevét, kezdési és befejezési időpontot, átviteli sebesség (Tulajdonságok), nyomon követése nyelven, bemutató (ablakban csúsztatható rögzített időtartamú), a videó kodek (FourCC). A arra utasítja a következő részlet letöltendő és információt a következő lejátszható videó töredékek érhető el, és a helyükre a Windows Media player is. Töredékek (vagy szegmensek) a tényleges "mennyiségű" a videót.


Íme egy példa nyilvánvalóan fájl: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dinamikus jegyzékfájlok

Vannak [helyzetek](media-services-dynamic-manifest-overview.md#scenarios) , amikor az ügyfelek van szüksége az alapértelmezett eszköz nyilvánvalóan fájl leírtakhoz-nál nagyobb rugalmasság érdekében. Példa:

- Eszköz adott: csak a megadott megjelenítések és/vagy a nyelv számok, az eszköz által támogatott megadott előadásához használt lejátszás ("a képmegjelenítés szűrés") tartalma. 
- Csökkentse a jegyzék ("alszint ClipArt szűrés") élő esemény alszint klip megjelenítéséhez.
- A kezdő ("csonkolása videó") Videoklip vágása.
- Állítsa be a bemutató ablak (DVR) a Windows Media player ("beállítása bemutató ablak") az DVR ablak korlátozott hossza biztosítása érdekében.
 
E rugalmasság eléréséhez a Media Services kínál, **Dinamikus jegyzékfájlok:** előre definiált [szűrők](media-services-dynamic-manifest-overview.md#filters)alapján.  A szűrők határozza meg, amikor az ügyfelek segítségével őket egy adott képmegjelenítés vagy a videoklip alszint klipek adatfolyam. Őket kellene szűrő adja meg az adatfolyam URL-címét. Szűrők alkalmazható a folyamatos átvitelű [Dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md)által támogatott protokollok adaptív átviteli sebesség: HLS, MPEG-vonal, zökkenőmentes Streaming és HDS. Példa:

MPEG kötőjel URL-cím szűrővel

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Zökkenőmentes Streaming URL-cím szűrővel

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


A tartalmak olvasása, és hozza létre, a folyamatos átvitelű URL-címek kapcsolatos további tudnivalókért olvassa el a [kézbesítése tartalom áttekintése](media-services-deliver-content-overview.md)című témakört.


>[AZURE.NOTE]Figyelje meg, dinamikus jegyzékfájlok: ne módosítsa az eszközt, és az alapértelmezett jegyzék, hogy az eszköz. Az ügyfél vagy a szűrők nélkül adatfolyam kérése választhatja. 


###<a id="filters"></a>Szűrők 

Az eszköz szűrők két típusa van: 

- Globális szűrők (bármely eszköz az Azure Media Services számlán alkalmazhatók, a fiók élettartamának van), és 
- Helyi szűrő (csak egy eszköz, amellyel a szűrő lett társított létrehozás után alkalmazhatók, egy, az eszköz élettartama). 

Globális és a helyi szűrő-típusok van pontosan ugyanazt a Tulajdonságok parancsot. A két fő különbségét érték melyik felhasználási területei egy filer típusának megfelelő. Globális szűrők alkalmasak általában eszköz profilok (a képmegjelenítés szűrés) hol helyi szűrők felhasználható levághatja egy bizonyos eszközt.


##<a id="scenarios"></a>Gyakori alkalmazási területek 

Lett említett, mielőtt a tartalmakat (az élő eseményeket vagy igény szerinti videó adatfolyam) ügyfelek során a célja egy jó minőségű videó végzi a különböző hálózati feltételek különféle eszközökön. Ezenkívül a elképzelhető, hogy más, amely magában foglalja a szűrést az eszközök és **Dinamikus cikkét**s használatával követelményeik vannak. Az alábbi szakaszok különböző szűrési forgatókönyvek egy rövid áttekintést ad.

- Adja meg, bizonyos eszközök kezelhető (helyett az összes a megjelenítések az eszköz társított) hang- és megjelenítések csak egy részét. 
- Lejátszás csak egy videoklip (helyett a teljes videó lejátszása) szakaszt.
- Állítsa be a DVR bemutató ablakot.

##<a name="rendition-filtering"></a>A képmegjelenítés szűrése 

Előfordulhat, hogy az eszköz több kódolási profilok (H.264 eredeti H.264 magas, AACL, AACH, Dolby Digital Plus), és több minőség bitrates kódolását választhatja. Azonban nem az összes ügyféleszközök támogatják az eszköz profilok és bitrates. Ha például régebbi Android-eszközön csak a H.264 eredeti + AACL támogatja. A sávszélesség és eszköz kiszámítása magasabb bitrates küld egy eszközt, amely nem tudják elérni az előnyökkel jár, hulladékok. Az ilyen eszköz kell dekódolását minden megadott információt, csak ha át kívánja méretezni a megjelenítését.

A dinamikus cikkét eszköz profilok hozhat létre például a mobil, konzol, HD/Ft, stb, és a számok és minőségű, amely az egyes profilok egy részét szeretné.

 
![A képmegjelenítés szűrési példa][renditions2]

A következő példa egy kódoló használt kódolását egy Emeletes eszköz be hét ISO MP4s videó megjelenítések (a 180p billentyűkombinációval 1080p). A kódolt eszköz dinamikusan csomagolt sem a következő adatfolyam protokollok: HLS, sima, MPEG szaggatott és HDS.  A diagram tetején jelenik meg az eszköz nincs szűrőkkel a HLS jegyzék (benne minden hét megjelenítések).  A bal alsó sarokban a HLS jegyzék, amelyhez "ott" nevű szűrőt alkalmaztak jelenik meg. A "ott" szűrő Itt adhatja meg, ha el szeretné távolítani az összes bitrates 1Mbps, az alsó minőség kétszintű éppen hivatkozásból a válasz eredményező alatt.  A jobb alsó sarokban a HLS jegyzék, amelyhez "mobil" nevű szűrőt alkalmaztak jelenik meg. A "mobil" szűrő Itt adhatja meg, ha el szeretné távolítani a megjelenítések 720p, ami a két nagyobb a felbontás esetén 1080p megjelenítések éppen hivatkozásból.

![A képmegjelenítés szűrése][renditions1]

##<a name="removing-language-tracks"></a>Távolítson el nyelvi számok

Az eszközök, például angol, spanyol, francia, stb hang többnyelvű tartalmazhatnak olyan. Általában Player SDK felettesét alapértelmezett hangsáv kijelölés, és elérhető hang nyomon követi, egy felhasználó kiválasztása. Nehéz ilyen Player SDK fejlesztése, különböző megvalósítás igényel eszköz-specifikus player-keretei között. Is bizonyos platformokon Player API-khoz korlátozódik, és arról, hogy a felhasználók hol nem jelölje be vagy módosítsa az alapértelmezett hangsáv hang kijelölés funkció nem tartalmazzák. Az eszköz szűrőkkel szabályozhatja, hogy a vezérlő viselkedése, amely csak a kívánt hangot nyelvek felvétele szűrők létrehozásával.

![Szűrés nyelvi számok][language_filter]


##<a name="trimming-start-of-an-asset"></a>Tárgyi eszköz kezdő csonkolása 

A legtöbb élő adatfolyam események operátorok futtatása néhány teszteket a tényleges esemény előtt. Előfordulhat, hogy például tartoznak egy lappal jelennek meg az esemény megkezdése előtt: "A Program pillanatnyilag fog kezdődni". A program az archiválást, ha a teszt lappal adatokat is archivált és fog szerepelni a bemutatót. Azonban ezek az információk nem megjelenjen-e az ügyfeleknek. A dinamikus cikkét kezdési idő szűrő létrehozása, és a nem kívánt adat törlése a jegyzék.

![Kezdés csonkolása][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Alszint klipek (nézetek) létrehozása egy élő archívumból

Sok élő események hosszúak fut, és élő archív tartalmazhatnak olyan több események. Az élő esemény utáni végű műsorszolgáltatók érdemes oldaltörés felfelé az élő archívum logikai programot be, és leállítja a sorozatok. Majd tegye közzé a virtuális programok külön-külön nélkül bejegyzését feldolgozása az élő archívumba, és ne hozzon létre külön eszközök (ez nem jelenik meg a meglévő gyorsítótárazott töredékek előnyeit az a CDN). Ilyen virtuális programok (alszint klipek), és melyek a labdarúgó vagy Kosárlabda mérkőzés szavakat, a innings a baseballban negyedéve vagy az olimpiai program délután az egyes események.

A dinamikus cikkét szűrők használatával kezdő és záró időpont létrehozhat és az élő archív tetején virtuális nézeteket hozhat létre. 

![Szűrő subclip][subclip_filter]

Szűrt eszköz:

![Sí][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Bemutató ablaka (DVR) beállítása

Jelenleg a Azure Media Services felajánlja a különféle körkörös archívumba, ahol beállítható az időtartam, között az 5 perc - 25 óra. A szűrés nyilvánvalóan használható hozhat létre egy közbeni DVR ablak felső részén a archívumba, az media törlése nélkül. Vannak olyan sok olyan esetek, ahol műsorszolgáltatók szeretne adni egy korlátozott DVR ablak, mely lép, az élő széllel és egyszerre tartani egy nagyobb archiválási ablakban. Egy műsorszóró érdemes használni az adatokat, amelyek kívül a DVR ablakban jelölje ki a klipek, vagy he\she érdemes különböző eszközök a különböző DVR windows szükséges. A mobileszközök a legtöbb például nagy DVR windows (lehet mobileszközökhöz és 1 óra 2 percig DVR ablak az asztali ügyfélprogramok) nem kezeli.

![DVR ablak][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>LiveBackoff (élő pozíció) beállítása

Szűrés nyilvánvalóan néhány másodpercig eltávolítása egy élő program élő széle használható. Ebben a csoportban adhatja műsorszolgáltatók megnézhessék őket a betekintő kiadvány pontot, és hozhat létre hirdetési beszúrási pontot, mielőtt a nézők fogadása az adatfolyam (általában biztonsági kikapcsolási által 30 másodperc). Műsorszolgáltatók ezután tartalomtípusban ezek hirdetések szeretné az ügyfél keretek időben, hogy a beérkezett és dolgozza fel az adatokat, mielőtt a közzététel lehetőséget.

A hirdetési támogatás mellett LiveBackoff beállítása az ügyfél élő letöltési helyére, hogy az ügyfelek eltolódás, és az élő él találati őket akkor is elérjék töredékek kiszolgálóról 404-es vagy 412 HTTP-hibák első helyett használható.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Egyetlen szűrő a több szabállyal kombinálása

Is összevonhatja egyetlen szűrő több szűrési szabályokat. Példaként határozhatja meg tartománya szabály lappal eltávolítása egy élő archívumba, és elérhető bitrates is szűrheti. Több szűrési szabályok vége eredménye a kialakítási (csak metszet) szabályok.

![több-szabályok][multiple-rules]

##<a name="create-filters-programmatically"></a>Szűrők programozás útján létrehozása

A következő témakör azt ismerteti, hogy a szűrők kapcsolódó Media Services szervezetek. A témakör azt is megtudhatja, hogy miként programozás útján hozza létre a szűrőket.  

[A REST API -k létrehozása szűrőket](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Több szűrőt (szűrés összeállítás) kombinálása

Több szűrőt egyetlen URL-kombinálhatja is. 

A következő példa bemutatja, hogy miért érdemes lehet kombinálni a szűrők:

1. Meg kell szűrésének a videó tulajdonságait, például az Android- és iPad-alapú mobileszközökön (korlátozza a videó minőségű). Ha el szeretné távolítani a felesleges minőségű, akkor globális szűrő, amely megfelelő eszköz profilok hozna létre. Fent említett globális szűrők minden további társítás nélküli azonos media services fiók csoportjában az összes eszköz használható. 
2. Le szeretné vágni a tárgyi eszköz kezdési és befejezési időpontot. Cél, akinek helyi szűrő létrehozása, és a kezdő és záró időpont beállítása. 
3. Ezek a szűrők (nélkül minőség szűrés hozzáadása a elrejtést szűrő, amely fog megnehezítik szűrő használatát kimutatásadatokat kombinációja) a egyesítésben használni kívánt.

Szűrők egyesítéséhez kell beállítania a szűrő neve a jegyzék lista pontosvesszővel tagolt URL-címe. Tegyük fel, hogy szűrők minőségű és rendelkezik-e egy adott kezdő időpont beállítása egy másik elnevezett *MyStartTime* *MyMobileDevice* nevű szűrő van. Őket kombinálhatja jelennek meg:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Legfeljebb 3 szűrőket is összevonhatja. 

További információt talál a [blogban](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) .


##<a name="know-issues-and-limitations"></a>Tudnivalók a hibákat és korlátozásokat

- Dinamikus jegyzék működik GOP határai (kulcs keretet), így csonkolása van GOP pontosságát. 
- A helyi és globális szűrőkben azonos szűrő neve is használhatja. Megjegyzés: a helyi szűrő magasabb elsőbbséget és felülírja a globális szűrőket.
- Ha frissíti a szűrőt, frissítheti a szabályok adatfolyam végpont 2 percig is eltarthat. Ha a tartalom egyes szűrőkkel lett felszolgált (és tárolja a proxykiszolgálók és CDN gyorsítótárát), ezek a szűrők frissítése okozhat player hibák. A gyorsítótárat a szűrő frissítése után ajánlott. Ha ez a beállítás nem lehetséges, akkor fontolja meg egy másik szűrőt.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Lásd még:

[Tartalom kézbesítése vevők – áttekintés](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 