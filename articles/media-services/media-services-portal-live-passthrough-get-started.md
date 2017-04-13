<properties 
    pageTitle="Hogyan végezhetők el az élő streaming az Azure portálon helyszíni kódolók |} Microsoft Azure" 
    description="Ebben az oktatóanyagban végigvezeti az, hogy be van állítva az átadó kézbesítési csatorna létrehozása." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Az Azure portálon helyszíni kódolók adatfolyam live módjáról

> [AZURE.SELECTOR]
- [Portál]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [TÖBBI]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Ebben az oktatóanyagban végigvezeti az, hogy be van állítva az átadó kézbesítési **csatorna** létrehozása az Azure portál használatával. 

##<a name="prerequisites"></a>Előfeltételek

Az alábbiak szükségesek az oktatóprogram elvégzéséhez:

- Az Azure-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 
- A Media Services fiók. Media Services fiók létrehozása, olvassa el a [Media Services-fiók létrehozása](media-services-portal-create-account.md)című témakört.
- A webkamera. Ha például [Telestream Wirecast kódoló](http://www.telestream.net/wirecast/overview.htm).

Azt ajánljuk, tekintse át az alábbi cikkekben:

- [Azure Media Services RTMP támogatja, és Live kódolók](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Élő is az Azure Media Services használatával – áttekintés](media-services-manage-channels-overview.md)
- [A helyszíni kódolók által létrehozott többszintű-átviteli sebesség adatfolyamok streaming Live](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Élő adatfolyam szokták

Az alábbi lépések leírják a feladatok elvégzésében közös élő adatfolyam használó alkalmazások konfigurált csatornák átadó kézbesítésre létrehozása. Ebből az oktatóanyagból megtudhatja, hogy miként átadó csatorna és az élő események létrehozása és kezelése.

1. Videokamera kapcsolódni a számítógéphez. Indítsa el, és állítsa be, amelyek több-átviteli sebesség RTMP vagy tördelt MP4 adatfolyam exportálja a helyszíni élő kódoló. További tudnivalókért lásd: az [Azure Media Services RTMP támogatási és Live kódolók](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    A csatorna létrehozása után is hajtható végre ezt a lépést.

1. Átadó csatorna létrehozása és.
1. A csatorna lekérés ingest URL-CÍMÉT. 

    Az élő kódoló ingest URL-CÍMÉT az adatfolyam küldése a csatorna használt.
1. A csatorna kép URL-Címének beolvasásához. 

    Ha ellenőrizni szeretné, hogy a csatorna megfelelően fogadja az élő adatfolyam az URL-cím használata

3. Hozzon létre egy élő esemény programot. 

    Az Azure portál használata esetén is élő események létrehozásáról létrehoz egy eszköz. 
      
    >[AZURE.NOTE]Győződjön meg arról, hogy legalább egy, a folyamatos átvitelű fenntartott egységre, amelyhez képest adatfolyam tartalom adatfolyam végpont.
1. Indítsa el a esemény/programot, ha készen áll a kezdésre streaming és az archiválás.
2. Másik lehetőségként a élő kódoló is jelezhető egy reklámot indításához. A Közzététel helye kerül a kimeneti adatfolyamban.
1. Ha meg szeretné szüntetni a folyamatos átvitelű, és az archiválás az esemény, állítsa le a esemény program.
1. Törölje az esemény program (, és tetszés szerint törölje az eszköz).     

>[AZURE.IMPORTANT] Véleményezze a következőt [élő streaming helyszíni kódolók, amely a többszörös-átviteli sebesség adatfolyamok létrehozása](media-services-live-streaming-with-onprem-encoders.md) a fogalmak és a helyszíni kódolók és átadó csatornák élő streaming kapcsolatos szempontok ismertetése.

##<a name="to-view-notifications-and-errors"></a>Értesítések és hibák megtekintése

Ha szeretné az értesítések és az Azure portal által gyártott hibák megtekintése, kattintson az értesítési ikon.

![Értesítések](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Adatfolyam-végpontok konfigurálása 

Media Services biztosít dinamikus csomagolása, amely lehetővé teszi a többszörös-átviteli sebesség MP4s előadása a következő adatfolyam formátumokban: MPEG kötőjel, HLS, zökkenőmentes Streaming vagy HDS nélkül az ezek a folyamatos átvitelű formátumok újracsomagolására. Dinamikus csomagolást csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services hoz létre, és a megfelelő választ, egy ügyfél érkező kérések alapján szolgál.

Dinamikus összecsomagolása kihasználhatja kell legalább egy adatfolyam egység beszerzése az adatfolyam végpontot, amelyből tervezi kézbesítési a tartalmakat.  

Szeretne létrehozni, és módosítsa a folyamatos átvitelű fenntartott egységek számát, tegye a következőket:

1. Jelentkezzen be a az [Azure-portálon](https://portal.azure.com/).
1. Kattintson a **Beállítások** ablak **adatfolyam végpontok**. 

2. Kattintson az alapértelmezett streaming végpontot. 

    A **Folyamatos ÁTVITELŰ VÉGPONT ADATAIT alapértelmezett** ablak.

3. Adja meg az adatfolyam egységek számát, húzza a **adatfolyam egységek** csúszkát.

    ![A folyamatos átvitelű erőforrás-mennyiség](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Kattintson a **Mentés** gombra a módosítások mentéséhez.

    >[AZURE.NOTE]A felosztás minden új egységek is legfeljebb 20 percet igénybe vehet.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Átadó csatornák és események létrehozása és

A csatorna társítva, amelyek segítségével szabályozhatja a közzétételi és az élő adatfolyam szegmensek tárolására esemény/programok. Csatorna kezelése eseményeket. 
    
Megadhatja, hogy meg szeretné őrizni a program a rögzített tartalmak **Archívum ablakban** hossza megadásával órák számát. Ezt az értéket kell az megadhatja legalább 5 perce legfeljebb 25 órát. Archiválás ablak hossza is szerint idő ügyfelek legnagyobb mennyiségű aktuális élő pozíciójában vissza az időt is ismert. Események futtatását is lehetővé teszi a megadott időmennyiséget fölé, de a tartalmat, az ablak hossza mögé helyezett folyamatosan elvész. Ez a tulajdonság értéke is határozza meg, hogy mennyi ideig is nagyobb az ügyfél-jegyzékfájlok.

Egyes események társítva tárgyi eszköz. Az esemény közzétenni, létre kell hoznia egy OnDemand megnevezés társított eszköz. A megnevezés problémákat lehetővé teszi, hogy az ügyfelek lehet nyújtani adatfolyam URL-cím összeállítása.

Csatorna támogatja a legfeljebb három párhuzamosan futó eseményeket, így az azonos bejövő adatfolyam több archívumok hozhat létre. Ez lehetővé teszi közzé, és esemény különböző részeire archiválni, szükség szerint. A vállalati követelmény például program 6 órával archiválni, de csak az utolsó 10 perc szétküldése is. Ehhez két párhuzamosan futó programok létrehozásához szükséges. Több programot van beállítva, 6 órával az esemény archiválását, de a program nincs közzétéve. A többi program 10 percig archiválása van állítva, és a program közzétételét.

Meglévő élő eseményeket nem szabad újra. Ehelyett hozzon létre, és indítsa el az egyes események az új esemény.

Indítsa el az eseményt, ha készen áll a folyamatos átvitelű és az archiválás indításához. Ha meg szeretné szüntetni a folyamatos átvitelű, és az archiválás az eseményt, állítsa le a program. 

Archivált tartalmat törléséhez leállítása és törölheti az eseményt, és törölje a társított eszközt. Egy tárgyi eszköz nem törölhető, ha már használja egy esemény; az esemény először is törli. 

Után le, és törölheti az eseményt, a felhasználók tudjanak adatfolyamként az archivált videóként igény szerint, az mindaddig, amíg ki nem törli az eszköz lenne.

Ha meg szeretné megőrizni az archivált tartalmat, de nem érhető el a a folyamatos átvitelű, törölje az adatfolyam megnevezés.

###<a name="to-use-the-portal-to-create-a-channel"></a>Csatorna létrehozása a portál használatával 

Ez a szakasz megtudhatja, hogy miként átadó csatorna létrehozásához adja meg a **Gyors létrehozása** beállítást.

Átadó csatornák kapcsolatos részletekért olvassa el a [helyszíni kódolók, amely a többszörös-átviteli sebesség adatfolyamok létrehozása a folyamatos átvitelű Live](media-services-live-streaming-with-onprem-encoders.md)című témakört.

1. Az [Azure portál](https://portal.azure.com/)jelölje ki az Azure Media Services-fiókját.
2. A **Beállítások** ablakban kattintson **a folyamatos átvitelű Live**. 

    ![Első lépések](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Az **élő streaming** ablak.

3. Kattintson a **Gyors létrehozása** átadó csatorna létrehozásához a RTMP ingest Protocol (protokoll).

    Az **Új csatorna létrehozása** ablak.
4. Adjon meg egy nevet az új csatornát, és kattintson a **Létrehozás**gombra. 

    Ez a RTMP átadó csatorna hoz létre ingest Protocol (protokoll).

##<a name="create-events"></a>Események létrehozása

1. Válasszon egy csatornát, felvehet egy eseményt kívánt.
2. Nyomja meg **Az esemény Live** gombot.

![Esemény](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Get ingest URL-címei

Amikor létrejött a csatornát, elérheti hozzá fog adni, az élő kódoló URL-címek ingest. A kódoló használja az alábbi URL-élő adatfolyam bemeneti.

![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Megtekintés az esemény

Megtekintés az esemény, az Azure-portálon kattintson a **Figyelőpont** vagy adatfolyam URL másolása másokkal, és egy tetszés szerinti lejátszó használhatja. 
 
![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Élő esemény automatikusan első alakul át igény szerinti tartalom leállítása

##<a name="clean-up"></a>Karbantartása:

Átadó csatornák kapcsolatos részletekért olvassa el a [helyszíni kódolók, amely a többszörös-átviteli sebesség adatfolyamok létrehozása a folyamatos átvitelű Live](media-services-live-streaming-with-onprem-encoders.md)című témakört.

- A csatorna leállítható, csak akkor, ha minden események/program a csatornán leállt.  Ha leállítja a csatornát, nem merülnek bármilyen díjakat. Ha ismét elindítani kell lesz azonos ingest URL-CÍMÉT, így Önnek nem kell a kódoló átkonfigurálása.
- A csatorna csak akkor, amikor a csatorna élő eseményeiről törölt lehet törölni.

##<a name="view-archived-content"></a>Archivált tartalmának megtekintése

Után le, és törölheti az eseményt, a felhasználók tudjanak adatfolyamként az archivált videóként igény szerint, az mindaddig, amíg ki nem törli az eszköz lenne. Egy tárgyi eszköz nem törölhető, ha már használja egy esemény; az esemény először is törli. 

A eszközeinek kezelésében, jelölje ki **a beállítást** , és kattintson az **eszközök**gombra.

![Eszközök](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Következő lépés

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
