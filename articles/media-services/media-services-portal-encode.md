<properties
    pageTitle="Kódolását Media Encoder szabványos használata az Azure portál tárgyi eszköz |} Microsoft Azure"
    description="Ebben az oktatóanyagban végigvezeti a kódolást tárgyi eszköz Media Encoder szabványos használata az Azure-portálra."
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
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Az Azure portál Media Encoder szabványos használata tárgyi eszköz kódolását

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 

Amikor az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség elkötelezett a leggyakoribb esetek egyike Azure Media Services használata. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak). A videó a felkészülés adaptív átviteli sebesség streaming, kell a forrás videó kódolását többszörös-átviteli sebesség fájlokba. A videók kódolása a **Media Encoder szabványos** kódoló kell használni.  

Media Services is biztosít dinamikus csomagolása, amely lehetővé teszi a többszörös-átviteli sebesség MP4s előadása a következő adatfolyam formátumokban: MPEG kötőjel, HLS, zökkenőmentes Streaming vagy HDS nélkül újra be ezeket a folyamatos átvitelű formátumok csomag. Dinamikus csomagolást csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services fog készítése és a megfelelő választ, egy ügyfél érkező kérések alapján szolgálnak.

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- A forrásfájl kódolása a többszörös-átviteli sebesség MP4-fájlokat (a kódolási lépéseket vannak igazolni később ebben a részben).
- Szerezze be legalább egy adatfolyam egység az adatfolyam végpontot, amelyből tervezi kézbesítési a tartalmakat. További tudnivalókért olvassa el a [adatfolyam végpontok konfigurálása](media-services-portal-vod-get-started.md#configure-streaming-endpoints)című témakört. 

Médiafájlok feldolgozás méretezéséhez lásd: [Ez](media-services-portal-scale-media-processing.md) a témakör.

## <a name="encode-with-the-azure-portal"></a>Az Azure portálján kódolását

Ez a szakasz ismerteti a olyan lépés, a tartalom Media Encoder standard kódolását.

1.  Az [Azure portál](https://portal.azure.com/)jelölje ki az Azure Media Services-fiókját.
2.  A **Beállítások** ablakban válassza ki az **eszközöket**.  
2.  Az **eszközök** ablakban válassza ki az eszközt, amelyeket szeretne kódolni.
3.  Nyomja le a **kódolása** gombra.
4.  A **Tárgyi eszköz kódolása** ablakban válassza ki a "Media Encoder szabványos" processzor- és egy készletet. Ha például ha tudja, hogy a bemeneti videó 1920 x 1080 képpont felbontású rendelkezik, majd használhatja is az "H264 több átviteli sebesség 1080p" előre definiált. Kapcsolatos további információkért [Lásd: – tartalom](https://msdn.microsoft.com/library/azure/mt269960.aspx) elhelyezés fontos jelölje ki a készletet, amely a legmegfelelőbb a videó adatbevitelt. Ha egy alacsony felbontású (640 x 360) van, akkor kell nem használ-e az alapértelmezett "H264 több átviteli sebesség 1080p" előre definiált.
    
    Egyszerűbb kezelése közül lehet választani a szerkesztésére, a kimeneti eszköz nevét, és a feladat nevét.
        
    ![Kódolását eszközök](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Nyomja le az ENTER **létrehozása**.


##<a name="next-step"></a>Következő lépés

Az Azure portálon kódolási projekt előrehaladásának figyelheti a [jelen](media-services-portal-check-job-progress.md) cikkben leírt módon.  

##<a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


