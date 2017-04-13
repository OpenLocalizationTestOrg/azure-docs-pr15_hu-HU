<properties
    pageTitle=" Első lépések az Azure portálon igény tartalom továbbítása |} Microsoft Azure"
    description="Ebben az oktatóanyagban végigvezeti az Azure Media Services (AMS) alkalmazással az Azure portálon egy egyszerű igény szerinti videó (VoD) tartalma kézbesítési szolgáltatás végrehajtásának."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Első lépések az Azure portálon igény tartalom továbbítása

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Ebben az oktatóanyagban végigvezeti az Azure Media Services (AMS) alkalmazással az Azure portálon egy egyszerű igény szerinti videó (VoD) tartalma kézbesítési szolgáltatás végrehajtásának.

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 

Ebben az oktatóanyagban megtalálhatók az alábbi műveleteket:

1.  Azure Media Services fiókot létrehozni.
2.  Állítsa be a továbbított végpontot.
1.  Töltse fel a videofájlokat.
1.  A forrásfájl kódolását be adaptív átviteli sebesség MP4 fájlokat.
1.  Az eszköz és a folyamatos átvitelű get URL- és fokozatos letöltési közzététele.  
1.  A tartalom lejátszása.


## <a name="create-an-azure-media-services-account"></a>Az Azure Media Services-fiók létrehozása

Ez a szakasz lépései bemutatják, hogyan hozhat létre AMS fiókot.

