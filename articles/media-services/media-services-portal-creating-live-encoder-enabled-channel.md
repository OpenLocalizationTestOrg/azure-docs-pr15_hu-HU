<properties 
    pageTitle="Hogyan végezhetők el az Azure Media Services használatával hogyan hozhat létre több-átviteli sebesség adatfolyamok az Azure portál élő folyamatos átvitelű |} Microsoft Azure" 
    description="Ebben az oktatóanyagban végigvezeti a csatorna, amely egyetlen-átviteli sebesség élő adatfolyam fogadása és kódolja a szeretne több-átviteli sebesség adatfolyam az Azure portálon létrehozása." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Hogyan végezhetők el az Azure Media Services használatával hogyan hozhat létre több-átviteli sebesség adatfolyamok az Azure portál élő folyamatos átvitelű

> [AZURE.SELECTOR]
- [Portál](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API-VAL](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Ebben az oktatóanyagban végigvezeti a **csatorna** , amely egyetlen-átviteli sebesség élő adatfolyam fogadása és kódolja a szeretne több-átviteli sebesség adatfolyam létrehozása.

>[AZURE.NOTE]Élő kódolást engedélyezett csatornák kapcsolatos további tájékoztatást talál [Live streaming az Azure Media Services hozhat létre több-átviteli sebesség adatfolyamok használata](media-services-manage-live-encoder-enabled-channels.md)

##<a name="common-live-streaming-scenario"></a>Élő adatfolyam szokták

Gyakori élő adatfolyam alkalmazások létrehozása általános lépései a következők.

>[AZURE.NOTE] Élő esemény max ajánlott hosszának jelenleg 8 órát. Ha hosszabb ideig csatorna futtatásához van szüksége, forduljon a amslived a Microsoft.com webhelyről.

1. Videokamera kapcsolódni a számítógéphez. Indítsa el, és állítsa be a helyszíni élő kódoló, amely az egyik a következő protokollok egyetlen átviteli sebesség adatfolyam is kimeneti: RTMP, zökkenőmentes Streaming vagy RTP (MPEG-TS). További tudnivalókért lásd: az [Azure Media Services RTMP támogatási és Live kódolók](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    A csatorna létrehozása után is hajtható végre ezt a lépést.

1. Csatorna létrehozása és. 

1. A csatorna lekérés ingest URL-CÍMÉT. 

    Az élő kódoló ingest URL-CÍMÉT az adatfolyam küldése a csatorna használt.
1. A csatorna kép URL-Címének beolvasásához. 

    Ha ellenőrizni szeretné, hogy a csatorna megfelelően fogadja az élő adatfolyam az URL-cím használata

3. Hozzon létre egy esemény/programot (is hoz létre a tárgyi eszköz). 
1. Közzététel az esemény (azaz létrehoz egy OnDemand megnevezés társított eszköz).  

    Győződjön meg arról, hogy legalább egy, a folyamatos átvitelű fenntartott egységre, amelyhez képest adatfolyam tartalom adatfolyam végpont.
1. Indítsa el az eseményt, ha készen áll a folyamatos átvitelű és az archiválás indításához.
2. Másik lehetőségként a élő kódoló is jelezhető egy reklámot indításához. A Közzététel helye kerül a kimeneti adatfolyamban.
1. Állítsa le a eseményt, ha meg szeretné szüntetni a folyamatos átvitelű, és az archiválás az eseményt.
1. Törölheti az eseményt (és tetszés szerint törölje az eszköz).   

##<a name="in-this-tutorial"></a>Ebben az oktatóanyagban

Ebben az oktatóprogramban az Azure portal segítségével az alábbi feladatok elvégzéséhez: 

2.  Állítsa be a továbbított végpontok.
3.  Hozzon létre egy csatorna, amely élő kódolási végrehajtásához engedélyezve van.
1.  Annak érdekében, hogy adja meg, hogy live encoder beszerzése a Ingest URL-CÍMÉT. Az élő encoder az URL-cím használatával az adatfolyam ingest be a csatorna. .
1.  Hozzon létre egy esemény program (és tárgyi eszköz)
1.  Az eszköz közzétegye, és a folyamatos átvitelű URL-címei  
1.  A tartalom lejátszása 
2.  Törlése

##<a name="prerequisites"></a>Előfeltételek

Az alábbiak szükségesek az oktatóprogram elvégzéséhez.

- Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- A Media Services fiók. Media Services fiók létrehozása, olvassa el a [Fiók létrehozása](media-services-portal-create-account.md)című témakört.
- Webkamera és -kódoló, amelyek egyetlen átviteli sebesség élő adatfolyam küldhetnek.

##<a name="configure-streaming-endpoints"></a>Adatfolyam-végpontok konfigurálása 

Media Services biztosít dinamikus csomagolása, amely lehetővé teszi a többszörös-átviteli sebesség MP4s előadása a következő adatfolyam formátumokban: MPEG kötőjel, HLS, zökkenőmentes Streaming vagy HDS nélkül újra be ezeket a folyamatos átvitelű formátumok csomag. Dinamikus csomagolást csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services fog készítése és a megfelelő választ, egy ügyfél érkező kérések alapján szolgálnak.

Dinamikus összecsomagolása kihasználhatja kell legalább egy adatfolyam egység beszerzése az adatfolyam végpontot, amelyből tervezi kézbesítési a tartalmakat.  

Szeretne létrehozni, és módosítsa a folyamatos átvitelű fenntartott egységek számát, tegye a következőket:

1. Jelentkezzen be az [Azure-portálra](https://portal.azure.com/) , és jelölje ki a AMS fiókját.
1. Kattintson a **Beállítások** ablak **adatfolyam végpontok**. 

2. Kattintson az alapértelmezett streaming végpontot. 

    A **Folyamatos ÁTVITELŰ VÉGPONT ADATAIT alapértelmezett** ablak.

3. Adja meg az adatfolyam egységek számát, húzza a **adatfolyam egységek** csúszkát.

    ![A folyamatos átvitelű erőforrás-mennyiség](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Kattintson a **Mentés** gombra a módosítások mentéséhez.

    >[AZURE.NOTE]A felosztás minden új egységek is legfeljebb 20 percet igénybe vehet.

##<a name="create-a-channel"></a>CSATORNA létrehozása

1. Az [Azure portálon](https://portal.azure.com/)válassza a Media Services, és kattintson a Media Services fióknév gombjára.
2. Jelölje ki az **élő adatfolyam**.
3. Jelölje ki az **egyéni létrehozása**. Ez a beállítás teszi lehetővé, hogy engedélyezve van az élő kódolást csatorna létrehozása.

    ![Csatorna létrehozása](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Kattintson a **Beállítások**elemre.
    
    1.  Válassza ki a **Kódolás Live** csatornát. Ez azt határozza meg, hogy szeretne-e létrehozni, hogy engedélyezve van az élő kódolást csatorna. Ez azt jelenti, hogy a bejövő levelek egyetlen átviteli sebesség adatfolyam küldött a csatorna és az élő encoder megadott beállításokkal többszörös-átviteli sebesség adatfolyam kódolva. További tudnivalókért olvassa el a [Live streaming az Azure Media Services hozhat létre több-átviteli sebesség adatfolyamok használata](media-services-manage-live-encoder-enabled-channels.md)című témakört. Kattintson az OK gombra.
    2. Adja meg a csatorna nevét.
    3. A képernyő alján az OK gombra.
    
5. Jelölje ki a **Ingest** fülre.

    1. Ezen a lapon jelölje be a továbbított Protocol (protokoll). Csatorna **Live kódolást** típusa érvényes protokoll beállítások a következők:
        
        - Egy átviteli sebesség Fragmented MP4 (sima Streaming)
        - Egy átviteli sebesség RTMP
        - RTP (MPEG-TS): MPEG-2 átviteli adatfolyam RTP fölé.
        
        Részletes ismertetése a minden egyes protokoll [használatával az Azure Media Services hozhat létre több-átviteli sebesség adatfolyamok streaming Live](media-services-manage-live-encoder-enabled-channels.md)talál.
    
        A Protocol (protokoll) lehetőséget a csatorna közben nem módosítható, vagy a kapcsolódó eseményeket programjait futtatja. A különböző protokollok van szüksége, ha minden adatfolyam protokollhoz külön csatornák kell létrehozni.  

    2. IP-korlátozás a ingest is alkalmazhat. 
    
        A csatorna videó ingest engedélyezett IP-címtartományokat adhat meg. Engedélyezett IP-címek lehet megadni egy egyetlen IP-címet (például 10.0.0.1), az IP-tartomány IP-címet és egy CIDR alhálózat maszk (pl. 10.0.0.1/22) vagy az IP-címet és egy pontozott decimális alhálózat maszk IP-tartomány (például "10.0.0.1(255.255.252.0)').

        Ha nincs IP-címek van megadva, és nincs szabály meghatározása nincs IP-címe lesz jogosult. Ha engedélyezni szeretné a bármely IP-cím, hozzon létre egy szabályt, és 0.0.0.0/0 beállítása.

6. A **kép** lapon az előnézet IP korlátozását vonatkoznak.
7. A **kódolás** lap adja meg a kódolási készletet. 

    Jelenleg csak a rendszerben beállított választhatja ki az **alapértelmezett 720 p**. Egy egyéni készletet, nyissa meg a Microsoft támogatási jegyek. Ezután adja meg a készlet jött létre nevét. 

>[AZURE.NOTE] Csatorna indítása jelenleg, akár 30 percig is eltarthat. Csatorna alaphelyzetbe állítása legfeljebb 5 percig is eltarthat.

Miután létrehozta a csatornát, kattintson a csatornát, és válassza a **Beállítások** , ahol megtekintheti a csatornák konfigurációk. 

További tudnivalókért olvassa el a [Live streaming az Azure Media Services hozhat létre több-átviteli sebesség adatfolyamok használata](media-services-manage-live-encoder-enabled-channels.md)című témakört.


##<a name="get-ingest-urls"></a>Get ingest URL-címei

Amikor létrejött a csatornát, elérheti hozzá fog adni, az élő kódoló URL-címek ingest. A kódoló használja az alábbi URL-élő adatfolyam bemeneti.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Létrehozhatja és kezelheti a események

###<a name="overview"></a>– Áttekintés

A csatorna társítva, amelyek segítségével szabályozhatja a közzétételi és az élő adatfolyam szegmensek tárolására esemény/programok. Csatorna kezelése esemény/programok. A csatorna és a Program a kapcsolat nagyon hasonlít hagyományos media, ahol csatorna állandó adatfolyam-tartalom van, és a program bizonyos csatorna a megadott eseményre megfelelően változik.

Megadhatja, hogy meg szeretné őrizni a rögzített tartalmak az esemény **Archívum ablakban** hossza megadásával órák számát. Ezt az értéket kell az megadhatja legalább 5 perce legfeljebb 25 órát. Archiválás ablak hossza is szerint idő ügyfelek legnagyobb mennyiségű aktuális élő pozíciójában vissza az időt is ismert. Események futtatását is lehetővé teszi a megadott időmennyiséget fölé, de a tartalmat, az ablak hossza mögé helyezett folyamatosan elvész. Ez a tulajdonság értéke is határozza meg, hogy mennyi ideig is nagyobb az ügyfél-jegyzékfájlok.

Egyes események társítva tárgyi eszköz. Az esemény közzététele létre kell hoznia egy OnDemand megnevezés társított eszköz. A megnevezés problémákat lehetővé teszi az ügyfelek lehet nyújtani adatfolyam URL-cím összeállítása.

Csatorna támogatja a legfeljebb három párhuzamosan futó eseményeket, így az azonos bejövő adatfolyam több archívumok hozhat létre. Ez lehetővé teszi közzé, és esemény különböző részeire archiválni, szükség szerint. A vállalati követelmény például egy esemény 6 órával archiválását, de csak az utolsó 10 perc szétküldése is. Ehhez két párhuzamosan futó létrehozásához szükséges esemény. Egy esemény van beállítva, 6 órával az esemény archiválása, de a program nincs közzétéve. Az események 10 percig archiválása van állítva, és a program közzétételét.

Új eseményre vonatkozóan meglévő programok nem szabad újra. Ehelyett létrehozása és az egyes események új programot.

Ha készen áll a kezdésre streaming és az archiválás esemény/alkalmazás indítása. Állítsa le a eseményt, ha meg szeretné szüntetni a folyamatos átvitelű, és az archiválás az eseményt. 

Archivált tartalmat törléséhez leállítása és törölheti az eseményt, és törölje a társított eszközt. Egy tárgyi eszköz nem törölhető, ha már használja az esemény; az esemény először is törli. 

Után le, és törölheti az eseményt, a felhasználók tudjanak adatfolyamként az archivált videóként igény szerint, az mindaddig, amíg ki nem törli az eszköz lenne.

Ha meg szeretné megőrizni az archivált tartalmat, de nem érhető el a a folyamatos átvitelű, törölje az adatfolyam megnevezés.

###<a name="createstartstop-events"></a>Események létrehozása és indítása és leállítása

Ha befejezte az adatfolyam szét a csatorna: hozzon létre egy eszköz, a Program és a folyamatos átvitelű megnevezés megkezdheti az adatfolyam esemény. Ez az adatfolyam archiválni, és a folyamatos átvitelű végpont keresztül a nézők számára elérhető legyen. 

Kétféleképpen esemény indítása: 

1. A **csatorna** lapról nyomja meg az **Esemény Live** új esemény hozzáadása.

    Adja meg: esemény neve, eszköz neve, archívum ablakban, és titkosítási beállítás.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Ha hagyta **most az élő esemény közzététel** ellenőrzi, első létrejön az esemény KÖZZÉTÉTELI URL-címét.
    
    Lenyomja a **Kezdés**, amikor készen áll a adatfolyam-eseményt.

    Indítja el az eseményt, ha lenyomja a **Megtekintés** lejátszásához a tartalmat.

2. Azt is megteheti billentyűparanccsal, és nyomja le az **Élő** gomb a **csatorna** lapon. Egy tárgyi eszköz, alapértelmezett alkalmazás és a folyamatos átvitelű megnevezés ez hoz létre.

    Az esemény **alapértelmezett** neve, és az archiválás ablak 8 órát van beállítva.

Nézze meg a közzétett esemény **Live esemény** lapra. 

Ha **Air kikapcsolása**gombra kattint, az összes élő esemény leáll. 


##<a name="watch-the-event"></a>Megtekintés az esemény

Megtekintés az esemény, az Azure-portálon kattintson a **Figyelőpont** vagy adatfolyam URL másolása másokkal, és egy tetszés szerinti lejátszó használhatja. 
 
![Létrehozva](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Élő esemény automatikusan átalakítja események igény szerinti tartalmat, amikor leállt.

##<a name="clean-up"></a>Karbantartása:

Ha végzett adatfolyam események és karbantartása: az erőforrások korábbi kiépítéstől szeretné, hajtsa végre az alábbi eljárást.

- Állítsa le a adatfolyam terjesztése a kódoló a.
- Állítsa le a csatornához. Amikor leállítja a csatornát, azt nem merülnek fel minden olyan díjak. Ha ismét elindítani kell lesz azonos ingest URL-CÍMÉT, így Önnek nem kell a kódoló átkonfigurálása.
- A folyamatos átvitelű végpont leállíthatja, kivéve, ha továbbra is a archívumba az élő esemény az igény szerinti adatfolyamként megadását. Ha a csatorna leállítva állapotban van, azt nem merülnek fel minden díjat.
  
##<a name="view-archived-content"></a>Archivált tartalmának megtekintése

Után le, és törölheti az eseményt, a felhasználók tudjanak adatfolyamként az archivált videóként igény szerint, az mindaddig, amíg ki nem törli az eszköz lenne. Egy tárgyi eszköz nem törölhető, ha már használja egy esemény; az esemény először is törli. 

A eszközeinek kezelésében, jelölje ki **a beállítást** , és kattintson az **eszközök**gombra.

![Eszközök](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Megfontolandó szempontok

- Élő esemény max ajánlott hosszának jelenleg 8 órát. Ha hosszabb ideig csatorna futtatásához van szüksége, forduljon a amslived a Microsoft.com webhelyről.
- Győződjön meg arról, hogy legalább egy, a folyamatos átvitelű fenntartott egységre, amelyhez képest adatfolyam tartalom adatfolyam végpont.


##<a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
