<properties 
    pageTitle="CastLabs használatával végzi a Widevine licencek Azure Media Services |} Microsoft Azure" 
    description="Ez a cikk azt ismerteti, hogyan használhatja Azure Media Services (AMS), amely a PlayReady és a Widevine DRMs AMS dinamikusan titkosítja adatfolyam előadásához. A PlayReady licenc Media Services PlayReady licenc kiszolgáló származik, és Widevine licenc hozta castLabs licenc kiszolgáló." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>CastLabs használatával végzi a Widevine licencek Azure Media Services

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>– Áttekintés

Ez a cikk azt ismerteti, hogyan használhatja Azure Media Services (AMS), amely a PlayReady és a Widevine DRMs AMS dinamikusan titkosítja adatfolyam előadásához. A PlayReady licenc Media Services PlayReady licenc kiszolgáló származik, és Widevine licenc hozta **castLabs** licenc kiszolgáló.

Lejátszás adatfolyam-CENC (PlayReady és/vagy Widevine) védett tartalmat, az [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)is használhatja. [AMP](http://amp.azure.net/libs/amp/latest/docs/) látni további információt.

Az alábbi ábra mutatja be, a magas szintű Azure Media Services és castLabs integrációs architektúra.

![integráció](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Tipikus rendszer beállítása

- Médiatartalom AMS vannak tárolva.
- Kulcs azonosítók tartalom kulcsok castLabs és a AMS tárolja.
- castLabs és AMS is rendelkezik beépített jogkivonat hitelesítési. Az alábbi szakaszokból megtudhatja, hogy hitelesítési tokenek. 
- Amikor egy ügyfél kérelmek adatfolyam a videót, a tartalom dinamikusan titkosított **Közös titkosítási** (CENC) és zavartalan Streaming és a SZAGGATÁS AMS által dinamikusan csomagolt. Azt is használhat az előadáshoz PlayReady M2TS általános iskolai adatfolyam titkosítási HLS adatfolyam protokollhoz.
- PlayReady licenc AMS server-licenc van beolvasása és Widevine licenc castLabs licenc kiszolgálóról veszi. 
- Media Player automatikusan úgy dönt, hogy melyik licenc-ból alapján az ügyfél platform lehetőséget. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Hitelesítési jogkivonat előállítása ismertető licenc

CastLabs és a AMS támogatja a JWT (JSON webes jogkivonat) jogkivonat fájlformátumát kattintva engedélyezheti egy licencet. 

###<a name="jwt-token-in-ams"></a>AMS JWT token 

Az alábbi táblázat ismerteti a AMS JWT jogkivonat. 

Kibocsátó|A választott kibocsátó karakterlánccal biztonságos jogkivonat szolgáltatás (STS)
---|---
A célközönség|A használt STS közönség karakterlánccal
Követelések|Követelések csoportja
NotBefore|Indítsa el a token érvényessége
Lejár|A token vége érvényessége
SigningCredentials|A kulcs között PlayReady licenc kiszolgáló castLabs licenc-kiszolgáló és STS, megosztott lehet szimmetrikus vagy aszimmetrikus billentyűt.

###<a name="jwt-token-in-castlabs"></a>CastLabs JWT token

Az alábbi táblázat ismerteti a castLabs JWT jogkivonat. 

név|Leírás
---|---
optData|Adatokat tartalmazó JSON karakterlánc. 
CRT|Az eszköz információt tartalmazó JSON karakterlánc a adatai, majd a lejátszás Licencjogok.
IAT|Az aktuális dátumot és időt a alapidőpont.
jti|Tudnivalók a (minden jogkivonat csak egyszer használható a castLabs rendszer) token egyedi azonosítója.

##<a name="sample-solution-set-up"></a>Minta megoldás beállítása 

A [minta megoldás](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) két projektet tartalmaz:

-   Egy konzol alkalmazás DRM korlátozások beállítása egy már j.leny eszköz PlayReady és Widevine használható.
-   Egy webalkalmazás, amely meg lehet tekinteni egy STS nagyon egyszerűsített verziója által jogkivonat kezek.


A New használata:

1.  Módosítsa a app.config AMS hitelesítő adatait, castLabs hitelesítő adatait, STS konfigurációs és megosztott kulcs beállítása.
2.  Egy tárgyi eszköz AMS feltölteni.
3.  A UUID kinyerése a feltöltött eszköz, és módosítsa a sor 32 a Program.cs fájl:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Az eszközkód elnevezési az eszköz a castLabs rendszer (a Program.cs fájlban sor 44) használata.

    Be kell eszközkód **castLabs**; egy egyedi alfanumerikus karakterlánc lesz szüksége van.

5.  Futtassa a program.


A webes alkalmazás (STS) használata:

1.  Módosítsa a web.config beállítási castlabs kereskedelmi Azonosítóját, a STS konfiguráció és a megosztott billentyűt.
2.  Telepítse az Azure webhelyre.
3.  Nyissa meg azt a webhelyet.

##<a name="playing-back-a-video"></a>Vissza a videók lejátszása

Gyakori titkosítással (PlayReady és/vagy Widevine) titkosított videó lejátszás az [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)is használhatja. A konzol alkalmazás fut, echoed az tartalom kulcs Azonosítóját, illetve cikkét URL-CÍMÉT.

1.  Nyisson meg egy új lapot, és indítsa el a STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Nyissa meg az [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Illessze be a továbbított URL-CÍMÉT.
4.  Kattintson a **Speciális beállítások** jelölőnégyzetet.
5.  A **védelem** tartalmazó legördülő listára válassza a PlayReady és/vagy Widevine.
6.  Illessze be a token a STS a Token rendelés_részletei.mennyiség kapott. 
    
    A castLab licenc kiszolgáló nem szükséges, a "Bearer =" előtagot a token elé. Így távolítsa el, amely a token elküldése előtt.
7.  Frissítés a Windows Media player.
8.  A videó lejátszása kell kell.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
