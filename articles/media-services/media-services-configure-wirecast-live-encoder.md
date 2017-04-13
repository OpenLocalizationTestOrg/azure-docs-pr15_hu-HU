<properties 
    pageTitle="A Telestream Wirecast kódoló küldhet egy átviteli sebesség élő adatfolyam beállítása |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan küldhet egy átviteli sebesség adatfolyam AMS csatornák élő kódolást engedélyezett a Wirecast élő kódoló konfigurálása. " 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>A Wirecast kódoló segítségével egyetlen átviteli sebesség élő adatfolyam küldése

> [AZURE.SELECTOR]
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [Élő elemi](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Ez a témakör bemutatja, hogyan küldhet egy átviteli sebesség adatfolyam AMS csatornák élő kódolást engedélyezett a [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) élő kódoló konfigurálása.  További tudnivalókért olvassa el a [csatornák, hogy engedélyezve vannak végrehajtása Live kódolást az Azure Media Services használata](media-services-manage-live-encoder-enabled-channels.md)című témakört.

Ebből az oktatóanyagból megtudhatja, hogy miként kezelheti az Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel. Ez az eszköz csak Windows rendszer fut. Ha a Mac vagy Linux rendszerben, az Azure portal segítségével [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és - [alkalmazások](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)létrehozása.


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

![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Adja meg a csatorna nevét, a Leírás mező kitöltése nem kötelező. A csatorna beállításai csoportban jelölje **szabványos** Live kódolást beállítással a bemeneti protokollal **RTMP**beállítása. Minden más beállítást, akkor hagyja.


**Most az új csatorna indítása** nincs bejelölve.

3. Kattintson a **csatorna létrehozása**gombra.
![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

>[AZURE.NOTE] A csatorna mindaddig indításához 20 percet is igénybe vehet.

Miközben a csatorna indítása azt is megteheti, [állítsa be a kódoló](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

>[AZURE.IMPORTANT] Figyelje meg, hogy a számlázási elindul, amint csatorna kész állapotba kerül. További tudnivalókért olvassa el a [csatorna államok](media-services-manage-live-encoder-enabled-channels.md#states)című témakört.

##<a id=configure_wirecast_rtmp></a>A Telestream Wirecast kódoló konfigurálása

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

1. Nyissa meg a Telestream Wirecast alkalmazás azon a számítógépen, használt, és állítsa be a folyamatos átvitelű RTMP.
2. Állítsa be a kimenet, lépjen a **Kimenet** lapon válassza a **Kimeneti beállítások...**.
    
    Győződjön meg arról, hogy a **Cél kimeneti** beállítása **RTMP kiszolgáló**.
3. Kattintson az **OK gombra**.
4. A beállítások lapon állítsa **a célmező kell az **Azure Media Services**** .
 
    A kódolás profil előre kiválasztott az **Azure H.264 720 p 16:9 (1280 x 720)**. Ezekkel a beállításokkal testreszabásához válassza a fogaskerék ikonra a jobb oldali legördülő listában, és válassza az **Új előre definiált**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)

5. Állítsa be a kódoló készleteket.

    Nevezze el a készletet, és ellenőrizze a következőket ajánlott beállítások:

    **A videó**
    
    - Kódoló: MainConcept: H.264
    - Keretek másodpercenként: 30
    - Átlagos átviteli sebessége: 5000 kbit/sec (módosítható a hálózati korlátozások okozta alapján)
    - Profil: fő
    - Fő keret minden: 60 vázak

    **Hang**

    - Cél: 192 kbit/sec
    - Minta ráta: 44.100 kHz
     
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)

6. Nyomja le az ENTER **menteni**.

    A kódolás mező ekkor megjelenik az újonnan létrehozott profil kijelölt használható. 

    Győződjön meg arról, hogy az új profil ki van jelölve.

7. A csatorna bemeneti URL-cím kaphat az Wirecast **RTMP végpont**hozzárendelheti érdekében.
    
    Lépjen vissza az AMSE eszközzel, és jelölje be a csatorna készenléti állapotát a. Miután állapota **indítása** **futó**módosult, a bemeneti URL-cím elérheti.
      
    A csatorna fut, kattintson a jobb gombbal a csatorna nevét, nyissa meg a le a hover a **Vágólapra másolása beviteli URL-címe** alatt, és válassza a **Elsődleges beviteli URL-CÍMÉT**.  
    
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)

8. Wirecast **Kimeneti beállítások** ablakban illessze be ezt az információt a Kimenet csoportban, a **cím** mezőbe, és hozzárendelése adatfolyam nevét. 


    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

9. Kattintson **az OK gombra**.

10. **Wirecast** főképernyőjén erősítse meg a bemeneti forrás video- és audiofájl készen áll, és nyomja le az **adatfolyam** baloldali felső sarokban.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

>[AZURE.IMPORTANT]**Adatfolyam**gombra kattint, mielőtt be **kell** győződjön meg arról, hogy készen áll-e a csatornához. 
>Győződjön meg róla, hogy nem hagyja a csatorna kész állapotban-hírcsatorna percnél hosszabb > 15 beviteli hozzájárulása nélkül.

##<a name="test-playback"></a>A lejátszás tesztelése
  
1. Nyissa meg azt a AMSE eszközt, és kattintson a jobb gombbal a csatorna vizsgálandó. A menüben mutasson a **Lejátszás az előnézet** , és válassza **az Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Ha a Windows Media player jelenik meg az adatfolyam, majd a kódoló megfelelően van beállítva AMS csatlakozni. 

Ha hiba érkezik, a csatorna alaphelyzetbe kell, és encoder beállításait módosítani. Kérjük, olvassa el a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) útmutatásért.  

##<a name="create-a-program"></a>Hozzon létre egy programot.

1. Miután csatorna lejátszás igazolja, hozzon létre egy programot. Az a AMSE eszközben **Live** lapon a program területén kattintson a jobb gombbal, és jelölje be az **Új alkalmazás létrehozása**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)

2. Nevezze el a program, és ha szükséges, módosítsa a **Archív ablak hossza** (Ez alapértelmezés szerint a 4 óra). Tárolási helyét, vagy hagyja az alapértelmezett érték is.  
3. Jelölje be **a Program ekkor indításához** .
4. Kattintson a **Program létrehozása**gombra.  
  
    Megjegyzés: A Program létrehozása az csatorna létrehozása-nél kevesebb ideig tart.    
 
5. Ha a program fut, erősítse meg a lejátszás jobb gombbal a program és Navigálás a **Lejátszás a programokat** , és kiválasztja az **Azure Media Player**.  
6. Megerősítést, miután kattintson a jobb gombbal a program újra, és válassza **a kimenő URL-CÍMÉT a vágólapra másolása** (vagy a adatok kinyerése a **Program adatait, és a beállítások** lehetőséget a menüből). 

Az adatfolyamban készen áll a beágyazott lejátszóval vagy a közönségnek élő megtekintésre meghatározva.  


## <a name="troubleshooting"></a>Hibaelhárítás
 
Kérjük, olvassa el a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) útmutatásért. 

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
