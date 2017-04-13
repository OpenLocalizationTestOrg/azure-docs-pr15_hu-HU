<properties 
    pageTitle="A NewTek TriCaster kódoló küldhet egy átviteli sebesség élő adatfolyam beállítása |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan küldhet egy átviteli sebesség adatfolyam AMS csatornák élő kódolást engedélyezett a Tricaster élő kódoló konfigurálása." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="juliako;cenkd;anilmur"/>

#<a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>A NewTek TriCaster kódoló segítségével egyetlen átviteli sebesség élő adatfolyam küldése

> [AZURE.SELECTOR]
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Élő elemi](media-services-configure-elemental-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Ez a témakör bemutatja, hogyan küldhet egy átviteli sebesség adatfolyam AMS csatornák élő kódolást engedélyezett a [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) élő kódoló konfigurálása. További tudnivalókért olvassa el a [csatornák, hogy engedélyezve vannak végrehajtása Live kódolást az Azure Media Services használata](media-services-manage-live-encoder-enabled-channels.md)című témakört.

Ebből az oktatóanyagból megtudhatja, hogy miként kezelheti az Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel. Ez az eszköz csak Windows rendszer fut. Ha a Mac vagy Linux rendszerben, az Azure portal segítségével [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és - [alkalmazások](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)létrehozása.

>[AZURE.NOTE]Az élő kódolást engedélyezett AMS csatornák adatcsatorna hozzájárulása küldésének Tricaster használatakor lehet videó és hang problémák az élő esemény Tricaster, például a gyors közötti hírcsatornák kivágása és -ről történő/palák az egyes szolgáltatások használatakor. A AMS csapatának dolgozik a problémák orvoslására: ezeket a problémákat, egészen addig, akkor van nem javasoljuk szeretne ezekkel a funkciókkal.


##<a name="prerequisites"></a>Előfeltételek

- [Az Azure Media Services-fiók létrehozása](media-services-portal-create-account.md)
- Gondoskodjon arról, hogy van egy kiosztott legalább egy adatfolyam egységgel fut a folyamatos átvitelű végpontot. További tudnivalókért lásd: [A folyamatos átvitelű végpont kezelése a Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)
- A [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz legújabb verziójának telepítéséhez.
- Indítsa el az eszközt, és a AMS csatlakozni.

##<a name="tips"></a>Tippek

- Amikor csak lehetséges, használja a beállíthatóként internetkapcsolat.
- Egy jó a legjobb megoldás esetén sávszélességi követelmények meghatározására, ha duplán a továbbított bitrates. Bár ez nem kötelező követelmény, hogy segítséget nyújt hálózati túlzsúfolt hatásainak.
- Ha kódolók szoftverrel alapján, felesleges programokat, zárja be.

## <a name="create-a-channel"></a>Csatorna létrehozása

1.  A AMSE eszközben nyissa meg azt a **Live** fülre, és a csatorna területén kattintson a jobb gombbal. Jelölje be a **csatorna létrehozása...** a menüből.

![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Adja meg a csatorna nevét, a Leírás mező kitöltése nem kötelező. A csatorna beállításai csoportban jelölje **szabványos** Live kódolást beállítással a bemeneti protokollal **RTMP**beállítása. Minden más beállítást, akkor hagyja.


**Most az új csatorna indítása** nincs bejelölve.

3. Kattintson a **csatorna létrehozása**gombra.
![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

>[AZURE.NOTE] A csatorna mindaddig indításához 20 percet is igénybe vehet.


Miközben a csatorna indítása azt is megteheti, [állítsa be a kódoló](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

>[AZURE.IMPORTANT] Figyelje meg, hogy a számlázási elindul, amint csatorna kész állapotba kerül. További tudnivalókért olvassa el a [csatorna államok](media-services-manage-live-encoder-enabled-channels.md#states)című témakört.

##<a id=configure_tricaster_rtmp></a>A NewTek TriCaster kódoló konfigurálása

Ebben az oktatóprogramban az alábbi kimeneti beállítások szolgálnak. Ebben a szakaszban a többi konfigurálási lépéseket részletesebben ismerteti. 

**Videó**:
 
- A kodek: H.264 
- Profil: Nagy (szint 4.0) 
- Átviteli sebesség: 5000 KB 
- Képkockára helyezheti: 2 másodpercig (60 másodperc) 
- Ráta keret: 30
 
**Hang**:

- A kodek: AAC (LC) 
- Átviteli sebesség: 192 KB 
- Minta ráta: 44,1 kHz


###<a name="configuration-steps"></a>Konfigurálási lépéseket

1. Attól függően, hogy milyen videó beviteli forrás van használatban **NewTek TriCaster** új projekt létrehozása. 
2. Egyszer, hogy a Project programban keresse meg az **adatfolyam** gombra, és kattintson a fogaskerék ikon látható a adatfolyam konfigurációs menü eléréséhez.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Miután megnyílt a a menüt, kattintson az **Új** kapcsolat címszó alatt. Amikor a rendszer kéri a kapcsolat típusának, jelölje be az **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)

4. Kattintson az **OK gombra**.

5. Egy FMLE profil most importálhatók a legördülő lista nyilára kattintva **a folyamatos átvitelű profil** csoportban lévő hivatkozásra, majd a **Tallózás**a Navigálás.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)

6. Nyissa meg a beállított FMLE profil lapja.
7. Jelölje ki azt, és kattintson az **OK gombra**.

    Miután a profil töltenek fel, folytassa a következő lépésben.

6. A csatorna bemeneti URL-cím kaphat az Tricaster **RTMP végpont**hozzárendelheti érdekében.
    
    Lépjen vissza az AMSE eszközzel, és jelölje be a csatorna készenléti állapotát a. Miután állapota **indítása** **futó**módosult, a bemeneti URL-cím elérheti.
      
    A csatorna fut, kattintson a jobb gombbal a csatorna nevét, nyissa meg a le a hover a **Vágólapra másolása beviteli URL-címe** alatt, és válassza a **Elsődleges beviteli URL-CÍMÉT**.  
    
    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)

7. Illessze be ezt az információt az Tricaster projektben **Flash-kiszolgáló** alatt **a hely** mezőben. Az **Adatfolyam-azonosító** mező adatfolyam nevét is rendelhet. 

    Adatfolyam-információk hozzá lett adva a FMLE profilhoz, ha azt is importálhatók szakaszban **Az importálási beállítások**parancsra, nyissa meg a mentett FMLE profilt, és kattintson az **OK gombra**. A Flash kiszolgáló mezőket fel kell töltenie FMLE adataival.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)

9. Amikor befejezte, kattintson **az OK gombra** a képernyő alján. Amikor készen áll a Tricaster hang- és videofájlok ráfordítások, kezdje el a folyamatos átvitelű való AMS **adatfolyam** gombra kattintva.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

>[AZURE.IMPORTANT]**Adatfolyam**gombra kattint, mielőtt be **kell** győződjön meg arról, hogy készen áll-e a csatornához. 
>Győződjön meg róla, hogy nem hagyja a csatorna kész állapotban-hírcsatorna percnél hosszabb > 15 beviteli hozzájárulása nélkül. 

##<a name="test-playback"></a>A lejátszás tesztelése
  
1. Nyissa meg azt a AMSE eszközt, és kattintson a jobb gombbal a csatorna vizsgálandó. A menüben mutasson a **Lejátszás az előnézet** , és válassza **az Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Ha a Windows Media player jelenik meg az adatfolyam, majd a kódoló megfelelően van beállítva AMS csatlakozni. 

Ha hiba érkezik, a csatorna alaphelyzetbe kell, és encoder beállításait módosítani. Kérjük, olvassa el a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) útmutatásért.  

##<a name="create-a-program"></a>Hozzon létre egy programot.

1. Miután csatorna lejátszás igazolja, hozzon létre egy programot. Az a AMSE eszközben **Live** lapon a program területén kattintson a jobb gombbal, és jelölje be az **Új alkalmazás létrehozása**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)

2. Nevezze el a program, és ha szükséges, módosítsa a **Archív ablak hossza** (Ez alapértelmezés szerint a 4 óra). Tárolási helyét, vagy hagyja az alapértelmezett érték is.  
3. Jelölje be **a Program ekkor indításához** .
4. Kattintson a **Program létrehozása**gombra.  
  
    Megjegyzés: A Program létrehozása az csatorna létrehozása-nél kevesebb ideig tart.    
 
5. Ha a program fut, erősítse meg a lejátszás jobb gombbal a program és Navigálás a **Lejátszás a programokat** , és kiválasztja az **Azure Media Player**.  
6. Megerősítést, miután kattintson a jobb gombbal a program újra, és válassza **a kimenő URL-CÍMÉT a vágólapra másolása** (vagy a adatok kinyerése a **Program adatait, és a beállítások** lehetőséget a menüből). 

Az adatfolyamban készen áll a beágyazott lejátszóval vagy a közönségnek élő megtekintésre meghatározva.  


## <a name="troubleshooting"></a>Hibaelhárítás

Kérjük, olvassa el a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) útmutatásért. 


##<a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
