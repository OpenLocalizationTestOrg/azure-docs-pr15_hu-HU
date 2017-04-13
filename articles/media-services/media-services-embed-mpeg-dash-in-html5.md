<properties 
    pageTitle="MPEG-vonal adaptív-adatfolyam videó beágyazása DASH.js a HTML5-ös alkalmazás |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan lehet adaptív MPEG-vonal folyamatos átvitelű videó beágyazása az DASH.js a HTML5-ös alkalmazásokban." 
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


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>MPEG-vonal adaptív-adatfolyam videó beágyazása az DASH.js a HTML5-ös alkalmazásokban

##<a name="overview"></a>– Áttekintés

MPEG-vonal videotartalmat, amely jelentős előnyökkel jár azok kiváló minőségű, adaptív video-adatfolyam kimeneti előadásához kívánó adaptív folyamatos ISO-szabvány. MPEG-gondolatjelre amikor a hálózat túlzsúfolt válik a video-adatfolyam automatikusan legördülő fog alsó definition. Ez a csökkenti annak valószínűségét, hogy a "Felfüggesztve" videó jelenik meg, amíg a Windows Media player letölti a következő néhány másodpercet (más néven pufferelési) lejátszásához megjelenítő. Csökkenti a hálózati túlzsúfolt, mint a videolejátszó viszont visszatérhet magasabb minőség adatfolyam. Ez azt jelenti, hogy a szükséges sávszélességet alkalmazkodás is eredményez videó gyorsabb kezdő időpontját. Ez azt jelenti, hogy az első néhány másodpercet játszhatók le egy gyors – letöltés alsó minőség szegmens majd felfelé nagyobb egyszer elegendő minőség-tartalom lépés van már pufferelt és.

Dash.js egy Megnyitás MPEG-vonal videolejátszó JavaScript nyelven íródott. A cél, adja meg, amelyek szabadon felhasználható videolejátszás igénylő alkalmazások robusztus, platformok lejátszó. MPEG-vonal lejátszás bármilyen böngészőben, amely támogatja a W3C Media forrás bővítmények (MSE), a ma, Chrome, Microsoft Edge és IE11 (más böngészők jelezte a szándék a támogatási MSE) biztosít. Js DASH.js kapcsolatos további tudnivalókért lásd: a a GitHub dash.js tárat.


##<a name="creating-a-browser-based-streaming-video-player"></a>Böngészőalapú adatfolyam videolejátszó létrehozása

Hozzon létre egy egyszerű weblap, amely megjeleníti a videolejátszó várt típusú vezérlők ilyen a lejátszás, szünet, Visszatekerés stb, meg kell:

1. HTML-lap létrehozása
1. A videoklip címkének hozzáadása
1. A dash.js Windows Media player hozzáadása
1. A Windows Media player inicializálni
1. A CSS-stílusok alkalmazása
1. Tekintse meg az eredményeket, amely MSE a böngészőben

A Windows Media player inicializálása is fejeződhet csak néhány JavaScript-kód sorait. Dash.js használ, akkor valóban, amely egyszerű MPEG-vonal videó beágyazása a böngészőalapú alkalmazásokat.

##<a name="creating-the-html-page"></a>A HTML-lap létrehozása

Első lépésként, az alábbi példa bemutatja a szabványos HTML-lapot, amely a **Videó** elem basicPlayer.html, a fájl létrehozásához:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>A DASH.js Windows Media Player hozzáadása

Hivatkozás dash.js végrehajtása az alkalmazás hozzáadásához kell ragadni dash.js project 1,0 kiadását dash.all.js fájlból. Ez célszerű lehet a JavaScript mappába menti az alkalmazás. A fájl gyűjti össze a közös az összes szükséges dash.js kód egyetlen fájlba kényelmesebbé fájl. Esetén nézze meg a dash.js tárházba körül fog keresse meg az egyes fájlokat, tesztelje a kódot és sok más, de ha az összes elvégzendő használata dash.js, akkor dash.all.js fájl, amit keres.

Az alkalmazások dash.js Windows Media player hozzáadásához fel egy parancsfájl basicPlayer.html központi szakaszához:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Ezután hozzon létre a Windows Media player inicializálni, a lap betöltésekor függvényt. A sor, amelyben dash.all.js betöltése után, adja hozzá a következőt:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Ez a funkció először létrehoz egy DashContext. Konfigurálja az alkalmazást az egy adott futtatókörnyezet szolgál. Műszaki szempontból megadja az osztályok, amelyet az függőség utasítások beszúrását keretrendszer kell használnia, amikor az alkalmazás megépítése. A legtöbb esetben az Dash.di.DashContext fogja használni.

Ezután hozható létre az elsődleges osztály dash.js keretrendszer MediaPlayer. Az osztály tartalmazza az alapvető, például szükséges módszerek játszhat le, és mutasson az egérrel, kezeli a kapcsolatot a videó elemmel és is kezeli a médiafájlok bemutató leírás (MPD) fájlt, amely játszható le a videó ismerteti értelmezését.

Annak érdekében, hogy a Windows Media player játszható le a videó a startup() függvény MediaPlayer osztály neve. Többek között ezt a funkciót biztosítja, hogy a szükséges osztályok (a környezet alapján meghatározott) lett betöltve. Amikor elkészült a Windows Media player, akkor csatolhat a videó elem a attachView() függvény használatával. Az elem helyezhet el a video-adatfolyam és is szabályozhatja a lejátszást szükség szerint a MediaPlayer szolgáltatás lehetővé teszi.

A MPD fájl URL-CÍMÉT a MediaPlayer át, hogy tudja, a videóra vonatkozó lejátszásához várhatóan. Miután az oldal teljesen betöltött futtatható az imént létrehozott setupVideo() függvény lesz szüksége. Ehhez a törzs elem betöltésre használatával. Módosítása a <body> elem:

    <body onload="setupVideo()">

Végül adja meg a videó elem stíluslapokkal méretét. Adatfolyam adaptív környezetben ennek oka különösen fontos lejátszását a videó mérete is megváltozhat lejátszás alkalmazkodik keresztül a hálózati feltételek szerint. E egyszerű bemutatóból egyszerűen kényszerítse a videó elem alatt szeretné tenni a rendelkezésre álló böngészőablakban 80 %-át a következő egyéni Stíluslap hozzáadása a központi csoport a lapon:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Videó lejátszása

Videó lejátszása, mutasson a basicPlayback.html fájlt a böngészőben, és megjelenik a videolejátszó a lejátszás gombra.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Lásd még:

[Videolejátszó alkalmazások fejlesztői számára](media-services-develop-video-players.md)

[GitHub dash.js tárházba](https://github.com/Dash-Industry-Forum/dash.js) 
