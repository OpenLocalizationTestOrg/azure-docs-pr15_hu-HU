<properties 
    pageTitle="A Megnyitás Media Framework zökkenőmentes adatfolyam beépülő modul" 
    description="Megtudhatja, hogy miként az Azure Media Services zökkenőmentes Streaming beépülő modul használata az Adobe megnyitott forrás Media keretrendszer." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Az Adobe Megnyitás Media keretrendszer adatfolyam-bővítmény a Microsoft zökkenőmentes használata

##<a name="overview"></a>– Áttekintés


A Microsoft zökkenőmentes Streaming beépülő modul megnyitása forrás Media 2.0-s (KK az OSMF) OSMF alapértelmezett bővíti, és új és meglévő OSMF játékosokat tartalom lejátszás Microsoft zökkenőmentes Streaming felveszi. A beépülő modul is hozzáadja zökkenőmentes Streaming lejátszás funkciók villogó Media lejátszás (SMP).

SS OSMF a beépülő modul két verziója tartalmazza:

- Statikus zökkenőmentes Streaming beépülő modul OSMF (.swc)

- Dinamikus zökkenőmentes Streaming beépülő modul OSMF (.SWF formátumba)

A dokumentum tartalma feltételezi, hogy az olvasó van-e egy általános ismeretek OSMF és OSMF bővítmények. További információt a OSMF című át a [hivatalos OSMF webhely](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Zökkenőmentes adatfolyam beépülő modul OSMF 2.0-s verziója

A bővítmény betöltése és igény szerinti zökkenőmentes Streaming tartalom, a következő szolgáltatásokkal lejátszás támogatja:

- Igény szerinti zökkenőmentes Streaming lejátszás (a lejátszás, szünet, Seek, leállítása)
- Élő zökkenőmentes Streaming lejátszás (a lejátszás)
- Élő DVR függvények (szünet, Seek DVR lejátszás, Ugrás-to-Live.)
- Videokodekek - H.264 támogatása
- Hang kodekek - AAC támogatása
- Váltás a OSMF beépített API-khoz hang többnyelvű
- Max lejátszásának minőségét kijelölésű OSMF beépített API-hoz
- Oldalkocsi lezárt feliratokat tartalmazó OSMF feliratok beépülő modul
- Adobe&reg; Flash&reg; Player 11.4 vagy újabb verziójában.
- Ez csak verzióval OSMF 2.0-s verziója.

## <a name="supported-features-and-known-issues"></a>Ismert problémák és a támogatott szolgáltatások

Teljes listáját a támogatott szolgáltatások, a nem támogatott szolgáltatások és az ismert problémák olvassa el [ezt a dokumentumot](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>A bővítmény betöltése
Statikus (időben fordítási) OSMF bővítmények tölthető vagy dinamikusan (futásidőben). A folyamatos zökkenőmentes beépülő modul OSMF letöltési dinamikus és statikus egyaránt tartalmaz.

- Statikus betöltése: statikus betöltéséhez, statikus library (SWC) fájl szükség. Statikus bővítmények fordítási időben bekerülnek a projektek és egyesítés belül a végleges kimeneti fájl mutató hivatkozás.

- Dinamikus betöltése: dinamikusan betöltéséhez, lefordított (SWF) fájl szükség. Dinamikus bővítmények töltődnek be a Runtime és nem szerepel a projekt eredménye. (Lefordított kimenet) Dinamikus bővítmények tölthető HTTP és a fájl protokollok használatával.

Statikus és dinamikus betöltése a további tudnivalókért lásd: a hivatalos [OSMF beépülő modul lapot](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>A OSMF statikus betöltése SS
Az alábbi kódrészletet megtudhatja, hogy miként statikus az OSMF töltse be a SS bővítményt, és OSMF MediaFactory osztály használatával egyszerű videó lejátszása. Többek között a OSMF kód SS, előtt győződjön meg arról, hogy a projekt összefoglaló tartalmazza a "MSAdaptiveStreamingPlugin v1.0.3, osmf2.0.swc" statikus bővítményt.

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>A dinamikus betöltése OSMF SS

Az alábbi kódrészletet mutatja az SS bővítmény betöltése OSMF az dinamikusan és játszhat le egy basic videó a OSMF MediaFactory osztály használatával. Többek között a OSMF kód SS, mielőtt átmásolhatja a "MSAdaptiveStreamingPlugin v1.0.3, osmf2.0.swf" dinamikus beépülő modul a projekt mappába Ha töltse be a fájl protokollon keresztül szeretné, vagy a HTTP-terhelés webkiszolgálóra. Nincs szükség az "MSAdaptiveStreamingPlugin v1.0.3, osmf2.0.swc" szerepeltetni a project hivatkozásokat.

 
{csomag
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Villogó lejátszás az SS ODMF dinamikus a beépülő modul

Zökkenőmentes Streaming OSMF dinamikus bővítmény kompatibilis [Villogó Media lejátszás (SMP)](http://osmf.org/strobe_mediaplayback.html). A bővítmény OSMF SS tartalom lejátszás zökkenőmentes Streaming hozzáadása SMP is használhatja. Ehhez másolja a "MSAdaptiveStreamingPlugin v1.0.3, osmf2.0.swf" csoportban az alábbi lépésekkel HTTP-terhelés webkiszolgálóra.

1.  Tallózással keresse meg a [villogó lejátszás beállítási lapra](http://osmf.org/dev/2.0gm/setup.html). 
2.  Állítsa az src zökkenőmentes Streaming forrást, (pl. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Hajtsa végre a kívánt konfiguráció módosításokat, majd kattintson a kép és frissítés.
 
    **Megjegyzés:** A tartalom webkiszolgáló szüksége van egy érvényes crossdomain.xml. 
4.  Másolja és illessze be a kódot, és egy egyszerű HTML-lapok használata a kedvenc szövegszerkesztőben, például az alábbi példában:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Zökkenőmentes Streaming OSMF-bővítmény felvétele a beágyazási kódra, és mentse.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  A HTML-lapok mentése és közzététele a webkiszolgálóra történő. Tallózással keresse meg a közzétett weblapot a kedvenc Flash&reg; Player engedélyezett internetböngésző (Internet Explorer, a Chrome, Firefox, stb.).
7.  Zökkenőmentes Streaming tartalom belül Adobe élvezzenek&reg; Flash&reg; Player.

Általános OSMF fejlesztési lásd a hivatalos [OSMF fejlesztését lapon](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Lásd még:

[A Microsoft adaptív beépülő modul Streaming OSMF frissítése](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
