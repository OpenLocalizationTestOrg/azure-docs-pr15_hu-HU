<properties 
    pageTitle="Partnerek használatával végzi a Widevine licencek Azure Media Services |} Microsoft Azure" 
    description="Ez a cikk azt ismerteti, hogyan használhatja Azure Media Services (AMS), amely a PlayReady és a Widevine DRMs AMS dinamikusan titkosítja adatfolyam előadásához. A PlayReady licenc Media Services PlayReady licenc kiszolgáló származik, és Widevine licenc hozta castLabs licenc kiszolgáló." 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Partnerek használatával végzi a Widevine licencek Azure Media Services

##<a name="overview"></a>– Áttekintés

Microsoft Azure Media Services lehetővé teszi, hogy előadása MPEG-vonal Widevine DRM, amely a közös titkosítási (CENC) specifikációja egy titkosított látták el.

A Media Services .NET SDK verzió 3.5.2 kezdve, a Media Services lehetővé teszi Widevine licenc sablon konfigurálása és Widevine licenceket. A következő AMS partnerek segít Widevine licencek előadása is használhatja: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

##<a name="castlabs"></a>castLabs

Használhat az előadáshoz Widevine licencek [castLabs](http://castlabs.com/company/partners/azure/) is használhatja. További tudnivalókért olvassa el a [használata castLabs előadásához az Azure Media Services DRM licenceket](media-services-castlabs-integration.md) című témakört.

##<a name="axinom"></a>Axinom

Használhat az előadáshoz Widevine licencek [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) is használhatja. További tudnivalókért olvassa el a [Használatával Axinom előadásához az Azure Media Services DRM licenceket](media-services-axinom-integration.md) című témakört.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még:

[Dinamikus PlayReady és/vagy Widevine közös titkosítással](media-services-protect-with-drm.md)

[Mingfei a blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

