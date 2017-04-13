<properties 
    pageTitle="Élő is használja az Azure Media Services áttekintése |} Microsoft Azure" 
    description="Ez a témakör áttekintést nyújt az Azure Media Services használata élő is." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Élő is az Azure Media Services használatával – áttekintés

##<a name="overview"></a>– Áttekintés

Az Azure Media Services élő adatfolyam események során az alábbi összetevőket gyakran játszik szerepet:

- Esemény közvetítése szolgáló kamera.
- Élő videó encoder, hogy a kamera jelek konvertál egy élő adatfolyam szolgáltatásnak küldött adatfolyamok.

    Ha szükséges több élő szinkronizálásakor kódolók. Az egyes kritikus élő eseményeket, hogy igény szerint nagyon magas elérhetősége és élmény minősége, célszerű alkalmaznia aktív-aktív felesleges kódolók--idő szinkronizálási zökkenőmentes feladatátvevő adatvesztés nélkül adatok eléréséhez.
- Élő adatfolyam szolgáltatás, amely lehetővé teszi, hogy tegye a következőket:
    
    - különböző élő adatfolyam protokollt használó (például RTMP vagy zökkenőmentes Streaming) élő tartalom ingest
    - (nem kötelező) kódolása a adatfolyam adaptív átviteli sebesség adatfolyam-be
    - az élő adatfolyam megtekintése
    - rögzítése és a j.leny tartalmat tároló annak érdekében, hogy újabb (igény szerinti videó) kell folyamatosan lejátszott.
    - közvetlenül az ügyfelekkel, illetve hogy a tartalom kézbesítési hálózati (CDN) további eloszlás használhat az előadáshoz keresztül közös adatfolyam protokollok (például MPEG kötőjel, sima, HLS, HDS) tartalma.


**Microsoft Azure Media Services** (AMS) lehetővé teszi a ingest, kódolását, megtekintheti, tárolása és előadása a élő adatfolyam tartalmat.

A tartalom ügyfelek során a célja egy jó minőségű videó végzi a különböző hálózati feltételek különféle eszközökön. Cél, élő kódolók segítségével a többszörös-átviteli sebesség (adaptív átviteli sebesség) video-adatfolyam adatfolyam kódolását.  Az eszközön adatfolyam gondoskodik, [dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md) Media Services segítségével dinamikusan csomag újra a különböző protokollok adatfolyam. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák kézbesítve: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak).

Az Azure Media Services, a **csatornák**, a **programok**és a **StreamingEndpoints** ingest, formázás, DVR, biztonsági, méretezhetőség és redundancia beleértve minden élő adatfolyam funkciókkal kezelheti.

A **csatorna** egy folyamat élő adatfolyam tartalom feldolgozásra jelöli. Csatorna egy élő kaphatnak a bemeneti adatfolyam megjelenítését az alábbi módon:

- Egy helyszíni meg a csatornát, hogy be van állítva az **átadó** kézbesítési élő a többszörös-átviteli sebesség **RTMP** vagy **Zökkenőmentes Streaming** (töredezett MP4) kódoló küld. **Átadó** kézbesítve esetén, a j.leny adatfolyamok haladnia **csatorna**s további feldolgozásra nélkül. A következő élő kódolók, amely több-átviteli sebesség zökkenőmentes Streaming kimeneti használható: elemi, Envivio, Cisco.  A következő élő kódolók kimeneti RTMP: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast és Tricaster transcoders.  Egy élő encoder is küldhet egy átviteli sebesség adatfolyam egy adott csatornába, amely élő kódolási nincs engedélyezve, de nem ajánlott. Kérés esetén Media Services biztosítja az adatfolyam ügyfeleknek.

    >[AZURE.NOTE] Átadó módszerrel módja a leggazdaságosabb élő adatfolyam, mivel vannak elfoglalva több eseményeket egy hosszú időn keresztül, és már fekteti helyszíni kódolók. [Árak](/pricing/details/media-services/) részletei című részben találja.
    
    
