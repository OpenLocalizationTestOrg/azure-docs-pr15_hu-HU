<properties 
    pageTitle="Ügyféloldali hirdetések beszúrása |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan szúrhat be az ügyféloldali hirdetések." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Ügyféloldali hirdetések beszúrása

Ez a témakör a különböző típusú ügyféloldali hirdetések beszúrása olvashat.

A folyamatos átvitelű videók Live kódolt feliratok és az Active Directory támogatási információt lásd: [támogatott kódolt feliratok és az Active Directory beszúrási szabványoknak](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player jelenleg nem támogatja a hirdetések.

##<a id="insert_ads_into_media"></a>A médiafájlok hirdetések beszúrásával

Azure Media Services támogatja a Windows Media-platformon keresztül ad beszúrási: Player keretek. Active Directory-támogatással Player keretek a Windows 8-ban, a Silverlight, a Windows Phone 8 és az iOS-eszközökhöz érhetők el. Minden egyes player keretrendszer példakódot arról, hogy miként tudnak megvalósítani a lejátszó alkalmazást tartalmazza. Vannak olyan három különböző típusú hirdetések szeretne beszúrni a médiafájlok: listában.

- **Lineáris** – teljes keret hirdetések, mutasson a fő videót.
- A Windows Media player **Nonlinear** – átfedő hirdetések, amelyek akkor jelennek meg, mint a fő videó lejátszása, általában emblémát vagy más statikus kép mutatnia.
- **Kereső** – jelennek meg a Windows Media player kívüli hirdetések.

A fő videó idővonal bármely pontján hirdetések lehet ügyfélwebhelyre. A Windows Media player kell mondani, mikor játszható le a ad, és a mely hirdetések lejátszása. Ehhez használja a szabványos XML-alapú fájlokat: videó Ad szolgáltatás sablon (VAST), a digitális videó több Ad lista (VMAP), a Media absztrakt szekvenálási sablon (OSZLOPOS) és a digitális videó Player Ad felület Definition (VPAID). NAGY fájlok adja meg, hogy milyen hirdetések megjelenítéséhez. VMAP fájlok adja meg a különböző hirdetések lejátszása és DÖNTŐ XML tartalmaz. A fájlok OSZLOPOS úgy is szekvencia hirdetések, amely is tartalmazhat DÖNTŐ XML. VPAID fájlokat a videolejátszó és az Active Directory vagy az Active Directory kiszolgáló közötti felület határozza meg.

Minden player keretrendszer másképp működik, és minden egyes vonatkozik a saját témakör. Ez a témakör leírja, az egyszerű mechanizmusok használatával Beszúrás hirdetések. Videolejátszó alkalmazások hirdetések kérése az Active Directory-kiszolgálóról. Az Active Directory-kiszolgáló többféle módon is válaszolhat:

- Vissza a nagy fájlok
- Vissza a VMAP fájl (az beágyazott VAST)
- Vissza a OSZLOPOS fájl (az beágyazott VAST)
- Vissza a nagy fájl az VPAID hirdetések

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Videó Ad szolgáltatás sablon (VAST) fájl használatával

NAGY fájlok Itt adhatja meg, milyen Active Directory vagy a hirdetések megjelenítéséhez. A következő XML egy lineáris ad nagy fájlok példája:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
A lineáris ad le a **<Linear>** elemet. Azt adja meg, az Active Directory időtartamát, a nyomon követés, kattintson a követés gombra, és számos keresztül **<MediaFile>** elemeket. A nyomon követés események adható belül a **<TrackingEvents>** elemet, és lehetővé teszi az ad-kiszolgáló az Active Directory megtekintésekor különböző események nyomon követéséhez. Ebben az esetben a start és a középpont befejezése után, és bontsa ki a események követik nyomon. A kezdő esemény akkor fordul elő, ha az Active Directory jelenik meg. A középpont esemény fordul elő, ha legalább az Active Directory ütemterv 50 %-os már megtekintett. A teljes esemény akkor fordul elő, ha az Active Directory futtatása végén. A kibontás esemény fordul elő, amikor a felhasználó kibővíti a videolejátszó teljes képernyőre. Clickthroughs vannak megadva, a **<ClickThrough>** elemének egy **<VideoClicks>** elemet, és adja meg az erőforráshoz szeretne megjeleníteni, amikor a felhasználó kattint az Active Directory URI. ClickTracking megadott egy **<ClickTracking>** elem is belül a **<VideoClicks>** elemet, és adja meg a Windows Media Player kérni, amikor a felhasználó kattint, az Active Directory a nyomon követés erőforrás. A **<MediaFile>** elemeket, adja meg az ad egy adott kódolást adatait. Ha egynél több **<MediaFile>** elem, a videolejátszó választhat a legjobb kódolást a platform. 

Lineáris hirdetések meghatározott sorrendben jeleníthetők meg. Ehhez a további hozzáadása <Ad> a VAST elemek fájl, és adja meg a sorrendben, a sorrend attribútum használatával. A következő példa bemutatja ezt:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Nemlineáris hirdetések meg vannak adva a <Creative> elemet is. Az alábbi példában látható egy <Creative> elemet, amelyet egy lineáris ad ismerteti.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
A **<NonLinearAds>** elemet is tartalmazhat, egy vagy több **<NonLinear>** elemek, amelyek leírhatja a lineáris ad. A **<NonLinear>** elem azt adja meg az erőforráshoz az nemlineáris ad. Az erőforrás lehet egy **<StaticResouce>**, egy **<IFrameResource>**, vagy egy **<HTMLResouce>**.**<StaticResource>** a HTML erőforrás ismerteti, és egy creativeType attribútum, amely meghatározza, hogy hogyan jelenjen meg az erőforrásra határozza meg:

Kép vagy gif, kép/jpeg, kép vagy png – HTML jelenik meg az erőforrásra **<img>** címke.

Alkalmazás/x-javascript – az erőforrás-<**parancsfájl**> HTML-kód jelenik meg.

A Flash player alkalmazás/x-shockwave-flash – az erőforrás jelenik meg.

**<IFrameResource>**egy, amely megjeleníthető az IFrame HTML-erőforrás ismerteti. **<HTMLResource>**az weblapon beilleszthető HTML-kód valamilyen ismerteti. **<TrackingEvents>**Adja meg a nyomon követés események és az URI kérni az esemény esetén. Ez a példa a acceptInvitation és összecsukás események nyomon követi. További információt a **<NonLinearAds>** elem és a hozzá tartozó gyermekwebhelyek, olvassa el a IAB.NET/VAST című témakört. Megjegyzendő, hogy a **<TrackingEvents>** elem található, a** <NonLinearAds> ** elem helyett a **<NonLinear>** elemet.

Companion hirdetések definiált belül egy <CompanionAds> elemet. A <CompanionAds> elemet is tartalmazhat, egy vagy több <Companion> elemeket. Minden egyes <Companion> elemet a kereső Active Directory ismertetése, valamint tartalmazhat egy <StaticResource>, <IFrameResource>, vagy <HTMLResource> amelyek megadott módon, mint ahogy egy lineáris ad. NAGY fájl tartalmazhat több companion hirdetések, és megadhatja, hogy a lejátszó alkalmazást a legmegfelelőbb ad megjelenítéséhez. VAST kapcsolatos további tudnivalókért olvassa el a [nagy 3.0](http://www.iab.net/media/file/VASTv3.0.pdf)című témakört.

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Digitális videó több Active Directory-lista (VMAP) fájl használatával

VMAP fájl lehetővé teszi, hogy adja meg, amikor ad oldaltörések fordul elő, mennyi ideig minden oldaltörés, hány hirdetések jeleníthető meg töréspont belül, és hirdetések típusú lehet töréspont alatt jelenik meg. Egy példa VMAP fájl, amely definiálja egyetlen ad Töréspont az alábbiakat:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
VMAP fájl kezdődik egy <VMAP> elem, amely tartalmazza az egy vagy több <AdBreak> elemek, az Active Directory oldaltörés definiálása. Egyes ad szünetek megadja egy oldaltörés típusa, oldaltörés Azonosítóját és eltolódás. A breakType attribútum az adja meg az Active Directory, a szünet során lejátszható: lineáris, nemlineáris, vagy megjelenítéséhez. Hirdetések térkép megjelenítése DÖNTŐ companion hirdetések. Egynél több ad típusa (szóköz nélkül) vesszővel tagolt lista adható meg. A breakID pedig az Active Directory azonosítója, amelyet a nem kötelező. A timeOffset adja meg, hogy mikor jelenjen meg az ad. Ez a következő módon kell megadni:

1. Idő ezredmásodperc .mmm esetén óó vagy hh:mm:ss.mmm formátumban. Az érték az attribútum megadja az Active Directory oldaltörést elejére a videó ütemterv az elejétől időpontját.
1. Százalék – n % formátumban Ha n az a százalékérték a videó ütemterv az Active Directory lejátszás előtt lejátszása
1. Kezdete/vége – itt adhatja meg, hogy az Active Directory megjelenjen-e, előtt vagy után megjelenik a videó
1. Vigye – ad oldaltörések sorrendjét határozza meg, amikor az Active Directory oldaltörések időzítésének ismeretlen, például live streaming. Egyes ad szünetek sorrendjének hol található az n a egész szám 1, vagy nagyobb #n formátumban van megadva. 1 azt jelzi, hogy az Active Directory célszerű lehet lejátszani az első alkalommal 2 azt jelzi, hogy az Active Directory kell játszható le a második lehetőséget, és így tovább.

Az <**AdBreak**> elemben lehet egy <**AdSource**> elemet. Az <**AdSource**> elemet tartalmazza a következő attribútumok:

1. Azonosító – Itt adhatja meg a forrást ad az azonosító
1. allowMultipleAds – egy logikai érték, amely meghatározza, hogy több hirdetések is megjeleníthetők legyenek alatt az Active Directory-törés
1. followRedirects – egy választható logikai érték, amely meghatározza, ha elfogadja kell-e a videolejátszó átirányítja belül hibát ad választ.

Az <**AdSource**> elemet a Windows Media player biztosít, beágyazott ad választ vagy a hivatkozás a hibát ad választ. A következő elemek közül is tartalmazza:

- <VASTAdData>azt jelzi, hogy DÖNTŐ ad választ a VMAP fájlban van beágyazva.
- <AdTagURI>más rendszerekből az Active Directory-válasz hivatkozó URI
- <CustomAdData>– egy tetszőleges karakterlánc adott respresents nem DÖNTŐ visszajelzés

Ebben a példában a soros ad választ a megadott egy <VASTAdData> elem, amely tartalmazza a nagy ad választ. Az egyéb elemek kapcsolatos további tudnivalókért olvassa el a [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap)című témakört.

Az <**AdBreak**> elemet is tartalmazhatnak egy <**TrackingEvents**> elemet. A <**TrackingEvents**> elem lehetővé teszi, a a elejére vagy végére az Active Directory szünet vagy e hiba történt-e az Active Directory oldaltörést nyomon követéséhez. A <**TrackingEvents**> elem egy vagy több <**követés**> elemet tartalmaz, amelyek a nyomon követés esemény és a nyomon követés URI határozza meg. A lehetséges nyomon követés eseményeket a következők:

1. breakStart – követi nyomon az Active Directory oldaltörés elejére
1. breakEnd – az Active Directory oldaltörés kiegészítésének nyomon követése
1. hibaüzenet – nyomon követi-az Active Directory oldaltörést során fellépő hiba

A következő példa bemutatja egy VMAP fájlt, amely meghatározza az események nyomon követése

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

A <**TrackingEvents**> elem és a hozzá tartozó gyermekwebhelyek a további tudnivalókért olvassa el a http://iab.org/VMAP.pdf című témakört.

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>A sablonfájlt (OSZLOPOS) szekvenálási Media absztrakt használatával

Egy OSZLOPOS fájl eseményindítók, amikor megjelenik az Active Directory meghatározó megadását teszi lehetővé. Egy példa OSZLOPOS fájl, amely tartalmazza a indítók egy előtti Filmtekercs ad, a közép Filmtekercs Active Directory és a utáni Filmtekercs ad a következő:

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Egy OSZLOPOS fájl kezdődik egy **<MAST>** elem, amely tartalmazza az egyik **<triggers>** elemet. A <triggers> elem tartalmaz egy vagy több **<trigger>** elemeket, amelyek meghatározzák, hogy mikor lehet lejátszani az ad. 

A **<trigger>** elem tartalmaz egy **<startConditions>** elemre, amely adja meg, amikor az Active Directory játszható le kell kezdődnie. A **<startConditions>** elem tartalmaz egy vagy több <condition> elemeket. Ha minden <condition> eredménye IGAZ, az eseményindító kezdeményezni, illetve visszavont attól függően, hogy a <condition> belül található egy **< startConditions**> vagy **<endConditions>** elem rendre. Ha több <condition> elemek szerepelnek, azok számít egy implicit vagy, bármely feltétel igaz kiértékelése hatására az eseményindító elindítására. <condition>elemek egymásba. Ha a gyermek <condition> elemek előre be van állítva, azok számít egy implicit és, minden feltétel kiértékelésének eredménye IGAZ, az eseményindító kezdeményezése. A <condition> elemet tartalmazza a következő attribútumok, amely a feltétel megadása: 

1. **típus** – az adja meg a feltételt, esemény vagy tulajdonság
1. **név** – a tulajdonság vagy a kiértékelés során használandó esemény neve
1. **érték** , az értéket, amely tulajdonság kiértékelésre kerül ellen
1. **operátor** – a művelet során kiértékelés: EQ (egyenlőségjel), a NEQ (nem egyenlő), a GTR (nagyobb), a GEQ (nagyobb vagy egyenlő), a LT (kisebb, mint), LEQ (kisebb vagy egyenlő), a maradék (moduló)

**<endConditions>**tartalmazó <condition> elemeket. Amikor egy feltétel kiértékelésének eredménye igaz az eseményindító alaphelyzetbe állítása. A <trigger> elem is tartalmaz egy <sources> elem, amely tartalmazza az egy vagy több <source> elemeket. A <source> elemek meghatározása a URI az Active Directory-válasz és az Active Directory-válasz típusú. Ebben a példában egy URI adandó DÖNTŐ választ. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>A videó ADFS-Player felület Definition (VPAID)

VPAID végrehajtható ad egységek a videolejátszó kommunikáció engedélyezése egy API-t nem. Ezzel a magas fokú interaktivitás ad élményt. A felhasználó használhatják az Active Directory és az Active Directory reagálni tud a megjelenítő által végzett műveletek. Például az Active Directory jeleníthet meg, amelyek lehetővé teszik a felhasználó számára további információt és az Active Directory hosszabb verziójának megtekintése gomb. A videolejátszó támogatnia kell az VPAID API, és a végrehajtható ad az API kell végrehajtása. Ha egy kéri az ad egy Active Directory-kiszolgálóról a kiszolgáló, amely tartalmazza a VPAID ad DÖNTŐ választ jelenhetnek meg.

Az végrehajtható ad kódot, például Adobe Flash™ vagy a böngészőben végrehajtható JavaScript futtatókörnyezet környezetben végre kell hajtani jön létre. Az Active Directory-kiszolgáló tartalmazó egy VPAID ad DÖNTŐ választ ad vissza, ha a apiFramework értékének attribútum a <MediaFile> elem "VPAID" kell lennie. Az attribútum határozza meg, hogy a benne lévő ad egy VPAID végrehajtható ad. A type attribútum a végrehajtható fájl, például "alkalmazás/x-shockwave-flash" vagy "alkalmazás/x-javascript" MIME típusú kell beállítani. Az alábbi XML-kódtöredékének látható a <MediaFile> egy VPAID végrehajtható ad tartalmazó DÖNTŐ választ elemét. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Az végrehajtható ad használatával sikerült inicializálni az <AdParameters> elemének a <Linear> vagy <NonLinear> elemeit nagy választ. További információt a <AdParameters> elem [DÖNTŐ 3.0](http://www.iab.net/media/file/VASTv3.0.pdf)látható. A VPAID API-val kapcsolatos további tudnivalókért olvassa el a [VPAID 2.0-s](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf)című témakört.

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>A Windows vagy a Windows Phone 8 Player Active Directory-támogatással végrehajtása

A Microsoft Media Platform: Player keretrendszer a Windows 8 és Windows Phone 8 tartalmaz, amely megmutatja, hogyan tudnak megvalósítani a videolejátszó alkalmazást keretében minta alkalmazások gyűjteménye. A minták és Player keretében letöltése [Player keretrendszer a Windows 8 és Windows Phone 8](https://playerframework.codeplex.com).

A Microsoft.PlayerFramework.Xaml.Samples megoldást megnyitásakor láthatja a projekt található mappák számos. A hirdetési mappára vonatkozó, a videolejátszó létrehozása az Active Directory-támogatással példakódot tartalmazza. A hirdetések belül az mappát, számos XAML/cs fájlt, amelyek bemutatják, hogyan hirdetések beszúrása eltérő módon. A következő lista ismerteti az egyes:

- AdPodPage.xaml szemlélteti, hogyan lehet az Active Directory pod megjelenítéséhez.
- AdSchedulingPage.xaml hirdetések ütemezése mutatja.
- FreeWheelPage.xaml megtudhatja, hogy miként hirdetések ütemezéséhez használja a FreeWheel bővítményt.
- MastPage.xaml megtudhatja, hogy miként ütemezhet hirdetések OSZLOPOS fájl.
- ProgrammaticAdPage.xaml programozás útján ütemezése hirdetések videóvá mutatja.
- ScheduleClipPage.xaml szemlélteti, hogyan ütemezhet az ad egy nagy fájl nélkül.
- VastLinearCompanionPage.xaml jeleníti meg, hogyan szúrhat be a lineáris és companion ad.
- VastNonLinearPage.xaml szemlélteti, hogyan szeretne beszúrni egy nemlineáris ad.
- VmapPage.xaml megtudhatja, hogy miként adja meg a hirdetések VMAP-fájl segítségével.

Minden egyes e minták használja a lejátszó keretrendszer MediaPlayer kódokban. A legtöbb minták, hogy támogassa az Active Directory-válasz különböző formátumokban bővítmények használata. A ProgrammaticAdPage minta programozás útján kommunikál egy MediaPlayer példányt.

###<a name="adpodpage-sample"></a>AdPodPage minta

Ez a példa a AdSchedulerPlugin használja mikor jelenjenek meg az ad meg. Ebben a példában a közép Filmtekercs hirdetési 5 másodperc után lejátszható van ütemezve. Az Active Directory pod (ha sorrendben szeretné megjeleníteni a hirdetések csoportjára) ad-kiszolgáló által visszaadott nagy fájlok van megadva. A URI nagy a fájl meg van adva a <RemoteAdSource> elemet.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

A AdSchedulerPlugin kapcsolatos további tudnivalókért olvassa el a [Windows 8 vagy Windows Phone 8 Player keretében hirdetési](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation) című témakört.

###<a name="adschedulingpage"></a>AdSchedulingPage

Ez a példa is használja a AdSchedulerPlugin. Ütemezi a három hirdetések, egy előtti Filmtekercs Active Directory, a közép Filmtekercs Active Directory és a utáni Filmtekercs ad. Az egyes Active Directory számára a VAST való URI megadott egy <RemoteAdSource> elemet.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

Ez a példa a FreeWheelPlugin, amely meghatározza, hogy egy adatforrás attribútum, amely meghatározza, amely mutat, amely meghatározza az Active Directory tartalom, valamint az ütemezési információk ad SmartXML fájl URI használja.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

Ez a példa a MastSchedulerPlugin, amely lehetővé teszi, hogy egy OSZLOPOS fájlt használ. Az adatforrás attribútum határozza meg, hogy a OSZLOPOS fájl helyét.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

Ez a példa a MediaPlayer programozás útján kommunikál. A ProgrammaticAdPage.xaml fájl elindítja a MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

A ProgrammaticAdPage.xaml.cs fájlt hoz létre egy AdHandlerPlugin, adja meg, hogy egy TimelineMarker hozzáadása az ad jelenjen meg, és hozzáadja a MarkerReached esemény, mely egy nagy fájlba URI megadása RemoteAdSource betölti, és majd lejátssza az Active Directory leíró.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

Ez a példa a AdSchedulerPlugin használja a közép Filmtekercs ad ütemezése .wmv fájl tartalmaz, az Active Directory megadásával.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

Ez a példa szemlélteti, hogyan kell a AdSchedulerPlugin egy midsize Filmtekercs lineáris Active Directory-companion ad a ütemezéséhez használja. A <RemoteAdSource> elemet a nagy fájl helyét adja meg.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

Ez a példa a AdSchedulerPlugin ütemezése egy lineáris és egy nem lineáris ad használja. A nagy fájl helyét van megadva, az a <RemoteAdSource> elemet.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

A minták használja a VmapSchedulerPlugin ütemezése hirdetések VMAP fájl használatával. Az adatforrás attribútuma a VMAP fájlt az URI van megadva a <VmapSchedulerPlugin> elemet.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Az Active Directory-támogatással videolejátszó iOS végrehajtása


A Microsoft Media Platform: A lejátszó keretrendszer IOS gyűjteménye, amely megmutatja, hogyan tudnak megvalósítani a videolejátszó alkalmazást keretében minta alkalmazások tartalmazza. Az [Azure Media Player keretrendszer](https://github.com/Azure/azure-media-player-framework)letöltheti és a minták Player keretében. A github lapon van, amely tartalmaz további tájékoztatást a lejátszó keretrendszer wikilap mutató hivatkozást, és a lejátszó minta bemutatása: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Az ütemezési VMAP hirdetések

A következő példa bemutatja, hogyan ütemezése hirdetések VMAP fájl használatával.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Az ütemezési VAST hirdetések

A következő példa bemutatja, hogyan ütemezése egy késő kötés DÖNTŐ ad.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   A következő példa bemutatja, hogyan ütemezése az korai kötés DÖNTŐ ad.
Példa: 4 ütemezés korai kötés DÖNTŐ Active Directory-//Download a VAST fájl, ha (! [ framework.adResolver downloadManifest: és nyilvánvalóan withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[saját logFrameworkError];} egyéb {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Az alábbi példa szemlélteti, hogyan lehet az ad durva kivágása szerkesztése (RCE) használatával Beszúrás

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

A következő példa bemutatja az Active Directory pod ütemezéséről.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

A következő példa bemutatja, hogyan ütemezése egy nem öntapadós midsize Filmtekercs ad. Egy nem öntapadós ad csak lejátszani, miután függetlenül minden keresése a megjelenítő hajt végre.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

A következő példa bemutatja, hogyan ütemezése egy öntapadós midsize Filmtekercs ad. Egy öntapadós ad minden alkalommal, amikor a videó ütemterv adott pontjához elérésekor fog megjelenni.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


A következő példa bemutatja, hogyan ütemezése egy utáni Filmtekercs ad.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

A következő példa bemutatja, hogyan ütemezése egy előtti Filmtekercs ad.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

A következő példa bemutatja, hogyan ütemezése egy midsize Filmtekercs átfedő ad.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Lásd még:

[Videolejátszó alkalmazások fejlesztői számára](media-services-develop-video-players.md)
