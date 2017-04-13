<properties
    pageTitle="  Az Azure portálján tartalom közzététele |} Microsoft Azure"
    description="Ebben az oktatóanyagban végigvezeti a közzétételi a tartalmakat az Azure portálján."
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

# <a name="publish-content-with-the-azure-portal"></a>Az Azure portálján tartalom közzététele

> [AZURE.SELECTOR]
- [Portál](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [TÖBBI](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>– Áttekintés

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 

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

További tudnivalókért olvassa el a [kézbesítése tartalom áttekintése](media-services-deliver-content-overview.md)című témakört.

>[AZURE.NOTE] A portál hozta létre Locator március 2015 előtt, ha két év lejárati dátummal Locator lettek létrehozva.  

Frissíti egy megnevezés a lejárati dátum, használja a [többi](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-khoz. Figyelje meg, hogy ha frissíti a Társítások megnevezés lejárati dátumát, az URL-cím megváltozik.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Tárgyi eszköz közzététele a portál használatával

A portál használatával tárgyi eszköz közzététele, tegye a következőket:

1. Az [Azure portál](https://portal.azure.com/)jelölje ki az Azure Media Services-fiókját.
1. Válassza a **Beállítások** > **eszközök**.
1. Válassza ki a közzétenni kívánt eszközt.
1. Kattintson a **Közzététel** gombra.
1. Válassza ki a megnevezés.
2. Nyomja le a **hozzá**.

    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Az URL-cím hozzáadódik **Közzétett URL-címek**listája.

## <a name="play-content-from-the-portal"></a>A portálról tartalom lejátszása

Az Azure portál tartalom lejátszó tesztelje a videó használható.

Kattintson a kívánt videót, és kattintson a **Lejátszás** gombra.

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

Néhány érvényesek:

- Győződjön meg arról, hogy a videoklip közzétételét követően.
- A **Media player** játssza le, az alapértelmezett végpont streaming. Ha az alapértelmezettől adatfolyam egyik végpontját a lejátszani kívánt, kattintson a másolja a vágólapra az URL-címet és egy másik lejátszóval. Ha például [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Az adatfolyam végpontot, amelyből streaming rendszernek kell futnia.  
- Adatfolyam-a egy adatfolyam végpontot, hozzáadhat kell legalább egy adatfolyam egység. További tudnivalókért lásd: [Ez](media-services-portal-scale-streaming-endpoints.md) a témakör.   

##<a name="next-steps"></a>Következő lépések

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


