<properties 
    pageTitle="Egyetlen átviteli sebesség élő adatfolyam küldése elemi Live kódoló beállítása |} Microsoft Azure" 
    description="Ez a témakör bemutatja, hogyan küldhet egy átviteli sebesség adatfolyam AMS csatornák élő kódolást engedélyezett elemi Live encoder konfigurálása." 
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
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>A elemi Live kódoló segítségével egyetlen átviteli sebesség élő adatfolyam küldése

> [AZURE.SELECTOR]
- [Élő elemi](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Ez a témakör bemutatja, hogyan küldhet egy átviteli sebesség adatfolyam AMS csatornák élő kódolást engedélyezett [Elemi Live](http://www.elementaltechnologies.com/products/elemental-live) encoder konfigurálása.  További tudnivalókért olvassa el a [csatornák, hogy engedélyezve vannak végrehajtása Live kódolást az Azure Media Services használata](media-services-manage-live-encoder-enabled-channels.md)című témakört.

Ebből az oktatóanyagból megtudhatja, hogy miként kezelheti az Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel. Ez az eszköz csak Windows rendszer fut. Ha a Mac vagy Linux rendszerben, az Azure portal segítségével [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és - [alkalmazások](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)létrehozása.

##<a name="prerequisites"></a>Előfeltételek

- Felhasználói felületén elemi Live webes élő eseményeket is létrehozhat egy munka ismeretekkel kell rendelkeznie.
- [Az Azure Media Services-fiók létrehozása](media-services-portal-create-account.md)
- Gondoskodjon arról, hogy van egy kiosztott legalább egy adatfolyam egységgel fut a folyamatos átvitelű végpontot. További tudnivalókért lásd: [A folyamatos átvitelű végpont kezelése a Media Services-fiók](media-services-portal-manage-streaming-endpoints.md).
- A [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz legújabb verziójának telepítéséhez.
- Indítsa el az eszközt, és a AMS csatlakozni.

##<a name="tips"></a>Tippek

- Amikor csak lehetséges, használja a beállíthatóként internetkapcsolat.
- Egy jó a legjobb megoldás esetén sávszélességi követelmények meghatározására, ha duplán a továbbított bitrates. Bár ez nem kötelező követelmény, hogy segítséget nyújt hálózati túlzsúfolt hatásainak.
- Ha kódolók szoftverrel alapján, felesleges programokat, zárja be.

## <a name="elemental-live-with-rtp-ingest"></a>A RTP elemi Live ingest

Ebben a részben beállításáról a elemi Live encoder által küldött egyetlen átviteli sebesség élő adatfolyam RTP fölé.  További tudnivalókért lásd: [MPEG-TS-adatfolyam RTP fölé](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Csatorna létrehozása

1.  A AMSE eszközben nyissa meg azt a **Live** fülre, és a csatorna területén kattintson a jobb gombbal. Jelölje be a **csatorna létrehozása...** a menüből.

![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Adja meg a csatorna nevét, a Leírás mező kitöltése nem kötelező. A csatorna beállításai csoportban jelölje **szabványos** Live kódolást beállítással a bemeneti protokollal **RTP (MPEG-TS)**értékűre. Minden más beállítást, akkor hagyja.


**Most az új csatorna indítása** nincs bejelölve.

3. Kattintson a **csatorna létrehozása**gombra.
![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] A csatorna mindaddig indításához 20 percet is igénybe vehet.

Miközben a csatorna indítása azt is megteheti, [állítsa be a kódoló](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Figyelje meg, hogy a számlázási elindul, amint csatorna kész állapotba kerül. További tudnivalókért olvassa el a [csatorna államok](media-services-manage-live-encoder-enabled-channels.md#states)című témakört.

###<a id=configure_elemental_rtp></a>A elemi Live kódoló konfigurálása 

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


####<a name="configuration-steps"></a>Konfigurálási lépéseket

1. Nyissa meg azt a **Elemi Live** webhelyről, és állítsa be a kódoló **UDP/TS** folyamatos. 

2. Új esemény létrehozása után görgessen le a kimeneti csoportokat, és adja hozzá a **UDP/TS** kimeneti csoportot. 

3. Hozzon létre egy új kimenet: **új adatfolyam** kijelölése, és válassza a **Kimeneti hozzáadása**.  
    
    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Javasolt, hogy a elemi esemény van-e a időkódok segíti a adatfolyam hiba esetén újracsatlakozáshoz kódoló "Rendszeróra" értékűre.

4. Most, hogy a kimenet már elkészült, kattintson a **adatfolyam hozzáadása**gombra. A kimeneti beállítások most beállíthatók. 
5. Görgessen le a "adatfolyam 1" újonnan létrehozott, kattintson a **Videó** fülre a bal oldalon, és bontsa ki a **Speciális** beállítások szakaszt. 

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    A rendelkezésre álló testreszabása széles köre elemi Live rendelkezik, az alábbi beállításokat az első lépések a folyamatos átvitelű AMS az ajánlott. 
    
    - Megoldás: 1280 x 720 
    - Képkockasebességhez: 30 
    - GOP mérete: 60 keretek 
    - Váltott mód: fokozatos 
    - Átviteli sebesség: 5000000 bit/s (Ez lehet beállítani a hálózati korlátozások okozta alapján) 
    

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Ismerkedés a csatorna beviteli URL-CÍMÉT.
    
    Lépjen vissza az AMSE eszközzel, és jelölje be a csatorna készenléti állapotát a. Miután állapota **indítása** **futó**módosult, a bemeneti URL-cím elérheti.
      
    A csatorna fut, kattintson a jobb gombbal a csatorna nevét, nyissa meg a le a hover a **Vágólapra másolása beviteli URL-címe** alatt, és válassza a **Elsődleges beviteli URL-CÍMÉT**.  
    
    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Illessze be ezt az információt a elemi az **Elsődleges** célmezőt. Minden más beállításokat az alapértelmezett marad.
    
    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    A felesleges redundancia ismételje meg a másodlagos beviteli URL-címmel UDP/TS folyamatos "Kimeneti" külön lapon létrehozásával.
    
7. Kattintson a **létrehozása** (ha az új esemény létrehozásának) vagy a **frissítés** (Ha szerkesztési módban a már meglévő esemény), majd folytassa a kódoló indításához. 

>[AZURE.IMPORTANT]Mielőtt elemi Live webes felületén a **Start** gombra kattint, be **kell** győződjön meg arról, hogy készen áll-e a csatornát. 
>Győződjön meg róla, hogy nem a csatorna elhagyása nélkül percnél hosszabb > 15 esemény kész állapotban.

Miután az adatfolyam 30 másodperc nem fut, lépjen vissza az AMSE eszköz és tesztelje a lejátszás.  

###<a name="test-playback"></a>A lejátszás tesztelése
  
1. Nyissa meg azt a AMSE eszközt, és kattintson a jobb gombbal a csatorna vizsgálandó. A menüben mutasson a **Lejátszás az előnézet** , és válassza **az Azure Media Player**.  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Ha a Windows Media player jelenik meg az adatfolyam, majd a kódoló megfelelően van beállítva AMS csatlakozni. 

Ha hiba érkezik, a csatorna alaphelyzetbe kell, és encoder beállításait módosítani. Kérjük, olvassa el a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) útmutatásért.   

###<a name="create-a-program"></a>Hozzon létre egy programot.

1. Miután csatorna lejátszás igazolja, hozzon létre egy programot. Az a AMSE eszközben **Live** lapon a program területén kattintson a jobb gombbal, és jelölje be az **Új alkalmazás létrehozása**.  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

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