1. Jelentkezzen be a az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+ Új** > **webes + Mobile** > **Media Services**.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. A **MEDIA SERVICES-fiók létrehozása** adja meg a szükséges értékeket.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. A **Fiók neve**mezőbe írja be az új AMS fiók nevére. A Media Services fióknév összes kisbetűs számokat vagy betűket, szóközök nélkül, és 3-24 karakter hosszú.
    2. Az előfizetését a különböző Azure-előfizetések listáját van hozzáférése közül választhat.
    
    2. Válassza az új vagy meglévő erőforrás **Erőforráscsoport**.  Erőforráscsoport információforrások, amelyek életciklus, engedélyeket és házirendeket megosztása gyűjteménye. További tudnivalók [az alábbi](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Helyét**jelölje ki a földrajzi területhez tartozik használják a média és metaadatok rekordokat a Media Services fiók tárolásához. Ez a terület folyamat, és a multimédia-adatfolyam szolgál. A rendelkezésre álló Media Services régiók jelennek meg a legördülő listából. 
    
    3. **Tárterület-fiókot**válassza ki a tárhely adja meg a Media Services fiókjából médiatartalom blob-tárolóhoz. Kijelölhet egy meglévő tárterület-fiókot a Media Services fiókként ugyanabban a földrajzi régióban, vagy létrehozhat egy tárterület-fiókkal. Új tároló fiók ugyanabban a régióban jön létre. Tárterület fióknevét szabályairól megegyeznek a Media Services fiókok.

        További tudnivalók a tárhely [Itt](storage-introduction.md).

    4. Válassza a **Irányítópult PIN-kódot** a fiók telepítési állapotának megtekintéséhez.
    
7. Kattintson a **Create** a képernyő alján.

    Miután sikeresen létrejön a fiók, az állapot **futó**változik. 

    ![Multimédia-szolgáltatások beállításai](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS fiókjának kezelését szeretné (például videó feltöltése, kódolását eszközök, figyelheti a projekt előrehaladásának) a **Beállítások** ablakban.

## <a name="manage-keys"></a>Billentyűk kezelése

A fiók nevét és az elsődleges kulcs információkat a Media Services fiók programozás útján történő eléréséhez szükséges.

1. Jelölje ki a fiókját az Azure-portálon. 

    **Beállítások** ablakban a jobb oldalon megjelenik. 

2. A **Beállítások** ablakban jelölje ki a **billentyűk**. 

    A **kulcsok kezelése** windows jeleníti meg a fiók nevét, és az elsődleges és másodlagos kulcsok jelenik meg. 
3. A Másolás gombra kattintva másolja az értékeket.
    
    ![Media Services kulcsok](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Adatfolyam-végpontok konfigurálása

Amikor az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség videofunkcióinak elkötelezett a leggyakoribb esetek egyike Azure Media Services használata. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak).

Dinamikus csomagolása, amely lehetővé teszi a adaptív átviteli sebesség kódolt MP4 tartalmának nélkül ezeknek a folyamatos átvitelű formátumok előre csomagolt változatának tárolni a Media Services (MPEG kötőjel HLS, zökkenőmentes Streaming, HDS) csak igény, által támogatott streaming előadásához Media Services ismertetése

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- Emeletes (forrás) fájlját kódolását be adaptív átviteli sebesség MP4-fájlokat (a kódolási lépéseket mutatni a később az oktatóprogram vannak).  
- Hozzon létre egy adatfolyam mennyisége, a *folyamatos átvitelű végpontot* , amelyhez használni kíván kézbesítési a tartalom. Az alábbi lépésekkel bemutatják, hogyan módosíthatja az adatfolyam egységek számát.

Dinamikus csomagolása, csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services hoz létre, és a megfelelő választ, egy ügyfél érkező kérések alapján szolgál.

Szeretne létrehozni, és módosítsa a folyamatos átvitelű fenntartott egységek számát, tegye a következőket:


1. Kattintson a **Beállítások** ablak **adatfolyam végpontok**. 

2. Kattintson az alapértelmezett streaming végpontot. 

    A **Folyamatos ÁTVITELŰ VÉGPONT ADATAIT alapértelmezett** ablak.

3. Adja meg az adatfolyam egységek számát, húzza a **adatfolyam egységek** csúszkát.

    ![A folyamatos átvitelű erőforrás-mennyiség](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kattintson a **Mentés** gombra a módosítások mentéséhez.

    >[AZURE.NOTE]A felosztás minden új egységek is legfeljebb 20 percet igénybe vehet.

## <a name="upload-files"></a>Fájlok feltöltése

Adatfolyam az Azure Media Services használata videók, kell a forrás videók feltöltése, kódolás több bitrates be, és az eredmény közzététele. Az első lépés a szakasz tartalma vonatkozik. 

1. A **beállítás** ablakában kattintson az **eszközök**.

    ![Fájlok feltöltése](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Kattintson a **Feltöltés** gombra.

    Megjelenik a **Töltse fel a videó eszköz** ablak.

    >[AZURE.NOTE] Nincs korlátozás nélkül fájl méretét.
    
4. Tallózással keresse meg a kívánt videó a számítógépen, jelölje ki azt, és kattintson az OK gombra.  

    A feltöltés elindul, és megtekintheti, hogy a fájl neve alatt a haladás.  

Ha a feltöltés befejeződött, hogy az új eszközre a **eszközök** ablakban megjelenő. 

## <a name="encode-assets"></a>Kódolását eszközök

Amikor az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség elkötelezett a leggyakoribb esetek egyike Azure Media Services használata. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak). A videó a felkészülés adaptív átviteli sebesség streaming, kell a forrás videó kódolását többszörös-átviteli sebesség fájlokba. A videók kódolása a **Media Encoder szabványos** kódoló kell használni.  

Media Services is biztosít dinamikus csomagolása, amely lehetővé teszi a többszörös-átviteli sebesség MP4s előadása a következő adatfolyam formátumokban: MPEG kötőjel, HLS, zökkenőmentes Streaming vagy HDS nélkül az ezek a folyamatos átvitelű formátumok újracsomagolására. Dinamikus csomagolása, csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services hoz létre, és a megfelelő választ, egy ügyfél érkező kérések alapján szolgál.

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- A forrásfájl kódolása a többszörös-átviteli sebesség MP4-fájlokat (a kódolási lépéseket vannak igazolni később ebben a részben).
- Szerezze be legalább egy adatfolyam egység az adatfolyam végpontot, amelyből tervezi kézbesítési a tartalmakat. További tudnivalókért olvassa el a [adatfolyam végpontok konfigurálása](media-services-portal-vod-get-started.md#configure-streaming-endpoints)című témakört. 

### <a name="to-use-the-portal-to-encode"></a>Kódolása a portál használatával

Ez a szakasz ismerteti a olyan lépés, a tartalom Media Encoder standard kódolását.

1.  A **Beállítások** ablakban válassza ki az **eszközöket**.  
2.  Az **eszközök** ablakban válassza ki az eszközt, amelyeket szeretne kódolni.
3.  Nyomja le a **kódolása** gombra.
4.  A **Tárgyi eszköz kódolása** ablakban válassza ki a "Media Encoder szabványos" processzor- és egy készletet. Ha például ha tudja, hogy a bemeneti videó 1920 x 1080 képpont felbontású rendelkezik, majd használhatja is az "H264 több átviteli sebesség 1080p" előre definiált. Kapcsolatos további információkért [Lásd: – tartalom](https://msdn.microsoft.com/library/azure/mt269960.aspx) elhelyezés fontos jelölje ki a készletet, amely a legmegfelelőbb a videó adatbevitelt. Ha egy alacsony felbontású (640 x 360) van, akkor kell nem használ-e az alapértelmezett "H264 több átviteli sebesség 1080p" előre definiált.
    
    Egyszerűbb kezelése közül lehet választani a szerkesztésére, a kimeneti eszköz nevét, és a feladat nevét.
        
    ![Kódolását eszközök](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Nyomja le az ENTER **létrehozása**.

### <a name="monitor-encoding-job-progress"></a>Projekt előrehaladásának kódolást monitorra

A kódolás feladat állapotának nyomon követéséhez, kattintson a **Beállítások** (az oldal tetején), és válassza a **feladatokat**.

![Feladatok](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Tartalom közzététele

Ahhoz, hogy a felhasználó URL-adatfolyam-vagy a tartalom letöltése használható, először kell "közzététel" a tárgyi eszköz egy megnevezés létrehozásával. Locator az eszköz tárolt fájlok hozzáférést biztosít. Media Services kétféle Locator támogatja: 

- A folyamatos átvitelű (OnDemandOrigin) Locator, használt adaptív streaming (például, hogy adatfolyam MPEG kötőjel, HLS vagy zökkenőmentes Streaming). Hozzon létre egy adatfolyam megnevezés a digitáliseszköz-.ism fájl kell tartalmaznia. 
- Fokozatos (Társítások) Locator, progresszív letöltés videofunkcióinak kézbesítve használható.


Adatfolyam URL-címet a következő formátumban van, és játssza le a folyamatos zökkenőmentes eszközök használhatja.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Egy URL-cím streaming HLS összeállításához hozzáfűző (formátum m3u8-aapl =) az URL-címet.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

A folyamatos átvitelű URL-cím MPEG GONDOLATJEL összeállításához hozzáfűző (formátum mpd – idő-csf =) az URL-címet.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Társítások URL-címet a következő formátumban van.

    {blob container name}/{asset name}/{file name}/{SAS signature}

>[AZURE.NOTE] A portál hozta létre Locator március 2015 előtt, ha két éves lejárati dátummal Locator lettek létrehozva.  

Frissíti egy megnevezés a lejárati dátum, használja a [többi](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-khoz. Ha egy Társítások megnevezés lejárati dátumát, akkor az URL-cím változik.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Tárgyi eszköz közzététele a portál használatával

A portál használatával tárgyi eszköz közzététele, tegye a következőket:

1. Válassza a **Beállítások** > **eszközök**.
1. Válassza ki a közzétenni kívánt eszközt.
1. Kattintson a **Közzététel** gombra.
1. Válassza ki a megnevezés.
2. Nyomja le a **hozzá**.

    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Az URL-cím bekerül a **Közzétett URL-címek**listáját.

## <a name="play-content-from-the-portal"></a>A portálról tartalom lejátszása

Az Azure portál tartalom lejátszó tesztelje a videó használható.

Kattintson a kívánt videót, és kattintson a **Lejátszás** gombra.

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

Néhány érvényesek:

- Győződjön meg arról, hogy a videoklip közzétételét követően.
- A **Media player** játssza le, az alapértelmezett végpont streaming. Ha az alapértelmezettől adatfolyam egyik végpontját a lejátszani kívánt, kattintson a másolja a vágólapra az URL-címet és egy másik lejátszóval. Ha például [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##<a name="next-steps"></a>Következő lépések

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