- A helyszíni élő kódoló egyetlen-átviteli sebesség adatfolyam küld a csatornát, amely élő kódolása a következő formátumokban a Media Services végrehajtásához engedélyezve van: RTMP vagy zökkenőmentes Streaming (töredezett MP4). RTP (MPEG-TS) szintén támogatottak, amennyiben van dedikált Azure adatközpontja. A következő élő kódolók RTMP kimeneti a tudjuk, hogy az adott típusú csatornák használata: Telestream Wirecast, FMLE. A csatorna hajtja végre a bejövő levelek élő kódolást többszörös-átviteli sebesség (alkalmazkodó) video-adatfolyam egyetlen átviteli sebesség adatfolyam. Kérés esetén Media Services biztosítja az adatfolyam ügyfeleknek.


Kezdve a Media Services 2.10 megjelenés csatorna létrehozásakor megadhatja milyen módon szeretné a csatornához, a bemeneti adatfolyam fogadásához, és engedélyezi vagy tiltja szeretne végrehajtani a adatfolyam élő kódolása a csatornához. Két lehetőség közül választhat:

- **Nincs lehetőség** (átadó) – adja meg ezt az értéket, ha a program kimeneti többszörös-átviteli sebesség adatfolyam (átadó adatfolyam), amely a helyszíni élő kódoló használni kívánt. Ebben az esetben a bejövő adatfolyam átadott keresztül a kimenet kódolási nélkül. Ez a megelőző 2,10 megjelenés csatorna viselkedését.  

- **Szabványos** – Ez az érték, ha a többszörös-átviteli sebesség adatfolyam egyetlen átviteli sebesség élő adatfolyam kódolása a Media Services használni kívánt választ. Ez a módszer akkor gazdaságosabb méretezés gyorsan ritka eseményekre vonatkozóan. Ügyeljen arra, hogy egy élő kódolási számlázási hatással van, és kell ne feledje, hogy élő kódolási csatorna elhagyása "Operációs rendszert futtató" állapotú merülnek fel, amelyekre számlázási díjak.  Javasoljuk, hogy azonnal leállítja a futó csatornák az élő adatfolyam esemény extra óránkénti költségek elkerülésére befejezése után. 

##<a name="comparison-of-channel-types"></a>Csatorna típusok összehasonlítása

Következő táblázat tartalmaz egy segédvonalat a Media Services támogatott csatorna kétféle összehasonlítása

A szolgáltatás|Átadó csatorna|Standard csatorna
---|---|---
Egyetlen átviteli sebesség beviteli van kódolva több bitrates a felhőben|nem|igen
Maximális felbontását, a rétegek száma|1080p, 8 rétegeket, 60 + képkocka|720p, 6 rétegeket, 30 képkocka
Beviteli protokollok|RTMP, folyamatos zökkenőmentes|RTMP zökkenőmentes Streaming és RTP
Ár|Látható, [oldal árak](/pricing/details/media-services/) , majd kattintson az "Élő Video" lapon|Lásd az [ár, oldal](/pricing/details/media-services/) 
Maximális futási idő|24 x 7|8 órát
Beszúrás palák támogatása|nem|igen
Active Directory-jelzés támogatása|nem|igen
Átadó CEA 608/708 feliratok|igen|igen
Azt jelenti, hogy a hírcsatorna hozzájárulása rövid parkolóhelyeket helyreállítása|igen|Nem (csatorna elindul slating másodperc 6 + bemeneti adatok nélküli után)
Nem egységes beviteli GOPs támogatása|igen|Nem – beviteli javítani kell 2sec GOPs
Változó keret ráta beviteli támogatása|igen|Nem – beviteli keret ráta kell oldódnia.<br/>Kis változatok is elfogadja a rendszer, például magas mozgásvonalas jelenetek során. De encoder húzással nem lehet 10 keretek/sec.
Automatikus-kikapcsolása a csatornák, amikor a szövegbeviteli hírcsatorna elvész.|nem|Ha a Program nem fut a 12 óra elteltével 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Csatornát, melyet a többszörös-átviteli sebesség élő adatfolyam kapott helyszíni kódolók (átadó) használata

