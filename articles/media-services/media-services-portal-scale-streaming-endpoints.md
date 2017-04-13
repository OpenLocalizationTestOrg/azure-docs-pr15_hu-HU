<properties
    pageTitle=" A folyamatos átvitelű Azure Portal végpontok skála |} Microsoft Azure"
    description="Ebben az oktatóanyagban végigvezeti a lépéseket a méretezés adatfolyam végpontok az Azure portálján."
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


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>A folyamatos átvitelű Azure Portal végpontok skála

##<a name="overview"></a>– Áttekintés

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 

Amikor az ügyfelek számára a folyamatos átvitelű adaptív átviteli sebesség videofunkcióinak elkötelezett a leggyakoribb esetek egyike Azure Media Services használata. Media Services támogatja az alábbi adaptív átviteli adatfolyam-technológiák: HTTP-Live Streaming (HLS), zökkenőmentes Streaming, MPEG szaggatott és HDS (az Adobe PrimeTime és a hozzáférés licenciavevők csak).

Dinamikus csomagolása, amely lehetővé teszi a adaptív átviteli sebesség kódolt MP4 tartalmának nélkül ezeknek a folyamatos átvitelű formátumok előre csomagolt változatának tárolni a Media Services (MPEG kötőjel HLS, zökkenőmentes Streaming, HDS) csak igény, által támogatott streaming előadása Media Services ismertetése

Dinamikus összecsomagolása kihasználhatja kell tegye a következőket:

- Emeletes (forrás) fájlját kódolását be adaptív átviteli sebesség MP4-fájlokat (a kódolási lépéseket mutatni a később az oktatóprogram vannak).  
- Hozzon létre egy adatfolyam mennyisége, a *folyamatos átvitelű végpontot* , amelyhez használni kíván kézbesítési a tartalom. Az alábbi lépésekkel bemutatják, hogyan módosíthatja az adatfolyam egységek számát.

Dinamikus csomagolást csak akkor van szükség tárolására és a fájlokat az egyetlen tárolási formátuma kifizetéséhez és Media Services fog készítése és a megfelelő választ, egy ügyfél érkező kérések alapján szolgálnak.

Ezeken kívül szabályozhatja, hogy a folyamatos átvitelű végpont szolgáltatás kezelése növekvő sávszélesség igényeinek eredményhez tartozó adatfolyam egységek kapacitása. Ajánlott lefoglalhat egy vagy több skála egységek alkalmazások üzemi környezetben. Adatfolyam egységek vásárolhatja 200 MB és további funkciókat lépésekben mely funkciókat, amelyek tartalmazzák még a két dedikált kilépési kapacitással nyújt: [dinamikus csomagolása](media-services-dynamic-packaging-overview.md), CDN-integráció és speciális konfigurációs. További tudnivalókért lásd: [kezelése adatfolyam végpontok az Azure portálján](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>A folyamatos átvitelű végpontok skála

Szeretne létrehozni, és módosítsa a folyamatos átvitelű fenntartott egységek számát, tegye a következőket:

1. Az [Azure portál](https://portal.azure.com/)jelölje ki az Azure Media Services-fiókját.
2. A **Beállítások** ablakban jelölje ki a **adatfolyam végpontok**.
3. Kattintson az adatfolyam végpontot, amelyet szeretne méretezni. 
4. A csúszka húzásával adatfolyam egységek számának megadása
 
![A folyamatos átvitelű végpont](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

A következő érvényesek:

- A felosztás minden új adatfolyam egységek befejezéséhez körülbelül 20 percig is eltarthat. 
- Jelenleg fog bármilyen pozitív értékek a folyamatos átvitelű nincs vissza az erőforrás-mennyiség, letilthatja az egy óra felfelé folyamatos átvitelű igény szerinti.
- A költség kiszámítása a legmagasabb a 24 órás időszakra megadott számú használják. Árak részletes információt olvassa el a [Media Services árak részletei](http://go.microsoft.com/fwlink/?LinkId=275107)című témakört.

##<a name="next-steps"></a>Következő lépések

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


