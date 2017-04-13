<properties 
    pageTitle="A folyamatos átvitelű Windows áruház-alkalmazás oktatóprogram sima |} Microsoft Azure" 
    description="Megtudhatja, hogy miként C# Windows áruház-alkalmazás tartalom lejátszás zökkenőmentes adatfolyam XML MediaElement vezérlőelem létrehozása az Azure Media Services segítségével." 
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



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>A folyamatos átvitelű Windows áruház-alkalmazás zavartalan létrehozásának

A zökkenőmentes Streaming ügyfél SDK a Windows 8 lehetővé teszi, hogy a fejlesztők számára, amely képes igény szerinti élő zökkenőmentes Streaming tartalmak és a Windows áruházból alkalmazásokat. Nemcsak a egyszerű lejátszását zökkenőmentes Streaming tartalom, a SDK is elérhetővé teszi gazdag funkciók, mint Microsoft PlayReady védelem szintű korlátozás, Live DVR hangminőség-adatfolyam váltás, figyeli állapotfrissítések (például a minőség webhelyszintű módosításokat végezhet) és a hiba események, és így tovább. A támogatott szolgáltatások további tudnivalókért olvassa el a [kibocsátási megjegyzések](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes)című témakört. További tudnivalókért lásd: a [Windows 8 Player keretrendszer](http://playerframework.codeplex.com/). 

Ebben az oktatóanyagban négy órákhoz tartalmazza:

1. Egyszerű zökkenőmentes adatfolyam áruház-alkalmazás létrehozása
2. Médiafájlok végrehajtását szabályozhatja csúszka hozzáadása
3. Jelölje ki a folyamatos zökkenőmentes adatfolyamok
4. Jelölje ki a zökkenőmentes adatfolyam-s zeneszámok

##<a name="prerequisites"></a>Előfeltételek

- A Windows 8 32 bites vagy 64 bites. [A Windows 8 vállalati kiértékelés](http://msdn.microsoft.com/evalcenter/jj554510.aspx) MSDN elérheti.
- Visual Studio 2012 vagy Visual Studio Express 2012 (vagy újabb verziója). Amely letölthető a próbaverzió [Itt](http://www.microsoft.com/visualstudio/11/downloads).
- [A folyamatos átvitelű ügyfél Windows 8 SDK Microsoft sima](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


A kész megoldás leckék letölthető MSDN Developer mintakódok (kód gyűjtemény): 

- [Lecketervező 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – egy egyszerű a Windows 8 sima Streaming Media Player alkalmazásban 
- [Lecketervező 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – egy egyszerű a Windows 8 zökkenőmentes Streaming Media Player a csúszka beállításához 
- [Lecketervező 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – a folyamatos átvitelű Media Player adatfolyam kijelölt, a Windows 8 zavartalan  
- [Lecketervező 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – a folyamatos átvitelű Media Player kijelölt nyomon követése a Windows 8 zavartalan.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>1 a lecke: Egyszerű zökkenőmentes adatfolyam áruház-alkalmazás létrehozása

Ez a lecke létrehoz egy Windows áruház-alkalmazás MediaElement vezérlőelemhez zökkenőmentes adatfolyam lejátszásához a tartalom.  A futó alkalmazás néz ki:

![Zökkenőmentes adatfolyam Windows áruház-alkalmazás például][PlayerApplication]
 
További információt a Windows áruház-alkalmazás elkészítésének olvassa el a [kidolgozása kiválóan alkalmazásokat verzió Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx)című témakört. Ez a lecke az alábbi eljárások valamelyikét tartalmazza:

1.  A Windows áruházból projekt létrehozása
2.  A felhasználói felület (XAML) tervezése
3.  A kód mögött fájl módosítása
4.  Állíthat össze, és az alkalmazás tesztelése

**A Windows áruházból projekt létrehozása**

1.  Visual Studio 2012 vagy újabb verziója fut.
2.  A **fájl** menüben kattintson az **Új**gombra, és kattintson a **Projekt**.
3.  Az új projekt párbeszédpanel írja be vagy válassza ki a következő értékeket:

név|Érték
---|---
Sablon csoport|Telepített/sablonok/Visual C# és a Windows áruházból
Sablon|Üres alkalmazás (XAML)
név|SSPlayer
Hely|C:\SSTutorials
Megoldás neve|SSPlayer
Megoldás könyvtár létrehozása|(be van jelölve)

4.  Kattintson az **OK gombra**.

**A folyamatos átvitelű ügyfél zökkenőmentes SDK mutató hivatkozás hozzáadása**

1.  Megoldás Intézőből kattintson a jobb gombbal a **SSPlayer**, és válassza a **Hivatkozás hozzáadása**gombra.
2.  Írja be vagy válassza ki a következő értékeket:

név|Érték
---|---
Viszonyítási csoport|A Windows/bővítmények
Hivatkozás|Jelölje be a Microsoft sima ügyfél SDK Streaming a Windows 8 és a Microsoft Visual C++ Runtime csomag
    
3.  Kattintson az **OK gombra**. 

Miután megadta a hivatkozásokat, ki kell választania a célként megadott platform (x64 vagy x86), hozzáadása hivatkozás nem működik az bármely Processzor platform beállításait.  A megoldás Intézőben jelenik meg e sárga figyelmeztetés be van jelölve hozzá hivatkozást.

**A lejátszó felhasználói felület tervezése**

1.  Megoldás Intézőből kattintson duplán **MainPage.xaml** nyissa meg a Tervező nézetben.
2.  Keresse meg a ** &lt;rács&gt; ** és ** &lt;/Grid&gt; ** címkék az XAML-fájlt, és illessze be a két címkék között a következő kódot:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    A MediaElement vezérlő lejátszás multimédiás használják. A csúszka sliderProgress nevű használandó a következő lecke szabályozhatja a médiafájlok előrehaladását.

3.  **CTRL + S** billentyűkombináció lenyomásával mentse a fájlt.

A MediaElement vezérlő nem támogatja a folyamatos zökkenőmentes tartalom out kész. Ha engedélyezni szeretné a folyamatos zökkenőmentes támogatás, regisztrálnia kell a folyamatos zökkenőmentes bájt-adatfolyam kezelő fájlnévkiterjesztés és MIME típusú.  Regisztrálni, használhatja a Windows.Media névtér MediaExtensionManager.RegisterByteStremHandler metódusát.

A XAML-fájl egyes eseménykezelők a vezérlők társítva.  Meg kell határoznia e eseménykezelők.

**A kód mögött fájl módosítása**

1.  Megoldás Intézőből kattintson a jobb gombbal a **MainPage.xaml**, és kattintson a **Kód megjelenítése**gombra.
2.  Kattintson a fájl lapon adja hozzá a következő utasítással:

        using Windows.Media;

3.  A **MainPage** osztály elejére, az alábbi adatokat tag felvétele:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  A **MainPage** konstruktor végén a következő két sor hozzáadása:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Végén található a **MainPage** osztály múltbeli a következő kódot:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Az sliderProgress_PointerPressed eseménykezelő itt van megadva.  Nincsenek további works kell megtenni, hajtsa végre a következő lecke: Ebben az oktatóanyagban tartozó.
6.  **CTRL + S** billentyűkombináció lenyomásával mentse a fájlt.

A kész fájl alapjául szolgáló kódot kell néz ki:

![A Visual Studio a zökkenőmentes Streaming Windows áruház-alkalmazás Codeview][CodeViewPic]

**Állíthat össze, és az alkalmazás tesztelése**

1.  A **SZERKESZTÉS** menüben kattintson a **Configuration Manager**parancsra.
2.  **Aktív megoldás platform** a fejlesztői platform megfelelően módosíthatja.
3.  Nyomja le az **F6 billentyűt** a projekt összeállításához. 
4.  Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.
5.  Az alkalmazás tetején, használja az alapértelmezett zökkenőmentes Streaming URL-címet, vagy adjon meg egy másik. 
6.  Kattintson a **forrás**gombra. **Automatikus lejátszása** alapértelmezés szerint engedélyezve van, mert a média kell játszhat le automatikusan.  A média **lejátszása**, **Szünet** és a **Stop** gomb használatával szabályozhatja, hogy.  A hangerő, a függőleges csúszkával vezérelheti.  Azonban a vízszintes csúszkát media végrehajtását biztonságkezelési teljesen még nincs implementálva. 

Lesson1 befejezése.  Ez a lecke MediaElement vezérlőelemhez lejátszás zökkenőmentes adatfolyam-tartalom használata.  A következő lecke, a zökkenőmentes Streaming tartalom végrehajtásának szabályozhatja a csúszkák ad hozzá.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>2 a lecke: Szabályozhatja Media végrehajtását csúszka hozzáadása
Lecke 1 a egy Windows áruház-alkalmazás a lejátszás médiatartalom zökkenőmentes Streaming MediaElement XAML vezérlőelem létrehozott.  Néhány egyszerű media függvények, például a Kezdés, a stop és a szünet származik.  Ez a lecke sáv csúszka hozzáadja az alkalmazást.

Ebben az oktatóanyagban fogjuk használni egy időzítő a csúszka pozíció alapján az aktuális pozíciótól a MediaElement vezérlő frissítéséhez.  A csúszka kezdési és befejezési idő is frissíteni kell a élő tartalom esetén.  Ez is jobban kezelhető adaptív forrás frissítés eseményt.

Multimédia-adatforrások olyan objektumok, multimédia-adatok készítése.  A forrás feloldó URL-cím vagy bájt adatfolyam elfoglalja, és létrehoz, hogy a tartalom forrása megfelelő media.  A forrás-feloldó a szokásos módon media forrás létrehozása az alkalmazások nem. 

Ez a lecke az alábbi eljárások valamelyikét tartalmazza:

1.  A folyamatos zökkenőmentes kezelő regisztrálása 
2.  A forrás adaptív manager szintű eseménykezelők hozzáadása
3.  A forrás adaptív eseménykezelők hozzáadása
4.  Eseménykezelők MediaElement hozzáadása
5.  Kapcsolódó vonalkód csúszka hozzáadása
6.  Állíthat össze, és az alkalmazás tesztelése

**A folyamatos zökkenőmentes bájt-adatfolyam-kezelő regisztrálhat, és a propertyset fázis**

1.  Megoldás Intézőből **MainPage.xaml**a jobb gombbal, és kattintson a **Kód megjelenítése**gombra.
2.  A fájl elején adja hozzá a következő utasítással:

        using Microsoft.Media.AdaptiveStreaming;

3.  A MainPage osztály elejére, a következő adatokat a tagok hozzáadása:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  A **MainPage** konstruktor belül Hozzáadás a következő kódot a **után. Components(); inicializálni** sor- és a regisztrációs kód soraiban írt az előző lecke:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  A **MainPage** konstruktor belül módosítása a két RegisterByteStreamHandler módon veheti fel a negyedik paraméterek:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  **CTRL + S** billentyűkombináció lenyomásával mentse a fájlt.

**Az adatforrás adaptív manager szintű eseménykezelő hozzáadása**

1.  Megoldás Intézőből **MainPage.xaml**a jobb gombbal, és kattintson a **Kód megjelenítése**gombra.
2.  A **MainPage** osztály belül a következő adatok tag felvétele:

        private AdaptiveSource adaptiveSource = null;

3.  A **MainPage** osztály végén a következő eseménykezelő hozzáadása:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  A **MainPage** konstruktor végén a következő sort a adaptív forrás open eseményt előfizetéséhez hozzáadása:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += új AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  **CTRL + S** billentyűkombináció lenyomásával mentse a fájlt.

**Szintű eseménykezelők adaptív adatforrás hozzáadása**

1.  Megoldás Intézőből **MainPage.xaml**a jobb gombbal, és kattintson a **Kód megjelenítése**gombra.
2.  A **MainPage** osztály belül a következő adatok tag felvétele:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  A **MainPage** osztály végén adja hozzá a következő eseménykezelők:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  A **mediaElement AdaptiveSourceOpened** módszerrel végén adja hozzá az események előfizetéséhez a következő kódot:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  **CTRL + S** billentyűkombináció lenyomásával mentse a fájlt.

Az azonos események adaptív forrás kezelő szint, valamint az alkalmazás az összes médiaelemek közös funkciók kezelésére szolgáló érhetők el. Minden egyes AdaptiveSource saját eseményeket tartalmazza, és az összes AdaptiveSource esemény átkerül a AdaptiveSourceManager.

**Eseménykezelők Media elem hozzáadása**

1.  Megoldás Intézőből **MainPage.xaml**a jobb gombbal, és kattintson a **Kód megjelenítése**gombra.
2.  A **MainPage** osztály végén adja hozzá a következő eseménykezelők:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  A **MainPage** konstruktor végén az események alsó index a következő kód hozzáadása:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  **CTRL + S** billentyűkombináció lenyomásával mentse a fájlt.

**Adja hozzá a csúszka kapcsolódó kódot.**

1.  Megoldás Intézőből **MainPage.xaml**a jobb gombbal, és kattintson a **Kód megjelenítése**gombra.
2.  A fájl elején adja hozzá a következő utasítással:
    
        using Windows.UI.Core;

3.  A **MainPage** osztály belül a következő adatokat a tagok hozzáadása:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  A **MainPage** konstruktor végén adja hozzá a következő kódot:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  A **MainPage** osztály végén adja hozzá a következő kódot:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Megjegyzés:** A nem felhasználói felületi szálon a felhasználói felület szál módosítások CoreDispatcher szolgál. Abban az esetben, ha feladó szálon szűk a feladó személy szándékozik frissítése felhasználói felület elem által biztosított lehetősége Fejlesztőeszközök.  Példa:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  A **mediaElement_AdaptiveSourceStatusUpdated** módszer végén adja hozzá a következő kódot:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  A **MediaOpened** módszer végén adja hozzá a következő kódot:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  **CTRL + S** billentyűkombináció lenyomásával mentse a fájlt.

**Állíthat össze, és az alkalmazás tesztelése**

1. Nyomja le az **F6 billentyűt** a projekt összeállításához. 
2.  Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.
3.  Az alkalmazás tetején, használja az alapértelmezett zökkenőmentes Streaming URL-címet, vagy adjon meg egy másik. 
4.  Kattintson a **forrás**gombra. 
5.  Tesztelje a csúszkát.

Lecketervező 2 befejezése.  Ez a lecke hozzáadott egy csúszka alkalmazást. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>3 lecke: Jelölje be a folyamatos zökkenőmentes adatfolyamok
Zökkenőmentes adatfolyam képes több nyelv zeneszámok, amelyek szerint a nézők ügyféloldalon választható adatfolyam-tartalom.  Ez a lecke lehetővé teszi a nézők adatfolyamok jelölje ki. Ez a lecke az alábbi eljárások valamelyikét tartalmazza:

1. A XAML-fájl módosítása
2. A kód behand fájl módosítása
3. Állíthat össze, és az alkalmazás tesztelése


**A XAML-fájl módosítása**

1. Megoldás Intézőből kattintson a jobb gombbal a **MainPage.xaml**, és válassza a **Tervező nézet**parancsra.
2. Keresse meg &lt;Grid.RowDefinitions&gt;, és módosíthatja a RowDefinitions, így azok a következőképpen néz:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Belül a &lt;rács&gt;&lt;/Grid&gt; címkék, adja hozzá a következő kódot meghatározása a lista vezérlők, így a felhasználók a rendelkezésre álló adatfolyamok listájának megjelenítéséhez, és válassza ki a adatfolyamok:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Nyomja le a **CTRL + S billentyűkombinációt** a módosítások mentéséhez.


**A kód mögött fájl módosítása**

1. Megoldás Intézőből kattintson a jobb gombbal a **MainPage.xaml**, és kattintson a **Kód megjelenítése**gombra.
2. A SSPlayer névtér belül hozzáadása egy új osztály: #region osztály adatfolyam
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. A MainPage osztály elején adja hozzá a következő változó definíciók:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. A MainPage osztály belül adja hozzá a következő területi:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Keresse meg a mediaElement_ManifestReady módszer, a Hozzáfűzés végén található a függvény a következő kódot:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Amikor készen áll a MediaElement jegyzék, a kód megkapja a rendelkezésre álló adatfolyamok az listáját, és feltölti a felhasználói felület legördülő listában a listában.

6. A MainPage osztály belül keresse meg a felhasználói felület gombok események terület gombra, és adja hozzá az alábbi függvény meghatározása:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Állíthat össze, és az alkalmazás tesztelése**

1. Nyomja le az **F6 billentyűt** a projekt összeállításához. 
2.  Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.
3.  Az alkalmazás tetején, használja az alapértelmezett zökkenőmentes Streaming URL-címet, vagy adjon meg egy másik. 
4.  Kattintson a **forrás**gombra. 
5.  Az alapértelmezett nyelv audio_eng. Próbálja meg audio_eng és audio_es közötti váltáshoz. Űrlap jelenik megnyitásakor, kijelöl egy új adatfolyam, kell, kattintson a Küldés gombra.

Lecketervező 3 befejezése.  Ez a lecke hozzáadása a funkciókat, válassza ki a adatfolyam megjelenítését.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>4 a lecke: Jelölje be a zökkenőmentes adatfolyam-s zeneszámok
Zökkenőmentes Streaming bemutató több videofájlok kódolása a különböző minőségi szinteket (díjak bit) és a megoldások is tartalmazhat. Ez a lecke számok felhasználóknak lehetővé teszi. Ez a lecke az alábbi eljárások valamelyikét tartalmazza:

1. A XAML-fájl módosítása
2. A kód behand fájl módosítása
3. Állíthat össze, és az alkalmazás tesztelése

**A XAML-fájl módosítása**

1. Megoldás Intézőből kattintson a jobb gombbal a **MainPage.xaml**, és válassza a **Tervező nézet**parancsra.
2. Keresse meg a &lt;rács&gt; címkézése, a név **gridStreamAndBitrateSelection**, hozzáfűzése a címke végén a következő kódot:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Nyomja le a **CTRL + S** billentyűkombinációt he-módosítások mentéséhez


**A kód mögött fájl módosítása**

1. Megoldás Intézőből kattintson a jobb gombbal a **MainPage.xaml**, és kattintson a **Kód megjelenítése**gombra.
2. A SSPlayer névtér belül új osztály hozzáadása:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. A MainPage osztály elején adja hozzá a következő változó definíciók:
    
        private List<Track> availableTracks;

4. A MainPage osztály belül adja hozzá a következő területi:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Keresse meg a mediaElement_ManifestReady módszer, a Hozzáfűzés végén található a függvény a következő kódot:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. A MainPage osztály belül keresse meg a felhasználói felület gombok események terület gombra, és adja hozzá az alábbi függvény meghatározása:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Állíthat össze, és az alkalmazás tesztelése**

1. Nyomja le az **F6 billentyűt** a projekt összeállításához. 
2.  Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.
3.  Az alkalmazás tetején, használja az alapértelmezett zökkenőmentes Streaming URL-címet, vagy adjon meg egy másik. 
4.  Kattintson a **forrás**gombra. 
5.  Alapértelmezés szerint minden, a videokép számok van jelölve. Kísérletezés a bit díjváltozást, jelölje be a legalacsonyabb bit ráta, és válassza a legmagasabb átviteli sebességet. Minden módosítása után kell kattintson a Küldés gombra.  Megtekintheti a videók minőségének módosításokat.

Lecketervező 4 befejezése.  Ez a lecke adja hozzá a funkciókat, válassza ki a számokat.


##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Más erőforrások:
- [Hogyan hozhat létre, a zökkenőmentes Streaming Windows 8 JavaScript-alkalmazások, a speciális funkciókat tartalmazó](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Zökkenőmentes adatfolyam technikai áttekintés](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
