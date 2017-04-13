<properties 
    pageTitle="Tartalom áttekintése védelme |} Microsoft Azure" 
    description="Ez a cikk áttekintést Media Services a tartalomvédelem." 
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
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Tartalom áttekintése védelme


Microsoft Azure Media Services lehetővé teszi a médiafájlokat a számítógépre tárhely, kezelését és kézbesítési elhagyja időpontot biztonságos. Media Services lehetővé teszi a titkosított dinamikusan speciális Encryption Standard (AES) (128 bites titkosítási kulcs használatával), vagy a fő DRMs élő és igény szerinti tartalmak olvasása: Microsoft PlayReady, a Google Widevine és az Apple FairPlay. Media Services is tartalmaz a kézbesítési AES kulcsok és DRM szolgáltatás hivatalos ügyfelek (PlayReady Widevine és FairPlay) licenceket. 

Az alábbi képen a tartalom védelme munkafolyamatok AMS támogató mutatja be. 

![A PlayReady védelme](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Ahhoz, dinamikus-titkosítás használatára, először a titkosított adatfolyamként szeretné adatfolyam végpont előbb be kell szereznie legalább egy adatfolyam fenntartott egység.

Ez a témakör ismerteti, hogy [fogalmak és kifejezések](media-services-content-protection-overview.md) fontos AMS a tartalom védelme ismertetése. A témakör a témakörök, amelyek bemutatják, hogyan tartalomvédelem feladatok eléréséhez [hivatkozásokat](media-services-content-protection-overview.md#common-scenarios) is tartalmaz. 

##<a name="dynamic-encryption"></a>Dinamikus titkosítás:

Microsoft Azure Media Services lehetővé teszik a tartalom, törölje a jelet a fő AES vagy DRM titkosítási dinamikusan titkosított: Microsoft PlayReady, a Google Widevine és az Apple FairPlay.

Jelenleg az alábbi adatfolyam-formátumokat titkosíthatja: HLS MPEG szaggatott és zavartalan Streaming. Nem titkosíthatja streaming format HDS, vagy az progresszív.

Ha azt szeretné, a Media Services titkosítása tárgyi eszköz, kell társítani a titkosítási kulcs (CommonEncryption vagy EnvelopeEncryption) a digitális eszköz kiválasztása és is beállíthatja az billentyű engedélyezési házirendeket.

Meg kell az eszköz kézbesítési házirend beállítása. Ha szeretne egy tároló titkosított digitáliseszköz-adatfolyam, ügyeljen arra, hogy adja meg, hogyan eszköz kézbesítési házirend konfigurálásával ad meg.

Adatfolyam kérésekor a Windows Media Player Media Services dinamikusan titkosítása a AES törölje a jelet a fő tartalomhoz vagy DRM titkosítási használja a megadott billentyűt. Visszafejteni az adatfolyam, a Windows Media player felkéri a kulcsot a fő kézbesítési szolgáltatásból. Döntse el, vagy sem a felhasználó jogosult a kulcs első, a szolgáltatás kiértékeli a kulcs megadott engedélyezési házirendek.

>[AZURE.NOTE]Dinamikus titkosítást előnyeit, először a továbbított végpontot, amelyből tervezi kézbesítési a titkosított tartalom előbb be kell szereznie legalább egy igény szerinti adatfolyam egység. További információért tájékozódhat [skála Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="storage-encryption"></a>Tárterület titkosítás:

A helyi meghajtóra titkosítással AES 256 bit törlése tartalom titkosítása, és töltse fel a Azure tárolóhoz tárolási helyére a többi titkosított tárterület-titkosítás használatára. Tárterület-titkosítással védett eszközök automatikusan titkosítatlan és egy titkosított fájlrendszer előtt kódolást helyezett, és tetszés szerint újra titkosított egy új kimeneti eszközként feltöltése előtt. Az elsődleges használatieset az tárterület-titkosítás esetén, a kiváló minőségű beviteli médiafájlokat a többi titkosítású a lemezen védeni kívánt.

Annak érdekében, hogy a tárhely titkosított eszköz olvasása, meg kell adnia az eszköz kézbesítési házirendet, így Media Services megtudhatja, hogyan kívánja-e a tartalmak olvasása. Az eszköz folyamatosan lejátszott is kell, mielőtt a továbbított kiszolgáló eltávolítja a tárterület-titkosítás, és adatfolyamként továbbítja a tartalom a házirenddel megadott kézbesítési (például AES, gyakori titkosítási vagy titkosítás nélkül).

## <a name="common-encryption-cenc"></a>Közös titkosítási (CENC)

A tartalom PlayReady titkosítja közös titkosítási használatos vagy / és a Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Cbcs-aapl titkosítással

A tartalom FairPlay titkosítja Cbcs-aapl használatos.

## <a name="envelope-encryption"></a>A boríték titkosítás: 

Akkor használja ezt a lehetőséget, ha azt szeretné, a tartalom védelme AES-128 törlése használatával. Ha azt szeretné, hogy egy nagyobb biztonságot nyújt lehetőséget, válassza a DRMs egyik cikkben felsorolt. 

##<a name="licenses-and-keys-delivery-service"></a>Licencek, és a billentyűk kézbesítési szolgáltatás

Media Services szolgáltatást (PlayReady, Widevine, FairPlay) DRM licencek előadásához nyújt, és AES hivatalos ügyfelek billentyűk törölje a jelet. A licencek, és a billentyűk hitelesítési és engedélyezési házirendek beállítása használhatja [az Azure-portálra](media-services-portal-protect-content.md), a REST API-t vagy a Media Services SDK a .NET rendszerhez.

##<a name="token-restriction"></a>Jogkivonat korlátozás

A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: Nyissa meg vagy token korlátozást. A token korlátozott házirend által egy biztonságos jogkivonat szolgáltatás (STS) kiadott token mellékelni kell. Media Services tokenek támogatja az egyszerű webes tokenek (SWT) és JSON webes jogkivonat (JWT) formátum. Media Services nem biztonságos jogkivonat szolgáltatást biztosít. Hozzon létre egy egyéni STS, vagy a Microsoft Azure ACS kihasználhatja a probléma tokenek. A STS kell beállítania, hogy hozzon létre egy jogkivonat, a megadott billentyű és a probléma követelések, a token korlátozás konfigurációban megadott aláírva. A Media Services fő kézbesítési szolgáltatás a kért kulcs (vagy licenccel) ad vissza az ügyfél, ha a token érvényes és követelések e konfigurálva jogkivonat megegyeznek a kulcs (vagy a licencet).

Ha házirend beállítása a token korlátozott, meg kell adnia az igazolás elsődleges kulcsot, a kibocsátó és a közönség paraméterek. Az ellenőrzés elsődleges kulcs, tartalmazza, a kulcs, amely a token van aláírva kibocsátó a biztonságos jogkivonat-szolgáltatás, amely a token problémák. A közönség felé (más néven hatókör) a token szándékának mutatja be, vagy az erőforrás a token engedélyezi a hozzáférést. A Media Services fő kézbesítési szolgáltatás ellenőrzi, hogy ezeket az értékeket a token értékekre a sablonban.

##<a name="streaming-urls"></a>Adatfolyam URL-címei

A digitális eszköz kiválasztása a egynél több DRM titkosítva van, ha egy titkosító címke kell használni az adatfolyam URL-cím: (formátum = "m3u8-aapl" titkosítási = "xxx").

A következő érvényesek:

- Csak nulla vagy egy típusú titkosítást adható meg.
- Titkosítási típus nem található meg az URL-címet kell megadni, ha csak egy titkosító alkalmaztak az eszközre.
- Titkosítási típus a kis-és nagybetűk.
- A következő típusú titkosítást adható meg:  
    - **cenc**: közös titkosítási (Playready vagy Widevine)
    - **cbcs-aapl**: Fairplay
    - **CBC**: AES boríték titkosítást.

##<a name="common-scenarios"></a>Gyakori alkalmazási területek

A következő témakörök szemléltetik a tárterület a tartalom védelme, dinamikusan titkosított adatfolyam olvasása, AMS kulcs/licenc kézbesítési szolgáltatás használata

- [A AES védelme](media-services-protect-with-aes128.md) 
- [PlayReady és/vagy Widevine védelme](media-services-protect-with-drm.md)
- [Adatfolyam-és/vagy az Apple FairPlay PlayReady a HLS tartalom védett](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>További felhasználási területei

- [Hogyan integrálása az Azure PlayReady licenc szolgáltatás saját titkosító/adatfolyam-kiszolgálóval](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [CastLabs használatával végzi a DRM licencek Azure Media Services](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Kapcsolódó hivatkozások

[PlayReady kihirdetése olyan szolgáltatás és az Azure Media Services AES dinamikus titkosítás](http://mingfeiy.com/playready)

[Azure Media Services PlayReady licenc kézbesítési árak magyarázata](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Hogyan szeretné hibakeresése AES titkosított adatfolyamok az Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT jogkivonat authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC integráció alapú alkalmazást és az Azure Active Directory és JWT jogcímeken alapuló kulcs tartalomkézbesítési korlátozása](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[A probléma tokenek használata az Azure ACS](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
