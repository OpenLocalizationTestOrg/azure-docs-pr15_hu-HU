<properties 
    pageTitle="Azure Media Services fogalmak |} Microsoft Azure" 
    description="Ez a témakör áttekintést nyújt az Azure Media Services fogalmak" 
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

#<a name="azure-media-services-concepts"></a>Azure Media Services fogalmak 

Ez a témakör áttekintést nyújt a legfontosabb Media Services fogalmak.

##<a id="assets"></a>Eszközök és tárolása

###<a name="assets"></a>Eszközök

Egy [Tárgyi eszköz](https://msdn.microsoft.com/library/azure/hh974277.aspx) digitális fájlokat (beleértve a videó, hang, képek, miniatűrök gyűjtemények, szöveg, számok és kódolt felirat fájlok) és a metaadatokat arról, hogy ezeket a fájlokat tartalmazza. Miután a digitális fájlok feltöltése az eszköz befejeződik, azok felhasználható a Media Services kódolásának és a folyamatos átvitelű munkafolyamatok.

Tárgyi eszköz blob-tárolóhoz Azure tároló fiók van megfeleltetve, és BLOB-tároló a következőképpen tárolja a fájlokat az eszköz.

Milyen Médiatartalom feltöltése és tárolása az eszköz kiválasztásakor alkalmazni a következő szempontokat mérlegelve:

- Egy tárgyi eszköz csak egyetlen, egyedi példánya médiatartalom kell tartalmaznia. Például a egyetlen módosítás TV rész, film, vagy a hirdetési.
- Egy tárgyi eszköz nem tartalmazhat több megjelenítések vagy audiovizuális fájlban a módosításokat. Egy tárgyi eszköz helytelen használatát egy példa volna próbál a egynél több TV rész, hirdetési vagy több kamera többszöröseire belül tárgyi eszköz egy gyártási. Több megjelenítések vagy audiovizuális fájlban a szerkesztések tárolása tárgyi eszköz kódolási feladatok, a folyamatos átvitelű, és később a dokumentumazonosítókkal a munkafolyamat kézbesítve biztonságossá tétele elküldése nehézségeket okozhat.  

###<a name="asset-file"></a>Digitáliseszköz-fájl 
Egy [AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) jelöl egy video- vagy fájl, amely egy blob-tárolóban található. Egy tárgyi eszköz fájl mindig társított tárgyi eszköz, és tárgyi eszköz egy vagy több fájlt tartalmaz. A Media Services Encoder tevékenység sikertelen lesz, ha az eszköz fájlt objektumként program nem társítja blob-tároló digitális fájl.

A **AssetFile** példány és a tényleges médiafájl két különálló objektummal. AssetFile példány, a multimédiás fájl metaadatokat tartalmaz, miközben a multimédiás fájl tartalmaz a tényleges médiatartalom.

Nem megpróbálja kell Media szolgáltatás API-k használata nélkül Media Services által létrehozott blob-tárolók tartalmának módosítása.

###<a name="asset-encryption-options"></a>Digitáliseszköz-titkosítás beállítások

Attól függően, hogy milyen típusú tartalmat szeretne feltölteni, tárolni és olvasása Media Services itt különböző titkosítási közül választhat.

**Nincs lehetőség** Titkosítás nem használatos. Az alapértelmezett értéket. Figyelje meg, hogy ez a beállítás használata esetén a tartalom nem védett a hálózaton átvitt vagy tárolóban lévő többi.

Ha szeretne egy MP4 előadása használata a progresszív letöltésére, használja ezt a lehetőséget a tartalmak feltöltése.

**StorageEncrypted** – titkosítja a helyi meghajtóra titkosítással AES 256 bit törlése tartalom használja ezt a lehetőséget, és töltse fel a Azure tárolóhoz tárolási helyére a többi titkosítva. Tárterület-titkosítással védett eszközök automatikusan titkosítatlan és egy titkosított fájlrendszer előtt kódolást helyezett, és tetszés szerint újra titkosított egy új kimeneti eszközként feltöltése előtt. Az elsődleges használatieset az tárterület-titkosítás esetén, a kiváló minőségű beviteli médiafájlokat a többi titkosítású a lemezen védeni kívánt. 

Annak érdekében, hogy a tárhely titkosított eszköz olvasása, meg kell adnia az eszköz kézbesítési házirendet, így Media Services megtudhatja, hogyan kívánja-e a tartalmak olvasása. Az eszköz folyamatosan lejátszott is kell, mielőtt a továbbított kiszolgáló eltávolítja a tárterület-titkosítás, és adatfolyamként továbbítja a tartalom a házirenddel megadott kézbesítési (például AES, PlayReady vagy titkosítás nélkül). 

**CommonEncryptionProtected** – ezt a lehetőséget, ha azt szeretné, titkosítása (vagy tölthet fel, már titkosított) használata közös titkosítási vagy PlayReady DRM (például zökkenőmentes Streaming látták el PlayReady DRM) tartalma.

**EnvelopeEncryptionProtected** – ezt a lehetőséget, ha azt szeretné, védelme (vagy tölthet fel, már védett) használata HTTP Live Streaming (HLS) titkosított a speciális Encryption Standard (AES). Figyelje meg, hogy már titkosított AES HLS vannak feltöltése, ha meg kell titkosított átalakítása menedzser.

###<a name="access-policy"></a>Hozzáférési házirend 

Egy [AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) az eszköz (például olvasható, írást és lista) engedélyek és hozzáférés időtartama határozza meg. Az objektum AccessPolicy általában egy megnevezés, majd az eszköz tárolt fájlok eléréséhez használandó szeretné átadni.


###<a name="blob-container"></a>BLOB-tárolóhoz

Blob-tároló tartalmaz egy sor olyan BLOB csoportja. BLOB tárolók segítségével a Media Services oszlopazonosító pontként hozzáférés-vezérlés, és a megosztott Access-aláírás (Társítások) Locator eszközein. Azure tároló fiók blob tárolók tetszőleges számú is tartalmazhat. A tároló BLOB tetszőleges számú tárolhat.

>[AZURE.NOTE]Nem megpróbálja kell Media szolgáltatás API-k használata nélkül Media Services által létrehozott blob-tárolók tartalmának módosítása.

###<a id="locators"></a>Locator

A szükséges tárgyi eszköz tárolt fájlok eléréséhez belépési pontjához [Megnevezés](https://msdn.microsoft.com/library/azure/hh974308.aspx)s. Az access-házirend határozza meg, hogy egy ügyfél fér hozzá egy adott eszköz időtartam és engedélyek szolgál. Locator beállíthatja, hogy több-a-hozzáférési házirend egy kapcsolatot, hogy az eltérő Locator lehet nyújtani más kezdési idő és a kapcsolatok típusai eltérő ügyfelek használatakor az összes az azonos jogosultsági és az időtartam beállítások; azonban Azure tárterület-szolgáltatások beállítása megosztott hozzáférési házirend korlátozás, miatt nem lehet egyszerre egy adott eszköz társított legfeljebb öt egyedi Locator. 

Media Services támogatja a kétféle Locator: fel- vagy letöltése media fájlok to\from Azure tároló használt OnDemandOrigin Locator, adatfolyam-media (például MPEG kötőjel, HLS vagy zökkenőmentes Streaming), vagy fokozatosan a média és Társítások URL-cím Locator, letöltéséhez használt. 

Fontos tudni, hogy a (AccessPermissions.List) engedéllyel nem alkalmazható egy OrDemandOrigin megnevezés létrehozásakor. 

###<a name="storage-account"></a>Tárterület-fiók

Az összes access Azure tárolóhoz befejeződött tároló fiókon keresztül. A multimédia-szolgáltatási fiók egy vagy több tárterületet fiókok társíthat. Fiók tárolók, tetszőleges számú is tartalmazhatnak, mindaddig, amíg a teljes méret csoportban 500TB per tárterület-fiók.  Media Services itt SDK szintű szerszámok engedélyezni, hogy több tárterületet fiókok kezelése és terheléselosztó eszközeit eloszlását ezekben a fiókokban való feltöltés közben a mértékek vagy véletlen eloszlás alapján. További tudnivalókért olvassa el a [Azure tároló](https://msdn.microsoft.com/library/azure/dn767951.aspx)használata című témakört. 

##<a name="jobs-and-tasks"></a>Projektek és tevékenységek

Egy [feladat](https://msdn.microsoft.com/library/azure/hh974289.aspx) felhasználási módjuk látható folyamat (például index vagy kódolását) egy hang-és videoadat-bemutatóban. Több videó feldolgozása esetén, a minden videó kell-e kódolt feladat létrehozása.

A feladat elvégzendő feldolgozása metaadatait tartalmazza. Minden feladat adja meg, hogy egy atomi feldolgozási tevékenység, a bemeneti fekteti kimeneti eszközök, egy media processzor és kapcsolódó beállításait egy vagy több [feladat](https://msdn.microsoft.com/library/azure/hh974286.aspx)s tartalmazza. A projekt tevékenységeinek is lehet módszere során együtt, ahol a kimeneti tárgyi eszköz egy tevékenység rendszer azért adja ezt a bemeneti eszköz a következő feladatra. Ezzel a módszerrel az összes szükséges media bemutató feldolgozása egy feladatot is tartalmazhat.

##<a id="encoding"></a>Kódolás

Azure Media Services a médiafájlokat a felhőben kódolási több lehetőséget nyújt.

Ha a Media Services kezdi az egyesítést, célszerű a kodekekről és fájlformátumot közötti különbség.
Kodekek alkalmazza a tömörítési és kibontási algoritmusok, mivel fájlformátumok, tartsa lenyomva az ujját a tömörített videó tárolók szoftver.

Media Services dinamikus csomagolása, amely lehetővé teszi a adaptív átviteli sebesség MP4 vagy zökkenőmentes adatfolyam-címként kódolt tartalmat a folyamatos átvitelű Media Services (MPEG kötőjel HLS, zökkenőmentes Streaming, HDS) által támogatott továbbít nélkül újra be ezeket a folyamatos átvitelű formátumok csomag ismertetése

[Dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md)kihasználhatja kell tegye a következőket:

- Emeletes (forrás) fájlját kódolását adaptív átviteli sebesség MP4 vagy adaptív átviteli sebesség zökkenőmentes adatfolyam-fájlokat (a kódolási lépéseket, később az oktatóprogram vannak igazolni) alakítja.
- Szerezze be legalább egy igény szerinti adatfolyam egység az adatfolyam végpontot, amelyből tervezi kézbesítési a tartalmakat. További információért tájékozódhat [skála igény szerinti Streaming fenntartott egységek](media-services-portal-manage-streaming-endpoints.md).

Igény szerint kódolók, a jelen cikkben ismertetett Media Services támogatja a következő:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder prémium munkafolyamat](media-services-encode-asset.md#media-encoder-premium-workflow)

Támogatott kódolók kapcsolatos információkért olvassa el a [kódolók](media-services-encode-asset.md)című témakört.


##<a name="live-streaming"></a>Élő adatfolyam

Azure Media Services csatorna jelöl egy folyamat feldolgozásához élő adatfolyam tartalmat. A csatorna élő beviteli adatfolyamok fogadása két módszer egyikével:

- A helyszíni élő kódoló többszörös-átviteli sebesség RTMP vagy zökkenőmentes Streaming (tördelt MP4) küld a csatornához. A következő élő kódolók, amely több-átviteli sebesség zökkenőmentes Streaming kimeneti használható: elemi, Envivio, Cisco. A következő élő kódolók kimeneti RTMP: Adobe Flash Live Telestream Wirecast és Tricaster transcoders. A j.leny adatfolyamok csatornákon keresztül anélkül, hogy minden további feldolgozásra adják át. Kérés esetén Media Services biztosítja az adatfolyam ügyfeleknek.

- Egy átviteli sebesség adatfolyam (a, a következő formátumokban: RTP (MPEG-TS)), RTMP vagy zökkenőmentes Streaming (tördelt MP4)) a rendszer elküldi a csatornát, amely élő kódolása a Media Services végrehajtásához engedélyezve van. A csatorna hajtja végre a bejövő levelek élő kódolást többszörös-átviteli sebesség (alkalmazkodó) video-adatfolyam egyetlen átviteli sebesség adatfolyam. Kérés esetén Media Services biztosítja az adatfolyam ügyfeleknek.

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


További tudnivalókért lásd:

- [Az Azure Media Services végrehajtása Live kódolást engedélyezett csatornák használata](media-services-manage-live-encoder-enabled-channels.md)
- [Többszörös-átviteli sebesség élő adatfolyam kapott helyszíni kódolók csatornák használata](media-services-live-streaming-with-onprem-encoders.md)
- [Kvóták és korlátozásokat](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Tartalom védelme

###<a name="dynamic-encryption"></a>Dinamikus titkosítás:

Azure Media Services lehetővé teszi a médiafájlokat a számítógépre tárhely, kezelését és kézbesítési elhagyja időpontot biztonságos. Media Services lehetővé teszi a titkosított dinamikusan speciális Encryption Standard (AES) (128 bites titkosítási kulcs használatával) és a gyakori titkosítás (CENC) PlayReady és/vagy Widevine DRM használata tartalmak olvasása. Media Services is tartalmaz AES kulcsok és a licencek PlayReady hivatalos ügyfelek számára a kézbesítési szolgáltatás.

Jelenleg az alábbi adatfolyam-formátumokat titkosíthatja: HLS MPEG szaggatott és zavartalan Streaming. Nem titkosíthatja streaming format HDS, vagy az progresszív.

Ha azt szeretné, a Media Services titkosítása tárgyi eszköz, kell társítani a titkosítási kulcs (CommonEncryption vagy EnvelopeEncryption) a digitális eszköz kiválasztása és is beállíthatja az billentyű engedélyezési házirendeket.

Ha szeretne egy tároló titkosított digitáliseszköz-adatfolyam, meg kell adnia az eszköz kézbesítési házirend annak érdekében, hogy adja meg, hogyan használhat az előadáshoz a digitális eszköz kiválasztása.

Adatfolyam a Windows Media Player kérésekor Media Services használja a megadott kulcs dinamikusan titkosítása a használata egy boríték (a AES) vagy közös titkosítást (PlayReady vagy Widevine) tartalma. Visszafejteni az adatfolyam, a Windows Media player felkéri a kulcsot a fő kézbesítési szolgáltatásból. Döntse el, vagy sem a felhasználó jogosult a kulcs első, a szolgáltatás kiértékeli a kulcs megadott engedélyezési házirendek.


###<a name="token-restriction"></a>Jogkivonat korlátozás

A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: Nyissa meg a, a korlátozás, és IP-korlátozás token. A token korlátozott házirend által egy biztonságos jogkivonat szolgáltatás (STS) kiadott token mellékelni kell. Media Services tokenek támogatja az egyszerű webes tokenek (SWT) és JSON webes jogkivonat (JWT) formátum. Media Services nem biztonságos jogkivonat szolgáltatást biztosít. Hozzon létre egy egyéni STS, vagy a Microsoft Azure ACS kihasználhatja a probléma tokenek. A STS kell beállítania, hogy hozzon létre egy jogkivonat, a megadott billentyű és a probléma követelések, a token korlátozás konfigurációban megadott aláírva. A Media Services fő kézbesítési szolgáltatás a kért kulcs (vagy licenccel) ad vissza az ügyfél, ha a token érvényes és követelések e konfigurálva jogkivonat megegyeznek a kulcs (vagy a licencet).

Ha házirend beállítása a token korlátozott, meg kell adnia az igazolás elsődleges kulcsot, a kibocsátó és a közönség paraméterek. Az ellenőrzés elsődleges kulcs, tartalmazza, a kulcs, amely a token van aláírva kibocsátó a biztonságos jogkivonat-szolgáltatás, amely a token problémák. A közönség felé (más néven hatókör) a token szándékának mutatja be, vagy az erőforrás a token engedélyezi a hozzáférést. A Media Services fő kézbesítési szolgáltatás ellenőrzi, hogy ezeket az értékeket a token értékekre a sablonban.

További információt az alábbi cikkekben talál:

[Tartalmának áttekintését védelme](media-services-content-protection-overview.md)
[védelme a AES-128](media-services-protect-with-aes128.md)
[a DRM védelme](media-services-protect-with-drm.md)

##<a name="delivering"></a>Előadásához

###<a id="dynamic_packaging"></a>Dinamikus csomagolása

Való munkavégzés során a Media Services kódolását Emeletes fájljait egy adaptív átviteli sebesség MP4 az ajánlott beállítása, és alakítsa át megadása használata a [Dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md)kívánt formátumra.


###<a name="streaming-endpoint"></a>A folyamatos átvitelű végpont

Egy StreamingEndpoint jelöl, amely közvetlenül a lejátszó ügyfélalkalmazás, vagy hogy a tartalom kézbesítési hálózati (CDN) további eloszlás (Azure Media Services most biztosít az Azure CDN-integráció.) tartalma tartana adatfolyam szolgáltatás A kimenő adatfolyam StreamingEndpoint szolgáltatásból lehet élő adatfolyam vagy egy videót arról, hogy igény szerint a digitális eszköz kiválasztása a Media Services-fiókjában. Ezeken kívül szabályozhatja, hogy miként kezelje a növekvő sávszélesség igényeinek eredményhez tartozó skála egységek (más néven adatfolyam egységek) a StreamingEndpoint szolgáltatás kapacitása. Ajánlott lefoglalhat egy vagy több skála egységek alkalmazások üzemi környezetben. Átméretezési egysége adja meg, amely az időosztáson 200 MB vásárolhatja dedikált kilépési kapacitás és a további funkció használata a dinamikus összecsomagolása jelenleg tartalmazó.

Ajánlott, dinamikus csomagolást és/vagy dinamikus-titkosítás használatára. Ezek a szolgáltatások használatához rendelkeznie kell legalább egy adatfolyam mennyisége, az adatfolyam-tervezi végpontot. További tudnivalókért lásd: a [Méretezés streaming egységek](media-services-portal-manage-streaming-endpoints.md).

Alapértelmezés szerint legfeljebb 2 végpontjait a Media Services-fiókjában streaming is. Magasabb határérték kérni, olvassa el a [kvótáinak és korlátai](media-services-quotas-and-limitations.md)című témakört.

Vannak csak számlát kapni, amikor a StreamingEndpoint állam futtatása.

###<a name="asset-delivery-policy"></a>A digitális eszköz kiválasztása kézbesítési házirend

[Kézbesítési házirendek eszközök ](https://msdn.microsoft.com/library/azure/dn799055.aspx)kell folyamatosan lejátszott kívánt konfigurálja a munkafolyamaton belül a Media Services tartalomkézbesítési lépések egyikét. Az eszköz kézbesítési házirend alapján Media Services hogyan szeretné az eszközre, kézbesítése: be kell a digitális eszköz kiválasztása lehet dinamikusan csomagolt (az például MPEG kötőjel, HLS, zökkenőmentes Streaming vagy az összes), attól függetlenül, hogy az eszköz dinamikusan titkosítani kívánt, és hogyan mely streaming jegyzőkönyv (a borítékon vagy a közös titkosítási).

Ha tároló titkosított eszköz, ahhoz is megkapják a digitális eszköz kiválasztása, a adatfolyam-kiszolgáló eltávolítja a tárterület-titkosítás és adatfolyamként továbbítja a tartalom, a megadott kézbesítési házirend használata. Például a titkosított titkosítókulcs Advanced Encryption Standard (AES) eszköz előadásához, állítsa a házirend típusa DynamicEnvelopeEncryption. Tárterület titkosításának eltávolítása és törlése a eszköz adatfolyam, állítsa a házirend típusa NoDynamicEncryption.

###<a name="progressive-download"></a>Progresszív letöltés

Progresszív letöltés lejátszásához media előtt a teljes fájl letöltődött teszi lehetővé. Fokozatosan csak töltheti le egy MP4-fájlt.

Figyelje meg, hogy kell visszafejtése titkosított eszközök, ha másnak szeretné fokozatos tölthetők.

A progresszív letöltés URL-címeit, hogy a felhasználók, először létre kell hoznia egy OnDemandOrigin megnevezés. Hozza létre a Megnevezés, biztosít az alap elérési útja az eszközt. Ezután kell hozzáfűző MP4 fájl nevét. Példa:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Adatfolyam URL-címei

Adatfolyam-ügyfelek számára a tartalmat. A folyamatos átvitelű URL-címeit, hogy a felhasználók, először létre kell hoznia egy OnDemandOrigin megnevezés. Hozza létre a Megnevezés, biztosít az alap elérési útja az eszköz, amely tartalmazza a kívánt adatfolyam-tartalom. Azonban engedélyezni szeretné a adatfolyamként, módosítania kell az elérési út további. A teljes URL-címet a továbbított nyilvánvalóan fájl összeállításához, a megnevezés Path érték és a jegyzék (filename.ism) kell ÖSSZEFŰZ fájl nevét. Ezután Hozzáfűzés megnevezés elérési /Manifest és a megfelelő formátumú (ha szükséges).

A tartalom is lehet adatfolyamként, SSL-kapcsolaton keresztül. Ehhez ellenőrizze, hogy a továbbított URL-címek Kiindulás HTTPS-e.

Figyelje meg, hogy akkor is csak adatfolyam SSL Ha az adatfolyam végpontot, amelyből a tartalmak olvasása után szeptember 10, 2014-es jött létre. A továbbított URL-ek a létrehozása után szeptember 10 adatfolyam végpontok alapulnak, ha az URL a "streaming.mediaservices.windows.net" (az új formátumban) tartalmaz. Adatfolyam URL-címeit tartalmazó "origin.mediaservices.windows.net" (a régi formátum) nem támogatják az SSL. Ha szeretné engedélyezni szeretné a SSL-adatfolyam az URL-címet a régi formátumban van, az új adatfolyam végpont létrehozásához. Az új adatfolyam végpontot alapján létrehozott URL használatával a adatfolyamként SSL.

Az alábbi lista adatfolyam formátumának módosítását ismerteti, és példákkal:

- Zökkenőmentes Streaming

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest


- MPEG-KÖTŐJEL

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=mpd-Time-CSF)



- Apple HTTP Live adatfolyam (HLS) V4

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)



- Apple HTTP Live adatfolyam (HLS) V3

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

- HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak)

{a folyamatos átvitelű végpontjának neve-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=f4m-f4f)


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
