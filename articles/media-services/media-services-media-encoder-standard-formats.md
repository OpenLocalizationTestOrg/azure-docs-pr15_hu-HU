<properties 
    pageTitle="Media Encoder szabványos formátumok és kodekek" 
    description="Ez a témakör áttekintést nyújt a Media Encoder szabványos formátumok és a kodekek." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder szabványos formátumok és kodekek


Ehhez a dokumentumhoz a leggyakoribb importálás és exportálás formátumok, amelyek segítségével használhatja Media Encoder standard listáját tartalmazza.


##<a name="input-containerfile-formats"></a>A szövegbeviteli tároló/fájlformátumok

A fájlformátumok (fájlkiterjesztések)|Támogatott
---|---|---|---
(A H.264 és AAC-kodekek segítségével) FLV (.flv)          |igen 
MXF (.mxf)                  |igen 
GXF (.gxf)                  |igen 
MPEG2-PS MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg)   |igen 
Windows Media Video (WMV) / ASP (.wmv, .asf) |igen 
(Nem tömörített 8 bites/10 bit) AVI (.avi)|igen 
MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)|igen 
[A Microsoft digitális videó Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr ms) |igen 
Matroska/WebM (.mkv)        |igen 
HULLÁMOS/WAV (.wav) |igen 
A QuickTime (.mov) |igen

>[AZURE.NOTE] A gyakrabban észlelt fájlkiterjesztések listája felett van. Media Encoder szabványos támogatja számos más (például: .m2ts, .mpeg2video, .qt). Ha kódolását egy fájlt próbál, és meg hibaüzenet a nem támogatott formátumára, adjon visszajelzést [Itt](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>A beviteli tárolók hangformátumok 

Media Encoder szabványos támogatja a következő hangformátumok beviteli tárolókban végrehajtása:

- MXF, GXF és QuickTime fájlokat, amelyeknek a időosztásos sztereó hang számok vagy 5.1 minták

vagy

- Amikor külön PCM számok szerint történik a hangot, de a csatorna hozzárendelést (sztereó vagy 5.1) a fájl metaadatok kitűnő MXF, GXF és QuickTime fájlok

Megjegyzés: a támogató explicit vagy felhasználó által megadott csatorna megfeleltetés a közeljövőben fog elhelyezni.


##<a name="input-video-codecs"></a>Beviteli videokodekek

Beviteli videokodekek|Támogatott
---|---|---|---
8 bites/10-bites felfelé 4 AVC: 2:2, beleértve a AVCIntra   |8 bites 4:2:0 és a 4:2:2 
Avid DNxHD (a MXF)                                 |igen 
DVCPro/DVCProHD (a MXF)                            |igen 
Digitális videó (DV) (AVI fájlokban)                   |igen
JPEG 2000                                           |igen 
MPEG-2 (422 profil és a magas szintű, ideértve például a XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 változatok)|Legfeljebb 422 profil 
MPEG-1                                              |igen 
VC-1/WMV9                                           |igen 
Canopus HQ/HQX                                      |nem 
MPEG-4-es rész 2                                       |igen 
[Theora](https://en.wikipedia.org/wiki/Theora)      |igen 
Nem tömörített YUV420 vagy Emeletes                   |igen
Apple ProRes 422                                    |igen
Apple ProRes 422 LT |igen
Apple ProRes 422 HQ |igen
Apple ProRes Proxy|igen
Apple ProRes 4444 |igen
Apple ProRes 4444 XQ |igen



##<a name="input-audio-codecs"></a>Beviteli audiokodekek

Beviteli audiokodekek|Támogatott
---|---|---|---
AAC (AAC-LC, az AAC-HE és az AAC-HEv2; legfeljebb 5.1)|igen 
MPEG-réteg 2|igen 
MP3 (MPEG-1 Audio Layer 3)|igen 
Windows Media Audio|igen 
WAV/PCM|igen 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|igen 
[Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |igen 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|igen 
AMR (adaptív több ráta)|igen
AES (SMPTE 331 M és 302 M, AES3 – 2003-as)        |nem 
E Dolby®                                    |nem 
Dolby® digitális (AC3)                        |nem 
Dolby® digitális plusz (az E-AC3)                 |nem 


##<a name="output-formats-and-codecs"></a>Kimeneti formátumban és kodekek

Az alábbi táblázat felsorolja az Exportálás támogatott kodekekről és fájl formátumok.


Fájlformátum|Videó kodek|A kodek hang
---|---|---
MP4 <br/><br/>(beleértve a többszörös-átviteli sebesség MP4 tárolók) |H.264 (magas, Main és eredeti profilok)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2-TS |H.264 (magas, Main és eredeti profilok)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még:

[Az Azure Media Services kódolási igény szerinti tartalom](media-services-encode-asset.md)

[Hogyan Media Encoder standard kódolását](media-services-dotnet-encode-with-media-encoder-standard.md)
