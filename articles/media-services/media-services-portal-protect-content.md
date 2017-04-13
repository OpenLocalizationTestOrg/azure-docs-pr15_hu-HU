<properties 
    pageTitle="Az Azure portálon tartalomvédelem házirendek beállítása |} Microsoft Azure" 
    description="Ez a cikk bemutatja, hogyan lehet az Azure portal segítségével tartalomvédelmi házirendek beállítása. A cikk azt is megtudhatja, hogy miként engedélyezni a dinamikus titkosítást eszközeit." 
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

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Tartalomvédelem házirendek az Azure portálon konfigurálása

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>– Áttekintés

Microsoft Azure Media Services (AMS) lehetővé teszi a médiafájlokat a számítógépre tárhely, kezelését és kézbesítési elhagyja időpontot biztonságos. Media Services lehetővé teszi, hogy az előadása a tartalom titkosított dinamikusan a speciális Encryption Standard (AES) (128 bites titkosítási kulcs használatával), a PlayReady és/vagy Widevine DRM és az Apple FairPlay közös titkosítási (CENC). 

AMS tartalmaz DRM licencek a kézbesítési szolgáltatás, illetve AES hivatalos ügyfelek billentyűk törölje a jelet. Az Azure portal lehetővé teszi, hogy hozzon létre egy **kulcsot/licenc engedélyezési házirend** diagramtípusokat titkosításhoz állított be.

Ez a cikk bemutatja, hogyan lehet tartalomvédelem házirendek beállítása az Azure portálján. A cikk azt is megtudhatja, hogy miként dinamikus titkosítást alkalmazása eszközeit.

