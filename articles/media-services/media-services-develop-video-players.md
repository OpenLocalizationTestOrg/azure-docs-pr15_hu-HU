<properties 
    pageTitle="Videolejátszó alkalmazások fejlesztői számára" 
    description="A témakör hivatkozások Player keretek és bővítmények saját igénybe vehet, a Media Services adatfolyam ügyfélalkalmazásokban hozzanak használható." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Videolejátszó alkalmazások fejlesztői számára

##<a name="overview"></a>– Áttekintés

Azure Media Services eszközeivel létre kell hoznia a sokoldalú, dinamikus player ügyfélalkalmazásokban többek között a legtöbb platformokon: iOS-eszközök, Android-eszközön, a Windows, Windows Phone, Xbox konzolon és látható mezőbe. Ez a témakör a saját igénybe vehet, az Azure Media Services adatfolyam ügyfélalkalmazásokban hozzanak használható Player keretek és SDK hivatkozásokat is tartalmaz.

##<a name="azure-media-player"></a>Azure Media Player

[Azure Media Player](http://aka.ms/ampinfo) webes videolejátszó lejátszásához médiatartalom a Microsoft Azure Media Services a böngészők és eszközök számos beépített. Azure Media Player ipari szabványoknak, például a HTML5-ös, Media forrás bővítmények (MSE) és titkosított multimédia-bővítmények (EME) ahhoz, hogy egy továbbfejlesztett adaptív adatfolyam élmény használja. Ha ezek a szabványok nem érhető el, egy eszközön, vagy a böngészőben, Azure Media Player használja Flash és a Silverlight technológia alaplekérdezések. A lejátszás technológiát használja, függetlenül a fejlesztők API-khoz eléréséhez egységesített JavaScript felhasználói felülete fog rendelkezni. Ez lehetővé teszi az Azure Media Services, melyet tartalom lejátszható wide-cellatartomány eszközök és böngészők anélkül, hogy minden további munkamennyiség között.

Microsoft Azure Media Services lehetővé teszi, hogy jutnak kötőjel, zökkenőmentes Streaming és a folyamatos átvitelű formátumok HLS tartalom vissza tartalom lejátszása. Azure Media Player figyelembe veszi a következő különböző formátumokban, és automatikusan lejátssza a legjobb hivatkozásra az platform/böngésző funkciók alapján. Microsoft Azure Media Services is lehetővé teszi, hogy az eszközök PlayReady titkosítással dinamikus titkosítás vagy AES-128 bites boríték titkosítást. Azure Media Player lehetővé teszi, hogy PlayReady visszafejtése és AES-128 bites titkosított tartalmat, amikor megfelelően konfigurálva. 

További információ:

- [Azure Media Player](http://aka.ms/ampinfo)
- [Azure Media Player dokumentáció](http://aka.ms/ampdocs) 
- [Azure Media Player az első lépések a Blog](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Iratkozzon fel naprakész a az Azure Media Player legújabb](http://aka.ms/ampsignup)
- [Új frissítéseiről, ötletek, visszajelzés hozzáadása](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Egyéb eszközöket lejátszó alkalmazások létrehozása

Is használhatja az alábbi SDK bármelyikét:

- [Adatfolyam-ügyfél SDK simítása](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Zökkenőmentes adatfolyam Windows áruház-alkalmazás](media-services-build-smooth-streaming-apps.md)
- [Microsoft Media Platform: Player keretrendszer](http://playerframework.codeplex.com/) 
- [HTML5-ös Player keretrendszer dokumentáció](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [A Microsoft sima OSMF a folyamatos átvitelű a beépülő modul](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Licencelési Microsoft® zökkenőmentes adatfolyam ügyfél portolása Kit](http://aka.ms/sspk) 
- [XBOX Video alkalmazás fejlesztési](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Hirdetési

Azure Media Services támogatja a Windows Media-platformon keresztül ad beszúrási: Player keretek. Active Directory-támogatással Player keretek a Windows 8-ban, a Silverlight, a Windows Phone 8 és az iOS-eszközökhöz érhetők el. Minden egyes player keretrendszer példakódot arról, hogy miként tudnak megvalósítani a lejátszó alkalmazást tartalmazza. Vannak három különböző típusú szeretne beszúrni a médiafájlokat hirdetések:

Lineáris – teljes keret hirdetések, mutasson a fő videó

Nemlineáris – átfedő hirdetések, amelyek akkor jelennek meg, mint a fő videó lejátszása, általában emblémát vagy más a Windows Media player mutatnia statikus kép

Kereső – jelennek meg a Windows Media player kívüli hirdetések

A fő videó idővonal bármely pontján hirdetések lehet ügyfélwebhelyre. A Windows Media player kell mondani, mikor játszható le a ad, és a mely hirdetések lejátszása. Ehhez használja a szabványos XML-alapú fájlokat: videó Ad szolgáltatás sablon (VAST), a digitális videó több Ad lista (VMAP), a Media absztrakt szekvenálási sablon (OSZLOPOS) és a digitális videó Player Ad felület Definition (VPAID). NAGY fájlok adja meg, hogy milyen hirdetések megjelenítéséhez. VMAP fájlok adja meg a különböző hirdetések lejátszása és DÖNTŐ XML tartalmaz. A fájlok OSZLOPOS úgy is szekvencia hirdetések, amely is tartalmazhat DÖNTŐ XML. VPAID fájlokat a videolejátszó és az Active Directory vagy az Active Directory kiszolgáló közötti felület határozza meg. További tudnivalókért olvassa el a [Hirdetések beszúrása](https://msdn.microsoft.com/library/dn387398.aspx)című témakört.

A folyamatos átvitelű videók Live kódolt feliratok és hirdetések támogatásával kapcsolatos további tudnivalókért lásd [támogatott kódolt feliratok és az Active Directory beszúrási szabványoknak](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még:

[MPEG-vonal adaptív-adatfolyam videó beágyazása az DASH.js a HTML5-ös alkalmazásokban](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js tárházba](https://github.com/Dash-Industry-Forum/dash.js)
 
