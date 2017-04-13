<properties 
    pageTitle="Áttekintés és Azure összehasonlítása a igény szerinti media kódolók |} Microsoft Azure" 
    description="Ez a témakör áttekintést és igény szerinti media kódolók a összehasonlítása Azure ad." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Áttekintés és a igény szerinti media kódolók Azure összehasonlítása

##<a name="encoding-overview"></a>Kódolási – áttekintés

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

Ez a cikk rövid áttekintést nyújt az igény media kódolók és részletesebb információkat foglalkozó cikkekre mutató hivatkozásokat tartalmaz. A témakör az összehasonlítása a kódolók is tartalmaz.

Ne feledje, hogy alapértelmezés szerint minden Media Services fiók van egy aktív kódolási feladat egyszerre. Kódolási egységek, amelyek lehetővé teszik, hogy az egyes kódolási fenntartott egység vásárolhat egyet egy időben, futó több kódolási feladatok lefoglalása Tudnivalókért lásd a [Méretezés kódolást egységek](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media Encoder Standard

###<a name="how-to-use"></a>Használata

[Hogyan Media Encoder standard kódolását](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Formátumok

[Formátumok és a kodekekről](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Elhelyezés

Media Encoder szabványos van beállítva az egyik a kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Bemeneti és kimeneti metaadatok

A beviteli kódolók-metaadatok leírása [Itt](http://msdn.microsoft.com/library/azure/dn783120.aspx).

A kódolók kimeneti metaadatok leírása [Itt](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>A miniatűrök készítése

Információ megtudhatja, [hogy miként készítése a miniatűrök Media Encoder szabványos használatával](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Videók (kivágási) vágása

Információ tájékozódhat [Videók Media Encoder szabványos vágása](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Átfedi létrehozása

Információ [létrehozásának](media-services-custom-mes-presets-with-dotnet.md#overlay)módját átfedi Media Encoder szabványos használatával.

###<a name="see-also"></a>Lásd még:

[A Media Services-blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media Encoder prémium munkafolyamat

###<a name="overview"></a>– Áttekintés

[Az Azure Media Services kódolást prémium bemutatása](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Használata

Media Encoder prémium munkafolyamat van beállítva, összetett a munkafolyamatok használatáról. Munkafolyamat-fájlok lehetett létrehozni, és frissíti a [Munkafolyamat-Tervező](media-services-workflow-designer.md) eszközzel.

[Azure Media Services kódolást prémium használata](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Ismert problémák

Ha a bemeneti videó nem tartalmaz kódolt feliratok, a kimeneti eszköz is továbbra is tartalmaz egy üres TTML fájlt. 


##<a id="compare_encoders"></a>Kódolók összehasonlítása

###<a id="billing"></a>Minden egyes encoder által használt számlázási állapotjelzője

Multimédia-feldolgozó neve|Vonatkozó árak|Jegyzetek
---|---|---
**Media Encoder Standard** |KÓDOLÓ|Feladatok kódolást ráterheljük a kimenet méretnek megfelelően eszköz gigabájt meghatározott kamatlábbal az [Itt][1], a KÓDOLÓ oszlopban.
**Media Encoder prémium munkafolyamat** |PRÉMIUM KÓDOLÓ|Feladatok kódolást ráterheljük a kimenet méretnek megfelelően eszköz gigabájt meghatározott kamatlábbal az [Itt][1], a PRÉMIUM ENCODER oszlopban.


Ebben a szakaszban a **Media Encoder szabványos** és **Media Encoder prémium munkafolyamat**kódolási funkcióinak összehasonlítása


###<a name="input-containerfile-formats"></a>A szövegbeviteli tároló/fájlformátumok

A szövegbeviteli tároló/fájlformátumok|Media Encoder Standard|Media Encoder prémium munkafolyamat
---|---|---
Adobe® Flash® F4V           |igen|igen
MXF/SMPTE 377M              |igen|igen
GXF                         |igen|igen
MPEG-2 átviteli adatfolyam megjelenítését.    |igen|igen
MPEG-2 Program adatfolyamok      |igen|igen
MPEG-4/MP4                  |igen|igen
A Windows Media/ASP           |igen|igen
AVI (nem tömörített 8 bites/10 bit)|igen|igen
3GPP/3GPP2                  |igen|nem
Zökkenőmentes adatfolyam fájlformátum (PIFF 1.3-as)|igen|nem
[A Microsoft digitális videó Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|igen|nem
Matroska/WebM               |igen|nem
A QuickTime (.mov) |igen|nem

###<a name="input-video-codecs"></a>Beviteli videokodekek

Beviteli videokodekek|Media Encoder Standard|Media Encoder prémium munkafolyamat
---|---|---
8 bites/10-bites felfelé 4 AVC: 2:2, beleértve a AVCIntra   |8 bites 4:2:0 és a 4:2:2|igen
Avid DNxHD (a MXF)                                 |igen|igen
DVCPro/DVCProHD (a MXF)                            |igen|igen
JPEG2000                                            |igen|igen
MPEG-2 (422 profil és a magas szintű, ideértve például a XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 változatok)|Legfeljebb 422 profil|igen
MPEG-1                                              |igen|igen
A Windows Media Video/VC-1                            |igen|igen
Canopus HQ/HQX                                      |nem|nem
MPEG-4-es rész 2                                       |igen|nem
[Theora](https://en.wikipedia.org/wiki/Theora)      |igen|nem
Apple ProRes 422    |igen|nem
Apple ProRes 422 LT |igen|nem
Apple ProRes 422 HQ |igen|nem
Apple ProRes Proxy|igen|nem
Apple ProRes 4444 |igen|nem
Apple ProRes 4444 XQ |igen|nem

###<a name="input-audio-codecs"></a>Beviteli audiokodekek

Beviteli audiokodekek|Media Encoder Standard|Media Encoder prémium munkafolyamat
---|---|---
AES (SMPTE 331 M és 302 M, AES3 – 2003-as)        |nem|igen
E Dolby®                                    |nem|igen
Dolby® digitális (AC3)                        |nem|igen
Dolby® digitális plusz (az E-AC3)                 |nem|igen
AAC (AAC-LC, az AAC-HE és az AAC-HEv2; legfeljebb 5.1)|igen|igen
MPEG-réteg 2|igen|igen
MP3 (MPEG-1 Audio Layer 3)|igen|igen
Windows Media Audio|igen|igen
WAV/PCM|igen|igen
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|igen|nem
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format)) |igen|nem
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|igen|nem


###<a name="output-containerfile-formats"></a>Kimeneti tároló/fájlformátumok

Kimeneti tároló/fájlformátumok|Media Encoder Standard|Media Encoder prémium munkafolyamat
---|---|---
Adobe® Flash® F4V|nem|igen
MXF (OP1a, XDCAM és AS02)|nem|igen
DPP (beleértve a AS11)|nem|igen
GXF|nem|igen
MPEG-4/MP4|igen|igen
MPEG-TS|igen|igen
A Windows Media/ASP|nem|igen
AVI (nem tömörített 8 bites/10 bit)|nem|igen
Zökkenőmentes adatfolyam fájlformátum (PIFF 1.3-as)|nem|igen

###<a name="output-video-codecs"></a>Kimeneti videokodekek

Kimeneti videokodekek|Media Encoder Standard|Media Encoder prémium munkafolyamat
---|---|---
AVC (H.264; 8 bites; felfelé magas profil szint 5.2; 4 K Ultra HD; AVC belüli)|Csak a 8 bites 4:2:0|igen
Avid DNxHD (a MXF)|nem|igen
MPEG-2 (422 profil és a magas szintű, ideértve például a XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 változatok)|nem|igen
MPEG-1|nem|igen
A Windows Media Video/VC-1|nem|igen
JPEG miniatűr létrehozása|nem|igen

###<a name="output-audio-codecs"></a>Kimeneti audiokodekek

Kimeneti audiokodekek|Media Encoder Standard|Media Encoder prémium munkafolyamat
---|---|---
AES (SMPTE 331 M és 302 M, AES3 – 2003-as)|nem|igen
Dolby® digitális (AC3)|nem|igen
Dolby® digitális Plus (az E-AC3) 7.1 felfelé|nem|igen
AAC (AAC-LC, az AAC-HE és az AAC-HEv2; legfeljebb 5.1)|igen|igen
MPEG-réteg 2|nem|igen
MP3 (MPEG-1 Audio Layer 3)|nem|igen
Windows Media Audio|nem|igen


##<a name="error-codes"></a>Hibakódok esetén  

Az alábbi táblázat abban az esetben, ha hiba történt a kódolási feladat-végrehajtás során kell visszaadott hibakódok.  Részletek a .NET-kódban, használja a [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) osztály. Részletek a többi kódban, használja a [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API-t.

ErrorDetail.Code|Hiba lehetséges okai
-----|-----------------------
Ismeretlen| A tevékenység végrehajtása során az ismeretlen hiba
ErrorDownloadingInputAssetMalformedContent|Kategória hibák beviteli eszköz, például a hibás fájlnevek, nulla hosszúságú fájlok, nem a megfelelő letöltés hibák eltakaró formázza, és így tovább.
ErrorDownloadingInputAssetServiceFailure|Kategória a hibákat ismerteti azokat a problémákat, szolgáltatási oldalán – például hálózati vagy tároló hibáinak letöltése közben.
ErrorParsingConfiguration|Kategória hibák hol tevékenység <see cref="MediaTask.PrivateData"/> (konfigurációja) nem érvényes, például a beállítás nem egy érvényes rendszer előre definiált vagy érvénytelen XML-kódot tartalmaz.
ErrorExecutingTaskMalformedContent|Kategória a tevékenységet, ahol belül a bemeneti médiafájlok okozza hiba végrehajtása során hibáit.
ErrorExecutingTaskUnsupportedFormat|Kategória a hibák, ahol a médiafájlok processzor nem lehet feldolgozni azokat a fájlokat, feltéve, – media formázása nem támogatott vagy nem egyezik meg a konfigurációs. Például próbál konzerv csak videót tartalmazó tárgyi eszköz csak hangot tartalmazó kimenete
ErrorProcessingTask|Kategória többi hibák a médiafájlok processzor észlel a tevékenység feldolgozása közben, amelyek nem kapcsolódó tartalmakra.
ErrorUploadingOutputAsset|A hibák, a kimeneti eszköz feltöltésekor kategória
ErrorCancelingTask|Kategória hibáit, hogy lefedje a hibák, amikor megpróbál a tevékenység megszakítása
TransientError|Kategória hibáit, hogy pontosan illeszkedjen átmeneti problémák (pl.. ideiglenes hálózati kérdések Azure adathordozós)


Segítség a **Media Services** csapatától, nyissa meg a [támogatási jegyek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Kapcsolódó cikkek

- [Speciális feladatokra kódolási Media Encoder szabványos készletek testre szabásával](media-services-custom-mes-presets-with-dotnet.md)
- [Kvóták és korlátai](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