> [AZURE.NOTE]  Védelmi szabályok létrehozása az Azure klasszikus portál használt, ha a házirendek esetleg nem jelennek meg az [Azure-portálon](https://portal.azure.com/). Jó helyen jár, mind a régi házirendeket továbbra is megmarad. Őket az Azure Media Services .NET SDK vagy az [Azure-multimédia-szolgáltatások-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) eszközzel ellenőrizheti (a házirendek megtekintéséhez kattintson a jobb gombbal az eszközre-információ (F4) -> Megjelenítés > tartalom billentyűk lapon kattintson a kattintson a kulcs ->). 
> 
> Ha azt szeretné, az eszköz használatával új házirendek titkosítására, állítson be őket az Azure-portálra, kattintson a Mentés gombra, és újbóli alkalmazása a dinamikus titkosítást. 

## <a name="start-configuring-content-protection"></a>Indítsa el a tartalomvédelem konfigurálása

Indítsa el a tartalom, a AMS fiókjához globális védelem beállítása a portál használatával tegye a következőket:

1. Az [Azure portál](https://portal.azure.com/)jelölje ki az Azure Media Services-fiókját.
2. Válassza a **Beállítások** > **tartalomvédelem**.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Kulcs/licenc engedélyezési házirend

AMS több módon is, hogy a felhasználók, akik billentyűt vagy licenccel kéréseket hitelesítése támogatja. A tartalom főbb engedélyezési házirend Ön által beállított legyen, és az ügyfél ahhoz, hogy a kulcs licencet az ügyfélnek kell delived teljesül. A tartalom főbb engedélyezési házirend lehetnek egy vagy több engedélyezési korlátozások: **Nyissa meg** vagy **jogkivonat** korlátozás.

Az Azure portal lehetővé teszi, hogy hozzon létre egy **kulcsot/licenc engedélyezési házirend** diagramtípusokat titkosításhoz állított be.

###<a name="open"></a>Nyissa meg 

Megnyitott korlátozás, az azt jelenti, hogy a rendszer a kulcs előadása fel bárki megtekintheti, aki kulcs kérést. Ez a korlátozástípus akkor lehet hasznos tesztelése céljából. 

### <a name="token"></a>Jogkivonat

A token korlátozott házirend által egy biztonságos jogkivonat szolgáltatás (STS) kiadott token mellékelni kell. Media Services tokenek támogatja az egyszerű webes tokenek (SWT) és JSON webes jogkivonat (JWT) formátum. Media Services nem biztonságos jogkivonat szolgáltatást biztosít. Hozzon létre egy egyéni STS, vagy a Microsoft Azure ACS kihasználhatja a probléma tokenek. A STS kell beállítania, hogy hozzon létre egy jogkivonat, a megadott billentyű és a probléma követelések, a token korlátozás konfigurációban megadott aláírva. A Media Services fő kézbesítési szolgáltatás a kért kulcs (vagy licenccel) ad vissza az ügyfél, ha a token érvényes és követelések e konfigurálva jogkivonat megegyeznek a kulcs (vagy a licencet).

Ha házirend beállítása a token korlátozott, meg kell adnia az igazolás elsődleges kulcsot, a kibocsátó és a közönség paraméterek. Az ellenőrzés elsődleges kulcs, tartalmazza, a kulcs, amely a token van aláírva kibocsátó a biztonságos jogkivonat-szolgáltatás, amely a token problémák. A közönség felé (más néven hatókör) a token szándékának mutatja be, vagy az erőforrás a token engedélyezi a hozzáférést. A Media Services fő kézbesítési szolgáltatás ellenőrzi, hogy ezeket az értékeket a token értékekre a sablonban.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady tartalomvédelmi sablon

Részletes információt a PlayReady tartalomvédelmi sablont a [Media Services PlayReady licenc sablon áttekintése](media-services-playready-license-template-overview.md)című témakörben találhat.

### <a name="non-persistent"></a>Állandó nem

Licenccel nem állandó, adja meg, ha azt csak tartja a memóriában közben a Windows Media player használja a licencet.  

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Állandó

Ha állandó, adja meg a licenc, oda menti a program az állandó tároló az ügyfélgépen.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine tartalomvédelmi sablon

A Widevine tartalomvédelmi sablon kapcsolatos részletes tudnivalókért [Widevine licenc sablon áttekintése](media-services-widevine-license-template-overview.md)című témakörben találhat.

### <a name="basic"></a>Egyszerű

**Egyszerű**jelölje be a sablon létrejön az összes alapértelmezett értékű.

### <a name="advanced"></a>Speciális

Részletes ismertetése a Speciális beállítások Widevine konfigurációk olvassa el a [témakör](media-services-widevine-license-template-overview.md) című témakört.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay konfigurálása

Ahhoz, hogy a titkosítási FairPlay, meg kell adnia az alkalmazás tanúsítványt és az alkalmazás titkos kulcs (ASK) keresztül a FairPlay konfiguráció lehetőséget. FairPlay konfigurálása és a követelmények részletes információt [ismertető](media-services-protect-hls-with-fairplay.md) témakört is.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dinamikus titkosítást alkalmazása a digitális eszköz kiválasztása

Dinamikus titkosítást kihasználhatja kell tegye a következőket:

- A forrásfájl kódolását be adaptív átviteli sebesség MP4 fájlokat.
- Szerezze be legalább egy igény szerinti adatfolyam egység az adatfolyam végpontot, ahonnan a tartalmak olvasása megtervezése. További információért megtudhatja, [hogy miként igény szerinti fenntartott egységek streaming méretezni](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Amely titkosítani kívánt digitális eszköz kiválasztása

Az összes a eszközök megtekintéséhez válassza a **Beállítások** > **eszközök**.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Titkosítás jelszóval: AES vagy DRM

Miután **titkosítása** nyomja meg az eszköz a, bemutatott két lehetőséggel: **AES** vagy **DRM**. 

#### <a name="aes"></a>AES

Törlése a titkosítási kulcs engedélyezi az összes adatfolyam protokollon AES: zökkenőmentes Streaming HLS és MPEG-vonal.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Ha bejelöli a DRM fülre, a különféle beállítási lehetőségekre húzva tartalomvédelem házirendek bemutatott (amely kell állította be, most) + protokollok streaming csoportja.

- **PlayReady és Widevine gondolatjelre MPEG** - dinamikusan a MPEG-vonal adatfolyam PlayReady és Widevine DRMs titkosítsa.
- **PlayReady és Widevine MPEG-vonal + FairPlay HLS együtt** – dinamikusan titkosítja, MPEG-vonal adatfolyam PlayReady és Widevine DRMs. A FairPlay HLS fogadhassák is titkosítja.
- **Csak a folyamatos zökkenőmentes, HLS és MPEG-vonal PlayReady** - dinamikusan titkosítja zökkenőmentes Streaming HLS, a PlayReady DRM MPEG-vonal adatfolyamok.
- **Csak gondolatjelre MPEG Widevine** - titkosítás MPEG-vonal Widevine DRM az Ön dinamikusan.
- **Csak a HLS FairPlay** - dinamikusan titkosítja a HLS adatfolyam FairPlay együtt.

Ahhoz, hogy a titkosítási FairPlay, meg kell adnia az alkalmazás tanúsítványt és az alkalmazás titkos kulcs (ASK) keresztül a a tartalomvédelmi beállítások lap FairPlay konfiguráció lehetőséget.

![A tartalom védelme](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Ha használni szeretné a kijelölt titkosítás, kattintson az **Alkalmaz**.

##<a name="next-steps"></a>Következő lépések

Tekintse át a Media Services tanulási javaslatok.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





