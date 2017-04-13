<properties 
    pageTitle="A lejátszás a tartalom |} Microsoft Azure" 
    description="Ez a témakör felsorolja a meglévő játékosokat, hogy is használhatja a lejátszás a tartalmakat." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>A meglévő játékosokat tartalom lejátszása

Azure Media Services számos népszerű adatfolyam-formátumokat támogatja, például a folyamatos zökkenőmentes, HTTP Live Streaming és MPEG-vonal. Ez a témakör mutat be meglévő játékosokat, hogy fogadhassák tesztelése is használhatja.

>[AZURE.NOTE]Dinamikusan csomagolt lejátszás vagy dinamikusan titkosított tartalmat, ellenőrizze, hogy legalább egy adatfolyam egység beszerzése az adatfolyam végpontot, ahonnan a tartalmak olvasása megtervezése. Adatfolyam egységek méretezés kapcsolatos további tudnivalókért lásd: [hogyan adatfolyam egységek méretezni](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure portál Media Services tartalom Windows Media player

Az **Azure** portál tartalom lejátszó tesztelje a videó használható.

Kattintson a kívánt videót (Győződjön meg arról, hogy a [közzétett](media-services-portal-publish.md)volt), és kattintson a **Lejátszás** gombra a portálon alján.

Néhány érvényesek:

- A **MEDIA SERVICES tartalom PLAYER** játssza le, az alapértelmezett streaming végpontot. Ha az alapértelmezettől adatfolyam egyik végpontját a lejátszani kívánt, egy másik lejátszóval. Ha például [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure Media Player

[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) segítségével a lejátszás a tartalom (törlése vagy védett) a következő formátumban:

- Zökkenőmentes Streaming
- MPEG-KÖTŐJEL
- HLS
- Fokozatos MP4


###<a name="flash-player"></a>A Flash Player

####<a name="aes-encrypted-with-token"></a>AES titkosított a Token

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight-lejátszókat

####<a name="monitoring"></a>Figyelése

[http://smf.cloudapp.NET/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>A Token PlayReady

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>SZAGGATOTT szereplők

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Más

Tesztelje a HLS URL-címeit is használhatja:

- IOS-eszközökön a **safari** vagy
- **3ivx HLS Player** Windows rendszeren.

##<a name="developing-video-players"></a>Videolejátszó kialakítása

A saját játékosokat kapcsolatos további tudnivalókért olvassa el a [fejlődő videolejátszó](media-services-develop-video-players.md) című témakört.




##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
