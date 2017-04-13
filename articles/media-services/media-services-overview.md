<properties 
    pageTitle="Azure Media Services – áttekintés és a gyakori alkalmazási területek |} Microsoft Azure" 
    description="Ez a témakör áttekintést nyújt az Azure Media Services" 
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
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Azure Media Services – áttekintés és tipikus esetei

Microsoft Azure Media Services felhőalapú egy bővíthető platform, amely lehetővé teszi, hogy a fejlesztők számára méretezhető media kezelési és kézbesítési alkalmazások. Media Services REST API-k, amelyekkel biztonságosan tölthet fel, tárolása, kódolni és igény szerinti és is élő adatfolyam kézbesítési különböző ügyfelek számára (például TV PC-re és mobileszközökre) video- vagy tartalmának csomag alapul.

Végpont munkafolyamatok teljesen Media Services használatával hozhat létre. A munkafolyamat egyes részeinek külső összetevők használandó is választhat. Például kódolását egy harmadik fél encoder használatával. Töltse védelme, csomag, előadása a Media Services használatával.

A tartalom live vagy a tartalmak olvasása igény adatfolyam választhatja. Ez a témakör bemutatja az [aktív](media-services-overview.md#live_scenarios) tartalom vagy [igény](media-services-overview.md#vod_scenarios)előadásához esetei. Kapcsolódó témakörök is kapcsolódik a témakört.

## <a name="sdks-and-tools"></a>SDK és eszközök

Media Services megoldások létrehozásához használható:

- [Media Services REST API-val](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- A rendelkezésre álló ügyfél SDK egyikét:
- [Azure Media Services SDK a .NET rendszerhez](https://github.com/Azure/azure-sdk-for-media-services)
- [Azure SDK Java](https://github.com/Azure/azure-sdk-for-java),
- [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
- [Node.js az Azure Media Services](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Ez a egy Node.js SDK egy-Microsoft verzióját. Azt a Közösség üzemeltet, és jelenleg nem rendelkezik a 100 %-os körét a AMS API-k).
- Meglévő eszközök:
- [Azure portál](https://portal.azure.com/)
- [Azure-multimédia-szolgáltatások-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) egy Winforms és C# alkalmazás Windows rendszerhez)

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

Tanulási javaslatok itt AMS tekintheti meg:

- [AMS Live adatfolyam munkafolyamat](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [A folyamatos átvitelű munkafolyamat igény AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Előfeltételek

Azure Media Services használatának megkezdéséhez rendelkeznie kell a következőket:

3. Az Azure-fiók. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com).
2. Az Azure Media Services fiók. Az Azure portal, .NET vagy REST API segítségével létrehozása az Azure Media Services fiókot. További tudnivalókért olvassa el a [Fiók létrehozása](media-services-portal-create-account.md)című témakört.
3. (Nem kötelező) Fejlesztői környezet beállítása Válassza a fejlesztői környezet .NET vagy a REST API-t. További tudnivalókért olvassa el a [környezet beállítása](media-services-dotnet-how-to-use.md)című témakört.

Ezenkívül megtudhatja, hogy miként csatlakozás programozás útján [Csatlakozás](media-services-dotnet-connect-programmatically.md).
4. (Ajánlott) Egy vagy több skála egységek lefoglalhat. Ajánlott lefoglalhat egy vagy több skála egységek alkalmazások üzemi környezetben.   További tudnivalókért olvassa el a [a folyamatos átvitelű végpontok kezelése](media-services-portal-manage-streaming-endpoints.md)című témakört.

##<a name="concepts-and-overview"></a>Fogalmak és – áttekintés

Az Azure Media Services fogalmak megértéséhez olvassa el a [fogalmak](media-services-concepts.md)című témakört.

Ennek sorozat, amely bemutatja az Azure Media Services fő összetevői [Azure Media Services lépésenkénti oktatóprogramok](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series)című témakör tartalmaz. Sorozat van fogalmak áttekintés, és használja a AMSE eszköz AMS feladatok bemutatására. Ne feledje, hogy AMSE eszköz Windows eszköz. Ez az eszköz támogatja a legtöbb érhet el programozás útján [AMS SDK a .NET rendszerhez](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK Java](https://github.com/Azure/azure-sdk-for-java)vagy [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)feladatot.

##<a id="vod_scenarios"></a>Igény szerint Media az Azure Media Services előadásához: gyakori esetek és feladatok

Ez a témakör ismerteti a gyakori alkalmazási területek, és megfelelő témakörökre mutató hivatkozásokat tartalmaz. Az alábbi ábra jeleníti meg a fő részből a Media Services platform kapcsolódó előadásához igény tartalom. 

![VoD munkafolyamat](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Tárhely a tartalom védelme és törlése a adatfolyamok előadása (titkosítatlan)

1. Egy tárgyi eszköz egy jó minőségű Emeletes fájl feltölteni.
    
    A digitális eszköz kiválasztása annak érdekében, hogy a tartalom védelme alatt a feltöltési és a többi tárolóban lévő tároló titkosítási beállítás alkalmazása ajánlott.
 
1. Kódolás adaptív átviteli sebesség MP4-fájlokat. 

    Ajánlott tároló titkosítási beállítás alkalmazása a kimeneti eszköz a többi a tartalom védelme érdekében.
    
1. Állítsa be az eszköz kézbesítési házirend (dinamikus összecsomagolása által használt). 
    
    Ha az eszköz titkosított tárhely, be **kell** konfigurálása eszköz kézbesítési házirend. 

1. Az eszköz közzététele egy OnDemand megnevezés létrehozásával.

    Győződjön meg arról, hogy legalább egy, a folyamatos átvitelű fenntartott egységre, amelyhez képest adatfolyam tartalom adatfolyam végpont.

1. Adatfolyam közzé tartalmat.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>A tárhely a tartalom védelme, dinamikusan titkosított adatfolyam olvasása  

Ahhoz, dinamikus-titkosítás használatára, először a titkosított adatfolyamként szeretné adatfolyam végpont előbb be kell szereznie legalább egy adatfolyam fenntartott egység.

1. Egy tárgyi eszköz egy jó minőségű Emeletes fájl feltölteni. Az eszköz tároló titkosítási beállítás vonatkoznak.
1. Kódolás adaptív átviteli sebesség MP4-fájlokat. A kimenet eszköz tároló titkosítási beállítás vonatkoznak.
1. Tartalom titkosítókulcs lejátszása közben dinamikusan titkosítani kívánt eszköz létrehozása.
2. Állítsa be a tartalom főbb engedélyezési házirend.
1. Állítsa be az eszköz kézbesítési házirend (dinamikus csomagolást és dinamikus titkosítást által használt).
1. Az eszköz közzététele egy OnDemand megnevezés létrehozásával.
1. Adatfolyam közzé tartalmat. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Médiafájlok Analytics segítségével értekezletekre háttérismeretek származtatása a videók 

Médiafájlok Analytics megkönnyítheti a szervezetek és a videofájlok az értekezletekre háttérismeretek forrásául vállalkozások beszéd- és látás összetevők gyűjteménye. További tudnivalókért olvassa el az [Azure Media Services Analytics – áttekintés](media-services-analytics-overview.md)című témakört.

1. Egy tárgyi eszköz egy jó minőségű Emeletes fájl feltölteni.
2. A következő Media Analytics-szolgáltatások segítségével a videók feldolgozása:
    
    - **Indexelő** – [az Azure Media indexelő 2 folyamat videók](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** – [Hyperlapse médiafájlok az Azure Media Hyperlapse](media-services-hyperlapse-content.md)
    - **Mozgásvonalas észlelése** – [Mozgás észlelése az Azure Media Analytics](media-services-motion-detection.md).
    - **Arc észlelési és arc érzelmek** – [arc és Emotion észlelése az Azure Media Analytics](media-services-face-and-emotion-detection.md).
    - **Videó összefoglaló** – [Használata Azure Media videó miniatűrjét egy videó összefoglaló létrehozása](media-services-video-summarization.md)
3. Médiafájlok Analytics media processzorok konzerv MP4 vagy JSON fájlokat. Ha egy media processzor előállítása MP4-fájlként, fokozatosan letöltheti a fájlt. Ha egy media processzor JSON fájl előállítása, melyet letölthet a fájlt az Azure blob-tárolóhoz. 


###<a name="deliver-progressive-download"></a>Progresszív letöltés olvasása 

1. Egy tárgyi eszköz egy jó minőségű Emeletes fájl feltölteni.
1. Kódolását egyetlen MP4-fájlba.
1. Az eszköz közzététele egy OnDemand vagy Társítások megnevezés létrehozásával.

    OnDemand megnevezés használata esetén győződjön meg róla, hogy legalább egy, a folyamatos átvitelű fokozatosan tartalomletöltés tervezi a továbbított végpont fenntartott egység.

    Társítások megnevezés használata esetén a program a tartalmat az Azure blob-tárolóhoz letölteni. Ebben az esetben, nem kell lennie a folyamatos átvitelű fenntartott egységek.
  
1. Fokozatosan a tartalom letöltését.

##<a id="live_scenarios"></a>Az Azure Media Services élő adatfolyam események előadásához

A folyamatos átvitelű Live-használatakor az alábbi összetevőket gyakran játszik szerepet:

- Esemény közvetítése szolgáló kamera.
- Élő videó encoder, hogy a kamera jelek konvertál egy élő adatfolyam szolgáltatásnak küldött adatfolyamok.

Ha szükséges több élő szinkronizálásakor kódolók. Az egyes kritikus élő eseményeket, hogy igény szerint nagyon magas elérhetősége és élmény minősége, célszerű alkalmaznia aktív-aktív felesleges kódolók synchronizationto elérése adatvesztés nélkül adatok folytonos feladatátvevő idővel.
- Élő adatfolyam szolgáltatás, amely lehetővé teszi, hogy tegye a következőket:

- különböző élő adatfolyam protokollt használó (például RTMP vagy zökkenőmentes Streaming) élő tartalom ingest
- (nem kötelező) kódolása a adatfolyam adaptív átviteli sebesség adatfolyam-be
- az élő adatfolyam megtekintése
- rögzítése és a j.leny tartalmat tároló annak érdekében, hogy újabb (igény szerinti videó) kell folyamatosan lejátszott.
- közvetlenül az ügyfelekkel, illetve hogy a tartalom kézbesítési hálózati (CDN) további eloszlás használhat az előadáshoz keresztül közös adatfolyam protokollok (például MPEG kötőjel, sima, HLS, HDS) tartalma.


**Microsoft Azure Media Services** (AMS) lehetővé teszi a ingest, kódolását, megtekintheti, tárolása és előadása a élő adatfolyam tartalmat.

A tartalom ügyfelek során a célja egy jó minőségű videó végzi a különböző hálózati feltételek különféle eszközökön. Hangminőség és a hálózati feltételek gondoskodik élő kódolók segítségével a többszörös-átviteli sebesség (adaptív átviteli sebesség) video-adatfolyam adatfolyam kódolását.  Az eszközön adatfolyam gondoskodik, [dinamikus összecsomagolása](media-services-dynamic-packaging-overview.md) Media Services segítségével dinamikusan csomag újra a különböző protokollok adatfolyam. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák kézbesítve: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak).

Az Azure Media Services, a **csatornák**, a **programok**és a **StreamingEndpoints** ingest, formázás, DVR, biztonsági, méretezhetőség és redundancia beleértve minden élő adatfolyam funkciókkal kezelheti.

A **csatorna** egy folyamat élő adatfolyam tartalom feldolgozásra jelöli. Csatorna egy élő kaphatnak a bemeneti adatfolyam megjelenítését az alábbi módon:

- Egy helyszíni meg a csatornát, hogy be van állítva az **átadó** kézbesítési élő a többszörös-átviteli sebesség **RTMP** vagy **Zökkenőmentes Streaming** (töredezett MP4) kódoló küld. **Átadó** kézbesítve esetén, a j.leny adatfolyamok haladnia **csatorna**s további feldolgozásra nélkül. A következő élő kódolók, amely több-átviteli sebesség zökkenőmentes Streaming kimeneti használható: elemi, Envivio, Cisco.  A következő élő kódolók kimeneti RTMP: Adobe Flash Live Telestream Wirecast és Tricaster transcoders.  Egy élő encoder is küldhet egy átviteli sebesség adatfolyam egy adott csatornába, amely élő kódolási nincs engedélyezve, de nem ajánlott. Kérés esetén Media Services biztosítja az adatfolyam ügyfeleknek.

>[AZURE.NOTE] Átadó módszerrel módja a leggazdaságosabb élő adatfolyam, mivel vannak elfoglalva több eseményeket egy hosszú időn keresztül, és már fekteti helyszíni kódolók. [Árak](/pricing/details/media-services/) részletei című részben találja.

- A helyszíni élő kódoló egyetlen-átviteli sebesség adatfolyam küld a csatornát, amely élő kódolása a következő formátumokban a Media Services végrehajtásához engedélyezve van: RTP (MPEG-TS), RTMP vagy zökkenőmentes Streaming (tördelt MP4). A csatorna hajtja végre a bejövő levelek élő kódolást többszörös-átviteli sebesség (alkalmazkodó) video-adatfolyam egyetlen átviteli sebesség adatfolyam. Kérés esetén Media Services biztosítja az adatfolyam ügyfeleknek.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Csatornát, melyet a többszörös-átviteli sebesség élő adatfolyam kapott helyszíni kódolók (átadó) használata

Az alábbi ábra a AMS platform fő részből kapcsolódó a **átadó** munkafolyamat jeleníti meg.

![Élő munkafolyamat][live-overview2]

További tudnivalókért lásd: [adott fogadási több csatornák használata-átviteli sebesség élő adatfolyam a helyszíni kódolók](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Az Azure Media Services élő kódolási végrehajtásához engedélyezett csatornák használata

A következő ábrán a AMS platform fő részből kapcsolódó Live Streaming munkafolyamatban, amelyen csatorna engedélyezve van a Media Services élő kódolást végrehajtásához.

![Élő munkafolyamat][live-overview1]

További tudnivalókért olvassa el a [csatornák, hogy engedélyezve vannak végrehajtása Live kódolást az Azure Media Services használata](media-services-manage-live-encoder-enabled-channels.md)című témakört.


##<a name="consuming-content"></a>Tartalom használata más

Azure Media Services eszközeivel létre kell hoznia a sokoldalú, dinamikus player ügyfélalkalmazásokban többek között a legtöbb platformokon: iOS-eszközök, Android-eszközön, a Windows, Windows Phone, Xbox konzolon és látható mezőbe. A következő témakör SDK és Player keretek saját igénybe vehet, a Media Services adatfolyam ügyfélalkalmazásokban hozzanak használható hivatkozások.

[Videolejátszó alkalmazások fejlesztéséhez](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Azure CDN engedélyezése

Media Services integrálása az Azure CDN támogatja. Azure CDN engedélyezésével kapcsolatos további tudnivalókért megtudhatja, [hogy miként kezelheti a folyamatos átvitelű végpontok pontra a Media Services-fiók](media-services-portal-manage-streaming-endpoints.md).

##<a name="scaling-a-media-services-account"></a>A méretezés a Media Services-fiók

**A folyamatos átvitelű fenntartott erőforrás-mennyiség** és a **Kódolás fenntartott mennyiség** , amelyeket szeretne a fiókját, hogy kell-e kiépítve megadásával **Media Services** méretezheti.

Tárterület-fiókok hozzáadásával is méretezheti Media Services-fiókját. Minden tároló fiók 500 TB korlátozódik. Bontsa ki a tárhely túl az alapértelmezett korlátozások, megadhatja több tárterületet fiók csatolása egy adott Media Services-fiókhoz.

[Ez](media-services-portal-scale-streaming-endpoints.md) a témakör a megfelelő témakörökre mutató hivatkozásokat tartalmaz.

##<a name="support"></a>Támogatás

[Azure támogatja](https://azure.microsoft.com/support/options/) az Azure, beleértve a Media Services támogatási lehetőségeket is biztosít.

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Szolgáltatásiszint-szerződés (SLA)

- A Media Services kódolást azt garantálja 99,9 % elérhetősége REST API-tranzakciók.
- Adatfolyam, az azt fogja sikeresen szolgáltatási kérelmek lapot meglévő médiatartalom 99,9 % elérhetősége garanciális legalább egy, a folyamatos átvitelű egység megvásárlásakor.
- A Live csatornákon hogy garantálja, hogy fut a csatornák lesz külső connectivity az idő legalább 99,9 %.
- A tartalom védelme érdekében azt garantálja, hogy azt sikeresen teljesítenek kulcs kérések legalább az idő 99,9 %.
- Az indexelő, azt fogja sikeresen szolgáltatáskérések indexelő tevékenység-kódolás fenntartott dolgozza fel az idő egység 99,9 %.

További tudnivalókért lásd: [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