Az alábbi ábra a AMS platform fő részből kapcsolódó a **átadó** munkafolyamat jeleníti meg.

![Élő munkafolyamat](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

További tudnivalókért lásd: [adott fogadási több csatornák használata-átviteli sebesség élő adatfolyam a helyszíni kódolók](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Az Azure Media Services élő kódolási végrehajtásához engedélyezett csatornák használata

A következő ábrán a AMS platform fő részből kapcsolódó Live Streaming munkafolyamatban, amelyen csatorna engedélyezve van a Media Services élő kódolást végrehajtásához.

![Élő munkafolyamat](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

További tudnivalókért olvassa el a [csatornák, hogy engedélyezve vannak végrehajtása Live kódolást az Azure Media Services használata](media-services-manage-live-encoder-enabled-channels.md)című témakört.

##<a name="description-of-a-channel-and-its-related-components"></a>A csatorna és a kapcsolódó összetevői leírása

###<a name="channel"></a>Csatorna

A Media Services [csatorna](https://msdn.microsoft.com/library/azure/dn783458.aspx)s felelősek élő adatfolyam tartalom feldolgozása. Csatorna biztosít beviteli zárólap (ingest URL-CÍMÉT), hogy ezután meg kell adnia egy élő transcoder szeretne. A csatorna élő beviteli adatfolyamok kap az élő transcoder, és egy vagy több StreamingEndpoints keresztül folyamatos számára elérhetővé teszi azt. Csatornák is biztosít egy előnézeti végpontot (előzetes verzió URL-cím) megtekintheti, és a adatfolyam érvényesítés előtt feldolgozásra és kézbesítési használt.

A csatorna létrehozásakor elérheti a ingest URL-CÍMEK és a kép URL-CÍMÉT. Úgy juthat az alábbi URL-címeket, a csatorna nem kell lenniük állapotba. Ha készen áll a kezdésre adatok küldése a egy élő transcoder be a csatornát, a csatorna kell indítani. Miután az élő transcoder elindítja az adatok ingesting, a megjelenítő Értékáram tekintheti meg.

Minden Media Services fiók több csatornák, több program elemre, és több StreamingEndpoints is tartalmazhat. Attól függően, hogy a sávszélesség és biztonsági igényeinek StreamingEndpoint szolgáltatások is dedikált egy vagy több csatornák. Bármely StreamingEndpoint bármely csatornából átteheti.


###<a name="program"></a>Program 

A [Program](https://msdn.microsoft.com/library/azure/dn783463.aspx) lehetővé teszi a közzétételi és az élő adatfolyam szegmensek tárolására. Csatorna kezelése programok. A csatorna és a Program a kapcsolat nagyon hasonlít hagyományos media, ahol csatorna állandó adatfolyam-tartalom van, és a program bizonyos csatorna a megadott eseményre megfelelően változik.
Megadhatja, hogy meg szeretné őrizni a program a rögzített tartalmak **ArchiveWindowLength** tulajdonság beállításával órák számát. Ezt az értéket kell az megadhatja legalább 5 perce legfeljebb 25 órát. 

ArchiveWindowLength is szerint idő ügyfelek legnagyobb mennyiségű aktuális élő pozíciójában vissza az időt is ismert. Programok futtatását is lehetővé teszi a megadott időmennyiséget fölé, de a tartalmat, az ablak hossza mögé helyezett folyamatosan elvész. Ez a tulajdonság értéke is határozza meg, hogy mennyi ideig is nagyobb az ügyfél-jegyzékfájlok.

Egyes programok társítva tárgyi eszköz. A program közzététele létre kell hoznia egy megnevezés társított eszköz. A megnevezés problémákat lehetővé teszi az ügyfelek lehet nyújtani adatfolyam URL-cím összeállítása.

A csatorna támogatja a legfeljebb három párhuzamosan futó programok, így az azonos bejövő adatfolyam több archívumok hozhat létre. Ez lehetővé teszi közzé, és esemény különböző részeire archiválni, szükség szerint. A vállalati követelmény például program 6 órával archiválni, de csak az utolsó 10 perc szétküldése is. Ehhez két párhuzamosan futó programok létrehozásához szükséges. Több programot van beállítva, 6 órával az esemény archiválását, de a program nincs közzétéve. A többi program 10 percig archiválása van állítva, és a program közzétételét.


##<a name="billing-implications"></a>Számlázási következmények

A csatorna megkezdi a számlázás, amint az API keresztül "Fut" állapot áttűnések.  

A következő táblázat mutatja, hogy hogyan feleltesse meg csatorna államok számlázási államok az API-val és Azure-portálon. Ne feledje, hogy az államok némileg eltér az API-val és a portálon UX között Amint egy adott csatornába állapotban "Futásának" keresztül az API-val, vagy az Azure-portálon "Kész" vagy "Adatfolyam" állapotban van, a számlázási aktív lesz.

A csatorna számlázással, további leállításához van le szeretné állítani a csatornát, vagy az Azure-portálon keresztül az API-t.
Ön a felelős a csatornák leállítás, ha be szeretné fejezni a csatornához. Hiba le szeretné állítani a csatorna folyamatos számlázási eredményezi.

>[AZURE.NOTE]Szabványos csatorna-használatakor AMS fog automatikus kikapcsolása bármelyik csatorna, amely a 12 óra, a bemeneti hírcsatorna nem vesznek el, és nincsenek futó programok után továbbra is "Operációs rendszert futtató" állapotban van. Azonban továbbra is számlázott "Operációs rendszert futtató" állapotban volt a csatorna alkalommal.

###<a id="states"></a>Csatorna államok, és hogyan azok feleltesse meg a számlázási mód 

A csatorna aktuális állapota. Lehetséges értékek a következők:

- **Leállítva**. Ez a csatorna Alapállapot annak létrehozása után (kivéve, ha az automatikus indítás kijelölve a portálon.) Nincs számlázási fordul elő, az Ez az állapot. Ezt az állapotot a csatorna tulajdonságainak frissíthető, de a folyamatos átvitelű visszacsatolás nem engedélyezett.
- A **kezdési**. A csatorna indítása folyamatban van. Nincs számlázási fordul elő, az Ez az állapot. Nincs frissítés vagy az Ez az állapot alatt a folyamatos átvitelű engedélyezett. Hiba történik, ha a csatorna leállítva állapotba adja eredményül.
- A **futó**. A csatorna is alkalmas élő adatfolyamok feldolgozása. Akkor van most számlázás használatát. Le kell állítania a csatornát, hogy a további számlázási. 
- **Leállítása**. A csatorna leállt. Nincs számlázási fordul elő, az Ez az állapot ideiglenes (tranziens). Nincs frissítés vagy az Ez az állapot alatt a folyamatos átvitelű engedélyezett.
- A **Törlés**. A csatorna törlődik. Nincs számlázási fordul elő, az Ez az állapot ideiglenes (tranziens). Nincs frissítés vagy az Ez az állapot alatt a folyamatos átvitelű engedélyezett.

A következő táblázat mutatja, hogy hogyan csatorna államok feleltesse meg a számlázási módot. 
 
Csatorna állam|A jelölők portál felhasználói felület|Számlázási található ez?
---|---|---
Indítása|Indítása|Nincs (tranziens állapot)
Fut|Készen áll a (nincs futó programot)<br/>vagy<br/>Adatfolyam (legalább egy futó program)|igen
Leállítása|Leállítása|Nincs (tranziens állapot)
Leállítva|Leállítva|nem


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Kapcsolódó témakörök

[Azure Media Services töredezett MP4 Live Ingest meghatározása](media-services-fmp4-live-ingest-overview.md)

[Az Azure Media Services végrehajtása Live kódolást engedélyezett csatornák használata](media-services-manage-live-encoder-enabled-channels.md)

[Többszörös-átviteli sebesség élő adatfolyam kapott helyszíni kódolók csatornák használata](media-services-live-streaming-with-onprem-encoders.md)

[Kvóták és korlátozásokat](media-services-quotas-and-limitations.md).  

[Media Services fogalmak](media-services-concepts.md)
