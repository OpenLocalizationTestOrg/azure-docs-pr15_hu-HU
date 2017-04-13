<properties 
    pageTitle="Media Encoder prémium munkafolyamat formátumok és kodekek |} Microsoft Azure" 
    description="Ez a témakör áttekintést Media Encoder prémium munkafolyamat formátumok formátumok és a kodekekről" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder prémium munkafolyamat formátumok és a kodekekről


>[AZURE.NOTE]A prémium encoder kérdésekre e-mailben mepd a Microsoft.com webhelyről.
>
>Ebben a témakörben tárgyalt Media Encoder prémium munkafolyamat media processzor Kínában nem érhető el. 

A dokumentum bemeneti és kimeneti formátumok és a nyilvános verziót a **Media Encoder prémium munkafolyamat** encoder által támogatott kodekekről listáját tartalmazza.

[Media Encoder prémium Worflow beviteli formátumok és a kodekekről](#input_formats)

[Media Encoder prémium Worflow kimeneti formátumban és kodekek](#output_formats)

**Media Encoder prémium munkafolyamat** támogatja, a [jelen](#closed_captioning) szakaszban ismertetett kódolt feliratok. 


##<a id="input_formats"></a>Media Encoder prémium munkafolyamat bemeneti formátumok és a kodekekről

A következő szakasz bemeneteként támogatja a médiafájlok processzor kodekekről és fájl formátumokat sorolja fel.

###<a name="input-containerfile-formats"></a>A szövegbeviteli tároló/fájlformátumok

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- MPEG-2 átviteli adatfolyam megjelenítését.
- MPEG-2 Program adatfolyamok
- MPEG-4/MP4
- A Windows Media/ASP
- AVI (nem tömörített 8 bites/10 bit)

###<a name="input-video-codecs"></a>Beviteli videokodekek

- 8 bites/10-bites felfelé 4 AVC: 2:2, beleértve a AVCIntra
- Avid DNxHD (a MXF)
- DVCPro/DVCProHD (a MXF)
- JPEG2000
- MPEG-2 (422 profil és a magas szintű, ideértve például a XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 változatok)
- MPEG-1
- A Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Beviteli audiokodekek

- AES (SMPTE 331 M és 302 M, AES3 – 2003-as)
- E Dolby®
- Dolby® digitális (AC3)
- AAC (AAC-LC, az AAC-HE és az AAC-HEv2; legfeljebb 5.1)
- MPEG-réteg 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV/PCM
 
##<a id="output_format"></a>Media Encoder prémium munkafolyamat kimeneti formátumban és kodekek

A következő szakasz ezt az eredményt, a media processzor a támogatott kodekekről és a fájl formátumokat sorolja fel.

###<a name="output-containerfile-formats"></a>Kimeneti tároló/fájlformátumok

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM és AS02)
- DPP (beleértve a AS11)
- GXF
- MPEG-4/MP4
- A Windows Media/ASP
- AVI (nem tömörített 8 bites/10 bit)
- Zökkenőmentes adatfolyam fájlformátum (PIFF 1.3-as)
- MPEG-TS 


###<a name="output-video-codecs"></a>Kimeneti videokodekek

- AVC (H.264; 8 bites; felfelé magas profil szint 5.2; 4 K Ultra HD; AVC belüli)
- Avid DNxHD (a MXF)
- DVCPro/DVCProHD (a MXF)
- MPEG-2 (422 profil és a magas szintű, ideértve például a XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 változatok)
- MPEG-1
- A Windows Media Video/VC-1
- JPEG miniatűr létrehozása

###<a name="output-audio-codecs"></a>Kimeneti audiokodekek

- AES (SMPTE 331 M és 302 M, AES3 – 2003-as)
- Dolby® digitális (AC3)
- Dolby® digitális Plus (az E-AC3) 7.1 felfelé
- AAC (AAC-LC, az AAC-HE és az AAC-HEv2; legfeljebb 5.1)
- MPEG-réteg 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Kódolt feliratok támogatása

A ingest, **Media Encoder prémium munkafolyamat** támogatja:

1. SCC fájlok
1. SMPTE-TT fájlok
1. CEA-608/CEA-708 – a felhasználói adatok (SEI H.264 általános iskolai adatfolyam, ATSC/53, SCTE20 üzeneteket), vagy kiegészítő adatként MXF/GXF fájlokban sor
1. STL alcím fájlok

A kibocsátás az alábbi lehetőségek állnak rendelkezésre:

1. CEA-608 való CEA-708 fordítási
1. CEA-608/CEA-708 haladnia (SEI üzenetek H.264 általános iskolai adatfolyam beágyazva, vagy a kiegészítő adatainak MXF fájlok végezni)
1. SCC
1. SMPTE Időzítette a szöveg (a forrás CEA-608 per SMPTE RP2052, beleértve a DFXP fájl létrehozása)
1. UTCA alcím fájl
1. DVB alcím adatfolyamok

Megjegyzés: a fenti kimeneti formátumban nem az összes támogatott kézbesítési az Azure Media Services streaming keresztül.

##<a name="known-issues"></a>Ismert problémák

Ha a bemeneti videó nem tartalmaz kódolt feliratok, a kimeneti eszköz is továbbra is tartalmaz egy üres TTML fájlt. 


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
